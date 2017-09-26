# Jumping into Truffle and rinkeby
This tutorial was made with truffle v3.4.9 (core: 3.4.8) and solidity v0.4.15.


## Truffle setup

### Windows

If you're a Windows user, we recommend installing and using Truffle via Windows PowerShell or [Git BASH](https://git-for-windows.github.io/). These two shells provide features far beyond the standard Command Prompt, and will make your life on the command line much easier.

### Non-windows


lets install testrpc. 

Testrpc is a Node.js based Ethereum client for testing and development. It uses ethereumjs to simulate full client behavior and make developing Ethereum applications much faster. It also includes all popular RPC functions and features (like events) and can be run deterministically to make development a breeze. [https://github.com/ethereumjs/testrpc](https://github.com/ethereumjs/testrpc)

* `$ npm install -g ethereumjs-testrpc`
* `$ testrpc`

![Alt text](https://scontent-lga3-1.xx.fbcdn.net/v/t1.0-9/21617956_1989181674672217_2485686153056585116_n.jpg?oh=7879be943e0516d39f5a5269cda09358&oe=5A3A8DB9)

Now we can go ahead and install truffle and create the core of our app.

* `$ npm install -g truffle`
* `$ cd ~/Desktop`
* `$ mkdir truftest`
* `$ cd truftest`
* `$ truffle unbox react`

Here we are using a shortcut `truffle unbox react` which does an initialization and adds the necessary modules for react. If you want to have a pure instalation you should instead run `truffle init` inside of the folder you created.


in another terminal go into your truffle folder.
* `$ cd ~/Desktop/truftest`
* `$ truffle compile`
* `$ truffle migrate`
* `$ npm start`

Compile and migrate and deploying the default solidity contracts.

### Solidity
lets mess around with the solidity contract. We are going to be making a contract called MySale. `truftest/contracts/MySale.sol`

```
$ truffle create contract MySale
```
Now we have to add a migration file for deploying this contract to the evm. Create a file called `truftest/migrations/3_add_Sale.js`

Note that the filename is prefixed with a number and is suffixed by a description. The numbered prefix is required in order to record whether the migration ran successfully. The suffix is purely for human readability and comprehension.

Ok lets fill out the migration.
```
var MySale = artifacts.require("./MySale.sol");

module.exports = function(deployer) {
  deployer.deploy(MySale);
};
```
Now you will be provided with a solidity file with no functions. I went ahead and filled it in with 3 rudimentary functions.
`truftest/contracts/MySale.sol`
```
pragma solidity ^0.4.4;

contract MySale {
  mapping(address => uint) balance;
  uint total_coins = 1;

  function printCoin(uint howMuch) public{
  	balance[msg.sender] += howMuch;
  	total_coins += howMuch;
  }

  function allCoins() constant public returns(uint){
  	return total_coins;
  }
  
  function myCoin() constant public returns(uint){
   return balance[msg.sender];
  }
}
```

Now lets go ahead and make our app have a touch event to trigger both a get and a set function from that contract. `App.js`
```
import React, { Component } from 'react'
import MySale from '../build/contracts/MySale.json'
import getWeb3 from './utils/getWeb3'

import './css/oswald.css'
import './css/open-sans.css'
import './css/pure-min.css'
import './App.css'

const contract = require('truffle-contract')
const mySale = contract(MySale)

class App extends Component {
  constructor(props) {
    super(props)

    this.state = {
      storageValue: 0,
      web3: null
    }
  }

  componentWillMount() {
    getWeb3
    .then(results => {
      this.setState({
        web3: results.web3
      })
    })
    .catch(() => {
      console.log('Error finding web3.')
    })
  }

  coinCount(){
    mySale.setProvider(this.state.web3.currentProvider)
    var mySaleInstance

    console.log("...getting data");
    this.state.web3.eth.getAccounts((error, accounts) => {
      mySale.deployed().then((instance) => {
        mySaleInstance = instance
        return mySaleInstance.allCoins.call({from: accounts[0]})
      }).then((result) => {
        console.log("result", result);
      })
    })

  }

  printCoin(){
    mySale.setProvider(this.state.web3.currentProvider)
    var mySaleInstance
    console.log("...setting data");
    this.state.web3.eth.getAccounts((error, accounts) => {
      mySale.deployed().then((instance) => {
        mySaleInstance = instance
        return mySaleInstance.printCoin(4, {from: accounts[0]})
      }).then((result) => {
        console.log("result", result);
      })
    })
  }


  render() {
    return (
      <div>
        <div id="get" onClick={this.coinCount.bind(this)}></div>
        <div id="set" onClick={this.printCoin.bind(this)}></div>
      </div>
    );
  }
}

export default App
```
I added some css to #get and #set so I can see those div's as buttons. You can find this in `App.css`

```
#set{
  height:50px;
  width:50px;
  background-color:red;
  float:left;
  margin: 20px;
  cursor: pointer;
}
#get{
  height:50px;
  width:50px;
  background-color:blue;
  float:left;
  margin: 20px;
  cursor: pointer;
}
```
### Dependencies

* [Node.js x7.x](https://nodejs.org/en/) (use the prefered installation method for your OS)
* [Meteor](https://www.meteor.com/install) javascript app framework
* [Yarn](https://yarnpkg.com/en/) package manager
* [Electron](https://electron.atom.io/) v1.4.15 cross platform desktop app framework
* [Gulp](https://gulpjs.com/) build and automation system

```
$ curl https://install.meteor.com/ | sh
$ curl -o- -L https://yarnpkg.com/install.sh | bash
```

* Make sure you have `yarn` installed. you can get it [here](https://yarnpkg.com/lang/en/docs/install/)

```
$ yarn global add electron@1.4.15
$ yarn global add gulp
```

### Mist setup

```
 $ cd ~/Desktop
 $ mkdir mist_test && cd mist_test
 $ git clone https://github.com/ethereum/mist.git
 $ cd mist
 $ yarn
 $ cd interface && meteor --no-release-check
```
 
Now you're ready to initialize Mist for development:


While meteor is running open another terminal window and go back to the folder you created `/Desktop/mist_test/mist`

Now lets run electron for the first time:

```
$ cd ~/Desktop/mist_test/mist
$ yarn dev:electron
```
![Alt text](https://scontent-iad3-1.xx.fbcdn.net/v/t1.0-9/21686188_1989133898010328_7137568609932587406_n.jpg?oh=42af6e65337729d4b88469533c99eec4&oe=5A55EC48)

* Launch the application in test network
* Wait for past blocks to download

![Rinkeby download](https://scontent-lga3-1.xx.fbcdn.net/v/t1.0-9/21766333_1990102794580105_1591728995222655320_n.jpg?oh=00e1a442ba4ccbae870cc2b54e7fad3b&oe=5A4FB9EC)

* (on the bottom left Mist is downloading the past blocks, wait for this to update.)
* Write down your ethereum address, we will need this laster in this tutorial
* `0x6a6401AEb4a3beb93820904E761b0d86364bb39E` 

### Were done with mist!

If you want to read more about mist [you can do so here.](https://github.com/ethereum/mist "Ethereum's github") For now close both terminals. The one running meteor and the one running mist/electron.
* Make sure to close both terminals

# Free Rinkeby ether
In order to use the test network we have to have test ether which we cant just print out. 

* Log into `github`, create an account if you dont have one
* Head to [https://gist.github.com/](https://gist.github.com/)
* Create a new public gist and only write put your ethereum address in there
* [Check out my example](https://gist.github.com/alaingoldman/6fe77a7a71b6c706ab31f2512247d870)
* [Get free Ether from https://faucet.rinkeby.io/](https://faucet.rinkeby.io/)
* Paste the url from the gist file into the faucet page



# Connecting to Rinkeby 

Make sure to close the terminal.

First, start geth with Rinkeby and make sure that the correct APIs for Truffle are enabled.
```
$ geth --rinkeby --rpc --rpcapi db,eth,net,web3,personal --unlock="0x6a6401AEb4a3beb93820904E761b0d86364bb39E" --rpccorsdomain http://localhost:3000
```
Please dont forget to replace my wallet with the wallet you got from mist/electron.

This will ask you for the password you gave mist/electron.

Next, we need to add Rinkeby to our truffle config file. If we open `truffle.js` in our contract code we’ll see something like:

```
module.exports = {
  rpc: {
    host: 'localhost',
    port: '8545'
 },
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*" // Match any network id
    },
    rinkeby: {
      host: "localhost", // Connect to geth on the specified
      port: 8545,
      from: "0x6a6401AEb4a3beb93820904E761b0d86364bb39E", // default address to use for any transaction Truffle makes during migrations
      network_id: 4,
      gas: 4612388 // Gas limit used for deploys
    }
  },
};
```
Now we just have to migrate the contract onto rinkeby. This is going to ask for the password you gave mist/electron.
```
$ truffle migrate --network rinkeby
```
If you want to view the current state of your contracts on rinkeby test network go to this url. 

(Replace my wallet with yours. [My address's transactions.](https://rinkeby.etherscan.io/address/0x6a6401aeb4a3beb93820904e761b0d86364bb39e) )
```
https://rinkeby.etherscan.io/address/YOUR-WALLET-HERE
```

Now we can turn on the server and we're done.
```
$ npm start 
```
