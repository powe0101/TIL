---
description: 코루틴 대신 async 와 await 사용하기
---

# Async to coroutine

### Introduce

일반적으로 코루틴을 사용하는것은 비동기 문제 해결에 좋은 방법이에요.\
하지만, 코루틴은 에러가 발생하는 경우 이걸 해결하기 힘들고, 예외처리를 다루는데 불편함을 초래하죠.

그래서 이런 과정을 해결하기 위한 이론들을 이야기 해보려고 해요.

c#, 자바 스크립트에는 async/await 의 개념으로 적용되어있기도 한데요.

표현에 따라 다르지만 비선점형 멀티태스킹이 핵심 키워드인 개념이에요.

### Concurrency & Parallelism

동시성(Concurrency) 와 병렬성(Parallelism)은 비동기 스레드를 다루는데 중요한 개념이에요.\
먼저 동시성은 해야할 작업이 여러개 있다면 이 작업을 조금씩 나누어서 처리하는걸 말하고, 병렬성은 병렬성은 말 그대로 해야할 작업이 한번에 동시에 실행되는거에요.

이 두개의 개념이 등장하게 된 이유는 동시성을 보장하기 위해 나왔어요. 여러개의 작업을 수행할 때의 스케쥴링을 다루는 방법에 대한 이론인거죠.\
보통 이런 개념은 다양한 언어에서 사용되고, 최근엔 코틀린에서 꽤 이슈가 되었던 개념이기도 해요.

코루틴은 동시성은 제공하지만 병렬성은 제공하지 않아요.

이 말은, 위에서 말한 비선점형 멀티태스킹을 사용한다는 뜻으로 볼 수 있어요.

그러니까, 컨텍스트 스위칭을 하지 않는 다는 말인데요.

이런 컨텍스트작스위칭이 없는 작업의 순서를 결정하고 우선 순위를 정하는 것은 타임라인을 그려서 정리 할 필요가 있어요.

### Context Switching VS Object Switching

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

크게 보면 코루틴의 사용은 명시적 return을 하는 방법과 하지 않는 방법 두개로 나눌 수 있어요.

하지만, 어쨋든 코루틴의 기법은 하나의 작업을 많은 단위로 분할해서 진행하는게 핵심이기 때문에 함수를 중간중간 왔다갔다 할 수 있는 것이에요.

즉, Context Switching이 아닌 Object Switching 이라고 볼 수 있어요.

#### Stackless Coroutine

코루틴이 최초 실행 되면 메모리 영역에 코루틴 state를 생성합니다. 이는 value copy과 ref copy 모두 진행하게 되어 코루틴 반환 객체를 최종적으로 가지고 있다가 코루틴을 호출한 스레드에 넘겨줍니다.

결국 이러한 과정을 간소하게 표현해서 라이브러리화 한것은 async await의 개념이고, 이걸 신경 쓰면서 개발해야 하는 경우엔 엄밀히 말하면 coroutine이라기보단 비선점형 멀티스레딩을 직접 구현해 사용한다고 생각하면 됩니다.







### Reference

{% embed url="https://www.charlezz.com/?p=44635" %}

{% embed url="https://dev.to/noseratio/asynchronous-coroutines-with-c-8-0-and-iasyncenumerable-2e04" %}

###
