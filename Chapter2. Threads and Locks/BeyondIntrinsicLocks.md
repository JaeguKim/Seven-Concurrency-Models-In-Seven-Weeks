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
>
