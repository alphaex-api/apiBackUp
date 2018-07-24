****주의사항: 고객님의 키는 마치 패스워드와 같은 것이므로 매우 중요한 보안 수단입니다. 절대로 고객님의 키를 제 3자에게 노출시키지 마십시오. **
## 1.	OpenSSL 도구
Mac 유저분들은 ‘Terminal’을 호출함으로서 OpenSSL 커맨드를 실행할 수 있습니다.

공식 OpenSSL 웹사이트는 리눅스용 소스만 제공을 하므로 윈도우 유저분들은 다음 링크를 통해서 다운로드 받으시면 됩니다. [Win32OpenSSLv1.1.0h_Light](https://slproweb.com/download/Win32OpenSSL_Light-1_1_0h.exe) (source: https://slproweb.com/products/Win32OpenSSL.html). 다운로드가 완료되면 인스톨을 완료하시기 바랍니다. OpenSSL 인스톨은 가급적이면 Window의 System폴더 외부에 설치해주시기 바랍니다. OpenSSL을 호출하기 위해서는 윈도우 탐색기(파일 탐색기)에서 인스톨된 폴더에서 우클릭을 하시면 됩니다. 예를 들면 C:\OpenSSL-Win64\bin 로 이동 후 ‘관리자 권한으로 실행’을 하시면 됩니다. 

<img width="483" alt="openssl-02" src="https://user-images.githubusercontent.com/9877081/42375133-28489afa-814d-11e8-87f8-4a5bbc268cc4.png">
 
다음 단계에서는 CMD창이 열리면서 OpenSSL 커맨드 프롬프트가 실행 될 것이며 다음과 같은 화면이 뜨게 됩니다.

<img width="478" alt="openssl-04" src="https://user-images.githubusercontent.com/9877081/42375188-5917c7fa-814d-11e8-955e-415265ece1e3.png">

## 2.	이제 다음에 나오는 명령어들을 통해서 Private Key와 Public Key를 생성하실 수 있습니다.
### // Private Key 생성

mac/linux： ```openssl ecparam -name secp256k1 -genkey -noout -out privatekey.pem```

windows： ```ecparam -name secp256k1 -genkey -noout -out privatekey.pem```

### // pk8 file 생성 (Java, C# 에서는 필수 사항)

mac/linux： ```openssl pkcs8 -topk8 -in privatekey.pem -nocrypt -out pk8-privatekey.pem```

windows：```pkcs8 -topk8 -in privatekey.pem -nocrypt -out pk8-privatekey.pem```

### // Public Key 생성

mac/linux： ```openssl ec -in privatekey.pem -pubout -out publickey.pem```

windows：```ec -in privatekey.pem -pubout -out publickey.pem```

샘플 윈도우：
1. 위에 기술된 명령어를 실행하세요.

<img width="684" alt="screen shot 2018-07-10 at 2 40 55 pm" src="https://user-images.githubusercontent.com/9877081/42493345-3ff71de8-844f-11e8-97c4-3fa5316ce5b8.png">

2. OpenSSL 도구가 설치된 폴더에서 3.pem 파일들을 검색하세요. ( privatekey.pem，pk8-privatekey.pem，publickey.pem）

<img width="476" alt="screen shot 2018-07-10 at 2 42 20 pm" src="https://user-images.githubusercontent.com/9877081/42493378-6869b7fe-844f-11e8-9599-fea5efede8bd.png">

3. 메모장으로 키 파일들의 내용을 확인하실 수 있습니다.

<img width="710" alt="screen shot 2018-07-10 at 2 43 34 pm" src="https://user-images.githubusercontent.com/9877081/42493432-99189870-844f-11e8-8ea8-a6cd614a6aee.png">


더 많은 정보가 필요하시다면 [API Authentication](https://github.com/alphaex-api/BAPI_Docs_ko/wiki/API-%EC%9D%B8%EC%A6%9D-%EB%B3%80%EA%B2%BD-%EC%95%8C%EB%A6%BC)를 클릭하세요.
