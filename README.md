# Truffle tutorial

### Setting up Mist


## Dependencies

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
$ mkdir mist_test
# cd mist_test
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

Launch the application in test network, this should take some time to download all the past blocks.

Create and save a password and copy your ethereum address. 
