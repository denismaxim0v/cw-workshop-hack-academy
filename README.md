# LocalOsmosis
## Setup
The setup instructions are [here](https://docs.osmosis.zone/developing/tools/localosmosis.html#what-is-localosmosis)
## Run
```
cd LocalOsmosis && make start
```
## Verify
```
osmosisd status
```

# Contracts
## Build
To create an optimized build
```bash
cd 'some contract dir

docker run --rm -v "$(pwd)":/code \
    --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
    --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
    cosmwasm/rust-optimizer:0.12.6
```
or workspace optimizer
```
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.12.6
```
## Upload and Contract interactions
### Create a key using one of the provided mnemonics
```
osmosisd keys add test --recover
```
notice oak worry limit wrap speak medal online prefer cluster roof addict wrist behave treat actual wasp year salad speed social layer crew genius

### Store contract binary to LocalOsmosis
```bash
cd artifacts

osmosisd tx wasm store cw_workshop_counter.wasm  --from test --chain-id=localosmosis --gas-prices 0.1uosmo --gas auto --gas-adjustment 1.3 -b block -y
```


### Instantiate the contract
```
osmosisd tx wasm instantiate 1 '{"count": 100}' --amount 50000uosmo  --label "Counter Contract" --from test --chain-id localosmosis --gas-prices 0.1uosmo --gas auto --gas-adjustment 1.3 -b block -y --no-admin
```


### Execute contract
```
osmosisd tx wasm execute $CONTRACT_ADDR '$INCREMENT_MSG' --from c1
```

### Query contract
```
osmosisd query wasm contract-state smart $CONTRACT_ADDR '$GET_STATE_MSG'
```

## CW20
### Store binary
```
osmosisd tx wasm store cw20_base.wasm  --from test --chain-id=localosmosis --gas-prices 0.1uosmo --gas auto --gas-adjustment 1.3 -b block -y
```

### Instantiate CW20 token
```
osmosisd tx wasm instantiate 2 '{"name": "HACK", "symbol": "HACK", "decimals": 18, "initial_balances": [], "mint":{"minter": "osmo1cyyzpxplxdzkeea7kwsydadg87357qnahakaks"}}' --amount 50000uosmo  --label "Counter Contract" --from test --chain-id localosmosis --gas-prices 0.1uosmo --gas auto --gas-adjustment 1.3 -b block -y --no-admin
```

### Mint some tokens
```
osmosisd tx wasm execute osmo1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqvlx82r '{"mint": {"recipient": "osmo148vum93aemw4480gsw30cxuq5sekt9j2e72ke5", "amount": "1000000000000000000"}}' --from test
```

### Check balance
```
osmosisd query wasm contract-state smart osmo1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqvlx82r '{"balance": {"address":"osmo148vum93aemw4480gsw30cxuq5sekt9j2e72ke5"}}'
```

# Resources
 - [Cosmos Academy]("https://cosmos-network.gitbooks.io/cosmos-academy/content/")
 - [CosmWasm docs]("https://docs.cosmwasm.com/docs/1.0/")
 - [Cosmos SDK]("https://docs.cosmos.network/master/intro/overview.html")
 - [Map of zones]("https://mapofzones.com/?testnet=false&period=24&tableOrderBy=ibcVolume&tableOrderSort=desc")
 - [SecretNetwork]("https://scrt.network/developers")
 - [Fadroma]("https://github.com/hackbg/fadroma")
