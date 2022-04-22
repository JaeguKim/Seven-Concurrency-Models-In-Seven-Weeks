## Clojure 방식 - Identity와 State를 분리

clojure는 mutual 상태를 인식하고 있는 mutable variable을 지원하며, 이러한 성질을 impure하다고 한다.

클로저는 persistent data structure를 지원하는데, 이 자료구조는 수정될때 이전의 버전을 항상 보존한다. 마치 수정될때마다 전체 복사본이 생기는 방식으로 동작하긴하지만, 실제로 구현은 CopyOnWrite방식처럼 되어있지 않고 structure sharing을 이용해서 효율적으로 구현되어있다. 또한 identity와 state를 분리시키는데, 하나의 identity는 하나의 값만을 가질수 있다. 만약 우리가 identity에 해당하는 current state를 받아오면, 해당 state는 변하지 않는다. 

### 강점

- persistent data structure는 identity와 state를 분리하도록 허락하며, 이는 lock-based 프로그램이 가지고 있는 여러문제를 제거한다.
- functional programming과 mutual state사이의 좋은 균형점을 찾아서, 순수 함수형 언어보다 더 쉽다.

### 약점

third-party 라이브러리들의 도움없이는, 단일 시스템에서만 사용이 가능하다.(no support for distributed)

