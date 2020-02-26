# Exchange Docking Guide

The purpose of this article is mainly to assist the exchange on the line GKM transactions.

github: https://github.com/dto-simba/vxx-chain  

## Basic concepts

The GKM account system uses the UTXO model.

# Hot node setup

## Linux (ubuntu) Node deployment

#### Install

 Download the executable program for the linux version:

 https://github.com/dto-simba/vxx-chain 


  Execute the program using gkmd/gkm-cli using the terminal command window
  If you want to run the program more conveniently, you can copy the executable to /usr/lib and use chmod to delegate it, you can use the gkmd/gkm-cli program in any directory.

#### Start

` ./gkmd` 

#### CLI  management node

` ./gkm-cli command `

#### Stop

` ./gkm-cli stop `

#### Connect to the specified node at startup

```bash
./gkmd -addnode=ip:port
```

## Mac node deployment

#### Install

 Download  https://github.com/dto-simba/vxx-chain
 Unzip mac.zip and enter
 Execute the program using gkmd/gkm-cli using the terminal command window

#### Start

` ./gkmd` 

#### CLI interactive mode management node

` ./gkm-cli command `

#### Stop

` ./gkm-cli stop `

#### Connect to the specified node at startup

```
./gkmd -addnode=ip:port
```


## Window node deployment

#### Install

Download https://github.com/dto-simba/vxx-chain
Unzip win.zip and enter
Execute the program using gkmd.exe/gkm-cli.exe using the cmd command window

#### Start

` gkmd.exe ` 

#### CLI interactive mode management node

` gkm-cli.exe command `

#### Stop

` gkm-cli.exe stop `

#### Connect to the specified node at startup

```
gkmd.exe -addnode=ip:port
```

After startup, the sync block will start and the RPC service will be started (how to use the command to view the developer guide documentation).

## Node startup important parameters
The parameter is incremented after gkmd with a - followed by the parameter name = parameter value

Storage directory

```bash
-datadir=/data/.gkm
```

Blockchain port

```bash
-port=6513
```

Rpc command port

```bash
-rpcport=6512
```

Allow ip to call rpc

```bash
-rpcallowip=127.0.0.1
```

Each time a new block is generated, the specified shell command is triggered, where %s is the HASH of the new block.

```bash
-blocknotify='curl http://xxx.xx.x.xxx:8080/gkm?blockHash=%s'
```

E.g:

```bash
./gkmd  -datadir=/data/.gkm -port=6513 -rpcport=6512 -blocknotify=‘curl http://xxx.xx.x.xxx:8080/gkm?blockHash=%s'
```

After the chain is started, the startup parameters can be written to the conf configuration file. The configuration file is located at /data/.gkm/gkm.conf.

E.g:

```bash
rpcuser=gkmchainrpc
rpcpassword=xxxxxxxxxxxxxxxxxxxxxxxxxxx
port=6513
rpcport=6512
rpcallowip=127.0.0.1
rpcallowip=xxx.xx.x.xxx
rpcallowip=xxx.xx.x.xxx
rpcallowip=xxx.xx.x.xxx
```

The node needs to be restarted after setting.

Use the following command to view basic node information.

```bash
getblockchaininfo
```

The result is

```json
{
    "paytxfee" : "0.0001",
    "rpcport" : 6513,
    "relayfee" : "0.0001",
    "maxout" : 1000000000000000000
}
```

Where paytxfee is the base fee setting of the current node, and rpcport is the callable rpc port.

## How to call a command through the hot node rpc

1. Confirm that the caller IP has been assigned to the rpcallowip

2. View the account password in gkm.conf in the blockchain directory.

   If you do not specify the blockchain directory Linux, Mac default directory is ~/.gkm

   The default directory for Windows is C:\users\{username}\AppData\Romaming\Gkm

## Generate a recharge address

There are a way to generate a recharge address:

- Dynamically create a gkm address

  Call the following command:

  ```bash
  ./gkm-cli getnewaddress
  ```

  The created address will be imported directly into the wallet file.

  ## Check the validity of the address

  Check the validity of the account address by calling the following command:

  ```bash
./gkm-cli validateaddress (address)
  ```

  The returned results are as follows：
  
  ```json
{
    "isvalid" : true|false,          
	"address" : "address",           
    "ismine" : true|false,           
    "isscript" : true|false,          
    "pubkey" : "publickeyhex",      
    "iscompressed" : true|false,      
  }
  ```
  

If the isvalid field is false, it is an invalid address.

## Monitor user recharge

To monitor user recharge, first check the block synchronization status and parse the block information to confirm whether the user recharges.

  ### Parsing block information

  Call the command as follows:

  ```bash
 ./gkm-cli getblock hash
  ```

We use the UTXO model, and the exchange needs to parse the VOUT part of each TX, where value is the local asset, GKM, and token is the created asset. There are two ways to compare recharge information:

  1. The exchange records all transactions related to itself as a record of user refill withdrawals. If it is found that there is an address belonging to the exchange in VOUT, modify the user balance of the recharge address in the database (of course, may also include the issue of assets).

2. If there is an address belonging to the exchange in VOUT, first record the recharge record in the database, wait for several blocks to confirm and then modify the user balance. Note that you need to check the duplicate address in vin and vout to judge vout. Is the transfer behavior, the collection behavior or the change behavior.

   ```bash
    ./gkm-cli getblock "0020d76cxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx993f6a754756f0e"
   ```

Return results:

  ```json
 {
    "hash" : "0020d76cxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx993f6a754756f0e",
    "miner" : "GxxxxxxxxxxxxxxxxxxxxxxxxxiVSd",
    "confirmations" : 10501,
    "size" : 298,
    "height" : 10,
    "version" : 3,
    "merkleroot" : "7eed1a68e6efe4axxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx7f0cd61f7726a59",
    "tx" : [
		"7eed1a68e6efexxxxxxxxxxxxxxxxxxxxxxxxxxaf7f0cd61f7726a59",
    ],
    "time" : 1572266966,
    "nonce" : 261,
    "bits" : "2000ffff",
    "difficulty" : 5.96046447753906e-8,
    "chainwork" : "0000000000000000000000000000000000000000000000000000000000000b00",
    "prevblockhash" : "00956a6039xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx5afdb7343893",
    "nextblockhash" : "00c313b56xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx72d6d4de714fb9"
}
  ```

  ## Handling withdrawal requests

    1. Record the user withdrawal request and change the balance;
    2. Call the sendtoaddress command to send the transaction to the user's withdrawal address. For details, please refer to the "developer-en.md";
    3. After the command is successfully executed, txid will be returned and recorded in the database;
    4. After the block is confirmed, the withdrawal is successful.
    Note that when transferring via the sendtoaddress command, the default will be zero to initiate the transfer address. If you need to specify the change address, please use the offline transaction method.

  ### Set node handling fee

The basic fee for node transfer can be set by settxfee. The node fee is 0.0001 by default. The specific fee is charged according to the byte.

#### note:

In a hot node, if the number of addresses of the node is too large, and the number of uspents of the address is too large, the performance of the node may be degraded. The unspent of the unused addresses may be merged into one unspent by the following two methods.

1. Transfer yourself to the address yourself

2. When starting the node, use -minconsolidation=50 -maxconsolidation=100 to configure the unspent of the address to be automatically merged. The node defaults to minconsolidation=-1 without unspent merge.

In both cases, a transaction will be generated for the transfer of the address itself. Please distinguish between this transaction and the recharge transaction to prevent the merged unspent transaction from being processed as a recharge transaction!

## Node encryption

After encrypting the wallet or changing the wallet password using gkm-cli encryptwallet "password", the node will stop the service by itself. Please use gkmd to restart the encrypted node.

### Set the keypool size

Keypool capacity will be reduced after node encryption, you can use

      ./gkm-cli keypoolrefill [size]

  Set the size of the keypool


## Offline transaction

### Query not spending output

Call the following command:

```bash
listunspent ( minconfirm maxconfirm addresses )
```

> Method parameter

| parameter  | description                       |
| ---------- | --------------------------------- |
| minconfirm | Block minimum confirmation number |
| maxconfirm | Block maximum confirmation number |
| addresses  | The set of addresses to query     |


> e.g.

```bash
	listunspent 6 9999999 "[\"address1\",\"address2\"]"
```

> return value

```bash
	[                                       
    {
      "txid" : "txid",                      
      "vout" : n,                         
      "address" : "address",                
      "account" : "account",                
      "scriptPubKey" : "key",               
      "amount" : x.xxx,                    
      "confirmations" : n                   
    }
    ,...
  ]
```

### Assembly transaction
  1.The exchange obtains the address by unresolving the unspent by parsing the block. The calling command is as follows:

#### Note: Please fill in the vin for the payment of the handling fee. For the calculation of the handling fee, please refer to the formula for calculating the fee at the end of the document.

  ```bash
	createrawtransaction [{"txid":"id","vout":n},...] {"address":amount,...} ( [data] "action" )
  ```
> Method parameter

| parameter    | description                                                  |
| ------------ | ------------------------------------------------------------ |
| transactions | Transaction collection                                       |
| addresses    | Address: Quantity                                            |
| data         | Additional data                                              |
| action       | The default is "", lock (locks the given input in the wallet), sign (signs the transaction using the wallet key), lock, sign, send (sign and send the transaction) |

> return value

```bash
   hex
   or
	{                                                        
	  "hex": "value",                                      
	  "complete": true|false                                 
	}
```

2.Set the change address and specify the transaction fee through the fundrawtransaction

> Call command

```bash
	fundrawtransaction "tx-hex" "address" ( fee )
```

> Method parameter

| parameter | description    |
| --------- | -------------- |
| tx-hex    | Hex            |
| address   | Change address |
| fee       | fee            |

> e.g.

```bash
	fundrawtransaction "HEX""ADDR"
	fundrawtransaction "HEX""ADDR" 0.01
```

> return value

```bash
	hex
```

It is recommended that the exchange's change address be set separately to prevent the analysis of vout from changing to zero.

3.Offline private key signing transaction via signrawtransactionwithkey

> Call command

```bash
	signrawtransactionwithkey "tx-hex" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )
```

> Method parameter

| parameter   | description                                                  |
| ----------- | ------------------------------------------------------------ |
| Tx-hex      | Hex                                                          |
| prevtxs     | Transaction array, can be set to []                          |
| privatekeys | An array of private keys, if signed at a node with a private key, set to [] |
| sighashtype | Signature type, default is all                               |

> return value

```bash
	{
	  "hex": "value",                           
	  "complete": true|false                   
	}
```

If complete is true, the signature is successful, otherwise it fails.

4.Broadcast transaction via sendrawtransaction

> Call command

```bash
	sendrawtransaction "tx-hex" ( allowhighfees )
```
> Method parameter

| parameter     | description                                           |
| ------------- | ----------------------------------------------------- |
| Tx-hex        | Hex to be broadcast after signature                   |
| allowhighfees | The default is false, whether higher fees are allowed |


> e.g.

```bash
	sendrawtransaction "signedhex"
```

> return value

```bash
	txid
```

例：

  ```shell
 createrawtransaction '[{"txid":"526be56xxxxxxxxxxxxxxxxxx573fada996c1e08b4c89","vout":0},{"txid":"526be56xxxxxxxxxxxxxxxxxxxxxxxxxxx7cc2573fada996c1e08b4c89","vout":1}]'  '{"GxxxxxxxxxxxxxxxxxxxxxxxxxxxFSSi":{"":50}}'
	#return hex
01000000023935dfgg455tf57cxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx00000e64ad8c6ebffc4d749dbd3e1f93090f002671000000007500000000
  ```

 ### Set the previous step Hex's change address GxxxxxxxxxxxxxxxxxxxxxxxxxxxFSSi and the handling fee 0.001
```shell
fundrawtransaction 01000000023935dfgg455tf57cxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx00000e64ad8c6ebffc4d749dbd3e1f93090f002671000000007500000000 VxxxxxxxxxxxxxxxxxxxxxxxxxxxFSSi 0.001
# return hex2
```
 ### Sign the transaction and broadcast the transaction

```shell
signrawtransactionwithkey hex2 '[]' '["privatekey1","privatekey2"]'

sendrawtransaction hex2 

Returns the signed hex and complete fields. When the complete field is true, it means that all unspents are signed and can be broadcast.
```


```json
{
    "hex" : "xxxxxxx",
    "complete" : true
}
```



  ## Related commands

  ### Check balances

  Use getbalances to query the user balance. The minimum unit of the asset is 0.00000001.

### Query transaction information

#### gettransaction

> Method description

Returns the specified wallet transaction details. This command can only query related transactions at the address of this node.

> Call command

```bash
	gettransaction txid
```

> Method parameter
> |parameter	|description	|
> |--	|--	|
> |txid  |txid	|

> e.g.

```bash
	gettransaction  "txid"
```

> return value

```bash
{
    "amount" : "100.00",
    "confirmations" : 10565,
    "generated" : true,
    "blockhash" : "0020xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx93f6a754756f0e",
    "blockindex" : 0,
    "blocktime" : 1572266966,
    "txid" : "7eed1a68exxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxcd61f7726a59",
    "walletconflicts" : [
    ],
    "valid" : true,
    "time" : 1572266966,
    "timereceived" : 1572266966,
    "details" : [
        {
            "account" : "",
            "address" : "Gxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxy",
            "category" : "generate",
            "amount" : "100.00",
            "vout" : 0
        }
    ],
    "hex" : "01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff0a5a0101062f503253482fffffxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxa5c0544adf1410683959178877b24a6bc72deb5a396d17ac2700000000"
}	
```

####  getrawtransaction

> Method description

Query transaction information.

> Call command

```bash
	getrawtransaction "txid" 
```

> Method parameter

|parameter			|description								|
|--				|--									|
|txid			|txid								|

> e.g.

```bash
	getrawtransaction "txid"
```

> return value

```bash
	hex 
```



### Fee calculation

If you want to set the transaction fee by calculating the fee based on the transaction size, you can calculate it by referring to the following formula:

fee = transactionsSize ( bytes ) / 10000000

The size of the transaction can be calculated by referring to the calculation method:

Ordinary transaction size:

```js
	transactionsSize  = vinNum*200+voutNum*34+10
```

Normal transaction size calculation with data:

```js
	transactionsSize  = vinNum*200+voutNum*34+21+（The hexadecimal byte size of the data）
```

Vin/vout contains transaction sizes for other assets (eg BTC):

```js
	transactionsSize  = vinNum*234+voutNum*58+20
```

