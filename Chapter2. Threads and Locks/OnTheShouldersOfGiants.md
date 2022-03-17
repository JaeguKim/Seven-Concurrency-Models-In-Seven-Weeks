## Thread Lock Model 강점, 약점

- 강점 : models's broad applicability
- 약점
  - Thread lock model은 오직 shared-memory 아키텍처만 지원
  - 단일 시스템에서 처리가능한 문제만 해결가능
  - 코딩하기 어렵다. => 유지보수하기 어렵고 확장하기 어렵다.

그 밖에 다른 언어에서도 스레드 락 모델을 지원하며, reordered memory access는 Java memory model에만 있는것이 아니지만, 대부분의 언어는 어떻게 언제 reordering이 허락될지 제약조건이 잘 정의되어있지 않다. 하지만 Java는 잘 정의되어있다. 