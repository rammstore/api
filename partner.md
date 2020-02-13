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

Может содержать секции Filter, Pagination.

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

Возвращаемые данные по методам поиска всегда содержат полную информацию об используемом фильтре и пагинации.

Пример:
```json
{
    "Filter": {
        "StrategyName": "Super"
    },
    "Pagination": {
        "TotalRecords": 15,
        "TotalPages": 1,
        "CurrentPage": 1,
        "PerPage": 100
    }
}
```

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, массивы Strategies и Chart:

Параметр | Тип | Описание 
---------|----------|----------
***Strategies***
ID	| number |	ID стратегии	
Name |	string	| Название стратегии
Fee |	real	| Вознаграждение с прибыли	
Commission	| real	| Размер комиссии	
MonthlyYield |	real	| Среднемесячная прибыль в %	
Yield	| real	| Прибыль в %	
AgeByDays	| number	| Возраст в днях	
Symbols	| string	| Строка с перечислением самых используемых торговых инструментов (не более 3-х)	
SignalSourceType	| number	| Тип источника сигнала (0 - RAMM token, 1 - MT Manager API)	
Status	| number	| 0-not activated, 1-active, 2-paused, 3-disabled, 4-closed	
Type	| number	| Тип стратегии	
Accounts	| number	| Количество инвесторов
***Chart***
Yield   | real | Значение доходности  |

**Пример вызова:**
```json
{
    "Filter": {
        "RatingType": 0,
        "StrategyName": "Super"
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
        "PerPage": 5
    },
    "Strategies": [
        {
            "Strategy": {
                "ID": 341,
                "Name": "SuperStrategy",
                "Fee": 0.25,
                "Commission": 0.00001,
                "MonthlyYield": 7,
                "Yield": 100,
                "AgeByDays": 365,
                "Symbols": "EURUSD",
                "SignalSourceType": 0,
                "Status": 0,
                "Type": 4,
                "Accounts": 10
            }
        }
    ],
    "Chart": [
        0.6735,
        0.6893,
        0.5596,
        0.5577,
        0.4956,
        0.1972,
        0.2336,
        1.0972,
        1.0927,
        0.8246,
        0.6976,
        0.9818,
        0.7313,
        0.5281,
        0.4259,
        0.2975
    ]
}
```

[Вернуться к содержанию](#Содержание)
#### strategies.get

Получение информации о стратегии

**URL:** `https://api.ramm.store/api/partner/v1/strategies.get`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
ID	| number	| ID стратегии

**Возвращаемые данные:**

Возвращаемые данные - структура Strategy:

Параметр | Тип | Описание 
---------|----------|----------
***Strategy***
ID|number|ID стратегии
Name|string|Имя стратегии
Yield|real|Размер комиссии
MonthlyYield|real|Размер вознаграждения
Fee|real|Размер вознаграждения
Commission|real|Размер комиссии
Accounts|number|Количество инвестиций
DTCreated|number|Дата создания
DTClosed|number|Дата закрытия
Equity|real|Суммарное эквити инвестиций в стратегию
Status|number|0-not activated, 1-active, 2-paused, 3-disabled, 4-closed

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
        "Commission": 0.00001,
        "DTCreated": "2018-09-21T11:09:38.243",
        "DTClosed": "2019-09-21T11:09:38.243",
        "Equity": 1000,
        "Status": 1
    }
}
```

[Вернуться к содержанию](#Содержание)
#### strategies.getChart

Получение графика заданного типа для заданной стратегии

**URL:** `https://api.ramm.store/api/partner/v1/strategies.getChart`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
StrategyID|number|ID стратегии
MaxPoints|number|Макс.количество точек графика
chartType|string|Тип графика (yield, yield-leverage)
chartSize|string|	Размер графика (full, large, medium, small)

**Возвращаемые данные:**

Возвращаемые данные - массив Chart:

Параметр | Тип | Описание 
---------|----------|----------
***Chart***
DT|number|Дата/время
Yield|real|Значение доходности

**Пример вызова:**
```json
{
    "StrategyID": 445,
    "chartType": "yield",
    "chartSize": "full",
    "MaxPoints": 10
}
```
**Пример ответа:**
```json
{
    "Chart": [
        {
            "DT": "2018-12-13T00:00:00",
            "Yield": 0.5
        },
        {
            "DT": "2018-12-14T00:00:00",
            "Yield": 1
        }
    ]
}
```

[Вернуться к содержанию](#Содержание)
#### strategies.getPortfolio

Получение информации о текущем портфеле позиций для заданной стратегии.
Возможность использования данной функции зависит от настроек компании.

**URL:** `https://api.ramm.store/api/partner/v1/strategies.getPortfolio`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
StrategyID|number|ID стратегии

**Возвращаемые данные:**

Возвращаемые данные - массив StrategyPortfolio:

Параметр | Тип | Описание 
---------|----------|----------
***StrategyPortfolio***
Symbol|string|Название символа
Share|real|Доля символа

**Пример вызова:**
```json
{
    "StrategyID": 1111
}
```
**Пример ответа:**
```json
{
    "StrategyPortfolio": [
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
#### strategies.getSymbolStat

Отдает информацию по статистике использования различных торговых инструментов стратегией.

**URL:** `https://api.ramm.store/api/partner/v1/strategies.getSymbolStat`

**Параметры:**

Параметр | Тип | Описание | По умолчанию
:--------|----------|----------|:--------------:
StrategyID|number|ID стратегии

**Возвращаемые данные:**

Возвращаемые данные - массив StrategySymbolStat:

Параметр | Тип | Описание 
---------|----------|----------
***StrategySymbolStat***
Symbol|string|Название символа
Share|real|Доля символа

**Пример вызова:**
```json
{
    "StrategyID": 1111
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
