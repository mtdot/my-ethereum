# Table of contents

1. Install
2. Prepare accounts
3. Prepare genesis.json
4. Running private network
5. Interaction with network
6. Deploy smart contract to private network

# 1. Install
- https://geth.ethereum.org/downloads/

# 2. Prepare accounts
```cmd
md node01                                     # creating data dir
geth --datadir .\node01\ account new          # creating account, remember to backup account address
```

# 3. Prepare genesis.json
- **Template**: Original template could be found [here](https://geth.ethereum.org/docs/interface/private-network)

```json
{
  "config": {
    "chainId": 15,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "ethash": {}
  },
  "difficulty": "1",
  "gasLimit": "8000000",
  "alloc": {
    "A6A489FbC1189d24E3ce6Af9216891edf8aADC1A": { "balance": "1000000000000" },
    "a2DfF448552EcB1b821AF76AeFc29e648B656ff3": { "balance": "1000000000" }
  }
}
```

- **Alloc**: modify this `aloc` block corresponsing with your created accounts from Section 2 (Balance unit: `wei`)

```json
  "alloc": {
    "A6A489FbC1189d24E3ce6Af9216891edf8aADC1A": { "balance": "1000000000000" },
    "a2DfF448552EcB1b821AF76AeFc29e648B656ff3": { "balance": "1000000000" }
  }
```

# 4. Running private network
- Startig Nodes (Remember to collect peer string for adding peer node later - Section 5)
```cmd
geth --identity "node01" --datadir node01 --port "30303" --nat "any" --ipcdisable --http --http.port 8545 --http.api "eth,net,web3,personal,miner,admin" --allow-insecure-unlock --miner.threads=1
geth --identity "node02" --datadir node02 --port "30304" --nat "any" --ipcdisable --http --http.port 8546 --http.api "eth,net,web3,personal,miner,admin" --allow-insecure-unlock --miner.threads=1
```

# 5. Interaction with network
- Open interactive shell
```cmd
geth attach http://127.0.0.1:8545
geth attach http://127.0.0.1:8546
```
- Useful commands
<details><summary>Adding Peer Node</summary>
<p>

```cmd
admin.addPeer("enode://7a34a32487a65f9eb2d01b172e6f6105da8413becb53296ed0d5166f311b95e09bc65bed9dd0c5e95a91445b6a08b33b3214c5acfb733996bd1c1ff9f31a0141@127.0.0.1:30304")
```

</p>
</details>

<details><summary>Starting miner (synchronizing - mining transaction block)</summary>
<p>

  - Run at least on a single node for making transaction to be confirmed on every nodes.
```cmd
miner.setEtherbase("0x57d863106f8e21e6962a6c5c9d8937fc429a9d14")
miner.start()
miner.stop()
```

</p>
</details>

<details><summary>Check balance</summary>
<p>

```cmd
eth.getBalance(eth.accounts[1])
```

</p>
</details>

<details><summary>Send transaction</summary>
<p>

```cmd
personal.unlockAccount(eth.accounts[0])
eth.sendTransaction({from:eth.accounts[0], to:eth.accounts[1], value:1000000})
```

</p>
</details>

# 6. Deploy smart contract to private network (TBA)
