# 이더리움 컨트랙트 포팅 <a id="porting-ethereum-contract"></a>

대부분의 경우 Klaytn에서 이더리움 컨트랙트를 수정 없이 사용할 수 있습니다. 그러나 다음 두 가지 문제에 유의하셔야 합니다.

## 솔리디티 지원 <a id="solidity-support"></a>

* Baobab 네트워크는 현재 **London** Ethereum Virtual Machine(EVM)과 호환 가능합니다.
* Cypress 네트워크는 현재 **Constantinople** Ethereum Virtual Machine(EVM)과 호환 가능합니다.

{% hint style="success" %}
v1.7.0 프로토콜 업그레이드 - **Istanbul** 하드포크 및 Klaytn의 자체 사항들을 포함하는 비호환 변경이 적용됩니다. Baobab 네트워크의 경우 블록 번호 `#75373312`부터 적용됩니다. Cypress mainnet will be subject to the same protocol upgrade in the next version.

v1.7.3 프로토콜 업그레이드 - **London** 하드 포크의 Base Fee를 포함한 비호환 변경이 적용됩니다. Baobab 네트워크의 경우 블록 번호 `#80295291`부터 적용됩니다. Cypress mainnet will be subject to the same protocol upgrade in the next version.
{% endhint %}

다른 EVM 버전들과의 하위 호환성은 보장되지 않습니다. 그렇기 때문에 프로토콜 업그레이드 상태에 따라 올바른 타겟 옵션과 함께 솔리디티 코드를 컴파일하는 것이 좋습니다.
* Baobab: --evm-version london
* Cypress: --evm-version constantinople
* 그 외(private/service chain): 프로토콜 업그레이드 상태에 따라 결정

자세한 내용은 [How to set the EVM version of solc](https://solidity.readthedocs.io/en/latest/using-the-compiler.html#setting-the-evm-version-to-target)를 참고해주세요.


예시는 다음과 같습니다.

```
$ solc --evm-version london contract.sol
```

## 분리된 키 쌍 <a id="decoupled-key-pairs"></a>

Klaytn은 [키 쌍을 주소에서 분리합니다](../klaytn/design/accounts.md#decoupling-key-pairs-from-addresses). 사용자가 [계정을 업데이트](../klaytn/design/transactions/basic.md#txtypeaccountupdate)할 경우, 특정 계정의 개인키는 다른 것으로 대체됩니다. 대부분의 경우 이는 비즈니스 로직에 영향을 미치지 않습니다. 그러나 비즈니스 로직에 ecrecover가 포함될 경우, validateSender를 고려해야 합니다. 자세한 내용은 [여기](precompiled-contracts/precompiled-contracts.md)를 참조하세요.
