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

