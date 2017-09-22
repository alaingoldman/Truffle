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

```
$ cd ~/Desktop/mist_test/mist
$ yarn dev:electron
```
