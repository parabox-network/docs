### Parabox 接入指南

#### 1. 创建一个账户

   1. 通过浏览器内置钱包或移动端钱包创建：

      直接根据页面提示创建即可。

      Parabox 浏览器内置钱包：

      ​	主网：<https://explorer.parabox.cc/#/wallet> 

      ​	测试网：<http://39.100.63.66/#/wallet> 

      Android 版：

      ​	主网：<https://download.parabox.cc/parabox.apk>  

      ​	测试网：<http://39.100.63.66:8091/parabox.apk>

      iOS 版：

      ​	主网：<https://testflight.apple.com/join/dB6G64aV>

      ​	测试网：https://testflight.apple.com/join/cnH8To1m

   2. 通过 SDK创建：

      1. 通过  JS SDK：

         https://github.com/parabox-network/parabox-sdk-js/blob/master/example/createAccount.js

      2. 通过  Java SDK：

         ```java
            ECKeyPair keyPair = Keys.createEcKeyPair();

            System.out.println("Address: 0x" + Keys.getAddress(keyPair));
            System.out.println("PublicKey: 0x" + keyPair.getPublicKey());
            System.out.println("PrivateKey: 0x" + keyPair.getPrivateKey());
         ```

   3. 使用以太坊账户：

      Parabox 账户与以太坊账户格式兼容，您可以直接使用以太坊上账户的助记词、keystore 或私钥导入Parabox 钱包直接使用；

   4. 测试网账户(有余额):

      ```
      {
        "address": "0xea57cde6138f1c049fc3894331e3ab16c5a07896",
        "private": "0x74e2aa5ae5e29aa00bd7a30b8f47db1990e57b66feb9e27984bc5d2c8e7e35be"
      }
      ```

#### 2. 查询账户余额：

   1. 通过 JS SDK：

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/example/getBalance.js>

   2. 通过 Java SDK： 
   
      ```java
           Paraboxj service = Paraboxj.build(new HttpService(rpcUrl));

           BigInteger balance = service.appGetBalance(
                   "0xea57cde6138f1c049fc3894331e3ab16c5a07896",
                   DefaultBlockParameterName.LATEST
           ).send().getBalance();

           System.out.println("Balance: " + balance);
       ```
      

   3. 通过 JSonRPC 接口：

      ```sh
      curl -X POST --data '{"jsonrpc":"2.0","method":"getBalance","params":["0xea57cde6138f1c049fc3894331e3ab16c5a07896", "latest"],"id":1}' 47.92.173.78:1337
      ```

#### 3. 发送一笔交易

   1. 通过 JS SDK：

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/example/transfer.js>

   2. 通过 Java SDK： 
   
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
      

#### 4. 查询交易回执

   1. 通过 JS SDK：

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/example/getTransactionReceipt.js>

   2. 通过 Java SDK：
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
      

   3. 通过 JsonRPC 接口：

      ```shell
      curl -X POST --data '{"jsonrpc":"2.0","method":"getTransactionReceipt","params":["0x0e77f2076377117d942c14576ff89a8071bd9f3881c97d886f3be52ae8509332"],"id":1}' 47.92.173.78:1337
      ```

#### 5. 熟悉 Parabox RPC 接口

   1. PRC 接口地址：

      主网：https://rpc.parabox.cc 或 http://rpc.parabox.cc

      测试网: http://47.92.173.78:1337

   2. RPC接口列表：

      <https://github.com/parabox-network/docs/blob/master/rpc.md>

#### 6. 熟悉 SDK 

   1. JS SDK:

      <https://github.com/parabox-network/parabox-sdk-js/blob/master/README.md>

   2. JAVA SDK:

      <https://github.com/parabox-network/parabox-sdk-java/blob/master/docs/index.md>

#### 7. 在链上创建智能合约

   <https://github.com/parabox-network/parabox-demo>
