# RAMM.STORE Client API Documentation
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
    * [Спецификации](#Спецификации)
        * [platform.getSpecification](#platformgetspecification)
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
        * [myStrategies.add](#myStrategiesadd)
        * [myStrategies.close](#myStrategiesclose)
        * [myStrategies.pause](#myStrategiespause)
        * [myStrategies.resume](#myStrategiesresume)
        * [myStrategies.search](#myStrategiessearch)
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
Для обращения к API необходимо сделать POST-запрос по адресу `https://maindc.ramm.store/api/client/v{VER}/{method}`, где:
* {VER} — версия API (на данный момент — 1);
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
### Спецификации
#### platform.getSpecification

Получение спецификации платформы

**URL:** `https://maindc.ramm.store/api/client/v1/platform.getSpecification`

**Параметры:** отсутствуют

**Возвращаемые данные:**

Структура	|	Поле	|	Тип	|	Описание	|
------------|--------|--------|-----------|

Account	|	AvailableCurrency	|	string	|	Доступные валюты счета	|
	      |	MinBalance	|	real	|	Минимальный баланс инвестиции инвестора	|
Strategy	|	MinAmountToCreate	|	real	|	Минимальный баланс инвестиции трейдера	|


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
    "IntervalPnL": 23.45
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
AccountID   | number | ID сделки |
DTFrom   | datetime | Начальная дата  |
DTTo   | datetime | Конечная дата  |
Type   | number | 0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners |

Допустимые поля для секции OrderBy:	
ID, StrategyID, AccountID, DealID, DT, AccrualDate, Amount, Type, Comment.

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
            "Type": 5
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
Yield	|	real	|	Размер комиссии	|
MonthlyYield	|	real	|	Размер вознаграждения	|
Fee	|	real	|	Признак собственной стратегии	|
Accounts	|	number	|	Количество инвестиций	|
DTCreated	|	number	|	Дата создания	|
DTClosed	|	number	|	Дата закрытия	|
Equity	|	real	|	Суммарное эквити инвестиций в стратегию	|
IsMyStrategy	|	bool	|	Признак собственной стратегии	|
***MyAccount***
ID	|	number	|	ID счета	|
ProfitCurrentIntervalGross	|	real	|	Прибыль в текущем торговои интервале	|
TotalProfitNet	|	real	|	Суммарная чистая прибыль	|
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
        "DTClosed": "2019-09-21T11:09:38.243",
        "Equity": 1000,
        "IsMyStrategy": true
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
        "Type": 2
    }
}
```
[Вернуться к содержанию](#Содержание)
#### charts.get

Получение графика заданного типа для заданной стратегии.

**URL:** `https://maindc.ramm.store/api/client/v1/charts.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|	number	|	ID стратегии	|
MaxPoints	|	number	|	Макс.количество точек графика	|
chartType	|	string	|	Тип графика (yield, yield-leverage)	|
chartSize	|	string	|	Размер графика (full, large, medium, small)	|



**Возвращаемые данные:**

Возвращаемые данные - массив Chart:

Параметр | Тип | Описание 
---------|----------|----------
***Chart***
DT	|	number	|	Дата/время	|
Yield	|	real	|	Значение доходности	|
Leverage	|	real	|	Плечо	|


**Пример вызова:**
```json
{
    "StrategyID": 445,
    "chartType": "yield-leverage",
    "MaxPoints": 10
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
[Вернуться к содержанию](#Содержание)
#### ratings.get
[Вернуться к содержанию](#Содержание)

### Собственные стратегии клиента
[Вернуться к содержанию](#Содержание)

#### myStrategies.search
[Вернуться к содержанию](#Содержание)

#### myStrategies.add
[Вернуться к содержанию](#Содержание)

#### myStrategies.close
[Вернуться к содержанию](#Содержание)

#### myStrategies.pause
[Вернуться к содержанию](#Содержание)

#### myStrategies.resume
[Вернуться к содержанию](#Содержание)

#### myStrategies.getToken
[Вернуться к содержанию](#Содержание)

#### myStrategies.setToken
[Вернуться к содержанию](#Содержание)

#### myStrategies.checkName
[Вернуться к содержанию](#Содержание)

#### strategyCommands.get
[Вернуться к содержанию](#Содержание)

#### myStrategies.getActiveAccounts
[Вернуться к содержанию](#Содержание)

#### myStrategies.getClosedAccounts
[Вернуться к содержанию](#Содержание)

### Торговые счета клиентов
[Вернуться к содержанию](#Содержание)

#### accounts.add
[Вернуться к содержанию](#Содержание)

#### accounts.fund
[Вернуться к содержанию](#Содержание)

#### accounts.withdraw
[Вернуться к содержанию](#Содержание)

#### accounts.close
[Вернуться к содержанию](#Содержание)

#### accounts.get
[Вернуться к содержанию](#Содержание)

#### accounts.getStatement
[Вернуться к содержанию](#Содержание)

#### accounts.pause
[Вернуться к содержанию](#Содержание)

#### accounts.resume
[Вернуться к содержанию](#Содержание)

#### accounts.setFactor
[Вернуться к содержанию](#Содержание)

#### accounts.setProtection
[Вернуться к содержанию](#Содержание)

#### accounts.setTarget
[Вернуться к содержанию](#Содержание)

#### accounts.search
[Вернуться к содержанию](#Содержание)

#### accounts.getCharts
[Вернуться к содержанию](#Содержание)

#### accounts.searchSpec
[Вернуться к содержанию](#Содержание)

#### accountCommands.get
[Вернуться к содержанию](#Содержание)

### Открытые позиции

#### positions.search
[Вернуться к содержанию](#Содержание)

### Сделки на счете

#### deals.search
[Вернуться к содержанию](#Содержание)
