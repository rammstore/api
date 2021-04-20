# RAMM.STORE Client3 API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
    * [Методы поиска данных](#Методы-поиска-данных) 
* [Ошибки](#Ошибки)
* [Константы](#Константы)
   * [Значения Account.State](#Значения-AccountState)
   * [Значения Deal.Type](#Значения-DealType)
* [Методы](#Методы)
    * [Аутентификация и авторизация клиента, действия с собственной сессией](#Аутентификация-и-авторизация-клиента-действия-с-собственной-сессией)
        * [session.login](#sessionlogin)
        * [session.logout](#sessionlogout)
        * [password.set](#passwordset)
        * [otp.get](#otpget)
    * [Общая информация о клиенте](#Общая-информация-о-клиенте)
        * [statistic.get](#statisticget)        
    * [Спецификации](#Спецификации)
        * [platform.getSpecification](#platformgetSpecification)
    * [Кошельки и операции с кошельком](#Кошельки-и-операции-с-кошельком)
        * [wallets.get](#walletsget)
        * [walletTransfers.search](#walletTransferssearch)
    * [Информация о стратегиях](#Информация-о-стратегиях)
        * [strategies.get](#strategiesget)
        * [strategies.search](#strategiessearch)
    * [Собственные стратегии клиента](#Собственные-стратегии-клиента)
        * [myStrategies.add](#mystrategiesadd)	
        * [myStrategies.addOffer](#mystrategiesaddOffer)	
        * [myStrategies.setPublicOffer](#mystrategiessetpublicoffer)	
        * [myStrategies.close](#myStrategiesclose)
        * [myStrategies.pause](#myStrategiespause)
        * [myStrategies.resume](#myStrategiesresume)        
        * [myStrategies.getToken](#myStrategiesgetToken)
        * [myStrategies.setToken](#myStrategiessetToken)
        * [myStrategies.setVideo](#myStrategiessetVideo)
        * [myStrategies.checkName](#myStrategiescheckName)
        * [myStrategies.getActiveAccounts](#myStrategiesgetActiveAccounts)
        * [myStrategies.getClosedAccounts](#myStrategiesgetClosedAccounts)
        * [strategyCommands.get](#strategyCommandsget)
    * [Торговые счета клиентов](#Торговые-счета-клиентов)
        * [accounts.add](#accountsadd)
        * [accounts.close](#accountsclose)
        * [accounts.fund](#accountsfund)
        * [accounts.get](#accountsget)
        * [accounts.get](#accountsget)accounts.
        * [accounts.setLeverage](#accountssetleverage)
        * [accounts.searchSpec](#accountssearchSpec)
        * [accountCommands.get](#accountCommandsget)
    * [Открытые позиции](#Открытые-позиции)        
        * [positions.search](#positionssearch)
    * [Сделки на счете](#Сделки-на-счете)    
        * [deals.search](#dealssearch)

## Выполнение запросов
Для обращения к API необходимо сделать POST-запрос по адресу `https://{env}.ramm.store/api/client/v3/{method}`, где:
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
2	|	active, no positions	|
3	|	active, with positions	|
14	|	closed, with positions	|
15	|	closed, no positions	|

### Значения Deal.Type  

Значение|Расшифровка
---------|----------
0	|	buy |
1	|	sell |
2	|	balance |
3	|	credit |
4	|	additional charge |
5	|  correction |
6	|	bonus	|
7  |  fee |
8  |  dividend |
9  |  interest |
10 |  cancelled/rejected buy deal |
11 |  cancelled/rejected sell deal |
12 |  periodical commission |
13 |  zero total order |


## Методы
### Аутентификация и авторизация клиента, действия с собственной сессией
#### session.login

Авторизует пользователя по логину и паролю, создает сессию. Возвращает токен для использования API и информацию по сессии.

Вместо логина и пароля может быть передан OTP-токен, в случае авторизации через сайт брокера.

**URL:** `https://ramm.store/api/client/v3/session.login`

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

**URL:** `https://ramm.store/api/client/v3/session.logout`

**Параметры:** отсутствуют

**Возвращаемые данные:** отсутствуют

[Вернуться к содержанию](#Содержание)

#### password.set

Установка пароля собственной учетной записи клиента. Перед установкой нового пароля проверяется правильность текущего пароля.

**URL:** `https://ramm.store/api/client/v3/password.set`

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
#### otp.get

Получение одноразового пароля для входа.

**URL:** `https://ramm.store/api/client/v3/otp.get`

**Параметры:** отсутствуют

**Возвращаемые данные:**
Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
OTP   | string | Одноразовый пароль |

**Пример ответа:**
```json
{ "OTP": "7ff8b899a6414086ad254158c9365a31" }
```
[Вернуться к содержанию](#Содержание)


### Общая информация о клиенте
#### statistic.get

Получение общей статистики клиента

**URL:** `https://ramm.store/api/client/v3/statistic.get`

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

**URL:** `https://ramm.store/api/client/v3/platform.getSpecification`

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

**URL:** `https://ramm.store/api/client/v3/wallets.get`

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

**URL:** `https://ramm.store/api/client/v3/walletTransfers.search`

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

**URL:** `https://ramm.store/api/client/v3/strategies.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
ID	| number    | ID стратегии |

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
ID	|	number	|	ID стратегии		|
Name	|	string	|	Название стратегии (Varchar(64))		|
Type |	string	|	Тип стратегии ( Simple, Advanced )		|
DTVideo	|	datetime	|	Дата последнего обновления видео		|
Youtube	|	string	|	ссылка на YouTube		|
LeverageMax |	number	|	Максимальное плечо
State	|	string	|	Состояние стратегии (Active, Hidden, Closed)		|
DTClosed |	datetime	|	Дата закрытия. Передается только когда стратегия закрыта.		|
***Tags (вложенный массив)***
{Tag}	|	string	|	Tag	|
***Account (вложенная структура)***
ID	|	number	|	ID счета	|
DT	|	datetime	|	Дата создания
State	|	number	|	[Состояние счета](#Значения-AccountState)	|
Balance	|	real	|	Баланс счета
Equity	|	real	|	Эквити
Asset	|	string	|	Название валюты депозита
Profit	|	real	|	Прибыль по счету |
Leverage |	number	|	Заданное плечо
***Positions (вложенный массив)***
ID	|	number	|	ID позиции
Symbol	|	string	|	Название инструмента
Volume	|	real	|	Открытый объем
Price	|	real	|	Средняя цена открытия
Swap	|	real	|	Накопленный своп
Profit	|	real	|	Прибыль/убыток без учета свопа
TotalProfit	|	real	|	Прибыль/убыток с учетом свопа
ProfitCalcQuote	|	real	|	Котировка, по которой вычислялась прибыль
PrecisionPrice	|	number	|	Количество знаков после запятой при выводе цены
PrecisionVolume	|	number	|	Количество знаков после запятой при выводе объема
***Deals (вложенный массив)***
ID	|	number	|	ID сделки
DT	|	datetime	|	Дата/время создания сделки
Type	|	number	|	См. выше в "Константах"
Symbol	|	string	|	Название инструмента
Volume	|	real	|	Объем сделки
VolumeClosed	|	real	|	Объем закрытия (для сделок Out и InOut)
Price	|	real	|	Средняя цена
Commission	|	real	|	Суммарная комиссия
Entry	|	number	|	0 - In, 1 - Out, 2 - InOut
Profit	|	real	|	Прибыль/убыток по сделке (без учета комиссий и свопов)
Swap	|	real	|	Своп
TotalProfit	|	real	|	Прибыль/убыток по сделке (с учетом комиссий и свопов)
PrecisionPrice	|	number	|	Количество знаков после запятой при выводе цены
PrecisionVolume	|	number	|	Количество знаков после запятой при выводе объема
**Пример вызова:**
```json
{
    "ID": 333
}
```
**Пример ответа:**
```json
{
    "ID": 341,
    "Name": "RTH",
    "Type": "Simple",
    "DTVideo": "2018-09-21T12:10:18",
    "Youtube": "BERFDOJK8",
    "LeverageMax": 10,
    "State": "Active",
    "Tags": [
        "MSFT",
        "EgorPetrov",
        "FastProfitSystem"
    ],
    "Account": {
        "ID": 4545,
        "DT": "2020-01-14T09:58:04.403",
        "State": 2,
        "Balance": 3000,
        "Equity": 3123.45,
        "Asset": "USD",
        "Profit": 123.45,
        "Leverage": 1
    },
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
    ],
    "Deals": [
        {
            "ID": 246529,
            "DT": "2020-03-24T10:52:45.637",
            "Type": 2,
            "Commission": 0,
            "Profit": 500,
            "Swap": 0,
            "TotalProfit": 500
        },
        {
            "ID": 246535,
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

#### strategies.search

Поиск стратегий с фильтрацией и сортировками.

**URL:** `https://ramm.store/api/client/v3/strategies.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
SearchMode |	string	| Обязательное поле. Режим поиска (Rating, MyActiveStrategies, MyClosedStrategies, MyActiveAccounts)	|
Name	|	string	|	Подстрока поиска	|
Type |	string	|	Варианты: Simple, Advanced	| 
IDMax |	number	|	максимальный номер стратегии |

Допустимые поля для секции OrderBy:	
Поле | Тип | Описание 
:--------|----------|----------
ID |	number	|	Порядковый номер стратегии	| 
Name	|	string	|	Название стратегии	|
DTVideo |	datetime	|	дата обновления видео |

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
Type |	string	|	Тип стратегии ( Simple, Advanced )		|
DTVideo	|	datetime	|	Дата последнего обновления видео		|
Youtube	|	string	|	ссылка на YouTube		|
LeverageMax |	number	|	Максимальное плечо
Status	|	number	|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed		|
****Account (вложенная структура)****
ID	|	number	|	ID счета	|
DT	|	datetime	|	Дата создания
State	|	number	|	[Состояние счета](#Значения-AccountState)	|
Balance	|	real	|	Баланс счета
Equity	|	real	|	Эквити
Asset	|	string	|	Название валюты депозита
Profit	|	real	|	Прибыль по счету 
Leverage |	number	|	Заданное плечо 
***Positions (вложенный массив)***
ID	|	number	|	ID позиции
Symbol	|	string	|	Название инструмента
Volume	|	real	|	Открытый объем
Price	|	real	|	Средняя цена открытия
Swap	|	real	|	Накопленный своп
Profit	|	real	|	Прибыль/убыток без учета свопа
TotalProfit	|	real	|	Прибыль/убыток с учетом свопа
ProfitCalcQuote	|	real	|	Котировка, по которой вычислялась прибыль
PrecisionPrice	|	number	|	Количество знаков после запятой при выводе цены
PrecisionVolume	|	number	|	Количество знаков после запятой при выводе объема
***Deals (вложенный массив)***
ID	|	number	|	ID сделки
DT	|	datetime	|	Дата/время создания сделки
Type	|	number	|	См. выше в "Константах"
Symbol	|	string	|	Название инструмента
Volume	|	real	|	Объем сделки
VolumeClosed	|	real	|	Объем закрытия (для сделок Out и InOut)
Price	|	real	|	Средняя цена
Commission	|	real	|	Суммарная комиссия
Entry	|	number	|	0 - In, 1 - Out, 2 - InOut
Profit	|	real	|	Прибыль/убыток по сделке (без учета комиссий и свопов)
Swap	|	real	|	Своп
TotalProfit	|	real	|	Прибыль/убыток по сделке (с учетом комиссий и свопов)
PrecisionPrice	|	number	|	Количество знаков после запятой при выводе цены
PrecisionVolume	|	number	|	Количество знаков после запятой при выводе объема

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
            "Name": "RTH",
            "Type": "Simple",
            "DTVideo": "2018-09-21T12:10:18",
            "Youtube": "BERFDOJK8",
            "LeverageMax": 10,
            "State": "Active",
            "Tags": [
                "MSFT",
                "EgorPetrov",
                "FastProfitSystem"
            ],
            "Account": {
                "ID": 4545,
                "DT": "2020-01-14T09:58:04.403",
                "State": 2,
                "Balance": 3000,
                "Equity": 3123.45,
                "Asset": "USD",
                "Profit": 123.45,
                "Leverage": 1
            },
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
            ],
            "Deals": [
                {
                    "ID": 246529,
                    "DT": "2020-03-24T10:52:45.637",
                    "Type": 2,
                    "Commission": 0,
                    "Profit": 500,
                    "Swap": 0,
                    "TotalProfit": 500
                },
                {
                    "ID": 246535,
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
    ]
}
```

[Вернуться к содержанию](#Содержание)

### Собственные стратегии клиента
#### myStrategies.add

Создание новой стратегии.

**URL:** `https://ramm.store/api/client/v3/myStrategies.add`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
Youtube	| string	| ссылка на YouTube |
Command	|	string	|	Торговая команда	|

**Возвращаемые данные:**

Возвращаемые данные - ID стратегии.

Параметр | Тип | Описание 
---------|----------|----------
StrategyID | number	|	ID стратегии	| 

**Пример вызова:**
```json
{
    "Youtube": "bH41TOREHVg",
    "Command": {"Name":"MSFT","market buy":{"x":1}}
}
```
**Пример ответа:**
```json
{
    "StrategyID": 12345
}
```

[Вернуться к содержанию](#Содержание)

#### myStrategies.addOffer

Создание новой оферты для стратегии.

**URL:** `https://ramm.store/api/client/v3/myStrategies.addOffer`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|
FeeRate	|	real	|	Вознаграждение с прибыли	|
CommissionRate	|	real	|	Размер комиссии в долларах на млн оборота в долларах	|

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
    "CommissionRate": 5
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

**URL:** `https://ramm.store/api/client/v3/myStrategies.setPublicOffer`

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

**URL:** `https://ramm.store/api/client/v3/myStrategies.close`

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

**URL:** `https://ramm.store/api/client/v3/myStrategies.pause`

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

**URL:** `https://ramm.store/api/client/v3/myStrategies.resume`

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

**URL:** `https://ramm.store/api/client/v3/myStrategies.getToken`

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

**URL:** `https://ramm.store/api/client/v3/myStrategies.setToken`

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

#### myStrategies.setVideo

Установка нового видео. Устанавливает текущее время в свойство стратегии DTVideo.

**URL:** `https://ramm.store/api/client/v3/myStrategies.setVideo`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|number	|ID стратегии|
Youtube	|	string	|	ссылка на YouTube		|

**Возвращаемые данные:**

В случае успешной установки возвращается ответ 200 OK.

**Пример вызова:**
```json
{
    "StrategyID": 445,
    "Youtube" : "BERFDOJK8"
}
```
[Вернуться к содержанию](#Содержание)


#### myStrategies.checkName

Проверка имени новой стратегии на уникальность.

**URL:** `https://ramm.store/api/client/v3/mystrategies.checkName`

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

**URL:** `https://ramm.store/api/client/v3/myStrategies.getActiveAccounts`

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
State	|	number	|	[Состояние счета](#Значения-AccountState)
Equity	|	real	|	Эквити инвестиции	|
Leverage |	number	|	Заданное плечо
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
            "State": 4,
            "Equity": 1000,
            "Leverage" : 1,
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

**URL:** `https://ramm.store/api/client/v3/myStrategies.getClosedAccounts`

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
IsMyAccount	|	boolean	|	Признак своего счета

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
            "IsMyAccount": false
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)

#### strategyCommands.get

Получение текущего статуса команды управления стратегиями по ее ID и ID стратегии.

**URL:** `https://ramm.store/api/client/v3/strategyCommands.get`

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

**URL:** `https://ramm.store/api/client/v3/accounts.add`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
StrategyID	|	number	|	ID стратегии
Money	|	real	|	Сумма счета (numeric (28,2))
Leverage |	number	| Плечо


**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
AccountID	|number	|ID счета
CommandBalanceID	|number	|ID команды пополнения счета


**Пример вызова:**
```json
{
    "StrategyID": 4566,
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

**URL:** `https://ramm.store/api/client/v3/accounts.fund`

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

**URL:** `https://ramm.store/api/client/v3/accounts.withdraw`

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

**URL:** `https://ramm.store/api/client/v3/accounts.close`

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

**URL:** `https://ramm.store/api/client/v3/accounts.get`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	| number    | ID счета|

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
***Strategy***
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
LeverageMax |	number	|	Максимальное плечо
Status	|	number	|	Статус стратегии	|
***PublicOffer (вложенная структура)***
ID	|	number	|	ID оферты	|
FeeRate	|	real	|	Размер вознаграждения	|
CommissionRate |	real	|	Размер комиссии в долларах на млн оборота в долларах	|
****TraderInfo (вложенная структура)****
MasterAccount | string | Логин внешнего счета |
ManageType |	number	|	0: управление через Trading API, 1: управление через Manager API |
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
***Account***
ID	|	number	|	ID счета	|
DT	|	datetime	|	Дата создания
DTClosed	|	datetime	|	Дата закрытия
IsMyAccount	|	boolean	|	Признак собственной инвестиции
IsSecurity	|	bool	|	Признак сигнальной инвестиции	|
Type	|	number	|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account	|
State	|	number	|	[Состояние счета](#Значения-AccountState)	|
Balance	|	real	|	Баланс счета
Equity	|	real	|	Эквити
Asset	|	string	|	Название валюты депозита
LeverageMax	|	number	|	Максимальное плечо
MCLevel	|	number	|	Уровень StopOut
Margin	|	real	|	Использованная маржа счета
Factor	|	real	|	Повышающий/понижающий коэффициент копирования
Target	|	real	|	Целевая доходность (numeric (8,3))
Protection	|	real	|	Процент защиты счета (numeric (4,3))
TargetEquity	|	real	|	Целевая доходность в валюте счета
ProtectionEquity	|	real	|	Значение эквити, при котором сработает защита счета
FreeMargin	|	real	|	Свободная маржа
MarginLevel	|	real	|	Уровень маржи
AccountMinBalance	|	real	|	Минимальный баланс инвестиции
AvailableToWithdraw	|	real	|	Средства, доступные к выводу
TotalProfit	|	real	|	Прибыль по счету |
FeeToPay	|	real	|	Невыплаченное вознаграждение	|
FeePaid	|	real	|	Выплаченное вознаграждение	|
ProfitCurrentIntervalGross	|	real	|	Прибыль в текущем торговом интервале	|
TotalCommissionPaid	|	real	|	Суммарная выплаченная комиссия	|
PositionsCount |	number	|	Количество открытых позиций |
***Offer  (вложенная структура)***
ID	|	number	|	ID оферты	|
FeeRate	|	real	|	Размер вознаграждения	|
CommissionRate |	real	|	Размер комиссии в долларах на млн оборота в долларах	|


**Пример вызова:**
```json
{
    "ID": 4545
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
        }
    },
    "Account": {
        "ID": 4545,
        "DT": "2020-01-14T09:58:04.403",
        "IsMyAccount": false,
        "IsSecurity": 0,
        "Type": 2,
        "State": 2,
        "Balance": 2900.82,
        "Equity": 2900.82,
        "Asset": "USD",
        "LeverageMax": 50,
        "MCLevel": 20,
        "Margin": 0,
        "Factor": 1,
        "Target": 1,
        "Protection": 0.5,
        "TargetEquity": 10000,
        "ProtectionEquity": 2500,
        "FreeMargin": 2900.82,
        "FreeLevel": 100,
        "AccountMinBalance": 200,
        "AvailableToWithdraw": 2700.82,
        "TotalProfit": 512.65,
        "FeeToPay": 54.56,
        "FeePaid": 101.58,
        "ProfitCurrentIntervalGross": 152.23,
        "TotalCommissionPaid": 25.34,
        "PositionsCount": 0,
        "Offer": {
            "ID": 123456,
            "FeeRate": 0.25,
            "CommissionRate": 5
        }
    }
}
```
[Вернуться к содержанию](#Содержание)

#### accounts.setLeverage

Устанавливает плечо .

**URL:** `https://ramm.store/api/client/v3/accounts.setLeverage`

**Параметры:**

Поле | Тип | Описание 
:--------|----------|----------
AccountID	|number	| ID счета
Leverage	| real	| Значение плеча для установки

**Возвращаемые данные:**

Параметр | Тип | Описание 
---------|----------|----------
CommandID	|number	|ID команды установки плеча


**Пример вызова:**
```json
{
    "AccountID": 445,
    "Leverage": 10
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

**URL:** `https://ramm.store/api/client/v3/accounts.searchClosed`

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

#### accounts.searchSpec

Получение настроек счетов клиента.

**URL:** `https://ramm.store/api/client/v3/accounts.searchSpec`

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

**URL:** `https://ramm.store/api/client/v3/accountCommands.get`

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

**URL:** `https://ramm.store/api/client/v3/positions.search`

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
***Account (вложенная структура)***
ID	|	number	|	ID счета	|
DT	|	datetime	|	Дата создания
State	|	number	|	[Состояние счета](#Значения-AccountState)	|
Balance	|	real	|	Баланс счета
Equity	|	real	|	Эквити
Asset	|	string	|	Название валюты депозита
Profit	|	real	|	Прибыль по счету |
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
    "Account": {
        "ID": 4545,
        "DT": "2020-01-14T09:58:04.403",
        "State": 2,
        "Balance": 3000,
        "Equity": 3123.45,
        "Asset": "USD",
        "Profit": 123.45
    },    
    "PositionsTotal": {
            "Profit": -18.39,
            "TotalProfit": -18.39,
            "Swap": 0
    },
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

**URL:** `https://ramm.store/api/client/v3/deals.search`

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
***Account (вложенная структура)***
ID	|	number	|	ID счета	|
DT	|	datetime	|	Дата создания
State	|	number	|	[Состояние счета](#Значения-AccountState)	|
Balance	|	real	|	Баланс счета
Equity	|	real	|	Эквити
Asset	|	string	|	Название валюты депозита
Profit	|	real	|	Прибыль по счету |
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
Type	|	number	|	См. выше в "Константах"
Symbol	|	string	|	Название инструмента
Volume	|	real	|	Объем сделки
VolumeClosed	|	real	|	Объем закрытия (для сделок Out и InOut)
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
    "Account": {
        "ID": 4545,
        "DT": "2020-01-14T09:58:04.403",
        "State": 2,
        "Balance": 3000,
        "Equity": 3123.45,
        "Asset": "USD",
        "Profit": 123.45
    },    
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
