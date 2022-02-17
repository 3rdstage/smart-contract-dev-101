---
# https://sli.dev/custom/#frontmatter-configures
theme: 'default'
title: ''

highlighter: prism   # shiki
colorSchema: 'dark'
aspectRatio: '16/9'
canvasWidth: 1200
defaults:
  layout: 'default'
---

---
layout: cover
---

# Smart Contract 개발 기본

Hello, World!

---

# Images

<img src='/collections.png' style='width:400px' class='bg-current border-sm'/>

    

<img src='/mindmap.svg' style='width:400px' class='bg-current'/>

---

# DApp




---

# DApp 개발 Lifecyle

* 응용 분석/설계
* Smart Contract **구현**
* Smart Contract **Audit**
* Smart Contract **Local배포/단위테스트**
* Smart Contract **Testnet배포/단위테스트**
* 응용 **구현**
* 응용 **배포/단위테스트**

---

# DApp 개발 환경/도구

## Smart Contract

<div class='grid grid-cols-5' markdown='1'>
<div class='col-span-3' markdown='1'>
<div class='inline-table' markdown='1'>

| Category | Tool/Service | Remarks |
| -------- | ---- | ------- |
| Editing  | [Remix IDE](https://github.com/ethereum/remix-project) | [Web](https://remix.ethereum.org/) based |
| Build/Deploy | [Truffle](https://github.com/trufflesuite/truffle) | JavaScript based |
|              | [Browine](https://github.com/eth-brownie/brownie) | Python based |
| Local Client | [Ganache](https://github.com/trufflesuite/ganache) | Ganache CLI |
| Mainnet/Testnet Gateway | [Infura](https://infura.io/) |   |
| Library      | [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) |   |
| Block Explorer | [Etherscan](https://etherscan.io/) | Mainnet |
|                | [Etherscan/Rinkeby](https://rinkeby.etherscan.io/) |

</div>
</div>
</div>

----

# Truffle

* Installing Node.js
    ```bash
    $ node --version
    ```

* Installing Truffle
    ```bash
    $ npm ls -g truffle         # check whether or not Truffle in installed in global scope
    $ npm uninstall -g truffle  # uninstalled currently installed Truffle
    $ npm install -g truffle
    ```

* Creating Truffle Project
    ```bash
    $ mkdir smart-contract-101 && cd smart-contract-101
    $ truffle init
    ...
    $ ls
    /     ../     contracts/    migrations/   test/     truffle-config.js
    $ cat truffle-config.js | less
    ...
    ```

---

# Truffle Commands

<div class='grid grid-cols-5 gap-x-10' markdown='1'>
<div class='col-span-3 col-start-1' markdown='1'>

```bash {1,4,8,10,14,16}
$ truffle help
Truffle v5.4.22 - a development framework for Ethereum
    
Usage: truffle <command> [options]

Commands:
  build     Execute build pipeline (if configuration present)
  compile   Compile contract source files
  ...
  console   Run a console with contract abstractions and commands available
  ...
  deploy    (alias for migrate)
  ...
  networks  Show addresses for deployed contracts on each network
  ...
  test      Run JavaScript and Solidity tests
  ...
See more at http://trufflesuite.com/docs
```

```bash
$ truffle help migrate
...
$ truffle help test
...
```
</div>

<div class='col-span-2 col-start-4' markdown='1'>

* Resources
    * [Truffle documentation](https://trufflesuite.com/docs/truffle/)
    * [Truffle commands](https://trufflesuite.com/docs/truffle/reference/truffle-commands.html)

</div>
</div>

----

# Truffle Configuration

<style>
.z-setup .slidev-code {
  font-size: 0.65em !important;
  line-height: 1.3 !important;
}

.z-config .slidev-code {
  font-size: 0.7em !important;
}
</style>

<div class='grid grid-cols-5 gap-x-10' markdown="1">
<div class='col-span-2 col-start-1 z-setup' markdown="1">

### Prerequisite

* Setup **Mnemonic**
    * macOS
    ```bash
    $ env | grep BIP39_MNEMONIC
    ...
    $ echo 'export BIP39_MNEMONIC="..."' >> ~/.profile
    $ cat ~/.profile
    ...
    ```
    * Windows
    ```dos
    > setx BIP39_MNEMONIC="..."
    ```

* Setup **Infura Project**
    * macOS
    ```bash
    $ env | grep INFURA_PROJECT_ID
    ...
    $ echo 'export INFURA_PROJECT_ID=...' >> ~/.profile
    $ cat ~/.profile
    ...
    ```
    * Windows
    ```dos
    > setx INFURA_PROJECT_ID=...
    ```
</div>

<div class='col-span-3 z-config' markdown='1'>


</div>
</div>
---

# Truffle Configuration

* `truffle-config.js`

```javascript
const HDWalletProvider = require("@truffle/hdwallet-provider");
const mnemonic = process.env.BIP39_MNEMONIC;

module.exports = {

  networks: {
    development: {
      host: '127.0.0.1',
      port: 8545,
      network_id: 2016,
      gas: 3E8,
      gasPrice: 0,
      websockets: false
    },
    
    mainnet: {
      provider: () => new HDWalletProvider(
        mnemonic, "https://mainnet.infura.io/v3/" + process.env.INFURA_PROJECT_ID),
      network_id: '1'
    },

    rinkeby: {
      provider: () => new HDWalletProvider(
        mnemonic, "https://rinkeby.infura.io/v3/" + process.env.INFURA_PROJECT_ID),
      network_id: '4',
    }
  }
}
```

---

# Testnets




---

# Truffle Console / Rinkeby




---

# JSON-RPC and Web3.js




---

# Ganache (Ganache CLI)

* Local standalone client

---
