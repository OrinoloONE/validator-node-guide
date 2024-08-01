<h1 align=center>0G LABS: Validator Node</h1>

![0_AxKIus_I-soNSp03](https://github.com/user-attachments/assets/eb3cd7ae-d588-4d3a-8096-60be1838a1c5)

# TestNet Configuration
|Field|Value|
|:----|:----|
|Chain-Name|0G-Newton-Testnet|
|Chain-Id|16600 (zgtendermint_16600-2)|
|Token-Symbol|A0GI|
|Chain-RPC|https://evmrpc-testnet.0g.ai|
|Chain-Websocket|https://cosmosrpc-testnet.0g.ai|
|Storage-RPC|https://rpc-storage-testnet.0g.ai|
|Storage-Turbo-RPC|https://rpc-storage-testnet-turbo.0g.ai|

# Validator Node
## Hardware Requirement
|Field|Value|
|:----|:----|
|Memory|64 GB|
|CPU|8 cores|
|Disk|1 TB NVME SSD|
|Bandwidth|100 MBps for Download / Upload|

## Install via CLI
```bash
git clone -b v0.2.3 https://github.com/0glabs/0g-chain.git
./0g-chain/networks/testnet/install.sh
source ~/.profile
```

### Set Chain ID
```bash
0gchaind config chain-id zgtendermint_16600-2
```

## Initialize Node
```bash
0gchaind init <your_validator_name> --chain-id zgtendermint_16600-2
```

## Genesis & Seeds
### Copy the Genesis.json file
```bash
sudo apt install -y unzip wget
rm ~/.0gchain/config/genesis.json
wget -P ~/.0gchain/config https://github.com/0glabs/0g-chain/releases/download/v0.2.3/genesis.json
0gchaind validate-genesis
```

### Add Seed Nodes
$HOME/.0gchain/config/config.toml
```bash
#######################################################
###           P2P Configuration Options             ###
#######################################################
[p2p]

# ...

# Comma separated list of seed nodes to connect to
seeds = "81987895a11f6689ada254c6b57932ab7ed909b6@54.241.167.190:26656,010fb4de28667725a4fef26cdc7f9452cc34b16d@54.176.175.48:26656,e9b4bc203197b62cc7e6a80a64742e752f4210d5@54.193.250.204:26656,68b9145889e7576b652ca68d985826abd46ad660@18.166.164.232:26656"
```

## Start Testnet
```bash
0gchaind start
```

## Create Validator
```bash
0gchaind keys add <key_name> --eth
```
```bash
0gchaind tx staking create-validator \
  --amount=<staking_amount>ua0gi \
  --pubkey=$(0gchaind tendermint show-validator) \
  --moniker="<your_validator_name>" \
  --chain-id=zgtendermint_16600-2 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --from=<key_name> \
  --gas=auto \
  --gas-adjustment=1.4
```
