# SNS

* 이벤트 프로듀서는 SNS에 메시지를 보낸다
* SNS Topic을 구독하는 컨슈머는 메시지를 받는다
* 토픽당 천만 컨슈머 구독 가능
* 토픽 제한은 10만개
* 구독은 아래 서비스가 가능
  * SQS
  * HTTP/S
  * 람다
  * 이메일
  * SMS
  * 모바일 알림
  