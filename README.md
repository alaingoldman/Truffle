## Setting up Mist


### Dependencies

* `Node.js` v7.x (use the prefered installation method for your OS)
* `Meteor` javascript app framework
* `Yarn` package manager
* `Electron` v1.4.15 cross platform desktop app framework
* `Gulp` build and automation system

```
$ curl https://install.meteor.com/ | sh
$ curl -o- -L https://yarnpkg.com/install.sh | bash
$ yarn global add electron@1.4.15
$ yarn global add gulp
```

### Mist setup

Now you're ready to initialize Mist for development:

```
$ cd ~/Desktop
$ mkdir mist_test && cd mist_test
$ git clone https://github.com/ethereum/mist.git
$ cd mist
$ yarn
$ cd interface && meteor --no-release-check
```
While meteor is running open another terminal window and go back to the folder you created `/Desktop/mist_test/mist`

Now lets run electron for the first time:

```
$ cd ~/Desktop/mist_test/mist
$ yarn dev:electron
```
![Alt text](https://scontent-iad3-1.xx.fbcdn.net/v/t1.0-9/21686188_1989133898010328_7137568609932587406_n.jpg?oh=42af6e65337729d4b88469533c99eec4&oe=5A55EC48)

* Launch the application in test network
* Wait for past blocks to download
* Write down your ethereum address (we need this later)
* `0x6a6401AEb4a3beb93820904E761b0d86364bb39E` 
* This is my test ethereum wallet, every time you see this wallet in my code you should replace this with your wallet

### Were done with mist!

If you want to read more about mist [you can do so here.](https://github.com/ethereum/mist "Ethereum's github") For now close both terminals. The one running meteor and the one running mist/electron.
* Make sure to close both terminals

# Truffle setup

Lets install truffle and get testrpc

* `$ npm install -g truffle`
* `$ npm install -g ethereumjs-testrpc`

Now we can go ahead and start a truffle app

* `$ cd ~/Desktop`
* `$ mkdir truftest`
* `$ cd truftest`
* `$ truffle unbox react`

So we can start the servers

* `$ testrpc`


![Alt text](https://scontent-lga3-1.xx.fbcdn.net/v/t1.0-9/21617956_1989181674672217_2485686153056585116_n.jpg?oh=7879be943e0516d39f5a5269cda09358&oe=5A3A8DB9)

in another terminal go into your truffle folder
* `$ cd ~/Desktop/truftest`
* `$ truffle compile`
* `$ truffle migrate`
* `$ npm start`

Now we can play with the solidity and react on the site.

### Solidity
lets mess around with the solidity contract and set it to use strings.
```
pragma solidity ^0.4.2;

contract SimpleStorage {
  string testVal = "testval default value";

  function set(string x) {
    testVal = x;
  }

  function get() constant returns (string) {
    return testVal;
  }
}

```

Now lets go ahead and make our app have a touch event to trigger both a get and a set function from that contract. `App.js`
```
import React, { Component } from 'react'
import SimpleStorageContract from '../build/contracts/SimpleStorage.json'
import getWeb3 from './utils/getWeb3'

import './css/oswald.css'
import './css/open-sans.css'
import './css/pure-min.css'
import './App.css'

const contract = require('truffle-contract')
const simpleStorage = contract(SimpleStorageContract)

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

  getValue(){
    simpleStorage.setProvider(this.state.web3.currentProvider)
    var simpleStorageInstance
    console.log("...getting data");
    this.state.web3.eth.getAccounts((error, accounts) => {
      simpleStorage.deployed().then((instance) => {
        simpleStorageInstance = instance
        return simpleStorageInstance.get.call({from: accounts[0]})
      }).then((result) => {
        console.log("result", result);
      })
    })

  }

  setValue(){
    simpleStorage.setProvider(this.state.web3.currentProvider)
    var simpleStorageInstance
    console.log("...setting data");
    this.state.web3.eth.getAccounts((error, accounts) => {
      simpleStorage.deployed().then((instance) => {
        simpleStorageInstance = instance
        return simpleStorageInstance.set("new string dude", {from: accounts[0], gas: 100000})
      }).then((result) => {
        console.log("result", result);
      })
    })
  }


  render() {
    return (
      <div>
        <div id="get" onClick={this.getValue.bind(this)}></div>
        <div id="set" onClick={this.setValue.bind(this)}></div>
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

# Free Rinkeby ether
In order to use the test network we have to have test ether which we cant just print out. 

* Log into `github`, create an account if you dont have one
* Head to [https://gist.github.com/](https://gist.github.com/)
* Create a new public gist and only write put your ethereum address in there
* [Check out my example](https://gist.github.com/alaingoldman/6fe77a7a71b6c706ab31f2512247d870)
* [Get free Ether from https://faucet.rinkeby.io/](https://faucet.rinkeby.io/)
* Paste the url from the gist file into the faucet page



# Connecting to Rinkeby 

Make sure to close the terminal 

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

Now we can turn on the server and we're done
```
$ npm start 
```
