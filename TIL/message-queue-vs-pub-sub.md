## Message Queue란
* 하나의 Publisher에 0..N개의 Consumer가 존재할 수 있다
* Publisher와 Consumer의 연관 관계를 Channel이라고 부른다
* Publisher에서 방출된 메시지는 하나의 Consumer에게 전달된다

## Pub/Sub이란
* 하나의 Publisher에 0..N개의 Subscriber가 존재할 수 있다
* Publisher와 Subscriber의 연관 관계를 Topic이라고 부른다
* Publisher에서 방출된 메시지는 연관된 모든 Subscriber에게 전달된다

## 결론
* 상황에 맞게 사용하는 것이 바람직하다
* Subscriber의 구독이 필요할 경우 Pub/Sub을 사용하고, 단 하나의 Consumer에서만 처리해야 하거나 Side Effect가 존재할 경우 Message Queue를 사용하면 된다
* Apache Kafka등의 성숙한 스트림 처리 플랫폼은 두 방법 모두를 지원