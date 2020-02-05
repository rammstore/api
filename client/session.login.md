Авторизует пользователя по логину и паролю, создает сессию. Возвращает токен для использования API и информацию по сессии.

URL вызова: https://maindc.ramm.store/api/client/v1/session.login

Тело запроса - строка JSON, содержит параметры:

Login	string	Логин
Password	string	Пароль
ExpirationMinutes	number	Автоматическое удаление сессии при неактивности в течении заданного времени. Значение по умолчанию: 60.
OTP	string	Одноразовый пароль
Возвращаемые данные: строка JSON, содержит структуры Session, Client, Company и массив Wallets:


Session	Token	string	Токен, уникальный идентификатор

WalletID	number	ID кошелька

DTLastActivity	number	Время последней активности

ExpirationMinutes	number	Длительность сессии
Client	Login	string	Логин

FirstName	string	Имя клиента

LastName	string	Фамилия клиента

Language	string	Язык интерфейса (1-англ., 2 - рус.)

PushToken	string	Токен клиента
Company	Name	string	Название компании

Demo	boolean	Признак демо-компании

Contacts	string	Контакты компании
Wallets	ID	number	ID кошелька

IDClient	number	ID клиента

DT	number	Дата создания кошелька

Asset	string	Название актива

Status	number	0-new, 1-active

Balance	real	Сумма в кошельке

Bonus	real	Сумма бонусов

Invested	real	Инвестированная сумма

Margin	real	Задействованная маржа

IntervalPnL	real	Прибыль/убыток в текущем торговом интервале
Пример вызова:


{
    "Login": "test@gmail.com",
    "Password": "qwert12345"
}

Пример ответа:

{
    "Session": {
        "Token": "398D6062-8D57-4B9D-99E3-5421E4D19E89",
        "WalletID": 136,
        "DTLastActivity": "2018-12-18T07:25:01.147",
        "ExpirationMinutes": 60
    },
    "Client": {
        "Login": "test@gmail.com",
        "FirstName": "test1",
        "LastName": "test2",
        "Language": "en",
        "PushToken": "80b5aaa0-21c7-494d-a0c8-1065098d912e"
    },
    "Company": {
        "Name": "FXTrade",
        "Demo": false
    },
    "Wallets": [
        {
            "ID": 12,
            "IDClient": 2132,
            "DT": "2018-12-18T07:25:01.147",
            "Asset": "USD",
            "Status": 1,
            "Balance": 0,
            "Bonus": 0,
            "Invested": 0,
            "Margin": 0,
            "IntervalPnL": 0
        }
    ]
}
