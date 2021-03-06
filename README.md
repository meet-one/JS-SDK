# DApp JS-Bridge SDK

MEET.ONE has supported Scatter protocol from the 1.3.0 version，so you can see the [Scatter Document](https://get-scatter.com/docs/dev/setting-up-for-web-apps)

Below version 1.3.0 you can use MEET.ONE JS-Bridge.

-------

[The Bridge Library](https://meetone.gitlab.io/meet-bridge/) for Meet.ONE Client

This library is used to assist to generating the protocol URI of the client, and encapsulates some common protocols and methods.

[DEMO](https://meet.one/test/index.html)

## GETTING STARTED  

### How to import  

```    
<!-- import meet.one bridge library -->
<script src="https://cdn.jsdelivr.net/npm/meet-bridge@1.0.10/dist/meet-bridge.umd.min.js"></script>    
```

or    

```    
npm i meet-bridge
```    



### How to generate protocol uri
The following is an example of how to generate protocol uri to ask for authorize

```
var bridge = new Bridge(); // 1. Creates an instance of Bridge.

// 2. generate autorize protocol uri
var uri = bridge.invokeTransfer({
amount: 1, 
to: 'eosiomeetone', 
tokenName: 'EOS,
tokenContract: 'eosio.token',
tokenPrecision: 4,
orderInfo: 'eosiomeetone', 
memo: 'The EOS Candy Box'
});

// 3. After the protocol URI address is generated, the web terminal can communicate with the client through the following two calling methods.
window.location.href = uri;
window.postMessage(uri);
```

### How to receive message from client
Receiving and parsing the things returned by the client can be achieved by listening to the Message event.

```
window.document.addEventListener('message', function(e) {
const message = e.data; // receive original message
document.getElementById('receive').innerHTML = messgae;

// can also be directly converted to an Object using `Bridge.revertParamsToObject(params)`;
const {params} = JSON.parse(message);
document.getElementById('decode').innerHTML = decodeURIComponent(atob(params));
});
```

### Explanation Of Decoded Message   

**Params In Decoded Message**   

* `code`: `Int` success = 0，failure != 0  

* `type`: `Int`transfer = 0;authorize = 1;getAccountInfo = 2;getBalance = 3;getWalletList = 4;pushTransactions = 5;getSignature = 6;   

* `data`: `Object` detail datas from different methods    


## Constructors
Creates an instance of Bridge.


```
new Bridge(scheme?: string): Bridge
```


**params：**
* `scheme`: `String` default = "meetone://"

**return：**

* `bridge`: `Bridge` an instance of Bridge


## EOS Methods

### 1.invokeAuthorizeInWeb
Get current account name in MEET.ONE Wallet.

```
bridge.invokeAuthorizeInWeb(params: object)
```


**params：**
* none

**return：**

* `uri`: `String` The protocol of uri

**e.g. for returned decoded message**   

```
{"code":0,"type":1,"data":{"account":"eosiomeetone"}}
```

### 2.invokeAccountInfo
Get current account info in MEET.ONE Wallet.

```
bridge.invokeAccountInfo(params: object)
```


**params：**
* none

**return：**

* `uri`: `String` The protocol of uri

**e.g. for returned decoded message**   

```
{"code":0,"type":2,"data":{"account":"ifuckalibaba", "publicKey":"", "isOwner":true, "currencyBalance":"0.0001"}}
```


### 3.invokeTransfer
Send a transfer request

```
bridge.invokeTransfer(params: object)
```


**params：**
* `to`: `String`
* `amount`: `String` 
* `tokenName`: `String` e.g. EOS
* `tokenContract`: `String`e.g. eosio.token
* `tokenPrecision`: `Int` e.g. 4
* `memo`: `String`
* `orderInfo`: `String` 

**return：**

* `uri`: `String` The protocol of uri


**e.g. for returned decoded message**   

```
{"code":0,"type":5,"data":Object/*retured data in chain*/}
```


### 4.invokeTransaction
Send a transaction request

```
bridge.invokeTransaction(params: object)
```


**params：**
* `actions`: `Array` transaction Actions
* `options`: `object` transaction Options
  * `broadcast`: `boolean`

**return：**

* `uri`: `String` The protocol of uri


**e.g. for returned decoded message**   

```
{"code":0,"type":5,"data":Object/*retured data in chain*/}
```


### 5.invokeSignature
Get a custom signature

```
bridge.invokeSignature(params: object)
```


**params：**
* `data`: `String` custom signature string

**return：**

* `uri`: `String` The protocol of uri


**e.g. for returned decoded message**   

```
//success
{"code":0,"type":6,"data":{"signature":"SIG_K1_KZUDvvbVjR1mZUoazS8VWPXFdonbsazt8jR9NGDnwb3wRQpAYZxw9Xpi6iBjinb4raozLX2pmkPYNZWYuxAmzgFTJigjZ5","account":"wujunchuan11","isOwner":true}}

//failed
{"code":500,"type":6,"data":{"origin":{"name":"AssertionError","actual":false,"expected":true,"operator":"==","message":"data is a required String or Buffer","generatedMessage":false},"message":"签名失败"}}
```

### 6.invokeBalance
Get account token balance

```
bridge.invokeBalance(params: object)
```


**params：**
* `contract`: `String` token contract,default is 'eosio.token'   
* `symbol`: `String` token symbol,default is 'EOS'   
* `accountName`: `String` default is wallet current account name   

**return：**

* `uri`: `String` The protocol of uri


**e.g. for returned decoded message**   

```
//success
{"code":0,"type":3,"data":{"accountName":"wujunchuan11","symbol":"EOS","contract":"eosio.token","balance":"0.0001"}}
```



## Other Methods 
Open the [link](https://meetone.gitlab.io/meet-bridge/classes/bridge.html),you can see the details.

## Test In MEET.ONE 
![image](https://github.com/meet-one/JS-SDK/raw/master/testInMeetOne.jpg)
