# 클래스 확장

* 일반적인 클래스 확장은 **상속**을 이용
* 클래스 내부에 선언된 함수와 프로퍼티 이외에 다른 프로퍼티나 함수를 추가
* 확장 함수는 기존 클래스 내에 정적으로 추가되지는 않음
* OOP 다형성 불가
* super 클래스의 함수 호출 불가ㅐ


# 프로퍼티 확장

* 프로퍼티 확장 가능
* getter에 의해 초기화해야 함

# 확장 구문의 위치에 따른 이용

* Top-level에 작성
* 확장 대상 클래스와 확장 프로퍼티or 함수를 따로 import해야 함
* 다른 클래스 내에서 작성
  * extension receiver: 확장 대상이 되는 클래스
  * dispatch receiver: 확장 구문이 작성된 클래스
  * dispatch receiver 클래스 안에서만 사용 가능
