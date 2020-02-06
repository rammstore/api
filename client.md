# RAMM.STORE Client API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
* [Ошибки](#Ошибки)
* [Методы](#Методы)
    * [Аутентификация и авторизация клиента, действия с собственной сессией](#Аутентификация-и-авторизация-клиента-действия-с-собственной-сессией)
        * [session.login](#sessionlogin)
        * [session.logout](#sessionlogout)
        * [password.set](#passwordset)
    * [Кошельки и операции с кошельком](#Кошельки-и-операции-с-кошельком)
        * [wallets.get](#walletsget)
        * [walletTransfers.search](#walletTransferssearch)


## Выполнение запросов
Для обращения к API необходимо сделать POST-запрос по адресу `https://maindc.ramm.store/api/client/v{VER}/{method}`, где:
* {VER} — версия API (на данный момент — 1);
* {method} — метод API.

Ответ вернётся в JSON.

## Получение данных
Методы получения данных могут быть двух типов:
* По уникальному идентификатору: <название сущности>.get
* Поиск по одному или нескольким полям: <название сущности>.search

Filter	
StrategyID	number	ID стратегии
AccountID	number	ID счета
DealID	number	ID сделки
DTFrom	number	Начальная дата
DTTo	number	Конечная дата
Type	number	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners

Pagination	
CurrentPage	number	Номер текущей страницы
PerPage	number	Количество записей на одной странице

OrderBy	
Field	string	Сортировка по параметру, варианты: ID, StrategyID, AccountID, DealID, DT, AccrualDate, Amount, Type, Comment
Direction	string	Направление сортировки, варианты: Asc, Desc

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
## Методы
### Аутентификация и авторизация клиента, действия с собственной сессией
#### session.login

Авторизует пользователя по логину и паролю, создает сессию. Возвращает токен для использования API и информацию по сессии.

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
Login   | string | Логин  |
FirstName   | string | Имя клиента  |
LastName   | string | Фамилия клиента  |
Language   | string | Язык интерфейса  |
PushToken   | string | Токен клиента для Push-нотификации  |
***Company*** |
Name   | string | Название компании  |
Demo   | boolean | Признак демо-компании  |
Contacts   | string | Контакты компании  |
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
        "Name": "FXTrade",
        "Demo": false,
        "Contacts": {
            "CompanyName": "TradeForex Ltd",
            "CompanyInfo": "TradeForex Ltd is a broker"
        }
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
            "IntervalPnL": 0
        }
    ]
}
```
#### session.logout

Удаляет сессию.

**URL:** `https://maindc.ramm.store/api/client/v1/session.logout`

**Параметры:** отсутствуют

**Возвращаемые данные:** отсутствуют

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
#### walletTransfers.search

Поиск переводов.

**URL:** `https://maindc.ramm.store/api/client/v1/walletTransfers.search`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
ID   | number | ID кошелька |

Тело запроса - строка JSON, содержит 
структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):



Filter	
StrategyID	number	ID стратегии
AccountID	number	ID счета
DealID	number	ID сделки
DTFrom	number	Начальная дата
DTTo	number	Конечная дата
Type	number	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners

Pagination	
CurrentPage	number	Номер текущей страницы
PerPage	number	Количество записей на одной странице

OrderBy	
Field	string	Сортировка по параметру, варианты: ID, StrategyID, AccountID, DealID, DT, AccrualDate, Amount, Type, Comment
Direction	string	Направление сортировки, варианты: Asc, Desc

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




Возвращаемые данные - структуры Pagination, Filter, OrderBy, массивы Wallets и WalletTransfers:


Filter	StrategyID	number	ID стратегии

AccountID	number	ID счета

DealID	number	ID сделки

DTFrom	number	Начальная дата

DTTo	number	Конечная дата

Type	number	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners
OrderBy	Field	string	Сортировка по параметру, варианты: ID, StrategyID, AccountID, DealID, DT, AccrualDate, Amount, Type, Comment

Direction	string	Направление сортировки, варианты: Asc, Desc
Pagination	TotalRecords	number	Общее количество записей

TotalPages	number	Общее количество страниц

CurrentPage	number	Номер текущей страницы

PerPage	number	Количество записей на одной странице

MaxPerPage	number	Максимальное количество записей на одной странице
Wallets	ID	number	ID кошелька

Asset	string	Название актива

Balance	real	Сумма в кошельке

Bonus	real	Сумма бонусов

Invested	real	Инвестированная сумма

Margin	real	Задействованная маржа

IntervalPnL	real	Прибыль/убыток в текущем торговом интервале
WalletTransfers	ID	number	ID перевода

StrategyID	number	ID стратегии

AccountID	number	ID счета

DealID	number	ID сделки

DT	number	Дата перевода

AccrualDate	number	Дата зачисления

Amount	real	Сумма перевода

Type	number	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners

Comment	string	Комментарий для клиента


Пример вызова:


{
"Filter":
{
"StrategyID":3457,
"DealID":111
},
"Pagination":
{
"CurrentPage": 1,
"PerPage": 5
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
}
}

Пример ответа:

{
"Filter":
{
"StrategyID":3457,
"DealID":111
},
"Pagination":
{
"TotalRecords": 1,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 5,
"MaxPerPage": 100
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Wallets":
[
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
"WalletTransfers":
[
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

