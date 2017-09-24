## 9/25 truffle class prep

In oder to be on the same page you guys / gals would be the most prepared if you installed mist/electron. This takes about 30 minutes. This guide is aimed towards apple and linux. If you are on windows you can check mist/electron's [official documentation here.](https://github.com/ethereum/mist) 

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

