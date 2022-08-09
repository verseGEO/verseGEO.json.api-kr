# verseGEO P2E API Guide

- 홈페이지 : [verseGEO.com](http://versegeo.com/).
- 백서참조 : [verseGEO Whitepaper](http://versegeo.com/whitepaper-k.html).

## Introduction
verseGEO는 검증된 게임 플랫폼 및 서비스를 기반으로 실증 사용이 가능한 Token과 NFT 모델을 제공합니다. 또한 단계적으로 추가 오픈 예정인 각 게임의 세계관과 빅데이터가 통합되는 메타버스와 현실세계가 통합 가능한 디지털 생태계를 제공합니다.
  -	VERSEGEO는 다양한 게임 및 블록체인 비즈니스를 지원합니다.
  -	Game 또는 Contents 서비스의 포인트 또는 리워드에 대한 Tokenization을 통해 다양한 채널에서 사용 가능합니다.
  -	NFT에 대한 발행 및 유통/판매가 가능한 내부 NFT Marketplace와 외부 NFT Marketplace를 지원합니다.

   <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/eco.png">


## WEB 3.0 Infra
MW30P API는 블록체인의 태생적 이슈와 문제점인 느린 네트워크, DAPP개발의 기능적 제한 등 인프라 구성의 접근성을 해결함과 기존 서비스 및 시스템(게임, 쇼핑, SNS등)가 블록체인에 용이하게 참여할 수 있도록 지원함을 목적으로 합니다. 

   <img src="https://github.com/verseGEO/verseGEO.json.api-kr/blob/main/web30.svg">

Metaverse WEB 3.0 Platform은 지속적으로 발전하고 있는 블록체인과 기존 기술 및 서비스에 대한 수용과 적용을 통해 WEB3.0기반 제공과 다양한 블록체인 플랫폼 수용을 용이하게 합니다.


## MW30P P2E Interface
API는 성능을 고려하여 Sync 타입과 Async 타입으로 분류됩니다. 제휴사는 각 API에 해당되는 기능을 적용하며 JSON, C/C++, JAVA등 다양한 개발언어를 지원합니다.

| API | 요청 | 처리 | 기능 | 비고 |
|----------------------------|--------|--------|------|------|
|Exchange Rate Inquiry|Partners|MW30P|Item/Point/etc to PlayToken 환율 조회<br>PlayToken to Item/Point/etc 환율 조회<br>(파트너사가 사용자에게 제공하는 각종 혜택 등)||
|Exchange |Partners|MW30P|Item/Point/etc to PlayToken 전환<br>PlayToken to Item/Point/etc 전환|Async|
|Passport|Partners|MW30P|주요 API 실행을 위한 인증 서비스||
|Withdrawal Address|Partners|MW30P|Partner사의 사용자가 보유한 외부에서 사용할 수 있는<br>블록체인 주소에 대한 검증 및 등록/변경 (인출주소) ||
|Withdrawal pre-trade|Partners|MW30P|인출 예비 거래||
|Withdrawal|Partners|MW30P|인출 가능 토큰(PlayToken 등)을 외부 주소로 인출|Async|
|Block Notify|MW30P|Partners|Exchange API, Withdrawal API의 블록체인 처리 결과 전송<br>(Confirm 완료 여부)|Async|



