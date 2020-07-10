Лог изменений методов API ramm.store.

Difference API начального и 2-ой версии с офертами.
1. Strategies.get: в возвращаемых: Fee и Commission перемещены из блока Strategy в новый блок PublicOffer под названиями FeeRate, CommissionRate.
Там же из MyAccount убрано поле TotalProfitNet.
Новый блок MyAccountOffer с полями ID, FeeRate, CommissionRate.

2. strategies.search: добавились 2 входных параметра AgeMin, DealsMin.
Вместо Offer теперь блок PublicOffer.
Новый блок TraderInfo с полями MasterAccount, FeePaid, FeeToPay, CommissionPaid, CommissionToPay.
Новый блок PartnerInfo с полями MasterAccount, FeePaid, FeeToPay, CommissionPaid, CommissionToPay.
Новый блок AccountOffer с полями ID, Commission, Fee.

3. Удален полностью метод myStrategies.search, нужно проверить поиском, что он нигде не используется.

4. Добавлены 3 новых метода по офертам: myStrategies.addOffer, myStrategies.setPublicOffer, strategies.getOffers.

5. myStrategies.add - входные параметры: FeeRate, CommissionRate, Shared - удалены; Protection, Target, Money - выделены в отдельный блок Account.
Возвращаемые параметры: StrategyID, AccountID, AccountCommandID, множество других старых параметров удалены.

6. accounts.add - во входных добавлены OfferID, Link.

7. accounts.get, accounts.search, session.login, wallets.get, positions.search  - в возвращаемых убран Bonus.

8. accounts.search - в возвращаемых убран PartnerShare.
