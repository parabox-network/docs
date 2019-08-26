## Parabox 节点启动步骤

​	请先准备 Ubuntu 18.04 服务器(推荐 4 核 16G )，以下操作除第一步上传文件及最后一步申请成为验证节点外，其余操作均在 Ubuntu 18.04 环境下执行。

1. 下载安装文件传到服务器上

   下载地址：<https://download.parabox.cc/mainnet/20190814/install.zip>

2. 安装 docker

   ```
   sudo apt install docker.io
   ```

3. 解压  install.zip 文件

   ```
   unzip install.zip
   ```

4. 修改 privkey 和 address 文件

   ```
   cd install/
   
   // 请将以下命令中的 YOURPRIVATEKEY 和 YOURADDRESS 更换为你将要使用的账户私钥和账户地址
   // 如果没有账户私钥和地址，
   // 可通过移动端 Parabox 钱包 
   // 或 Parascan 浏览器中内值的钱包（ https://explorer.parabox.cc/#/wallet ）创建账户
   echo YOURPRIVATEKEY > Parabox/0/privkey
   echo YOURADDRESS > Parabox/0/address
   
   // 通过cat命令验证修改后的 privkey 和 address 
   cat Parabox/0/privkey
   cat Parabox/0/address
   ```

5. 启动节点

   1. 进入 docker 环境
      第一次进入时会自动拉取镜像，此时会出现拉取镜像进度提示，请耐心等待;
      如果出现 permission denied 提示，您可能需要 sudo 权限， 请在以下命令前添加 sudo;

      请确保当前位于 install 目录下

      ```
      ./bin/cita-env
      // 或：sudo ./bin/cita-env
      ```

   2. 设置节点
      ```
      ./bin/cita bebop setup Parabox/0
      ```

   3. 启动节点
      ```
      ./bin/cita bebop start Parabox/0
      ```

   4. 验证节点启动
      ```
      curl -X POST --data '{"jsonrpc":"2.0","method":"blockNumber","params":[],"id":1}'  127.0.0.1:1337
      ```
      ```
      curl -X POST --data '{"jsonrpc":"2.0","method":"peerCount","params":[],"id":2}'  127.0.0.1:1337
      ```

6. 申请成为验证节点

   ​        请到 Parascan 治理模块下的节点投票页面申请成为候选节点，成为候选节点后您需要获得用户投票，系统根据得票数和押金数量计算综合得分。得分靠前的候选节点将会成为验证节点参与共识出块。注意：请在节点同步到最新高度后进行申请，否则在通过审核后不能出块可能会被惩罚。

   申请地址：  <https://explorer.parabox.cc/#/manage/nodeVoting> 

   

