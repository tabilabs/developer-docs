## Deploy Tabi Chain test v2 from source code

## Hardware preparation
- Hard disk: 2T SSD
- CPU: 16 Core
- Main Memory: 32G

## Software preparation
- System: Ubuntu server 24.04 LTS, 64bits
- Golang: 1.20+

## Deploy
### Download source code
```shell
~$ git clone https://github.com/tabilabs/tabi.git
```

### Build source code
```shell
~$ cd tabi
~/tabi$ make build
```

check the output file in ~/tabi/build dir


### Init Chain and wallet
```shell
~/tabi$ cd build
~/tabi/build$ ./tabid init node0 --chain-id tabi_9788-2
~/tabi/build$ ./tabid keys add node0
```

### Download genesis config file
```shell
~$ cd 
~$ git clone https://github.com/tabilabs/networks.git
~$ cp networks/testnet-v2/genesis.json  ./tabid/config/
```

### Config the node
```shell
~$ cd ~/.tabid
~/.tabid$ vi config/config.toml

set timeout_commit = "3s"
set persistent_peers = "8d2cdfc8732e6121e735ccf442d625b45df2ba2a@52.53.128.4:26656,a2f70d88465a039d35a81d7e162c31cb01270f78@54.241.128.139:26656,966a1b1bcd43cca983e369e6f0ab1e412ec79446@54.176.175.50:26656,08aef064d88d4201891d20232c89d8a7e9b4a8e4@54.177.180.136:26656" 

```



### Start your full node
```shell
~$ cd tabi/build
~/tabi/build$ ./tabid start
```

Open another terminal, and check the status of your full node(by querying the latest block):
```shell
~/tabi/build$ ./tabid q block
```

OK, your full node has been installed.

