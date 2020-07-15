# RAMM.STORE Manager API Documentation
### Содержание
Образец формата для редактирования ссылок: * [Выполнение запросов](#Выполнение-запросов)
* [Введение] (#Введение)
* Регистрация менеджера
* Описание методов API
  * Аутентификация, действия с собственной сессией
    * session.login
    * session.get
    * session.getMethods
    * session.heartbeat
    * session.getMySessions
    * session.logout
  * Управление сессиями других менеджеров
    * sessions.search
    * sessions.logout
  * Информация о залогиненном менеджере, получение и изменение собственного профиля.
    * profile.get
    * profile.set
  * Действия с настройками пользователей
    * settings.set
    * settings.get
  * Действия с паролями
    * password.set
    * clients.setPassword
    * clients.getOTP
  * Операции с менеджерами, управляющими сервисом RAMM.
    * managers.add
    * managers.search
    * managers.get
    * managers.set
    * managers.getMethods
    * managers.setMethods
  * Операции с зарегистрированными в RAMM компаниями.
    * companies.add
    * companies.search
    * companies.get
  * Операции с компанией текущей сессии менеджера
    * company.get
    * company.set
    * company.getEquity
  * Операции с клиентами сервиса RAMM
    * clients.addWithWallet
    * clients.search
    * clients.get
    * clients.set
    * clients.getCharges
    * clients.searchStatistic
  * Кошельки клиентов
    * wallets.get
    * wallets.search
  * Операции с кошельками клиентов
    * walletTransfers.add
    * walletTransfers.search
    * walletTransfers.get
    * walletTransfers.getRewards
  * Операции со стратегиями RAMM
    * strategies.search
    * strategies.get
    * strategies.add
    * strategies.set
    * strategies.close
    * strategysymbolstat.get
  * Операции с торговыми счетами клиентов
    * accounts.search
    * accounts.get
    * accounts.set
    * accounts.close
    * accounts.getStatement
    * accounts.pause
    * accounts.resume
  * Операции со сделками на счете
    * deals.search
    * deals.get
  * Операции с открытыми позициями
    * positions.search
    * positions.get
  * Операции с записями об исполнении агрегированного ордера.
    * fills.search
    * fills.get
  * Операции с запросами на исполнение
    * fillRequests.search
    * fillRequests.get
  *Операции с сигналами на исполнение
    * signals.search
    * signals.get
  * Активы (валюты, акции, фьючерсы и т.п.)
    * assets.search
    * assets.get
  * Торговые инструменты
    * symbols.search
    * symbols.get
  *Передача торговых сигналов
    *trading.searchStrategies
    * trading.addSignal
    * trading.addSync
  * Операции со стримами
    * streams.get
  * Ошибки и возможные коды ответа

##Введение

Менеджерский API доступен по адресу https://maindc.ramm.store/api/manager/.
API построен на указании названий вызываемых методов в URL и передаче дополнительной информации в JSON. Для получения и изменения информации используется метод POST.
Например, https://maindc.ramm.store/api/manager/v1/clients.search

Любые действия с использованием API требуют аутентификации от имени зарегистрированного менеджера.

Заголовок запроса должен содержать значения: 

Token: <токен>
Content-Type: application/jsonstrategies

Регистрация менеджера
Новый менеджер может быть зарегистрирован следующими способами:

На сайте RAMM.TECH, при создании новой компании:
Войти на сайт http://ramm.tech
Заполнить анкету (данные о компании и себе).
Нажать кнопку "Продолжить".
На указанную почту высылается письмо, в которой надо пройти по ссылке для подтверждения почты.
Нужно открыть письмо и пройти по ссылке в нем.
Происходит регистрация компании и менеджера. Менеджеру добавляются все права на данную компанию.
На указанную почту приходит регистрационная информация с указанием логина и пароля для входа, а так же ссылка на вход в бекофис.
При первом входе рекомендуется поменять пароль.
По инвайту
Зарегистрированный менеджер может создать инвайт на регистрацию менеджера и новой компании.
После создания, если произойдет регистрация с использованием кода инвайта, то новый менеджер и его компания будут считаться привлеченными создателем инвайта.
В остальном, регистрация менеджера и новой компании происходит аналогично варианту создания без инвайта.
Регистрация через бэкофис
Регистрация в существующей компании (родительской или дочерней). Указывается логин, пароль, набор прав.
Зарегистрированный менеджер, используя логин и пароль, может войти на сайт https://backoffice.ramm.tech и нажать на кнопку Login.
В открывшемся окне вводит логин и пароль, при этом отправляется запрос к сайту. Сайт возвращает URL AccessServer'а, на который можно войти с этими регистрационными данными. Далее происходит автоматический редирект на этот URL с указанными данными.

Аналогично реализован функционал API для входа. Сначала отправляется запрос к основному домену, который возвращает URL AccessServer'а. Далее, на выданном URL вызывается метод session.login, который возвращает токен. Все остальные функции менеджерского API используют этот токен для авторизации.

Описание методов API
Аутентификация, действия с собственной сессией.

session.login

session.get

session.getMethods

session.heartbeat

session.getMySessions

session.logout

Управление сессиями других менеджеров

sessions.search

sessions.logout

Информация о залогиненном менеджере, получение и изменение собственного профиля.

profile.get

profile.set

Действия с паролями

password.set

clients.setPassword

clients.getOTP

Операции с менеджерами, управляющими сервисом RAMM.

managers.add

managers.search

managers.get

managers.set

managers.getMethods

managers.setMethods

Операции с зарегистрированными в RAMM компаниями.

companies.add

companies.search

companies.get

Операции с компанией текущей сессии менеджера

company.get

company.set

Операции с клиентами сервиса RAMM

clients.addWithWallet

clients.search

clients.get

clients.set

clients.getCharges

Кошельки клиентов

wallets.get

wallets.search

Операции с кошельками клиентов

walletTransfers.add

walletTransfers.search

walletTransfers.get

walletTransfers.getRewards

Операции со стратегиями RAMM

strategies.search

strategies.get

strategies.add

strategies.set

strategies.getSymbolStat

Операции с торговыми счетами клиентов

accounts.search

accounts.get

accounts.set

accounts.close

accounts.getStatement

accounts.pause

accounts.resume

Операции со сделками на счете

deals.search

deals.get

Операции с открытыми позициями

positions.search

positions.get

Операции с записями об исполнении ордеров.

fills.search

fills.get

Операции с запросами на исполнение

fillRequests.search

fillRequests.get

Операции с торговыми сигналами

signals.search

signals.get

Активы (валюты, акции, фьючерсы и т.п.)

assets.search

assets.get

Торговые инструменты

symbols.search

symbols.get

Передача торговых сигналов

trading.searchStrategies

trading.addSignal

trading.addSync

Операции со стримами

streams.get
