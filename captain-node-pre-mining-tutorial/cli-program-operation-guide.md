# CLI Program Operation Guide



#### 1. Wallet Command



You can use the following command to view wallet related instructions

`node-cli wallet`

**Import wallet address**



If you already have a wallet address, use the following command to import the wallet: (Please delete the leading 0x from the wallet private key)

`node-cli wallet import {private_key}`

**Create new wallet**



You can create a new wallet using the following command:

`node-cli wallet create`

After the command is successfully executed, you will see your wallet mnemonic, please save it safely. We will not be able to recover your wallet if it is lost.

**Export current wallet**



You can use the following command to export the private key of the current wallet:

`node-cli wallet export`

**View the current wallet address**



You can use the following command to view the current wallet address:

`node-cli wallet show`

**Delete the current wallet address**



If you want to change a wallet address, you can use the following command to delete the current wallet address:

`node-cli wallet delete`

#### 2. Nodes Command



**Query all nodes of the current wallet address**



How to get nodes? Click here [https://app.tabichain.com/](https://app.tabichain.com/)

`node-cli nodes list`

**Approve nodes**



If you want to accept some nodes and mine for them, please use the following command:

`node-cli nodes approve`

**Start mining**



Please use the following command to mine:

`node-cli nodes mining`

After running the mining command, you will see some printed information, please do not end the process.

You can use nohup, screen, supervisor, etc. to maintain the process. For example, use nohup:

`nohup node-cli nodes mining &`
