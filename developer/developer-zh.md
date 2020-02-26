# 1 GKM开发指南

GKM提供了一个简单、高效的、丰富的API，帮助开发者在GKM中开发、调用底层rpc服务。

# 2.JSON-RPC API指令集

## 2.1 可用API指令

调用命令的方式 gkm-cli (command),例如gkm-cli help.

#### help
> 方法说明

返回可用命令列表

> 调用命令

```bash
	help (command)
```
> 方法参数

|	参数|描述	|
|--	|--	|
|	command|命令名称或无参	|

> e.g.

```bash
	help pause
```

> 返回值

若无参数显示命令列表，若有参数显示该命令的使用方法

#### stop
> 方法说明

停止区块链运行

> 调用命令

```bash
	stop
```
> 方法参数

无

> 返回值

GKM server stopping

#### 本地钱包相关

#### getblock
> 方法说明

查看指定hash的区块信息

> 调用命令

```bash
	getblock "hash" ( type )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|hash	|区块哈希	|
|type	|0- encoded data, 1- json object, 2- with tx encoded data, 4- with tx json object	|



> e.g.

```bash
	getblock "00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"
```

> 返回值

```bash
	Outputs (for type = 1):
	{
	  (string)  "hash" : "hash",             
	  (string)  "miner" : "miner",           
	  (numeric) "confirmations" : n,         
	  (numeric) "size" : n,                  
	  (numeric) "height" : n,             
	  (numeric) "version" : n,              
	  (string)  "merkleroot" : "xxxx",      
				"tx" : [                     
	  (string)            "TXID"             
							,...
						],
	  (numeric) "time" : ttt,                 
	  (numeric) "nonce" : n,                 
	  (string)  "bits" : "1000ffff",         
	  (numeric) "difficulty" : x.xxx,       
	  (string)  "prevblockhash" : "hash"
	  (string)  "nextblockhash" : "hash" 
	}

	Outputs (for type=0):
	  (string)  "data"                      
```

#### getblockhash
> 方法说明

返回指定高度块的HASH值

> 调用命令

```bash
	getblockhash index
```
> 方法参数
|参数	|描述	|
|--	|--	|
|index	|区块高度	|


> e.g.

```bash
	getblockhash 20
```

> 返回值

```bash
	区块hash值
```

#### gettransaction

> 方法说明

返回指定的钱包交易明细,此命令只可查询在本节点的地址的相关交易

> 调用命令

```bash
	gettransaction txid
```

> 方法参数
> |参数	|描述	|
> |--	|--	|
> |txid  |txid	|

> e.g.

```bash
	gettransaction  "txid"
```

> 返回值

```bash
{
    "amount" : "0.00",
    "fee" : "0.000069",
    "confirmations" : 10370,
    "blockhash" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "blockindex" : 4,
    "blocktime" : 1562300531,
    "txid" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "walletconflicts" : [
    ],
    "valid" : true,
    "time" : 1562300524,
    "timereceived" : 1562300524,
    "details" : [
        {
            "account" : "",
            "address" : "vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv",
            "category" : "send",
            "amount" : "4.00",
            "vout" : 0,
            "fee" : "0.000069"
        },
        {
            "account" : "",
            "address" : "vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv",
            "category" : "send",
            "amount" : "0.019771",
            "vout" : 1,
            "fee" : "0.000069"
        },
        {
            "account" : "",
            "address" : "vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv",
            "category" : "receive",
            "amount" : "4.00",
            "vout" : 0
        },
        {
            "account" : "",
            "address" : "vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv",
            "category" : "receive",
            "amount" : "0.019771",
            "vout" : 1
        }
    ],
    "hex" : "010vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv00000"
}	
```

#### getnewaddress

> 方法说明

创建一个新地址

> 调用命令

```bash
	getnewaddress
```
> 方法参数

无

> 返回值

```bash
	创建出的新地址
```
#### addmultisigaddress
> 方法说明

创建多签地址并添加至节点

> 调用命令

```bash
	addmultisigaddress nrequired keys
```
> 方法参数

|参数	|描述	|
|--	|--	|
|nrequired	|签名时需要签名地址的数量	|
|keys	|公钥集合	|


> e.g.

```bash
	addmultisigaddress 2 "[\"HEXPUBKEYS\",\"HEXPUBKEYS\"]"
```

> 返回值

```bash
	多签地址
```
#### createmultisig
> 方法说明

创建多签地址但不加入钱包，不建议使用

> 调用命令

```bash
	createmultisig nrequired keys
```
> 方法参数

|参数	|描述	|
|--	|--	|
|nrequired	|签名时需要签名地址的数量	|
|keys	|公钥集合	|

> e.g.

```bash
	createmultisig 2 "[\"HEX-PUBKEYS\",\"HEX-PUBKEYS\"]"
```

> 返回值

```bash
	{
	  "address":"multisigaddress",         
	  "redeemScript":"script"             
	}
```

#### listtransactions

> 方法说明

查询有关钱包的交易记录

> 调用命令

```bash
	listtransactions ( count skip false|true)
```
> 方法参数

|参数		|描述								|
|--			|--									|
|count		|交易记录的数量						|
|skip		|开始的下表，默认为0				|
|false/true	|默认为false,若为true则包含只读账户	|


> e.g.

```bash
	listtransactions
	listtransactions 20 100 true
```

> 返回值

```bash
	[
	  {
		"balance": {...},                 
		{
		  "amount": x.xxx,                
		  "tokens": {...},                
		}
		"myaddresses": [...],            
		"addresses": [...],              
		"permissions": [...],            
		"sell": {...},                    
		"data" : "metadata",            
		"confirmations": n,              
		"blockhash": "hashvalue",       
		"blockindex": n,                
		"txid": "TXID",                  
		"time": xxx,                      
		"timereceived": xxx,             
		"comment": "...",                
		"vin": [...],                    
		"vout": [...],                    
		"hex" : "data"                   
	  }
	]
```


#### dumpprivkey

> 方法说明

导出私钥

> 调用命令

```bash
	dumpprivkey "address"
```

> 方法参数

|参数	|描述	|
|--	|--	|
|address	|要导出私钥的地址	|


> 返回值

```bash
	私钥
```

#### importprivkey

> 方法说明

导入私钥

> 调用命令

```bash
	importprivkey privkey(s)
```

> 方法参数

|参数		|描述									|
|--			|--										|
|privkey(s)	|必填，单个私钥或私钥数组，				|
|rescan		|是否重新扫描该钱包的交易，默认为true	|



> e.g.

```bash
	importprivkey "PRIKEY"
```

> 返回值

```bash
	无
```

#### importaddress

> 方法说明

将address 添加到钱包中(无私钥),创建一个或多个只读账户

> 调用命令

```bash
	importaddress address(es)
```

> 方法参数

|参数		|描述									|
|--			|--										|
|address(es)|必填，单个地址或地址数组				|
|rescan		|是否重新扫描该钱包的交易，默认为true	|


> e.g.

```bash
	importaddress "ADDR"
```

> 返回值

```bash
	无
```

#### backupwallet

> 方法说明

备份wallet.dat钱包文件

> 调用命令

```bash
	backupwallet "destination"
```

> 方法参数

|参数	|描述	|
|--	|--	|
|destination	|文件路径	|


> e.g.

```bash
	backupwallet "backup.dat"
```

> 返回值

```bash
	无
```

#### dumpwallet

> 方法说明

将钱包中的全部私钥转储为文本文件

> 调用命令

```bash
	dumpwallet "filename"
```

> 方法参数

|参数	|描述	|
|--	|--	|
|	filename|导出到的文件路径	|


> e.g.

```bash
	dumpwallet "test"
```

> 返回值

```bash
	无
```

#### encryptwallet

> 方法说明

首次加密节点的钱包， 使用password作为解锁密码
请注意，加密之后无法还原到无密码状态，除非删除节点所有文件，重新同步区块！

> 调用命令

```bash
	encryptwallet "password"
```

> 方法参数

|	参数|描述	|
|--	|--	|
|	password|密码	|


> e.g.

```bash
	encryptwallet "password"
```

> 返回值

```bash
	无
```

注意，节点一旦加密，无论是重启节点还是修改密码，需要调用resume incoming,mining命令。

#### walletpassphrasechange

> 方法说明

更改钱包密码

> 调用命令

```bash
	walletpassphrasechange password newpassword
```

> 方法参数

|	参数|描述	|
|--	|--	|
|	password|旧密码	|
|	newpassword|新密码	|


> 返回值

```bash
	无
```

#### walletpassphrase

> 方法说明

使用password解锁节点的钱包, 在time时间内有效。

> 调用命令

```bash
	walletpassphrase password time
```

> 方法参数

|参数	|描述	|
|--	|--	|
|password	|密码	|
|	time|解锁时间，单位为秒	|


> e.g.

```bash
	walletpassphrase password 60
```

> 返回值

```bash
	true|false
```

#### validateaddress

> 方法说明

校验地址是否正确

> 调用命令

```bash
	validateaddress "address"|"pubkey"|"privkey"
```
> 方法参数

|参数	|描述	|
|--	|--	|
|	|地址或公钥或私钥	|


> e.g.

```bash
	validateaddress "ADDR"
```

> 返回值

```bash
	{
	  "isvalid" : true|false,          
	  "address" : "address",            
	  "ismine" : true|false,            
	  "isscript" : true|false,         
	  "pubkey" : "publickeyhex",        
	  "iscompressed" : true|false,     
	}
```
#### signmessage

> 方法说明

根据私钥对消息进行签名

> 调用命令

```bash
	signmessage "address|privkey" "message"
```

> 方法参数
> |参数		|描述			|
> |--			|--				|
> |address|privkey	|地址或私钥，若为地址，则必须保证该地址的私钥也在节点上			|
> |message	|要签名的消息	|


> e.g.

```bash
	signmessage "addr" "my message"
```

> 返回值

```bash
	签名后的字符串
```

#### verifymessage

> 方法说明

验证消息

> 调用命令

```bash
	verifymessage "publickey" "signature" "message"
```

> 方法参数

|参数	|描述	|
|--	|--	|
|publickey	|公钥	|
|signature	|签名后的字符串	|
|message	|签名前的消息	|


> 返回值

```bash
			true|false
```

#### listunspent

> 方法说明

查询地址未花费输出

> 调用命令

```bash
	listunspent ( minconf maxconf addresses )
```

> 方法参数

|参数		|描述								|
|--			|--									|
|minconf		|区块最小确认数						|
|maxconf		|区块最大确认数				|
|addresses	|要查询的地址集合	|


> e.g.

```bash
	listunspent 6 9999999 "[\"address1\",\"address2\"]"
```

> 返回值

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

#### listlockunspent

> 方法说明

返回钱包中未使用的事务输出锁定列表

> 调用命令

```bash
	listlockunspent
```
> 方法参数

无


> 返回值

```bash
	[
	  {
	   (string) "txid" : "TXID",              
	   (numeric) "vout" : n                    
	  }
	  ,...
	]
```

#### getmempoolinfo

> 方法说明

返回内存池信息

> 调用命令

```bash
	getmempoolinfo
```

> 方法参数

无

> 返回值

```bash
	{
		"size" : 0,
		"bytes" : 0
	}
```

#### addnode

> 方法说明

手动添加或删除点对点连接

> 调用命令

```bash
	addnode "node" "add"|"remove"|"onetry"
```

> 方法参数
> |参数	|描述	|
> |--	|--	|
> |node	|节点	|
> |command	|add将节点加入连接集合;remove将节点删除出集合;onetry尝试连接节点测试	|
> e.g.

```bash
	addnode "ip:port" "add"
```

#### getaddednodeinfo

> 方法说明

查询连接节点

> 调用命令

```bash
	getaddednodeinfo  ( "node" )
```

> 方法参数
> |参数	|描述	|
> |--	|--	|
> |(boolean)true/false|true=返回关于使用addnode添加节点的信息，false=返回使用addnode添加节点的列表	|
> |node	|包含或省略ip(:port)的节点	|

> e.g.

```bash
	getaddednodeinfo true
	getaddednodeinfo true "ip"
```

> 返回值

```bash
	[
		{
			"addednode": "xx.xxx.xxx.xx:xxxx", 
			"connected": true, 
			"addresses": 
				[
					{
						"address": "xx.xxx.xxx.xx:xxxx", 
						"connected": "outbound"
					}
				]
		}
	]
```

#### getnetworkinfo

> 方法说明

返回连接的网络IP及端口信息

> 调用命令

```bash
	getnetworkinfo
```

> 方法参数

无


> 返回值

```bash
	{
  "version": xxxxx,                        
  "subversion": "/x.x.x/",           
  "ProVer": xxxxx,                         
  "localservices": "xxxxxxxxxxxxxxxx",     
  "timeoffset": xxxxx,                     
  "connections": xxxxx,                   
  "networks": [                           
  {
    "name": "xxx",                        
    "limited": true|false,              
    "reachable": true|false,               
    "proxy": "host:port"                 
  }
  ,...
  ],
  "relayfee": x.xxxxxxxx,                 
  "localaddresses": [                      
  {
    "address": "xxxx",                   
    "port": xxx,                           
    "score": xxx                          
  }
  ,...
  ]
}
```

#### getpeerinfo

> 方法说明

返回连接到的其他节点的信息

> 调用命令

```bash
	getpeerinfo
```

> 方法参数

无

> 返回值

```bash
	[
	  {
		"id": n,                           
		"addr":"host:port",              
		"addrlocal":"ip:port",            
		"services":"xxxxxxxxxxxxxxxx",    
		"lastsend": ttt,                   
		"lastrecv": ttt,                  
		"bytessent": n,                    
		"bytesrecv": n,                    
		"conntime": ttt,                 
		"pingtime": n,                    
		"pingwait": n,                    
		"version": v,                    
		"subver": "/x.x.x/",      
		"handlocal": n,               
		"handshake": n,                    
		"inbound": true|false,             
		"startingheight": n,               
		"banscore": n,                     
		"synced_headers": n,               
		"synced_blocks": n,                
		"inflight": [
		   n,                           
		   ...
		]
	  }
	  ,...
	]
```

### 原始交易

#### lockunspent

> 方法说明

锁定或解锁未花费输出

> 调用命令

```bash
	lockunspent unlock [{"txid":"txid","vout":n},...]
```

> 方法参数

|参数	|描述				|
|--		|--					|
|unlock	|false锁定，true解锁|
|transactions	|要锁定或解锁的交易	|


> e.g.

```bash
	lockunspent false "[{\"txid\":\"txid\",\"vout\":1}]"
	lockunspent true "[{\"txid\":\"txid\",\"vout\":1}]"
```

> 返回值

```bash
	true or false,是否锁定或者解锁成功
```

#### createrawtransaction

> 方法说明

创建一个花费指定输入的事务，发送给给定的地址

> 调用命令

```bash
	createrawtransaction [{"txid":"id","vout":n},...] {"address":amount,...} ( [data] "action" )
```

> 方法参数

|参数	|描述	|
|--	|--	|
|transactions	|交易集合	|
|addresses	|地址：数量	|
|data	|附加数据	|
|action	|默认为“”,lock（锁定钱包中的给定输入），sign（使用钱包密钥签署交易），lock,sign，send（签署并发送交易	|



> e.g.

```bash
	createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "{\"address\":0.01}"
```

> 返回值

```bash
	#若action为""或lock或send
	hex
	#若action为其他几种
	{                                                        
	  "hex": "value",                                      
	  "complete": true|false                                 
	}
```

#### getrawtransaction

> 方法说明

查询交易信息。

> 调用命令

```bash
	getrawtransaction "txid" 
```

> 方法参数

|参数			|描述								|
|--				|--									|
|txid			|txid								|

> e.g.

```bash
	getrawtransaction "txid"
```

> 返回值

```bash
	hex 交易的完整hex信息
```

#### decoderawtransaction

> 方法说明

解码交易的TX-HEX

> 调用命令

```bash
	decoderawtransaction "tx-hex"
```

> 方法参数

|参数	|描述	|
|--	|--	|
|tx-hex	|要解析的hex	|


> 返回值

```bash
	{
	  "txid" : "id",                  
	  "version" : n,                   
	  "locktime" : ttt,                
	  "vin" : [
		 {
		   "txid": "id",              
		   "vout": n,                
		   "scriptSig": {             
			 "asm": "asm",            
			 "hex": "hex"            
		   },
		   "sequence": n              
		 }
		 ,...
	  ],
	  "vout" : [
		 {
		   "value" : x.xxx,           
		   "n" : n,                    
		   "scriptPubKey" : {
			 "asm" : "asm",            
			 "hex" : "hex",            
			 "reqSigs" : n,            
			 "type" : "pubkeyhash",    
			 "addresses" : [
			   "address"  address
			   ,...
			 ]
		   }
		 }
		 ,...
	  ],
	}
```

#### fundrawtransaction

> 方法说明

接受createrawtransaction的输出。指定找零地址以及手续费

> 调用命令

```bash
	fundrawtransaction "tx-hex" "address" ( fee )
```

> 方法参数

|参数	|描述		|
|--		|--			|
|tx-hex	|hex		|
|address|找零地址	|
|fee	|手续费		|

> e.g.

```bash
	fundrawtransaction "HEX""ADDR"
	fundrawtransaction "HEX""ADDR" 0.01
```

> 返回值

```bash
	hex
```

#### signrawtransactionwithkey

> 方法说明

签署原始交易

> 调用命令

```bash
	signrawtransactionwithkey "tx-hex" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )
```

> 方法参数

|参数	|描述	|
|--	|--	|
|tx-hex	|hex	|
|prevtxs	|交易数组	|
|privatekeys	|私钥数组	|
|sighashtype	|签署类型，默认为ALL|

> e.g.

```bash
	signrawtransactionwithkey "HEX-STR"
```

> 返回值

```bash
	{
	  "hex": "value",                          
	  "complete": true|false                    
	}
```

#### sendrawtransaction

> 方法说明

进行广播交易。

> 调用命令

```bash
	sendrawtransaction "tx-hex" ( allowhighfees )
```

> 方法参数

|参数			|描述								|
|--				|--									|
|tx-hex			|tx-hex								|
|allowhighfees	|默认为false,是否允许更高的手续费	|


> e.g.

```bash
	sendrawtransaction "signedhex"
```

> 返回值

```bash
	txid
```

#### sendtoaddress

> 方法说明

GKM 转账

> 调用命令

```bash
	sendtoaddress "address" amounttosend
```

> 方法参数

|参数			|描述								|
|--				|--									|
|address			|目的地址						|
|amounttosend	|转账金额。 比如 0.1	|


> e.g.

```bash
	sendtoaddress "GM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1
```

> 返回值

```bash
	txid
```

#### gkm_getbalance

> 方法说明

返回地址代币余额

> 调用命令

```bash
	gkm_getbalance "address" propertyid
```

> 方法参数

|参数			|描述								|
|--				|--									|
|address			|查询地址							|
|propertyid	|代币标识	|


> e.g.

```bash
	gkm_getbalance "GM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1
```

> return value

```bash
	{
		"balance" : "n.nnnnnnnn",   
		"reserved" : "n.nnnnnnnn"   
		"frozen" : "n.nnnnnnnn"   
	}
```

#### gkm_send

> 方法说明

创建并广播代币交易

> 调用命令

```bash
	gkm_send "fromaddress" "toaddress" propertyid "amount"
```

> 方法参数

|参数			|描述								|
|--				|--									|
|fromaddress			|源地址			|
|toaddress	|接收地址	|
|propertyid	|代币标识	|
|amount	|转账金额	|


> e.g.

```bash
	gkm_send "GM9qvHKtgARhqcMtM5cRT9VaiDJ5PSfQGY" "G7FaKponF7zqoMLUjEiko25pDiuVH5YLEa" 3 "100.0"
```

> 返回值

```bash
	txid
```

#### gkm_gettransaction

> 方法说明

获取Gkm代币交易详情。

> 调用命令

```bash
	gkm_gettransaction "txid"
```

> 方法参数

|参数			|描述								|
|--				|--									|
|txid			|待查找交易ID|


> e.g.

```bash
	gkm_gettransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"
```

> 返回值

```bash
	{
		"txid" : "hash",                  
		"sendingaddress" : "address",     
		"referenceaddress" : "address",   
		"ismine" : true|false,            
		"confirmations" : nnnnnnnnnn,     
		"fee" : "n.nnnnnnnn",             
		"blocktime" : nnnnnnnnnn,         
		"valid" : true|false,             
		"invalidreason" : "reason",     
		"version" : n,                    
		"type_int" : n,                   
		"type" : "type",                  
	}
```

#### getgidinfo

> 方法说明

获取某地址的gip信息

> 调用命令

```bash
	getgidinfo "address"
```

> 方法参数

|参数			|描述								|
|--				|--									|
|address			|待查地址|


> e.g.

```bash
	getgidinfo "g7iNckcuHRjh7DMSTxyFt9TYte485iyrnn"
```

> 返回值

```bash
	{
		"isgid": xxxxx,                 
		"txid": xxxxx,                 
		"address": xxxxx,              
		"inviter": xxxxx,                             
	}
```

#### creategip

> 方法说明

创建GIP。

> 调用命令

```bash
	creategip "from" "to" amounttosend "inviter"
```

> 方法参数

|参数			|描述								|
|--				|--									|
|from			|源地址							|
|to	|目的地址	|
|amounttosend	|创建GIP所需GKM金额	|
|inviter	|GIP邀请地址|


> e.g.

```bash
	creategip "GM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" "GM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 10 "GM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd"
```

> 返回值

```bash
	txid
```

# 3.示例

## 3.1 常规交易示例
GKM采用UTXO 模型作底层存储的数据结构，全称为 Unspent Transaction output，也就是未被使用的交易输出。账户中的余额并不是由一个数字表示的，而是由当前区块链网络中所有跟当前账户有关的 UTXO 组成的。
### 3.1.1 创建多个新地址
```bash
	getnewaddress
	addr1
	getnewaddress
	addr2
	getnewaddress
	addr3
```
### 3.1.2 发送GKM
```bash
	sendtoaddress addr1 10
	txid1

```
### 3.1.3 发送token
```bash
	gkm_send addr1 addr2 3 10
	txid1
```
## 3.2多签账户交易及样例

### 3.2.1 多签账户简介

GKM使用“ x/y ” 多签地址，定义为:给定y个常规地址，至少对应于x个地址的私有密钥必须同时签署事务才能执行执行交易或事务。

### 3.2.2 创建多签账户地址

* 查找地址，找到地址addr1,addr2
* 创建共管账户 节点一运行以下命令来创建2/2的多重地址并将其添加到节点的钱包中:
```bash
	addmultisigaddress 2 '["A1-publickey", "A2-publickey"]'
```
得到多签地址MA1A2 ，并自动开启跟踪 节点二可重复节点一命令
```bash
	addmultisigaddress 2 '["A1-publickey", "A2-publickey"]'
```
### 3.2.3 通过多签账户交易

通过多签账户发送资金。因为这是一个2/2的共管合约账户，所以这个过程将需要两个地址的共同签名。
* 节点一:创建交易并部分签名
```bash
	createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "{\"address\":0.01}"
  {
      "hex" : "XXXXXXXXXXXXXXXXXXXXXXXX",
      "complete" : false
  }
```
响应complete:false，十六进制hex字段HEX1。已经部分签名。
* 节点二签名:
```bash
	  signrawtransactionwithkey HEX1
  
	  {
		  "hex" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
		  "complete" : true
	  }
```
响应complete:true，十六进制hex字段HEX2。这意味着交易签名完成，可以向区块链广播。
* 广播交易
```bash
	sendrawtransaction HEX2
```

##  JSON RPC配置

RPC用户名密码存储在~/.gkm/gkm.conf（linux/MAC）或%APPDATA%\gkm\gkm.conf（Windows）文件中,可以使用gkm-cli命令行工具或者gkm GUI工具(PC端钱包)内置的CLI界面连接，这些工具会自动读取RPC用户名密码并连接至已运行区块链,请不要将rpc port开放到外网。

> 命令方式运行：

```bash
	[command] [params]
```
