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
        "StrategyName": "Super",
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
        {
            "Yield": 100
        },
        {
            "Yield": 105
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
