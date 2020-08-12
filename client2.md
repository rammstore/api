# RAMM.STORE Client2 API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
    * [Методы поиска данных](#Методы-поиска-данных) 
* [Ошибки](#Ошибки)
* [Константы](#Константы)
   * [Значения Account.State](#Значения-AccountState)
* [Методы](#Методы)
    * [Аутентификация и авторизация клиента, действия с собственной сессией](#Аутентификация-и-авторизация-клиента-действия-с-собственной-сессией)
        * [session.login](#sessionlogin)
        * [session.logout](#sessionlogout)
        * [password.set](#passwordset)
    * [Общая информация о клиенте](#Общая-информация-о-клиенте)
        * [statistic.get](#statisticget)        
    * [Спецификации](#Спецификации)
        * [platform.getSpecification](#platformgetSpecification)
    * [Кошельки и операции с кошельком](#Кошельки-и-операции-с-кошельком)
        * [wallets.get](#walletsget)
        * [walletTransfers.search](#walletTransferssearch)
    * [Информация о стратегиях](#Информация-о-стратегиях)
        * [strategies.get](#strategiesget)
        * [charts.get](#chartsget)
        * [strategyportfolio.get](#strategyportfolioget)
        * [strategysymbolstat.get](#strategysymbolstatget)
        * [strategies.search](#strategiessearch)
        * [strategies.getOffers](#strategiesgetoffers)
    * [Собственные стратегии клиента](#Собственные-стратегии-клиента)
        * [myStrategies.add](#mystrategiesadd)	
        * [myStrategies.addOffer](#mystrategiesaddOffer)	
        * [myStrategies.setPublicOffer](#mystrategiessetpublicoffer)	
        * [myStrategies.close](#myStrategiesclose)
        * [myStrategies.pause](#myStrategiespause)
        * [myStrategies.resume](#myStrategiesresume)        
        * [myStrategies.getToken](#myStrategiesgetToken)
        * [myStrategies.setToken](#myStrategiessetToken)
        * [myStrategies.checkName](#myStrategiescheckName)
        * [myStrategies.getActiveAccounts](#myStrategiesgetActiveAccounts)
        * [myStrategies.getClosedAccounts](#myStrategiesgetClosedAccounts)
        * [strategyCommands.get](#strategyCommandsget)
    * [Торговые счета клиентов](#Торговые-счета-клиентов)
        * [accounts.add](#accountsadd)
        * [accounts.close](#accountsclose)
        * [accounts.fund](#accountsfund)
        * [accounts.withdraw](#accountswithdraw)
        * [accounts.get](#accountsget)
        * [accounts.getStatement](#accountsgetStatement)
        * [accounts.pause](#accountspause)
        * [accounts.resume](#accountsresume)
        * [accounts.setFactor](#accountssetFactor)
        * [accounts.setProtection](#accountssetProtection)
        * [accounts.setTarget](#accountssetTarget)
        * [accounts.searchClosed](#accountssearchClosed)
        * [accounts.getCharts](#accountsgetCharts)
        * [accounts.searchSpec](#accountssearchSpec)
        * [accountCommands.get](#accountCommandsget)
    * [Открытые позиции](#Открытые-позиции)        
        * [positions.search](#positionssearch)
    * [Сделки на счете](#Сделки-на-счете)    
        * [deals.search](#dealssearch)

## Выполнение запросов
Для обращения к API необходимо сделать POST-запрос по адресу `https://{env}.ramm.store/api/client/v2/{method}`, где:
* {env} — название среды (получите у поддержки).
* {method} — метод API.

Ответ вернётся в JSON.

### Методы поиска данных

Все методы поиска данных ***.search*** могут содержать следующие секции:

**Filter**	

Список допустимых полей для поиска зависит от вызываемого метода.

**Pagination**	

Задание текущей страницы и количества записей на страницу.

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
CurrentPage   | number | Номер текущей страницы | 1
PerPage   | number | Количество записей на одной странице | 100


**OrderBy**

Задание поля для сортировки и направления сортировки.

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
Field   | string | Поле для сортировки |
Direction   | string | Направление сортировки, варианты: Asc, Desc | Asc

**Возвращаемые данные**

Возвращаемые данные по методам поиска всегда содержат полную информацию об используемом фильтре, пагинации и сортировке.

Пример:
```json
{
    "Filter": {
        "StrategyID": 3457
    },
    "Pagination": {
        "TotalRecords": 15,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 100,
        "MaxPerPage": 100
    },
    "OrderBy": {
        "Field": "DealID",
        "Direction": "Desc"
    }
}
```
## Ошибки
API может возвращать различные ошибки в следующем формате:
```json
{
    "Error": {
        "Code": "invalid_input",
        "Message": "Invalid input in the field 'Login'"
    }
}
```
## Константы
### Значения Account.State  

Значение|Расшифровка
---------|----------
0	|	new	|
1	|	(not used)	|
2	|	active, no positions	|
3	|	active, with positions	|
4	|	margin call, with positions	|
5	|	margin call, no positions	|
6	|	protection, with positions	|
7	|	protection, no positions	|
8	|	target, with positions	|
9	|	target, no positions	|
10	|	pause, with positions	|
11	|	pause, no positions	|
12	|	disabled, with positions	|
13	|	disabled, no positions	|
14	|	closed, with positions	|
15	|	closed, no positions	|

## Методы
### Аутентификация и авторизация клиента, действия с собственной сессией
#### session.login

Авторизует пользователя по логину и паролю, создает сессию. Возвращает токен для использования API и информацию по сессии.

Вместо логина и пароля может быть передан OTP-токен, в случае авторизации через сайт брокера.

**URL:** `https://ramm.store/api/client/v2/session.login`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
Login   | string | Логин |
Password   | string | Пароль |
ExpirationMinutes   | number | Автоматическое удаление сессии при неактивности в течении заданного времени | 60
OTP   | string | Одноразовый пароль |

**Возвращаемые данные:** структуры Session, Client, Company и массив Wallets:

Параметр | Тип | Описание 
---------|----------|----------
***Session*** |
Token   | string | Токен, уникальный идентификатор |
WalletID   | number | ID кошелька по-умолчанию |
DTLastActivity   | datetime | Время последней активности  |
ExpirationMinutes   | number | Длительность сессии  |
***Client*** |
ID    | number | Уникальный номер клиента
Login   | string | Логин  |
FirstName   | string | Имя клиента  |
LastName   | string | Фамилия клиента  |
Language   | string | Язык интерфейса  |
PushToken   | string | Токен клиента для Push-нотификации  |
***Company*** |
ID    | number | Уникальный номер компании
Name   | string | Название компании  |
Demo   | boolean | Признак демо-компании  |
BrandKey   | string | Код брендирования  |
***Wallets*** |
ID   | number | ID кошелька  |
IDClient   | number | ID клиента  |
DT   | datetime | Дата создания кошелька  |
Asset   | string | Название актива  |
Status   | number | 0-new, 1-active  |
Balance   | real | Баланс кошелька  |
Invested   | real | Инвестированная сумма  |
Margin   | real | Задействованная маржа  |
IntervalPnL   | real | Прибыль/убыток в текущем торговом интервале |	
ActiveStrategiesCount | number | количество активных стратегий привязанных к кошельку

**Пример вызова:**
```json
{
    "Login": "test@gmail.com",
    "Password": "qwert12345"
}
```
**Пример ответа:**
```json
{
    "Session": {
        "Token": "398D6062-8D57-4B9D-99E3-5421E4D19E89",
        "WalletID": 136,
        "DTLastActivity": "2018-12-18T07:25:01.147",
        "ExpirationMinutes": 60
    },
    "Client": {
        "Login": "test@gmail.com",
        "FirstName": "test1",
        "LastName": "test2",
        "Language": "en",
        "PushToken": "80b5aaa0-21c7-494d-a0c8-1065098d912e"
    },
    "Company": {
        "Name": "BrokerName",
        "Demo": false
    },
    "Wallets": [
        {
            "ID": 12,
            "IDClient": 2132,
            "DT": "2018-12-18T07:25:01.147",
            "Asset": "USD",
            "Status": 1,
            "Balance": 0,
            "Invested": 0,
            "Margin": 0,
            "IntervalPnL": 0,
            "ActiveStrategiesCount": 2
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### session.logout

Удаляет сессию.

**URL:** `https://ramm.store/api/client/v2/session.logout`

**Параметры:** отсутствуют

**Возвращаемые данные:** отсутствуют

[Вернуться к содержанию](#Содержание)

#### password.set

Установка пароля собственной учетной записи клиента. Перед установкой нового пароля проверяется правильность текущего пароля.

**URL:** `https://ramm.store/api/client/v2/password.set`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
Password   | string | Новый пароль |
OldPassword   | string | Текущий пароль |

**Возвращаемые данные:** отсутствуют

**Пример вызова:**
```json
{
    "Password": "qwert12345",
    "OldPassword": "12345qwert"
}
```
[Вернуться к содержанию](#Содержание)
### Общая информация о клиенте
#### statistic.get

Получение общей статистики клиента

**URL:** `https://ramm.store/api/client/v2/statistic.get`

**Параметры:** отсутствуют

**Возвращаемые данные:**

Возвращаемые данные:

Параметр | Тип | Описание 
---------|----------|----------
IDClient                          	|	number	|	ID клиента
IDCompany                         	|	number	|	ID компании
Test                              	|	number	|	Признак тестового счета
AccountsBalance                   	|	real	|	Суммарный баланс инвестиций
AccountsEquity                    	|	real	|	Суммарное эквити инвестиций
AccountsProfitCurrentIntervalGross	|	real	|	Суммарный профит в текущем интервале
AccountsProfitPositionsGross      	|	real	|	Суммарный профит по открытым позициям
AccountsPastProfit                	|	real	|	Суммарная прибыль предыдущих периодов
AccountsFeeCharged                	|	real	|	Суммарное начисленное вознаграждение (по инвестициям)
AccountsFeePaid                   	|	real	|	Суммарное выплаченное вознаграждение (по инвестициям)
AccountsCommissionPaid            	|	real	|	Суммарная выплаченная комиссия (по инвестициям)
AccountsActive                    	|	number	|	Количество активных инвестиций
AccountsClosed                    	|	number	|	Количество закрытых инвестиций
StrategiesActive                  	|	number	|	Количество активных стратегий
StrategiesClosed                  	|	number	|	Количество закрытых стратегий
StrategiesFeeReceived             	|	real	|	Суммарное полученное вознаграждение (по стратегиям)
StrategiesFeeDue                  	|	real	|	Суммарное начисленное, но не полученное вознаграждение (по стратегиям)
StrategiesCommissionReceived      	|	real	|	Суммарная полученная комиссия (по стратегиям)
StrategiesCommissionDue           	|	real	|	Суммарная начисленная, но не полученная комиссия (по стратегиям)
WalletTransfersTotalIn            	|	real	|	Сумма входящих перечислений
WalletTransfersTotalOut           	|	real	|	Сумма исходящих перечислений
WalletTransfersTotalDelta         	|	real	|	Разница входящих и исходящих
Swap                              	|	real	|	Свопы
DealsVolume                       	|	real	|	Объем сделок
WalletsTotalBalance               	|	real	|	Суммарный баланс кошельков
PositionsCount                    	|	number	|	Количество открытых позиций
Login	|	string	|	Логин клиента


**Пример ответа:**
```json
{
    "IDClient": 34,
    "IDCompany": 2,
    "Test": 0,
    "AccountsBalance": 3423.23,
    "AccountsEquity": 3423.23,
    "AccountsProfitCurrentIntervalGross": 3423.23,
    "AccountsProfitPositionsGross": 3423.23,
    "AccountsPastProfit": 0,
    "AccountsFeeCharged": 454.45,
    "AccountsFeePaid": 454.55,
    "AccountsCommissionPaid": 87.78,
    "AccountsActive": 5,
    "AccountsClosed": 10,
    "StrategiesActive": 3,
    "StrategiesClosed": 33,
    "StrategiesFeeReceived": 786,
    "StrategiesFeeDue": 455,
    "StrategiesCommissionReceived": 24,
    "StrategiesCommissionDue": 54,
    "WalletTransfersTotalIn": 4000,
    "WalletTransfersTotalOut": 300,
    "WalletTransfersTotalDelta": 3700,
    "Swap": 4.15,
    "DealsVolume": 45000,
    "WalletsTotalBalance": 4000,
    "PositionsCount": 5,
    "Login": "khgjh@gfg.tu"
}
```

[Вернуться к содержанию](#Содержание)

### Спецификации
#### platform.getSpecification

Получение спецификации платформы

**URL:** `https://ramm.store/api/client/v2/platform.getSpecification`

**Параметры:** отсутствуют

**Возвращаемые данные:**

Возвращаемые данные - структуры Account и Strategy:

Параметр | Тип | Описание 
---------|----------|----------
***Account***
AvailableCurrency	|	string	|	Доступные валюты счета	|
MinBalance	|	real	|	Минимальный баланс инвестиции инвестора	|
***Strategy***	      
MinAmountToCreate	|	real	|	Минимальный баланс инвестиции трейдера	|

**Пример ответа:**
```json
{
    "Account": {
        "AvailableCurrency": "USD",
        "MinBalance": 250
    },
    "Strategy": {
        "MinAmountToCreate": 200
    }
}
```

[Вернуться к содержанию](#Содержание)

### Кошельки и операции с кошельком
#### wallets.get

Полная информацию по кошельку.

**URL:** `https://ramm.store/api/client/v2/wallets.get`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
ID   | number | ID кошелька |

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
ID   | number | ID кошелька  |
Asset   | string | Название актива  |
Balance   | real | Баланс кошелька  |
Invested   | real | Инвестированная сумма  |
Margin   | real | Задействованная маржа  |
IntervalPnL   | real | Прибыль/убыток в текущем торговом интервале |
ActiveStrategiesCount| number | Количество активных стратегий  |

**Пример вызова:**
```json
{
    "ID": 12345
}
```
**Пример ответа:**
```json
{
    "ID": 12345,
    "Asset": "USD",
    "Balance": 900,
    "Invested": 123.45,
    "Margin": 12.34,
    "IntervalPnL": 23.45,
    "ActiveStrategiesCount": 2
}
```
[Вернуться к содержанию](#Содержание)
#### walletTransfers.search

Поиск переводов.

**URL:** `https://ramm.store/api/client/v2/walletTransfers.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
StrategyID   | number | ID стратегии  |
AccountID   | number | ID счета |
DealID | number | ID сделки |
DTFrom   | datetime | Начальная дата  |
DTTo   | datetime | Конечная дата  |
Type   | number | 0-fund, 1-withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners |

Допустимые поля для секции OrderBy:	
ID, StrategyID, AccountID, DealID, DT, AccrualDate, Amount, Type, Comment, StrategyName.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets и WalletTransfers:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID   | number | ID кошелька  |
Asset   | string | Название актива  |
Balance   | real | Баланс кошелька  |
Invested   | real | Инвестированная сумма  |
Margin   | real | Задействованная маржа  |
IntervalPnL   | real | Прибыль/убыток в текущем торговом интервале |
***WalletTransfers***
ID   | number | ID перевода  |
StrategyID   | number | ID стратегии  |
AccountID   | number | ID счета  |
DealID   | number | ID сделки  |
DT   | datetime | Дата перевода  |
AccrualDate   | datetime | Дата зачисления  |
Amount   | real | Сумма перевода  |
Type   | number | 0-fund, 1-withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners |
Comment   | string | Комментарий для клиента  |
StrategyName   | string | Название стратегии  |

**Пример вызова:**
```json
{
    "Filter": {
        "StrategyID": 3457
    },
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 100
    },
    "OrderBy": {
        "Field": "ID",
        "Direction": "Desc"
    }
}
```
**Пример ответа:**
```json
{
    "Filter": {
        "StrategyID": 3457,
        "DealID": 111
    },
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 100,
        "MaxPerPage": 1000
    },
    "OrderBy": {
        "Field": "ID",
        "Direction": "Desc"
    },
    "Wallets": [
        {
            "ID": 48,
            "Asset": "USD",
            "Balance": 0.132,
            "Invested": 90.07,
            "Margin": 0,
            "IntervalPnL": -9.93
        }
    ],
    "WalletTransfers": [
        {
            "ID": 101,
            "StrategyID": 3457,
            "AccountID": 1312,
            "DealID": 111,
            "DT": "2018-12-12T07:27:50.75",
            "Amount": 500,
            "Type": 5,
            "StrategyName": "SuperStrategy"
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)
### Информация о стратегиях
#### strategies.get

Получение информации о стратегии.

**URL:** `https://ramm.store/api/client/v2/strategies.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
ID	| number    | ID стратегии |
Link	| string    | Ссылка оферты |

Нужно заполнить одно из полей: ID или Link. 
Получить информацию по ID стратегии можно только в том случае, если у стратегии имеется публичная оферта и стратегия не закрыта.
Получение по Link возможно, если стратегия имеет оферту с данной ссылкой.
Массив Offers возвращается только владельцу и партнерам. Массив Offers содержит все строки при показе владельцу, а при показе партнерам массив содержит только те оферты, где в качестве партнера указан данный клиент.

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
ID	|	number	|	ID стратегии	|
Name	|	string	|	Имя стратегии	|
Yield	|	real	|	Доходность	|
MonthlyYield	|	real	|	Среднемесячная доходность|
Accounts	|	number	|	Количество инвестиций	|
DTCreated	|	datetime	|	Дата создания	|
DTStat	|	datetime	|	Дата начала сбора статистики	|
DTClosed	|	datetime	|	Дата закрытия	|
Equity	|	real	|	Суммарное эквити инвестиций в стратегию	|
IsMyStrategy	|	bool	|	Признак собственной стратегии	|
Status	|	number	|	Статус стратегии	|
***PublicOffer (вложенная структура)***
ID	|	number	|	ID оферты	|
FeeRate	|	real	|	Размер вознаграждения	|
CommissionRate |	real	|	Размер комиссии	|
***LinkOffer (вложенная структура)***
ID	|	number	|	ID оферты	|
Link	| string    | Ссылка оферты |
FeeRate	|	real	|	Размер вознаграждения	|
CommissionRate |	real	|	Размер комиссии	|
****TraderInfo (вложенная структура)****
MasterAccount | string | Логин внешнего счета
FeePaid	|	real	|	Выплаченное вознаграждение	|
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
CommissionPaid	|	real	|	Выплаченная комиссия	|
CommissionToPay	|	real	|	Невыплаченная комиссия	|
****PartnerInfo (вложенная структура)****
FeePaid	|	real	|	Выплаченное вознаграждение	|
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
CommissionPaid	|	real	|	Выплаченная комиссия	|
CommissionToPay	|	real	|	Невыплаченная комиссия	|
***Tags (вложенная структура)***
***Account (вложенная структура)***
ID	|	number	|	ID счета	|
TotalProfit	|	real	|	Прибыль по счету |
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
FeePaid	|	real	|	Выплаченное вознаграждение	|
ProfitCurrentIntervalGross	|	real	|	Прибыль в текущем торговом интервале	|
TotalCommissionPaid	|	real	|	Суммарная выплаченная комиссия	|
State	|	number	|	[Состояние счета](#Значения-AccountState)	|
Equity	|	real	|	Название валюты депозита	|
Factor	|	real	|	Повышающий/понижающий коэффициент копирования	|
AvailableToWithdraw	|	real	|	Доступные к выводу средства	|
AccountMinBalance	|	real	|	Минимальный баланс инвестиции	|
IsSecurity	|	bool	|	Признак сигнальной инвестиции	|
Target	|	real	|	Целевая доходность	|
Protection	|	real	|	Защита капитала	|
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account	|
Status	|	number	|	Статус счета	|
***Offer  (вложенная структура)***
ID	|	number	|	ID оферты	|
FeeRate	|	real	|	Размер вознаграждения	|
CommissionRate |	real	|	Размер комиссии	|


**Пример вызова:**
```json
{
    "ID": 333
}
```
**Пример ответа:**
```json
{
    "ID": 333,
    "Name": "TEST1",
    "Yield": 0.00001,
    "MonthlyYield": 0.05,
    "Accounts": 5,
    "DTCreated": "2018-09-21T11:09:38.243",
    "DTStat": "2017-09-21T11:09:38.243",
    "Equity": 1000,
    "IsMyStrategy": true,
    "Status": 0,
    "PublicOffer": {
        "ID": 123456,
        "FeeRate": 0.25,
        "CommissionRate": 5
    },
    "LinkOffer": {
        "ID": 123457,
        "Link": "cca6x1qoq1",
        "FeeRate": 0.2,
        "CommissionRate": 2
    },
    "Tags": {
        "Youtube": "BERFDOJK8"
    },
    "Account": {
        "ID": 4545,
        "TotalProfit": 512.65,
        "FeeToPay": 54.56,
        "FeePaid": 101.58,
        "ProfitCurrentIntervalGross": 152.23,
        "TotalCommissionPaid": 25.34,
        "State": 0,
        "Equity": 1500.56,
        "Factor": 1,
        "AvailableToWithdraw": 1000,
        "AccountMinBalance": 100,
        "IsSecurity": false,
        "Target": 5000,
        "Protection": 500,
        "Type": 2,
        "Status": 0,
        "Offer": {
            "ID": 123456,
            "FeeRate": 0.25,
            "CommissionRate": 5,
            "IsPublic": true
        }
    }
}
```
[Вернуться к содержанию](#Содержание)
#### charts.get

Получение графика доходности для заданной стратегии.

**URL:** `https://ramm.store/api/client/v2/charts.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|	number	|	ID стратегии	|
MaxPoints	|	number	|	Макс.количество точек графика. Варианты: 25, 250 |

Другие поддерживаемые варианты MaxPoints могут быть реализованы по запросу.

**Возвращаемые данные:**

Возвращаемые данные - массив Chart:

Параметр | Тип | Описание 
---------|----------|----------
***Chart***
DT	|	datetime	|	Дата/время	|
Yield	|	real	|	Значение доходности	|


**Пример вызова:**
```json
{
    "StrategyID": 445,
    "MaxPoints": 25
}
```
**Пример ответа:**
```json
{
    "Chart": [
        {
            "DT": "2018-12-13T00:00:00",
            "Yield": 0.626
        },
        {
            "DT": "2018-12-14T00:00:00",
            "Yield": 1.62
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)
#### strategyportfolio.get

Получение информации о текущем портфеле позиций для заданной стратегии.

**URL:** `https://ramm.store/api/client/v2/strategyportfolio.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	| number	| ID стратегии |

**Возвращаемые данные:**

Возвращаемые данные - массив StrategyPortfolio:

Параметр | Тип | Описание 
---------|----------|----------
***StrategyPortfolio***
Symbol	|	string	|	Название инструмента	|
Share	|	real	|	Доля инструмента	|

**Пример вызова:**
```json
{
    "StrategyID": 445
}
```
**Пример ответа:**
```json
{
    "StrategyPortfolio": [
        {
            "Symbol": "EURUSD",
            "Share": 0.75
        },
        {
            "Symbol": "GBPUSD",
            "Share": 0.25
        }
    ]
}
```

[Вернуться к содержанию](#Содержание)
#### strategysymbolstat.get

Отдает информацию по статистике использования различных торговых инструментов стратегией.

**URL:** `https://ramm.store/api/client/v2/strategysymbolstat.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

**Возвращаемые данные:**

Возвращаемые данные - массив StrategySymbolStat:

Параметр | Тип | Описание 
---------|----------|----------
***StrategySymbolStat***
Symbol	|	string	|	Название символа	|
Share	|	real	|	Доля символа	|

**Пример вызова:**
```json
{
    "StrategyID": 445
}
```
**Пример ответа:**
```json
{
    "StrategySymbolStat": [
        {
            "Symbol": "EURUSD",
            "Share": 50.2
        },
        {
            "Symbol": "GBPUSD",
            "Share": 49.8
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### strategies.search

Поиск стратегий с фильтрацией и сортировками.

**URL:** `https://ramm.store/api/client/v2/strategies.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
Name	|	string	|	Подстрока поиска	|
MyActiveAccounts	|	boolean	|	если = true, то ищем только стратегии с активными инвестициями данного клиента	|
MyStrategies	|	boolean	|	если = true, то ищем собственные стратегии клиента	|
ActiveStrategies	|	boolean	|	если = true, то ищем только активные стратегии	|
AgeMin |	number	|	минимальный возраст в днях |
DealsMin |	number	|	минимальное количество сделок |

Допустимые поля для секции OrderBy:	
ID, Name, DT, DTStat, DTClosed, PublicOffer.FeeRate, PublicOffer.CommissionRate, Status, Yield, MonthlyYield, Accounts, Symbols, IsMyStrategy, Account.ID, Account.IsSecurity, Account.Type, Account.AccountSpecAssetID, Account.Asset, Account.TradingIntervalCurrentID, Account.DTCreated, Account.Balance, Account.Equity, Account.Margin, Account.MarginLevel, Account.IntervalPnL, Account.Status, Account.Factor, Account.MCReached, Account.Protection, Account.ProtectionEquity, Account.ProtectionReached, Account.Target, Account.TargetEquity, Account.TargetReached, Account.Positions, Account.AccountMinBalance, Account.AvailableToWithdraw, Account.FeePaid, Account.FeeToPay.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets и Strategies:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID	|	number	|	ID кошелька (bigint)		|
Asset	|	string	|	Название актива		|
Balance	|	real	|	Сумма в кошельке		|
Invested	|	real	|	Инвестированная сумма		|
Margin	|	real	|	Задействованная маржа		|
ProfitCurrentIntervalGross	|	real	|	Прибыль/убыток в текущем торговом интервале		|
***Strategies***
ID	|	number	|	ID стратегии		|
Name	|	string	|	Название стратегии (Varchar(64))		|
DTCreated	|	datetime	|	Дата создания стратегии		|
DTStat	|	datetime	|	Дата сбора статистики		|
DTClosed	|	datetime	|	Дата закрытия стратегии		|
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed		|
Yield	|	real	|	Прибыль в %		|
MonthlyYield	|	real	|	Среднемесячная прибыль в %		|
Accounts	|	number	|	Количество счетов		|
Symbols	|	string	|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)		|
IsMyStrategy	|	boolean	|	Признак собственной стратегии		|
***Tags (вложенная структура)***
****PublicOffer (вложенная структура)****
ID	|	number	|	ID публичной оферты		|
FeeRate	|	real	|	Вознаграждение с прибыли (numeric (3,2))		|
CommissionRate	|	number	|	Размер комиссии в долларах на млн оборота в долларах 		|
****TraderInfo (вложенная структура)****
MasterAccount | string | Логин внешнего счета
FeePaid	|	real	|	Выплаченное вознаграждение	|
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
CommissionPaid	|	real	|	Выплаченная комиссия	|
CommissionToPay	|	real	|	Невыплаченная комиссия	|
****PartnerInfo (вложенная структура)****
FeePaid	|	real	|	Выплаченное вознаграждение	|
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
CommissionPaid	|	real	|	Выплаченная комиссия	|
CommissionToPay	|	real	|	Невыплаченная комиссия	|
****Account (вложенная структура)****
ID	|	number	|	ID счета		|
IsSecurity	|	boolean	|	Признак счета управляющего		|
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account		|
AccountSpecAssetID	|	number	|	Спецификация счета для заданного актива		|
Asset	|	string	|	Название валюты счета		|
TradingIntervalCurrentID	|	number	|	ID текущего торгового интервала		|
DTCreated	|	datetime	|	Дата создания		|
Balance	|	real	|	Баланс счета		|
Equity	|	real	|	Эквити		|
Margin	|	real	|	Задействованная маржа		|
MarginLevel	|	real	|	Уровень маржи		|
ProfitCurrentIntervalGross	|	real	|	Прибыль/убыток в текущем торговом интервале		|
TotalProfit | real	| итого прибыль по счету	|
Status	|	number	|	0-new (without money), 1-active (trading), 2-MC, 3-ProtectionTarget, 4-Pause, 5-disabled (cant trade), 6-closed (cant activate)		|
Factor	|	real	|	Повышающий/понижающий коэффициент копирования		|
MCReached	|	number	|	Дата/время срабатывания StopOut		|
Protection	|	real	|	Процент защиты счета (numeric (4,3))		|
ProtectionEquity	|	real	|	Значение эквити, при котором сработает защита счета		|
ProtectionReached	|	number	|	Дата/время срабатывания защиты счета		|
Target	|	real	|	Целевая доходность (numeric (8,3))		|
TargetEquity	|	real	|	Целевая доходность в валюте счета		|
TargetReached	|	number	|	Дата/время достижения целевой доходности		|
Positions	|	number	|	Количество открытых		|
AccountMinBalance	|	real	|	Минимальный баланс счета		|
AvailableToWithdraw	|	real	|	Средства, доступные к выводу		|
FeePaid	|	real	|	Выплаченное вознаграждение		|
FeeToPay	|	real	|	Невыплаченное вознаграждение		|
****Offer (вложенная структура)****
ID	|	number	|	ID оферты счета		|
FeeRate	|	real	|	Вознаграждение с прибыли (numeric (3,2))		|
CommissionRate	|	number	|	Размер комиссии в долларах на млн оборота в долларах 		|
****Chart (вложенный массив)****
Yield	|	real	|	Прибыль в %	|

**Пример вызова:**
```json
{
    "Filter": {
        "Name": "TEST",
        "MyActiveAccounts": true
    },
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 5
    },
    "OrderBy": {
        "Field": "ID",
        "Direction": "Desc"
    }
}
```
**Пример ответа:**
```json
{
    "Filter": {
        "Name": "TEST",
        "MyActiveAccounts": true
    },
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 5,
        "MaxPerPage": 100
    },
    "Wallets": [
        {
            "ID": 48,
            "Asset": "USD",
            "Balance": 0.132,
            "Invested": 90.07,
            "Margin": 0,
            "ProfitCurrentIntervalGross": -9.93
        }
    ],
    "Strategies": [
        {
            "ID": 341,
            "Name": "TEST_1",
            "DTCreated": "2018-09-21T11:09:38.23",
            "DTStat": "2017-09-21T11:09:38.23",
            "PublicOffer": {
                "ID": 23148,
                "Fee": 0.25,
                "Commission": 10
            },
            "Status": 2,
            "Yield": 1.076,
            "MonthlyYield": 0.07,
            "Accounts": 17,
            "Symbols": "EURUSD",
            "IsMyStrategy": 1,
            "Tags": {
                "Youtube": "BERFDOJK8"
            },
            "Account": {
                "ID": 1185,
                "IsSecurity": true,
                "Type": 0,
                "AccountSpecAssetID": 5,
                "Asset": "USD",
                "TradingIntervalCurrentID": 164,
                "DTCreated": "2018-09-21T11:09:38.243",
                "Balance": 1000.46,
                "Equity": 1006.64,
                "Margin": 2.27,
                "MarginLevel": 5.34,
                "ProfitCurrentIntervalGross": 6.64,
                "TotalProfit": 3.32,
                "Status": 3,
                "Factor": 1,
                "Protection": 0.01,
                "ProtectionEquity": 10,
                "Target": 0.01,
                "TargetEquity": 1010,
                "TargetReached": "2018-12-12T15:34:54.217",
                "Positions": 2,
                "Offer": {
                    "ID": 23140,
                    "Fee": 0.0,
                    "Commission": 0
                }
            },
            "Chart": [
                {
                    "Yield": -0.391
                },
                {
                    "Yield": -2.886
                },
                {
                    "Yield": -4.735
                },
                {
                    "Yield": -9.322
                }
            ]
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### strategies.getOffers

Получение доступных оферт для трейдера и партнера.
Партнер получает информацию по офертам, в которых он указаны в качестве партнера.
Владелец стратегии получает полную информацию.

**URL:** `https://ramm.store/api/client/v2/strategies.getOffers`

**Параметры:**
Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|	number	|	ID стратегии	|

**Возвращаемые данные:**

Возвращаемые данные - массив Offers:

Параметр | Тип | Описание 
---------|----------|----------
***Offers***
ID	|	number	|	ID оферты	|
DTCreated	|	datetime	|	Дата создания стратегии		|
FeeRate	|	real	|	Вознаграждение с прибыли	|
CommissionRate	|	real	|	Размер комиссии	|
PartnerID |	number	|	ID партнера	|
PartnerShareRate |	real	|	Доля вознаграждения партнера	|
Link | string | Cсылка на оферту. Отображается партнеру и владельцу стратегии. |
IsPublic |	boolean	|	Является ли оферта публичной. Для непубличных оферт не передается.	|
Status | number | 0-active, 1-disabled (new investments prohibited), 2-closed (all active investments will closed) |
Description | string | Описание, заданное при создании оферты. Отображается только владельцу стратегии. | 
ActiveAccounts |	number	|	количество активных счетов	|
TotalAccounts |	number	|	количество счетов всего	|
FeePaid	|	real	|	Выплаченное вознаграждение	|
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
CommissionPaid	|	real	|	Выплаченная комиссия	|
CommissionToPay	|	real	|	Невыплаченная комиссия	|

**Пример вызова:**
```json
{
    "StrategyID": 12345
}
```
**Пример ответа:**
```json
{
    "Offers": [
        {
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

### Собственные стратегии клиента
#### myStrategies.add

Создание клиентом новой стратегии.

**URL:** `https://ramm.store/api/client/v2/myStrategies.add`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
Name	|	string	|	Название стратегии	|
****Account (вложенная структура)****
Protection	|	real	|	Процент защиты счета	|
Target	|	real	|	Целевая доходность	|
Money	|	real	|	Сумма на собственном счете трейдера	|

**Возвращаемые данные:**

Возвращаемые данные - ID стратегии, счета трейдра и команды на пополнение. После создания стратегии нужно создать оферту и, если требуется, сделать ее публичной.

Параметр | Тип | Описание 
---------|----------|----------
StrategyID | number	|	ID стратегии	| 
AccountID | number	|	ID счета трейдера	| 
AccountCommandID	|	number	|	ID команды на пополнение счета трейдера	|

**Пример вызова:**
```json
{
    "Name": "TEST_1",
    "Account": {
        "Protection": 0.5,
        "Target": 1.5,
        "Money": 1000
    }
}
```
**Пример ответа:**
```json
{
    "StrategyID": 12345,
    "AccountID": 654321,
    "AcountCommandID": 7654321
}
```

[Вернуться к содержанию](#Содержание)

#### myStrategies.addOffer

Создание новой оферты для стратегии.

**URL:** `https://ramm.store/api/client/v2/myStrategies.addOffer`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|
FeeRate	|	real	|	Вознаграждение с прибыли	|
CommissionRate	|	real	|	Размер комиссии	|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
OfferID	| number	|ID созданной оферты 
Link | string | ссылка на оферту

**Пример вызова:**
```json
{
    "StrategyID": 12345,
    "FeeRate": 0.25,
    "CommissionRate": 0
}
```
**Пример ответа:**
```json
{
    "OfferID": 5654,
    "Link": "cca6x1qoq1"
}
```

#### myStrategies.setPublicOffer

Установка публичной оферты для стратегии. Сброс публичной оферты.

**URL:** `https://ramm.store/api/client/v2/myStrategies.setPublicOffer`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|
OfferID	|	real	|	ID оферты, которую нужно сделать публичной. Если параметр не передан, то все оферты станут непубличными. 	|

**Возвращаемые данные:**

В случае успешной установки возвращается ответ 200 OK.

**Пример вызова:**
```json
{
    "StrategyID": 12345,
    "OfferID": 23456
}
```

[Вернуться к содержанию](#Содержание)

#### myStrategies.close

Закрывает стратегию.

**URL:** `https://ramm.store/api/client/v2/myStrategies.close`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды закрытия счета

**Пример вызова:**
```json
{
    "StrategyID": 445
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.pause

Ставит стратегию на "Паузу" (копирование сигналов временно прекращается, все позиции закрываются).

**URL:** `https://ramm.store/api/client/v2/myStrategies.pause`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды

**Пример вызова:**
```json
{
    "StrategyID": 445
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.resume

Снимает стратегию с "Паузы" (копирование сигналов возобновляется). При возобновлении торговли на стратегии выставляется признак NeedSync, который используется в торговом АПИ для отправки синхронизации.

**URL:** `https://ramm.store/api/client/v2/myStrategies.resume`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды

**Пример вызова:**
```json
{
    "StrategyID": 445
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.getToken

Возвращает текущий токен стратегии.

**URL:** `https://ramm.store/api/client/v2/myStrategies.getToken`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
Token	|string	|Токен, уникальный идентификатор

**Пример вызова:**
```json
{
    "StrategyID": 445
}
```
**Пример ответа:**
```json
{
    "Token": "A0AA3ED3-FCC1-459A-8F30-B1DD0E7EA0C9"
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.setToken

Получение нового токена стратегии (старый немедленно перестает работать).

**URL:** `https://ramm.store/api/client/v2/myStrategies.setToken`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
Token	|string	|Токен, уникальный идентификатор

**Пример вызова:**
```json
{
    "StrategyID": 445
}
```
**Пример ответа:**
```json
{
    "Token": "A0AA3ED3-FCC1-459A-8F30-B1DD0E7EA0C9"
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.checkName

Проверка имени новой стратегии на уникальность.

**URL:** `https://ramm.store/api/client/v2/mystrategies.checkName`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
Name	|string	|Название стратегии|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
StrategyNameAvailable	|boolean |Признак уникальности имени

**Пример вызова:**
```json
{
    "Name": "Strategy1"
}
```
**Пример ответа:**
```json
{
    "StrategyNameAvailable": 0
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.getActiveAccounts

Поиск активных инвестиций в стратегию.

**URL:** `https://ramm.store/api/client/v2/myStrategies.getActiveAccounts`

**Параметры:**

Может содержать секции [Pagination, OrderBy](#Методы-поиска-данных).

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

Допустимые поля для секции OrderBy:	
ID, DT, Equity, ProfitCurrentIntervalGross, FeePaid, TotalCommissionTrader, FeeToPay, IsMyAccount, TotalProfitNet.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, OrderBy, массив Accounts:

Параметр | Тип | Описание 
---------|----------|----------
StrategyID	|number	|ID стратегии
***Accounts***
ID	|	number	|	ID инвестиции	|
DT	|	datetime	|	Дата создания инвестиции	|
Equity	|	real	|	Эквити инвестиции	|
ProfitCurrentIntervalGross	|	real	|	Грязная прибыль за текущий торговый интервал	|
FeePaid	|	real	|	Выплаченное вознаграждение	|
TotalCommissionTrader	|	real	|	Общая сумма комиссии трейдера	|
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
TotalProfitNet	|	real	|	Суммарная чистая прибыль	|
IsMyAccount	|	boolean	|	Признак собственного счета	|

**Пример вызова:**
```json
{
    "StrategyID": 3256,
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 5
    },
    "OrderBy": {
        "Field": "ID",
        "Direction": "Desc"
    }
}
```
**Пример ответа:**
```json
{
    "StrategyID": 3256,
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 5,
        "MaxPerPage": 100
    },
    "Accounts": [
        {
            "ID": 48,
            "DT": "2019-02-07T14:28:09.580",
            "Equity": 1000,
            "ProfitCurrentIntervalGross": 100,
            "FeePaid": 10,
            "TotalCommissionTrader": 5,
            "FeeToPay": 7.93,
            "TotalProfitNet": 56.45,
            "IsMyAccount": 1
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.getClosedAccounts

Поиск закрытых инвестиций в стратегию.

**URL:** `https://ramm.store/api/client/v2/myStrategies.getClosedAccounts`

**Параметры:**

Может содержать секции [Pagination, OrderBy](#Методы-поиска-данных).

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|

Допустимые поля для секции OrderBy:	
ID, DT, DTClosed, FeePaid, TotalCommissionTrader, IsMyStrategy,TotalProfitNet.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, OrderBy, массив Accounts:

Параметр | Тип | Описание 
---------|----------|----------
StrategyID	|number	|ID стратегии
***Accounts***
ID	|	number	|	ID инвестиции
DT	|	datetime	|	Дата создания инвестиции
DTClosed	|	datetime	|	Дата закрытия инвестиции
FeePaid	|	real	|	Выплаченное вознаграждение
TotalCommissionTrader	|	real	|	Общая сумма комиссии трейдера
TotalProfitNet	|	real	|	Суммарная чистая прибыль
IsMyStrategy	|	boolean	|	Признак собственной стратегии

**Пример вызова:**
```json
{
    "StrategyID": 3256,
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 5
    },
    "OrderBy": {
        "Field": "ID",
        "Direction": "Desc"
    }
}
```
**Пример ответа:**
```json
{
    "StrategyID": 3256,
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 5,
        "MaxPerPage": 100
    },
    "Accounts": [
        {
            "ID": 48,
            "DT": "2019-02-07T14:28:09.580",
            "DTClosed": "2020-02-07T14:28:09.580",
            "FeePaid": 10,
            "TotalCommissionTrader": 5,
            "TotalProfitNet": 56.45,
            "IsMyStrategy": 1
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### strategyCommands.get

Получение текущего статуса команды управления стратегиями по ее ID и ID стратегии.

**URL:** `https://ramm.store/api/client/v2/strategyCommands.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyCommandID |number	|ID команды
StrategyID |number	|ID стратегии


**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
StrategyCommandStatus	|number |Статус команды (0-new, 1-ok, 2-reject, 3-error)

**Пример вызова:**
```json
{
    "StrategyCommandID": 445,
    "StrategyID": 223
}
```
**Пример ответа:**
```json
{
    "StrategyCommandStatus": 0
}
```
[Вернуться к содержанию](#Содержание)

### Торговые счета клиентов
[Вернуться к содержанию](#Содержание)

#### accounts.add

Создание счета.

**URL:** `https://ramm.store/api/client/v2/accounts.add`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|	number	|	ID стратегии
OfferID	|	number	|	ID оферты
Link	|	string	|	Уникальный код ссылки
Factor	|	real	|	Повышающий/понижающий коэффициент копирования
Protection	|	real	|	Процент защиты счета (numeric (4,3))
Target	|	real	|	Целевая доходность (numeric (8,3))
Money	|	real	|	Сумма счета (numeric (28,2))

Метод может использоваться как для инвестирования в публичную стратегию, так и для инвестирования по скрытой ссылке.

*Инвестирование по публичной оферте:*
Должны быть заполнены параметры StrategyID и OfferID, а Link должен отсутствовать. OfferID необходим для избежания ситуаций, когда трейдер меняет оферту, а инвестор в этом время инвестирует и получает не те условия, которые он до этого видел.

*Инвестирование по скрытой ссылке:*
Должен передаваться Link, а параметры StrategyID и OfferID должны отсутствовать.

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
AccountID	|number	|ID счета
CommandBalanceID	|number	|ID команды пополнения счета


**Пример вызова для инвестирования по публичной оферте:**
```json
{
    "StrategyID": 4566,
    "OfferID": 5678,
    "Factor": 1,
    "Protection": 0.5,
    "Target": 1,
    "Money": 500
}
```
**Пример вызова для инвестирования по скрытой ссылке:**
```json
{
    "Link": "cca6x1qoq1",
    "Factor": 1,
    "Protection": 0.5,
    "Target": 1,
    "Money": 500
}
```
**Пример ответа:**
```json
{
    "AccountID": 225,
    "CommandBalanceID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.fund

Пополняет счет.

**URL:** `https://ramm.store/api/client/v2/accounts.fund`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	|ID счета
Amount	|real	|Сумма пополнения

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
AccountID	|number	|ID счета
CommandBalanceID	|number	|ID команды пополнения счета


**Пример вызова:**
```json
{
    "AccountID": 445,
    "Amount": 500
}
```
**Пример ответа:**
```json
{
    "AccountID": 445,
    "CommandBalanceID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.withdraw

Снимает средства со счета.

**URL:** `https://ramm.store/api/client/v2/accounts.withdraw`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	|ID счета
Amount	|real	|Сумма пополнения

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
AccountID	|number	|ID счета
CommandBalanceID	|number	|ID команды снятия средств


**Пример вызова:**
```json
{
    "AccountID": 445,
    "Amount": 500
}
```
**Пример ответа:**
```json
{
    "AccountID": 445,
    "CommandBalanceID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.close

Закрывает счет.

**URL:** `https://ramm.store/api/client/v2/accounts.close`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	|ID счета

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды закрытия


**Пример вызова:**
```json
{
    "AccountID": 445
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.get

Получение информации о счете.

**URL:** `https://ramm.store/api/client/v2/accounts.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	| number    | ID счета|


**Возвращаемые данные:**

Возвращаемые данные содержат структуру Strategy:

Параметр | Тип | Описание 
---------|----------|----------
ID	|	number	|	ID счета
DT	|	datetime	|	Дата создания
DTClosed	|	datetime	|	Дата закрытия
Fee |	real	|	Размер вознаграждения
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account
IsSecurity	|	boolean	|	Признак сигнальной инвестиции
Status	|	number	|	см.ниже
Balance	|	real	|	Баланс счета
Equity	|	real	|	Эквити
AvailableToWithdraw	|	real	|	Доступно для снятия
AccountMinBalance	|	real	|	Минимальный баланс счета
Factor	|	real	|	Повышающий/понижающий коэффициент копирования
Margin	|	real	|	Задействованная маржа
MarginLevel	|	real	|	Уровень маржи
MCReached	|	number	|	Дата/время срабатывания StopOut
Protection	|	real	|	Процент защиты счета (numeric (4,3))
ProtectionEquity	|	real	|	Значение эквити, при котором сработает защита счета
ProtectionReached	|	number	|	Дата/время срабатывания защиты счета
Target	|	real	|	Целевая доходность (numeric (8,3))
TargetEquity	|	real	|	Целевая доходность в валюте счета
TargetReached	|	number	|	Дата/время достижения целевой доходности
ProfitBase	|	real	|	База для подсчета вознаграждения
AssetName	|	string	|	Название валюты счета
Precision	|	number	|	Точность округления, знаки после запятой
PositionsCount	|	number	|	Количество позиций
TotalProfitGross	|	real	|	"Грязная" прибыль, до вычета вознаграждения
***Strategy***
ID	|	number	|	ID стратегии
Name	|	string	|	Имя стратегии
Status	|	number	|	Код статуса стратегии

**Пример вызова:**
```json
{
    "AccountID": 333
}
```
**Пример ответа:**
```json
{
    "ID": 22,
    "Strategy": {
        "ID": 17,
        "Name": "TEST1",
        "Status": 0
    },
    "Fee": 0.25,
    "DT": "2019-01-24T10:05:39.960",
    "DTClosed": "2019-02-07T14:28:09.580",
    "Type": 0,
    "IsSecurity": true,
    "Status": 6,
    "Balance": 0,
    "Equity": 0,
    "AvailableToWithdraw": 0,
    "Factor": 1,
    "Margin": 0,
    "Protection": 0.5,
    "ProtectionEquity": 501.795,
    "Target": 1,
    "TargetEquity": 2007.18,
    "ProfitBase": 61.87,
    "AssetName": "USD",
    "Precision": 2,
    "PositionsCount": 0
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.getStatement

Получение стейтмента счета.

**URL:** `https://ramm.store/api/client/v2/accounts.getStatement`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	| number    | ID счета|


**Возвращаемые данные:**

Возвращаемые данные содержат массив Statement:

Параметр | Тип | Описание 
---------|----------|----------
***Statement***
CurrentDate	|	number	|	Дата получения стейтмента
****Strategy (вложенная структура)****
ID	|	number	|	ID стратегии
Name	|	string	|	Имя стратегии
Fee	|	real	|	Размер вознаграждения
Commission	|	real	|	Размер комиссии
IsMyStrategy	|	bool	|	Признак собственной стратегии
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed
****Account (вложенная структура)****
ID	|	number	|	ID счета
IsMyAccount	|	boolean	|	Признак собственной инвестиции
IsSecurity	|	bool	|	Признак сигнальной инвестиции
IDCompany	|	number	|	ID компании
DT	|	datetime	|	Дата создания
DTClosed	|	datetime	|	Дата закрытия
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account
AssetName	|	string	|	Название валюты депозита
LeverageMax	|	number	|	Максимальное плечо
MCLevel	|	number	|	Уровень StopOut
Balance	|	real	|	Баланс счета
Margin	|	real	|	Использованная маржа счета
State	|	number	|	[Состояние счета](#Значения-AccountState)
Factor	|	real	|	Повышающий/понижающий коэффициент копирования
Target	|	real	|	Целевая доходность (numeric (8,3))
Protection	|	real	|	Процент защиты счета (numeric (4,3))
TargetEquity	|	real	|	Целевая доходность в валюте счета
ProtectionEquity	|	real	|	Значение эквити, при котором сработает защита счета
Equity	|	real	|	Эквити
FreeMargin	|	real	|	Свободная маржа
MarginLevel	|	real	|	Уровень маржи
Status	|	number	|	0-new (without money), 1-active (trading), 2-MC, 3-ProtectionTarget, 4-Pause, 5-disabled (cant trade), 6-closed (cant activate)
AccountMinBalance	|	real	|	Минимальный баланс инвестиции
AvailableToWithdraw	|	real	|	Средства, доступные к выводу

**Пример вызова:**
```json
{
  "AccountID": "1000005"
}
```
**Пример ответа:**
```json
{
    "Statement": [
        {
            "CurrentDate": "2020-03-24T08:52:30.233",
            "Strategy": {
                "ID": 744,
                "Name": "EURUSD_sell",
                "Fee": 0.25,
                "Commission": 0,
                "IsMyStrategy": true,
                "Status": 1
            },
            "Account": {
                "ID": 1000005,
                "IsMyAccount": false,
                "IsSecurity": 0,
                "IDCompany": 9,
                "DT": "2020-01-14T09:58:04.403",
                "Type": 2,
                "AssetName": "USD",
                "LeverageMax": 50,
                "MCLevel": 20,
                "Balance": 2900.82,
                "Margin": 0,
                "State": 2,
                "Factor": 1,
                "Target": 1,
                "Protection": 0.5,
                "TargetEquity": 10000,
                "ProtectionEquity": 2500,
                "Equity": 2900.82,
                "FreeMargin": 2900.82,
                "Status": 1,
                "AccountMinBalance": 200,
                "AvailableToWithdraw": 2700.82
            }
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.pause

Ставит счет на паузу (временное прекращение копирования с закрытием всех открытых позиций)
ВНИМАНИЕ. Счет трейдера этим методом поставить на паузу нельзя! Используйте метод myStrategies.pause для постановки на паузу всей стратегии.

**URL:** `https://ramm.store/api/client/v2/accounts.pause`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	|ID счета

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды постановки на паузу


**Пример вызова:**
```json
{
    "AccountID": 445
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.resume

Снимает счет с паузы (возобновление копирования, открытие всех позиций стратегии по текущим ценам).
ВНИМАНИЕ. Счет трейдера этим методом снять с паузы нельзя! Используйте метод myStrategies.resume для снятия с паузы всей стратегии.

**URL:** `https://ramm.store/api/client/v2/accounts.resume`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	|ID счета

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды снятия с паузы


**Пример вызова:**
```json
{
    "AccountID": 445
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.setFactor

Устанавливает на счета значение повышающего/понижающего коэффициента копирования.

**URL:** `https://ramm.store/api/client/v2/accounts.setFactor`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	| ID счета
Factor	|real	| Значение коэффициента для установки

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды установки коэффициента


**Пример вызова:**
```json
{
    "AccountID": 445,
    "Factor": 1.5
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.setProtection

Устанавливает процент защиты счета.

**URL:** `https://ramm.store/api/client/v2/accounts.setProtection`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	| ID счета
Protection	|real	| Значение процента защиты счета (numeric (4,3))

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды установки защиты


**Пример вызова:**
```json
{
    "AccountID": 445,
    "Protection": 0.5
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.setTarget

Устанавливает целевую доходность счета.

**URL:** `https://ramm.store/api/client/v2/accounts.setTarget`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	| ID счета
Target	|real	| Значение целевой доходности (numeric (8,3))

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды установки доходности


**Пример вызова:**
```json
{
    "AccountID": 445,
    "Target": 1.5
}
```
**Пример ответа:**
```json
{
    "CommandID": 5654
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.searchClosed

Поиск закрытых счетов и соответствующих им стратегий с фильтрацией по подстроке (из имени стратегии).

**URL:** `https://ramm.store/api/client/v2/accounts.searchClosed`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:

Поле | Тип | Описание 
:--------|----------|----------
Name	|	string	|	Подстрока поиска	|

Допустимые поля для секции OrderBy:	
ID, Name, DTCreated, DTStat, DTClosed, Status, Yield, MonthlyYield, Accounts, Symbols, IsMyStrategy, PublicOffer.FeeRate, PublicOffer.CommissionRate, Account.ID, Account.Type, Account.Asset, Account.DTCreated, Account.DTClosed, Account.TotalProfit.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets и Strategies:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID	|	number	|	ID кошелька (bigint)
Asset	|	string	|	Название актива
Balance	|	real	|	Сумма в кошельке
Invested	|	real	|	Инвестированная сумма
Margin	|	real	|	Задействованная маржа
TotalProfit	|	real	|	Прибыль/убыток в текущем торговом интервале
****Strategies****
ID	|	number	|	ID стратегии		|
Name	|	string	|	Название стратегии (Varchar(64))		|
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed		|
Symbols	|	string	|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)		|
IsMyStrategy	|	boolean	|	Признак собственной стратегии		|
ActiveAccountID |	number	|	Номер счета активной инвестиции в ту же стратегию (для определения возможных действий по стратегии)
****PublicOffer (вложенная структура)****
ID	|	number	|	ID публичной оферты		|
FeeRate	|	real	|	Вознаграждение с прибыли (numeric (3,2))		|
CommissionRate	|	number	|	Размер комиссии в долларах на млн оборота в долларах 		|
***Account (вложенная структура)***
ID	|	number	|	ID счета
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account
Asset	|	string	|	Название валюты счета
DTCreated	|	datetime	|	Дата создания
DTClosed	|	datetime	|	Дата закрытия счета
TotalProfit	|	real	|	Итоговый профит
****Offer (вложенная структура)****
ID	|	number	|	ID оферты счета		|
FeeRate	|	real	|	Вознаграждение с прибыли (numeric (3,2))		|
CommissionRate	|	number	|	Размер комиссии в долларах на млн оборота в долларах 		|

**Пример вызова:**
```json
{
    "Filter": {
        "Value": "Strategy"
    },
    "OrderBy": {
        "Field": "Strategy.Name",
        "Direction": "Desc"
    },
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 10
    }
}
```
**Пример ответа:**
```json
{
    "Filter": {
        "Value": "Strategy"
    },
    "OrderBy": {
        "Field": "Strategy.Name",
        "Direction": "Desc"
    },
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 10,
        "MaxPerPage": 100
    },
    "Wallets": [
        {
            "ID": 11120,
            "Asset": "USD",
            "Balance": 9000.6,
            "Invested": 914.63,
            "Margin": 0,
            "IntervalPnL": -85.37
        }
    ],
    "Strategies": [
        {
            "ID": 1252,
            "Name": "Strategy158",
            "Status": 1,
            "Symbols": "EURUSD",
            "IsMyStrategy": true,
            "ActiveAccountID": 1000357,
            "Offer": {
                "ID": 123456,
                "FeeRate": 0.25,
                "CommissionRate": 5
            },
            "Account": {
                "ID": 1000196,
                "IsSecurity": true,
                "Type": 0,
                "Asset": "USD",
                "DTCreated": "2020-03-20T14:29:13.697",
                "DTClosed": "2020-03-20T15:32:57.123",
                "Offer": {
                    "ID": 123456,
                    "FeeRate": 0.25,
                    "CommissionRate": 5
                }
            }
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.getCharts

Получение графика заданного типа для заданного счета.

**URL:** `https://ramm.store/api/client/v2/accounts.getCharts`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|	number	|	ID стратегии
MaxPoints	|	number	|	Макс.количество точек графика
chartType	|	string	|	Тип графика (yield, yield-leverage)
chartSize	|	string	|	Размер графика (full, large, medium, small)


**Возвращаемые данные:**

Возвращаемые данные - массив Chart:

Параметр | Тип | Описание 
---------|----------|----------
***Chart***
DT	|datetime	|Дата/время
Yield	|real	|Значение доходности
Equity	|real	|Эквити счета


**Пример вызова:**
```json
{
    "AccountID": 445,
    "chartType": "yield",
    "MaxPoints": 10
}
```
**Пример ответа:**
```json
{
    "Chart": [
        {
            "DT": "2018-12-13T00:00:00",
            "Yield": 0.5,
            "Equity": 1500
        },
        {
            "DT": "2018-12-14T00:00:00",
            "Yield": 1,
            "Equity": 2000
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.searchSpec

Получение настроек счетов клиента.

**URL:** `https://ramm.store/api/client/v2/accounts.searchSpec`

**Параметры: отсутствуют**

**Возвращаемые данные:**

Возвращаемые данные - структура AccountSpecAsset:

Параметр | Тип | Описание 
---------|----------|----------
***AccountSpecAsset***
IDAccountSpecAsset	|	number	|	ID cпецификация счета для заданного актива
IDSpec	|	number	|	ID спецификации
IDAsset	|	number	|	ID актива
IDStreamDefault	|	number	|	ID стрима по умолчанию (для инвесторов)
IDStreamDefaultSecurity	|	number	|	ID стрима по умолчанию (для трейдеров)
AccountMinBalance	|	real	|	Минимальный баланс инвестиции (для инвестора)
SecurityMinBalance	|	real	|	Минимальный баланс инвестиции (для трейдера)
Precision	|	number	|	Точность (знаки после запятой)

**Пример ответа:**
```json
{
    "AccountSpecAsset": {
        "IDAccountSpecAsset": 1,
        "IDSpec": 1,
        "IDAsset": 2,
        "IDStreamDefault": 10,
        "IDStreamDefaultSecurity": 11,
        "AccountMinBalance": 100,
        "SecurityMinBalance": 200,
        "Precision": 2
    }
}
```
[Вернуться к содержанию](#Содержание)

#### accountCommands.get

Получение текущего статуса команды управления счетами по ее ID и ID счета.

**URL:** `https://ramm.store/api/client/v2/accountCommands.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountCommandID	|number	|ID команды
AccountID	|number	|ID счета

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
AccountCommandStatus	|number	|Статус команды (0-new, 1-ok, 2-reject, 3-error)

**Пример вызова:**
```json
{
    "AccountCommandID": 445,
    "AccountID": 223
}
```
**Пример ответа:**
```json
{
    "AccountCommandStatus": 0
}
```
[Вернуться к содержанию](#Содержание)

### Открытые позиции

#### positions.search

Поиск открытых позиций с фильтрацией по номеру счета.

**URL:** `https://ramm.store/api/client/v2/positions.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	|ID счета

Допустимые поля для секции OrderBy:	
ID, Symbol, Volume, Price, Margin, ProfitCalcQuote, Profit, Swap, TotalProfit.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets, PositionsTotal и Positions:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID	|	number	|	ID кошелька
Asset	|	string	|	Название актива
Balance	|	real	|	Сумма в кошельке
Invested	|	real	|	Инвестированная сумма
Margin	|	real	|	Задействованная маржа
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале
***PositionsTotal***
Profit	|	real	|	Прибыль
TotalProfit	|	real	|	Суммарная прибыль
Swap	|	real	|	Свопы
***Positions***
ID	|	number	|	ID позиции
Symbol	|	string	|	Название инструмента
Volume	|	real	|	Открытый объем
Price	|	real	|	Средняя цена открытия
Margin	|	real	|	Маржа под открытый объем
Swap	|	real	|	Накопленный своп
Profit	|	real	|	Прибыль/убыток без учета свопа
TotalProfit	|	real	|	Прибыль/убыток с учетом свопа
ProfitCalcQuote	|	real	|	Котировка, по которой вычислялась прибыль
PrecisionPrice	|	number	|	Количество знаков после запятой при выводе цены
PrecisionVolume	|	number	|	Количество знаков после запятой при выводе объема

**Пример вызова:**
```json
{
    "Filter": {
        "AccountID": "1000201"
    },
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 10
    }
}
```
**Пример ответа:**
```json
{
    "Filter": {
        "AccountID": 1000201
    },
    "OrderBy": {
        "Field": "ID",
        "Direction": "Asc"
    },
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 10,
        "MaxPerPage": 100
    },
    "Wallets": [
        {
            "ID": 11121,
            "Asset": "USD",
            "Balance": 10200,
            "Invested": 781.99,
            "Margin": 83.99,
            "IntervalPnL": -18.01
        }
    ],
    "PositionsTotal": [
        {
            "Profit": -18.39,
            "TotalProfit": -18.39,
            "Swap": 0
        }
    ],
    "Positions": [
        {
            "ID": 13912,
            "Symbol": "EURUSD",
            "Volume": 0.0388,
            "Price": 1.08673,
            "Margin": 83.97,
            "Swap": 0,
            "Profit": -18.39,
            "TotalProfit": -18.39,
            "ProfitCalcQuote": 1.08199,
            "PrecisionPrice": 6,
            "PrecisionVolume": 4
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

### Сделки на счете

#### deals.search

Поиск сделок с фильтрацией по номеру счета.

**URL:** `https://ramm.store/api/client/v2/deals.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	|ID счета

Допустимые поля для секции OrderBy:	
ID, SignalID, CommandID, SOID, TradingIntervalID, DT, Type, Symbol, Volume, Price, Commission, Entry, Profit, Swap, TotalProfit, DealToID, Netting.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets, DealsTotal и Deals:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID	|	number	|	ID кошелька
Asset	|	string	|	Название актива
Balance	|	real	|	Сумма в кошельке
Invested	|	real	|	Инвестированная сумма
Margin	|	real	|	Задействованная маржа
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале
***DealsTotal***
Profit	|	real	|	Прибыль
TotalProfit	|	real	|	Суммарная прибыль
Swap	|	real	|	Свопы
Commission	|	real	|	Комиссия
***Deals***
ID	|	number	|	ID сделки
SignalID	|	number	|	ID сигнала
CommandID	|	number	|	ID команды
SOID	|	number	|	ID StopOut
TradingIntervalID	|	number	|	ID спецификации инструментов
DT	|	datetime	|	Дата/время создания сделки
Type	|	number	|	См.ниже
Symbol	|	string	|	Название инструмента
Volume	|	real	|	Суммарный объем
Price	|	real	|	Средняя цена
Commission	|	real	|	Суммарная комиссия
Entry	|	number	|	0 - In, 1 - Out, 2 - InOut
Profit	|	real	|	Прибыль/убыток по сделке (без учета комиссий и свопов)
Swap	|	real	|	Своп
TotalProfit	|	real	|	Прибыль/убыток по сделке (с учетом комиссий и свопов)
DealToID	|	number	|	ID результирующей сделки
PrecisionPrice	|	number	|	Количество знаков после запятой при выводе цены
PrecisionVolume	|	number	|	Количество знаков после запятой при выводе объема
Netting	|	number	|	признак неттинговой сделки

**Пример вызова:**
```json
{
    "Filter": {
        "AccountID": "1000201"
    },
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 10
    }
}
```
**Пример ответа:**
```json
{
    "Filter": {
        "AccountID": 1000201
    },
    "OrderBy": {
        "Field": "ID",
        "Direction": "Asc"
    },
    "Pagination": {
        "TotalRecords": 3,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 10,
        "MaxPerPage": 100
    },
    "Wallets": [
        {
            "ID": 11121,
            "Asset": "USD",
            "Balance": 10200,
            "Invested": 762.71,
            "Margin": 0,
            "IntervalPnL": -37.29
        }
    ],
    "DealsTotal": [
        {
            "Profit": 462.87,
            "TotalProfit": 462.71,
            "Swap": 0,
            "Commission": 0.16
        }
    ],
    "Deals": [
        {
            "ID": 246529,
            "CommandID": 62925,
            "TradingIntervalID": 7936,
            "DT": "2020-03-24T10:52:45.637",
            "Type": 2,
            "Commission": 0,
            "Profit": 500,
            "Swap": 0,
            "TotalProfit": 500
        },
        {
            "ID": 246535,
            "SignalID": 62945,
            "TradingIntervalID": 7936,
            "DT": "2020-03-24T11:36:34.063",
            "Type": 0,
            "Symbol": "EURUSD",
            "Volume": 0.0388,
            "Price": 1.08673,
            "Commission": 0.08,
            "Entry": 0,
            "Profit": 0,
            "Swap": 0,
            "TotalProfit": -0.08,
            "PrecisionPrice": 6,
            "PrecisionVolume": 4
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)
