Node.js
=======
# 1. 노드의 정의
* Node.js는 Chrome V8 Javascript 엔진으로 빌드된 Javascript 런타임이다.
### 1.1 자바스크립트 런타임
* 노드는 자바스크립트 런타임이다. 런타임은 특정 언어로 만든 프로그램들을 실행할 수 있는 환경을 뜻한다.
### 1.2 이벤트 기반
* 이벤트 기반이란 이벤트가 발생할 때 미리 지정해둔 작업을 수행하는 방식을 의미한다. 이벤트로는 클릭, 네트워크 요청 등이 있을 수 있다.
* 이벤트 기반 시스템에서는 특정 이벤트가 발생할 때 무엇을 할지 미리 등록해두어야 한다. 이를 이벤트 리스너에 콜백 함수를 등록한다고 표현한다.

  * **이벤트 루프** : 이벤트 발생 시 호출할 콜백 함수들을 관리하고, 호출된 콜백 함수의 실행 순서를 결정하는 역할을 담당한다. 노드가 종료될 때까지 이벤트 처리를 위한 작업을 반복하므로 루프(loop)라고 부른다.
  * **백그라운드** : setTimeout 같은 타이머나 이벤트 리스너들이 대기하는 곳이다. 여러 작업이 동시에 실행될 수 있다.
  * **태스크 큐** : 이벤트 발생 후, 백그라운드에서는 태스크 큐로 타이머나 이벤트 리스너의 콜백 함수를 보낸다. 콜백 큐라고도 불린다. 콜백들은 보통 완료된 순서대로 줄을 서 있지만 특정한 경우에는 순사가 바뀌기도 한다.
