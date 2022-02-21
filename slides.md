---
# https://sli.dev/custom/#frontmatter-configures
theme: 'seriph'
title: ''

download: true
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

<div class='grid grid-cols-5'>
<div class='col-span-3'>
<div class='inline-table'>

| Category |    | Tool/Service | Remarks |
| -------- | -- |---- | ------- |
| Editing  |   | [Remix IDE](https://github.com/ethereum/remix-project) | [Web](https://remix.ethereum.org/) based |
| Build/Deploy | <logos-truffle class='text-3xl' /> | [Truffle](https://github.com/trufflesuite/truffle)| JavaScript based |
|              |    | [Browine](https://github.com/eth-brownie/brownie) | Python based |
| Local Client | <logos-ganache class='text-3xl' /> | [Ganache](https://github.com/trufflesuite/ganache) | Ganache CLI |
| Mainnet/Testnet Gateway |   | [Infura](https://infura.io/) |   |
| Library      |   |[OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) |   |
| Block Explorer |   | [Etherscan](https://etherscan.io/) | Mainnet |
|                |   | [Etherscan/Rinkeby](https://rinkeby.etherscan.io/) |
| Wallet         | <logos-metamask class='text-3xl' /> | [MetaMask](https://metamask.io/) |

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

<div class='grid grid-cols-5 gap-x-10'>
<div class='col-span-3 col-start-1'>

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

<div class='col-span-2 col-start-4'>

* Resources
    * [Truffle documentation](https://trufflesuite.com/docs/truffle/)
    * [Truffle commands](https://trufflesuite.com/docs/truffle/reference/truffle-commands.html)

</div>
</div>

----

# Truffle Configuration

### Prerequisite

<div class='grid grid-cols-4 gap-x-10'>
<div class='col-span-2 col-start-1 z-font-sm'>

* Setup **Node.js** project
    ```bash
    $ npm init -y
    $ npm install -D @truffle/hdwallet-provider
    ...
    $ cat package.json
    ```

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

</div>

<div class='col-span-2 z-font-sm'>

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
    
    * Resources
        * [Getting Started With Infura](https://blog.infura.io/getting-started-with-infura-28e41844cc89/)

</div>
</div>
---

# Truffle Configuration

* <tt>truffle-config.js</tt>

<div class='grid grid-cols-5 gap-x-10'>
<div class='col-span-3 z-font-sm'>

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

</div>

<div class='col-span-2 z-font-sm'>

* Resources <carbon-document/>
    * [Truffle configuration reference](https://trufflesuite.com/docs/truffle/reference/configuration.html)
    * <tt>[@truffle/hdwallet-provider](https://github.com/trufflesuite/truffle/tree/develop/packages/hdwallet-provider)</tt>
    * [BIP-32 : Hierarchical Deterministic Wallets(HD Wallets](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
    * [BIP-39 : Mnemonic code for generating deterministic keys](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)
    * [Ethereum 201: HD Wallets](https://wolovim.medium.com/ethereum-201-hd-wallets-11d0c93c87f7)
    
</div>
</div>

---

# Testnets

<div class='grid grid-cols-5 gap-x-10'>
<div class='col-span-3 col-start-1 z-font-sm'>

| Network(Chain) | Chain ID | Consensus | Avg. Block Time | Explorer |
| -------------- | -------- | --------- | --------------- | -------- |
| Mainnet        | 1        | PoW       | 15 min.         | [Etherscan](https://etherscan.io/) |
| Ropsten        | 3        | PoW       | 30 sec.         | [Etherscan/Ropsten](https://ropsten.etherscan.io/) |
| [Rinkeby](https://www.rinkeby.io/) | 4 | PoA | 15 sec.  | [Etherscan/Rinkeby](https://rinkeby.etherscan.io/) |
| [Kovan](https://kovan-testnet.github.io/website/) | 42 | PoA | 4 sec. | [Etherscan/Kovan](https://kovan.etherscan.io/) |

</div>

<div class='col-span-2 col-start-1 mt-8 z-font-sm'>

### Faucets

| Network | Faucet |
| ------- | ------ |
| Ropsten | https://faucet.ropsten.be/ |
| Rinkeby | https://faucet.rinkeby.io/ |
|         | https://faucets.chain.link/rinkeby |
| Kovan   | https://faucet.kovan.network/ |
|         | https://ethdrop.dev/ |

</div>

<div class='col-span-2 col-start-4 row-full row-start-1'>

* Resources
    * [Chainlist](https://chainlist.org/)
    * [EIP-155: Simple replay attack protection](https://eips.ethereum.org/EIPS/eip-155)

</div>
</div>

---

# Truffle Console / Rinkeby

<div class='grid grid-cols-5 gap-x-10'>
<div class='col-span-4 z-font-sm'>

```bash
$ truffle console --network rinkeby
...
truffle(rinkeby)>
truffle(rinkeby)> web3.version
'1.5.3'
truffle(rinkeby)> web3.eth.net.getId()
4
truffle(rinkeby)> web3.eth.getBlockNumber().then(n => parseInt(n).toLocaleString())
'10,206,304'
truffle(rinkeby)> web3.eth.getBlock('latest')
...
truffle(rinkeby)> web3.eth.getBlock('latest').then(b => new Date(b.timestamp * 1000))
2022-02-21T10:46:03.000Z
truffle(rinkeby)> web3.eth.getBlock(0)
...
truffle(rinkeby)> web3.eth.getBlock(0).then(b => new Date(b.timestamp * 1000))
2017-04-12T14:59:06.000Z
truffle(rinkeby)> web3.eth.getCoinbase()
'0xb009cd53957c0d991cabe184e884258a1d7b77d9'
truffle(rinkeby)> web3.eth.getBalance(_).then(b => parseInt(b).toLocaleString())
'101,000,000,000,000,000'
truffle(rinkeby)> web3.eth.isMining()
false
ruffle(rinkeby)> web3.eth.net.getPeerCount()
100
truffle(rinkeby)> web3.eth.getAccounts()
...
truffle(rinkeby)> accounts
...
truffle(rinkeby)> web3.eth.getBalance(accounts[0])
'101000000000000000'
truffle(rinkeby)> web3.eth.getBalance(accounts[1])
'100000000000000000'
truffle(rinkeby)>
```
</div>

</div>

---

# JSON-RPC and Web3.js

| JSON-RPC | Description | web3.js |
| -------- | ----------- | ------- |
| [<tt>web3_clientVersion</tt>](https://eth.wiki/json-rpc/API#web3_clientversion) |  the current client version | [<tt>web3.eth.getNodeInfo()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getnodeinfo) |
| [<tt>eth_chainId</tt>](https://eips.ethereum.org/EIPS/eip-695) | the chain ID of the current connected node | [<tt>web3.eth.getChainId()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getchainid) |

---

# Ganache (Ganache CLI)

* Local standalone client

---

# Standards


