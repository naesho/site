+++
date        = "2017-04-21T11:30:27+09:00"
title       = "Blocking-Nonblocking and Sync-Async"
tags        = [ "blocking", "nonblocking", "sync", "async", "io" ]
categories = [ "Concepts"]
thumbnailImage = "http://www.ibm.com/developerworks/library/l-async/figure1.gif"
+++

# IBM 에서 그려놓은 2x2 매트릭스

![IBM developerWorks의 단순화된 리눅스 I/O 모델 매트릭스](http://www.ibm.com/developerworks/library/l-async/figure1.gif)

# Blocking / NonBlocking

>**호출되는 함수가 바로 리턴되는가, 안되는가 차이**

blocking 은 말 그대로, 어떤 작업 (read, write) 을 하는 도중에는 다른 프로세스나 스레드가 작업을 할 수 없는 상태 (작업이 끝날때까지 리턴되지 않음)

NonBlocking 은
컨텍스트 스위칭 (context-switch) 를 통해서 프로세스나, 스레드가 각자 필요한 read, write 같은 시스템 콜을 번갈아 가며 사용가능 함.
잦은 시스템 콜, 컨텍스트 스위칭 때문에 비효율 적이다.
(작업이 덜 끝나도, 바로 리턴됨)

# Syncronous / Asynchronous

>**호출 되는 함수의 작업 완료 여부를 누가 신경 쓰는가 차이**

호출하는 쪽에서 호출되는 함수의 리턴을 기다리거나, 작업 완료를 계속 체크하려는 (동기화하려는) 시도를 한다면, Syncronous

호출하는 쪽에서는 완료를 신경 쓰지 않고 callback 을 넘겨서 호출되는 쪽에서 callback 을 실행하고 호출하는 함수는 작업의 완료 여부를 신경 쓰지 않는다면, Asynchronous

# 정리

동기-블록킹은 흔히 file 을 읽거나, 쓰기 시에 사용함.

비동기-블로킹은 대표적인 예는 select() 를 호출하는 경우임. select 호출시 블록킹은 하나, 결과 완료를 신경 쓰지 않음

동기-논블록킹의 EWOULDBLOCK 처럼 바로 끝나버리는 작업을 매번 체크함 (future.isDone())

비동기-논블록킹은 boost.asio



