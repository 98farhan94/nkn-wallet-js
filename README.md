# nkn-wallet-js

JavaScript implementation of NKN wallet.

**Note: This repository is in the early development stage and may not have all
functions working properly. It should be used only for testing now.**

## Build
```
npm install
grunt dist
```

## Usage

For browser, use `dist/nkn-wallet.js` or `dist/nkn-wallet.min.js`.

NKN wallet only stores some static information such as encrypted private keys, addresses and so on.
All dynamic information needs to be queried from a running nkn node.
So you need some configure first before you use the wallet sdk.

First of all, set the node you want to link and the NKN token id:
```javascript
nknWalletSDK.configure({
    //this is a node ip address in our test net
    rpcNode: '35.229.164.111', 
    
    //rpc port of the node
    rpcPort: '30003', 
  })
```

Then you can do everything you want to with the wallet sdk.

## API

+ nknWalletSDK 

```javascript

/***
 * global configuration:
 * {
 *  assetId: '',  // the NKN Token id, default value: 4945ca009174097e6614d306b66e1f9cb1fce586cb857729be9e1c5cc04c9c02
 *  rpcNode:'',   // node ip for dynamic information query, default value: 127.0.0.1
 *  rpcPort:'',   // node port for dynamic information query, default value: 30003
 * }
 *
 * @param config | Object
 */
nknWalletSDK.configure(config)
```

```javascript
/**
 * create a new wallet
 * @param password : string : the password to encrypt wallet
 * @returns {NknWallet} : a NknWallet instance
 */
nknWalletSDK.newWallet(password)
```

```javascript
/***
 * load wallet from json string
 * @param walletJson : string : a json format wallet
 * @param password : string : password for this wallet
 * @returns {NknWallet | null} : return NknWallet instance or null if key information is missing.
 * 
 * !this method will thow an error if the password is wrong!
 */
nknWalletSDK.loadJsonWallet(walletJson, password)
```

```javascript
/***
 * restore a wallet from private key
 * @param privateKey : string : the private key for wallet restore
 * @param password : string : password for new wallet
 * @returns {NknWallet} : a NknWallet instance
 */
nknWalletSDK.restoreWalletByPrivateKey(privateKey, password)
```

+ NknWallet 

All of the following methods are instance methods

```javascript
/***
 * generate wallet json
 * @returns {string} : wallet json
 */
toJSON()
```

```javascript
/***
 * get the public key of this wallet
 * @returns {string} : the public key of this wallet
 */
getPublicKey()
```

```javascript
/***
 * get the private key of this wallet
 * @returns {string} : the private key of this wallet
 *
 * !!! anyone with the private key has the power to restore a full-featured wallet !!!!
 */
getPrivateKey()
```

```javascript
/***
 * transfer nkn to some valid address
 * @param toAddress : string : valid nkn address
 * @param value : number : value for transfer
 * @param password : string : wallet password
 * @param success : function : callback for transfer success
 * @param fail : function : callback for all errors that happened in transfer
 *
 * !!! the fail function will be called for any transfer errors  
 *     and the parameter applied is a WalletError instance. !!!
  */
transferTo(toAddress, value, password, success, fail)
```

```javascript
/***
 * query asset balance
 * @param success : function : callback for query success and the parameter is a decimal.js instance
 * @param fail : function : callback for query fail
 * @returns {boolean} : true for http request success and false for fail
 */
queryAssetBalance(success, fail)
```

```javascript
/***
 * recharge the prepaid balance
 * @param value : number : how much NKN you want to prepay
 * @param rates : number : how much NKN you want to pay for one data transfer
 * @param password : string : password for this wallet
 * @param success : function : callback for prepay success
 * @param fail : function : callback for prepay failed
 */
prepay(value, rates, password, success, fail)
```

```javascript
/***
 * query the prepay balance
 * @param success : function(data) : callback for query success
 *                                    the data's will be an object like this {Amount: string, Rates: string}
 * @param fail : function : callback for query failed.
 */
queryPrepaiedInfo(success, fail)
```

## Examples
+ create a new wallet
```javascript
const wallet = nknWallet.newWallet('pwd')
```

+ get wallet's json string
```javascript
const walletJson = wallet.toJSON()
```

+ load wallet from a wallet json string
```javascript
const walletFromJson = nknWallet.loadJsonWallet(walletJson, 'pwd')
```

+ restore wallet from a private key
```javascript
const walletFromPrivateKey = nknWallet.restoreWalletByPrivateKey('the private key', 'new-wallet-password')
```

+ transfer asset to some address
```javascript
wallet.transferTo('some valid address', 100, 'pwd', 
  function(data) {
    console.log('success: ', data)
  }, 
   function(error) {
    console.log('fail: ', error)
  })
```

+ query asset balance for this wallet
```javascript
wallet.queryAssetBalance(
  function(value) {
    console.log('asset balance for this wallet is: ', value.toString())
  }, function(error) {
    console.log('query balance fail: ', error)
  })
```
