# verseB P2E API Guide

## Introduction
verseB P2E API는 블록체인의 태생적 이슈와 문제점인 느린 네트워크, DAPP개발의 기능적 제한 등 인프라 구성의 접근성을 해결함과 기존 서비스 및 시스템(게임, 쇼핑, SNS등)가 블록체인에 용이하게 참여할 수 있도록 지원함을 목적으로 합니다. 

verseB Blockchain Platform은 지속적으로 발전하고 있는 블록체인과 기존 기술 및 서비스에 대한 수용과 적용을 통해 WEB3.0기반 제공과 다양한 블록체인 플랫폼 수용을 용이하게 합니다.

## 1. Exchange Rate API (제휴사 → verseB)

Exchange Rate 조회 API는 게임 또는 서비스의 Item, Point등에 대한 Play Token 변환 전 반드시 호출해 환율을 참조해야 합니다. Exchange Rate 조회를 통해 참조된 환율은 후속 API인 “Exchange 요청 API”에 적용되어 Play Token으로 전환 가능합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Exchange Rate API|/api/ExRate.json|POST|application/json|

## 2. Exchange Request API (제휴사 → verseB)

Exchange Rate 조회 후 수신된 환율기준으로 Item 또는 Point를 Play Token으로 전환 요청합니다. 이때 Play Token용 주소가 미발급 상태이면 verseB 측에서 Play Token 주소를 자동할당 하게 됩니다. 전환하게 될 Item 또는 Point는 Exchange Rate 조회에서 수신된 환율에 따라 해당 수량을 계산하여 수량이 최소 수량 이하이면 거래 불가에 대한 안내를 수행해야 합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Exchange Request API|/api/ExReq.json|POST|application/json|

## 3. P2E Withdrawal Address Verification API (제휴사 → verseB)

P2E 출금주소 검증 요청은 Game 또는 Contents 등 Metaverse 생태계 내부에서 먼 사용 가능한 Play Token을 블록체인 Public Chain에 전송하기 위해 필요한 Ethereum 주소의 정합성을 검증하는 기능입니다. P2E 출금주소 검증은 주소의 정당성 여부만 Check 합니다. 이에 따라 사용자는 P2E 출금주소를 반드시 정확히 입력해야 합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Withdrawal Address Verification API|/api/OutAddrVerify.json|POST|application/json|

## 4. P2E Password Registration API (제휴사 → verseB)

P2E 출금을 위해서는 사용자 비밀번호 등록이 필요합니다. 제휴사는 사용자에게 비밀번호를 입력 받아 검증후 저장합니다. 또한 등록 비밀번호는 verseB에 등록 요청을 하여 제휴사에서 1회, verseB에서 2회 검증하여 진행합니다. verseB에 등록되는 비밀번호는 제휴사에서 암호화한 비밀번호를 2차 암호화(단방향)여 적용합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Password Registration API|/api/RegOutPasword.json|POST|application/json|

## 5. P2E Withdrawal API (제휴사 → verseB)

P2E 출금은 Play Token을 외부 이더리움 주소로 전송하는 기능입니다. 외부 이더리움 주소 전송된 P2E Token은 거래소 등 다양한 분야에 자유롭게 사용 가능합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Withdrawal API|/api/ReqWithdraw.json|POST|application/json|

## 6. Notification of Processing Result API (verseB → 제휴사)

처리결과 알림 API는 Exchange 요청 API와 P2E 출금 요청 API의 블록체인 처리결과를 제휴사 서버에 제공하여 블록체인의 Node 지연으로 인한 성능 저하를 방지할 수 있도록 합니다. 제휴사 Server는 Exchange 요청과 P2E 출금 요청의 해당 txid의 상태를 수신 받은 처리결과 알림으로 진행상태를 업데이트 처리합니다.

* REST API Interface Specification

| API | API URI |Method|Content-Type|
|-----|---------|------|------------|
|Notification of Processing Result API|/api/Channel Request URL.json|POST|application/json|
