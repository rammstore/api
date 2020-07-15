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

URL вызова: https://maindc.ramm.store/api/manager/v1/sessions.logout

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
---------|----------|----------|----------
Manager|	Login|	string|	Логин|

|Password|	string|	Пароль|

|FirstName|	string|	Имя

|LastName|	string|	Фамилия|

|Mobile|	string|	Номер телефона|

|Comment|	string|	Комментарий|
Methods||string|	Название метода, на который устанавливаются права|

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

## managers.get

## managers.set

## managers.getMethods

## managers.setMethods

## Операции с зарегистрированными в RAMM компаниями

## companies.add

## companies.search

## companies.get

## Операции с компанией текущей сессии менеджера

## company.get

## company.set

## company.getEquity

## Операции с клиентами сервиса RAMM

## clients.addWithWallet

## clients.search

## clients.get

## clients.set

## clients.getCharges

## clients.searchStatistic

## Кошельки клиентов

## wallets.get

## wallets.search

## Операции с кошельками клиентов

## walletTransfers.add

## walletTransfers.search

## walletTransfers.get

## walletTransfers.getRewards

## Операции со стратегиями RAMM

## strategies.search

## strategies.get

## strategies.add

## strategies.set

## strategies.close

## strategysymbolstat.get

## Операции с торговыми счетами клиентов

## accounts.search

## accounts.get

## accounts.set

## accounts.close

## accounts.getStatement

## accounts.pause

## accounts.resume

## Операции со сделками на счете

## deals.search

## deals.get

## Операции с открытыми позициями

## positions.search

## positions.get

## Операции с записями об исполнении агрегированного ордера

## fills.search

## fills.get

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

