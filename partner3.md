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

### strategies.search

Поиск стратегий с фильтрацией и сортировками.

**URL:** `https://ramm.store/api/partner/v3/strategies.search`

**Параметры:**

Может содержать секции [Filter, Pagination, OrderBy](#Методы-поиска-данных).

Допустимые поля для секции Filter:	

Поле | Тип | Описание 
:--------|----------|----------
Name	|	string	|	Подстрока поиска	|
Type |	string	|	Варианты: Portfolios, Ideas, Bets	| 
DTVideoFrom |	datetime	|	дата видео не раньше указанного времени |
Tag |	string	|	Таг (например, "MSFT")	|

Допустимые поля для секции OrderBy:	
ID, Name, DTVideo.

**Возвращаемые данные:**

Возвращаемые данные - структуры Pagination, Filter, OrderBy и массив Strategies:

Параметр | Тип | Описание 
---------|----------|----------
***Strategies***
ID	|	number	|	ID стратегии		|
Name	|	string	|	Название стратегии (Varchar(64))		|
Type |	string	|	Тип стратегии ( Portfolio, Idea, Bet )		|
DTCreated	|	datetime	|	Дата создания стратегии		|
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
            "DTCreated": "2018-09-21T11:09:38.23",
            "DTVideo": "2018-09-21T12:10:18.33",
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


