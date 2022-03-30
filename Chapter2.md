## Threads And Locks

스레드와 락은 하드웨어가 실제로 동작하는 것에 대한 약간의 formalization에 불과하며, 이는 강점이기도 하고 약점이기도 하다.

스레드 락 모델의 장점은 거의 모든 언어에서 지원한다는 것이고, 구현에 대해서 거의 제약사항이 없다는 것이다. 반면 단점은 프로그래머가 프로그램 짜기가 더 어렵고 유지보수하기 힘들다는 것이다.



스레드 세이프한 프로그램을 만들기 위해서, 락을 여러개 가질수도 있다. 하지만 락을 여러개 갖게 되면  데드락이 발생할 위험이 있다.



데드락을 해결하는 첫번째 방법은 락을 항상 fixed, global order로 획득하는것이다. 하지만 프로그램의 규모가 큰 경우 모든 코드가 하는일에 대해서 이해가 필요하기 때문에 실용적이지 않다.



또한 synchronized 함수에서 ailen method를 호출하는 경우에도 데드락이 발생할 수 있다. 이 경우 ailen method를 호출하기 하는 객체들을 clone하는 방법이 있다.



```java
// 데드락 발생가능
private synchronized void updateProgress(int n) {
  for (ProgressListener listener : listeners) {
    listener.onProgress(n);
  }
}

// defensive copy를 통해 데드락 가능성 차단
private void updateProgress(int n) {
  ArrayList<ProgressListener> listenersCopy;
  synchronized (this) {
    listenerCopy = (ArrayList<ProgressListener>)listeners.cline();
  }
  for (ProgressListener listener : listenersCopy) {
    listener.onProgress(n);
  }
}
```

## Beyond Intrinsic Locks

Intrinsic locks은 편리하지만 제한사항이 있다. 

- 먼저 블락된 스레드를 interrupt할수가 없다. 

- intrinsic lock에 timeout을 걸수가 없다.

- intrinsic lock을 획득하는 방법으로 `synchronized` block을 사용하는 방법이 있다.

  - ```java
    synchronized(object) {
      <<use shared resource>>
    }
    ```

    위 방법은 블락안에서만 잠금해제를 해야한다는 한계가 있다.

- 데드락이 발생했을 경우, 유일한 해결방법은 JVM을 죽이는것이다.

이러한 제한사항을 해결하는 방법이 `ReentrantLock`이다.

ReentrantLock은 interrupt가 가능하고 lock을 얻기위한 timeout을 걸수가 있다.

하지만 timeout을 걸어서 deadlock을 회피하려고 하는 경우 livelock 현상을 겪을 수 있다.

> Livelock현상이란 모든 스레드가 동시에 timeout 될 경우 데드락이 다시 발생하는 현상이다. 이 경우 데드락이 짧은 시간이 끝나더라도 곧이어 데드락이 다시 발생하기 때문에 프로그램이 진행이 되지 않게된다.

따라서 timeout방식은 데드락을 해결하는 좋은 방법이 아니다.

그밖에도 Condition Variable을 사용하면, 좀더 좋은 스레드 concurrency 조절이 가능하다. Philosopher Dining 문제를 생각해보면, ReentrantLock을 사용한 버전에서는 한 사람이 락을 두개 집어야 식사가 가능하기 때문에 한 순간에 한 사람만 식사를 할 확률이 더 높다. 하지만 Condition Variable을 사용한 버전에서는 양쪽에 있는 사람만 식사를 하지 않으면 식사를 할수 있다.

Atomic Variable을 사용하면 lock을 사용하지 않고 thread-safe한 코드를 짤수도 있다.

다음과 같은 두가지 장점이 있다.

- lock을 해제하는것을 잊어버릴 염려가 없다.
- lock이 없으므로 deadlock 걸릴 염려도 없다.



>`volatile` 키워드는 해당 변수를 읽고 쓰는 작업이 재배열이 안되도록 보장을 한다. 이는 JVM에서 lock을 통한 제어방식이 오버헤드가 컸을때는 유용했지만, 현재는 JVM에서 lock을 통한 제어가 오버헤드가 거의 없고, 해당 방식이 동기화를 완벽히 보장해주지 않기 때문에 사용할일이 거의 없다. 
>

## Thread Lock Model 강점, 약점

- 강점 : models's broad applicability
- 약점
  - Thread lock model은 오직 shared-memory 아키텍처만 지원
  - 단일 시스템에서 처리가능한 문제만 해결가능
  - 코딩하기 어렵다. => 유지보수하기 어렵고 확장하기 어렵다.

그 밖에 다른 언어에서도 스레드 락 모델을 지원하며, reordered memory access는 Java memory model에만 있는것이 아니지만, 대부분의 언어는 어떻게 언제 reordering이 허락될지 제약조건이 잘 정의되어있지 않다. 하지만 Java는 잘 정의되어있다. 



