# 使用OpenSSL工具生成密钥(Private Key & Public Key)
****重要提示：密钥与账号安全紧密相关，无论何时都请勿向其它人透露**
## 1.	OpenSSL工具
Mac用户，可以直接调用‘Terminal’然后执行OpenSSL的命令。

Windows用户，OpenSSL官网没有提供Windows版本的安装包，需要选择其他开源平台提供的工具，建议下载安装包 [Win32OpenSSLv1.1.0h_Light](Https://slproweb.com/download/Win32OpenSSL_Light-1_1_0h.exe)（安装包来源[https://slproweb.com/products/Win32OpenSSL.html](https://slproweb.com/products/Win32OpenSSL.html) ），请按引导将工具安装在Windows系统文件夹之外。然后在Windows浏览器里找到OpenSSL，右击鼠标，在菜单里选择‘Run as administrator’.

<img width="483" alt="openssl-02" src="https://user-images.githubusercontent.com/9877081/42375133-28489afa-814d-11e8-87f8-4a5bbc268cc4.png">
 
之后，cmd窗口会自动打开，然后就可以运行OpenSSL 命令了

<img width="478" alt="openssl-04" src="https://user-images.githubusercontent.com/9877081/42375188-5917c7fa-814d-11e8-955e-415265ece1e3.png">

## 2.	执行下列命令生成Private Key和Public Key
### // 生成私钥 

mac/linux系统命令： ```openssl ecparam -name secp256k1 -genkey -noout -out privatekey.pem```

windows系统命令： ```ecparam -name secp256k1 -genkey -noout -out privatekey.pem```

### // 生成pk8文件 如果是java, C#开发私钥必须转换成这个格式

mac/linux系统命令： ```openssl pkcs8 -topk8 -in privatekey.pem -nocrypt -out pk8-privatekey.pem```

windows系统命令：```pkcs8 -topk8 -in privatekey.pem -nocrypt -out pk8-privatekey.pem```

### // 生成公钥 

mac/linux系统命令： ```openssl ec -in privatekey.pem -pubout -out publickey.pem```

windows系统命令：```ec -in privatekey.pem -pubout -out publickey.pem```

以Windows为例：
1. 执行以上三个命令行

<img width="684" alt="screen shot 2018-07-10 at 2 40 55 pm" src="https://user-images.githubusercontent.com/9877081/42493345-3ff71de8-844f-11e8-97c4-3fa5316ce5b8.png">

2. 在OpenSSL工具所在的文件夹找到示例生成的3个密钥文件（privatekey.pem，pk8-privatekey.pem，publickey.pem）

<img width="476" alt="screen shot 2018-07-10 at 2 42 20 pm" src="https://user-images.githubusercontent.com/9877081/42493378-6869b7fe-844f-11e8-9599-fea5efede8bd.png">

3. 用Notepad打开文件即可看到密钥的内容

<img width="710" alt="screen shot 2018-07-10 at 2 43 34 pm" src="https://user-images.githubusercontent.com/9877081/42493432-99189870-844f-11e8-8ea8-a6cd614a6aee.png">


返回[API安全认证](https://github.com/alphaex-api/BAPI_Docs_cn/wiki/REST_authentication)获得安全认证的其他内容。