# Вопросы по Solidity

## Try / Catch

1. Что такое try / catch? Как эта конструкция используется в языке Solidity? Какие могут быть причины ошибки вызова функции смарт-контракта?
2. Какие типы блоков Catch поддерживаются в Solidity?
    - Какой тип блока catch используется для ```revert("reasonString")```?
    - Какой тип блока catch используется, если ошибка была вызвана делением на ноль?
    - Какие типы блока catch используется, если ошибка не подходит под два предыдущих случая, но ошибку необходимо обработать?
3. Чем отличается try / catch от других вариантов обработки ошибок(require, assert, revert)?
4. Может ли вызов функции через call заменить конструкцию try / catch?

- [Solidity docs](https://docs.soliditylang.org/en/v0.8.19/control-structures.html#try-catch)

## Unchecked Math

1. Что такое **overflow/underflow** в solidity?
2. Для чего используется библиотека Safe Math от OpenZeppelin? Нужно ли в версии solidity выше 0.8 использовать библиотеку Safe Math?
3. Для чего в solidity существует **unchecked**?

4. Расскажи, что вернет функция ```getCounter()```, после вызова функции ```increase()``` или ```decrease()``` в каждой реализации контракта ```Counter```.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;

contract Counter {
    uint8 _counter= 255;

    function increase() public payable {
        _counter++;
    }

    function getCounter() external view returns(uint8) {
        return _counter;
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;

contract Counter {
    uint8 _counter= 255;

    function increase() public payable {
        unchecked {_counter++;}
    }

    function getCounter() external view returns(uint8) {
        return _counter;
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.7.0;

contract Counter {
    uint8 _counter= 255;

    function increase() public payable {
        _counter++;
    }

    function getCounter() external view returns(uint8) {
        return _counter;
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;

contract Counter {
    int8 _counter= -128;

    function decrease() public payable {
        unchecked {_counter--;}
    }

    function getCounter() external view returns(int8) {
        return _counter;
    }
}
```

5. Как можно использовать **unchecked** для уменьшения затрат газа при работе с циклами?

- [Integer Overflow and Underflow](https://blog.solidityscan.com/integer-overflow-and-underflow-in-smart-contracts-9598032b5a99)
- [Sample Smart Contract to see how it works](https://ethereum-blockchain-developer.com/010-solidity-basics/03-integer-overflow-underflow/)
- [Unchecked arithmetic trick](https://medium.com/@ashwin.yar/solidity-tips-and-tricks-1-unchecked-arithmetic-trick-cefa18792f0b)

## Function selector

1. Что такое сигнатура функции? Что такое селектор функций и как он создается в Solidity? Какие есть способы получить селектор функции в смарт-контракте? (назвать несколько)
2. Как селекторы функций связаны с ABI (Application Binary Interface) в Solidity и какое значение имеет ABI для разработки контрактов?
3. Можете ли быть так, что две функции в контракте Solidity могут иметь одинаковое имя, но разные селекторы функций?
4. Как можно использовать селекторы функций для оптимизации использования газа в контрактах Solidity?
5. Как селекторы функции используются в вычислении `interfaceId` для интерфейса смарт-контракта?

-   [Solidity docs](https://docs.soliditylang.org/en/develop/abi-spec.html#function-selector)
-   [A technical primer on using encoded function calls](https://medium.com/linum-labs/a-technical-primer-on-using-encoded-function-calls-50e2b9939223)
-   [How To Decipher A Smart Contract Method Call](https://medium.com/@hayeah/how-to-decipher-a-smart-contract-method-call-8ee980311603)
-   [Advanced gas optimization tips for Solidity](https://coinsbench.com/advanced-gas-optimizations-tips-for-solidity-85c47f413dc5#:~:text=to%20gas%20specifications.-,Function%20names,-Solidity%20compiler%20reads)
-   [ERC-165: Standard Interface Detection](https://github.com/fullstack-development/blockchain-wiki/blob/main/EIPs/erc-165.md)

## Contract ABI Specification

1. Кто генерирует ABI? Что из себя представляет ABI?
    - Что обозначают поля: type, name, inputs, outputs, stateMutability в ABI?
2. Какие типы Solidity не являются частью ABI?
3. Как кодируются статические типы?
4. Как кодируются динамические типы?
5. Как кодируются вызовы функций?
6. Как кодируются ошибки?
7. Как кодируются события?
    - Как кодируются индексированные аргументы?

- [Solidity docs](https://docs.soliditylang.org/en/v0.8.19/abi-spec.html#contract-abi-specification)

## Call, staticcall, delegatecall, сalling other сontract

1. Для чего нужна низкоуровневая функция `call`, какие аргументы принимает?
2. Что возвращает функция `call`? Как декодировать возвращаемые значения?
3. Почему метод передачи эфира через `call` является предпочтительным?
4. Можно ли указывать лимит газа для транзакции при вызове `call`? Если да, то зачем указывать лимит газа и какие могут быть проблемы?
5. Для чего нужна низкоуровневая функция `staticcall`, в чем отличие от `call`?
6. Для чего нужна низкоуровневая функция `delegatecall`, в чем отличие от `call` и `staticcall`?
7. Какие возможны уязвимости связанные с `delegatecall`?
8. Как все эти функции связаны с `msg.value` и `msg.data`?
9. Как вызвать другой контракт с помощью этих функций? Как вызвать конкретную функцию другого контракта с аргументами?
10. По каким причинам низкоуровневый вызов других смарт-контрактов через `call` не является предпочтительным?
11. Приведи несколько примеров, когда без низкоуровневого вызова другого смарт-контракта через `call` и `delegatecall` не обойтись?

- [Solidity docs: ](https://docs.soliditylang.org/en/v0.8.19/introduction-to-smart-contracts.html#message-calls)
- [Solidity docs: Members of Address Types](https://docs.soliditylang.org/en/v0.8.11/units-and-global-variables.html#members-of-address-types)
- [Staticcall](https://eips.ethereum.org/EIPS/eip-214)
- [Delegatecall](https://eips.ethereum.org/EIPS/eip-7)
- [Learn Solidity lesson 34. Call, staticcall and delegatecall](https://medium.com/coinmonks/call-staticcall-and-delegatecall-1f0e1853340)
- [Solidity by example - Call](https://solidity-by-example.org/call/)
- [SWC-112 - delegatecall](https://swcregistry.io/docs/SWC-112)
- [What is gas limit](https://ethereum.org/en/developers/docs/gas/#what-is-gas-limit)

## Import

1. Идея ```import``` основана на **коцепции модулей**. В чем основная суть этого концепта?
2. Local **vs** external. В чем разница?
3. **Specific import**. Как этим пользоваться?
    - Нужно ли стремиться использовать **specific import** за место импорта всего, что есть в файле? Например, ```import "./Storage.sol"```
    - Что можно импортировать из файла ```Storage.sol``` для использования в контракте ```SpecificImport```?

  ```solidity
    /// SpecificImport.sol

    // SPDX-License-Identifier: MIT

    pragma solidity 0.8.17;

    import {
        // Что можно импортировать из контракта Storage?
    } from "./Storage.sol";

    contract SpecificImport {}
  ```

  ```solidity
    /// Storage.sol

    // SPDX-License-Identifier: MIT

    pragma solidity 0.8.17;

    address constant OWNER = 0x607B5e673D3ea42A0F85aDD6f529196500FC9E04;
    uint256 constant STORAGE_SLOT = 113;

    struct StorageList {
        address storageContract;
    }

    interface IStorage {
        function getStorageSlot() external view returns (uint256);
        function setStorageSlot(uint256 newStorageSlot) external;
    }

    library Array {
        function removeItem(uint256 index) external {}
    }

    enum STATUS {
        ACTIVE,
        PAUSED
    }

    error Storage_SetStorageLimit();

    // User-defined value type. More https://docs.soliditylang.org/en/v0.8.13/types.html#user-defined-value-types
    type UFixed256x18 is uint256;

    contract Storage {
        uint256 private _storageSlot = STORAGE_SLOT;
        uint256 _maxSetStorageCount;

        mapping(address => uint256) _counter;

        event StorageSlotUpdated(address sender);

        constructor(uint256 maxSetStorageCount) {
            _maxSetStorageCount = maxSetStorageCount;
        }

        function getStorageSlot() external view returns (uint256) {
            return _storageSlot;
        }

        function setStorageSlot(uint256 newStorageSlot) external {
            if (_counter[msg.sender] >= _maxSetStorageCount) {
                revert Storage_SetStorageLimit();
            }

            _storageSlot = newStorageSlot;

            _counter[msg.sender] += _counter[msg.sender];

            emit StorageSlotUpdated(msg.sender);
        }
    }

    function createStorage(uint256 maxSetStorageCount) returns (address)  {
        Storage storageImpl = new Storage(maxSetStorageCount);
        return address(storageImpl);
    }

   ```
4. **Import Aliases**. Ключевое слово **as**. Для чего это нужно?

- [Solidity docs](https://docs.soliditylang.org/en/v0.8.19/layout-of-source-files.html#importing-other-source-files)

## Library

1. Общие вопросы по библиотекам?
    - Можно ли отправить эфир библиотеке?
    - Есть ли в библиотеке переменные состояния?
    - Могут ли библиотеки содержать в себе ```receive()```, ```callback()``` или ```payable``` функции?
    - Могут ли библиотеки быть уничтожены через вызов ```selfdestruct()```?
    - Можно ли наследовать библиотеку и быть унаследованным?
2. Почему использование библиотеки дешевле, чем наследование контрактов?
3. Какие из перечисленных типов данных можно реализовать в библиотеке?
    - struct
    - enum
    - интерфейс
    - любую публичную переменную ```uint256 public myVar = 100;```
    - константу ```uint256 constant MY_CONSTANT = 100;```
    - модификатор
4. Какие есть варианты развертывания библиотеки? В чем разница между этими вариантами?
5. Можно ли объявлять в библиотеках функции без имплементации, как в интерфейсах?
  ```solidity
  function onConnect() public pure returns (bool);
  ```
6. Можно ли использовать ```Event``` в библиотеках? Могут ли возникнуть какие-то сложности в связи с этим?
7. Можно ли использовать модификаторы внутри библиотеки? Какие есть особенности с этим связанные?

- [Solidity docs](https://docs.soliditylang.org/en/v0.8.19/contracts.html#libraries)
- [Solidity Tutorial: all about Libraries](https://jeancvllr.medium.com/solidity-tutorial-all-about-libraries-762e5a3692f9)

## Creating Contracts

1. Какие есть два возможных способа создания контрактов?
2. Как создать контракт через new? Что на самом деле внутри происходит при создании таким способом?
3. Для чего нужен constructor? Сколько раз он вызывается? Можно ли делать перегрузку для constructor?
4. Как передать эфир при создании контракта?
5. Как создать контракт через create? Как создать контракт через create2? Какие различия между create и create2? Как у них происходит образование адреса контракта? Почему не безопасно создавать через create?
6. Можно ли каким-то образом передеплоить смарт-контракт на тот же адрес но с другим кодом?
7. Что такое Factory pattern в solidity? Какие есть типы? какие преимущества их использования? Когда необходимо их использовать?

- [Solidity by example](https://solidity-by-example.org/new-contract/)
- [EVM Dialect](https://docs.soliditylang.org/en/v0.8.18/yul.html#evm-dialect)
- [About create and create2](https://mixbytes.io/blog/pitfalls-of-using-cteate-cteate2-and-extcodesize-opcodes)
- [Factory patterns](https://blog.logrocket.com/cloning-solidity-smart-contracts-factory-pattern/)

## Keccak256

1. Что такое хэш-функции? Для чего они нужны?
2. Почему в solidity используется функция `keccak256`?
3. Как эта функция используется на уровне языка Solidity?
4. В каких случаях может понадобиться хэшировать данные на смарт-контракте? Как это сделать?
5. Есть ли различия между хэшированием и шифрованием?

- [Hashing Functions In Solidity Using Keccak256](https://medium.com/0xcode/hashing-functions-in-solidity-using-keccak256-70779ea55bb0)
- [Hashing with Keccak256](https://solidity-by-example.org/hashing/)
- [SHA-3 Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.202.pdf)

## Upgradeable contracts

1. Что такое обновляемые контракты? Для чего это нужно?
2. Можно ли мигрировать данные с одного смарт-контракта на другой?
    - Как это сделать?
    - Что мешает это сделать или в чем основная сложность?
3. Может ли помочь в обновление кода контрактов разделение хранения данных и логики реализации по разным контрактам?
4. Можно ли применять поведенческие шаблоны проектирования для разделения хранения данных и логики? Как применить шаблон Strategy?
    - Можно ли при использование паттерна Strategy обновить главный контракт логики?
5. Как применить шаблон проектирования Proxy для обновления контрактов?
    - Как работает прокси для смарт-контрактов? В чем основной концепт? Как в этом концепте участвует функция `delegateCall()`?
    - Что такое конфликт селекторов функций?
    - Для чего нужен стандарт ERC-1967: Proxy Storage Slots?
    - Что можно рассказать про Transparent proxy? Как он устроен? Для чего предназначался?
    - Что такое UUPS? В чем его отличие от Transparent?
    - Для чего был придуман Beacon Proxy?
    - Что такое EIP-1167? Для чего библиотека Clones от OpenZeppelin? Можно ли обновлять прокси, созданные при помощи этой библиотеки?
6. В чем основная идея Diamond Proxy? Для каких случаеа предназачался этот подход?
    - В чем отличие Inherited storage VS Diamond Storage VS App Storage?
7. В чем плюсы и минусы использования обновляемых контрактов?

## Вопросы по Foundry тестам

1. Что такое Foundry? Для чего используют forge, anvil, cast? Чем Foundry отличается от hardhat?
2. Как в Foundry реализовать процесс тестирования контракта? Что такое Forge Std библиотека? Для чего она нужна?
3. Какие префиксы бывают у имен тестовых функций?
4. Будет ли протестирована функция если указать ее видимость как internal или private?
5. Можно ли использовать вспомогательные контракты для хранения функций и переменных применяемые при тестированиии? Как это сделать?
6. Что делает функция setUp()? Она обязательная?
7. Что такое cheatcode и для чего они нужны?
  Назовите cheatcode под каждый случай:
 - Как вызвать метод от имени другого адреса?
 - Как проверить что метод ревертится с кастомной ошибкой?
 - Как проверить что метод генерирует ивент? Расскажи про boolean параметры в cheatcode  vm.expectEmit(topic1, topic2, topic3, topic4). Что за topic и для чего они нужны?
8. Как работают читкоды vm.roll и vm.warp?
9. Можно дебажить вместе с Foundry?
10. Поддерживает ли Foundry Fork testing? Какие есть способы?
11. Что такое Fuzz Testing? Для чего это может быть полезно?
12. Что такое Invariant Testing?
13. Что такое Differential Testing?
14. Как тестировать подписи сообщений?

- [Foundry documentation](https://book.getfoundry.sh/)