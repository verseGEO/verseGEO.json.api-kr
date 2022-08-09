# verseGEO P2E API Guide

- 홈페이지 : [verseGEO.com](http://versegeo.com/).
- 백서참조 : [verseGEO Whitepaper](http://versegeo.com/whitepaper-k.html).

## 1. Exchange Rate API (Partners → verseGEO)

Exchange Rate Inquiry API는 환율 정보를 제공합니다(Item, Point, etc  PlayToken, PlayToken  Item, Point, etc). 이 API를 통해 Exchange API에 환율정보를 적용해야 합니다.

※ Exchange Rate Inquiry API의 응답결과인 환율은 별도 ADMIN거래로  등록이 필요합니다. 환율정보가 사전에 등록이 되어 있지 않으면 오류를 반환합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Exchange Rate API|/api/ExRate.json|POST|application/json|

* Exchange Rate Inquiry Request Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>0000000000013</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>000001</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>20220316192601001</sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>전환 요청 통화코드(Item, Token 등)</sub>|<sub>GOLD</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>전환 대상 통화코드(Item, Token 등)</sub>|<sub>SLAYB</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>Item, Point, Token 수량 (환율등록 어드민에서 지정한 최소 단위 이하로 요청 시 오류 반환)</sub>|<sub>100</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 (“2. 보안적용 Guide” 참조)</sub>|<sub></sub>|
<br>
   
* Exchange Rate Inquiry Response Interface Layout

| KEY |RQD|Len| Contents |Described|note|
|-----|:-:|:-:| -------- |---------|----|
|<sub>merchantInformation.merchantId</sub>|<sub>Y</sub>|<sub>50</sub>|<sub>채널번호</sub>|<sub>MW30P에서 할당된 채널 번호</sub>|<sub>Respond the same as the requested value</sub>|
|<sub>merchantInformation.merchantSiteId</sub>|<sub>Y</sub>|<sub>30</sub>|<sub>채널하위번호</sub>|<sub>MW30P에서 할당된 하위채널 번호</sub>|<sub>Respond the same as the requested value</sub>|
|<sub>clientReferenceInformation.code</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>거래번호</sub>|<sub>채널에서 생성하는 거래 유일값 (ex) System ID or Server ID+yyyMMdd+hhmmss+milisecond)</sub>|<sub>Respond the same as the requested value</sub>|
|<sub>fromCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>요청단위</sub>|<sub>전환 요청 통화코드(Item, Token 등)</sub>|<sub>GOLD</sub>|
|<sub>toCurrency</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>전환단위</sub>|<sub>전환 대상 통화코드(Item, Token 등)</sub>|<sub>SLAYB</sub>|
|<sub>fromAmount</sub>|<sub>Y</sub>|<sub>20</sub>|<sub>요청수량</sub>|<sub>Item, Point, Token 수량 (환율등록 어드민에서 지정한 최소 단위 이하로 요청 시 오류 반환)</sub>|<sub>100</sub>|
|<sub>toAmount</sub>|<sub>N</sub>|<sub>20</sub>|<sub>전환수량</sub>|<sub>전환수량 = 요청수량 * 환율</sub>|<sub>1</sub>|
|<sub>exchangeRate</sub>|<sub>N</sub>|<sub>20</sub>|<sub>환율</sub>|<sub>요청단위, 전환단위에 대한 환율정보</sub>|<sub>10%</sub>|
|<sub>status</sub>|<sub>Y</sub>|<sub>10</sub>|<sub>처리결과</sub>|<sub>성공(SUCCSS), 실패(DECLINED)</sub>|<sub>SUCCESS, DECLINED</sub>|
|<sub>errorInformation.errCd</sub>|<sub>N</sub>|<sub>8</sub>|<sub>오류코드</sub>|<sub>성공일 경우 NULL, 오류일 경우 코드 확인</sub>|<sub>See Error Code</sub>|
|<sub>errorInformation.reason</sub>|<sub>N</sub>|<sub>192</sub>|<sub>오류메시지</sub>|<sub>오류 발생시 해당 오류 메시지 </sub>|<sub>See Error Code</sub>|
|<sub>sign</sub>|<sub>Y</sub>|<sub>64</sub>|<sub>서명검증 값</sub>|<sub>보안 서명 (“2. 보안적용 Guide” 참조)</sub>|<sub></sub>|
<p>
   
* Exchange Rate Inquiry Sequence
<img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/01SEQ-01.Exchange_Rate_Inquiry-KR.jpg">

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

## 2. Exchange Request API (제휴사 → verseGEO)

Exchange Rate 조회 후 수신된 환율기준으로 Item 또는 Point를 Play Token으로 전환 요청합니다. 이때 Play Token용 주소가 미발급 상태이면 verseGEO 측에서 Play Token 주소를 자동할당 하게 됩니다. 전환하게 될 Item 또는 Point는 Exchange Rate 조회에서 수신된 환율에 따라 해당 수량을 계산하여 수량이 최소 수량 이하이면 거래 불가에 대한 안내를 수행해야 합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Exchange Request API|/api/ExReq.json|POST|application/json|

  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/02REQ-01.Exchange.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/02RES-01.Exchange.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/01SEQ-01.Exchange.jpg">


## 3. P2E Password Registration API (제휴사 → verseGEO)

P2E 출금을 위해서는 사용자 비밀번호 등록이 필요합니다. 제휴사는 사용자에게 비밀번호를 입력 받아 검증후 저장합니다. 또한 등록 비밀번호는 verseGEO에 등록 요청을 하여 제휴사에서 1회, verseGEO에서 2회 검증하여 진행합니다. verseGEO에 등록되는 비밀번호는 제휴사에서 암호화한 비밀번호를 2차 암호화(단방향)여 적용합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Password Registration API|/api/RegOutPasword.json|POST|application/json|

  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/03REQ-01.Password_registration.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/03RES-01.Password_registration.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/03SEQ-01.Password_registration.jpg">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/03SEQ-02.Password_change.jpg">


## 4. P2E Withdrawal Address Verification API (제휴사 → verseGEO)

P2E 출금주소 검증 요청은 Game 또는 Contents 등 Metaverse 생태계 내부에서 먼 사용 가능한 Play Token을 블록체인 Public Chain에 전송하기 위해 필요한 Ethereum 주소의 정합성을 검증하는 기능입니다. P2E 출금주소 검증은 주소의 정당성 여부만 Check 합니다. 이에 따라 사용자는 P2E 출금주소를 반드시 정확히 입력해야 합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Withdrawal Address Verification API|/api/OutAddrVerify.json|POST|application/json|

  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/04REQ-01.Withdrawal_address_verification.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/04RES-01.Withdrawal_address_verification.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/04SEQ-01.Withdrawal_address_verification.jpg">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/04SEQ-02.Withdrawal_address_change.jpg">


## 5. P2E Withdrawal API (제휴사 → verseGEO)

P2E 출금은 Play Token을 외부 이더리움 주소로 전송하는 기능입니다. 외부 이더리움 주소 전송된 P2E Token은 거래소 등 다양한 분야에 자유롭게 사용 가능합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Withdrawal API|/api/ReqWithdraw.json|POST|application/json|

  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/05REQ-01.Withdrawal.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/05RES-01.Withdrawal.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/05SEQ-01.Withdrawal.jpg">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/05SEQ-02.Withdrawal.jpg">


## 6. Notification of Processing Result API (verseGEO → 제휴사)

처리결과 알림 API는 Exchange 요청 API와 P2E 출금 요청 API의 블록체인 처리결과를 제휴사 서버에 제공하여 블록체인의 Node 지연으로 인한 성능 저하를 방지할 수 있도록 합니다. 제휴사 Server는 Exchange 요청과 P2E 출금 요청의 해당 txid의 상태를 수신 받은 처리결과 알림으로 진행상태를 업데이트 처리합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Notification of Processing Result API|/api/Channel Request URL.json|POST|application/json|
  
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/06REQ-01.Back-end_Notify.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/06RES-01.Back-end_Notify.jpg" width="80%">
  <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/src/06SEQ-01.Back-end_Notify.jpg">
  
  
