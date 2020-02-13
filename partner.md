# RAMM.STORE Partner API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
* [Ошибки](#Ошибки)
* [Методы](#Методы)
    * [Информация о стратегиях](#Информация-о-стратегиях)
        * [ratings.get](#ratingsget)
        * [strategies.get](#strategiesget)
        * [strategies.getChart](#strategiesgetChart)
        * [strategies.getPortfolio](#strategiesgetPortfolio)
        * [strategies.getSymbolStat](#strategiesgetSymbolStat)

## Выполнение запросов
Для обращения к API необходимо сделать POST-запрос по адресу `https://api.ramm.store/api/partner/v{VER}/{method}`, где:
* {VER} — версия API (на данный момент — 1);
* {method} — метод API.

Ответ вернётся в JSON.

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

### Информация о стратегиях
#### ratings.get

Получение рейтинга стратегий с краткой информацией по каждой из них.

**URL:** `https://api.ramm.store/api/partner/v1/ratings.get`

**Параметры:**

Может содержать секции Filter, Pagination, OrderBy.

**Filter**	

Допустимые поля для секции Filter:	

Поле | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
RatingType   | number | Тип рейтинга: 0-rating, 1-all, 2-popular  | 0
StrategyName   | string | Название стратегии или его часть |

**Pagination**	

Задание текущей страницы и количества записей на страницу.

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
CurrentPage   | number | Номер текущей страницы | 1
PerPage   | number | Количество записей на одной странице | 100


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
    }
}
```

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
        "RatingType": 0,
        "StrategyName": "Super",
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
        "RatingType": 0,
        "StrategyName": "Super",
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

#### strategies.get
[Вернуться к содержанию](#Содержание)
#### strategies.getChart
[Вернуться к содержанию](#Содержание)
#### strategies.getPortfolio
[Вернуться к содержанию](#Содержание)
#### strategies.getSymbolStat
[Вернуться к содержанию](#Содержание)
