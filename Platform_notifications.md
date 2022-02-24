# Список событий платформы RAMM

### Содержание

* [event_deal_create](#event_deal_create)
* [event_investment_close](#event_investment_close)
* [event_investment_create](#event_investment_create)
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
**Типы сделки: 0 - buy; 1 - sell; 2 - balance**

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
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
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
      "entry": "in",
      "swap": 1.23 
    },
    "account": {
      "id": 1004187
    },
    "position": {
      "id": 17379,
      "volume": 0.01,
      "priceopen": 1800.83,
      "pricecurrent": 1800.83,
      "swap": 1.4    
  }
}
```
[Вернуться к содержанию](#Содержание)

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
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
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
[Вернуться к содержанию](#Содержание)

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
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
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
[Вернуться к содержанию](#Содержание)

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
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
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
[Вернуться к содержанию](#Содержание)

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
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
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
[Вернуться к содержанию](#Содержание)

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
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
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
[Вернуться к содержанию](#Содержание)

### event_strategy_close

**Закрытие стратегии**

**Возвращаемые данные:**
```json
{
  "type": "event_strategy_close",
  "sent_at": "2021-03-22T10:17:55.010",
  "uuid": "CEAFF01B-679D-4A85-877C-6347D02A2DBE",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
    }
  }
}
```
[Вернуться к содержанию](#Содержание)

### event_strategy_create
**Создание стратегии**

**Возвращаемые данные:**
```json
{
  "type": "event_strategy_create",
  "sent_at": "2021-01-21T10:18:31.517",
  "uuid": "312B2FDD-D006-4DE8-9674-A36D4891D95F",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "wallet": {
      "id": 11436
    },    
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
    }
  }
}
```
[Вернуться к содержанию](#Содержание)

### event_wallet_create

**Создание кошелька (клиента)**

**Возвращаемые данные:**
```json
{
  "type": "event_wallet_create",
  "sent_at": "2021-01-21T10:01:24.903",
  "uuid": "A885E0D3-ACD3-44EE-8C0D-17A7BC647269",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158,
      "email": "test@test.com"
    },
    "wallet": {
      "id": 158,
      "dt": "2021-01-21T10:01:24.903",
      "balance": 0,
      "invested": 0
    }
  }
}
```
[Вернуться к содержанию](#Содержание)

### event_wallet_in

**Пополнение кошелька**

**balance - баланс кошелька до пополнения средств, amount - сумма пополнения**

**transfer.type см. https://github.com/rammstore/api/blob/dev/client.md#walletTransferssearch**

**Возвращаемые данные:**
```json
{
  "type": "event_wallet_in",
  "sent_at": "2021-01-21T10:17:56.887",
  "uuid": "B1A8EA44-F39F-4486-810D-5C81C0D3580A",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "transfer": {
      "type": 0,
      "id": 18483
    }, 
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
    },
    "account": {
      "id": 158
    },    
    "wallet": {
      "id": 158,
      "dt": "2021-01-21T07:57:43.897",
      "balance": 26922.64,
      "invested": 3871.02
    },
    "amount": 10608.61
  }
}
```
[Вернуться к содержанию](#Содержание)

### event_wallet_out

**Вывод средств с кошелька**

**balance - баланс кошелька до вывода средств, amount - сумма вывода**

**transfer.type см. https://github.com/rammstore/api/blob/dev/client.md#walletTransferssearch**

**Возвращаемые данные:**
```json
{
  "type": "event_wallet_out",
  "sent_at": "2021-01-21T08:23:16.343",
  "uuid": "DF12AA0A-3607-402F-95F3-140E0D27B28A",
  "publisher": "ramm",
  "version": "1",
  "data": {
    "event": {
      "dt": "2021-01-21T10:39:34.057"
    },
    "client": {
      "id": 158
    },
    "transfer": {
      "type": 0,
      "id": 18483
    }, 
    "strategy": {
      "id": 1,
      "executiontags": "Tags",
      "name": "Best Technologies",
      "externalaccount": "2046940abc"
    },
    "account": {
      "id": 158
    },    
    "wallet": {
      "id": 158,
      "dt": "2021-01-21T07:57:43.897",
      "balance": 26922.64,
      "invested": 3871.02
    },
    "amount": 10608.61
  }
}
```
[Вернуться к содержанию](#Содержание)
