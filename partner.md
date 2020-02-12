# RAMM.STORE Partner API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
    * [Методы поиска данных](#Методы-поиска-данных) 
* [Ошибки](#Ошибки)
* [Методы](#Методы)
    * [Информация о стратегиях](#Информация-о-стратегиях)
        * [strategies.get](#strategiesget)
        * [strategies.getChart](#strategiesgetChart)
        * [strategies.getPortfolio](#strategiesgetPortfolio)
        * [strategies.getSymbolStat](#strategiesgetSymbolStat)
        * [strategies.search](#strategiessearch)
        * [ratings.get](#ratingsget)

## Выполнение запросов
Для обращения к API необходимо сделать POST-запрос по адресу `https://api.ramm.store/api/partner/v{VER}/{method}`, где:
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
        "Message": "Invalid input in the field 'ID'"
    }
}
```
## Методы

### Информация о стратегиях
#### strategies.get
[Вернуться к содержанию](#Содержание)
#### strategies.getChart
[Вернуться к содержанию](#Содержание)
#### strategies.getPortfolio
[Вернуться к содержанию](#Содержание)
#### strategies.getSymbolStat
[Вернуться к содержанию](#Содержание)
#### strategies.search
[Вернуться к содержанию](#Содержание)
#### ratings.get
[Вернуться к содержанию](#Содержание)

