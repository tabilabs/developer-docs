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
~$ cp networks/testnet-v2/genesis.json  ~/.tabid/config/
```

### Config the node
```shell
~$ cd ~/.tabid
~/.tabid$ vi config/config.toml

set timeout_commit = "3s"
set persistent_peers = "f9db4db84c51971cd648622c95d6456239954375@54.241.209.39:26656,222a1a72cbeab675379c017ced64f2e27f156725@54.241.130.156:26656,13e5b935798f612f9d64dde263376dedc81bb64a@13.57.216.236:26656,b39f1c0bb689516020653b4937fcc42d6e4d39b6@3.101.61.50:26656"
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

