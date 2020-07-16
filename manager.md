# RAMM.STORE Manager API Documentation
### Содержание
* [Введение](#Введение)
    * [Регистрация менеджера](#Регистрация-менеджера)
* [Описание методов API](#Описание-методов-API)
    * [Аутентификация, действия с собственной сессией](#Аутентификация-действия-с-собственной-сессией)
        * [session.login](#sessionlogin)
        * [session.get](#sessionget)
        * [session.getMethods](#sessiongetMethods)
        * [session.heartbeat](#sessionheartbeat)
        * [session.getMySessions](#sessiongetMySessions)
        * [session.logout](#sessionlogout)
    * [Управление сессиями других менеджеров](#Управление-сессиями-других-менеджеров)
        * [sessions.search](#sessionssearch)
        * [sessions.logout](#sessionslogout)
    * [Информация о залогиненном менеджере, получение и изменение собственного профиля](#Информация-о-залогиненном-менеджере-получение-и-изменение-собственного-профиля)
        * [profile.get](#profileget)
        * [profile.set](#profileset)
    * [Действия с настройками пользователей](#Действия-с-настройками-пользователей)
        * [settings.set](#settingsset)
        * [settings.get](#settingsget)
    * [Действия с паролями](#Действия-с-паролями)
        * [password.set](#passwordset)
        * [clients.setPassword](#clientssetPassword)
        * [clients.getOTP](#clientsgetOTP)
    * [Операции с менеджерами, управляющими сервисом RAMM](#Операции-с-менеджерами-управляющими-сервисом-RAMM)
        * [managers.add](#managersadd)
        * [managers.search](#managerssearch)
        * [managers.get](#managersget)
        * [managers.set](#managersset)
        * [managers.getMethods](#managersgetMethods)
        * [managers.setMethods](#managerssetMethods)
    * [Операции с зарегистрированными в RAMM компаниями](#Операции-с-зарегистрированными-в-RAMM-компаниями)
        * [companies.add](#companiesadd)
        * [companies.search](#companiessearch)
        * [companies.get](#companiesget)
    * [Операции с компанией текущей сессии менеджера](#Операции-с-компанией-текущей-сессии-менеджера)
        * [company.get](#companyget)
        * [company.set](#companyset)
        * [company.getEquity](#companygetEquity)
    * [Операции с клиентами сервиса RAMM](#Операции-с-клиентами-сервиса-RAMM)
        * [clients.addWithWallet](#clientsaddWithWallet)
        * [clients.search](#clientssearch)
        * [clients.get](#clientsget)
        * [clients.set](#clientsset)
        * [clients.getCharges](#clientsgetCharges)
        * [clients.searchStatistic](#clientssearchStatistic)
    * [Кошельки клиентов](#Кошельки-клиентов)
        * [wallets.get](#walletsget)
        * [wallets.search](#walletssearch)
    * [Операции с кошельками клиентов](#Операции-с-кошельками-клиентов)
        * [walletTransfers.add](#walletTransfersadd)
        * [walletTransfers.search](#walletTransferssearch)
        * [walletTransfers.get](#walletTransfersget)
        * [walletTransfers.getRewards](#walletTransfersgetRewards)
    * [Операции со стратегиями RAMM](#Операции-со-стратегиями-RAMM)
        * [strategies.search](#strategiessearch)
        * [strategies.get](#strategiesget)
        * [strategies.add](#strategiesadd)
        * [strategies.set](#strategiesset)
        * [strategies.close](#strategiesclose)
        * [strategysymbolstat.get](#strategysymbolstatget)
    * [Операции с торговыми счетами клиентов](#Операции-с-торговыми-счетами-клиентов)
        * [accounts.search](#accountssearch)
        * [accounts.get](#accountsget)
        * [accounts.set](#accountsset)
        * [accounts.close](#accountsclose)
        * [accounts.getStatement](#accountsgetStatement)
        * [accounts.pause](#accountspause)
        * [accounts.resume](#accountsresume)
    * [Операции со сделками на счете](#Операции-со-сделками-на-счете)
        * [deals.search](#dealssearch)
        * [deals.get](#dealsget)
    * [Операции с открытыми позициями](#Операции-с-открытыми-позициями)
        * [positions.search](#positionssearch)
        * [positions.get](#positionsget)
    * [Операции с записями об исполнении агрегированного ордера](#Операции-с-записями-об-исполнении-агрегированного-ордера)
        * [fills.search](#fillssearch)
        * [fills.get](#fillsget)
    * [Операции с запросами на исполнение](#Операции-с-запросами-на-исполнение)
        * [fillRequests.search](#fillRequestssearch)
        * [fillRequests.get](#fillRequestsget)
    * [Операции с сигналами на исполнение](#Операции-с-сигналами-на-исполнение)
        * [signals.search](#signalssearch)
        * [signals.get](#signalsget)
    * [Активы: валюты, акции, фьючерсы и т. п.](#Активы-валюты-акции-фьючерсы-и-т-п)
        * [assets.search](#assetssearch)
        * [assets.get](#assetsget)
    * [Торговые инструменты](#Торговые-инструменты)
        * [symbols.search](#symbolssearch)
        * [symbols.get](#symbolsget)
    * [Передача торговых сигналов](#Передача-торговых-сигналов)
        * [trading.searchStrategies](#tradingsearchStrategies)
        * [trading.addSignal](#tradingaddSignal)
        * [trading.addSync](#tradingaddSync)
    * [Операции со стримами](#Операции-со-стримами)
        * [streams.get](#streamsget)
* [Ошибки и возможные коды ответа](#Ошибки-и-возможные-коды-ответа)

## Введение

Менеджерский API доступен по адресу https://ramm.store/api/manager/.
API построен на указании названий вызываемых методов в URL и передаче дополнительной информации в JSON. Для получения и изменения информации используется метод POST.
Например, https://ramm.store/api/manager/v1/clients.search

Любые действия с использованием API требуют аутентификации от имени зарегистрированного менеджера.

Заголовок запроса должен содержать значения: 

Token: <токен>
Content-Type: application/jsonstrategies

## Регистрация менеджера
Новый менеджер может быть зарегистрирован следующими способами:

1. На сайте RAMM.TECH, при создании новой компании:
* Войти на сайт http://ramm.tech
* Заполнить анкету (данные о компании и себе).
* Нажать кнопку "Продолжить".
* На указанную почту высылается письмо, в которой надо пройти по ссылке для подтверждения почты.
* Нужно открыть письмо и пройти по ссылке в нем.
* Происходит регистрация компании и менеджера. Менеджеру добавляются все права на данную компанию.
* На указанную почту приходит регистрационная информация с указанием логина и пароля для входа, а так же ссылка на вход в бекофис.
* При первом входе рекомендуется поменять пароль.
2. По инвайту
* Зарегистрированный менеджер может создать инвайт на регистрацию менеджера и новой компании.
* После создания, если произойдет регистрация с использованием кода инвайта, то новый менеджер и его компания будут считаться привлеченными создателем инвайта.
* В остальном, регистрация менеджера и новой компании происходит аналогично варианту создания без инвайта.
3. Регистрация через бэкофис
* Регистрация в существующей компании (родительской или дочерней). Указывается логин, пароль, набор прав.
Зарегистрированный менеджер, используя логин и пароль, может войти на сайт https://backoffice.ramm.store и нажать на кнопку Login.
В открывшемся окне вводит логин и пароль, при этом отправляется запрос к сайту. Сайт возвращает URL AccessServer'а, на который можно войти с этими регистрационными данными. Далее происходит автоматический редирект на этот URL с указанными данными.

*Аналогично реализован функционал API для входа. Сначала отправляется запрос к основному домену, который возвращает URL AccessServer'а. Далее, на выданном URL вызывается метод session.login, который возвращает токен. Все остальные функции менеджерского API используют этот токен для авторизации.*

## Описание методов API
## Аутентификация, действия с собственной сессией

# session.login
Авторизует пользователя по логину и паролю, создает сессию. Возвращает токен для использования API и информацию по сессии (без списка прав).

При вызове без указания номера компании возвращается список всех доступных компаний.

URL вызова: https://ramm.store/api/manager/v1/session.login

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
Login	| string	| Логин |
Password |	string |	Пароль |
CompanyID |	number |	Номер компании |
ExpirationMinutes |	number |	Автоматическое удаление сессии при неактивности в течении заданного времени. Значение по умолчанию: 10. |

Возвращаемые данные 1 (при вызове с указанием CompanyID): строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
Token	| string |	Токен, уникальный идентификатор |
Login	| string |	Логин |
ManagerID |	number |	ID менеджера |
CompanyID |	number |	ID компании |
DTCreated |	number |	Время создания |
DTLastActivity |	number |	Время последней активности |
DTRightsLastUpdate |	number |	Время последнего изменения прав |
ExpirationMinutes |	number |	Длительность сессии |
Demo |	boolean |	Признак демо-сессии (сессия для работы с демо-компанией) |

Возвращаемые данные 2 (при вызове без указания CompanyID) - массив Companies, каждый элемент которого содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID	| number |	ID компании |
Name |	string |	Название компании |
Demo |	boolean |	Признак демо-компании |
URL  |	string |	URL для создания сессии по данной компании |


Пример вызова:


{
"Login":"test@gmail.com",
"Password": "qwert12345",
"CompanyID": 0,
"ExpirationMinutes":100
}

Примеры ответа:

(1) вызов с указанием CompanyID

{
"Token":"43C2CD3D-D6B6-4E0E-AD14-3954A0CA2EE6",
"Login":"test@gmail.com",
"ManagerID":5,
"CompanyID":0,
"DTCreated":"2018-11-23T11:59:12.493",
"DTLastActivity":"2018-11-23T11:59:12.493",
"DTRightsLastUpdate":"2018-11-23T11:54:00",
"ExpirationMinutes":100,
"Demo":false
}


(2) вызов без указания CompanyID

{
"Companies":
[
{
"ID": 158,
"Name": "SomeBrokerCompany",
"Demo": false,
"URL": "https://dc1.ramm.tech"
},
{
"ID": 356,
"Name": "AnotherCompnay",
"Demo": true,
"URL": "https://dc2.ramm.tech"
}
]
}

## session.get
Отдает информацию по активной сессии (без списка прав).

URL вызова: https://ramm.store/api/manager/v1/session.get

Тело запроса: отсутствует

Возвращаемые данные: строка JSON, содержит параметры
Параметр | Тип | Описание 
---------|----------|----------
Token	| string |	Токен, уникальный идентификатор |
Login |	string |	Логин |
ManagerID |	number |	ID менеджера|
CompanyID |	number |	ID компании |
DTCreated |	number |	Время создания |
DTLastActivity |	number |	Время последней активности |
DTRightsLastUpdate|	number |	Время последнего изменения прав |
ExpirationMinutes |	number |	Длительность сессии |
Demo|	boolean|	Признак демо-сессии (сессия для работы с демо-компанией)|


Пример ответа:

{
"Token":"43C2CD3D-D6B6-4E0E-AD14-3954A0CA2EE6",
"Login":"test@gmail.com",
"ManagerID":5,
"CompanyID":0,
"DTCreated":"2018-11-23T11:59:12.493",
"DTLastActivity":"2018-11-23T11:59:12.493",
"DTRightsLastUpdate":"2018-11-23T11:54:00",
"ExpirationMinutes":100,
"Demo":false
}

## session.getMethods
Отдает список доступных методов по текущей сессии.

URL вызова: https://ramm.store/api/manager/v1/session.getMethods

Тело запроса: отсутствует

Возвращаемые данные: строка, содержащая массив Methods из названий методов, упакованный в JSON

Пример ответа:

{
"Methods":
[
"managers.getMethods",
"clients.search"
]
}

## session.heartbeat
Используется для поддержания сессии в активном состоянии. Возвращает дату последнего обновления прав менеджера.

URL вызова: https://ramm.store/api/manager/v1/session.heartbeat

Тело запроса: отсутствует

Возвращаемые данные: строка JSON, содержит параметры
Параметр | Тип | Описание 
---------|----------|----------
DTRightsLastUpdate|	number|	Время последнего обновления прав менеджера|
UpdateImportance|	number|	0-no notification, 1-minor, 2-standard, 3-important, 4-very important, 5-critical|


Пример ответа:

{
"DTRightsLastUpdate":"2018-11-23T11:54:00",
"UpdateImportance":0
}

## session.getMySessions
Отдает список всех активных сессий данного менеджера.

URL вызова: https://ramm.store/api/manager/v1/session.getMySessions

Тело запроса: отсутствует

Возвращаемые данные - массив JSON, каждый элемент которого содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
Token|	string|	Токен, уникальный идентификатор|
Login|	string|	Логин|
ManagerID|	number|	ID менеджера|
CompanyID|	number|	ID компании|
DTCreated|	number|	Время создания|
DTLastActivity|	number|	Время последней активности|
DTRightsLastUpdate|	number|	Время последнего изменения прав|
ExpirationMinutes|	number|	Длительность сессии|
Demo|	boolean|	Признак демо-сессии (сессия для работы с демо-компанией)|


Пример ответа:

[
{
"Token":"43C2CD3D-D6B6-4E0E-AD14-3954A0CA2EE6",
"Login":"test@gmail.com",
"ManagerID":5,
"CompanyID":0,
"DTCreated":"2018-11-23T11:50:12.493",
"DTLastActivity":"2018-11-23T11:51:12.493",
"DTRightsLastUpdate":"2018-11-23T11:50:59",
"ExpirationMinutes":100,
"Demo":false
},

{
"Token":"43C2CD3D-D6B6-4E0E-AD14-3954A0CA2EE6",
"Login":"test@gmail.com",
"ManagerID":5,
"CompanyID":0,
"DTCreated":"2018-11-23T11:53:11.769",
"DTLastActivity":"2018-11-23T11:55:25.583",
"DTRightsLastUpdate":"2018-11-23T11:54:00",
"ExpirationMinutes":100,
"Demo":false
}
]

## session.logout
Удаляет сессию.

URL вызова: https://ramm.store/api/manager/v1/session.logout

Тело запроса: отсутствует

Возвращаемые данные: отсутствуют

## Управление сессиями других менеджеров

## sessions.search
Отдает все сессии (без токенов) по компании запрашивающего менеджера.

URL вызова: https://ramm.store/api/manager/v1/sessions.search

Тело запроса: отсутствует

Возвращаемые данные - массив Sessions, каждый элемент которого содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
Token|	string|	Токен, уникальный идентификатор|
Login|	string|	Логин|
ManagerID|	number|	ID менеджера|
CompanyID|	number|	ID компании|
DTCreated|	number|	Время создания|
DTLastActivity|	number|	Время последней активности|
DTRightsLastUpdate|	number|	Время последнего изменения прав|
ExpirationMinutes|	number|	Длительность сессии|
Demo|	boolean|	признак демо-сессии (сессия для работы с демо-компанией)|


Пример ответа:

{
"Sessions":
[
{
"Token":"43C2CD3D-D6B6-4E0E-AD14-3954A0CA2EE6",
"Login":"test1@gmail.com",
"ManagerID":1,
"CompanyID":0,
"DTCreated":"2018-11-23T11:50:12.493",
"DTLastActivity":"2018-11-23T11:51:12.493",
"DTRightsLastUpdate":"2018-11-23T11:50:59",
"ExpirationMinutes":100,
"Demo":false
},
{
"Token":"43C2CD3D-D6B6-4E0E-AD14-3954A0CA2EE6",
"Login":"test2@gmail.com",
"ManagerID":2,
"CompanyID":0,
"DTCreated":"2018-11-23T11:53:11.769",
"DTLastActivity":"2018-11-23T11:55:25.583",
"DTRightsLastUpdate":"2018-11-23T11:54:00",
"ExpirationMinutes":100,
"Demo":false
}
]
}

## sessions.logout
Удаляет все сессии другого менеджера в той же компании по логину.

URL вызова: https://ramm.store/api/manager/v1/sessions.logout

Тело запроса: строка JSON, содержит параметры

Параметр | Тип | Описание 
---------|----------|----------
Login|	string|	Логин менеджера|

Возвращаемые данные: отсутствуют

## Информация о залогиненном менеджере, получение и изменение собственного профиля

## profile.get
Информация о залогиненном менеджере (то есть о самом себе)

URL вызова: https://ramm.store/api/manager/v1/profile.get

Тело запроса: отсутствует

Возвращаемые данные: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID менеджера|
Login|	string|	Логин|
FirstName|	string|	Имя|
LastName|	string|	Фамилия|
Mobile|	string|	Номер телефона|


Пример ответа:

{
"ID":1,
"Login":"test1@gmail.com",
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482"
}

## profile.set
Обновление менеджером информации о себе.

URL вызова: https://ramm.store/api/manager/v1/profile.set

Тело запроса: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
FirstName|	string|	Имя|
LastName|	string|	Фамилия|
Mobile|	string|	Номер телефона|

Возвращаемые данные: отсутствуют

Пример вызова:

{
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482"
}

## Действия с настройками пользователей
        
## settings.set
Сохранение настроек пользователя. При вызове с указанием SettingsKey, но без указания Settings, 
существующая настройка с ключом SettingsKey будет удалена.

URL вызова: https://ramm.store/api/manager/v1/settings.set

Тело запроса: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
SettingsKey|	string|	Ключ настройки|
Settings|	string|	Строка с настройками|

Возвращаемые данные: отсутствуют

Пример вызова:

{
"SettingsKey" : "test",
"Settings" : "test settings"
}

## settings.get
Получение настроек пользователя

URL вызова: https://ramm.store/api/manager/v1/settings.get

Тело запроса: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
SettingsKey|	string|	Ключ настройки|

Возвращаемые данные: Строка настроек

Пример вызова:

{
"SettingsKey" : "test"
}

## Действия с паролями

## password.set
Установка пароля собственной учетной записи менеджера.

URL вызова: https://ramm.store/api/manager/v1/password.set

Тело запроса: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
OldPassword|	string|	Строка со старым паролем для сверки|
Password|	string|	Строка с новым паролем|

Возвращаемые данные: отсутствуют

Пример вызова:

{
"OldPassword":"trewq54321",
"Password":"qwert12345"
}

## clients.setPassword
Установка пароля для учетной записи клиента.

URL вызова: https://ramm.store/api/manager/v1/clients.setPassword

Тело запроса: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
ClientID|	string|	ID клиента|
Password|	string|	Строка с паролем|

Возвращаемые данные: отсутствуют

Пример вызова:

{
"ClientID":333,
"Password":"qwert12345"
}

## clients.getOTP
Получение одноразового пароля для входа в платформу из ЛК брокера

URL вызова: https://ramm.store/api/manager/v1/clients.getOTP

Тело запроса: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
ClientID|	string|	ID клиента|


Возвращаемые данные: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
OTP|	string|	Одноразовый пароль|


Пример вызова:

{
"ClientID":333
}


Пример ответа:

{
"OTP": "7ff8b899a6414086ad254158c9365a31"
}

## Операции с менеджерами, управляющими сервисом RAMM

## managers.add
Создает менеджера в компании запрашивающего менеджера.

URL вызова: https://ramm.store/api/manager/v1/managers.add

Тело запроса - строка JSON, содержит структуру Manager и массив Methods с методами, на которые нужно установить права:

Структура |	Параметр | Тип | Описание
---------|----------|----------|---------------
Manager|	   Login|	   string|	   Логин|
 -|  Password|	   string|	Пароль|
 -|    FirstName|	   string|	Имя|
 -|    LastName|	   string|	Фамилия|
 -|    Mobile|	   string|	Номер телефона|
 -|    Comment|	   string|	Комментарий|
Methods|         |   string|	Название метода, на который устанавливаются права|

Возвращаемые данные: строка JSON, содержит параметры:

Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID менеджера|
Login|	string|	Логин|
Password|	string|	Пароль|


Пример вызова:

{
"Manager":
{  
"Login":"test@gmail.com",
"Password": "qwert12345",
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Comment":"Some comment"
},
"Methods": 
[
"session.login",
"session.get",
"session.heartbeat",
"session.getMySessions",
"session.getMethods"
]
}


Пример ответа:

{
"ID": 158,
"Login":"test@gmail.com",
"Password": "qwert12345"
}

## managers.search
Поиск менеджеров по подстроке, либо по фильтру.

URL вызова: https://ramm.store/api/manager/v1/managers.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|---------------
Filter|	Value|	string|	Подстрока поиска|
-| ID|	number|	ID менеджера|
-| Login|	string|	Логин|
-| FirstName|	string|	Имя|
-| LastName|	string|	Фамилия|
-| Mobile|	string|	Номер телефона|
-| Comment|	string|	Комментарий|
-| Status|	number|	0-new not activated, 1-active, 2-temporary disabled, 3-disabled and hidden, 4-deleted|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, Login, FirstName, LastName, Status, Tag, Comment, Mobile|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Managers, каждый элемент которого содержит параметры:
Структура |	Параметр | Тип | Описание
---------|----------|----------|---------------
Filter|	Value|	string|	Подстрока поиска|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
-| MaxPerPage|	number|	Максимальное количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, Login, FirstName, LastName, Status, Tag, Comment, Mobile|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|
Managers|	ID|	number|	ID менеджера|
-| Login|	string|	Логин|
-| FirstName|	string|	Имя|
-| LastName|	string|	Фамилия|
-| Mobile|	string|	Номер телефона|
-| Comment|	string|	Комментарий|
-| Status|	number|	0-new, not activated, 1-active, 2-temporarily disabled, 3-disabled and hidden, 4-deleted|
-| Tag|	number|	Произвольное число (для увязки с внешними системами)|

Пример вызова:

{
"Filter":
{
"Value":"Pupk"
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
"Value":"Pupk"
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Pagination":
{
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 5,
"MaxPerPage": 100
},
"Managers":
[
{ 
"ID":1,
"Login":"test@gmail.com",
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Comment":"",
"Status":0,
"Tag":0
},
{ 
"ID":2,
"Login":"test2@gmail.com",
"FirstName":"Basilio",
"LastName":"Pupkini",
"Mobile":"+74959466483",
"Comment":"",
"Status":0,
"Tag":0
}
]
}

## managers.get
Информация по менеджеру из компании залогиненного менеджера.

URL вызова: https://ramm.store/api/manager/v1/managers.get

Тело запроса - строка JSON, содержит параметр:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID менеджера|

Возвращаемые данные: строка JSON, содержит параметры:
Параметр/Структура | Параметр | Тип | Описание
--------|----------|----------|--------------
ID| |	number|	ID менеджера|
Login| | string|	Логин|
FirstName| | string|	Имя|
LastName| | string|	Фамилия|
Mobile| | string|	Номер телефона|
Comment| | string|	Комментарий|
Tag| | string|	Метка|
Company|	ID|	number|	ID компании|
-| Name|	string|	Название компании|
-| Demo|	boolean|	Признак "демо"|

Пример вызова:

{
"ID":1,
}

Пример ответа:

{ 
"ID":1,
"Login":"test@gmail.com",
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Comment":"Some comment",
"Tag":"Tag1",
"Company":
{
"ID":2,
"Name":"FXInvest",
"Demo": false
}
}

## managers.set
Изменение менеджера, в том числе его статуса. Изменяемый менеджер должен быть из компании залогиненного менеджера.
URL вызова: https://ramm.store/api/manager/v1/managers.set

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID менеджера|
FirstName|	string|	Имя|
LastName|	string|	Фамилия|
Mobile|	string|	Номер телефона|
Comment|	string|	Комментарий|
Tag|	number|	Произвольное число (для увязки с внешними системами)|

Возвращаемые данные: отсутствуют.

Пример запроса:
{ 
"ID":1,
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Comment":"",
"Tag":0
}

## managers.getMethods
Информация по правам менеджера в определенной компании.

URL вызова: https://ramm.store/api/manager/v1/managers.getMethods

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ManagerID|	number|	ID менеджера|
CompanyID|	number|	ID компании|

Возвращаемые данные: строка, содержащая массив из названий методов, упакованный в JSON

Пример запроса:
{ 
"ManagerID":1,
"CompanyID":2
}

Пример ответа:
[
{"Method":"managers.getMethods"},
{"Method":"clients.search"}
]


## managers.setMethods
Изменение прав менеджера. Доступны для изменения только те права, которые есть в данной компании у запрашивающего менеджера.

URL вызова: https://ramm.store/api/manager/v1/managers.setMethods

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ManagerID|	number|	ID менеджера|
Methods|	массив string|	Массив имен методов|

Возвращаемые данные: отсутствуют.

Пример запроса:
{ 
"ManagerID":1,
"Methods":
[
"companies.add",
"companies.search"
]
}

## Операции с зарегистрированными в RAMM компаниями

## companies.add
Создает новую компанию в компании сессии запрашивающего менеджера.

URL вызова: https://ramm.store/api/manager/v1/companies.add

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
Name|	string|	Название компании|
URL|	string|	URL компании|
Demo|	boolean|	Признак "демо"|

Возвращаемые данные: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
CompanyID|	number|	ID компании|


Пример вызова:

{
"Name":"FXInvest",
"URL":"fxinvest.com",
"Demo": false
}

Пример ответа:

{ 
"CompanyID":1
}

## companies.search
Поиск компании

URL вызова: https://ramm.store/api/manager/v1/companies.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	Name|	string|	Подстрока поиска (в названии)|
-| ParentID|	number|	ID родительской компании|
-| ID|	number|	ID компании|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, ParentID, Name, URL, Demo, Status, ParentCompanyName
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Companies, каждый элемент которого содержит параметры:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	Name|	string|	Подстрока поиска (в названии)|
-| ParentID|	number|	ID родительской компании|
-| ID|	number|	ID компании|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
-| MaxPerPage|	number|	Максимальное количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, ParentID, Name, URL, Demo, Status, ParentCompanyName|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|
Companies|	ID|	number|	ID компании|
-| ParentID|	number|	ID родительской компании|
-| ParentCompanyName|	string|	Название родительской компании|
-| Name|	string|	Название компании|
-| URL|	string|	URL компании|
-| Demo|	boolean|	Признак "демо"|
-| Status|	number|	Статус компании (0-new, 1-ok, 2-deleted, 3... errors )|

Пример вызова:

{
"Filter":
{
"Name":"FX"
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
"Name":"FX"
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Pagination":
{
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 5,
"MaxPerPage": 100
},
"Companies":
[
{
"ID":2,
"ParentID":1,
"ParentCompanyName":"InvestFX",
"Name":"FXInvest",
"URL":"fxinvest.com",
"Demo":false,
"Status":0
},
{
"ID":3,
"ParentID":1,
"ParentCompanyName":"InvestFX",
"Name":"FXTrade",
"URL":"fxtrade.com",
"Demo":false,
"Status":0
}
]
}

## companies.get
Отдает полные данные по конкретной компании

URL вызова: https://ramm.store/api/manager/v1/companies.get

Тело запроса - строка JSON, содержит параметр:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID компании|

Возвращаемые данные: строка JSON, содержит параметры
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID компании|
ParentID|	number|	ID родительской компании|
ParentCompanyName|	string|	Название родительской компании|
Name|	string|	Название компании|
URL|	string|	URL компании|
Demo|	boolean|	Признак "демо"|
Status|	number|	Статус компании (0-new, 1-ok, 2-deleted, 3... errors )|


Пример вызова:

{
"ID":1,
}

Пример ответа:

{
"ID":2,
"ParentID":1,
"ParentCompanyName":"FXTrade",
"Name":"FXInvest",
"URL":"fxinvest.com",
"Demo":false,
"Status":0
}

## Операции с компанией текущей сессии менеджера

## company.get
Отдает базовую информацию по компании

URL вызова: https://ramm.store/api/manager/v1/company.get

Тело запроса: отсутствует.

Возвращаемые данные: строка JSON, содержит параметры:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
ID| - | number|	ID компании|
Name| - | string|	Название компании|
URL| - | string|	URL компании|
Demo| - | boolean|	Признак "демо"|
Status| - | number|	Статус компании (0-new, 1-ok, 2-deleted, 3... errors)|
Parent|	ID|	number|	ID родительской компании|
-| Name|	string|	Название родительской компании|


Пример ответа:

{
"ID":2,
"Name":"FXInvest",
"URL":"fxinvest.com",
"Demo":false,
"Status":0,
"Parent":
{
"ID":1,
"Name":"FXTrade"
}
}

## company.set
Меняет информацию по компании (Name, URL)

URL вызова: https://ramm.store/api/manager/v1/company.set

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
Name|	string|	Название компании|
URL|	string|	URL компании|

Возвращаемые данные: отсутствуют.

Пример запроса:
{ 
"Name":"FXInvest",
"URL":"fxinvest.com"
}

## company.getEquity
Суммарные данные эквити и баланса клиентов по конкретной компании.

URL вызова: https://ramm.store/api/manager/v1/company.getEquity

Тело запроса - строка JSON, содержит параметры:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	DTFrom|	number|	Дата начала|
-| DTTo|	number|	Дата конца|

Возвращаемые данные: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
IDCompany|	number|	ID компании|
DT|	number|	Дата записи|
Equity|	real|	Суммарное эквити инвестиций|
Balance|	string|	Суммарный баланс инвестиций|
WalletsBalance|	string|	Суммарный баланс кошельков клиентов|


Пример вызова:

{
"Filter":
{
"DTFrom" : "2018-11-23T11:59:12.493",
"DTTo" : "2018-11-23T12:59:12.493"
}
}

Пример ответа:

{
[
{
"IDCompany" : 2,
"DT" : "2018-11-23T11:59:12.493",
"Equity": 10000.14,
"Balance": 10000.14,
"WalletsBalance": 10000.14
},
{
"IDCompany" : 2,
"DT" : "2018-11-24T11:59:12.493",
"Equity": 10000.14,
"Balance": 10000.14,
"WalletsBalance": 10000.14
}
]
}

## Операции с клиентами сервиса RAMM

## clients.addWithWallet
Создает клиента с кошельком. На данный момент кошельки создаются только в валюте USD.

URL вызова: https://ramm.store/api/manager/v1/clients.addWithWallet

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
Login|	string|	Логин|
Password|	string|	Пароль|
FirstName|	string|	Имя|
LastName|	string|	Фамилия|
Email|	string|	Email|
Mobile|	string|	Номер телефона|
Tag|	number|	Произвольное число (для увязки с внешними системами)|
Test|	boolean|	Признак "тестовый"|
Language|	string|	Язык интерфейса, макс. 3 символа, например ISO 639-1|
Comment|	string|	Комментарий|
Country|	string|	Страна|
ExternalPartnerReference|	string|	Произвольная строка для привязки клиента к партнеру|
ExecutionServerName|	string|	Название внешнего сервера|
ExecutionAccount|	string|	Номер счета на внешнем сервере|
PrecisionVolume|	number|	Точность округления лотов|

Возвращаемые данные: строка JSON, содержит структуры Client и Wallet:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Client|	ID|	number|	ID клиента|
-| Login|	string|	Логин
-| Password|	string|	Пароль|
Wallet|	ID|	number|	ID кошелька|
-| AssetID|	number|	ID актива|
-| AssetName|	string|	Название актива|


Пример вызова:

{ 
"Login":"test@gmail.com",
"Password": "qwert12345",
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Tag":1,
"Test":true,
"Language":"ru",
"Comment":"Some comment",
"Country":"Slovenia",
"ExternalPartnerReference":"RX5676",
"ExecutionServerName":"FXTrade-MT5",
"ExecutionAccount":"454654654",
"PrecisionVolume": 2
}

Пример ответа:

{
"Client":
{
"ID": 13,
"Login": "test@gmail.com",
"Password": "qwert12345"
},
"Wallet":
{
"ID": 11,
"AssetID": 840,
"AssetName": "USD"
}
}

## clients.search
Поиск по клиентам

URL вызова: https://ramm.store/api/manager/v1/clients.search

Тело запроса - строка JSON, может (но не обязана) содержать структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
-| Test|	boolean|	Признак тестового клиента|
Filter|	Value|	string|	Подстрока поиска|
-| ID|	number|	ID клиента|
-| Login|	string|	Логин|
-| FirstName|	string|	Имя|
-| LastName|	boolean|	Фамилия|
-| Mobile|	number|	Номер телефона|
-| Comment|	string|	Комментарий|
-| Status|	number| 0 - не было балансовых проводок, 1 - были получены бонусы, 2 - было получено вознаграждение партнера, 3 - было получено вознаграждение трейдера, 4 - вносил собственные средства|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, Login, Email, FirstName, LastName, Language, Status, Tag, Test, ActiveAccountsCount, ActiveAccountsTotalBalance, ActiveAccountsTotalEquity, ActiveAccountsTotalPositionsCount, ClosedAccountsCount, ActiveStrategiesCount, ClosedStrategiesCount, WalletsTotalBalance, TotalFunds|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Clients, каждый элемент которого содержит поля:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	Value|	string|	Подстрока поиска|
OrderBy|	Field|	string|	Параметр сортировки|
-| OrderDirection|	string|	Направление сортировки|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
-| MaxPerPage|	number|	Максимальное количество записей на одной странице|
Clients|	ID|	number|	ID клиента|
-| Login|	string|	Логин|
-| Email|	string|	Адрес эл.почты|
-| FirstName|	string|	Имя|
-| LastName|	boolean|	Фамилия|
-| Mobile|	number|	Номер телефона|
-| Language|	string|	Язык интерфейса, макс. 3 символа, например ISO 639-1|
-| Status|	number|	0 - не было балансовых проводок, 1 - были получены бонусы, 2 - было получено вознаграждение партнера, 3 - было получено вознаграждение трейдера, 4 - вносил собственные средства|
-| Tag|	number|	Произвольное число (для увязки с внешними системами)|
-| Comment|	string|	Комментарий|
-| Test|	boolean|	Признак тестового клиента|
-| Enabled|	boolean|	Признак отсутствия блокировки клиента|
-| ActiveAccountsCount|	number|	Количество активных инвестиций|
-| ActiveAccountsTotalBalance|	real|	Суммарный баланс активных инвестиций|
-| ActiveAccountsTotalEquity|	real|	Суммарное эквити активных инвестиций|
-| ActiveAccountsTotalPositionsCount|	number|	Суммарное количество позиций, открытых на активных инвестициях|
-| ClosedAccountsCount|	number|	Количество закрытых инвестиций|
-| ActiveStrategiesCount|	number|	Количество активных стратегий|
-| ClosedStrategiesCount|	number|	Количество закрытых стратегий|
-| WalletsTotalBalance| real|	Суммарный баланс кошельков клиентов|
-| TotalFunds|	real|	Общая сумма средств клиентов|


Пример вызова:

{
"Filter":
{
"Value":"te"
},
"OrderBy":
{
"Field": "Login",
"Direction": "Desc"
},
"Pagination":
{
"CurrentPage": 2,
"PerPage": 20
}
}


Пример ответа:

{
"Filter":
{
"Value": "te"
},
"OrderBy": {
"Field": "Login",
"Direction": "Desc"
},
"Pagination": {
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 20,
"MaxPerPage": 100
},
"Clients":
[
{
"ID":2,
"Login":"test@test.ru",
"Email":"test@test.ru",
"FirstName":"Vassily",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Comment":"",
"Language":"ru",
"Status":0,
"Tag": 232,
"Test":0,
"Enabled":1,
"ActiveAccountsCount":3,
"ActiveAccountsTotalBalance":523.45,
"ActiveAccountsTotalEquity":575.49,
"ActiveAccountsTotalPositionsCount":5,
"ClosedAccountsCount":10,
"ActiveStrategiesCount":5,
"ClosedStrategiesCount":6,
"WalletsTotalBalance":1542.43,
"TotalFunds":2065.88
},
{
"ID":3,
"Login":"test1@test.ru",
"Email":"test1@test.ru",
"FirstName":"Vasilisa",
"LastName":"Pupkina",
"Mobile":"+74959466482",
"Comment":"",
"Language":"en",
"Status":0,
"Tag": 233,
"Test":0,
"Enabled":1,
"ActiveAccountsCount":3,
"ActiveAccountsTotalBalance":523.45,
"ActiveAccountsTotalEquity":575.49,
"ActiveAccountsTotalPositionsCount":5,
"ClosedAccountsCount":10,
"ActiveStrategiesCount":5,
"ClosedStrategiesCount":6,
"WalletsTotalBalance":1542.43,
"TotalFunds":2065.88
}
]
}

## clients.get
Получает информацию о клиенте.

URL вызова: https://ramm.store/api/manager/v1/clients.get

Тело запроса - строка JSON, содержит параметр:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID клиента|

Возвращаемые данные: строка JSON, содержит параметры:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
-| ID|	number|	ID клиента|
-| IDCompany|	number|	ID компании|
-| IDReferralToken|	number|	ID реферала|
-| RegisteredDate|	number|	Дата регистрации|
-| FirstName|	string|	Имя|
-| LastName|	string|	Фамилия|
-| Mobile|	string|	Номер телефона|
-| Login|	string|	Логин|
-| Comment|	string|	Комментарий|
-| Language|	string|	Язык интерфейса, макс. 3 символа, например ISO 639-1|
-| Status| 	number| 	0 - не было балансовых проводок, 1 - были получены бонусы, 2 - было получено вознаграждение партнера, 3 - было получено вознаграждение трейдера, 4 - вносил собственные средства|
-| Tag|	number|	Произвольное число (для увязки с внешними системами)|
-| Country|	string|	Страна регистрации|
-| Email|	string|	Эл.почта|
-| Enabled|	boolean|	Флаг "Клиент активен"|
-| ExternalPartnerReference|	string|	Внешняя партнерская ссылка|
-| ReferralRebate|	real|	Размер партнерских выплат|
-| Test|	boolean|	Флаг "тестовый клиент"|
-| StrategyDefaultStreamID|	number|	Стрим при создании стратегии, настройка по умолчанию|
-| StrategyDefaultHedge|	real|	Доля А-бук при создании стратегии, настройка по умолчанию|
-| StrategyDefaultStreamName|	string|	Название стрима по умолчанию при создании стратегии|
AvailableStreams|	StreamID|	number|	ID стрима|
-| Name|	string|	Название стрима|
Statistic|	GrossBalance| 	real|	Суммарный текущий баланс всех счетов клиента, кроме типа 1|
-| NetBalance|	real|	Sum (IIF ((a.Balance - a.Bonus) < 0 and a.Margin = 0, 0, a.Balance - a.Bonus)) кроме типа 1|
-| Equity|	real|	Суммарный эквити всех счетов клиента, кроме типа 1|
-| NetEquity|	real|	Разница между суммой эквити и бонусов по всем счетам клиента, кроме типа 1|
-| ActiveAccounts|	real|	Количество счетов клиента, у которых State < 15, кроме типа 1|
-| ClosedAccounts|	real|	Количество счетов клиента, у которых State = 15, кроме типа 1|
-| ActiveStrategies|	real|	Количество стратегий клиента, у которых DTClosed is null|
-| ClosedStrategies|	real|	Количество стратегий клиента, у которых DTClosed is not null|
-| ClientFundsTotalIn|	real|	Сумма трансферов типа 0 на все кошельки клиента|
-| ClientFundsTotalOut|	real|	Модуль суммы трансферов типа 1 на все кошельки клиента|
-| ClientFundsDelta|	real|	ClientFundsTotalIn-ClientFundsTotalOut|
-| ClientBonusDelta|	real|	Сумма трансферов типа 2 на все кошельки клиента минус модуль суммы трансферов типа 3 на все кошельки клиента|
-| GrossProfit|	real|	Сумма Profit по дилам типа 0 и 1 (c учётом свопов и комиссий)|
-| NetProfit|	real|	Сумма Profit по дилам типа 0 и 1 (без учёта свопов и комиссий)|
-| GrossSwap|	real|	Сумма Swap по дилам типа 0 и 1|
-| CommissionBroker|	real|	Сумма CommissionBroker по дилам типа 0 и 1|
-| CommissionTrader|	real|	Сумма CommissionTrader по дилам типа 0 и 1|
-| FeeCharged|	real|	Сумма Profit по дилам типа 7|
-| FeeReceived|	real|	Сумма трансферов типа 6 на все кошельки клиента|
-| CommissionReceived|	real|	Сумма трансферов типа 7 на все кошельки клиента|
-| Volume|	real|	Сумма модуля значений колонки Volume дилов типа 0 и 1|
-| NetFloatingProfit|	real|	Сумма TotalProfit по всем позициям|


Пример вызова:


{
"ID":1,
}

Пример ответа:

{
"ID":1,
"IDCompany":2,
"IDLanguage":0,
"IDReferralToken":0,
"RegisteredDate":"2018-11-23T11:59:12.493",
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Login":"test@test.ru",
"Comment":"",
"Language":"ru",
"Status":0,
"Tag":4545,
"Country":"Vanuatu",
"Email":"test@test.ru",
"Enabled":true,
"ExternalPartnerReference":"121321321",
"ReferralRebate":23.45,
"Test":false,
"StrategyDefaultStreamID":5,
"StrategyDefaultHedge":0.75,
"StrategyDefaultStreamName":"Stream A",
"AvailableStreams":
{
"StreamID":5,
"Name":"Stream A"
}
"Statistic":
{

"GrossBalance":100.00,
"NetBalance":100.00,
"Equity":100.00,
"NetEquity":100.00,
"ActiveAccounts":100.00,
"ClosedAccounts":100.00,
"ActiveStrategies":100.00,
"ClosedStrategies":100.00,
"ClientFundsTotalIn":100.00,
"ClientFundsTotalOut":100.00,
"ClientFundsDelta":100.00,
"ClientBonusDelta":100.00,
"GrossProfit":100.00,
"NetProfit":100.00,
"GrossSwap":100.00,
"CommissionBroker":100.00,
"CommissionTrader":100.00,
"FeeCharged":100.00,
"FeeReceived":100.00,
"CommissionReceived":100.00,
"Volume":100.00,
"NetFloatingProfit":100.00
}
}

## clients.set
Изменяет клиента.

URL вызова: https://ramm.store/api/manager/v1/clients.set

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID клиента|
FirstName|	string|	Имя|
LastName|	string|	Фамилия|
Mobile|	string|	Номер телефона|
Login|	string|	Логин|
Comment|	string|	Комментарий|
Tag|	number|	Произвольное число (для увязки с внешними системами)|
Country|	string|	Страна регистрации|
Email|	string|	Эл.почта|
Language|	string|	Язык интерфейса, макс. 3 символа, например ISO 639-1|
Enabled|	boolean|	Флаг "Клиент активен"|
ExternalPartnerReference|	string|	Внешняя партнерская ссылка|
StrategyDefaultStreamID|	number|	Стрим при создании стратегии, настройка по умолчанию|
StrategyDefaultHedge|	real|	Доля А-бук при создании стратегии, настройка по умолчанию|

Возвращаемые данные: отсутствуют

Пример вызова:

{
"ID":1,
"IDLanguage":0,
"FirstName":"Vasiliy",
"LastName":"Pupkin",
"Mobile":"+74959466482",
"Login":"test@test.ru",
"Comment":"",
"Tag":4545,
"Country":"Vanuatu",
"Email":"test@test.ru",
"Language":"ru",
"Enabled":true,
"ExternalPartnerReference":"121321321",
"StrategyDefaultStreamID":5,
"StrategyDefaultHedge":0.75
}

## clients.getCharges
Возвращает информацию о снятиях фи и комиссий по всем счетам с заданного сервера MT с ID больше, чем указанный в заголовке ID

URL вызова: https://ramm.store/api/manager/v1/clients.getCharges

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ExecutionServerName|	string|	Имя сервера MT|
ID|	number|	ID последней проведенной операции|


Возвращаемые данные - массив Charges, каждый элемент которого содержит поля:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Charges|	ExecutionAccount|	number|	ID счета в MT|
-| ID|	number|	ID операции|
-| Charges|	real|	Сумма снятия|


Пример вызова:

{
"ExecutionServerName":"Server1",
"ID":146
}

Пример ответа:

{
    "Charges": [
        {
            "ExecutionAccount": "50000059",
            "ID": 150,
            "Charges": 16.81
        },
        {
            "ExecutionAccount": "50000059",
            "ID": 153,
            "Charges": 16.81
        }
    ]
}

## clients.searchStatistic
Поиск клиентской статистики

URL вызова: https://ramm.store/api/manager/v1/clients.searchStatistic

Тело запроса - строка JSON, может (но не обязана) содержать структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	Login|	string|	Логин|
-| Test|	number|	Признак тестового клиента|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: IDClient, IDCompany, Test, AccountsBalance, AccountsEquity, AccountsProfitCurrentIntervalGross, AccountsProfitPositionsGross, AccountsPastProfit, AccountsFeeCharged,  AccountsFeePaid, AccountsCommissionPaid, AccountsActive, AccountsClosed, StrategiesActive, StrategiesClosed, StrategiesFeeReceived, StrategiesFeeDue, StrategiesCommissionReceived, StrategiesCommissionDue, WalletTransfersTotalIn, WalletTransfersTotalOut, WalletTransfersTotalDelta, Swap, DealsVolume, WalletsTotalBalance, PositionsCount, Login|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Statistic, каждый элемент которого содержит поля:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	Login|	string|	Логин|
-| Test|	number|	Признак тестового клиента|
OrderBy|	Field|	string|	Параметр сортировки|
-| OrderDirection|	string|	Направление сортировки|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
Statistic|	IDClient|	number|	ID клиента|
-| IDCompany|	number|	ID компании|
-| Test|	number|	Признак тестового счета|
-| AccountsBalance|	real|	Суммарный баланс инвестиций|
-| AccountsEquity|	real|	Суммарное эквити инвестиций|
-| AccountsProfitCurrentIntervalGross|	real|	Суммарный профит в текущем интервале|
-| AccountsProfitPositionsGross|	real|	Суммарный профит по открытым позициям|
-| AccountsPastProfit|	real|	Суммарная прибыль предыдущих периодов|
-| AccountsFeeCharged| real|	Суммарное начисленное вознаграждение (по инвестициям)|
-| AccountsFeePaid|	real|	Суммарное выплаченное вознаграждение (по инвестициям)|
-| AccountsCommissionPaid|	real|	Суммарная выплаченная комиссия (по инвестициям)|
-| AccountsActive|	number|	Количество активных инвестиций|
-| AccountsClosed	|number|	Количество закрытых инвестиций|
-| StrategiesActive|	number|	Количество активных стратегий|
-| StrategiesClosed|	number|	Количество закрытых стратегий|
-| StrategiesFeeReceived|	real|	Суммарное полученное вознаграждение (по стратегиям)|
-| StrategiesFeeDue|	real|	Суммарное начисленное, но не полученное вознаграждение (по стратегиям)|
-| StrategiesCommissionReceived|	real|	Суммарная полученная комиссия (по стратегиям)|
-| StrategiesCommissionDue|	real|	Суммарная начисленная, но не полученная комиссия (по стратегиям)|
-| WalletTransfersTotalIn| real|	Сумма входящих перечислений|
-| WalletTransfersTotalOut|	real|	Сумма исходящих перечислений|
-| WalletTransfersTotalDelta|	real|	Разница входящих и исходящих|
-| Swap|	real|	Свопы|
-| DealsVolume|	real|	Объем сделок|
-| WalletsTotalBalance|	real|	Суммарный баланс кошельков|
-| PositionsCount|	number|	Количество открытых позиций|
-| Login|	string|	Логин клиента|


Пример вызова:

{
"Filter":
{
"Login":"vp@test.com",
"Test":0
},
"OrderBy":
{
"Field": "Login",
"Direction": "Desc"
},
"Pagination":
{
"CurrentPage": 2,
"PerPage": 20
}
}

Пример ответа:

{
"Filter":
{
"Login":"vp@test.com",
"Test":0
},
"OrderBy":
{
"Field": "Login",
"Direction": "Desc"
},
"Pagination":
{
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 20
},
"Statistic":
[
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
]
}

## Кошельки клиентов

## wallets.get
Отдает полную информацию по кошельку.

URL вызова: https://ramm.store/api/manager/v1/wallets.get

Тело запроса - строка JSON, содержит параметр:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID кошелька|

Возвращаемые данные: строка JSON, содержит параметры (некоторые поля могут отсутствовать):
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID кошелька|
ClientID|	number|	ID клиента|
Asset|	string|	Название валюты кошелька|
DTCreated|	number|	Дата создания кошелька|
Balance|	real|	Баланс кошелька|
Bonus|	real|	Начисленный бонус|
Status|	number|	0-new, 1-active|
Invested|	real|	Инвестированная сумма|
Margin|	real|	Задействованная маржа|
IntervalPnL|	real|	Прибыль/убыток в текущем торговом интервале|
Test|	boolean|	Признак тестового клиента|


Пример вызова:


{
"ID":1,
}

Пример ответа:

{
"ID":222,
"ClientID":111,
"Asset":"USD",
"DT":"2018-11-23T11:59:12.493",
"Balance": 1000,
"Status": 0,
"Invested": 0,
"Margin": 0,
"IntervalPnL": 0,
"Test": 0
}



## wallets.search
Поиск кошельков по ID клиента (в будущем количество параметров поиска будет расширяться)

URL вызова: https://ramm.store/api/manager/v1/wallets.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	ClientID|	number|	ID клиента|
-| TestClient|	boolean|	Признак тестового клиента|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, ClientID, Asset, DTCreated, Balance, Bonus, Status, Invested, Margin, IntervalPnL|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Wallets, каждый элемент которого содержит поля:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	ClientID|	number|	ID клиента|
-| TestClient|	boolean|	Признак тестового клиента|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, ClientID, Asset, DTCreated, Balance, Bonus, Status, Invested, Margin, IntervalPnL|
-| Direction|	string|	Направление сортировки|
Wallets|	ID|	number|	ID кошелька|
-| ClientID|	number|	ID клиента|
-| Asset|	string|	Название валюты кошелька|
-| DTCreated|	number|	Дата создания кошелька|
-| Balance|	real|	Баланс кошелька|
-| Bonus|	real|	Начисленный бонус|
-| Status|	number|	0-new, 1-active|
-| Invested|	real|	Инвестированная сумма|
-| Margin|	real|	Задействованная маржа|
-| IntervalPnL|	real|	Прибыль/убыток в текущем торговом интервале|


Пример вызова:

{
"Filter":
{
"ClientID":111
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Pagination":
{
"CurrentPage": 1,
"PerPage": 20
}
}


Пример ответа:

{
"Filter":
{
"ClientID":111
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Pagination":
{
"TotalRecords": 1,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 20
},
"Wallets":
[
{
"ID":220,
"ClientID":111,
"Asset":"USD",
"DTCreated":"2018-11-23T11:59:12.493",
"Balance": 1000,
"Status": 0,
"Invested": 0,
"Margin": 0,
"IntervalPnL": 0
}
]
}

## Операции с кошельками клиентов

## walletTransfers.add
Добавляет перевод средств: пополнение, снятие, пополнение бонусом, снятие бонуса.
При указании Amount с точностью больше, чем точность в БД, такой трансфер будет отклоняться.

URL вызова: https://ramm.store/api/manager/v1/walletTransfers.add

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
WalletID|	number|	ID кошелька|
ExecutionServerName|	string|	Название торговго сервера исполнения|
ExecutionAccount|	string|	Счет исполнения|
ExternalTransferID|	number|	ID внешнего перевода|
Type|	number|	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners|
Amount|	real|	Сумма|
CommentClient|	string|	Комментарий для клиента|
CommentCompany|	string|	Внутренний комментарий|

Возвращаемые данные: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
WalletTransferID|	number|	ID перевода|


Пример вызова:

{ 
"WalletID":12321,
"ExecutionServerName":"FXTrade MT5",
"ExecutionAccount":"465465465",
"ExternalTransferID": 546546,
"Type":2,
"Amount":254.56,
"CommentClient":"Бонус",
"CommentCompany":"Бонусы исчерпаны"
}

Пример ответа:

{
"WalletTransferID":7654
}



## walletTransfers.search
Поиск переводов с фильтрацией по ID кошелька и ID клиента. Входные параметры могут быть не заданы или заданы частично, тогда поиск вернет весь список переводов.

URL вызова: https://ramm.store/api/manager/v1/walletTransfers.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	WalletID|	number|	ID кошелька|
-| ID|	number|	ID трансфера|
-| Type|	number|	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners|
-| ClientID|	number|	ID клиента|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, WalletID, ExternalTransferID, DT, Type, Amount, CommentClient, CommentCompany, Test|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|

Возвращаемые данные - структуры Pagination, Filter, OrderBy и массив WalletTransfers:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	WalletID|	number|	ID кошелька|
-| ID|	number|	ID трансфера|
-| Type|	number|	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners|
-| ClientID|	number|	ID клиента|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, WalletID, ExternalTransferID, DT, Type, Amount, CommentClient, CommentCompany, Test|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
-| MaxPerPage|	number|	Максимальное количество записей на одной странице|
WalletTransfers|	ID|	number|	ID перевода|
-| WalletID|	number|	ID кошелька|
-| ExternalTransferID|	number|	ID внешнего перевода|
-| DT|	number|	Дата перевода|
-| Type|	number|	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners|
-| Amount|	real|	Сумма перевода|
-| CommentClient|	string|	Комментарий для клиента|
-| CommentCompany|	string|	Внутренний комментарий|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|


Пример вызова:

{
"Filter":
{
"WalletID":222,
"ClientID":111,
"Type":0
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
"WalletID":222,
"ClientID":111,
"Type":0
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Pagination":
{
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 5,
"MaxPerPage": 100
},
"WalletTransfers":
[
{
"ID":220,
"WalletID":222,
"ExternalTransferID": 546546,
"DT":"2018-11-23T11:59:12.493",
"Type":0,
"Amount":254.56,
"CommentClient":"Платеж по карте",
"CommentCompany":"",
"Test":0
},
{
"ID":221,
"WalletID":222,
"ExternalTransferID": 546547,
"DT":"2018-11-23T11:59:15.000",
"Type":0,
"Amount":254.56,
"CommentClient":"Платеж по карте",
"CommentCompany":"",
"Test":0
}
]
}

## walletTransfers.get
Информация по конкретному переводу.

URL вызова: https://ramm.store/api/manager/v1/walletTransfers.get

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID| number|	ID перевода|


Возвращаемые данные - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID перевода|
WalletID|	number|	ID кошелька|
ExternalTransferID|	number|	ID внешнего перевода|
DT|	number|	Дата перевода|
Type|	number|	0-fund, 1-withdraw, 2-bonus fund, 3-bonus withdraw, 4-to account, 5-from account, 6-fee, 7-commission, 8-partners|
Amount|	real|	Сумма перевода|
CommentClient|	string|	Комментарий для клиента|
CommentCompany|	string|	Внутренний комментарий|


Пример вызова:

{
"ID":223322
}

Пример ответа:

{
"ID":223322,
"IDWallet":222,
"ExternalTransferID": 546546,
"DT":"2018-11-23T11:59:12.493",
"Type":0,
"Amount":254.56,
"CommentClient":"Платеж по карте",
"CommentCompany":""
}


## walletTransfers.getRewards
Возвращает информацию о начислениях фи и комиссий по всем счетам с заданного сервера MT с ID больше, чем указанный в заголовке ID

URL вызова: https://ramm.store/api/manager/v1/walletTransfers.getRewards

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ExecutionServerName|	string|	Имя сервера MT|
ID|	number|	ID последней проведенной операции|


Возвращаемые данные - массив Transactions, каждый элемент которого содержит поля:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Transactions|	ExecutionAccount|	number|	ID счета в MT|
-| ID|	number|	ID операции|
-| Amount|	real|	Сумма перевода|


Пример вызова:

{
"ExecutionServerName":"Server1",
"ID":146
}

Пример ответа:

{
    "Transactions": [
        {
            "ExecutionAccount": "50000059",
            "ID": 150,
            "Amount": 16.81
        },
        {
            "ExecutionAccount": "50000059",
            "ID": 153,
            "Amount": 16.81
        },
        {
            "ExecutionAccount": "50000059",
            "ID": 171,
            "Amount": 16.81
        },
        {
            "ExecutionAccount": "50000059",
            "ID": 187,
            "Amount": 16.81
        }
    ]
}

## Операции со стратегиями RAMM

## strategies.search
Поиск стратегий по параметрам.

URL вызова: https://ramm.store/api/manager/v1/strategies.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	WalletID|	number|	ID кошелька (bigint)|
-| ClientID|	number|	ID клиента (трейдера) (bigint)|
-| Name|	string|	Подстрока поиска имени (Varchar(64))|
-| DTClosedFrom|	number|	Начальная дата закрытия стратегии|
-| DTClosedTo|	number|	Конечная дата закрытия стратегии|
-| DTCreatedFrom|	number|	Начальная дата создания стратегии|
-| DTCreatedTo|	number|	Конечная дата создания стратегии|
-| YieldFrom|	real|	Прибыль в %, начало диапазона|
-| YieldTo|	real|	Прибыль в %, конец диапазона|
-| Status|	number|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed|
-| Symbols|	string|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)|
-| TraderLogin|	string|	Логин трейдера|
-| Value|	string|	Значение поиска|
-| ExternalAccount|	string|	Номер внешнего счета|
-| Closed|	boolean|	Признак закрытой стратегии|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, ClientID, WalletID, Name, DTCreated, DTClosed, Yield, Accounts, Status, Symbols, UnitPrice, Leverage, MonthlyYield, TraderLogin|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Strategies, каждый элемент которого содержит поля:
Структура, 1 уровень| Структура, 2 уровень| Параметр| Тип| Описание
---------|----------|----------|----------|----------
Filter| -|Closed|	boolean|	Признак закрытой стратегии|
-|-| ClientID|	number|	ID клиента (трейдера) (bigint)|
-|-| WalletID|	number|	ID кошелька (bigint)|
-|-| Name|	string|	Подстрока поиска (Varchar(64))|
-|-| DTClosedFrom|	number|	Начальная дата закрытия стратегии|
-|-| DTClosedTo|	number|	Конечная дата закрытия стратегии|
-|-| DTCreatedFrom|	number|	Начальная дата создания стратегии|
-|-| DTCreatedTo|	number|	Конечная дата создания стратегии|
-|-| YieldFrom|	real|	Прибыль в %, начало диапазона|
-|-| YieldTo|	real|	Прибыль в %, конец диапазона|
-|-| Status|	number|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed|
-|-| Symbols|	string|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)|
-|-| TraderLogin|	string|	Логин трейдера|
-|-| Value|	string|	Значение поиска|
OrderBy| -| Field|	string|	Сортировка по параметру, варианты: ID, ClientID, WalletID, Name, DTCreated, DTClosed, Yield, Accounts, Status, Symbols, UnitPrice, Leverage, MonthlyYield, TraderLogin|
-| -| Direction|	string|	Направление сортировки, варианты: Asc, Desc|
Pagination|	-| TotalRecords|	number|	Общее количество записей|
-| -| TotalPages|	number|	Общее количество страниц|
-| -| CurrentPage|	number|	Номер текущей страницы|
-| -| PerPage|	number|	Количество записей на одной странице|
-| -| MaxPerPage|	number|	Максимальное количество записей на одной странице|
Strategies| -| ID|	number|	ID стратегии|
-| -| ClientID|	number|	ID клиента (трейдера) (bigint)|
-| -| WalletID|	number|	ID кошелька (bigint)|
-| -| TradingCoreID|	number|	ID торгового ядра|
-| -| Name|	string|	Название стратегии Varchar(64)|
-| -| DTCreated|	number|	Дата создания стратегии|
-| -| DTClosed|	number|	Дата закрытия стратегии (если она закрыта)|
-| -| DTStat|	number|	Дата начала сбора статистики|
-| -| DTStamp|	number|	Текущая дата|
-| -| Commission|	real|	Размер комиссии (numeric (6,6))|
-| -| Fee|	real|	Вознаграждение с прибыли (numeric (3,2))|
-| -| PartnerShare|	real|	Доля партнера|
-| -| Yield|	real|	Прибыль в %|
-| -| Accounts|	number|	Количество счетов|
-| -| Status|	number|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed|
-| -| Symbols|	string|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)|
-| -| UnitPrice|	real|	Цена пая|
-| -| Leverage|	real|	Фактическое плечо|
-| -| MonthlyYield|	real|	Среднемесячная доходность|
-| -| TraderLogin|	string|	Логин трейдера|
-| -| ExternalServer|	string|	Название внешнего сервера|
-| -| ExternalAccount|	string|	Номер внешнего счета|
-| Chart|	Yield|	real|	Значение доходности|

Ограничения и типы:

([Commission]>=(0) AND [Commission]<=(1)) 6 знаков после запятой

([Fee]>=(0) AND [Fee]<=(1)) 2 знака после запятой

([PartnerShare]>=(0) AND [PartnerShare]<=(1)) 3 знака после запятой


Пример вызова:

{
"Filter":
{
"Closed":true,
"ClientID":222,
"Name":"SuperProfit"
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
"Closed":true,
"ClientID":222,
"Name":"SuperProfit"
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Pagination":
{
"TotalRecords": 1,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 5,
"MaxPerPage": 100
},
"Strategies":
[
{
"ID":7986,
"ClientID": 222,
"WalletID":4565465,
"TradingCoreID":1,
"Name":"SuperProfit USD 25",
"DTCreated": "2018-09-21T11:09:38.243",
"DTClosed":"2018-09-23T11:09:39.243",
"DTStat":"2017-09-23T11:09:39.243",
"DTStamp":"2018-09-22T11:09:38.243",
"Commission":0.00001,
"Fee":0.25,
"Yield":1.076,
"Accounts":5,
"Status":2,
"Symbols":"EURUSD",
"UnitPrice":115.45,
"Leverage":14.2,
"MonthlyYield":25.5,
"TraderLogin":"Trader2",
"ExternalServer":"MT5-FXBroker-real",
"ExternalAccount":"100000555",
"Chart":
[
{
"Yield":0.0000000000
},
{
"Yield":1.1000000000
}
]
}
]
}


## strategies.get
Информация о заданной стратегии.

URL вызова: https://ramm.store/api/manager/v1/strategies.get

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID стратегии|


Возвращаемые данные - строка JSON, содержит указанные параметры и массив Chart с данными графика:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
-| ID|	number|	ID стратегии|
-| CompanyID|	number|	ID компании|
-| TraderID|	number|	ID клиента (трейдера) (bigint)|
-| WalletID|	number|	ID кошелька (bigint)|
-| TradingCoreID|	number|	ID торгового ядра|
-| Name|	string|	Название стратегии (Varchar(64))|
-| Demo|	boolean|	Признак "демо"|
-| DTClosed|	number|	Дата закрытия стратегии|
-| Commission|	real|	Размер комиссии (numeric (6,6))|
-| Fee|	real|	Вознаграждение с прибыли (numeric (3,2))|
-| PartnerShare|	real|	Доля партнера|
-| DTCreated|	number|	Дата создания стратегии|
-| DTStat|	number|	Дата начала сбора статистики|
-| DTStamp|	number|	Текущая дата|
-| Yield|	real|	Прибыль в %|
-| Accounts|	number|	Количество счетов|
-| Status|	number|	0-not activated, 1-active, 2-paused, 3-disabled, 4-closed|
-| Symbols|	string|	Строка с перечислением самых используемых торговых инструментов (не более 3-х)|
-| UnitPrice|	real|	Цена пая|
-| Leverage|	real|	Фактическое плечо|
-| MonthlyYield|	real|	Среднемесячная доходность|
-| Equity|	real|	Сумма средств всех инвестиций стратегии|
-| TraderLogin|	string|	Логин трейдера|
-| TraderLastName|	string|	Фамилия трейдера|
-| TraderFirstName|	string|	Имя трейдера|
-| ExternalServer|	string|	Название внешнего сервера|
-| ExternalAccount|	string|	Номер внешнего счета|
-| CompanyName|	string|	Название компании|
Chart|	Yield|	real|	Значение доходности|
-| DT|	number|	Дата значения|

Ограничения и типы:

([Commission]>=(0) AND [Commission]<=(1)) 6 знаков после запятой

([Fee]>=(0) AND [Fee]<=(1)) 2 знака после запятой

([PartnerShare]>=(0) AND [PartnerShare]<=(1)) 3 знака после запятой


Пример вызова:

{
"ID":7986
}

Пример ответа:

{
"ID":7986,
"CompanyID":1,
"TraderID": 222,
"WalletID":4565465,
"TradingCoreID":1,
"Name":"SuperProfit USD 25",
"Demo":true,
"DTClosed":"2018-11-23T11:59:12.493",
"Commission":0.00001,
"Fee":0.25,
"PartnerShare":0,
"DTCreated":"2018-05-23T11:59:12.493",
"DTStat":"2017-05-23T11:59:12.493",
"DTStamp":"2018-09-22T11:09:38.243",
"Yield":1.076,
"Accounts":5,
"Status":2,
"Symbols":"EURUSD",
"UnitPrice":115.45,
"Leverage":14.2,
"MonthlyYield":25.5,
"Equity":1500.00
"TraderLogin":"Trader2",
"TraderLastName":"John",
"TraderFirstName":"Doe",
"ExternalServer":"TradeServer1",
"ExternalAccount":"223322",
"CompanyName":"FXTrade",
"Chart":
[
{
"Yield":0.0000000000,
"DT":"2018-05-23T11:59:12.493"
},
{
"Yield":0.0000000000,
"DT":"2018-05-23T11:59:12.493"
}
]
}

## strategies.add
Создает новую стратегию.

URL вызова: https://ramm.store/api/manager/v1/strategies.add

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
WalletID|	number|	ID кошелька (bigint)|
ClientID|	number|	ID клиента (трейдера) (bigint)|
AccountSpecAssetID|	number|	Спецификация счета для заданного актива|
Name|	string|	Название стратегии (Varchar(50))|
FeeRate|	real|	Вознаграждение с прибыли (numeric (3,2))|
Protection|	real|	Процент защиты счета (numeric (4,3))|
Target|	real|	Целевая доходность (numeric (8,3))|
Money|	real|	Сумма (numeric (28,2))|
CommissionRate|	real| Комиссия трейдера (целое число, USD / млн. USD оборота)|
ExternalServer|	string|	Название внешнего сервера (Varchar(50))|
ExternalAccount|	string|	ID внешнего счета (Varchar(20))|
ExecutionTags|	string|	JSON-строка с необязательными параметрами исполнения. Кавычки нужно экранировать как \"|

Возвращаемые данные - строка JSON, содержит параметры:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Strategy|	ID|	number|	ID стратегии|

Ограничения и типы:

([CommissionRate]>=(0) ) целое число

([FeeRate]>=(0) AND [FeeRate]<=(1)) 2 знака после запятой

([PartnerShare]>=(0) AND [PartnerShare]<=(1)) 3 знака после запятой

[Protection]>=(0) AND [Protection]<(1) имеет 3 знака после запятой

[ProtectionEquity]>=(0)

[Target]>(0) OR [Target] IS NULL имеет 3 знака после запятой

[TargetEquity]>(0)


Пример вызова:

{
"WalletID":4565465,
"ClientID":7986,
"AccountSpecAssetID":1,
"Name":"SuperProfit USD 25",
"FeeRate": 0.25,
"Protection":0.5,
"Target":1.5,
"Money":1000,
"CommissionRate":0.0001,
"ExternalServer":"FXTrade MT5 Server",
"ExternalAccount": "10000005454"
}

Пример ответа:

{
"Strategy":
{
"ID":7986
}
}

## strategies.set
Изменение названия стратегии по заданному ID стратегии

URL вызова: https://ramm.store/api/manager/v1/strategies.set

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	Название компании|
Name|	string|	Название стратегии|

Возвращаемые данные: отсутствуют.

Пример запроса:
{ 
"ID":4565,
"Name":"SuperProfit45"
}

## strategies.close
Закрывает стратегию с заданным ID

URL вызова: https://ramm.store/api/manager/v1/strategies.close

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID стратегии|

Возвращаемые данные: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
CommandID|	number|	ID команды закрытия стратегии|

Пример вызова:

{
"ID":1111
}

Пример ответа:

{
"CommandID":2222
}

## strategysymbolstat.get
Отдает информацию по статистике использования различных торговых инструментов стратегией.

URL вызова: https://ramm.store/api/manager/v1/strategysymbolstat.get

Тело запроса - строка JSON, содержит параметр:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID стратегии|

Возвращаемые данные: строка JSON, содержит массив StrategySymbolStat
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
StrategySymbolStat|	Symbol|	string|	Название символа|
-| Share|	real|	Доля символа|


Пример вызова:

{
"ID":1111,
}


Пример ответа:

{
"StrategySymbolStat":
[
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

## Операции с торговыми счетами клиентов

## accounts.search
Поиск счета по заданным параметрам.

URL вызова: https://ramm.store/api/manager/v1/accounts.search

Тело запроса - строка JSON, содержит структуры Filter (в том числе массив Statuses), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура, 1 уровень |	Структура, 2 уровень | Параметр | Тип | Описание
---------|----------|----------|----------|----------
Filter| -| StrategyID|	number|	ID стратегии|
-| -| ClientID|	number|	ID клиента|
-| -| DTCreatedFrom|	number|	Дата создания, начало диапазона|
-| -| DTCreatedTo|	number|	Дата создания, конец диапазона|
-| -| DTClosedFrom|	number|	Дата закрытия, начало диапазона|
-| -| DTClosedTo|	number|	Дата закрытия, конец диапазона|
-| -| Type|	number|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account|
-| -| State|	number|	См. ниже|
-| -| ID|	number|	ID счета|
-| -| StreamID|	number|	ID потока исполнения|
-| -| Test|	number|	Признак тестового счета (0,1, 2 - all)|
-| Statuses|	-| number|	См. ниже|
Pagination|	-| CurrentPage|	number|	Номер текущей страницы|
-| -| PerPage|	number|	Количество записей на одной странице|
OrderBy|	-| Field|	string|	Сортировка по параметру, варианты: ID, ClientID, WalletID, StrategyID, TradingCoreID, AccountSpecAssetID, LiquidityID, StreamID, StreamName, PartnerID, CommandCloseID, TradingIntervalCurrentID, IsSecurity, DTCreated, DTClosed, Type, ABook, Balance, Factor, Equity, ProfitBase, Bonus, Margin, Status, MCReached, Protection, ProtectionEquity, ProtectionReached, Target, TargetEquity, TargetReached, AssetName, Precision, State, Test|
-| -| Direction|	string|	Направление сортировки, варианты: Asc, Desc|

Возвращаемые данные - структуры Filter (в т.ч. массив Statuses), OrderBy, Pagination и массив Accounts:
Структура, 1 уровень |	Структура, 2 уровень | Параметр | Тип | Описание
---------|----------|----------|----------|----------
Filter| -|	StrategyID|	number|	ID стратегии|
-| -| ClientID|	number|	ID клиента|
-| -| DTCreatedFrom|	number|	Дата создания, начало диапазона|
-| -| DTCreatedTo|	number|	Дата создания, конец диапазона|
-| -| DTClosedFrom|	number|	Дата закрытия, начало диапазона|
-| -| DTClosedTo|	number|	Дата закрытия, конец диапазона|
-| -| Type|	number|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account|
-| -| State|	number|	См. ниже|
-| -| ID|	number|	ID счета|
-| -| StreamID|	number|	ID потока исполнения|
-| -| Test|	number|	Признак тестового счета (0,1, 2 - all)|
-| Statuses|  -|	number| 	См. ниже|
OrderBy|  -|	Field|	string|	Сортировка по параметру, варианты: ID, ClientID, WalletID, StrategyID, TradingCoreID, AccountSpecAssetID, LiquidityID, StreamID, StreamName, PartnerID, CommandCloseID, TradingIntervalCurrentID, IsSecurity, DTCreated, DTClosed, Type, ABook, Balance, Factor, Equity, ProfitBase, Bonus, Margin, Status, MCReached, Protection, ProtectionEquity, ProtectionReached, Target, TargetEquity, TargetReached, AssetName, Precision, State|
-| -| Direction|	string|	Направление сортировки, варианты: Asc, Desc|
Pagination| -| TotalRecords|	number|	Общее количество записей|
-| -| TotalPages|	number|	Общее количество страниц|
-| -| CurrentPage|	number|	Номер текущей страницы|
-| -| PerPage|	number|	Количество записей на одной странице|
-| -| MaxPerPage|	number|	Максимальное количество записей на одной странице|
Accounts|  -|	ID|	number|	ID счета|
-| -| StrategyID|	number|	ID стратегии|
-| -| ClientID|	number|	ID клиента (bigint)|
-| -| WalletID|	number|	ID кошелька (bigint)|
-| -| TradingCoreID|	number|	ID торгового ядра|
-| -| DTCreated|	number|	Дата создания|
-| -| DTClosed|	number|	Дата закрытия|
-| -| AssetName|	string|	Название валюты счета|
-| -| AccountSpecAssetID|	number|	Спецификация счета для заданного актива|
-| -| LiquidityID|	number|	ID ликвидности|
-| -| StreamID|	number|	ID потока котировок|
-| -| StreamName|	string|	Название стрима|
-| -| PartnerID|	number|	ID партнера|
-| -| CommandCloseID|	number|	ID команды закрытия счета|
-| -| TradingIntervalCurrentID|	number|	ID текущего торгового интервала|
-| -| Type|	number|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account|
-| -| ABook|	real|	Доля A-Book (numeric (3,2))|
-| -| Balance|	real|	Баланс счета|
-| -| Factor|	real|	Повышающий/понижающий коэффициент копирования|
-| -| Equity|	real|	Эквити|
-| -| ProfitBase|	real|	База для подсчета вознаграждения|
-| -| Bonus|	real|	Бонус|
-| -| Margin|	real|	Задействованная маржа|
-| -| Status|	number|	См. ниже|
-| -| MCReached|	number|	Дата/время срабатывания StopOut|
-| -| Protection|	real|	Процент защиты счета (numeric (4,3))|
-| -| ProtectionEquity|	real|	Значение эквити, при котором сработает защита счета|
-| -| ProtectionReached|	number|	Дата/время срабатывания защиты счета|
-| -| Target|	real|	Целевая доходность (numeric (8,3))|
-| -| TargetEquity|	real|	Целевая доходность в валюте счета|
-| -| TargetReached|	number|	Дата/время достижения целевой доходности|
-| -| Precision|	number|	Точность округления, знаки после запятой|
-| -| IsSecurity|	number|	Признак собственной счета трейдера|
-| -| Test|	number|	Признак тестового счета (0,1, 2 - all)|

Значения поля "Status"
Значение | Расшифровка
---------|----------
0|	new (without money)|
1|	active (trading)|
2|	MC|
3|	ProtectionTarget|
4|	pause|
5|	disabled (cant trade)|
6|	closed (cant activate)|

Значения поля "State"
Значение | Расшифровка
---------|----------
0|	new|
1|	(not used)|
2|	active, no positions|
3|	active, with positions|
4|	margin call, with positions|
5|	margin call, no positions|
6|	protection, with positions|
7|	protection, no positions|
8|	target, with positions|
9|	target, no positions|
10|	pause, with positions|
11|	pause, no positions|
12|	disabled, with positions|
13|	disabled, no positions|
14|	closed, with positions|
15|	closed, no positions|

Ограничения и типы

[ABook]>=(0) AND [ABook]<=(1) имеет 2 знака после запятой

[Bonus]>=(0)

[Factor]>=(-1000) AND [Factor]<=(-0.01) OR [Factor]>=(0.01) AND [Factor]<=(1000)

[IsSecurity]=(0) OR [Factor]=(1)

[Margin]>=(0)

[Protection]>=(0) AND [Protection]<(1) имеет 3 знака после запятой

[ProtectionEquity]>=(0)

[Target]>(0) OR [Target] IS NULL имеет 3 знака после запятой

[TargetEquity]>(0)

 

Пример вызова:

{
"Filter":
{
"StrategyID":222,
"ClientID":4545,
"Statuses": [0,1]
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
"StrategyID":222,
"ClientID":4545,
"Statuses": [0,1]
},
"Pagination":
{
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 5,
"MaxPerPage": 100,
"OrderByField": "ID",
"OrderDirection": "Asc"
},
"Accounts":
[
{
"ID":111,
"StrategyID":222,
"ClientID":454,
"WalletID":4545,
"TradingCoreID":1,
"DTCreated":"2018-11-23T11:59:12.493",
"DTClosed":"2018-11-23T11:59:12.493",
"AssetName":"USD",
"AccountSpecAssetID":5,
"LiquidityID":5,
"StreamID":5,
"StreamName":"StreamA"
"PartnerID":454,
"CommandCloseID":6852,
"TradingIntervalCurrentID":180,
"Type":0,
"ABook":1.00,
"Balance":500.00,
"Factor":1.00,
"Equity":500.00,
"ProfitBase":500.00,
"Bonus":0,
"Margin":0,
"Status":1,
"MCReached":"2018-11-23T11:59:12.493",
"Protection":0.5,
"ProtectionEquity":250,
"ProtectionReached":"2018-11-23T11:59:12.493",
"Target":1.00,
"TargetEquity":1000.00,
"TargetReached":"2018-11-23T11:59:12.493",
"Precision":2,
"IsSecurity":1,
"Test":0
}
]
}


## accounts.get
Информация о заданном счете

URL вызова: https://ramm.store/api/manager/v1/accounts.get

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID счета|


Возвращаемые данные - строка JSON, содержит параметры:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
-| ID|	number|	ID счета|
-| StrategyID|	number|	ID стратегии|
-| CompanyID|	number|	ID компании|
-| ClientID|	number|	ID клиента (bigint)|
-| WalletID|	number|	ID кошелька (bigint)|
-| TradingCoreID|	number|	ID торгового ядра|
-| DT|	number|	Дата создания|
-| DTClosed|	number|	Дата закрытия|
-| AssetName|	string|	Название валюты счета|
-| AccountSpecAssetID|	number|	Спецификация счета для заданного актива|
-| LiquidityID|	number|	ID ликвидности|
-| StreamID|	number|	ID потока котировок|
-| PartnerID|	number|	ID партнера|
-| CommandCloseID|	number|	ID команды закрытия счета|
-| TradingIntervalCurrentID|	number|	ID текущего торгового интервала|
-| Type|	number|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account|
-| ABook|	real|	Доля A-Book (numeric (3,2))|
-| Balance|	real|	Баланс счета|
-| Factor|	real|	Повышающий/понижающий коэффициент копирования|
-| Equity|	real|	Эквити|
-| ProfitBase|	real|	База для подсчета вознаграждения|
-| Bonus|	real|	Бонус|
-| Margin|	real|	Задействованная маржа|
-| Status|	number|	См. ниже|
-| MCReached|	number|	Дата/время срабатывания StopOut|
-| Protection|	real|	Процент защиты счета (numeric (4,3))|
-| ProtectionEquity|	real|	Значение эквити, при котором сработает защита счета|
-| ProtectionReached|	number|	Дата/время срабатывания защиты счета|
-| Target|	real|	Целевая доходность (numeric (8,3))|
-| TargetEquity|	real|	Целевая доходность в валюте счета|
-| TargetReached|	number|	Дата/время достижения целевой доходности|
-| Precision|	number|	Точность округления, знаки после запятой|
-| IsSecurity|	number|	Признак собственного счета трейдера|
-| SuccessFee|	real|	Вознаграждение с прибыли|
-| FeeTotal|	real|	Общая сумма вознаграждений|
-| AccountMinBalance|	real|	Минимальный баланс счета|
-| AvailableToWithdraw|	real|	Сумма, доступная к выводу|
-| State|	number|	См. ниже|
-| ProfitCurrentIntervalGross|	real|	"Грязная" прибыль за текущий интервал|
-| ProfitCurrentIntervalNet|	real|	Чистая прибыль за текущий интервал|
-| ProfitPositions|	real|	Прибыль по открытым позициям|
-| FeeRate|	real|	Ставка вознаграждения с прибыли|
-| DrawdownIn|	real|	Входящий уровень просадки|
-| PositionsCount|	number|	Количество позиций|
AvailableStreams|	StreamID|	number|	ID стрима|
-| Name|	string|	Имя стрима|

Значения поля "Status"
Значение | Расшифровка
---------|----------
0|	new (without money)|
1|	active (trading)|
2|	MC|
3|	ProtectionTarget|
4|	pause|
5|	disabled (can't trade)|
6|	closed (can't activate)|

Значения поля "State"
Значение | Расшифровка
---------|----------
0|	new|
1|	(not used)|
2|	active, no positions|
3|	active, with positions|
4|	margin call, with positions|
5|	margin call, no positions|
6|	protection, with positions|
7|	protection, no positions|
8|	target, with positions|
9|	target, no positions|
10|	pause, with positions|
11|	pause, no positions|
12|	disabled, with positions|
13|	disabled, no positions|
14|	closed, with positions|
15|	closed, no positions|

Ограничения и типы

[ABook]>=(0) AND [ABook]<=(1) имеет 2 знака после запятой

[Bonus]>=(0)

[Factor]>=(-1000) AND [Factor]<=(-0.01) OR [Factor]>=(0.01) AND [Factor]<=(1000)

[IsSecurity]=(0) OR [Factor]=(1)

[Margin]>=(0)

[Protection]>=(0) AND [Protection]<(1) имеет 3 знака после запятой

[ProtectionEquity]>=(0)

[Target]>(0) OR [Target] IS NULL имеет 3 знака после запятой

[TargetEquity]>(0)



Пример вызова:

{
"ID":333
}

Пример ответа:

{
"ID":111,
"StrategyID":222,
"CompanyID":1,
"ClientID":454,
"WalletID":4545,
"TradingCoreID":1,
"DT":"2018-11-23T11:59:12.493",
"DTClosed":"2018-11-23T11:59:12.493",
"AssetName":"USD",
"AccountSpecAssetID":5,
"LiquidityID":5,
"StreamID":5,
"PartnerID":454,
"CommandCloseID":6852,
"TradingIntervalCurrentID":180,
"Type":0,
"ABook":1.00,
"Balance":500.00,
"Factor":1.00,
"Equity":500.00,
"ProfitBase":500.00,
"Bonus":0,
"Margin":0,
"Status":1,
"MCReached":"2018-11-23T11:59:12.493",
"Protection":0.5,
"ProtectionEquity":250,
"ProtectionReached":"2018-11-23T11:59:12.493",
"Target":1.00,
"TargetEquity":1000.00,
"TargetReached":"2018-11-23T11:59:12.493",
"Precision":2,
"IsSecurity":1,
"SuccessFee":155.25,
"FeeTotal":155.25,
"AccountMinBalance":200,
"AvailableToWithdraw":300,
"State":3,
"ProfitCurrentIntervalGross":121.23,
"ProfitCurrentIntervalNet":121.23,
"ProfitPositions":2,
"FeeRate":0.25,
"DrawdownIn":0,
"PositionsCount":2,
"AvailableStreams":
[
{
"StreamID":2,
"Name":"StreamA"
},
{
"StreamID":5,
"Name":"StreamB"
}
]
}

## accounts.set
Устанавливает новую долю A-Book по счету.

URL вызова: https://ramm.store/api/manager/v1/accounts.set

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID счета|
ABook|	real|	Доля A-Book (numeric (3,2))|
StreamID|	number|	ID стрима|

Возвращаемые данные: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ABookCommandID|	number|	ID команды изменения доли A-Book|
CommandStreamID|	number|	ID команды изменения стрима|

Ограничения

[ABook]>=(0) AND [ABook]<=(1) имеет 2 знака после запятой

Пример запроса:
{ 
"ID":4565,
"ABook":0.5,
"StreamID":5
}

Пример ответа:
{ 
"ABookCommandID":6899,
"CommandStreamID":7222
}

## accounts.close
Закрывает счет с указанным ID.

URL вызова: https://ramm.store/api/manager/v1/accounts.close

Тело запроса: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID счета|

Возвращаемые данные: строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
CommandID|	number|	ID команды закрытия счета|

Пример запроса:
{ 
"ID":4565
}

Пример ответа:
{ 
"CommandID":7222
}

## accounts.getStatement
Получение данных для построения стейтмента

URL вызова: https://ramm.store/api/manager/v1/accountStatements.get

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID счета|


Возвращаемые данные - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
IDStrategy|	number|	ID стратегии|
StrategyName|	string|	Название стратегии|
Commission|	real|	Комиссия|
Fee|	real|	Вознаграждение|
IDAccount|	number|	ID счета|
DT|	number|	Дата/время|
ABook|	real|	доля ABook|
Type|	number|	См. ниже|
DTStamp|	number|	Дата/время снятия статистики|
AccountAssetName|	string|	Название валюты счета|
LeverageMax|	number|	Максимальное плечо|
MCLevel|	number|	Уровень маржинколла|
State|	number|	см.ниже|
Factor|	real|	Значение фактора|
Target|	real|	Цель|
Protection|	real|	Защита|
TargetEquity|	real|	Целевой уровень эквити|
ProtectionEquity|	real|	Уровень срабатывания защиты|

Значения поля "Type"
Значение | Расшифровка
---------|----------
0|	real security|
1| virtual master|
2|	real internal ramm account|
3|	real external account|

Значения поля "State"
Значение | Расшифровка
---------|----------
0|	new|
1|	(not used)|
2|	active, no positions|
3|	active, with positions|
4|	margin call, with positions|
5|	margin call, no positions|
6|	protection, with positions|
7|	protection, no positions|
8|	target, with positions|
9|	target, no positions|
10|	pause, with positions|
11|	pause, no positions|
12|	disabled, with positions|
13|	disabled, no positions|
14|	closed, with positions|
15|	closed, no positions|


Пример вызова:

{
"ID":333
}

Пример ответа:

{
"IDStrategy":341,
"StrategyName":"TEST_1",
"Commission":0.000010,
"Fee":0.25,
"IDAccount":333,
"DT":"2018-09-21T11:09:38.243",
"ABook":1.00,
"Type":0,
"DTStamp":"2019-05-14T17:16:00.490",
"AccountAssetName":"USD",
"LeverageMax":100,
"MCLevel":20,
"State":8,
"Factor":1.00,
"Target":0.010,
"Protection":0.010,
"TargetEquity":1010.0000000000,
"ProtectionEquity":10.0000000000
}

## accounts.pause
Ставит счет на паузу (временное прекращение копирования с закрытием всех открытых позиций)
ВНИМАНИЕ. Счет трейдера этим методом поставить на паузу нельзя!

URL вызова: https://ramm.store/api/manager/v1/accounts.pause

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID счета|

Возвращаемые данные - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
CommandID|	number|	ID команды постановки на паузу|


Пример вызова:

{
"ID": 445
}

Пример ответа:

{
"CommandID": 5654
}

## accounts.resume
Снимает счет с паузы (возобновление копирования, открытие всех позиций стратегии по текущим ценам).

ВНИМАНИЕ. Счет трейдера этим методом снять с паузы нельзя!

URL вызова: https://ramm.store/api/manager/v1/accounts.resume

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID счета|

Возвращаемые данные - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
CommandID|	number|	ID команды снятия с паузы|


Пример вызова:

{
"ID": 445
}

Пример ответа:

{
"CommandID": 5654
}

## Операции со сделками на счете

## deals.search
Поиск сделок по критериям фильтрации. Критерии могут быть не заданы или заданы частично.

URL вызова: https://ramm.store/api/manager/v1/deals.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	AccountID|	number|	ID счета|
-| SignalID|	number|	ID сигнала|
-| SOID|	number|	ID StopOut|
-| CommandID|	number|	ID команды|
-| StrategyID|	number|	ID стратегии|
-| Symbol|	string|	Название инструмента|
-| Type|	number|	См. ниже|
-| AccountType|	number|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account|
-| ID|	number|	ID сделки|
-| DTFrom|	number|	Начальная дата|
-| DTTo|	number|	Конечная дата|
-| ClientID|	number|	ID клиента|
-| WalletID|	number|	ID кошелька|
-| Test|	number|	Признак сделок с тестового счета (0,1, 2 - all)|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Параметр сортировки, варианты: ID, StrategyID, AccountID, AccountType, SignalID, CommandID, SOID, SymbolSpecID, StreamID, FillRequestID, ClientID, WalletID, DT, Type, Symbol, Volume, VolumeA, Price, Commission, Entry, Profit, Swap, TotalProfit, DealToID, Netting, ClientLogin, ClientEmail, Test, TurnoverUSD|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Deals, каждый элемент которого соответствует сделке:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	AccountID|	number|	ID счета|
-| SignalID|	number|	ID сигнала|
-| SOID|	number|	ID StopOut|
-| CommandID|	number|	ID команды|
-| StrategyID|	number|	ID стратегии|
-| Symbol|	string|	Название инструмента|
-| Type|	number|	См. ниже|
-| DTFrom|	number|	Начальная дата|
-| DTTo|	number|	Конечная дата|
-| ClientID|	number|	ID клиента|
-| WalletID|	number|	ID кошелька|
-| ID	number|	ID| сделки|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
-| MaxPerPage|	number|	Максимальное количество записей на одной странице|
OrderBy|	Field|	string|	Параметр сортировки, варианты: ID, StrategyID, AccountID, AccountType, SignalID, CommandID, SOID, SymbolSpecID, StreamID, FillRequestID, ClientID, WalletID, DT, Type, Symbol, Volume, VolumeA, Price, Commission, Entry, Profit, Swap, TotalProfit, DealToID, Netting, ClientLogin, ClientEmail, Test|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|
Deals|	ID|	number|	ID сделки|
-| StrategyID|	number|	ID стратегии|
-| AccountID|	number|	ID счета|
-| AccountType|	number|	0-real security, 1-virtual master, 2-real internal ramm account, 3-real external account|
-| SignalID|	number|	ID сигнала|
-| CommandID|	number|	ID команды|
-| SOID|	number|	ID StopOut|
-| SymbolSpecID|	number|	ID спецификации инструментов|
-| StreamID|	number|	ID потока котировок|
-| FillRequestID|	number|	ID запроса на открытие позиции|
-| ClientID|	number|	ID клиента|
-| WalletID|	number|	ID кошелька|
-| DT|	number|	Дата/время создания сделки|
-| Type|	number|	См.ниже|
-| Symbol|	string|	Название инструмента|
-| Volume|	real|	Суммарный объем|
-| VolumeA|	real|	В том числе, объем A-Book|
-| Price|	real|	Средняя цена|
-| Commission|	real|	Суммарная комиссия|
-| Entry|	number|	0 - In, 1 - Out, 2 - InOut|
-| Profit|	real|	Прибыль/убыток по сделке (без учета комиссий и свопов)|
-| Swap|	real|	Своп|
-| TotalProfit|	real|	Прибыль/убыток по сделке (с учетом комиссий и свопов)|
-| DealToID|	number|	ID результирующей сделки|
-| Netting|	number|	признак неттинговой сделки|
-| PrecisionPrice|	number|	Количество знаков после запятой при выводе цены|
-| PrecisionVolume|	number|	Количество знаков после запятой при выводе объема|
-| ClientLogin|	string|	Логин клиента|
-| ClientEmail|	string|	E-mail клиента|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
-| TurnoverUSD|	real|	Сгенерированный дилом оборот в USD|
-| StrategyTags|	string|	Постфиксы и прочие инструкции исполнения|

Значения поля "Type"
Значение | Расшифровка
---------|----------
0|	Buy|
1|	Sell|
2|	Balance|
3|	Credit|
4|	Additional charge|
5|	Correction|
6|	Bonus|
7|	Fee|
8|	Dividend|
9|	Interest|
10|	Canceled/Rejected buy deal|
11|	Canceled/Rejected sell deal|
12|	Periodical commission|
13|	Zero total  order|


Пример вызова:

{
"Filter":
{
"DTFrom": "2018-12-25T17:19:16.547",
"SignalID": 7326
},
"OrderBy":
{
"Field": "DT",
"Direction": "Asc"
},
"Pagination":
{
"CurrentPage": 1,
"PerPage": 50
}
}

Пример ответа:

{
"Filter":
{
"SignalID": 7326,
"DTFrom": "2018-12-25T17:19:16.547"
},
"OrderBy":
{
"Field": "DT",
"Direction": "Asc"
},
"Pagination":
{
"TotalRecords": 1,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 50,
"MaxPerPage": 100
},
"Deals":
[
{
"ID":4546,
"StrategyID":2233,
"AccountID":545,
"AccountType":0,
"SignalID":4878,
"CommandID":4554,
"SOID":7821,
"SymbolSpecID":1,
"StreamID":1,
"FillRequestID":54546,
"ClientID":222,
"WalletID":333,
"DT":"2018-11-23T11:59:12.493",
"Type":0,
"Symbol":"EURUSD",
"Volume":0.15,
"VolumeA":0.1,
"Price":1.1254,
"Commission":0.06,
"Entry":0,
"Profit":1.23,
"Swap":-0.03,
"TotalProfit":1.2,
"DealToID":4455,
"Netting":1,
"PrecisionPrice":2,
"PrecisionVolume":4,
"ClientLogin":"TraderA",
"ClientEmail":"trader1@gmail.com",
"Test":0,
"TurnoverUSD": 16783.02,
"StrategyTags": "Postfix: c"
}
]
}

## deals.get
Информация о заданной сделке.

URL вызова: https://ramm.store/api/manager/v1/deals.get

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID сделки|


Возвращаемые данные - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
DealID|	number|	ID сделки|
AccountID|	number|	ID счета|
SignalID|	number|	ID сигнала|
CommandID|	number|	ID команды|
SOID|	number|	ID StopOut|
SymbolSpecID|	number|	ID спецификации инструментов|
StreamID|	number|	ID потока котировок|
FillRequestID|	number|	ID запроса на открытие позиции|
DT|	number|	Дата/время создания сделки|
Type|	number|	См. ниже|
Symbol|	string|	Название инструмента|
Volume|	real|	Суммарный объем|
VolumeA|	real|	В том числе, объем A-Book|
Price|	real|	Средняя цена|
CommissionLiquidity|	real|	Комиссия за использование ликвидности|
CommissionBroker|	real|	Комиссия брокера|
CommissionTrader|	real|	Комиссия трейдера|
CommissionPartner|	real|	Комиссия партнера|
Commission|	real|	Суммарная комиссия|
Entry|	number|	0 - In, 1 - Out, 2 - InOut|
Profit|	real|	Прибыль/убыток по сделке (без учета комиссий и свопов)|
Swap|	real|	Своп|
TotalProfit|	real|	Прибыль/убыток по сделке (с учетом комиссий и свопов)|
Comment|	string|	Комментарий|
Tag|	number|	Произвольное число (для увязки с внешними системами)|
DealToID|	number|	ID результирующей сделки|
PrecisionPrice|	number|	Количество знаков после запятой при выводе цены|
PrecisionVolume|	number|	Количество знаков после запятой при выводе объема|
Netting|	number|	Признак неттинговой сделки|
TurnoverUSD|	real|	Сгенерированный дилом оборот в USD|

Значения поля "Type"
Значение | Расшифровка
---------|----------
0|	Buy|
1|	Sell|
2|	Balance|
3|	Credit|
4|	Additional charge|
5|	Correction|
6|	Bonus|
7|	Fee|
8|	Dividend|
9|	Interest|
10|	Canceled/Rejected buy deal|
11|	Canceled/Rejected sell deal|
12|	Periodical commission|
13|	Zero total  order|


Пример вызова:

{
"ID":3335
}

Пример ответа:

{
"DealID":3335,
"AccountID":545,
"SignalID":4878,
"CommandID":4554,
"SOID":7821,
"SymbolSpecID":1,
"StreamID":1,
"FillRequestID":54546,
"DT":"2018-11-23T11:59:12.493",
"Type":0,
"Symbol":"EURUSD",
"Volume":0.15,
"VolumeA":0.1,
"Price":1.1254,
"CommissionLiquidity":0.01,
"CommissionBroker":0.01,
"CommissionTrader":0.03,
"CommissionPartner":0.01,
"Commission":0.06,
"Entry":0,
"Profit":1.23,
"Swap":-0.03,
"TotalProfit":1.2,
"Comment":"",
"Tag":4546545,
"DealToID":4455,
"PrecisionPrice":2,
"PrecisionVolume":4,
"Netting":1,
"TurnoverUSD": 16783.02
}

## Операции с открытыми позициями

## positions.search
Поиск открытых позиций с фильтрацией по номеру счета.

URL вызова: https://ramm.store/api/manager/v1/positions.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерий выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	AccountID|	number|	ID счета|
-| ClientID|	number|	ID клиента|
-| WalletID|	number|	ID кошелька|
-| StrategyID|	number|	ID стратегии|
-| Symbol|	string|	Название инструмента|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, AccountID, SymbolSpecID, Symbol, VolumeRequestLong, VolumeRequestShort, VolumeOpen, VolumeRejected, PriceOpen, Swap, SwapDT, MarginRequestLong, MarginRequestShort, MarginOpen, MarginLongTotal, MarginShortTotal, MarginTotal, Profit, TotalProfit, ProfitCalcQuote, ClientID, WalletID, IDStrategy, StrategyName, TraderID, TraderLogin, Test|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|

Возвращаемые данные - структуры Filter, Pagination, OrderBy, массив Positions:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	AccountID|	number|	ID счета|
-| ClientID|	number|	ID клиента|
-| WalletID|	number|	ID кошелька|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
-| MaxPerPage|	number|	Максимальное количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты:ID, AccountID, SymbolSpecID, Symbol, VolumeRequestLong, VolumeRequestShort, VolumeOpen, VolumeRejected, PriceOpen, Swap, SwapDT, MarginRequestLong, MarginRequestShort, MarginOpen, MarginLongTotal, MarginShortTotal, MarginTotal, Profit, TotalProfit, ProfitCalcQuote, ClientID, WalletID, IDStrategy, StrategyName, TraderID, TraderLogin, Test|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|
Positions|	ID|	number|	ID позиции|
-| AccountID|	number|	ID счета|
-| SymbolSpecID|	number|	ID спецификации символа|
-| Symbol|	string|	Название инструмента|
-| VolumeRequestLong|	real|	Запрошенный в лонг объем|
-| VolumeRequestShort|	real|	Запрошенный в шорт объем|
-| VolumeOpen|	real|	Открытый объем|
-| VolumeRejected|	real|	Объем реджектов|
-| PriceOpen|	real|	Средняя цена открытия|
-| Swap|	real|	Накопленный своп|
-| SwapDT|	number|	Дата последнего начисления свопа|
-| MarginRequestLong|	real|	Маржа, зарезервированная под запрошенный в лонг объем|
-| MarginRequestShort|	real|	Маржа, зарезервированная под запрошенный в шорт объем|
-| MarginOpen|	real|	Маржа под открытый объем|
-| MarginLongTotal|	real|	Суммарная маржа в лонг|
-| MarginShortTotal|	real|	Суммарная маржа в шорт|
-| MarginTotal|	real|	Итоговая необходимая маржа|
-| Profit|	real|	Прибыль/убыток без учета свопа|
-| TotalProfit|	real|	Прибыль/убыток с учетом свопа|
-| ProfitCalcQuote|	real|	Котировка, по которой вычислялась прибыль|
-| ClientID|	number|	ID клиента|
-| WalletID|	number|	ID кошелька|
-| StrategyID|	number|	ID стратегии|
-| StrategyName|	string|	Название стратегии|
-| TraderID|	number|	ID трейдера|
-| TraderLogin|	string|	Логин трейдера|
-| PrecisionPrice|	number|	Количество знаков после запятой при выводе цены|
-| PrecisionVolume|	number|	Количество знаков после запятой при выводе объема|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|


Пример вызова:

{
"Filter":
{
"AccountID":333
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
"AccountID":333
},
"Pagination":
{
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 5,
"MaxPerPage": 100
},
"Positions":
[
{
"ID":333,
"AccountID":454777,
"SymbolSpecID":1,
"Symbol":"EURUSD",
"VolumeRequestLong":0.1,
"VolumeRequestShort":0,
"VolumeOpen":0.1,
"VolumeRejected":0,
"PriceOpen":1.1200,
"Swap":-0.13,
"SwapDT":"2018-11-23T11:59:12.493",
"MarginRequestLong":112.00,
"MarginRequestShort":0,
"MarginOpen":112.00,
"MarginLongTotal":224.00,
"MarginShortTotal":0,
"MarginTotal":224.00,
"Profit":100.00,
"TotalProfit":99.87,
"ProfitCalcQuote":1.1300,
"ClientID":222,
"WalletID":333,
"StrategyID":444,
"StrategyName":"ProfitTrading",
"TraderID":555,
"TraderLogin":"trader@trader.com",
"PrecisionPrice":2,
"PrecisionVolume":4,
"Test":0
}
]
}

## positions.get
Информация о заданной позиции.

URL вызова: https://ramm.store/api/manager/v1/positions.get

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID позиции|


Возвращаемые данные - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
PositionID|	number|	ID позиции|
AccountID|	number|	ID счета|
SymbolSpecID|	number|	ID спецификации символа|
Symbol|	string|	Название инструмента|
VolumeRequestLong|	real|	Запрошенный в лонг объем|
VolumeRequestShort|	real|	Запрошенный в шорт объем|
VolumeOpen|	real|	Открытый объем|
VolumeRejected|	real|	Объем реджектов|
PriceOpen|	real|	Средняя цена открытия|
Swap|	real|	Накопленный своп|
SwapDT|	number|	Дата последнего начисления свопа|
MarginRequestLong|	real|	Маржа, зарезервированная под запрошенный в лонг объем|
MarginRequestShort|	real|	Маржа, зарезервированная под запрошенный в шорт объем|
MarginOpen|	real|	Маржа под открытый объем|
MarginLongTotal|	real|	Суммарная маржа в лонг|
MarginShortTotal|	real|	Суммарная маржа в шорт|
MarginTotal|	real|	Итоговая необходимая маржа|
Profit|	real|	Прибыль/убыток без учета свопа|
TotalProfit|	real|	Прибыль/убыток с учетом свопа|
ProfitCalcQuote|	real|	Котировка, по которой вычислялась прибыль|
ClientID|	number|	ID клиента|
WalletID|	number|	ID кошелька|
PrecisionPrice|	number|	Количество знаков после запятой при выводе цены|
PrecisionVolume|	number|	Количество знаков после запятой при выводе объема|


Пример вызова:

{
"PositionID":333
}

Пример ответа:

{
"PositionID":333,
"AccountID":454777,
"SymbolSpecID":1,
"Symbol":"EURUSD",
"VolumeRequestLong":0.1,
"VolumeRequestShort":0,
"VolumeOpen":0.1,
"VolumeRejected":0,
"PriceOpen":1.1200,
"Swap":-0.13,
"SwapDT":"2018-11-23T11:59:12.493",
"MarginRequestLong":112.00,
"MarginRequestShort":0,
"MarginOpen":112.00,
"MarginLongTotal":224.00,
"MarginShortTotal":0,
"MarginTotal":224.00,
"Profit":100.00,
"TotalProfit":99.87,
"ProfitCalcQuote":1.1300,
"ClientID":222,
"WalletID":333,
"PrecisionPrice":2,
"PrecisionVolume":4
}


## Операции с записями об исполнении агрегированного ордера

## fills.search
Поиск записей об исполнении ордеров (филлов) по критериям фильтрации.
Если не заданы параметры @IDFillRequest, или @Symbol, или @Ticket, то нельзя задавать диапазон дат больше месяца.

URL вызова: https://ramm.store/api/manager/v1/fills.search

Тело запроса - строка JSON, содержит структуры Filter (задает критерии выбора), Pagination (разбивка на страницы), OrderBy (сортировка возвращаемых данных):
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	DTProcessedFrom|	number|	Начальное время обработки записи|
-| DTProcessedTo|	number|	Конечное время обработки записи|
-| FillRequestID|	number|	ID записи запроса на исполнение|
-| Symbol|	string|	Название инструмента|
-| Ticket|	number|	Номер тикета внешней ликвидности|
-| PriceFrom|	real|	Цена исполнения, начало диапазона|
-| PriceTo|	real|	Цена исполнения, конец диапазона|
-| ID|	number|	ID записи|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
Pagination|	CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, FillRequestID, Symbol, Price, VolumeFilled, VolumeRejected, CommissionFill, Ticket, DT, DTExecuted, DTProcessed, Test|
-| Direction|	string|	Направление сортировки, варианты: Asc, Desc|


Возвращаемые данные - структуры Filter, Pagination, OrderBy и массив Fills, каждый элемент которого содержит поля:
Структура |	Параметр | Тип | Описание
---------|----------|----------|----------
Filter|	FillRequestID|	number|	ID записи запроса на исполнение|
-| Symbol|	string|	Название инструмента|
-| Ticket|	number|	Номер тикета внешней ликвидности|
-| DTProcessedFrom|	number|	Начальное время обработки записи|
-| DTProcessedTo|	number|	Конечное время обработки записи|
-| PriceFrom|	real|	Цена исполнения, начало диапазона|
-| PriceTo|	real|	Цена исполнения, конец диапазона|
-| ID|	number|	ID записи|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|
OrderBy|	Field|	string|	Сортировка по параметру, варианты: ID, FillRequestID, Symbol, Price, VolumeFilled, VolumeRejected, CommissionFill, Ticket, DT, DTExecuted, DTProcessed, Test|
-| OrderDirection|	string|	Направление сортировки|
Pagination|	TotalRecords|	number|	Общее количество записей|
-| TotalPages|	number|	Общее количество страниц|
-| CurrentPage|	number|	Номер текущей страницы|
-| PerPage|	number|	Количество записей на одной странице|
-| MaxPerPage|	number|	Максимальное количество записей на одной странице|
Fills|	ID|	number|	ID записи об исполнении|
-| FillRequestID|	number|	ID запроса на исполнение|
-| Symbol|	string|	Название символа|
-| Price|	real|	Цена исполнения|
-| VolumeFilled|	real|	Открытый объем|
-| VolumeRejected|	real|	Объем реджекта|
-| CommissionFill|	real|	Комиссия за исполнение|
-| Ticket|	string|	Номер тикета внешней ликвидности|
-| DT|	number|	Время создания записи об исполнении|
-| DTExecuted|	number|	Время исполнения (полученное из внешней ликвидности, в ее временной зоне)|
-| DTProcessed|	number|	Время обработки записи в базе данных|
-| Test|	number|	Признак тестового счета (0,1, 2 - all)|


Пример вызова:

{
"Filter":
{
"FillRequestID":789565,
"Symbol":"EURUSD",
"Ticket":"158",
"PriceFrom":1.1200,
"PriceTo":1.1300
},
"OrderBy":
{
"Field": "ID",
"Direction": "Desc"
},
"Pagination":
{
"CurrentPage": 2,
"PerPage": 20
}
}

Пример ответа:

{
{
"Filter":
{
"FillRequestID":789565,
"Symbol":"EURUSD",
"Ticket":"158"
},
"OrderBy": {
"Field": "ID",
"Direction": "Desc"
},
"Pagination": {
"TotalRecords": 2,
"TotalPages": 1,
"CurrentPage": 1,
"PerPage": 20,
"MaxPerPage": 100
},
"Fills":
[
{
"ID":789565,
"FillRequestID":45465654,
"Symbol": "EURUSD",
"Price":1.1200,
"VolumeFilled":1,
"VolumeRejected":0.5,
"CommissionFill":-0.15,
"Ticket":"044544565",
"DT":"2018-11-23T11:55:12.493",
"DTExecuted":"2018-11-23T11:57:12.493",
"DTProcessed":"2018-11-23T11:59:12.493",
"Test":0
}
]
}

## fills.get
Информация о заданном филле (записи об исполнении).

URL вызова: https://ramm.store/api/manager/v1/fills.get

Тело запроса - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID записи об исполнении|


Возвращаемые данные - строка JSON, содержит параметры:
Параметр | Тип | Описание 
---------|----------|----------
ID|	number|	ID записи об исполнении|
FillRequestID|	number|	ID запроса на исполнение|
Symbol|	string|	Название символа|
Price|	real|	Цена исполнения|
VolumeFilled|	real|	Открытый объем|
VolumeRejected|	real|	Объем реджекта|
CommissionFill|	real|	Комиссия за исполнение|
Ticket|	string|	Номер тикета с внешней ликвидности|
DT|	number|	Время создания записи об исполнении|
DTExecuted|	number|	Время исполнения (полученное из внешней ликвидности, в ее временной зоне)|
DTProcessed|	number|	Время обработки записи в базе данных|


Пример вызова:

{
"ID":789565
}

Пример ответа:

{
"ID":789565,
"FillRequestID":45465654,
"Symbol": "EURUSD",
"Price":1.1200,
"VolumeFilled":1,
"VolumeRejected":0.5,
"CommissionFill":-0.15,
"Ticket":"044544565",
"DT":"2018-11-23T11:55:12.493",
"DTExecuted":"2018-11-23T11:57:12.493",
"DTProcessed":"2018-11-23T11:59:12.493"
}


## Операции с запросами на исполнение

## fillRequests.search


## fillRequests.get


## Операции с сигналами на исполнение

## signals.search


## signals.get


## Активы: валюты, акции, фьючерсы и т. п.

## assets.search


## assets.get


## Торговые инструменты

## symbols.search


## symbols.get


## Передача торговых сигналов

## trading.searchStrategies


## trading.addSignal


## trading.addSync


## Операции со стримами

## streams.get


## Ошибки и возможные коды ответа

