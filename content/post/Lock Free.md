+++
date        = "2017-11-18T19:30:00+09:00"
title       = "Lock Free 알고리즘"
tags        = [ "lock", "concurrency"]
categories = [ "Concepts"]
thumbnailImage = ""
+++

# lock-free 정의

>**멀티스레드 환경에서 동시에 호출해도 정확한 결과를 만들어주는 알고리즘**

여러개의 스레드에서 동시에 작업이 호출되었을 경우 정해진 시간마다

적어도 한개의 작업이 호출되는 알고리즘.

흔히 사용하는 Mutex 를 사용하면 하나도 실행이 안될 수 있다.

하지만, lock-free는 적어도 1개는 무조건 실행이 되고 있어야 한다.

(작업이 몇 천만번이라면, 생각보다 큰 차이를 만들게 된다.)

호출이 다른 스레드와 충돌하였을 경우 적어도 하나의 승자가 있어서, 승자는 delay 없이 완료된다.

## Non-Blocking 알고리즘

* 다른 스레드가 어떤 상태에 있건 상관없이 호출 완료.

## wait-free 알고리즘

* 호출이 다른 스레드와 충돌해도 모두 delay 없이 완료 된다.

## PS

* lock 을 사용하지 않는 다고 lock-free 알고리즘이 아니다.
* lock 을 사용하면 무조건 lock-free 알고리즘이 아니다.

## blocking 알고리즘 예

```C++
mylock.lock();
q.push(item);
mylock.unlock();

mylock.lock();
sum = sum + 2;
mylock.unlock();

while(dataReady == false) {
  _asm mfense;
  temp = g_data;
}
```

* 위 코드는 dataReady 가 true 가 될때까지 busyWait 를 하게됨
* 여러 가지 이유로 dataReady 가 true 값이 되는 것이 지연될 수 있다. (cpu 스케쥴링)

## nonblocking 이란? 

* CAS 가 필요하다.
* CAS 란 CheckAndSwap 임
* lockfree 알고리즘의 핵심, 모든 싱글쓰레드 알고리즘을 lockfree 알고리즘으로 변환 할 수 있다.
* CAS(&target, old, new);
  * 의미 : target 의 값이 old 면 new 로 바꾸고 true를 리턴
  * 다른의미 : target 메모리를 다른 쓰레드가 먼저 업데이트 해서 false가 나왔다. 모든것을 포기하라.

```C++
#스레드 0
int old_sum = sum;
CAS(&sum, old_sum, old_sum+2);

#스레드 1
int old_sum=sum;
 CAS(&sum, old_sum, old_sum+2);
```

즉 여러 쓰레드가 충돌(경쟁)을 해도 적어도 한개는 업데이트에 성공한다는 것이다.
CAS를 수행하는 동안에 다른 스레드에서 작업을 할 수 있는 것이 lock (mutex) 과의 큰 차이일 것이다.

하지만, new 값이 업데이트가 실패했을 경우에 다시 값을 써야하는지에 대해서는 고민해볼 필요가 있을 것 같다.
스레드간 값 반영의 순서가 중요하지 않다면 re-try 해볼 수 있겠지만,
업데이트 순서가 중요하다면, CAS를 사용하면 안 될것 같다. (트랜잭션이 필요한 경우?)