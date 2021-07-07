# The LeefCoin Block Explorer
Live here - [Explorer.LeefCoin.org](http://explorer.leefcoin.org)

Based on the [iquidus explorer](https://github.com/iquidus/explorer)

# Install on ubuntu 18.04 server

First of all, make sure you have a LeefCoin node installed.

  If you don't have a LeefCoin Core installed, you can find the tutorial about how to install it here


* Install the required dependencies:


      sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 libboost-system-dev libboost-filesystem-dev             libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-all-dev libboost-program-options-dev -y

* Install the additional dependencies:


      sudo apt-get install libminiupnpc-dev libzmq3-dev libprotobuf-dev protobuf-compiler unzip software-properties-common libkrb5-dev mongodb nodejs npm git nano cmake screen -y


# Start LeefCoin Node & Create the database

* Type the following command to start the LeefCoin daemon:

      leefcoind

* Type the following command to start MongoDB:

      mongo
    
    
* Type the following command to create a MongoDB database named “explorerdb”:

      use explorerdb
* Create a user for your database:
      
      db.createUser( { user: "user", pwd: "password", roles: [ "readWrite" ] } )
 
 Replace "user" and "password" with a special username and password.

Type the following command to close MongoDB:

     exit
     
# Install LeefCoin Explorer

Type the following command to clone LeefCoin Explorer:

      git clone https://github.com/leefcoin/explorer.git 
      
* Type the following command to install LeefCoin Explorer:

      cd explorer && npm install --production
      
# Modify the following values in file settings.json

* Network

      Change the value “127.0.0.1” to the IPv4 address of your server.

* Database

      “password” -> “your database password”.

* Wallet daemon

      “user” -> “Your node JSON-RPC user”.

      "password” -> “Your node JSON-RPC password”.

# Run LeefCoin Explorer

* Type the following command to start a screen session:

      screen

*Type the following commands to start your block explorer:

      cd $HOME/explorer && npm start

Press the keyboard shortcut ctrl + a + d to disconnect from your screen session.

* Type the following command to open crontab:

      crontab -e

* Paste the following text into crontab:

      @reboot leefcoind
      */1 * * * * cd /root/explorer && /usr/bin/nodejs scripts/sync.js index update > /dev/null 2>&1
      */5 * * * * cd /root/explorer && /usr/bin/nodejs scripts/peers.js > /dev/null 2>&1
      
      
Save the crontab with the keyboard shortcut ctrl + x

Confirm that you want to save the crontab with the keyboard shortcut y + enter

The block explorer should be up and running on the IPv4 of the server on port 80.
