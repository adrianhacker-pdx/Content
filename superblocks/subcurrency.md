# Introduction to dapps - Creating a subcurrency

This dapp implements the simplest form of a cryptocurrency that uses the ERC-20 standard to define a token you can create and send to others. This tutorial is intended to be followed using the online IDE available at [studio.ethereum.org](https://studio.ethereum.org), and selecting the "Coin" template.

![Select Coin template](./coin-template.png)

## The smart contract

The contract allows only its creator to create new coins (different issuance scheme are possible). Anyone can send coins to each other without a need for registering with a username and password, and all you need is an Ethereum keypair.

The line `address public minter;` declares a state variable of an [`address` type](https://solidity.readthedocs.io/en/latest/types.html#address). The `address` type is a 160-bit value that does not allow any arithmetic operations. It is suitable for storing addresses of contracts, or a hash of the public half of a keypair belonging to [external accounts](https://solidity.readthedocs.io/en/latest/introduction-to-smart-contracts.html#accounts).

The keyword `public` automatically generates a function that allows you to access the current value of the state variable from outside of the contract. Without this keyword, other contracts have no way to access the variable. The code of the function generated by the compiler is equivalent to the following (ignore `external` and `view` for now):

```solidity
function minter() external view returns (address) { return minter; }
```

You could add a function like the above yourself, but you would have a function and a state variable with the same name. This is not necessary, the compiler figures it out for you.

The next line, `mapping (address => uint) public balances;` also creates a public state variable, but it is a more complex data type. The [mapping](https://solidity.readthedocs.io/en/latest/types.html#mapping-types) type maps addresses to [unsigned integers](https://solidity.readthedocs.io/en/latest/types.html#integers).

You can think of mappings as [hash tables](https://en.wikipedia.org/wiki/Hash_table) which are virtually initialized such that every possible key exists from the start and is mapped to a value whose byte-representation is all zeros. It is not possible to obtain a list of all keys of a mapping, nor a list of all values. Record what you added to the mapping, or use it in a context where this is not needed. Even better, keep a list, or use a more suitable data type.

The [getter function](https://solidity.readthedocs.io/en/latest/contracts.html#getter-functions) created by the `public` keyword is more complex in the case of a mapping. It looks like the following::

```solidity
function balances(address _account) external view returns (uint) {
    return balances[_account];
}
```

You can use this function to query the balance of a single account.

The line `event Sent(address from, address to, uint amount);` declares an "[event](https://solidity.readthedocs.io/en/latest/contracts.html#events)", which is emitted in the last line of the function `send`. Ethereum clients such as web applications can listen for these events emitted on the blockchain without much cost. As soon as the event is emitted, the listener receives the arguments `from`, `to` and `amount`, which makes it possible to track transactions.

The [constructor](https://solidity.readthedocs.io/en/v0.5.11/contracts.html#constructor) is a special function run during the creation of the contract, and you cannot call it afterwards. In this case, it permanently stores the address of the person creating the contract. The `msg` variable (together with `tx` and `block`) is a [special global variable](https://solidity.readthedocs.io/en/v0.5.11/units-and-global-variables.html#special-variables-functions) that contains properties which allow access to the blockchain. `msg.sender` is always the address where the current (external) function call came from.

The functions that make up the contract, and that users and contracts can call are `mint` and `send`.

The `mint` function sends an amount of newly created coins to another address. The [require](https://solidity.readthedocs.io/en/v0.5.11/control-structures.html#assert-and-require) function call defines conditions that reverts all changes if not met. In this example, `require(msg.sender == minter);` ensures that only the creator of the contract can call `mint`, and `require(amount < 1e60);` ensures a maximum amount of tokens, without which could cause overflow errors in the future.

Anyone can use the `send` (who already has some of these coins) to send coins to anyone else. If the sender does not have enough coins to send, the `require` call fails and provides the sender with an appropriate error message string.

## The Web app

This tutorial doesn't cover the HTML or CSS as it's not web3 specific, aside from the element IDs that the JavaScript manipulates. A lot of the JavaScript code follows standard patterns for object-oriented JavaScript, so this tutorial focuses on the web3js specific parts.

First create an instance of the smart contract, [passing it as a property](https://web3js.readthedocs.io/en/v1.2.0/web3-eth-contract.html), which allows web3js to interact with it.

```javascript
function Coin(Contract) {
    this.web3 = null;
    this.instance = null;
    this.Contract = Contract;
}
```

Initialize the `Coin` object and create an instance of the web3js library, passing Metamask as a provider for the contract. The initialization function then defines the interface for the contract using [the web3js contract object](https://web3js.readthedocs.io/en/v1.2.1/web3-eth-contract.html#new-contract) and then defines the address of the instance of the contract for the `Coin` object.

```javascript
Coin.prototype.init = function() {

    this.web3 = new Web3(
        (window.web3 && window.web3.currentProvider) ||
        new Web3.providers.HttpProvider(this.Contract.endpoint));

    var contract_interface = this.web3.eth.contract(this.Contract.abi);

    this.instance = contract_interface.at(this.Contract.address);
};
```

Add other JavaScript boilerplate to create the instance of the `Coin` object defined above, and bind the functions for interacting with the contract to the buttons defined in the HTML:

```javascript
Coin.prototype.bindButtons = function() {
    var that = this;

    $(document).on("click", "#button-create", function() {
        that.createTokens();
    });

    $(document).on("click", "#button-check", function() {
        that.showAddressBalance();
    });
}

Coin.prototype.onReady = function() {
    this.bindButtons();
    this.init();
    this.main();
};

var coin = new Coin(Contracts['Coin']);

$(document).ready(function() {
    coin.onReady();
});
```

Create the function triggered when someone clicks the "send" button which creates a specified number of new coin tokens and sends them to the address specified. Before doing this, it checks the address and amount are valid using two utility functions we create later. Web3js provides access to functions in the smart contract by accessing the instance of the contract. For example, the function below calls the `mint` function of the contract to create the tokens with `this.instance.mint`, passing the variables it needs, waiting for confirmation, or returning an error.

```javascript
Coin.prototype.createTokens = function() {
    var that = this;

    var address = $("#create-address").val();
    var amount = $("#create-amount").val();
    console.log(amount);

    if(!isValidAddress(address)) {
        console.log("Invalid address");
        return;
    }

    if(!isValidAmount(amount)) {
        console.log("Invalid amount");
        return;
    }

    this.instance.mint(address, amount, { from: window.web3.eth.accounts[0], gas: 100000, gasPrice: 100000, gasLimit: 100000 },
        function(error, txHash) {
            if(error) {
                console.log(error);
            }
            else {
                that.waitForReceipt(txHash, function(receipt) {
                    if(receipt.status) {
                        $("#create-address").val("");
                        $("#create-amount").val("");
                    }
                    else {
                        console.log("error");
                    }
                });
            }
        }
    )
}
```

The `waitForReceipt` function called above uses the web3js function `[getTransactionReceipt](https://web3js.readthedocs.io/en/v1.2.1/web3-eth.html?highlight=getTransactionReceipt#gettransactionreceipt)` to wait for a receipt for the transaction, trying again if the first attempt fails:

```javascript
Coin.prototype.waitForReceipt = function(hash, cb) {
    var that = this;

    this.web3.eth.getTransactionReceipt(hash, function(err, receipt) {
        if (err) {
            error(err);
        }
        if (receipt !== null) {
            if (cb) {
                cb(receipt);
            }
        } else {
            window.setTimeout(function() {
                that.waitForReceipt(hash, cb);
            }, 2000);
        }
    });
}
```

Create the function called when someone clicks the "Check balance" button. The function takes the address value specified, and checks it is a valid address with the same utility function used earlier. The function then calls the `getBalance` function which in turn calls the `balances` function of the instance of the `Coin` contract, passing the address value to it, and returning the result or an error.

```javascript
Coin.prototype.showAddressBalance = function(hash, cb) {
    var that = this;

    var address = $("#balance-address").val();

    if(!isValidAddress(address)) {
        console.log("Invalid address");
        return;
    }

    this.getBalance(address, function(error, balance) {
        if(error) {
            console.log(error)
        }
        else {
            console.log(balance.toNumber());
                $("#message").text(balance.toNumber());
        }
    })
}

Coin.prototype.getBalance = function(address, cb) {
    this.instance.balances(address, function(error, result) {
        cb(error, result);
    })
}
```

These are the two utility functions that check for a valid address and amount:

```javascript
function isValidAddress(address) {
    return /^(0x)?[0-9a-f]{40}$/i.test(address);
}

function isValidAmount(amount) {
    return amount > 0 && typeof Number(amount) == 'number';
}
```

And that's all the code. To see the dapp in action, click _Compile_, then _Deploy_ found under the disclosure triangle of the contract file, then open the _Preview_ tab to see the frontend of the dapp.
