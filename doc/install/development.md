Development [Mac and Linux]
-------------------------------------

## 1. Requirements

* Linux / Mac OSX
* Ruby 2.1.0, using [RVM](http://rvm.io/) or [rbenv](https://github.com/sstephenson/rbenv)
* MySQL
* Redis
* PhatomJS
* qrencode
* Pusher

** More details are in the [requirements doc](doc/install/requirements.md)

### reCAPTCHA

Peatio use [reCAPTCHA](https://www.google.com/recaptcha) to make sure certain operations is not done by bots. A development key/secrect pair is provided in `config/application.yml` (uncomment to use). PLEASE USE IT IN DEVELOPMENT/TEST ENVIRONMENT ONLY!

### Pusher

Peatio depends on [Pusher](http://pusher.com). A development key/secret pair for development/test is provided in `config/application.yml` (uncomment to use). PLEASE USE IT IN DEVELOPMENT/TEST ENVIRONMENT ONLY!

More details to visit [pusher official website](http://pusher.com)

##### Install PhatomJS

**For Mac**

    brew install phantomjs

**For Ubuntu**

    sudo apt-get install -y libfontconfig libfontconfig-dev libfreetype6-dev

* Download the [32 bit](https://phantomjs.googlecode.com/files/phantomjs-1.9.2-linux-i686.tar.bz2)
or [64 bit](https://phantomjs.googlecode.com/files/phantomjs-1.9.2-linux-x86_64.tar.bz2)
binary.
* Extract the tarball and copy `bin/phantomjs` into your `PATH`

** More details are in the [poltergeist](https://github.com/jonleighton/poltergeist/blob/master/README.md) doc.

##### Install qrencode

**For Mac**

    brew install qrencode

**For Ubuntu**

    sudo apt-get install qrencode libqrencode-dev

## 2. Bitcoind

#### Install bitcoind

**For Mac**

Download and Install [Bitcoin](http://bitcoin.org/en/download)

**For Ubuntu**

    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install -y bitcoind

#### Configure bitcoind

Insert the following lines into your bitcoin.conf, and replce with your username and password.

    server=1
    daemon=1
    rpcuser=INVENT_A_UNIQUE_USERNAME
    rpcpassword=INVENT_A_UNIQUE_PASSWORD

    # If run on the test network instead of the real bitcoin network
    testnet=1


**For Mac**

    ~/Library/Application\ Support/Bitcoin/bitcoin.conf

**For Linxu**

    ~/.bitcoin/bitcoin.conf


#### Start Bitcoind

**For Mac**

    open /Applications/Bitcoin-Qt.app --args -server

**For Linux**

    bitcoind


## 3. Peatio

##### Clone the project

    git clone git@github.com:peatio/peatio.git
    cd peatio
    bundle install

##### Prepare configure files:

    cp config/application.yml.example config/application.yml
    cp config/database.yml.example config/database.yml
    cp config/currency.yml.example config/currency.yml

##### Setup reCAPTCHA/Pusher:

    # uncomment reCAPTCHA and Pusher related settings
    vim config/application.yml

##### Setup bitcoind rpc endpoint

    # replace username:password and port with the one you set in
    # username and password should only contain letters and numbers, do not use email as username
    # bitcoin.conf in previous step
    vim config/currency.yml

##### Config database settings:

    vim config/database.yml

    # Initialize the database and load the seed data
    bundle exec rake db:setup

##### Run Peatio

    # start matching engine
    # Caution: NEVER start worker with QUEUE=* or QUEUE=matching! There must be only one matching engine running at any time
    rake environment resque:matching

    # start withdraw & examine workers.
    rake environment resque:work QUEUE=coin,examine,mailer

    # start trade, deposit and withdraw daemons
    rake daemons:start

    # start server
    rails server

##### Visit [http://localhost:3000](http://localhost:3000)

    user: admin@peatio.dev
    pass: Pass@word8

