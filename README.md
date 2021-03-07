# Open Ethereum Pool Docker

Use the awesome [Docker CE](https://www.docker.com/community-edition) to get
[dalareo/open-ethereum-pool](https://github.com/dalareo/open-ethereum-pool) up and running quickly.

## Instructions on how to run this project

1. Install Docker. Please [see official instructions](https://docs.docker.com/install/).

2. Clone this repository to local drive:

```
git clone https://github.com/dalareo/eth-pool-docker-infra.git
```

3. Build Pool UI web application. Locally clone the [Open Ethereum Pool](https://github.com/dalareo/open-ethereum-pool-ui) project, and follow the instructions
to build the UI found in the [README](https://github.com/dalareo/open-ethereum-pool-ui/blob/master/README.md) file.

10. Transfer build UI app `dist` folder to server:

```
rsync -avz -e 'ssh' ./dist root@<your_IP>:/root/dev/eth-pool-docker-infra/pool-ui
```

NOTE: Please modify the IP and paths according to your setup!

5. Run:

```
docker-compose up
```

## FAQ

1. How to enable RPC access to geth from anywhere:

```
geth {{usual parameters}} --rpc --rpcport 13270 --rpcaddr "0.0.0.0" --rpccorsdomain "*"
```

2. How to create an account using `geth` CLI:

```
geth --datadir ./data account new
```

3. How to unlock an account when `geth` launches:

```
geth {{usual parameters}} --unlock "0xf5d6ecb770db5a3a9de21e2adc531befa6f2c551" --password passwd.txt
```

where `passwd.txt` is a text file containing the account's password.

4. How to setup a private Ethereum test network:

We can permanently disable the built-in Ethereum mainnet bootnodes by commenting
out (deleting) all of the `enode` URIs in the file `params/bootnodes.go` and rebuilding `geth`.

Create a `genesis.json` file with desirable blockchain initialization parameters. For example:

```
{
  "config": {
    "chainId": 847283914,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
  },
  "difficulty": "200000000",
  "gasLimit": "2100000",
  "alloc": {}
}
```

Initialize the private blockchain node:

```
geth --datadir ./data init ./genesis.json
```

Start up the node:

```
geth {{usual parameters}} --networkid 847283914
```

5. How to launch a second `geth` node, and connect to your private network:

Launch `geth` with the same command you used to launch the first private node,
but use different parameters for `port` and `rpcport`:

```
geth {{usual parameters}} --port 42371 --rpcport 13271
```

6. Summary. A full example of launching a private geth node (assuming all of the preparatory steps have been completed):

```
geth \
  --datadir ./data \
  --networkid 847283914 \
  --port 4237 \
  --rpc --rpcport 13270 --rpcaddr "0.0.0.0" --rpccorsdomain "*" \
  --unlock "0xf5d6ecb770db5a3a9de21e2adc531befa6f2c551" --password passwd.txt \
  --cache=1024
```

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for more details.
