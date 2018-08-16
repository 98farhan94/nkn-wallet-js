# nkn-wallet-js

JavaScript implementation of NKN wallet.

**Note: This repository is in the early development stage and may not have all
functions working properly. It should be used only for testing now.**

## Install

For npm:

```shell
npm install nkn-wallet
```

And then in your code:

```javascript
const nknWalletSDK = require('nkn-wallet');
```

For browser, use `dist/nkn-wallet.js` or `dist/nkn-wallet.min.js`.

## Usage

+ import
```javascript
const nknWalletSDK = require('nkn-wallet');
```

+ create a new wallet
```javascript
const wallet = nknWalletSDK.newWallet('pwd')
```

+ get wallet's json string
```javascript
const walletJson = wallet.toJSON()
```

+ load wallet from a wallet json string
```javascript
const walletFromJson = nknWalletSDK.loadJsonWallet(walletJson, 'pwd')
```

+ restore wallet from a private key
```javascript
const walletFromPrivateKey = nknWalletSDK.restoreWalletByPrivateKey('the private key', 'new-wallet-password')
```

+ transfer asset to some address
```javascript
wallet.queryAssetBalance().then(function(value) {
  console.log('asset balance for this wallet is: ', value.toString())
}).catch(function(error) {
  console.log('query balance fail: ', error)
})
```

+ query asset balance for this wallet
```javascript
wallet.transferTo(wallet.address, 100, 'pwd').then(function(data) {
  console.log('success: ', data)
}).catch(function(error) {
  console.log('fail: ', error)
})
```

Check [examples](examples) for full examples.

## Configure

NKN wallet only stores some static information such as encrypted private keys,
addresses and so on. All dynamic information needs to be queried from a NKN
node. By default the SDK will try to use RPC server provided by us, but you can
change it (together with NKN token ID) by calling the global configure function:

```javascript
nknWalletSDK.configure({
  rpcAddr: 'http://127.0.0.1:30003',
})
```

Note that configure is optional. If you don't call `configure()`, default
configurations will be used.

## API

+ nknWalletSDK

```javascript

/***
 * global configuration:
 * {
 *  assetId: '',  // the NKN Token id, default value: 4945ca009174097e6614d306b66e1f9cb1fce586cb857729be9e1c5cc04c9c02
 *  rpcAddr:'',   // node addr for dynamic information query, default value: http://cluster2-oregon.nkn.org:30003
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
 *
 * !!! the fail function will be called for any transfer errors  
 *     and the parameter applied is a WalletError instance. !!!
  */
transferTo(toAddress, value, password)
```

```javascript
/***
 * query asset balance
 * @returns {promise} : if resolved, the parameter is a decimal.js instance
 */
queryAssetBalance()
```

```javascript
/***
 * recharge the prepaid balance
 * @param value : number : how much NKN you want to prepay
 * @param rates : number : how much NKN you want to pay for one data transfer
 * @param password : string : password for this wallet
 */
prepay(value, rates, password)
```

```javascript
/***
 * query the prepay balance
 * @returns {promise} : if resolved, the data's will be an object like this {Amount: string, Rates: string}
 */
queryPrepaiedInfo()
```
