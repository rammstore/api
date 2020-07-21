# RAMM.STORE Trading API Documentation
Trading API предназначен для передачи торговых приказов в систему RAMM.

Для упрощения настройки советников, использующих торговый API (требуется руками задавать исключения в настройках МТ), для реализации сигнального способа открытия позиций, нужно реализовать всего один метод, покрывающий весь функционал передачи сигналов и получения всей нужной информации.

Например, https://ramm.store/api/trading/v1/signals.send



**Заголовок запроса**

Заголовок запроса имеет вид:

Content-Type: application/json

Token: 099b40eb-cca6-4316-8d35-cc333899f9a2

AppToken: 7f33625b-0e13-4d87-abf4-42eef85c6d99,

где Token относится к конкретной стратегии RAMM, а AppToken идентифицирует приложение, из которого отправлен сигнал.


**Команды**


Все команды имеют свой тип.

Type:
Код | Значение
---------|----------
0|	heartbeat
1|	signal
2|	sync

Ответ содержится в коде http.

Коды ответов http:
Код | Значение | Комментарий
---------|----------|----------
200|	OK|	Запрос выполнен успешно.
400|	Bad Request|	Запрос содержит синтаксическую ошибку.
401|	Unauthorized|	Токен стратегии и/или токен приложения не указаны или устарели.
404|	Not Found|	Инструмент не найден.

Дополнительная информация возвращается в JSON.


**heartbeat**
Данный сигнал сообщает серверу об активности приложения, передающего торговые приказы.

Рекомендуется отправлять регулярно, раз в 1 минуту.

Пример сигнала:

{"Type":0, "GetInfo": true}


**Response**

В случае запроса с признаком GetInfo = false возвращается сокращенный набор параметров:


{
"App":
{
"CurrentVersion": "1.0.0.0",
"NewestVersion": "1.0.0.1",
"UpdateImportance": 0
}
}

В случае запроса с признаком GetInfo = true возвращается полный набор параметров:

{
"App":
{
"CurrentVersion": "1.0.0.0",
"NewestVersion": "1.0.0.1",
"UpdateImportance": 0
},
"Strategy":
{
"Yield": 123.45,
"Accounts": 123,
"Status": 0,
"NeedSync": false
},
"Account":
{
"Equity": 12333.45,
"Margin": 454.56,
"Factor": 1.00,
"ProfitBase": 10000.00,
"MCReached": "2018-12-11 08:22",
"Protection":0.20,
"ProtectionEquity":2133.23,
"ProtectionReached":"2018-12-11 08:22",
"Target":1.0,
"TargetEquity":20000.00,
"TargetReached":"2018-12-11 08:22",
"Asset":"USD"
}
}

Status:
Код | Значение
---------|----------
0|	not activated
1|	active
2|	paused
3|	disabled
4|	closed


**signal**
Отправлять сразу после исполнения ордеров на торговом счете. В случае, если по какой-либо причине накопилось несколько сигналов, – допускается отправить их все разом в одной коллекции.

{
"Type":1,
"Symbol": "MSFT",
"Lot": 3.2,
"Equity": 10025.00,
"Ticket": "1234568",
"Comment": "EA2"
}

**Response**

В случае успешного выполнения тело ответа сдержит ID сигнала. В случае неуспеха см. раздел "Ошибки". 


**sync**
Синхронизация. Отправляется при инициализации или при получении ответа от сервера о необходимости синхронизации.

{

Type:2,

Signals: 

[{"Symbol": "EURUSD", "Lot": 3.2, "Equity": 10025.00, "Ticket": "1234567", "Comment": "EA1"}, 

{"Symbol": "MSFT", "Lot": 3.2, "Equity": 10025.00, Ticket: "1234568", "Comment": "EA2"}]

}

**Response**

В случае успешного выполнения тело ответа пустое. В случае неуспеха см. раздел "Ошибки". 

Ответ на любой из запросов: 

{

Yield

Accounts

Status

Symbols

}



**Ошибки и возможные коды ответа**
Базовые ошибки возвращаются с помощью кодов HTTP.
При необходимости, более подробный код ошибки возвращается в объекте error (в виде строки JSON).

Пример возвращаемого значения:

400

Bad Request

{
error:
{
"code":"invalid_input",
"message":"invalid input in field 'ID'"
}
}



**Внутренние коды ошибок**

Код | Расшифровка
---------|----------
incorrect_method|	The method you are trying to invoke doesn't exist
access_denied|	No rights to use the invoked method
invalid_input|	Invalid input in the field <field name>
bad_credentials|	Login/Password incorrect or not found
login_blocked|	The login you are using was blocked
  
  
**Схема валидации входящего сигнала**

{
"type": "object",
"title": "Trading API Sent Signal Schema",
"required": [
"Type"
],
"properties": {
"Type": {
"$id": "#/properties/Type",
"type": "integer",
"title": "The Type Schema",
"default": 0,
"minimum": 0
},
"Signals": {
"$id": "#/properties/Signals",
"type": "array",
"title": "The Signals Schema",
"items": {
"$id": "#/properties/Signals/items",
"type": "object",
"title": "The Items Schema",
"required": [
"Symbol",
"leverage"
],
"properties": {
"Symbol": {
"$id": "#/properties/Signals/items/properties/Symbol",
"type": "string",
"title": "The Symbol Schema",
"default": ""
},
"leverage": {
"$id": "#/properties/Signals/items/properties/leverage",
"type": "number",
"title": "The Leverage Schema",
"default": 0.0
},
"Ticket": {
"$id": "#/properties/Signals/items/properties/Ticket",
"type": "string",
"title": "The Ticket Schema",
"default": ""
},
"Comment": {
"$id": "#/properties/Signals/items/properties/Comment",
"type": "string",
"title": "The Comment Schema",
"default": ""
}
}
}
}
}
}
