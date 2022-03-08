## Concurrent or Parallel

concurrent와 parallel은 종종 같은 의미로 사용되기도 하는데, 실제로는 전혀 다른 의미이다.

`concurrent program`이란 프로그램이 여러 logical thread들을 제어함을 의미한다.

반면 `parallel program`이란 프로그램이 여러 파트를 동시에 실행함을 의미한다.



또 다른 정의를 살펴보면, concurrency란 problem domain의 의미이다. 즉, 프로그램이 여러 이벤트들을 다룰때 concurrent하다고 표현한다.

반면 parallel이란 solution domain의 의미이다. 즉, 프로그램의 여러 부분을 병렬로 처리하여 속도를 증가시킬 수 있다는 의미이다.



## Example

>concurrent but not parallel : 만약 한 선생님이 수업시간에 여러 일을 동시에 처리한다면, concurrent하다고 볼수 있다. 하지만 parallel하지는 않은데 그 이유는 선생님은 한명이기 때문이다.
>
>parallel but not concurrent : 만약 수업시간에 여러 선생님이 greeting card를 작성해서 학생들에게 배포하는 일을 한다고 가정해보자. 이 경우는 parallel 하지만 concurrent하지는 않다. 왜냐하면 high level관점에서는 greeting card를 작성하는 일 하나만 처리하고 있는것이기 때문이다.



## Beyond Sequential Programming

Parallelism과 concurrency의 공통점은 traditional sequential programming model의 수준을 넘어선다는 것이다. concurrent program은 event의 타이밍으로 인해서 non deterministic 하다고 볼수 있고 parallel program은 배열에 있는 수들을 병렬로 2배하는 작업과 같이 deterministic하다고 볼수 있다.



## Parallel Architecture

### Bit-Level Parallelism

컴퓨터가 처리할수 있는 인스트럭션의 길이가 64비트까지 증가함에 따라서, 비트 레벨에서 한번에 처리할수 있는 비트수가 증가하였다. 히지만 하드웨어 구조상 한계에 도달했다.



### Instruction-level Parallelism

CPU 단일 코어의 성능을 높이는데는 한계에 부딪혔으므로, 여러 CPU 코어에서 인스트럭션을 실행하여 instruction level 병렬성을 높이고 있다.



### Data Parallelism

대량의 데이터에 대해서 하나의 같은 오퍼레이션을 병렬로 처리하는것을 의미한다. 대표적으로 GPU를 사용해서 image processing을 하는것을 예로 들수 있다.



### Task-Level Parallelism

Shard-memory 방식의 경우, 각각의 processor가 shard memory에 접근하여 서로 커뮤니케이션하는 방식이다.

Distributed-memory 방식의 경우, 각각의 로컬 메모리를 가지고 있고 processor 각각은 서로 네트워크를 통해서 통신하는 방식이다.

일반적으로 shared-memory 방식이 코드를 작성하기 더 쉽지만 processor의 수가 증가함에 따라서 결국은 bottleneck이 발생하여 distributed memory방식을 사용할 수 밖에 없다. 그리고 fault tolerant 시스템을 구현하기 위해서는 distributed memory방식을 사용할 수 밖에 없다.



## Concurrency: Beyond Multiple cores

- Concurrency는 앞에서 설명했듯이 여러가지 일을 동시에 처리하는 것이다. 따라서 resilient, fault-tolerant한 소프트웨어를 가능하게 한다.
- 여러가지 일을 처리하기 위해 sequential programming을 사용할 수 있다. 이 경우 단일 스레드에서 여러가지 일을 모두 고려해야 한다. 하지만 여러 스레드를 생성해서 각각의 스레드에게 하나의 일을 맞기면 코드도 훨씬 간단해질수 있다.



### The Seven Models

- Thread and locks
- Functional Programming : 프로그래밍 모델 자체가 thread들간의 mutual state를 제거하므로, 프로그램 자체가 thread-safe하며 쉽게 병렬화될수 있다.
- The Clojure Way - separating identity and state : imperative, functional programming을 적절히 혼합하여 가장 좋은 접근 방법을 설정할 수 있다.
- Actor
- Communicating Sequential Processes : 메시지 전달에 기반을 두어 Actor model과 유사하지만, 커뮤니케이션 과정에서 사용되는 channel에 더 강조점을 두고 있다.
- Data parallelism
- The Lambda Architecture : MapReduce와 stream processing의 강점을 결합하여 다양한 빅데이터 문제를 해결하기 위한 모델
