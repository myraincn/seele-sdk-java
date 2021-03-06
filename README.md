# SEELE-SDK-JAVA

SEELE-SDK-JAVA is a generic java API library for the Seele blockchain.<br>
The program is a maven project.


# Methods

## Sign

	API：static String sign(SignTransactionDTO transactionDTO)

	Generate transaction and sign, the rawTx transactionDTO be in the example format, otherwise an error will occur.

	example：
```java
SignTransactionDTO signTransactionDTO = new SignTransactionDTO();
signTransactionDTO.setPrivateKey("0xd738b0c1198e55050f754bdf0f824ee4febd962a6b751faab86c081ad5033b0d");
RawTx rawTx = new RawTx();
rawTx.setTo("0x0a57a2714e193b7ac50475ce625f2dcfb483d741");
rawTx.setFrom("0xb265a2e04087a9a83492ffe191316f46b4730751");
rawTx.setAmount(0);
rawTx.setAccountNonce(0);
rawTx.setTimestamp(0);
rawTx.setGasPrice(1);
rawTx.setGasLimit(3000000);
signTransactionDTO.setRawTx(rawTx);

String jsonStr = SeeleTransactionManager.sign(signTransactionDTO);
```
the successful response message is：
		
     {
	    "result":
	     {
		   "data":
		    {
		      "from":"0xe95d99fec90954eb8f6f899c188aef5caa20d501",
		      "to":"0x0a57a2714e193b7ac50475ce625f2dcfb483d741",
		      "amount":0,
		      "accountNonce":0,
		      "gasPrice":1,
		      "gasLimit":3000000,
		      "timestamp":0,
		      "payload":""
		     },
		      "signature:{
		      "sig":"ob6nXGQy7VKylMPHJTfmxbsJZVQr6HdV2U/dYF/bc9kIU55u/2HMWo16ngsIWlo87aZCqlUY6H5h1+boImfDowA="
	             },
		      "hash":"0x78be64c6d3c1438184713f3dc1c207eeb93543d82808292b8ce74019511cb057"
		 }
         }

the failure response message is:
		
	{
		errMsg: "generateTx failed:"
	}

------------------------------------------------------------------------------

## SendTx

	API:static String sendTx(SignTransactionDTO signTransactionDTO,String uri)

	send transaction , the SignTransactionDTO must be in the example format, otherwise an error will occur.

	example：
	
```java
SignTransactionDTO signTransactionDTO = new SignTransactionDTO();
signTransactionDTO.setPrivateKey("0xd738b0c1198e55050f754bdf0f824ee4febd962a6b751faab86c081ad5033b0d");
RawTx rawTx = new RawTx();
rawTx.setTo("0x0a57a2714e193b7ac50475ce625f2dcfb483d741");
rawTx.setFrom("0xb265a2e04087a9a83492ffe191316f46b4730751");
rawTx.setAmount(0);
rawTx.setAccountNonce(0);
rawTx.setTimestamp(0);
rawTx.setGasPrice(1);
rawTx.setGasLimit(3000000);
signTransactionDTO.setRawTx(rawTx);

String jsonStr = SeeleTransactionManager.sendTx(signTransactionDTO, "http://117.50.20.225:8037");
```
		the successful response message is：
		
		{
			"result":
			{
				"result":true,
				"id":1542684211408,
				"jsonrpc":"2.0"
			}
		}

		the failure response message is：
		
		{
			errMsg: "error:addTransactionDTO is null"
		}

------------------------------------------------------------------------------

## GetTxByHash

	API:static String getTxByHash(String hash,String uri)

	get transaction by hash, the hash must be in the example format(64 length), otherwise an error will occur.

	example：
```java
String jsonStr = 
SeeleTransactionManager.getTxByHash("0x03f097fef1bbda18257b020b80a3a79834bcd324635fcc4f932173c1767c2889"
, "http://117.50.20.225:8037");
```
the successful response message is：
		
        {
                "result":
                {
                        "result":
		        {
			        "blockHash":"0x00000152de8784bc264cc43d05b1ac5da040141aeb087ca01761a2028b6fd7f7",
			         "blockHeight":3,
			         "transaction":
			         {
			 	        "gasLimit":3000000,
			 	        "amount":0,
			 	        "payload":"",
			 	        "from":"0xe95d99fec90954eb8f6f899c188aef5caa20d501",
			 	        "to":"0x0a57a2714e193b7ac50475ce625f2dcfb483d741",
			 	        "accountNonce":0,
			 	        "hash":"0x78be64c6d3c1438184713f3dc1c207eeb93543d82808292b8ce74019511cb057",
			 	        "gasPrice":1
			        },
			        "txIndex":1,
			        "status":"block"
		        },
		        "id":1542962072982,
		        "jsonrpc":"2.0"
	         }
        }
	
the failure response message is ：
		
	{
		errMsg: "leveldb: not found"
	}

------------------------------------------------------------------------------

## Key

	API:static String key(int shard)

	generate the private key ,the parameter shard must between 1 and 2

	example：
```java
String jsonStr = KeyManager.key(1);
```

		the successful response message is ：
		
		{
			"result":
			{
				"public key":"0xe95d99fec90954eb8f6f899c188aef5caa20d501",
				"private key":"0xa417551e1522d88d8b2c1592f9e273f7f8bf68517195418b4b21d40e17cdaa1f"
			}
		}

------------------------------------------------------------------------------

## GetAddress

	API: static byte[] getAddress(byte[] pubString)

	get public address by pubString

	example：
```java
 String pubString = 
 "040947751e3022ecf3016be03ec77ab0ce3c2662b4843898cb068d74f698ccc8ad75"
 +"aa17564ae80a20bb044ee7a6d903e8e8df624b089c95d66a0570f051e5a05b";
 byte[] pubKey = Hex.decode(pubString);
 String address = Hex.toHexString(AddressManager.getAddress(pubKey)); 
```

------------------------------------------------------------------------------

## GetBalance

	API: static String getBalance(String accountAddress,String uri)

	get balance by account address

	example：
```java
String jsonResult 
= AddressManager.getBalance("0xe95d99fec90954eb8f6f899c188aef5caa20d501","http://117.50.20.225:8037"); 
```
the successful response message is ：
		
	{
	   "result":
		{
		    "result":
		    {
			    "Account":"0xe95d99fec90954eb8f6f899c188aef5caa20d501",
			    "Balance":999999832000
		    },
		    "id":1543308520798,
		    "jsonrpc":"2.0"
		}
	}
the failure response message is ：
		
	{
		errMsg: "invalid argument 0: invalid address length 2, expected length is 20"
	}
------------------------------------------------------------------------------

## AddressValidate

	API: static boolean addressValidate(String address)

	Check whether the address is legitimate.If success return true,otherwise return false.

	example：
```java
String addr = "4c10f2cd2159bb432094e3be7e17904c2b4aeb21";
boolean isSuccess = AddressManager.addressValidate(addr)
```

------------------------------------------------------------------------------
		
# To build this project:
	
	Run $ mvn clean package

