# RAMM.STORE Partner3 API Documentation
### Содержание
* [Выполнение запросов](#Выполнение-запросов)
* [Получение данных](#Получение-данных)
* [Ошибки](#Ошибки)
* [Методы](#Методы)
   * [strategies.get](#strategiesGet)
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

**URL:** `https://ramm.store/api/partner/v3/strategies.get`

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
State	|	string	|	Состояние стратегии (Active, Hidden, Closed)		|
DTClosed |	datetime	|	Дата закрытия. Передается только когда стратегия закрыта.		|
***Tags (вложенный массив)***
{Tag}	|	string	|	Tag	|


**Пример вызова:**
```json
{
    "ID": 341
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
    "State": "Active",
    "Tags": [
        "MSFT",
        "EgorPetrov",
        "FastProfitSystem"
    ]
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
Type |	string	|	Варианты: Simple, Advanced	| 
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
Type |	string	|	Тип стратегии ( Simple, Advanced )		|
DTVideo	|	datetime	|	Дата последнего обновления видео		|
Youtube	|	string	|	ссылка на YouTube		|
***Tags (вложенный массив)***
{Tag}	|	string	|	Tag	|


**Пример вызова:**
```json
{
    "Filter": {
        "Name": "RTH"
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
        "Name": "RTH"
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
            "Name": "RTH",
            "Type": "Simple",
            "DTVideo": "2018-09-21T12:10:18",
            "Youtube": "BERFDOJK8",
            "Tags": [
                "MSFT",
                "EgorPetrov",
                "FastProfitSystem"
            ]
        }
    ]
}
```
[Вернуться к содержанию](#Содержание)


