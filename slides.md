---
# https://sli.dev/custom/#frontmatter-configures
theme: 'seriph'
title: 'Smart Contract and DApp 101'

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

# DApp 개발 기초

## 오상문
## Feb. 2022

---

# DApp Architecture

<img class='h-11/12' src='/ethereum-dapp-architecture-2.png'/>


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

<div class='grid grid-cols-5'>
<div class='col-span-4'>

| Category |       | Tool/Service | Remarks |
| -------- | :---: | ---- | ------- |
| Editing      | <img class='h-10 w-10 filter invert' src='/remix/remix-icon.webp'/> | [Remix IDE](https://github.com/ethereum/remix-project) | [Web](https://remix.ethereum.org/) based |
| Build/Deploy | <img class='h-10 w-10' src='/truffle/truffle-icon.png'/> | [Truffle](https://github.com/trufflesuite/truffle)| JavaScript based |
|              |    | [Browine](https://github.com/eth-brownie/brownie) | Python based |
| Local Client | <img class='h-10 w-10' src='/ganache/ganache-favicon.png'/> | [Ganache](https://github.com/trufflesuite/ganache) | Ganache CLI |
| Mainnet/Testnet Gateway |   | [Infura](https://infura.io/) |   |
| Library      | <img class='h-10 w-10' src='/openzeppelin/oz-icon.svg'/> | [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) |   |
| Block Explorer | <img class='h-10 w-10' src='/etherscan/logos/etherscan-logo-light-circle.svg'/>  | [Etherscan](https://etherscan.io/) | Mainnet |
|                |   | [Etherscan/Rinkeby](https://rinkeby.etherscan.io/) |
| Wallet         | <img class='h-10 w-10' src='/metamask/metamask-icon.svg'/> | [MetaMask](https://metamask.io/) |

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
| -------------- | :------: | :-------: | --------------: | -------- |
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

<div class='grid grid-cols-5 gap-x-10'>
<div class='col-span-4 z-font-xs'>

| JSON-RPC | Description | web3.js |
| -------- | ----------- | ------- |
| [<tt>eth_chainId</tt>](https://eips.ethereum.org/EIPS/eip-695) | the chain ID of the current connected node | [<tt>web3.eth.getChainId()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getchainid) |
| [<tt>eth_blockNumber</tt>](https://eth.wiki/json-rpc/API#eth_blocknumber) | the number of most recent block | [<tt>web3.eth.getBlockNumber()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getblocknumber) |
| [<tt>eth_getBlockByNumber</tt>](https://eth.wiki/json-rpc/API#eth_getblockbynumber) | information about a block by block number.  | [<tt>web3.eth.getBlock()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getblock) |
| [<tt>web3_clientVersion</tt>](https://eth.wiki/json-rpc/API#web3_clientversion) |  the current client version | [<tt>web3.eth.getNodeInfo()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getnodeinfo) |
| [<tt>eth_coinbase</tt>](https://eth.wiki/json-rpc/API#eth_coinbase) | the coinbase address to which mining rewards will go | [<tt>web3.eth.getCoinbase()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getcoinbase) |
| [<tt>eth_accounts</tt>](https://eth.wiki/json-rpc/API#eth_accounts) | a list of addresses owned by client | [<tt>web3.eth.getAccounts()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getaccounts) |
| [<tt>eth_getBalance</tt>](https://eth.wiki/json-rpc/API#eth_getbalance) | the balance of the account of given address | [<tt>web3.eth.getBalance()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getbalance) |
| [<tt>eth_getTransactionCount</tt>](https://eth.wiki/json-rpc/API#eth_gettransactioncount) | the number of transactions sent from an address. (nonce) | [<tt>web3.eth.getTransactionCount()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#gettransactioncount) |
| [<tt>eth_getCode</tt>](https://eth.wiki/json-rpc/API#eth_getcode) | code at a given address  | [<tt>web3.eth.getCode()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#getcode) |
| [<tt>eth_signTransaction</tt>](https://eth.wiki/json-rpc/API#eth_signtransaction) | signs a transaction can be submitted to the network at a later time.| [<tt>web3.eth.signTransaction()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#signtransaction) |
| [<tt>eth_sendTransaction</tt>](https://eth.wiki/json-rpc/API#eth_sendtransaction) | creates new message call transaction or a contract creation  | [<tt>web3.eth.sendTransaction()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#sendtransaction) |
| [<tt>eth_sendRawTransaction</tt>](https://eth.wiki/json-rpc/API#eth_sendrawtransaction) |  creates new message call transaction or a contract creation for signed transactions.  | [<tt>web3.eth.sendSignedTransaction()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#sendsignedtransaction) |
| [<tt>eth_call</tt>](https://eth.wiki/json-rpc/API#eth_call) | executes a new message call immediately without creating a transaction on the block chain.| [<tt>web3.eth.call()</tt>](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html#call) |

</div>
<div class='col-span-1'>

* Resources <carbon-document/>
    * [JSON-RPC API](https://eth.wiki/json-rpc/API#json-rpc-methods)
    * [Web3.js API](https://web3js.readthedocs.io/en/v1.5.2/web3.html)
    * [Web3.py API](https://web3py.readthedocs.io/en/latest/web3.main.html)

</div>

</div>

---

# JSON-RPC Samples

<div class='grid grid-cols-5 gap-x-10'>
<div class='col-span-4'>

```bash
$ curl -sSX POST --data '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":21}' \
  https://rinkeby.infura.io/v3/${INFURA_PROJECT_ID} | jq .
{
  "jsonrpc": "2.0",
  "id": 21,
  "result": "0x4"
}
$ curl -sSX POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":22}' \
  https://rinkeby.infura.io/v3/${INFURA_PROJECT_ID} | jq .
{
  "jsonrpc": "2.0",
  "id": 22,
  "result": "0x9bced3"
}
$ curl -sSX POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest", false],"id":23}' \
  https://rinkeby.infura.io/v3/${INFURA_PROJECT_ID} | jq .
...
$ curl -sSX POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x0", false],"id":24}' \
  https://rinkeby.infura.io/v3/${INFURA_PROJECT_ID} | jq .
...
$ curl -sSX POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":25}' \
  https://rinkeby.infura.io/v3/${INFURA_PROJECT_ID} | jq .
{
  "jsonrpc": "2.0",
  "id": 25,
  "result": []
}
$
```

</div>
<div class='col-span-1'>

* Resources <carbon-document/>
    * [<tt>jq</tt>](https://stedolan.github.io/jq/) : <tt>sed</tt> for JSON data

</div>
</div>

---

# Ganache (Ganache CLI)
Local standalone client mainly for testing

<div class='grid grid-cols-4 gap-x-10'>
<div class='col-span-2'>

* Installing

```bash
$ npm install -g ganache
...
$ ganache --help

```
<div class='container py-4'/>

* Launching

```bash
$ ganache --chain.networkId 2016 \
  --chain.chainId 2016 \
  --server.host 127.0.0.1 \
  --server.port 8545 \
  --miner.defaultGasPrice 25000000000 \
  --miner.defaultTransactionGasLimit 400000000 \
  --miner.blockTime 0 \
  --wallet.totalAccounts 15 \
  --wallet.defaultBalance 10000 \
  --wallet.unlockedAccounts 0 1 2 3 4 \
  --database.dbPath run/ganache/data
  
  
```

<div class='container py-4'/>

* Resources <carbon-document/>
    * [Ganache startup options](https://github.com/trufflesuite/ganache#startup-options)

</div>
<div class='col-span-2'>

* Playing

```bash
$ truffle config get networks
{
  dashboard: {
    network_id: '*',
    networkCheckTimeout: 120000,
    url: 'http://localhost:24012/rpc',
    skipDryRun: true
  },
  development: {
    host: '127.0.0.1',
    port: 8545,
    network_id: 2016,
    gas: 300000000,
    gasPrice: 0,
    websockets: false
  },
  mainnet: { provider: [Function: provider], network_id: '1' },
  rinkeby: { provider: [Function: provider], network_id: '4' }
}
$ truffle console
truffle(development)>

...

truffle(development)> .exit
$
```

</div>
</div>

---

# Remix IDE
Best Solidity editor ever.

<div class='grid grid-cols-5 gap-x-10'>
<div class='col-span-2'>

* Intalling <tt>remixd</tt>

```bash

$ npm install @remix-project/remixd
...

$ npx remixd --help
```

<div class='container py-4'/>

* Launching <tt>remixd</tt>

```bash

$ npx remixd --shared-folder ./ \
  --remix-ide https://remix.ethereum.org
...
$
```

</div>
<div class='col-span-3'>

* Open <button class='animate-pulse'>https://remix.ethereum.org</button>
    * Click **<span class='underline'>Connect to Localhost</span>**
      under the **<span class='underline'>File</span>** section in the main pannel.
    * In the **<span class='underline'>File Explorers</span>**
      <img class='inline h-6 filter invert mx-2' src='/remix/file-manager.webp'/> on the left pannel,
      Click mouse right button on <span class='underline'>***<tt>contracts</tt>***</span> directory,
      select **<span class='underline'>New Folder</span>** menu to
      create ***<span class='underline'><tt>cryptopunks</tt></span>*** directory under it.
    * Click mouse right button on ***<span class='underline'><tt>contracts/cryptopunks</tt></span>*** directory
      select **<span class='underline'>New Folder</span>** menu
      to create ***<span class='underline'><tt>CryptoPunksMarket.sol</tt></span>*** file under it.
    * Copy the source from *<tt class='break-all'>https://github.com/larvalabs/cryptopunks/blob/master/contracts/CryptoPunksMarket.sol</tt>*
      and paste it into the ***<span class='underline'><tt>contracts/cryptopunks/CryptoPunksMarket.sol</tt></span>*** file.
    * Change left pannel to **<span class='underline'>Solidity Compiler</span>**
      by clicking <img class='inline h-6 filter invert mx-4' src='/remix/solidity-compiler.webp'/> in the leftmost bar
    * In the **<span class='underline'>Solidity Compiler</span>**
      <img class='inline h-6 filter invert mx-2' src='/remix/solidity-compiler.webp'/> on the left pannel,
      click **<span class='underline'>Compile CryptoPunksMarket.sol</span>** button to compile the contract source.
    
 

</div>
</div>


---

# Sample Contract (<span>1/4</span>)
<span class='text-sm'>https://github.com/larvalabs/cryptopunks/blob/master/contracts/CryptoPunksMarket.sol</span>

<div class='grid' grid='cols-7' gap='x-4'>
<div col='span-5'>

```solidity
pragma solidity ^0.4.8;
contract CryptoPunksMarket {

    // You can use this hash to verify the image file containing all the punks
    string public imageHash = "ac39af4793119ee46bbff351d8cb6b5f23da60222126add4268e261199a2921b";

    address owner;

    string public standard = 'CryptoPunks';
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;

    uint public nextPunkIndexToAssign = 0;

    bool public allPunksAssigned = false;
    uint public punksRemainingToAssign = 0;

    //mapping (address => uint) public addressToPunkIndex;
    mapping (uint => address) public punkIndexToAddress;

    /* This creates an array with all balances */
    mapping (address => uint256) public balanceOf;
    
    ...

```
</div>

<div col='span-2' text='sm'>

* Resources <carbon-document/>
    * [Structure of Contract](https://docs.soliditylang.org/en/v0.8.12/structure-of-a-contract.html)
        * State variables
        * Events
        * Functions
    * [Data types](https://docs.soliditylang.org/en/v0.8.12/types.html)
        * Boolean
        * Integer
        * Address
        * Fixed-sized byte array
        * Dynamically-sized byte array
        * String
        * Enum
        * Fixed-sized array
        * Dynamically-sided array
        * Mapping
    * [Visibility](https://docs.soliditylang.org/en/v0.8.12/contracts.html)

</div>

</div>

---

# Sample Contract (<span>2/4</span>)
<span class='text-sm'>https://github.com/larvalabs/cryptopunks/blob/master/contracts/CryptoPunksMarket.sol</span>

<div class='grid' grid='cols-7' gap='x-4'>
<div col='span-5'>

```solidity
    ...
    
    struct Offer {
        bool isForSale;
        uint punkIndex;
        address seller;
        uint minValue;          // in ether
        address onlySellTo;     // specify to sell only to a specific person
    }

    struct Bid {
        bool hasBid;
        uint punkIndex;
        address bidder;
        uint value;
    }

    // A record of punks that are offered for sale at a specific minimum value, and perhaps to a specific person
    mapping (uint => Offer) public punksOfferedForSale;

    // A record of the highest punk bid
    mapping (uint => Bid) public punkBids;

    mapping (address => uint) public pendingWithdrawals;
        
    ...
```

</div>
<div col='span-2' text='sm'>

* Resources <carbon-document/>
    * [Structs](https://docs.soliditylang.org/en/v0.8.12/types.html#structs)
    * [Mapping Types](https://docs.soliditylang.org/en/v0.8.12/types.html#mapping-types)
</div>
</div>

---

# Sample Contract (<span>3/4</span>)
<span class='text-sm'>https://github.com/larvalabs/cryptopunks/blob/master/contracts/CryptoPunksMarket.sol</span>

<div class='grid' grid='cols-7' gap='x-4'>
<div col='span-5'>

```solidity
    event Assign(address indexed to, uint256 punkIndex);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event PunkTransfer(address indexed from, address indexed to, uint256 punkIndex);
    event PunkOffered(uint indexed punkIndex, uint minValue, address indexed toAddress);
    event PunkBidEntered(uint indexed punkIndex, uint value, address indexed fromAddress);
    event PunkBidWithdrawn(uint indexed punkIndex, uint value, address indexed fromAddress);
    event PunkBought(uint indexed punkIndex, uint value, address indexed fromAddress, address indexed toAddress);
    event PunkNoLongerForSale(uint indexed punkIndex);

    /* Initializes contract with initial supply tokens to the creator of the contract */
    function CryptoPunksMarket() payable {
        //        balanceOf[msg.sender] = initialSupply;              // Give the creator all initial tokens
        owner = msg.sender;
        totalSupply = 10000;                        // Update total supply
        punksRemainingToAssign = totalSupply;
        name = "CRYPTOPUNKS";                                   // Set the name for display purposes
        symbol = "Ͼ";                               // Set the symbol for display purposes
        decimals = 0;                                       // Amount of decimals for display purposes
    }
    
    function setInitialOwner(address to, uint punkIndex) {
        if (msg.sender != owner) throw;
        if (allPunksAssigned) throw;
        if (punkIndex >= 10000) throw;
        ...
    }
    
    ...
```

</div>
<div col='span-2' text='sm'>

* Resources <carbon-document/>
    * [Functions](https://docs.soliditylang.org/en/v0.8.12/contracts.html#functions)
        * [View Functions](https://docs.soliditylang.org/en/v0.8.12/contracts.html#view-functions)
        * [Pure Functions](https://docs.soliditylang.org/en/v0.8.12/contracts.html#pure-functions)
    * [Events](https://docs.soliditylang.org/en/v0.8.12/contracts.html#events)
</div>
</div>

---

# Sample Contract (<span>4/4</span>)
<span class='text-sm'>https://github.com/larvalabs/cryptopunks/blob/master/contracts/CryptoPunksMarket.sol</span>

<div class='grid' grid='cols-4' gap='x-4'>
<div col='span-2'>

```solidity
    ...

    function setInitialOwners(address[] addresses, uint[] indices) {
      ...
    }

    function allInitialOwnersAssigned() {
      ...
    }

    function getPunk(uint punkIndex) {
      ...
    }

    function transferPunk(address to, uint punkIndex) {
      ...
    }

    function punkNoLongerForSale(uint punkIndex) {
      ...
    }

    function offerPunkForSale(uint punkIndex, uint minSalePriceInWei) {
      ...
    }
    
    
    ...
```

</div>
<div col='span-2'>
    
```solidity
    ...
    
    function offerPunkForSaleToAddress(uint punkIndex,
                uint minSalePriceInWei, address toAddress) {
      ...
    }

    function buyPunk(uint punkIndex) payable {
      ...
    }

    function withdraw() {
      ...
    }

    function enterBidForPunk(uint punkIndex) payable {
      ...
    }

    function acceptBidForPunk(uint punkIndex, uint minPrice) {
      ...
    }

    function withdrawBidForPunk(uint punkIndex) {
      ...
    }

}
```

</div>
</div>

---

# Solidity Features

<div class='my-8'/>

<div class='grid' grid='cols-5' gap='x-4'>
<div col='span-3' leading='loose'>

### <mdi-check-circle-outline text='2xl lime-400' align='middle' m='r-2'/> C, Java, JavaScript like syntax
<div m='l-12'>
Curly-brace block
</div>

<div class='my-8'/>

### <mdi-check-circle-outline text='2xl lime-400' align='middle' m='r-2'/> Statically typed
<div m='l-12'>
Compile-time type safety
</div>

<div class='my-8'/>

### <mdi-check-circle-outline text='2xl lime-400' align='middle' m='r-2'/> Imperative and object-oriented
<div m='l-12'>
Support <code>interface</code>, <code>abstract contract</code>, multiple <code>inheritance</code>
</div>

<div class='my-8'/>

### <mdi-check-circle-outline text='2xl lime-400' align='middle' m='r-2'/> Radicall growing
<div m='l-12'>
Breaking change in every major version upgrade from <tt>v0.5.0</tt>(Nov 2018) to <tt>v0.8.0</tt>(Dec 2020)
</div>

<div class='my-8'/>

### <mdi-check-circle-outline text='2xl lime-400' align='middle' m='r-2'/> Runs on EVM<span text='sm top'>Ethereum Virtual Machine</span>
<div m='l-12'>
Compiled into bytecode and executed as a number of EVM opcodes.
</div>


</div>
<div col='span-2'>

* Resources <carbon-document/>
    * https://github.com/ethereum/solidity
    * [Inheritance](https://docs.soliditylang.org/en/v0.8.12/contracts.html#inheritance)
    * [Interfaces](https://docs.soliditylang.org/en/v0.8.12/contracts.html#interfaces)
    * [Abstract Contracts](https://docs.soliditylang.org/en/v0.8.12/contracts.html#abstract-contracts)
    * [EVM](https://ethereum.org/en/developers/docs/evm/)
    * [EVM Opcodes](https://github.com/wolflo/evm-opcodes)
    * [Ethereum Yello Paper](https://ethereum.github.io/yellowpaper/paper.pdf)

</div>
</div>


---

# Solidity Types (<span>1/2</span>)

<div text='sm'>

| Type | Keyword | Operators | Fields/Methods | Literal |
| ---- | ------- | --------- | -------------- | ------- |
| Boolean | <span font='bold' text='teal-200'>`bool`</span> | <span whitespace='nowrap'>`!`, `&&`, `\|\|`, `==`, `!=`</span> |   |   | `tru`, `false`
| Unsigned Integer | <span font='bold' text='teal-200'>`uint8`, `uint16`, `uint24`, ..., `uint248`, `uint256`, `uint`</span> | <span whitespace='nowrap'>`<`, `<=`, `==`, `>=`, `>`,<br/>`&`, `\|`, `^`, `~`, `<<`, `>>`,<br/>`+`, `-`, `*`, `/`, `%`, `**`</span> |   | <span text='xs'>`100`, `0x2eff`, `300_000_000`, `2e10`, `2.1e10`</span> |
| Signed Integer | <span font='bold' text='teal-200'>`int8`, `int16`, `int24`, ..., `int248`, `int256`, `int`</span> | <span whitespace='nowrap'>`<`, `<=`, `==`, `>=`, `>`,<br/>`&`, `\|`, `^`, `~`, `<<`, `>>`,<br/>`+`, `-`, `*`, `/`, `%`, `**`</span> |   |   |
| Address | <span font='bold' text='teal-200'>`address`</span> |    | `balance`, `code`, `call()`, `delegatecall()`, `staticcall()` | <span text='xs'>`0xdCad3a6d3569DF655070DEd06cb7A1b2Ccd1D3AF`</span> |
| Address Payable | <span font='bold' text='teal-200'>`address payable`</span> |    | `balance`, `code`, `call()`, `delegatecall()`, `staticcall()`, `transfer()`, `send()` |   |
| Fixed-sized Byte Array | <span font='bold' text='teal-200'>`byte1`, `byte2`, `byte3`, ... `byte31`, `byte32`</span> | <span whitespace='nowrap'>`<`, `<=`, `==`, `>=`, `>`,<br/>`&`, `\|`, `^`, `~`, `<<`, `>>`,<br/>`x[k]`</span> | `length` |   |
 
</div>

---

# Solidity Types (<span>2/2</span>)

<div class='text-sm'>

| Type | Keyword | Operators | Fields/Methods | Literal |
| ---- | ------- | --------- | -------------- | ------- |
| (Dynamically-sized) Byte Array | <span font='bold' text='teal-200'>`bytes`</span> | <code>*x*[*k*]</code>  | `push()`, <code>push(*x*)</code>, `pop()`, <code>bytes(*string*)</code>, `concat()` |   |
| String | <span font='bold' text='teal-200'>`string`</span> |    | `concat()`  | <span class='text-xs'>`'foo'`, `"foo"`, `'foo\nbar'`, `'foo\\bar'`</span> |
| Array | <span font='bold' text='teal-200'><code>*T*[*n*]</code>, <code>*T*[]</code>, <code>*T*[*n*][*m*]</code>, <code>*T*[*n*][]</code>, <code>*T*[][*m*]</code>, <code>*T*[][]</code></span> | <code>*a*[*k*]</code>, <code>*a*[*m*:*n*]</code> | `length`, `push()`, <code>push(*x*)</code>, `pop()` | <span text='xs'>[1, 2, 3]</span>  |
| Mapping | <span font='bold' text='teal-200'><code>mapping(*key-type* => *value-type*)</code></span> | <code>*m*[*key*]</code> |
| Struct | <span font='bold' text='teal-200'><code>struct *T* { ... }</code></span> |   |   |   |

</div>

---

# Deployment and ...



---

