# Javascript

JS는 가벼운, 인터프리터 혹은 just-in-time 컴파일 프로그래밍 언어로, 일급 함수를 지원합니다.
웹 페이지를 위한 스크립트 언어로 잘 알려져 있지만, Node.js, Apache CouchDB, Adobe Acrobat처럼 많은 비 브라우저 환경에서도 사용하고 있습니다.
Javascript는 프로토타입 기반, 다중 패러다임, 단일 스레드, 동적 언어로, 객체지향형, 명령형, 선언형(함수형 프로그래밍 등) 스타일을 지원합니다.

Javascript의 표준은 ECMAScript 언어사양 및 ECMAScript국제화 입니다.

## JIT(Just-in-time)
[참고](https://meetup.toast.com/posts/77)
JITC(just-in-time compilation)은 동적 번역이라고 불리우고, 이름에 맞게 프로그램을 실제 실행하는 시점에 바이트 코드를 기계어로 번약하는 컴파일 기법
굳이 한글로 하자면 그때그때로 해석이 가능하다.

현재 많이 쓰이고 있는 Javascript엔진 (Safari, Chrome, FireFox등)은 모두 JITC 방식을 사용합니다.

Javascript는 기본적으로 text 형태로 배포되기 때문에 이를 수행하기 위해서는 일단 변환이 필요합니다.
처음에 source code를 parsing하여 중간언어(IR, intermediate representation)인 bytecode 형태로 먼저 변환합니다.
그런 다음 interoreter 모드라면 bytecode를 하나씩 읽어가며 동작을 수행하고, JIT 모드라면 생성된 bytecode를 기반으로 native code로 컴파일 하여 수행하게 됩니다.
원래 JITC는 Java VM에서 많이 사용되던 방식인데, Java는 보통 bytecode형태인 .class 파일로 배포되기 때문에 parsing 과정이 생략됩니다.

그럼에도 불구하고 interpreter 수행시간보다 native code의 수행 성능이 월등히 낫기 때문에 JITC에 overhead가 포함되더라도 interpreter보다 빠르게 수행하므로
Java VM에서는 JITC를 많이 사용합니다.

#### Javascript에서는 JITC보다 interpreter가 더 낫다?
Javascript JITC가 최소한의 최적화를 적용한다는 점 외에도 두가지 문제가 더 있습니다.
Javascript는 잘 안려진 대로 변수의 타입이 수행 도중에 달라질 수 있고, class 대신 object로 상속되는 prototype-based방식을 사용하는 등
매우 동적인 언어이기 때문에 Javascript JIT compiler는 모든 예외적인 케이스를 다 고려하여 코드를 생성해야 합니다.

만약 두개의 변수 덧셈에서 int + int, string + string, int + string 등의 케이스를 모두 native code로 뽑아낸다면 엄청나게 많은 native code가 필요합니다.

또 다른 문제는 Javascript는 주로 web page의 layout을 건드리거나 사용자 입력에 반응하는 방식의 프로그램이 많습니다.
자주 반복되서 사용되는 구간(hotspot)이 매우 적다

#### Adaptive JIT Compilation
최근 Javascript엔진들을 대부분 adaptive compilation 방식을 택하고 있습니다.
Adaptive compilation이란, 모든 코드를 일괄적으로 같은 수준의 최적화를 적용하는 것이 아니라, 반복 수행되는 정도에 따라 유동적으로 서로 다른 최적화 수준을 적용하는 방식입니다.

기본적으로 모든 코드는 처음에 interpreter로 수행합니다. 그러다가 자주 반복되는 부분(hotspot)이 발견되면, 그부분에 대해서만 JITC를 적용하여 native code로 수행합니다.