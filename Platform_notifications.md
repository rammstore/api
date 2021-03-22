# Список событий платформы RAMM

* [event_deal_create](#event_deal_create)
* [event_investment_close](#event_investment_close)
* [event_investment_create](#event_investment_create)
* [event_investment_in](#event_investment_in)
* [event_investment_out](#event_investment_out)
* [event_investment_pause](#event_investment_pause)
* [event_investment_start](#event_investment_start)
* [event_strategy_close](#event_strategy_close)
* [event_strategy_create](#event_strategy_create)
* [event_wallet_create](#event_wallet_create)
* [event_wallet_in](#event_wallet_in)
* [event_wallet_out](#event_wallet_out)

## События

### event_deal_create

**Создание сделки. Сделка в платформе RAMM это и покупка-продажа объёма, и переводы с кошелька на инвестицию и обратно.**

**Возвращаемые данные:**

*- Покупка-продажа объёма*

```json
{
  "type": "event_deal_create",
  "sent_at": "2021-01-21T10:39:35.057",
  "uuid": "F2442CC5-19FA-4584-9EF1-D18858B2010F",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "client": {
      "id": 158
    },
    "strategy": {
      "id": 1,
      "name": "Best Technologies"
    },
    "deal": {
      "id": 264400,
      "symbol": "XAUUSD",
      "type": 0,
      "volume": 0.01,
      "price": 1732.4,
      "dtexecuted": "2021-01-21T10:39:35.057",
      "commissionbroker": 0,
      "commissionliquidity": 0.02,
      "commissiontrader": 0,
      "profit": 0,
      "entry": "in"
    },
    "account": {
      "id": 1004187
    }
  }
}
```
*- Перевод денег*

Отрицательное значение profit говорит о том, что это перевод с инвестиции на кошелёк. При переводе с кошелька на инвестицию значение будет положительное.

```json
{
  "type": "event_deal_create",
  "sent_at": "2021-01-21T14:50:24.010",
  "uuid": "FDF0FF74-0A52-4007-B54A-0CA672C7E91B",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "client": {
      "id": 158
    },
    "strategy": {
      "id": 1,
      "name": "Best Technologies"
    },
    "deal": {
      "id": 264231,
      "type": 2,
      "dtexecuted": "2021-01-21T14:50:24.010",
      "profit": -3092.79
    },
    "account": {
      "id": 1004746
    }
  }
}
```
### event_investment_close

**Закрытие инвестиции**

**Возвращаемые данные:**
```json
{
  "type": "event_investment_close",
  "sent_at": "2021-01-21T10:17:55.980",
  "uuid": "D394B6A4-D780-4E68-811E-39805C99648E",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "client": {
      "id": 158
    },
    "strategy": {
      "id": 1,
      "name": "Best Technologies"
    },
    "account": {
      "id": 1004788,
      "balance": 0,
      "equity": 0,
      "dt": "2021-01-21T08:23:16.343",
      "margin": 0,
      "state": 15,
      "status": 6
    }
  }
}
```
### event_investment_create

**Создание инвестиции**

**Возвращаемые данные:**
```json
{
  "type": "event_investment_create",
  "sent_at": "2021-01-21T08:23:16.343",
  "uuid": "EDA4A76C-FE46-464F-9CC9-8F4F72C4EADE",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "сlient": {
      "id": 158
    },
    "strategy": {
      "id": 1,
      "name": "Best Technologies"
    },
    "account": {
      "id": 1004788,
      "balance": 0,
      "equity": 0,
      "dt": "2021-01-212T08:23:16.343",
      "margin": 0,
      "state": 0,
      "status": 0
    }
  }
}
```
### event_investment_in

**Пополнение средств инвестиции**

**Возвращаемые данные:**
```json
{
  "type": "event_investment_in",
  "sent_at": "2021-01-21T08:23:16.390",
  "uuid": "9AB7C768-8063-40B6-BEA7-7F134007A225",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "client": {
      "id": 158
    },
    "strategy": {
      "id": 1,
      "name": "Best Technologies"
    },
    "account": {
      "id": 1004788,
      "balance": 0,
      "equity": 0,
      "dt": "2021-01-21T08:23:16.343",
      "margin": 0,
      "state": 0,
      "status": 0
    },
    "amount": 2800.74
  }
}
```
### event_investment_out

**Частичный вывод средств из инвестиции**

**Возвращаемые данные:**
```json
{
  "type": "event_investment_out",
  "sent_at": "2021-01-21T10:10:08.270",
  "uuid": "C59E9AFA-8D86-4412-A6CA-EA79AC4482F0",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "client": {
      "id": 158
    },
    "strategy": {
      "id": 1,
      "name": "Best Technologies"
    },
    "account": {
      "id": 1004187,
      "balance": 605.93,
      "equity": 605.93,
      "dt": "2021-01-21T20:04:57.533",
      "margin": 0,
      "state": 11,
      "status": 4
    },
    "amount": 200
  }
}
```
### event_investment_pause

**Постановка инвестиции на паузу**

**Возвращаемые данные:**
```json
{
  "type": "event_investment_pause",
  "sent_at": "2021-01-21T10:09:38.483",
  "uuid": "7C5FA86D-40AE-4345-9FEF-BFF1DEDB8152",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "client": {
      "id": 158
    },
    "strategy": {
      "id": 2175,
      "name": "Best Technologies"
    },
    "account": {
      "id": 1004187,
      "balance": 579.18,
      "equity": 605.95,
      "dt": "2021-01-21T20:04:57.533",
      "margin": 17.33,
      "state": 3,
      "status": 1
    }
  }
}
```
### event_investment_start

**Снятие инвестиции с паузы**

**Возвращаемые данные:**
```json
{
  "type": "event_investment_start",
  "sent_at": "2021-01-21T10:39:33.853",
  "uuid": "CBC6817D-15E8-468F-A9FE-2B9F84C3B759",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "client": {
      "id": 158
    },
    "strategy": {
      "id": 2175,
      "name": "Best Technologies"
    },
    "account": {
      "id": 1004187,
      "balance": 405.93,
      "equity": 405.93,
      "dt": "2021-01-21T20:04:57.533",
      "margin": 0,
      "state": 11,
      "status": 4
    }
  }
}
```
