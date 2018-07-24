
** 중요 정보 : rhrorsa의 키는 패스워드와 같이 매우 중요한 보안 수단입니다. 절대로 고객님의 키를 제3자에게 노출시키지 마십시오.

### 병행 사용 종료일자：2018 년 7 월 24 일 01:00 (한국 기준)

### 병행 사용 기간：2018 년 7 월 9 일, 15:00 - 7 월 24 일, 01:00 (한국 기준)

## 현재 상태
Huobi는HTTPS 요청사항에 맞춘 서명(Signature)을 요구합니다. 서명(Signature)은 표준 문자열과 비밀 키로 계산된 파라메터 변수 값으로 포함되어야 합니다.

## 변경사항 요약
• 사용자는 개인 키와 공개 키를 만들어야 합니다. 개인 키는 사용자가 보관하지만 공개 키는 Huobi에게 제공되어야 합니다. (아래 1 절, 2 절)

•	기존의 'Signature'외에 사용자는 개인 키로 'PrivateSignature'를 계산해야 합니다 (아래의 3.3 단계 참조). 그리고 'Signature'와 'PrivateSignature'는 질의 요청에 포함되어야 한다. (아래 4 단계의 세부 단계)


## 사용자에게 미치는 영향
•	병행 사용 기간 중에 'Signature'가 있는 기존 인증은 계속 유효하며 이는 'PrivateSignature'가 요청에서 제외될 수 있습니다.

• 병행 사용 종료일 이후부터 사용자는 공개 키를 기존 API 키와 연결하거나 공개 키로 새로운 API 키를 생성하여야 합니다.

• 병행 사용 종료일 이후부터 새로운 인증방식으로 변경되어 적용됩니다. Signature와 PrivateSignature에 대한 질의 요청을 보낼 수 있습니다.


## 인증 생성 단계별 상세 설명

### 1.	개인 키 및 공개 키 만들기

개인 키 및 공개 키를 만들려면 [OpenSSL](https://github.com/alphaex-api/BAPI_Docs_ko/wiki/Private-Key%EC%99%80-Public-Key-%EC%83%9D%EC%84%B1%EC%9D%84-%EC%9C%84%ED%95%9C-OpenSSL)을 참조 하십시오. 자세한 내용을 확인할 수 있습니다.

### 2.	기존 API 키 연결 또는 공개 키로 새 API 키 만들기
2.1.	API 관리 섹션에서 사용자는 기존 API 키를 '편집'하여 기존 API 키와 공개 키를 연결할 수 있습니다. 병행 사용 기간동안 공개 키가 링크되어 있지 않으면 기존 API 키는 더이상 유효하지 않습니다.
2.2.	사용자는 아래와 같이 공개 키를 사용하여 새 API 키를 만들 수 있습니다.


<img width="881" alt="screen shot 2018-07-05 at 1 06 24 pm" src="https://raw.githubusercontent.com/alphaex-api/API_Docs_cn/master/create_public_key_1.png">

API 키가 성공적으로 생성되면 접근키(AccessKey)와 보안키(SecretKey)가 제공됩니다 (현재와 동일) 보안키(SecretKey)는 한 번만 표시되고 다시 볼 수 없으므로 키를 노트 등에 작성하여 잘 보관하여 주시기 바랍니다.

<img width="559" alt="screen shot 2018-07-05 at 1 51 07 pm" src="https://raw.githubusercontent.com/alphaex-api/API_Docs_cn/master/create_public_key_2.png">

### 3.	서명 및 서명 생성

웹 서비스는 인터넷을 통해 전송되며 변조에 취약합니다. 보안상의 이유로 Huobi는 모든 요청의 일부로 서명을 요구합니다.

3.1. 서명을 생성하기 전에 지정된 형태의 요청을 표준화 된 형식으로 정규화 해야 합니다. 이것은 다른 형식으로 인해 다른 HMAC 서명이 발생할 수 있기 때문에 필요합니다. 이 프로세스는 응용 프로그램과 Huobi 서버가 주어진 요청에 대해 동일한 서명을 계산하도록 합니다. 다음 단계는 샘플 요청을 표준화 된 형식으로 저장하는 방법을 보여줍니다.


```
https://api.huobi.pro/v1/order/orders?
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T15:19:30 //您发出请求的时间 YYYY-MM-DDTHH:MM:SS(UTC 时区)。
&order-id=1234567890
```
3.1.1. 요청 메소드 (GET 또는 POST)와 개행 문자로 시작하십시오. 가독성을 높이기 위해 개행 문자는 \n으로 표시됩니다.。
```
GET\n
```
3.1.2. 루트 URL을 소문자로 추가하고 줄 바꿈 문자를 추가하십시오.
```
api.huobi.pro\n
```
3.1.3. 메서드를 추가 한 다음 줄 바꿈 문자를 추가합니다.
```
/v1/order/orders\n
```
3.1.4. 다음으로 인코딩 된 URL 쿼리 문자열을 만듭니다. 구성 요소 입력 시 암호화된  UTF-8 문자를 추가하십시오 (16 진수는 대문자여야 함). 요청사항 입력 시 물음표 문자(?)를 인코딩하지 마십시오. 그런 다음 바이트 순서로 구성 요소를 정렬합니다. 바이트 순서는 대소문자를 구분합니다.
예를 들어 요청 구성 요소의 원래 순서입니다.

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
order-id=1234567890
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
```
등호 (=)는 매개 변수 이름과 해당 값 사이에서 사용해야합니다. & 문자는 매개 변수 이름과 값의 각 쌍 사이에서 사용해야 합니다. 그런 다음 매개 변수와 해당 값을 공간이 없는 하나의 긴 문자열로 연결하십시오. 매개 변수 값 내의 공백은 허용되며 URL은 %20로 인코딩 되어야합니다. 연결된 문자열에서 마침표 (.)는 빠지지 않습니다.
```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
order-id=1234567890
```

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```
3.1.5. 최종 정식 요청을 작성하려면 위 단계의 모든 구성 요소를 결합하십시오.

```
GET\n
api.huobi.pro\n
/v1/order/orders\n
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```
3.2. 서명은 다음 정식 문자열과 비밀 키가 있는 키 해시 함수로 계산되어야 합니다.

표준 쿼리 문자열 :

- 표준 쿼리 문자열 :
```
GET\n
api.huobi.pro\n
/v1/order/orders\n
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```
- 비밀 키 (APIKEY 신청시 Huobi가 배포 한 'SecretKey') :
```
b0xxxxxx-c6xxxxxx-94xxxxxx-dxxxx
```
- 마지막 단계의 서명 결과를 Base64로 인코딩해야합니다.
```
4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=
```
3.3.	PrivateSignature 계산 (NEW) PrivateSignature는 'Signature'

(3.2 단계의 결과) 문자열과 Private Key (1 단계에서 작성)를 사용하여 키 해싱 함수로 계산해야 합니다. 결과는 Base64로 인코딩 해야합니다.

### 4. Signature와 PrivateSignature 모두에 대한 API 요청

 'Signature'및 'PrivateSignature'를 URI로 인코딩 한 후 API 요청에 포함하십시오. Signature 용으로 포맷 된 각 HTTPS 요청에 다음 구성 요소가 포함 되어야 합니다.


 - 경로 : api.huobi.pro 또는 api.hadax.com 다음에 'api.huobi.pro/v1/order/orders'와 같은 서비스 문자열이 옵니다.

 - AccessKey（AccessKeyId）
   AccessKeyId : APIKEY를 신청했을 때 Huobi가 배포 한 'AccessKey'.

 - 签名方法（SignatureMethod）
   用户计算签名的基于哈希的协议，此处使用 HmacSHA256。

 - SignatureMethod 
   서명 을 계산하는 데 사용되는 해시 기반 프로토콜입니다. 이것은 HmacSHA256이어야합니다.


 - Timestamp
   타임 스탬프 : 요청한 시간. 제 3자가 사용자의 요청을 가로 채지 못하도록 쿼리 요청에 이를 포함 시키십시오. 이것은 '2017-05-11T16 : 22 : 06'과 같이 UTC 시간으로 형식화 되어야 합니다.

 - 요청에 대한 필수 및 선택적 매개 변수
   각 메소드에는 API 호출에서 정의 된 필수 및 선택 매개 변수 집합이 있으며 API 참조에서 사용할 수 있습니다.

   중요한 주의 사항：
   GET 방법의 모든 매개 변수는 서명 계산에 포함 되어야 합니다. 

   POST 방법의 경우 서명 계산시 AccessKeyId, SignatureMethod, SignatureVersion, Timestamp 만 포함되어야합니다.

 - Signature
   서명 : 비밀 키로 계산 된 서명.

- PrivateSignature
개인 키로 계산 된 개인 서명. (새로운)

샘플 요청：
```
http://api.huobi.pro/v1/account/accounts?AccessKeyId=f70d805f-2467cb8a-3d40d7bd-a50f7&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2018-07-05T08:26:22&Signature=85WuwICm8gypsc5a/qTvWBs2EaKzNevscan4IH9P5v4=&PrivateSignature=ybFd+VTwfX6SPP92hbwnjGpycW6und6lbHdPeW1PJV5necpj1sn7+iTxxbTeF+SZG1fdc3uMQ/JRw8UumG0e/g==
```

#### 响应码说明

| err-code | 설명 영어 | 설명 | 유형 |
| ------- | ----- | ---- | ---- |
| 500 | System error | 시스템 오류 | M |
|502	|Parameter error	|매개 변수 오류	|M|
|12001	|Invalid submission time or incorrect time format	|제출 시간이 잘못되었거나 시간 형식이 잘못되었습니다	|M|
|12002	|Incorrect signature version	|부정확 한 서명 버전	|M|
|12003	|Incorrect signature method	|잘못된 서명 방법	|M|
|12004	|API key has expired	|API 키가 만료되었습니다.	|M|
|12005	|Incorrect IP address	|잘못된 서명 방법	|M|
|12006	|Submission time is required	|제출 시간이 필요합니다. 	|M|
|12007	|Incorrect Access key	|잘못된 액세스 키	|M|
|12008	|Verification failure	|검증 실패	|M|
|12009	|Abnormal user status	|비정상적인 사용자 상태	|M|
|12010	|Incorrect Private Key signature	|잘못된 개인 키 서명	|M|
|12011	|Incorrect Public key	|잘못된 공개 키	|M|

#### 응답 샘플

```json
{
    "status":"error",
    "err-code":"api-signature-not-valid",
    "err-msg":"Signature not valid: Invalid submission time or incorrect time format [无效的提交时间，或时间格式错误]",
    "data":null
}
```

### 부록 DEMO

1. [REST-Java-demo](https://github.com/huobiapi/REST-API-demos/tree/master/REST-Java-demo)
2. [Rest-C#-demo](https://github.com/huobiapi/REST-API-demos/tree/master/Rest-C%23-demo)
3. [REST-Python3-demo](https://github.com/huobiapi/REST-API-demos/tree/master/REST-Python3-demo)
