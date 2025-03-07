

# New Transaction Block Structure



## 1. New transaction block structure changes

​	The new 512-byte transaction block data includes a new field: ***XDAG_FIELD_TRANSACTION_NONCE***. This field is ***located after the Header Field***.

- Old 512-byte transaction block data ***(Does not include the transaction nonce field)***:

  ```
  0000000000000000c19d5605000000002d0445659b01000000e1f50500000000  // header
  1200000075894a638afe14b6fdb34b26548cd189e81912169a99991900000000  // input
  000000004465b8b0ba4893a35511cfe245496d6f759309b19a99991900000000  // output
  3838383800000000000000000000000000000000000000000000000000000000  // remark
  5efa9b70b456768cf532e999429fbb48a0391cd83a4265be40942627db095c04  // publicKey
  d31ab87f0255b5ef0efb92ec8711b243303e2173d17378da662b9da42de0b8b0  // signature_R
  0b2f8f1c4735c47eb8b682e2a55440dbfd3516f1be54b4f5b0d26c58c33d7b25  // signature_S
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  ```

- New 512-byte transaction block data ***(Includes the transaction nonce field)***:

  ```
  0000000000000000e1dc6955000000009239ab5d9c01000000e1f50500000000 // header
  0000000000000000000000000000000000000000000000000a00000000000000 // transaction nonce
  00000000640ccd278a792930126c5b5ffa6f827250b13ffb9a99991900000000 // input
  000000004465b8b0ba4893a35511cfe245496d6f759309b19a99991900000000 // output
  3838383800000000000000000000000000000000000000000000000000000000 // remark
  fc167f72bacf6eb8cd53321ae39a84171902311e39fc75e65edb77a620562a53 // public key
  60b380ce0a11f39bfe5fdbbc4e0885ad411987e75b32ecd42f62b7ec66725fed // signature_R
  0d451ac4425a8f7bebe6ce503227fe58e906e312073d26ceb6593bed64d4e62a // signature_s
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000
  ```



## 2. Field type of XDAG_FIELD_TRANSACTION_NONCE

​	The ***field type*** of XDAG_FIELD_TRANSACTION_NONCE is ***0x0E***.



## 3. Method to get the transaction nonce

​	The transaction nonce is obtained through an ***RPC method*** named ***xdag_getTransactionNonce***. This RPC method takes the ***sender's account address*** as a parameter (e.g., PuUubHqqkRaoiJpYCVWJ5pRyCpUGbGKSH) and returns the result as a ***String type***.

- The ***request*** and ***response*** of the ***xdag_getTransactionNonce*** method:

```shell
# Request:
curl http://127.0.0.1:10001/ -s -X POST -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"xdag_getTransactionNonce\",\"params\":[\"PuUubHqqkRaoiJpYCVWJ5pRyCpUGbGKSH\"],\"id\":1}"

# Response:
{"jsonrpc":"2.0","id":1,"result":"15"}
```



## 4. Conversion of transaction nonce

​	The transaction nonce obtained through the RPC method needs to be converted into a ***little-endian hexadecimal string*** with a length of ***8 bytes*** (a total of ***16 hexadecimal characters***).

- ***example***: Assuming the transaction nonce obtained through the RPC method is ***`10`*** (String type), after conversion, it will be ***`0a00000000000000`*** (little-endian hexadecimal string).



## 5. Fill the transaction nonce into the 512-byte transaction data

​	After conversion, the transaction nonce will be padded to ***64 hexadecimal characters*** and then filled into the ***512-byte transaction data***.

- ***example***: Assuming the converted transaction nonce is ***`0a00000000000000`***, after padding 48 zeros in front of ***`0a00000000000000`***, the data length will be 64 hexadecimal characters. The resulting string will be:

  ***`0000000000000000000000000000000000000000000000000a00000000000000`***.


- ***`0000000000000000000000000000000000000000000000000a00000000000000`*** is the transaction nonce data to be filled into the 512-byte transaction data.


