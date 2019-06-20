# Цель статьи
Цель данной статьи – дать базовое представление о сети Ethereum (Эфириум) каждому, кто захочет ее использовать.


# Что такое Ethereum?
Объяснить, что такое Ethereum, можно по-разному, и у разных людей будут разные представления о нем. К концу этой статьи Вы сформируете собственное виденье Ethereum. Хочется процитировать Андреаса Антонопулуса, который, как мне кажется, довольно точно и лаконично описал Ethereum и с практической, и с точки зрения компьютерных наук.
 

Цитата из книги «[Mastering Ethereum](https://github.com/ethereumbook/ethereumbook)» за авторством Андреаса Антонопулоса и Гэвина Вуда:

> Ethereum часто описывают как «Всемирный компьютер» («the world computer»). Но что же это означает? Для начала, рассмотрим определение с точки зрения компьютерных наук, а потом попробуем расшифровать его с помощью практического анализа возможностей и характеристик Ethereum, а так же сравнения его с Bitcoin (Биткоином) и другими децентрализованными платформами обмена информацией (так называемые блокчейнами).
  С точки зрения компьютерных наук, Ethereum является детерминированной, но в то же время практически неограниченной, машиной состояний,  которая состоит из глобально доступного единого состояния (singleton state) и виртуальной машины, которая применяет изменения к этому самому состоянию.
  С более практичной точки зрения, Ethereum – это глобально децентрализованная вычислительная инфраструктура с открытым исходным кодом, которая выполняет программы, называемые смарт-контрактами. Эта инфраструктура использует блокчейн для синхронизации и хранения состояний системы, а также криптовалюту ether (эфир) для измерения и ограничения затрат ресурсов на выполнение.
  Платформа Ethereum дает разработчикам возможность делать мощные децентрализованные приложения со встроенными экономическими функциями. Кроме обеспечения высокой доступности, возможности аудита, прозрачности и нейтральности, она так же снижает или полностью убирает цензуру и значительно сокращает риск контрагента.


Цитата из книги «[Mastering Ethereum, Section 1 - What is Ethereum?](https://github.com/ethereumbook/ethereumbook/blob/develop/01what-is.asciidoc)» за авторством Андреаса Антонопулоса и Гэвина Вуда.

### Ethereum как «Всемирный компьютер»


В определении Ethereum Антонопулос использует несколько примечательных терминов: всемирный компьютер, блокчейн, детерминированный, состояние, машина состояний, децентрализованная вычислительная инфраструктура, смарт-контракты, децентрализованные приложения и прочие.

Давайте рассмотрим эти термины, но сначала стоит также обратить внимание на последнее предложение последнего параграфа:

> Кроме обеспечения высокой доступности, возможности аудита, прозрачности и нейтральности, она так же снижает или полностью убирает цензуру и значительно сокращает риск контрагента.

Это очень примечательно. Держите это в уме, пока мы будем дальше разбираться с Ethereum.

Давайте рассмотрим некоторые из вышеупомянутых терминов и, в качестве мысленного упражнения, дадим им свободное определение:
* Всемирный компьютер – компьютер, доступный всем, не имеющий географических ограничений;
* Блокчейны – связанные блоки данных, Ethereum является блокчейном;
* Детерминированный – независимо от узла, на котором запущена программа, результат вычислений будет одинаковым;
* Состояние — сохраненная информация о программе или системе;
* Машина состояний — механизм, который изменяет вышеупомянутое состояние, соблюдая некий консенсус;
* Децентрализованная вычислительная инфраструктура – децентрализованная инфраструктура, в котором все узлы, поддерживающие сеть, имеют одинаковы; привилегии и являются равноправными,
* Смарт-контракт – код, выполнение которого можно ожидать в децентрализованной вычислительной инфраструктуре;
* Децентрализованные приложения – приложения, которые касаются децентрализованной вычислительной инфраструктуры либо используют смарт-контракты, или и то, и то.

Отдельные лица, проекты и бизнесы обычно предпочитают использовать устойчивые, стабильные системы, на которые можно положиться. Это именно то, что Ethereum предлагает своим разработчикам.

Ethereum отказоустойчив. Это значит, что узлы могут пропадать из сети без какого-либо существенного эффекта на безопасность или пропускную способность транзакций сети. Узлы синхронизируются к текущему состоянию при следующем подключении к сети.

Ethereum позволяет разработчикам писать и развертывать иммутабельные (неизменяемые) программы в блокчейн. После развертывания, этим программам можно доверить выполнение без какого-либо вмешательства внешних для блокчейна событий. Такие программы принято называть смарт-контрактами.

# Родная валюта Ethereum и не только 

Для лаконичности приведем несколько проверенных и верных определений:


> В связи с тем, что Ethereum нацелен на любые приложения, не обязательно валютные или финансовые, существует фундаментальная единица стоимости в сети для предотвращения возможности злоупотребления сетью путем излишних вычислительных нагрузок. Эта единица называется газ (gas),...  

Автор: Майка Дамерон, [«Beigepaper: An Ethereum Technical Specification»](https://github.com/chronaeon/beigepaper/blob/master/beigepaper.pdf) Раздел 1.1 Native Currency

Чтобы использовать сеть Ethereum для отправки данных с одного аккаунта на другой или развертывания смарт-контрактов в блокчейне нужно заплатить майнерам, которые обеспечивают безопасность сети, комиссию, вышеупомянутый газ (gas fee).

Ether (эфир) используется для оплаты данной комиссии. Ether является родной валютой для блокчейна Ethereum. Ether можно делить на части, в таблице ниже представлены некоторые соотношения.



| Единица | Ether | Wei |
| -------- | -------- | -------- |
| Ether     | Ξ1.00000000000000000     | 1,000,000,000,000,000,000     |
| Finney     | Ξ0.001000000000000000     | 1,000,000,000,000,000     |
| Ether     | Ξ0.000001000000000000     | 1,000,000,000,000     |
| Ether     | Ξ0.000000000000000001     | 1     |

Из: [«Beigepaper: An Ethereum Technical Specification»](https://github.com/chronaeon/beigepaper/blob/master/beigepaper.pdf)

(WEI - это наименьшая и неделимая частица Ethereum, 1 Ether = 10^18 Wei - прим. перевод.)

# Децентрализованные приложения (Decentralized Apps, DApps)

В самом узком определении, децентрализованные приложения являются смарт-контрактом с фронтендом (пользовательским интерфейсом). В контексте данной статьи, децентрализованное приложение – это приложение, которое распространяет свою логику, хранилище данных или коммуникацию с помощью протокола Ethereum или другого web3 протокола.

* «web3» – разговорный термин для децентрализованного веба;
* «web2» – разговорный термин для текущего состояния интернета.


# Причины для использования DApp

В список примеров децентрализованных приложений, которые появились за последние несколько лет, можно отнести:
* Децентрализованные обмены;
* Рынки прогнозирования;
* Децентрализованные базы знаний (такие как Kauri);
* Системы идентификации с открытым исходным кодом;
* Казино;
* Игры и множество других.

Список далеко не исчерпывающий, быстрый поиск в интернете покажет множество текущих проектов децентрализованных приложений, многие из которых решают проблемы реального мира.

Более подробный список можно найти на https://www.stateofthedapps.com/.


