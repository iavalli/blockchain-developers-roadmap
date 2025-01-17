# NFT Marketplace

1. Ознакомится с  [Lesson 15: NextJS NFT Marketplace](https://github.com/smartcontractkit/full-blockchain-solidity-course-js#lesson-15-nextjs-nft-marketplace-if-you-finish-this-lesson-you-are-a-full-stack-monster)
2. Написать контракт ончейн NFT-маркетплейса

## ТЗ

Маркетплейс выступает площадкой где торгуются NFT-коллекции или отдельные токены ERC721(NFT). Маркетплейс имеет простой функционал - токены ERC721(NFT) выставляются на продажу и покупаются за токены ERC20.

## Как это выглядит со стороны продавца NFT

- продавец может листить (размещать нфт на площадке)
- продавец может изменять параметры листинга
- продавец может отменять листинг
- продавец может принять оффер на покупку NFT по цене, которую предложил покупатель

## Как это выглядит со стороны покупателя NFT

- покупатель может купить NFT, которая листится по цене, которую установил продавец
- покупатель может предложить офер на покупку NFT по своей цене и установить срок окончания офера

## Как это выглядит со стороны маркетплейса

- маркетплейс может установить комиссию площадки за каждую сделку
	- по умолчанию комиссия должна быть 2% (прописывается в контракте либо задается через конструктор)
	- должна быть возможность просматривать комиссию на смарт-контракте
	- должна быть возможность изменять комиссию на смарт-контракте
	- должна быть возможность установить минимальную комиссию 0.01% (смотри базисные пункты и как с ними работать в солидити для точных расчетов)
- маркетплейс может установить получателя комиссии
    - получатель комиссии должен задаваться в конструкторе
    - должна быть возможность изменить получателя комиссий площадки
    - установить нового получателя комиссии может только владелец маркетплейса (либо адрес, у которого есть доступ к подобным операциям, например владелец контракта (owner))
    - должна быть возможность посмотреть адрес получателя комиссии площадки

### Функционал смарт-контракта:

- листинг нфт
- отмена листинга нфт
- изменение параметров листинга (токен оплаты и цена)
- покупка нфт, которая листится
- создание офера
- отмена офера
- принятие офера продавцом
- запись комиссии платформы
- запись получателя комиссии платформы
- просмотр комиссии платформы
- просмотр получателя комиссии платформы

### Дополнительные фичи, которые можно реализовать

- Проверка интерфейса ERC-721 во время листинга нфт и создания офера (ERC-165)
- Списание роялти и отправка создателю NFT, если NFT поддерживает ERC-2981
- Аукцион

## Требования к выполнению задания

- **В смарт-контракте должны быть модификаторы**: `notListed`, `isListed`, `notOffered`, `isOffered`
- **В смарт-контракте должны быть события**: `ItemListed`, `ItemUpdated`, `ItemSold`, `OfferCreated`, `OfferCanceled`,  `UpdatePlatformFee`,  `UpdatePlatformFeeRecipient`
- **В смарт контракте должны быть все необходимые проверки** - например токен оплаты ERC20 должен проверяться на нулевой адрес, а ERC721 должен поддерживать интерфейс ERC721, в функции должны передаваться корректные аргументы и т.д.
- **Функции площадки должны быть с ограниченным доступом** (использовать Ownable)
- **Использовать ReentrancyGuard от OpenZeppelin** - для защиты функций смарт-контракта от повторных вызовов.

---

- Итогом выполнения задания должен быть github-репозиторий, в котором будет лежать `hardhat + typescript` проект.
- Покрытие юнит тестами должно быть 100%. Для проверки покрытия тестами кода можно использовать плагин [solidity-coverage](https://www.npmjs.com/package/solidity-coverage)