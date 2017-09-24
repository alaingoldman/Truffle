## 9/25 truffle class prep

In oder to be prepared you guys/gals should install mist/electron before class. This takes about 30 minutes. This guide is aimed towards apple and linux. If you are on windows you can check mist/electron's [official documentation here.](https://github.com/ethereum/mist) 

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
```

* Make sure you have `yarn` installed. you can get it [here](https://yarnpkg.com/lang/en/docs/install/)

```
$ yarn global add electron@1.4.15
$ yarn global add gulp
```

### Mist setup

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
* Write down your ethereum address (we need this in class)
* `0x6a6401AEb4a3beb93820904E761b0d86364bb39E` 

### Were done with mist!

If you want to read more about mist [you can do so here.](https://github.com/ethereum/mist "Ethereum's github") For now close both terminals. The one running meteor and the one running mist/electron.
* Make sure to close both terminals

