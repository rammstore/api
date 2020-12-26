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
    * [Активы: валюты, акции, фьючерсы и т.п.](#Активы-валюты-акции-фьючерсы-и-т-п)
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

Менеджерский API доступен по адресу https://maindc.ramm.store/api/manager/.
API построен на указании названий вызываемых методов в URL и передаче дополнительной информации в JSON. Для получения и изменения информации используется метод POST.
Например, https://maindc.ramm.store/api/manager/v1/clients.search

Любые действия с использованием API требуют аутентификации от имени зарегистрированного менеджера.

Заголовок запроса должен содержать значения: 

Token: <токен>
Content-Type: application/jsonstrategies

## Регистрация менеджера
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

## Описание методов API
## Аутентификация, действия с собственной сессией

## session.login

## session.get

## session.getMethods

## session.heartbeat

## session.getMySessions

## session.logout

## Управление сессиями других менеджеров

## sessions.search

## sessions.logout

## Информация о залогиненном менеджере, получение и изменение собственного профиля

## profile.get

## profile.set

## Действия с настройками пользователей
        
## settings.set
        
## settings.get

## Действия с паролями

## password.set

## clients.setPassword

## clients.getOTP

## Операции с менеджерами, управляющими сервисом RAMM

## managers.add

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

## Активы: валюты, акции, фьючерсы и т.п.

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
