# RAMM.STORE Client2 API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
    * [Методы поиска данных](#Методы-поиска-данных) 
* [Ошибки](#Ошибки)
* [Стандартные статусы](#Стандартные-статусы)
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
        * [ratings.get](#ratingsget)
    * [Собственные стратегии клиента](#Собственные-стратегии-клиента)
        * [myStrategies.search](#myStrategiessearch)
        * [myStrategies.add](#mystrategiesadd)	
        * [myStrategies.close](#myStrategiesclose)
        * [myStrategies.pause](#myStrategiespause)
        * [myStrategies.resume](#myStrategiesresume)        
        * [myStrategies.getToken](#myStrategiesgetToken)
        * [myStrategies.setToken](#myStrategiessetToken)
        * [myStrategies.checkName](#myStrategiescheckName)
        * [strategyCommands.get](#strategyCommandsget)
        * [myStrategies.getActiveAccounts](#myStrategiesgetActiveAccounts)
        * [myStrategies.getClosedAccounts](#myStrategiesgetClosedAccounts)
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
        * [accounts.search](#accountssearch)
        * [accounts.getCharts](#accountsgetCharts)
        * [accounts.searchSpec](#accountssearchSpec)
        * [accountCommands.get](#accountCommandsget)
    * [Открытые позиции](#Открытые-позиции)        
        * [positions.search](#positionssearch)
    * [Сделки на счете](#Сделки-на-счете)    
        * [deals.search](#dealssearch)

## Выполнение запросов
Для обращения к API необходимо сделать POST-запрос по адресу `https://ramm.store/api/client/v2/{method}`, где:
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
## Стандартные статусы

**Значения поля "State"**

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

**Значения поля "Status"**

Значение|Расшифровка
---------|----------
0	|	New	|
1	|	Active	|
2	|	Paused (MC)	|
3	|	Paused (SLTP)	|
4	|	Paused (Client)	|
5	|	Disabled	|
6	|	Closed	|


## Методы
### Аутентификация и авторизация клиента, действия с собственной сессией
#### session.login

Авторизует пользователя по логину и паролю, создает сессию. Возвращает токен для использования API и информацию по сессии.

Вместо логина и пароля может быть передан OTP-токен, в случае авторизации через сайт брокера.

**URL:** `https://maindc.ramm.store/api/client/v1/session.login`

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
DTLastActivity   | number | Время последней активности  |
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
DT   | number | Дата создания кошелька  |
Asset   | string | Название актива  |
Status   | number | 0-new, 1-active  |
Balance   | real | Баланс кошелька  |
Bonus   | real | Сумма бонусов  |
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
            "Bonus": 0,
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

**URL:** `https://maindc.ramm.store/api/client/v1/session.logout`

**Параметры:** отсутствуют

**Возвращаемые данные:** отсутствуют

[Вернуться к содержанию](#Содержание)

#### password.set

Установка пароля собственной учетной записи клиента. Перед установкой нового пароля проверяется правильность текущего пароля.

**URL:** `https://maindc.ramm.store/api/client/v1/password.set`

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

**URL:** `https://maindc.ramm.store/api/client/v1/statistic.get`

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

**URL:** `https://maindc.ramm.store/api/client/v1/platform.getSpecification`

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

**URL:** `https://maindc.ramm.store/api/client/v1/wallets.get`

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
Bonus   | real | Сумма бонусов  |
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
    "Bonus": 0,
    "Invested": 123.45,
    "Margin": 12.34,
    "IntervalPnL": 23.45,
    "ActiveStrategiesCount": 2
}
```
[Вернуться к содержанию](#Содержание)
#### walletTransfers.search

Поиск переводов.

**URL:** `https://maindc.ramm.store/api/client/v1/walletTransfers.search`

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
Type   | number | 0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners |

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
Bonus   | real | Сумма бонусов  |
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
Type   | number | 0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners |
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
            "Bonus": 90,
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

**URL:** `https://maindc.ramm.store/api/client/v1/strategies.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
ID	| number    | ID стратегии|


**Возвращаемые данные:**

Возвращаемые данные - структуры Strategy и MyAccount:

Параметр | Тип | Описание 
---------|----------|----------
***Strategy***
ID	|	number	|	ID стратегии	|
Name	|	string	|	Имя стратегии	|
Yield	|	real	|	Доходность	|
MonthlyYield	|	real	|	Среднемесячная доходность|
Fee	|	real	|	Размер вознаграждения	|
Commission |	real	|	Размер комиссии	|
Accounts	|	number	|	Количество инвестиций	|
DTCreated	|	number	|	Дата создания	|
DTStat	|	number	|	Дата начала сбора статистики	|
DTClosed	|	number	|	Дата закрытия	|
Equity	|	real	|	Суммарное эквити инвестиций в стратегию	|
IsMyStrategy	|	bool	|	Признак собственной стратегии	|
Status	|	number	|	Статус стратегии	|
***MyAccount***
ID	|	number	|	ID счета	|
ProfitCurrentIntervalGross	|	real	|	Прибыль в текущем торговои интервале	|
TotalProfitNet	|	real	|	Суммарная чистая прибыль	|
TotalProfit	|	real	|	Сумма "чистой" прибыли прошлых периодов и "грязной" прибыли текущего периода |
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
FeePaid	|	real	|	Выплаченное вознаграждение	|
TotalCommissionPaid	|	real	|	Суммарная выплаченная комиссия	|
State	|	number	|	Состояние счета (см. ниже)	|
Equity	|	real	|	Название валюты депозита	|
Factor	|	real	|	Повышающий/понижающий коэффициент копирования	|
AvailableToWithdraw	|	real	|	Доступные к выводу средства	|
AccountMinBalance	|	real	|	Минимальный баланс инвестиции	|
IsSecurity	|	bool	|	Признак сигнальной инвестиции	|
Target	|	real	|	Целевая доходность	|
Protection	|	real	|	Защита капитала	|
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account	|
Status	|	number	|	Статус счета	|

**Пример вызова:**
```json
{
    "ID": 333
}
```
**Пример ответа:**
```json
{
    "Strategy": {
        "ID": 333,
        "Name": "TEST1",
        "Yield": 0.00001,
        "MonthlyYield": 0.05,
        "Fee": 0.25,
        "Accounts": 5,
        "DTCreated": "2018-09-21T11:09:38.243",
        "DTStat": "2017-09-21T11:09:38.243",
        "DTClosed": "2019-09-21T11:09:38.243",
        "Equity": 1000,
        "IsMyStrategy": true,
        "Status":0
    },
    "MyAccount": {
        "ID": 4545,
        "ProfitCurrentIntervalGross": 152.23,
        "TotalProfitNet": 512.65,
        "FeeToPay": 54.56,
        "FeePaid": 101.58,
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
        "Status":0
    }
}
```
[Вернуться к содержанию](#Содержание)
#### charts.get

Получение графика доходности для заданной стратегии.

**URL:** `https://maindc.ramm.store/api/client/v1/charts.get`

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
DT	|	number	|	Дата/время	|
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

**URL:** `https://maindc.ramm.store/api/client/v1/strategyportfolio.get`

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

**URL:** `https://maindc.ramm.store/api/client/v1/strategysymbolstat.get`

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

Поиск стратегий с фильтрацией по подстроке, содержащейся в названии стратегии (Name) и по трем фильтрам.
Каждый фильтр может принимать три значения: true, false и "не задан".

**URL:** `https://maindc.ramm.store/api/client/v1/strategies.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
Name	|	string	|	Подстрока поиска	|
MyActiveAccounts	|	boolean	|	если = true, то ищем только стратегии с активными инвестициями данного клиента	|
MyStrategies	|	boolean	|	если = true, то ищем собственные стратегии клиента	|
ActiveStrategies	|	boolean	|	если = true, то ищем только активные стратегии	|


Допустимые поля для секции OrderBy:	
ID, Name, DTCreated, DTStat, DTClosed, Offer.Commission, Offer.Fee, PartnerShare, Status, Yield, MonthlyYield, Accounts, Symbols, IsMyStrategy, Account.ID, Account.IsSecurity, Account.Type, Account.AccountSpecAssetID, Account.Asset, Account.TradingIntervalCurrentID, Account.DTCreated, Account.Balance, Account.Equity, Account.Margin, Account.MarginLevel, Account.IntervalPnL, Account.Status, Account.Factor, Account.MCReached, Account.Protection, Account.ProtectionEquity, Account.ProtectionReached, Account.Target, Account.TargetEquity, Account.TargetReached, Account.Positions, Account.AccountMinBalance, Account.AvailableToWithdraw, Account.FeePaid, Account.FeeToPay.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets и Strategies:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID	|	number	|	ID кошелька (bigint)		|
Asset	|	string	|	Название актива		|
Balance	|	real	|	Сумма в кошельке		|
Bonus	|	real	|	Сумма бонусов		|
Invested	|	real	|	Инвестированная сумма		|
Margin	|	real	|	Задействованная маржа		|
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале		|
***Strategies***
ID	|	number	|	ID стратегии		|
Name	|	string	|	Название стратегии (Varchar(64))		|
DTCreated	|	number	|	Дата создания стратегии		|
DTStat	|	number	|	Дата сбора статистики		|
DTClosed	|	number	|	Дата закрытия стратегии		|
PartnerShare	|	real	|	Доля партнера		|
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed		|
Yield	|	real	|	Прибыль в %		|
MonthlyYield	|	real	|	Среднемесячная прибыль в %		|
Accounts	|	number	|	Количество счетов		|
Symbols	|	string	|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)		|
IsMyStrategy	|	boolean	|	Признак собственной стратегии		|
****PublicOffer (вложенная структура)****
ID	|	number	|	ID публичной оферты		|
Commission	|	number	|	Размер комиссии в долларах на млн оборота в долларах 		|
Fee	|	real	|	Вознаграждение с прибыли (numeric (3,2))		|
****Account (вложенная структура)****
ID	|	number	|	ID счета		|
IsSecurity	|	boolean	|	Признак счета управляющего		|
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account		|
AccountSpecAssetID	|	number	|	Спецификация счета для заданного актива		|
Asset	|	string	|	Название валюты счета		|
TradingIntervalCurrentID	|	number	|	ID текущего торгового интервала		|
DTCreated	|	number	|	Дата создания		|
Balance	|	real	|	Баланс счета		|
Equity	|	real	|	Эквити		|
Margin	|	real	|	Задействованная маржа		|
MarginLevel	|	real	|	Уровень маржи		|
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале		|
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
Commission	|	number	|	Размер комиссии в долларах на млн оборота в долларах 		|
Fee	|	real	|	Вознаграждение с прибыли (numeric (3,2))		|
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
            "Bonus": 90,
            "Invested": 90.07,
            "Margin": 0,
            "IntervalPnL": -9.93
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
                "Commission": 10,
                "Fee": 0.25
            },
            "Status": 2,
            "Yield": 1.076,
            "MonthlyYield": 0.07,
            "Accounts": 17,
            "Symbols": "EURUSD",
            "IsMyStrategy": 1,
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
                "IntervalPnL": 6.64,
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
                    "Commission": 0,
                    "Fee": 0.0
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
#### ratings.get

Получение рейтинга стратегии и информации о собственных инвестициях в них.

**URL:** `https://maindc.ramm.store/api/client/v1/ratings.get`

**Параметры:**

Может содержать секции [Filter, Pagination](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
RatingType	|	number	|	Тип рейтинга: 0-rating, 1-all, 2-popular	|
StrategyName	|	string	|	Название стратегии	|

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, массивы MyFunds, Strategies и Chart:

Параметр | Тип | Описание 
---------|----------|----------
***MyFunds***
InvestedNet	|	real	|	Общая инвестированная сумма
EquityNet	|	real	|	Суммарное эквити (свободные + инвестированные средства)
***Strategies***
****Strategy (вложенная структура)****
ID	|	number	|	ID стратегии	|
Name	|	string	|	Название стратегии	|
Fee	|	real	|	Вознаграждение с прибыли	|
Commission	|	real	|	Размер комиссии	|
MonthlyYield	|	real	|	Среднемесячная прибыль в %	|
Yield	|	real	|	Прибыль в %	|
AgeByDays	|	number	|	Возраст в днях	|
Symbols	|	string	|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)	|
SignalSourceType	|	number	|	Тип источника сигнала (0 - RAMM token, 1 - MT Manager API)	|
IsMyStrategy	|	boolean	|	Признак собственной стратегии	|
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed	|
Type	|	number	|	Тип стратегии	|
Accounts	|	number	|	Количество инвесторов	|
****Account (вложенная структура)****
ID	|	number	|	ID инвестиции	|
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account	|
Equity	|	real	|	Эквити счета	|
ProfitCurrentIntervalNet	|	real	|	Прибыль/убыток в текущем торговом интервале	|
TotalProfitNet	|	real	|	Суммарная прибыль/убыток|
TotalProfit	|	real	|	Сумма "чистой" прибыли прошлых периодов и "грязной" прибыли текущего периода |
Factor	|	real	|	Повышающий/понижающий коэффициент копирования	|
Target	|	real	|	Целевая доходность инвестиции, в %	|
Protection	|	real	|	Уровень защиты инвестиции, в %	|
ProtectionEquity	|	real	|	Уровень защиты инвестиции, в валюте счета	|
TargetEquity	|	real	|	Целевая доходность инвестиции, в валюте счета	|
State	|	number	|	Код состояния счета (см.ниже)	|
Status	|	number	|	0-new (without money), 1-active (trading), 2-MC, 3-ProtectionTarget, 4-Pause, 5-disabled (cant trade), 6-closed (cant activate)	|
IsSecurity	|	bool	|	Признак инвестиции трейдера	|
Balance	|	real	|	Баланс инвестиции	|
AccountMinBalance	|	real	|	Минимальный баланс инвестиции	|
AvailableToWithdraw	|	real	|	Доступные к выводу средства	|
***Chart***
Yield	|	real	|	Прибыль в %	|


**Пример вызова:**
```json
{
    "Filter": {
        "RatingType": 0,
        "StrategyName": "test0702_1"
    },
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 100
    }
}
```

**Пример ответа:**
```json
{
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 100
    },
    "Strategies": [
        {
            "Strategy": {
                "ID": 1218,
                "Name": "Test0702_1",
                "Fee": 0.25,
                "Commission": 0.000003,
                "MonthlyYield": 0.3859,
                "Yield": 0.5278,
                "AgeByDays": 41,
                "Symbols": "EURUSD, USDJPY",
                "IsMyStrategy": true,
                "Status": 2,
                "SignalSourceType": 0,
                "Accounts": 1,
                "Type": 0
            },
            "Account": {
                "ID": 1000097,
                "Type": 0,
                "Equity": 1528.83,
                "ProfitCurrentIntervalNet": 527.83,
                "Factor": 1,
                "Target": 1,
                "Protection": 0.5,
                "ProtectionEquity": 764.415,
                "TargetEquity": 3057.66,
                "State": 11,
                "Status": 4,
                "IsSecurity": 1,
                "Balance": 1528.83,
                "AccountMinBalance": 200,
                "AvailableToWithdraw": 1328.83
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
    ],
    "MyFunds": [
        {
            "InvestedNet": 75303.25,
            "EquityNet": 201623.65
        }
    ]
}
```

[Вернуться к содержанию](#Содержание)

### Собственные стратегии клиента
[Вернуться к содержанию](#Содержание)

#### myStrategies.search
[Вернуться к содержанию](#Содержание)

Поиск собственных стратегий клиента с фильтрацией по подстроке, содержащейся в названии стратегии (Name), и по признаку активной/закрытой стратегии.

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
Value	|string	|Подстрока поиска|
IsActive	|boolean	|Признак активной стратегии|

Допустимые поля для секции OrderBy:	
ID, Name, DTCreated, DTStat, DTClosed, Offer.Commission, Offer.Fee, PartnerShare, Status, Yield, MonthlyYield, Accounts, Symbols, FeeToPay, FeePaid, CommissionPaid, CommissionToPay, Account.ID, Account.AccountSpecAssetID, Account.Asset, Account.TradingIntervalCurrentID, Account.DTCreated, Account.Balance, Account.Equity, Account.Margin, Account.MarginLevel, Account.IntervalPnL, Account.TotalProfitNet, Account.TotalProfit, Account.Status, Account.Factor, Account.MCReached, Account.Protection, Account.ProtectionEquity, Account.ProtectionReached, Account.Target, Account.TargetEquity, Account.TargetReached, Account.AvailableToWithdraw, Account.AccountMinBalance.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets и Strategies:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID	|	number	|	ID кошелька	|
Asset	|	string	|	Название актива	|
Balance	|	real	|	Сумма в кошельке	|
Bonus	|	real	|	Сумма бонусов	|
Invested	|	real	|	Инвестированная сумма	|
Margin	|	real	|	Задействованная маржа	|
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале	|
***Strategies***
ID	|	number	|	ID стратегии	|
Name	|	string	|	Название стратегии	|
DTCreated	|	number	|	Дата создания стратегии	|
DTStat	|	number	|	Дата сбора статистики	|
DTClosed	|	number	|	Дата закрытия стратегии	|
PartnerShare	|	real	|	Доля партнера	|
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed	|
Yield	|	real	|	Прибыль в %	|
MonthlyYield	|	real	|	Среднемесячная прибыль в %	|
Accounts	|	number	|	Количество счетов	|
Symbols	|	string	|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)	|
MasterAccount | string | Логин внешнего счета
FeePaid	|	real	|	Выплаченное вознаграждение	|
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
CommissionPaid	|	real	|	Выплаченная комиссия	|
CommissionToPay	|	real	|	Невыплаченная комиссия	|
IsMyStrategy	|	bool	|	Признак собственной стратегии	|
****Offer (вложенная структура)****
Commission	|	real	|	Размер комиссии (numeric (6,6))		|
Fee	|	real	|	Вознаграждение с прибыли (numeric (3,2))		|
****Account (вложенная структура)****
ID	|	number	|	ID счета	|
IsSecurity	|	number	|	Признак инвестиции трейдера (0/1)	|
AccountSpecAssetID	|	number	|	Спецификация счета для заданного актива	|
Asset	|	string	|	Название валюты счета	|
TradingIntervalCurrentID	|	number	|	ID текущего торгового интервала	|
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account	|
DTCreated	|	number	|	Дата создания	|
Balance	|	real	|	Баланс счета	|
Equity	|	real	|	Эквити	|
Margin	|	real	|	Задействованная маржа	|
MarginLevel	|	real	|	Уровень маржи	|
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале	|
TotalProfitNet	|	real	|	Суммарная прибыль/убыток |
TotalProfit	|	real	|	Сумма "чистой" прибыли прошлых периодов и "грязной" прибыли текущего периода |
Status	|	number	|	0-new (without money), 1-active (trading), 2-MC, 3-ProtectionTarget, 4-Pause, 5-disabled (cant trade), 6-closed (cant activate)	|
Factor	|	real	|	Повышающий/понижающий коэффициент копирования	|
MCReached	|	number	|	Дата/время срабатывания StopOut	|
Protection	|	real	|	Процент защиты счета	|
ProtectionEquity	|	real	|	Значение эквити, при котором сработает защита счета	|
ProtectionReached	|	number	|	Дата/время срабатывания защиты счета	|
Target	|	real	|	Целевая доходность	|
TargetEquity	|	real	|	Целевая доходность в валюте счета	|
TargetReached	|	number	|	Дата/время достижения целевой доходности	|
AvailableToWithdraw	|	real	|	Доступно к выводу	|
AccountMinBalance	|	real	|	Мин. баланс инвестиции	|


**Пример вызова:**
```json
{
    "Filter": {
        "Name": "TEST",
        "IsActive": true
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
        "IsActive": true
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
            "Bonus": 90,
            "Invested": 90.07,
            "Margin": 0,
            "IntervalPnL": -9.93
        }
    ],
    "Strategies": [
        {
            "ID": 341,
            "Name": "TEST_1",
            "DTCreated": "2018-09-21T11:09:38.23",
            "DTStat": "2017-09-21T11:09:38.23",
            "DTClosed": "2018-09-22T11:09:38.23",
            "Offer": {
                "Commission": 0.00001,
                "Fee": 0.25
            },
            "Status": 2,
            "Yield": 1.076,
            "Accounts": 17,
            "Symbols": "EURUSD",
            "MasterAccount": "12132545",
            "MonthlyYield": 0.5,
            "FeePaid": 5.01,
            "FeeToPay": 1.01,
            "Account": {
                "ID": 1185,
                "AccountSpecAssetID": 5,
                "Asset": "USD",
                "TradingIntervalCurrentID": 164,
                "Type": 0,
                "DTCreated": "2018-09-21T11:09:38.243",
                "Balance": 1000.46,
                "Equity": 1006.64,
                "Margin": 2.27,
                "MarginLevel": 5.23,
                "IntervalPnL": 6.64,
                "Status": 3,
                "Factor": 1,
                "Protection": 0.01,
                "ProtectionEquity": 10,
                "Target": 0.01,
                "TargetEquity": 1010,
                "TargetReached": "2018-12-12T15:34:54.217",
                "AvailableToWithdraw": 100.01,
                "AccountMinBalance": 10.01
            }
        }
    ]
}
```

#### myStrategies.add

Создание клиентом новой стратегии.

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.add`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
Name	|	string	|	Подстрока поиска	|
FeeRate	|	real	|	Вознаграждение с прибыли	|
CommissionRate	|	real	|	Размер комиссии	|
Shared	|	boolean	|	Признак копирования стратегии клиентам других брокеров	|
Protection	|	real	|	Процент защиты счета	|
Target	|	real	|	Целевая доходность	|
Money	|	real	|	Сумма собственной счета трейдера	|

**Возвращаемые данные:**

Возвращаемые данные - структура AccountCommand и массивы Wallets, Strategies:

Параметр | Тип | Описание 
---------|----------|----------
***AccountCommand***
ID	|	number	|	ID команды	|
***Strategy***
ID	|	number	|	ID стратегии	|
Name	|	string	|	Название стратегии	|
DTCreated	|	number	|	Дата создания стратегии	|
DTStat	|	number	|	Дата сбора статистики	|
PartnerShare	|	real	|	Доля партнера	|
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed	|
Yield	|	real	|	Прибыль в %	|
Accounts	|	number	|	Количество счетов	|
Symbols	|	string	|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)	|
****Offer (вложенная структура)****
Commission	|	real	|	Размер комиссии	|
Fee	|	real	|	Вознаграждение с прибыли	|
***Account***
ID	|	number	|	ID счета	|
IsSecurity	|	boolean	|	Признак счета управляющего	|
AccountSpecAssetID	|	number	|	Спецификация счета для заданного актива	|
Asset	|	string	|	Название валюты счета	|
TradingIntervalCurrentID	|	number	|	ID текущего торгового интервала	|
DTCreated	|	number	|	Дата создания	|
Balance	|	real	|	Баланс счета	|
Equity	|	real	|	Эквити	|
Margin	|	real	|	Задействованная маржа	|
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале	|
Status	|	number	|	См. ниже	|
Factor	|	real	|	Повышающий/понижающий коэффициент копирования	|
MCReached	|	number	|	Дата/время срабатывания StopOut	|
Protection	|	real	|	Процент защиты счета	|
ProtectionEquity	|	real	|	Значение эквити, при котором сработает защита счета	|
ProtectionReached	|	number	|	Дата/время срабатывания защиты счета	|
Target	|	real	|	Целевая доходность	|
TargetEquity	|	real	|	Целевая доходность в валюте счета	|
TargetReached	|	number	|	Дата/время достижения целевой доходности	|

**Пример вызова:**
```json
{
    "Name": "TEST_1",
    "FeeRate": 0.2,
    "Shared": true,
    "CommissionRate": 0.0001,
    "Protection": 0.5,
    "Target": 1.5,
    "Money": 1000
}
```
**Пример ответа:**
```json
{
    "Strategies": [
        {
            "Strategy": {
                "ID": 341,
                "Name": "TEST_1",
                "DTCreated": "2018-09-21T11:09:38.23",
                "DTStat": "2017-09-21T11:09:38.23",
                "Offer": {
                    "Commission": 0.0001,
                    "Fee": 0.2
                },
                "Status": 1,
                "Yield": 0,
                "Accounts": 1,
                "Symbols": ""
            },
            "Account": {
                "ID": 1185,
                "IsSecurity": true,
                "AccountSpecAssetID": 5,
                "Asset": "USD",
                "TradingIntervalCurrentID": 164,
                "DTCreated": "2018-09-21T11:09:38.243",
                "Balance": 1000,
                "Equity": 1000,
                "Margin": 0,
                "IntervalPnL": 0,
                "Status": 3,
                "Factor": 1,
                "Protection": 0.5,
                "ProtectionEquity": 500,
                "Target": 1.5,
                "TargetEquity": 1500,
                "TargetReached": "2018-12-12T15:34:54.217"
            }
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### myStrategies.close

Закрывает стратегию.

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.close`

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

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.pause`

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

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.resume`

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

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.getToken`

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

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.setToken`

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

**URL:** `https://maindc.ramm.store/api/client/v1/mystrategies.checkName`

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

#### strategyCommands.get

Получение текущего статуса команды управления стратегиями по ее ID и ID стратегии.

**URL:** `https://maindc.ramm.store/api/client/v1/strategyCommands.get`

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

#### myStrategies.getActiveAccounts

Поиск активных инвестиций в стратегию.

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.getActiveAccounts`

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
DT	|	number	|	Дата создания инвестиции	|
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

**URL:** `https://maindc.ramm.store/api/client/v1/myStrategies.getClosedAccounts`

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
DT	|	number	|	Дата создания инвестиции
DTClosed	|	number	|	Дата закрытия инвестиции
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

### Торговые счета клиентов
[Вернуться к содержанию](#Содержание)

#### accounts.add

Создание счета.

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.add`

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


**Пример вызова для публичной оферты:**
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
**Пример вызова по скрытой ссылке:**
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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.fund`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.withdraw`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.close`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	| number    | ID счета|


**Возвращаемые данные:**

Возвращаемые данные содержат структуру Strategy:

Параметр | Тип | Описание 
---------|----------|----------
ID	|	number	|	ID счета
DT	|	number	|	Дата создания
DTClosed	|	number	|	Дата закрытия
Fee |	real	|	Размер вознаграждения
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account
IsSecurity	|	boolean	|	Признак сигнальной инвестиции
Status	|	number	|	см.ниже
Balance	|	real	|	Баланс счета
Bonus	|	real	|	Бонус
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
    "Bonus": 0,
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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.getStatement`

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
Commission	|	real	|	Размер комиссии
Fee	|	real	|	Размер вознаграждения
IsMyStrategy	|	bool	|	Признак собственной стратегии
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed
****Account (вложенная структура)****
ID	|	number	|	ID счета
IsMyAccount	|	boolean	|	Признак собственной инвестиции
IsSecurity	|	bool	|	Признак сигнальной инвестиции
IDCompany	|	number	|	ID компании
DT	|	number	|	Дата создания
DTClosed	|	number	|	Дата закрытия
ABook	|	real	|	Доля А-Бук
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account
AssetName	|	string	|	Название валюты депозита
LeverageMax	|	number	|	Максимальное плечо
MCLevel	|	number	|	Уровень StopOut
Balance	|	real	|	Баланс счета
Margin	|	real	|	Использованная маржа счета
State	|	number	|	Код состояния счета (см.ниже)
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
                "Commission": 0,
                "Fee": 0.25,
                "IsMyStrategy": true,
                "Status": 1
            },
            "Account": {
                "ID": 1000005,
                "IsMyAccount": false,
                "IsSecurity": 0,
                "IDCompany": 9,
                "DT": "2020-01-14T09:58:04.403",
                "ABook": 1,
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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.pause`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.resume`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.setFactor`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.setProtection`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.setTarget`

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

#### accounts.search

Поиск счетов и соответствующих им стратегий с фильтрацией по подстроке (из имени стратегии).

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
Value	|string	|Подстрока поиска
MyActiveAccounts	|boolean	|флаг поиска собственных счетов. (1 - только собственные, 0 - только чужие, нет параметра - все)

Допустимые поля для секции OrderBy:	
Strategy.ID, Strategy.Name, Strategy.DTCreated, Strategy.DTStat, Strategy.DTClosed, Strategy.Offer.Commission, Strategy.Offer.Fee, Strategy.PartnerShare, Strategy.Status, Strategy.Yield, Strategy.MonthlyYield, Strategy.Accounts, Strategy.Symbols, ID, IsSecurity, Type, AccountSpecAssetID, Asset, TotalProfitNet, TotalProfit, TradingIntervalCurrentID, DTCreated, DTClosed, Balance, Equity, Margin, MarginLevel, IntervalPnL, Status, Factor, MCReached, Protection, ProtectionEquity, ProtectionReached, Target, TargetEquity, TargetReached, AvailableToWithdraw, AccountMinBalance, IsMyStrategy.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets и Accounts:

Параметр | Тип | Описание 
---------|----------|----------
***Wallets***
ID	|	number	|	ID кошелька (bigint)
Asset	|	string	|	Название актива
Balance	|	real	|	Сумма в кошельке
Bonus	|	real	|	Сумма бонусов
Invested	|	real	|	Инвестированная сумма
Margin	|	real	|	Задействованная маржа
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале
***Accounts***
ID	|	number	|	ID счета
IsSecurity	|	boolean	|	Признак счета управляющего
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account
AccountSpecAssetID	|	number	|	Спецификация счета для заданного актива
Asset	|	string	|	Название валюты счета
TradingIntervalCurrentID	|	number	|	ID текущего торгового интервала
DTCreated	|	number	|	Дата создания
DTClosed	|	number	|	Дата закрытия счета
Balance	|	real	|	Баланс счета
Equity	|	real	|	Эквити
Margin	|	real	|	Задействованная маржа
MarginLevel	|	real	|	Уровень маржи
IntervalPnL	|	real	|	Прибыль/убыток в текущем торговом интервале
TotalProfitNet	|	real	|	Суммарная прибыль/убыток
TotalProfit	|	real	|	Сумма "чистой" прибыли прошлых периодов и "грязной" прибыли текущего периода
Status	|	number	|	см. ниже
Factor	|	real	|	Повышающий/понижающий коэффициент копирования
MCReached	|	number	|	Дата/время срабатывания StopOut
Protection	|	real	|	Процент защиты счета (numeric (4,3))
ProtectionEquity	|	real	|	Значение эквити, при котором сработает защита счета
ProtectionReached	|	number	|	Дата/время срабатывания защиты счета
Target	|	real	|	Целевая доходность (numeric (8,3))
TargetEquity	|	real	|	Целевая доходность в валюте счета
TargetReached	|	number	|	Дата/время достижения целевой доходности
AvailableToWithdraw	|	real	|	Доступно для вывода
AccountMinBalance	|	real	|	Минимальный баланс
****Strategy (вложенная структура)****
ID	|	number	|	ID стратегии
Name	|	string	|	Название стратегии (Varchar(64))
DTCreated	|	number	|	Дата создания стратегии
DTStat	|	number	|	Дата сбора статистики
DTClosed	|	number	|	Дата закрытия стратегии
PartnerShare	|	real	|	Доля партнера
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed
Yield	|	real	|	Прибыль в %
MonthlyYield	|	real	|	Среднемесячная прибыль в %
Accounts	|	number	|	Количество счетов
Symbols	|	string	|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)
IsMyStrategy	|	bool	|	Признак собственной стратегии
*****Offer (вложенная структура)*****
Commission	|	real	|	Размер комиссии (numeric (6,6))
Fee	|	real	|	Вознаграждение с прибыли (numeric (3,2))
****Charts (вложенный массив)****
Yield	|	real	|	Значение доходности

**Пример вызова:**
```json
{
    "Filter": {
        "MyActiveAccounts": true
    },
    "OrderBy": {
        "Field": "Strategy.Yield",
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
        "MyActiveAccounts": true
    },
    "OrderBy": {
        "Field": "Strategy.Yield",
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
    "Accounts": [
        {
            "Strategy": {
                "ID": 1252,
                "Name": "TestStr2003_1",
                "DTCreated": "2020-03-20T14:29:13.697",
                "DTStat": "2020-03-20T14:29:13.697",
                "Offer": {
                    "Commission": 0.000002,
                    "Fee": 0.25
                },
                "PartnerShare": 0,
                "Status": 1,
                "Yield": -0.0854,
                "MonthlyYield": -0.0854,
                "Accounts": 2,
                "Symbols": "EURUSD",
                "IsMyStrategy": true
            },
            "ID": 1000196,
            "IsSecurity": true,
            "Type": 0,
            "AccountSpecAssetID": 5,
            "Asset": "USD",
            "TradingIntervalCurrentID": 7931,
            "DTCreated": "2020-03-20T14:29:13.697",
            "Balance": 914.63,
            "Equity": 914.63,
            "Margin": 0,
            "IntervalPnL": -85.37,
            "Status": 1,
            "Factor": 1,
            "Protection": 0.5,
            "ProtectionEquity": 500,
            "Target": 1,
            "TargetEquity": 2000,
            "AvailableToWithdraw": 714.63,
            "AccountMinBalance": 200,
            "Chart": [
                {
                    "Yield": -10.173
                },
                {
                    "Yield": -13.131
                },
                {
                    "Yield": -8.644
                }
            ]
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.getCharts

Получение графика заданного типа для заданного счета.

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.getCharts`

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
DT	|number	|Дата/время
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

**URL:** `https://maindc.ramm.store/api/client/v1/accounts.searchSpec`

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

**URL:** `https://maindc.ramm.store/api/client/v1/accountCommands.get`

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

**URL:** `https://maindc.ramm.store/api/client/v1/positions.search`

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
Bonus	|	real	|	Сумма бонусов
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

**URL:** `https://maindc.ramm.store/api/client/v1/deals.search`

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
Bonus	|	real	|	Сумма бонусов
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
DT	|	number	|	Дата/время создания сделки
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
