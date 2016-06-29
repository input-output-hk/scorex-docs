---
title: Settings
---

# Network settings
Network-wide settings are compiled with the source code and can be managed with `application.conf` file via [typesafe config library](https://github.com/typesafehub/config).

```
app {
  # Release name. Will be used by peers to identify the network
  release = "Lagonaki"

}
scorex {
  # Full path to the main hash function. It should implement CryptographicHash interface. You can find a lot of hash functions in scrypto library (https://github.com/ScorexProject/scrypto).
  fastHash = "scorex.crypto.hash.Blake256"

  # Full path to slow and most secure hash function. It should implement CryptographicHash interface. You can find a lot of hash functions in scrypto library (https://github.com/ScorexProject/scrypto).
  secureHash = "scorex.crypto.hash.ScorexHashChain"
}
```

# Node settings
To run your Scorex node you should create json file with application settings. Most of settings have default values so you may provide just few settings to customize your app.

## Scorex-basics settings

| Parameter        | Default value  | Description  |
| ----------------- | ---------- |  ---------- |
| `dataDir` | undefined | Optional folder to keep your data. When undefined, all the data is kept in memory |
| `localOnly` | `false` | Node is only available locally |
| `rpcAddress` | `0.0.0.0` | Bind address for remote procedure call |
| `rpcPort` | `9085` | Port for remote procedure call |
| `rpcAllowed` | `127.0.0.1` | Space separated list of addresses, that can use rpc |
| `offlineGeneration` | `false` | Allow node to generate new blocks when it's offline |
| `historySynchronizerTimeout` | `30` | Timeout in seconds for the history synchronizer to wait a response from remote nodes |
| `blockGenerationDelay` | `1000` | Time in milliseconds for block generator to try to generate a new block |
| `mininigThreads` | `1` | Number of threads for mining |
| `walletDirOpt` | undefined | Optional folder to keep your wallet. When undefined, your wallet is kept in memory |
| `walletPassword` | undefined | Password for your wallet. When undefined, you should pass it on application start |
| `walletSeed` | undefined | Seed for your wallet to generate new accounts. When undefined, you should pass it on application start |
| `apiKeyHash` | undefined | Hash for API key. When undefined, RPC requests are allowed without any authentication |
| `genesisTimestamp` | `1460952000000` | Timestamp for genesis block. |
| `cors` | `false` | Allow API to called from another domain outside the domain from which the resource originated. |
| `p2p`:`nodeName` | Random string |  Name of the node|
| `p2p`:`bindAddress` | `127.0.0.1` | The socket address to bind to |
| `p2p`:`port` | `9084` | Port of the socket address to bind to |
| `p2p`:`maxConnections` | `20` | Maximum number of outgoing connections |
| `p2p`:`connectionTimeout` | `60` | Connection timeout in seconds |
| `p2p`:`upnp` | `true` | Enable upnp |
| `p2p`:`upnpGatewayTimeout` | `true` |  The timeout for actions on the device |
| `p2p`:`upnpDiscoverTimeout` | `true` | The timeout for socket connections of the initial broadcast request |
| `p2p`:`myAddress` | `None` | Optional setting to configure declared address |
| `p2p`:`knownPeers` | `[]` | List of known peers |

## Scorex-transaciton settings

| Parameter        | Default value  | Description  |
| ----------------- | ---------- |  ---------- |
| `history` | `blockchain` | History type. Blockchain or blocktree (experimental) |
| `maxRollback` | `100` | Maximum number of blocks to rollback |

## Scorex-perma settings

| Parameter        | Default value  | Description  |
| ----------------- | ---------- |  ---------- |
| `perma`:`rootHash` | `13uSUANW...` | Base58-encoded root hash of Merkle tree with permacoin data. |
| `perma`:`isTrustedDealer` | `false` | Set current node as trusted dealer |
| `perma`:`treeDir` | required | Folder to keep permacoin data |
| `perma`:`authDataStorage` | `authDataStorage.db` | Name of file to put authenticated permacoin data. File will be created in treeDir |


## Settings example

Typical json files with settings looks like

```json
{
  "p2p": {
    "bindAddress": "0.0.0.0",
    "upnp": false,
    "upnpGatewayTimeout": 7000,
    "upnpDiscoverTimeout": 3000,
    "port": 9084,
    "knownPeers": [
      "23.94.190.226:9185"
    ],
    "maxConnections": 10
  },
  "walletDir": "/tmp/scorex/wallet",
  "dataDir": "/tmp/scorex/data",
  "rpcPort": 9085,
  "rpcAllowed": [],
  "maxRollback": 100,
  "blockGenerationDelay": 0,
  "genesisTimestamp": 1460952000000,
  "apiKeyHash": "GmVvcpx1BRUPDZiADbZ7a6zgQV3Sgj2GhNoEiTH9Drdx",
  "cors": false
}
```
