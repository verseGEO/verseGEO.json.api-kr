# MW30P P2E API Guide

- 홈페이지 : [verseGEO.com](http://versegeo.com/).
- 백서참조 : [verseGEO Whitepaper](http://versegeo.com/whitepaper-k.html).

## 1. Exchange Rate Inquiry API (Partners → MW30P)

Exchange Rate Inquiry API는 환율 정보를 제공합니다(Item, Point, etc  PlayToken, PlayToken  Item, Point, etc). 이 API를 통해 Exchange API에 환율정보를 적용해야 합니다.

※ Exchange Rate Inquiry API의 응답결과인 환율은 별도 ADMIN거래로  등록이 필요합니다. 환율정보가 사전에 등록이 되어 있지 않으면 오류를 반환합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Exchange Rate Inquiry API|/api/ExchangeRate.json|POST|application/json|
<br>

* Exchange Rate Inquiry Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>0000000000013</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>000001</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>20220316192601001</sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>전환 요청 통화코드(Item, Token 등)</sub>|<sub>GOLD</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>전환 대상 통화코드(Item, Token 등)</sub>|<sub>SLAYB</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>Item, Point, Token 수량 (환율등록 어드민에서 지정한 최소 단위 이하로 요청 시 오류 반환)</sub>|<sub>100</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>
   
* Exchange Rate Inquiry Response Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>전환 요청 통화코드(Item, Token 등)</sub>|<sub>GOLD</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>전환 대상 통화코드(Item, Token 등)</sub>|<sub>SLAYB</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>Item, Point, Token 수량 (환율등록 어드민에서 지정한 최소 단위 이하로 요청 시 오류 반환)</sub>|<sub>100</sub>|
|<sub>toAmount</sub>|<sub>N</sub>|<sub>20</sub>|<sub>전환수량</sub>|<sub>전환수량 = 요청수량 * 환율</sub>|<sub>1</sub>|
|<sub>exchangeRate</sub>|<sub>N</sub>|<sub>20</sub>|<sub>환율</sub>|<sub>요청단위, 전환단위에 대한 환율정보</sub>|<sub>10%</sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub>SUCCESS, DECLINED</sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>
   
* Exchange Rate Inquiry Sequence
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/01SEQ-01.Exchange_Rate_Inquiry-KR.jpg">
<br>

* Exchange Rate Inquiry Interface JSON Sample
   
[Request]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "fromCurrency" : "GOLD",
    "toCurrency" : "SLAYB",
    "fromAmount" : "100", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : SUCCESS]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "fromCurrency" : "GOLD",
    "toCurrency" : "SLAYB",
    "fromAmount" : "100", 
    "toAmount" : "1",
    "exchangeRate" : "10%",
    "status" : "SUCCESS", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : DECLINED]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "fromCurrency" : "GOLD",
    "toCurrency" : "SLAYB",
    "fromAmount" : "99", 
    "toAmount" : "",
    "exchangeRate" : "",
    "status" : "DECLINED", 
    "errorInformation.errCd" : "7007",
    "errorInformation.reason" : " 최소 금액 부족",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```
<br>
<br>

## 2. Exchange API (Partners → MW30P)

제휴사의 사용자가 보유한 요청단위(아이템, 포인트, 토큰 등)에 대해 전환단위(아이템, 포인트, 토큰 등)로 변환합니다. Exchange 요청을 위해서는 Exchange Rate Inquiry API를 통해 환율정보를 적용해야 합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Exchange API|/api/ExchangeRequest.json|POST|application/json|
<br>

* Exchange Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>000000000001</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>000001</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>20220316192601001</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY). 사용자의 디바이스(모바일, PC 등)의 분실 또는 파손 등으로 변경이 발생해도 사용자의 자산을 관리가 가능하도록 사용자 고유 Key가 반드시 필요함.</sub>|<sub>userid@usermail.url</sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>전환 요청 통화코드(Item, Token 등)</sub>|<sub>GOLD</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>전환 대상 통화코드(Item, Token 등)</sub>|<sub>SLAYB</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>Item, Point, Token 수량 (환율등록 어드민에서 지정한 최소 단위 이하로 요청 시 오류 반환)</sub>|<sub>100</sub>|
|<sub>exchangeRate</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>환율</sub>|<sub>Exchange Rate Inquiry API에서 수신 받은 환율 사용</sub>|<sub>10%</sub>|
|<sub>outPassword</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>비밀번호(암호화)</sub>|<sub>비밀번호 (암호화 적용, "2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
|<sub>Papers</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>거래검증 KEY</sub>|<sub>Passport API 호출 후 수신 받은  거래검증 KEY</sub>|<sub></sub>|
|<sub>notifyUrl</sub>|<sub>N</sub>|<sub>128</sub>|<sub>처리결과 수신 URL</sub>|<sub>비동기 처리결과를 수신 받을 URL (채널 측에서 블록체인 처리결과를 수신 처리하도록 개발 필요함)</sub>|<sub>Partners_URL<br>(Only HTTPS)</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Exchange Response Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>전환 요청 통화코드(Item, Token 등)</sub>|<sub>GOLD</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>전환 대상 통화코드(Item, Token 등)</sub>|<sub>SLAYB</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>Item, Point, Token 수량 (환율등록 어드민에서 지정한 최소 단위 이하로 요청 시 오류 반환)</sub>|<sub>100</sub>|
|<sub>toAmount</sub>|<sub>N</sub>|<sub>20</sub>|<sub>전환수량</sub>|<sub>전환수량 = 요청수량 * 환율</sub>|<sub>1</sub>|
|<sub>exchangeRate</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>환율</sub>|<sub>Exchange Rate Inquiry API에서 수신 받은 환율 사용</sub>|<sub>10%</sub>|
|<sub>internalAddress</sub>|<sub>N</sub>|<sub>128</sub>|<sub>내부주소</sub>|<sub>PlayToken(Internal Token) Address</sub>|<sub></sub>|
|<sub>txId</sub>|<sub>N</sub>|<sub>128</sub>|<sub>TXID</sub>|<sub>블록체인 Transaction ID</sub>|<sub></sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub>SUCCESS, DECLINED</sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Exchange Sequence
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/02SEQ-01.Exchange-KR.jpg">
<br>

* Exchange Interface JSON Sample
   
[Request]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "fromCurrency" : "GOLD",
    "toCurrency" : "SLAYB",
    "fromAmount" : "100", 
    "customerId" : "userid@usermail.url",
    "outPassword" : "f3a0ea7f63724bbd18194bf3a77974df0c8be6a58264ec2df860ad636b31fac6",
    "Papers" : "77974df0c8be6a58264ec2df860af3a0ea7f63724bbd18194bf3ad636b31fac6",
    "notifyUrl" : "https://Partners_URL/API/backNotify"
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : SUCCESS]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "fromCurrency" : "GOLD",
    "toCurrency" : "SLAYB",
    "fromAmount" : "100", 
    "toAmount" : "1",
    "exchangeRate" : "10%",
    "internalAddress" : "0xb440a6cdfbfe4870fc06385d0533476344bdc557",
    "customerId" : "userid@usermail.url",
    "txId" : "0x7104afd5b61c5df952c8e9afd2dafa222d543111b2ee3862c80502b3f2aed93b ",
    "status" : "SUCCESS", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : DECLINED]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "fromCurrency" : "GOLD",
    "toCurrency" : "SLAYB",
    "fromAmount" : "99", 
    "toAmount" : "",
    "exchangeRate" : "13%",
    "internalAddress" : "0xb440a6cdfbfe4870fc06385d0533476344bdc557",
    "customerId" : "userid@usermail.url",
    "txId" : "",
    "status" : "DECLINED",
    "errorInformation.errCd" : "7010",
    "errorInformation.reason" : "금액 검증 오류",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```
<br>
<br>

## 3. Passport API (Partners → MW30P)

Exchange API, Passport API(비밀번호변경), Withdrawal Address API, API, Withdrawal API를 통한 등록, 변경, 전환, 인출 등의 주요 서비스는 Passport API를  인증 후 실행해야 합니다.

※ 인증 방식은 파트너사와 협의하여 FIDO, 패턴, 2FACT등 다양한 방식이 적용 가능합니다. 

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Passport API|/api/PasswordRegistration.json|POST|application/json|
<br>

* Passpoort Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>000000000001</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>000001</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>20220316192601001</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>outAddress </sub>|<sub>Y</sub>|<sub>128</sub>|<sub>인출주소</sub>|<sub>사용 안함</sub>|<sub></sub>|
|<sub>PassportOption</sub>|<sub>Y</sub>|<sub>32</sub>|<sub>비밀번호 Option</sub>|<sub>비밀번호 처리 옵셥(Register : 최초등록 시, Certify : 비밀번호 검증 시, Change : 비밀번호 변경 시)</sub>|<sub></sub>|
|<sub>outPassword</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>비밀번호(암호화)</sub>|<sub>비밀번호 (암호화 적용, "2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
|<sub>newPassword</sub>|<sub>N</sub>|<sub>192</sub>|<sub>신규 비밀번호(암호화)</sub>|<sub>비밀번호 최초 등록 또는 변경 시  (암호화 적용 전송, "2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
|<sub>Papers</sub>|<sub>N</sub>|<sub>192</sub>|<sub>거래검증 KEY</sub>|<sub>Passport API 인증 후 수신된 Papers</sub>|<sub></sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Passpoort Response Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>Papers</sub>|<sub>N</sub>|<sub>192</sub>|<sub>거래검증 KEY</sub>|<sub>Exchange API, Withdrawal Address API, Withdrawal API, Passport API에서 사용. Papers 생성 후 5분이후 해당 인증 소멸됨</sub>|<sub></sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub>SUCCESS, DECLINED</sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Passport Required Option (PassportOption)

|KEY|Register<br>Request|Register<br>Response|Certify<br>Request|Certify<br>Response|Change<br>Request|Change<br>Response|
|:-----|:----:|:----:|:----:|:----:|:----:|:----:|
|outPassword|N|-|Y|-|Y|-|
|newPassword|Y|-|N|-|Y|-|
|Papers     |N|Y|N|Y|Y|Y|
<br>

* Passport Sequence(Password Registration)

<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/03SEQ-01.Passport-KR.jpg">
<br>

* Passport Sequence(Password Change)

<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/03SEQ-02.Passport-KR.jpg">
<br>

* Passport Interface JSON Sample
   
[Request]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outPassword" : "f3a0ea7f63724bbd18194bf3a77974df0c8be6a58264ec2df860ad636b31fac6",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"

   }
```

[Response : SUCCESS]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "Papers" : "77974df0c8be6a58264ec2df860af3a0ea7f63724bbd18194bf3ad636b31fac6",
    "status" : "SUCCESS", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : DECLINED]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "status" : "DECLINED",
    "errorInformation.errCd" : "B010",
    "errorInformation.reason" : "사용자 고유ID 오류 발생",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```
<br>
<br>

## 4. Withdrawal Address API (Partners → MW30P)

파트너사의 사용자가 보유한 PlayToken(내부토큰)을 외부에서 사용할 수 있도록 외부주소(거래소 등에서 사용 가능한)를 정상여부 검증 및 등록을 수행합니다. 외부주소를 사용자가 정확히 입력하지 않아 발생하는 손실은 사용자에게 있습니다.

※ 현재는 이더리움 기반의 블록체인만 지원하고 있으나 파트너사 및 사업 제휴 모델에 따라 다양한 블록체인 모델로 확장 가능합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Withdrawal Address API|/api/WithdrawalAddressVerification.json|POST|application/json|
<br>

* Withdrawal Address Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>000000000001</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>000001</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>20220316192601001</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>outAddress</sub>|<sub>Y</sub>|<sub>128</sub>|<sub>인출주소</sub>|<sub>거래소등에서 사용 가능한 채널 사용자의 퍼블릭 블록체인 주소</sub>|<sub></sub>|
|<sub>outPassword</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>비밀번호(암호화)</sub>|<sub>비밀번호 (암호화 적용, "2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
|<sub>Papers</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>거래검증 KEY</sub>|<sub>Passport API 인증 후 수신된 Papers</sub>|<sub></sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Withdrawal Address Response Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>outAddress</sub>|<sub>N</sub>|<sub>128</sub>|<sub>인출주소</sub>|<sub>거래소등에서 사용 가능한 채널 사용자의 퍼블릭 블록체인 주소</sub>|<sub></sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub></sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Withdrawal Address Sequence(Registration)
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/04SEQ-01.Withdrawal_Address-KR.jpg">
<br>

* Withdrawal Address Sequence(Change)
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/04SEQ-02.Withdrawal_Address-KR.jpg">
<br>

* Withdrawal Address Interface JSON Sample
   
[Request]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outPassword" : "f3a0ea7f63724bbd18194bf3a77974df0c8be6a58264ec2df860ad636b31fac6",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : SUCCESS]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xcce4726a8bca553e31c5341fa456a43062b46520",
    "status" : "SUCCESS", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : DECLINED]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xb440a6cdfbfe4870fc06385d0533476344bdc557",
    "status" : "DECLINED",
    "errorInformation.errCd" : "B100",
    "errorInformation.reason" : " 지갑 주소가 올바르지 않습니다",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```
<br>
<br>

## 5. Withdrawal 
### 5.1 Withdrawal pre-trade API (Partners → ME30P)

Withdrawal pre-trade API는 Withdrawal API를 호출하기전 반드시 실행해야 합니다.  Withdrawal pre-trade API의 결과 중 주요 데이터는 인출 본거래인 Withdrawal API 반영되어야 합니다. (Related (required field) API flow 가이드문서 참조-)

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Withdrawal pre-trade API|/api/Reqpretrade.json|POST|application/json|
<br>

* Withdrawal pre-trade Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>000000000001</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>000001</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>20220316192601001</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>인출 요청 통화코드</sub>|<sub>SLAYB</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>인출 대상 통화코드</sub>|<sub>VGEO</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>인출 요청 토큰 수량</sub>|<sub>1</sub>|
|<sub>sign</sub>|<sub></sub>|<sub></sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Withdrawal pre-trade Response Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>outAddress</sub>|<sub>Y</sub>|<sub>128</sub>|<sub>인출주소</sub>|<sub>거래소등에서 사용 가능한 채널 사용자의 퍼블릭 블록체인 주소</sub>|<sub></sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>인출 요청 통화코드</sub>|<sub>SLAYB</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>인출 대상 통화코드</sub>|<sub>VGEO</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>인출 요청 토큰 수량</sub>|<sub>1</sub>|
|<sub>withdrawalRate</sub>|<sub>N</sub>|<sub>20</sub>|<sub>인출환율</sub>|<sub>인출 환율</sub>|<sub>100%</sub>|
|<sub>toAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>전환수량</sub>|<sub>전환수량 = 요청수량 * 인출환율</sub>|<sub>1</sub>|
|<sub>withdrawalFee</sub>|<sub>N</sub>|<sub>20</sub>|<sub>인출수수료</sub>|<sub>인출 수수료</sub>|<sub>0</sub>|
|<sub>withdrawalAmount</sub>|<sub>N</sub>|<sub>20</sub>|<sub>인출수량</sub>|<sub>인출수량 = 전환수량 - 인출 수수료</sub>|<sub>1</sub>|
|<sub>ReserveWID</sub>|<sub>N</sub>|<sub>192</sub>|<sub>인출검증 WID</sub>|<sub>인출거래 수행을 위한 거래검증 WID</sub>|<sub></sub>|
|<sub>pinNumber</sub>|<sub>N</sub>|<sub>6</sub>|<sub>인출 PIN NUMBER</sub>|<sub>Withdrawal 본거래에서 사용한 Pin Number</sub>|<sub>854021</sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub>SUCCESS, DECLINED</sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Withdrawal pre-trade Sequence
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/05SEQ-01.Withdrawal-KR.jpg">
<br>

* Withdrawal pre-trade Interface JSON Sample
   
[Request]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outPassword" : "f3a0ea7f63724bbd18194bf3a77974df0c8be6a58264ec2df860ad636b31fac6",
    "fromCurrency" : "SLAYB",
    "toCurrency" : "VEGO",
    "fromAmount" : "1", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : SUCCESS]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xcce4726a8bca553e31c5341fa456a43062b46520",
    "fromCurrency" : "SLAYB",
    "toCurrency" : "VEGO",
    "fromAmount" : "1", 
    "withdrawalRate" : "100%",
    "toAmount" : "1",
    "withdrawalFee" : "0",
    "withdrawalAmount" : "1",
    "ReserveWID" : "d18194bf3a77974df0c8be6a58264ec2df860ad636b31fac6f3a0ea7f63724bb",
    "pinNumber" : "880154",
    "status" : "SUCCESS", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : DECLINED]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xcce4726a8bca553e31c5341fa456a43062b46520",
    "fromCurrency" : "SLAYB",
    "toCurrency" : "VEGO",
    "fromAmount" : "1", 
    "status" : "DECLINED",
    "errorInformation.errCd" : "B011",
    "errorInformation.reason" : "비밀번호 검증 오류",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```
<br>
<br>

### 5.2 Withdrawal API (Partners → ME30P)

인출은 PlayToken을 외부주소로 전송하는 기능입니다. 외부주소 전송된 PlayToken은 거래소 등 다양한 분야에 자유롭게 사용 가능합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Withdrawal pre-trade API|/api/Withdrawal.json|POST|application/json|
<br>

* Withdrawal Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>000000000001</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>000001</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>20220316192601001</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>outAddress</sub>|<sub>Y</sub>|<sub>128</sub>|<sub>인출주소</sub>|<sub>거래소등에서 사용 가능한 채널 사용자의 퍼블릭 블록체인 주소</sub>|<sub></sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 fromCurrency 값</sub>|<sub>SLAYB</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 toCurrency 값</sub>|<sub>VGEO</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 fromAmount 값</sub>|<sub>1</sub>|
|<sub>withdrawalRate</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>인출환율</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 withdrawalRate 값</sub>|<sub>100%</sub>|
|<sub>toAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>전환수량</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 toAmount 값</sub>|<sub>1</sub>|
|<sub>withdrawalFee</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>인출수수료</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 withdrawalFee 값</sub>|<sub>0</sub>|
|<sub>withdrawalAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>인출수량</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 withdrawalAmount 값</sub>|<sub>1</sub>|
|<sub>ReserveWID</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>인출검증 WID</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 ReserveWID 값</sub>|<sub></sub>|
|<sub>pinNumber</sub>|<sub>Y</sub>|<sub>6</sub>|<sub>인출 PIN NUMBER</sub>|<sub>Withdrawal pre-trade API에서 수신 받은 pinNumber 값</sub>|<sub>854021</sub>|
|<sub>outPassword</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>비밀번호(암호화)</sub>|<sub>비밀번호 (암호화 적용, "2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
|<sub>Papers</sub>|<sub>Y</sub>|<sub>192</sub>|<sub>거래검증 KEY</sub>|<sub>Passport API 호출 후 수신 받은 거래 검증 KEY</sub>|<sub></sub>|
|<sub>notifyUrl</sub>|<sub>N</sub>|<sub>128</sub>|<sub>처리결과 수신 URL</sub>|<sub>비동기 처리결과를 수신 받을 URL (채널 측에서 블록체인 처리결과를 수신 처리하도록 개발 필요함)</sub>|<sub>Partners_URL<br>(Only HTTPS)</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Withdrawal Response Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>outAddress</sub>|<sub>Y</sub>|<sub>128</sub>|<sub>인출주소</sub>|<sub>거래소등에서 사용 가능한 채널 사용자의 퍼블릭 블록체인 주소</sub>|<sub></sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>인출 요청 통화코드</sub>|<sub>SLAYB</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>인출 대상 통화코드</sub>|<sub>VGEO</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>인출 요청 토큰 수량</sub>|<sub>1</sub>|
|<sub>withdrawalRate</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>인출환율</sub>|<sub>인출 환율</sub>|<sub>100%</sub>|
|<sub>toAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>전환수량</sub>|<sub>전환수량 = 요청수량 * 인출환율</sub>|<sub>1</sub>|
|<sub>withdrawalFee</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>인출수수료</sub>|<sub>인출 수수료</sub>|<sub>0</sub>|
|<sub>withdrawalAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>인출수량</sub>|<sub>인출수량 = 전환수량 - 인출 수수료</sub>|<sub>1</sub>|
|<sub>txId</sub>|<sub>N</sub>|<sub>128</sub>|<sub>TXID</sub>|<sub>블록체인 Transaction ID</sub>|<sub></sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub>SUCCESS, DECLINED</sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Withdrawal Sequence
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/05SEQ-01.Withdrawal-KR.jpg">
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/05SEQ-02.Withdrawal-KR.jpg">
<br>

* Withdrawal Interface JSON Sample
   
[Request]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xcce4726a8bca553e31c5341fa456a43062b46520",
    "fromCurrency" : "SLAYB",
    "toCurrency" : "VEGO",
    "fromAmount" : "1", 
    "withdrawalRate" : "100%",
    "toAmount" : "1",
    "withdrawalFee" : "0",
    "withdrawalAmount" : "1",
    "ReserveWID" : "d18194bf3a77974df0c8be6a58264ec2df860ad636b31fac6f3a0ea7f63724bb",
    "pinNumber" : "880154",
    "Papers" : "77974df0c8be6a58264ec2df860af3a0ea7f63724bbd18194bf3ad636b31fac6",
    "notifyUrl" : "https://Partners_URL/API/backNotify"
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : SUCCESS]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xcce4726a8bca553e31c5341fa456a43062b46520",
    "fromCurrency" : "SLAYB",
    "toCurrency" : "VEGO",
    "fromAmount" : "1", 
    "withdrawalRate" : "100%",
    "toAmount" : "1",
    "withdrawalFee" : "0",
    "withdrawalAmount" : "1",
    "txId" : "0x7104afd5b61c5df952c8e9afd2dafa222d543111b2ee3862c80502b3f2aed93b ",
    "status" : "SUCCESS", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : DECLINED]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xcce4726a8bca553e31c5341fa456a43062b46520",
    "fromCurrency" : "SLAYB",
    "toCurrency" : "VEGO",
    "fromAmount" : "1", 
    "toAmount" : "",
    "exchangeRate" : "",
    "txId" : "",
    "status" : "DECLINED",
    "errorInformation.errCd" : "B011",
    "errorInformation.reason" : "비밀번호 검증 오류",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```
<br>
<br>


## 6. Block Notify API (MW30P → Partners)

Block Notify API는 Exchange API, Withdrawal API 등 블록체인 트랜잭션관련 주요 거래에 대한 효율을 고려하여 비동기 방식으로 트랜잭션 처리결과(Block Confirm 여부)를 통보합니다. 

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Block Notify API|/api/Partners_URL|POST|application/json|
<br>

* Block Notify Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|    
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>netDivision</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>네트워크 구분</sub>|<sub>블록체인 네트워크 구분</sub>|<sub>SLAYB, VGEO</sub>|
|<sub>outAddress</sub>|<sub>Y</sub>|<sub>128</sub>|<sub>인출주소</sub>|<sub>내부주소(Exchange API) 또는 외부주소(Withdrawal API)</sub>|<sub></sub>|
|<sub>txId</sub>|<sub>N</sub>|<sub>128</sub>|<sub>TXID</sub>|<sub>블록체인 Transaction ID</sub>|<sub></sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>32</sub>|<sub>TXID 상태</sub>|<sub>블록체인 Transaction 상태</sub>|<sub>confirm</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Block Notify Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|    
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Same as requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Same as requested value</sub>|
|<sub>customerId</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>사용자 ID</sub>|<sub>전체 생태계에서 유일한 사용자 고유 ID(KEY)</sub>|<sub>userid@usermail.url</sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub>SUCCESS, DECLINED</sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 ("2. 보안적용 Guide" 참조)</sub>|<sub></sub>|
<br>

* Block Notify Sequence
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/06SEQ-01.Block_notify-KR.jpg">
<br>

* Block Notify Interface JSON Sample
   
[Request]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "netDivision" : "SLAY",
    "customerId" : "userid@usermail.url",
    "outAddress" : "0xcce4726a8bca553e31c5341fa456a43062b46520",
    "txid" : "0x7104afd5b61c5df952c8e9afd2dafa222d543111b2ee3862c80502b3f2aed93b",
    "status" : "confirm",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : SUCCESS]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "status" : "SUCCESS", 
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```

[Response : DECLINED]
```json
   {
    "merchantInformation.merchantId" : "000000000001",
    "merchantInformation.merchantSiteId" : "000001",
    "clientReferenceInformation.code" : "20220316192601000",
    "customerId" : "userid@usermail.url",
    "status" : "DECLINED",
    "errorInformation.errCd" : "XXXX",
    "errorInformation.reason" : "오류메시지",
    "sign" : "DEDC93DB5CFE0F06CBB54B937266D378C27E2DE985E999B7F319666857E6C9EE"
   }
```
<br>
<br>
