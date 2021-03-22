# Список событий платформы RAMM

* [event_deal_create](#event_deal_create)
* [event_investment_close]
* [event_investment_create]
* [event_investment_in]
* [event_investment_out]
* [event_investment_pause]
* [event_investment_start]
* [event_strategy_close]
* [event_strategy_create]
* [event_wallet_create]
* [event_wallet_in]
* [event_wallet_out]

## События

### event_deal_create

Создание сделки. Сделка в платформе RAMM это и покупка-продажа объёма, и переводы с кошелька на инвестицию и обратно.

**Возвращаемые данные**

***- Покупка-продажа объёма***

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
      "dtexecuted": "2021-03-22T10:39:35.057",
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
***- Перевод денег***

Отрицательное значение profit говорит о том, что это перевод с инвестиции на кошелёк. При переводе с кошелька на инвестицию значение будет положительное.

```json
{
  "type": "event_deal_create",
  "sent_at": "2021-03-19T14:50:24.010",
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

Закрытие инвестиции

**Возвращаемые данные**
