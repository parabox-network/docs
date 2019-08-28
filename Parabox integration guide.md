### Parabox integration guide

#### 1. Create an account

   1. Create through a browser built-in wallet or mobile wallet:

      Create it directly according to the page prompt.

      Parabox browser built-in wallet：

      ​	Main network：<https://explorer.parabox.cc/#/wallet> 

      ​	Test network：<http://39.100.63.66/#/wallet> 

      Android ：

      ​	Main network：<https://download.parabox.cc/parabox.apk>  

      ​	Test network：<http://39.100.63.66:8091/parabox.apk>

      iOS ：

      ​	Main network：<https://testflight.apple.com/join/dB6G64aV>

      ​	Test network：https://testflight.apple.com/join/cnH8To1m

   2. Create through SDK：

      1. Through  JS SDK：

         https://github.com/parabox-network/parabox-sdk-js/blob/master/example/createAccount.js

      2. Through  Java SDK：

         ```java
            ECKeyPair keyPair = Keys.createEcKeyPair();

            System.out.println("Address: 0x" + Keys.getAddress(keyPair));
            System.out.println("PublicKey: 0x" + keyPair.getPublicKey());
            System.out.println("PrivateKey: 0x" + keyPair.getPrivateKey());
         ```

   3. Use Ethereum account：

      The Parabox account is compatible with ethereum account format. You can import the mnemonic, keystore or private key of your ethereum account into the Parabox wallet for direct use；

   4. Test network account (with balance):

      ```
      {
        "address": "0xea57cde6138f1c049fc3894331e3ab16c5a07896",
        "private": "0x74e2aa5ae5e29aa00bd7a30b8f47db1990e57b66feb9e27984bc5d2c8e7e35be"
      }
      ```

#### 2. Inquiry account balance：

   1. Through JS SDK：

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/example/getBalance.js>

   2. Through Java SDK： 

      ```java
           Paraboxj service = Paraboxj.build(new HttpService(rpcUrl));
      
           BigInteger balance = service.appGetBalance(
                   "0xea57cde6138f1c049fc3894331e3ab16c5a07896",
                   DefaultBlockParameterName.LATEST
           ).send().getBalance();
      
           System.out.println("Balance: " + balance);
      ```

   3. Through JsonRpc interface：

      ```sh
      curl -X POST --data '{"jsonrpc":"2.0","method":"getBalance","params":["0xea57cde6138f1c049fc3894331e3ab16c5a07896", "latest"],"id":1}' 47.92.173.78:1337
      ```

#### 3. Send a transaction

   1. Through JS SDK：

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/example/transfer.js>

   2. Through Java SDK： 
   
      ```java
         Paraboxj service = Paraboxj.build(new HttpService(rpcUrl));

        // create a signed transaction
        String to = "0xf499ed0e7a5c28bcf610ff4c866c8e5ea421f63c";

        Random random = new Random(System.currentTimeMillis());
        String nonce = String.valueOf(Math.abs(random.nextLong()));

        long quota = 20000000L;

        long valid_until_block =
                service.appBlockNumber().send().getBlockNumber().longValue() + 88;

        AppMetaData.AppMetaDataResult metaDataResult =
                service.appMetaData(DefaultBlockParameterName.PENDING).send().getAppMetaDataResult();

        int version = metaDataResult.getVersion();
        BigInteger chainId = metaDataResult.getChainId();

        String value = "100000000";
        String data = "";
        Transaction tx = new Transaction(
                to, nonce, quota, valid_until_block,
                version, chainId, value, data
        );

        String privateKey = "0x74e2aa5ae5e29aa00bd7a30b8f47db1990e57b66feb9e27984bc5d2c8e7e35be";
        String signedData = tx.sign(privateKey);

        AppSendTransaction.SendTransactionResult sendTransaction =
                service.appSendRawTransaction(signedData).send().getSendTransactionResult();

        // get hash of the transaction
        String hash = sendTransaction.getHash();
      ```
      

#### 4.  Inquiry Transaction Receipt

   1. Through JS SDK：

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/example/getTransactionReceipt.js>

   2. Through Java SDK：
      ```java
         Paraboxj service = Paraboxj.build(new HttpService(rpcUrl));
      
         String hash = "0x0e77f2076377117d942c14576ff89a8071bd9f3881c97d886f3be52ae8509332";
         
         TransactionReceipt txReceipt = service.appGetTransactionReceipt(hash).send().getTransactionReceipt();
         
         if (txReceipt != null) {
            if(txReceipt.getErrorMessage() == null) {
               System.out.println("Transaction success, txReceipt: " + txReceipt);
            } else {
               System.out.println("Transaction error, errorMessage: " + txReceipt.getErrorMessage());
            }
         }
      ```

   3. Through JsonRpc interface：

      ```shell
      curl -X POST --data '{"jsonrpc":"2.0","method":"getTransactionReceipt","params":["0x0e77f2076377117d942c14576ff89a8071bd9f3881c97d886f3be52ae8509332"],"id":1}' 47.92.173.78:1337
      ```

#### 5. Be familiar with Parabox RPC interface

   1. PRC Interface Address：

      Main network：https://rpc.parabox.cc Or http://rpc.parabox.cc

      Test network: http://47.92.173.78:1337

   2. RPC interface List：

      <https://github.com/parabox-network/docs/blob/master/rpc.md>

#### 6. Be familiar with SDK 

   1. JS SDK:

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/README.md>

   2. JAVA SDK:

      <https://github.com/parabox-network/parabox-sdk-java/blob/master/docs/index.md>

#### 7. Create Smart Contract on the Chain

   <https://github.com/parabox-network/parabox-demo>
