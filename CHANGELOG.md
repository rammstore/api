Лог изменений методов API ramm.store.

Различия 1-ой и 2-ой версии клиентского API.

Основные изменения связаны с добавлением понятия Offer, т.е. условия вознаграждения трейдера.
API v2 разработан для создания множественных оферт, но в данный момент в стратегии может быть максимум две оферты - трейдерская (создается при создании стратегии, все параметры вознаграждения равны 0) и инвесторская (создается в произвольный момент времени).

Инвесторская Оферта может быть публичной или непубличной. Поменять публичность оферты можно в любое время.
Доступ к непубличной оферте возможен по линку (Link) в методе strategies.get.

1. Strategies.get: в возвращаемых: Fee и Commission перемещены из блока Strategy в новый блок PublicOffer под названиями FeeRate, CommissionRate.
Блок MyAccount переименован в Account, из него убрано поле TotalProfitNet и добавлено несколько новых полей.
Новый блок MyAccountOffer с полями ID, FeeRate, CommissionRate.
Добавлены блоки PublicOffer, TraderInfo, PartnerInfo.
Добавлен фильтр по оферте (поле Link)
2. strategies.search: добавились входные параметры AgeMin, DealsMin, YieldMin, удалены старые входные фильтры, кроме Name.
Вместо Offer теперь блок PublicOffer.
Новый блок TraderInfo с полями MasterAccount, FeePaid, FeeToPay, CommissionPaid, CommissionToPay.
Новый блок PartnerInfo с полями MasterAccount, FeePaid, FeeToPay, CommissionPaid, CommissionToPay.
Новый блок AccountOffer с полями ID, Commission, Fee.
3. Удален полностью метод myStrategies.search. Для поиска своих стратегий надо пользоваться методом strategies.search в режиме 'SearchMode' = 'MyActiveStrategies'.
4. Добавлены 3 новых метода по офертам: myStrategies.addOffer, myStrategies.setPublicOffer, strategies.getOffers.
5. myStrategies.add - входные параметры: FeeRate, CommissionRate, Shared - удалены; Protection, Target, Money - выделены в отдельный блок Account.
Возвращаемые параметры: StrategyID, AccountID, AccountCommandID, множество других старых параметров удалены.
6. accounts.add - во входных добавлены OfferID, Link.
7. accounts.get, accounts.search, session.login, wallets.get, positions.search, wallettransfers.search, deals.search  - в возвращаемых убран Bonus.
8. accounts.search - в возвращаемых убран PartnerShare, в структуру Offer Добавлено ID. Фильтры в accounts.search переименованы, везде добавлено "Rate" в конце: Strategy.Offer.FeeRate, Strategy.Offer.CommissionRate.
9. Fee (там, где имеется в виду ставка фи) переименовано в FeeRate, Commission (там, где имеется в виду ставка комиссии) переименован в CommissionRate.
10. Везде выставлен порядок FeeRate, затем CommissionRate, не наоборот.
11. Удален метод ratings.get, вместо него при получении рейтинга используется strategies.search с параметром SearchMode = "Rating"
12. Удален метод accounts.getStatement
13. Метод accounts.search удален, вместо него добавлен метод accounts.searchClosed для поиска только закрытых счетов
14. Во всех методах, возвращающих параметры счета, вместо поля Status теперь используется State (strategies.get, strategies.search - в блоке Account, accounts.xxx - в основном блоке возвращаемых параметров)
15. Accounts.get - изменен набор входных параметров

