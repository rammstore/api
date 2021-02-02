# RAMM.STORE Partner API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
* [Ошибки](#Ошибки)
* [Методы](#Методы)
   * [strategies.search](#strategiesSearch)

## Выполнение запросов
Для обращения к API необходимо сделать `POST`-запрос по адресу `https://ramm.store/api/partner/v{VER}/{method}`, где:
* `{VER}` — версия API (на данный момент — 3);
* `{method}` — метод API.

Headers:

Key | Value |
:--------|----------|
Content-Type   | application/json
Token   | <Токен>

Где <Токен> - партнерский токен, например, `9cdd81e2-72db-4c7d-b862-a868d417bf15`

Ответ вернется в JSON.

## Ошибки
API может возвращать различные ошибки в следующем формате:
```json
{
    "Error": {
        "Code": "invalid_input",
        "Message": "Invalid input in the field 'ID'"
    }
}
```
## Методы
[Вернуться к содержанию](#Содержание)
### Информация о стратегиях
#### strategies.get

Получение информации о стратегии.

**URL:** `https://ramm.store/api/client/v3/strategies.get`

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
CommissionRate |	real	|	Размер комиссии в долларах на млн оборота в долларах	|
***LinkOffer (вложенная структура)***
ID	|	number	|	ID оферты	|
Link	| string    | Ссылка оферты |
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
***Account (вложенная структура)***
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
### strategies.search

Поиск стратегий с фильтрацией и сортировками.

**URL:** `https://ramm.store/api/partner/v3/strategies.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
Name	|	string	|	Подстрока поиска по названию стратегии	|
Type |	string	|	Варианты: Portfolios, Ideas, Bets	| 
DTVideoFrom |	datetime	|	дата видео не раньше указанного времени |
Tag |	string	|	Таг (например, "MSFT")	|

Допустимые поля для секции OrderBy:	
Поле | Тип | Описание 
:--------|----------|----------
ID |	number	|	Порядковый номер стратегии	| 
Name	|	string	|	Название стратегии	|
DTVideo |	datetime	|	дата обновления видео |

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy и массив Strategies:

Параметр | Тип | Описание 
---------|----------|----------
***Strategies***
ID	|	number	|	ID стратегии		|
Name	|	string	|	Название стратегии (Varchar(64))		|
Type |	string	|	Тип стратегии ( Portfolio, Idea, Bet )		|
DTVideo	|	datetime	|	Дата последнего обновления видео		|
Youtube	|	string	|	ссылка на YouTube		|
FactorMax	|	number	|	Максимальное значение повышающего коэффициента		|
***Tags (вложенный массив)***
{Tag}	|	string	|	Tag	|


**Пример вызова:**
```json
{
    "Filter": {
        "Name": "TEST"
    },
    "Pagination": {
        "CurrentPage": 1,
        "PerPage": 5
    },
    "OrderBy": {
        "Field": "DTVideo",
        "Direction": "Desc"
    }
}
```

**Пример ответа:**

```json
{
    "Filter": {
        "Name": "TEST"
    },
    "OrderBy": {
        "Field": "DTVideo",
        "Direction": "Desc"
    },
    "Pagination": {
        "TotalRecords": 1,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 5,
        "MaxPerPage": 100
    },
    "Strategies": [
        {
            "ID": 341,
            "Name": "TEST_1",
            "Type": "Idea",
            "DTVideo": "2018-09-21T12:10:18",
            "Youtube": "BERFDOJK8",
            "FactorMax": 10,
            "Tags": [
                "MSFT",
                "EgorPetrov",
                "FastProfitSystem"
            ]
        }
    ]
}```

[Вернуться к содержанию](#Содержание)


