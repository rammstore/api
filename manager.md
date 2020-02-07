# RAMM.STORE Manager API Documentation
### Содержание
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
Действия с настройками пользователей
**settings.set
settings.get**
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
