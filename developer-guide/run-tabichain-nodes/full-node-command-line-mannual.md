
## Key(wallet) management
### List all wallets
```shell
   tabid keys list
```
### Create a wallet
```shell
   tabid keys add node0          // here "node0" is your wallet name
```
### Delete a wallet
```shell
   tabid keys delete node0       // here "node0" is your wallet name
```
### Export a wallet
```shell
   tabid keys export node0       // here "node0" is your wallet name
```
### Recover a wallet from mnemonic
```shell
   tabid  keys add node0 --recover
```

## Bank
### Transfer
- Usage:
   
  tabid tx bank send _from-address_  _to-address_ _amount_ [flags]

- Example:

```shell
   tabid tx bank send from_address \
   to_address \
   10000000000000000000atabi \
   --chain-id tabi_9788-2 \
   -y -b block \
   --fees 4000000000000000atabi
``` 

### Balance
- Usage

   tabid q bank balances _your-address_



- Example:
```shell
tabid q bank balances tabis1yyxltd3hkz97g75w6neu7xa2djx4wu4er5jcsq
```
## Staking

### Create a validator

- Example
```shell
   tabid tx staking create-validator \
   --amount=100000000000000000000atabi \
   --pubkey=$(tabid tendermint show-validator) \
   --moniker="node1" \
   --chain-id=tabi_9788-2 \
   --commission-rate="0.10" \
   --commission-max-rate="0.20" \
   --commission-max-change-rate="0.01" \
   --min-self-delegation="100" \
   --gas=2000000 \
   --from=node0 \
   -y -b block --fees 4000000000000000atabi
```
### Query all validators
- Example:
```shell
   tabid q staking validators
```

### Stake
- Example:
```shell
   tabid tx staking delegate tabisvaloper184wyej6rgvtgm783mvcmwntwcq9wpr27uu3a2d 1000000000000000000000atabi --chain-id tabi_9788-2 -y -b block --fees 6000000000000000atabi --from=test1
```
