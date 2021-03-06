# ethereumx-pool
Highly Efficient mining pool designed for ETX coin
![alt text](https://raw.githubusercontent.com/techievee/ethash-mining-pool/master/images/FrontEnd.png)

# watch our setup guides on Youtube (later)
https://www.youtube.com/channel/UCK-0BBAm5FBzMsroUvzi9yQ

I highly recommend to use Ubuntu 16.04.* LTS.

# install ethereumx-pool
    sudo su
    useradd -m -s /bin/bash pool
    usermod -aG sudo pool
    echo "pool:pool" | sudo chpasswd
    su - pool
    git clone https://github.com/notminerproduction/ethereumx-pool.git
    cd ~/ethereumx-pool
    chmod +x ./install.sh
    ./install.sh

# create wallet
    su - pool
    cd ~/ethereumx-pool
    ./getx account new

# put wallet password
    su - pool
    nano ~/pwd

# run blockchain sync
    nano ./start_getx.sh #set wallet address
    #!/bin/bash
    WALLET="HERE_POOL_WALLET_ADDRESS"
    screen -S getx ./getx --rpc --maxpeers 75 --syncmode "fast" --rpcapi "eth,net,web3,personal" --etherbase "$WALLET" --cache=12288 --mine --unlock "$WALLET" --allow-insecure-unlock --password ~/pwd
    
    chmod +x ./start_getx.sh
    ./start_getx.sh

# It must be changed, and then the source code must be rebuilt.
    su - pool
    nano ~/ethereumx-pool/config_api.json #wallet address
    nano ~/ethereumx-pool/www/config/environment.js #192.168.0.200 of your IP or DNS
    
    cd ~/ethereumx-pool/www
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
    ./build.sh
    sudo systemctl restart nginx.service

# run pool
    su - pool
    cd ~/ethereumx-pool
    screen -S pool ./build/bin/ethash-mining-pool config_api.json

# DONT FORGET CHANGE PASSWORD FOR USER pool!
    sudo passwd pool
