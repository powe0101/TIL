---
description: CLR 메모리를 효율적으로 관리하고 최적화 하는 방법. 내부 동작 방식과 성능 최적화를 중심으로.
---

# CLR에서 GC를 다루는 방법 - 1

{% hint style="info" %}
이 글은 기본적인 GC나 C#의 기초에 대해서 다루진 않아요. \
CLR의 내부 처리에 대한 글을 정리하고자 아주 아주 간단하게 작성해 보았어요.&#x20;
{% endhint %}

### GC 내부 동작 원리

CLR의 가비지 컬렉션(GC)은 Mark-Sweep-Compact(이하 MSC) 알고리즘을 기반으로 해요.

처음 보는  알고리즘이어서, 이  알고리즘을 사용해 처리하는GC 처리 단계를 찾아보게 되었어요.

### Mark-Sweep-Compact

#### 1.1 루트 탐색 (Root Enumeration)

* GC는 스택, 스태틱 필드, GC 핸들 테이블을 검사하여 도달 가능한 객체를 식별.
* 루트로 식별된 객체만이 컬렉션 프로세스에서 유지.

#### 1.2 마킹 단계 (Marking)

* GC는 루트 객체부터 참조를 따라가며 도달 가능한 객체를 마킹.
* WeakReference 및 Finalizer의 특별 처리도 수행.

#### 1.3 스위핑 단계 (Sweeping)

* 마크되지 않은 객체는 정리 대상이 되며, 관리 힙에서 해제.

#### 1.4 컴팩션 단계 (Compaction)

* 메모리 단편화를 방지하기 위해 객체를 재배치하며, 포인터 갱신이 수행.

### Large Object Heap(LOH)

(작성하다 중단한)이전 포스팅에서 가볍게 설명했지만, LOH는 85KB이상의 대형 객체를 저장하는데, 일반적으로 압축이 수행되진 않아요. 그렇기 때문에 성능 최적화를 위해서 다양한 기법을 사용하게 되는데, 대표적으로 두가지가 있어요.

1. 메모리 풀링 : LOH를 효율적으로 관리하기 위해 활용
2. Generation 2 컬렉션 : 대형 객체는 주로 Gen 2 영역에서 수집되므로, 빈번한 할당은 주의해야 해요.

### Handle Table and Finalizer Queue

평소에 가볍게 사용하는 IDisposable 패턴이나 Finalizer는 리소스를 해제하는데 사용되는데, 실제로 내부 동작을 보면 Finalizer는 큐에 등록되어 GC 수집시 추가적인 내부 처리 과정을 거치게 돼요.&#x20;



### Server vs Workstation GC

CLR은 두가지의 Mode로 GC를 사용해요. 간단하게 말하면 Workstation GC, Server GC가 있어요.

일반적으로 Workstation GC는 이름 그대로 클라이언트 앱에서, Server GC는 ASP.NET등의 호스트된 앱에서 기본적으로 Server GC를 사용하게 됩니다.

### Reference

{% embed url="https://learn.microsoft.com/ko-kr/dotnet/standard/garbage-collection/workstation-server-gc" %}

{% embed url="https://learn.microsoft.com/ko-kr/dotnet/standard/garbage-collection/performance" %}

{% embed url="https://community.dynamics.com/blogs/post/?postid=9810fa84-62ec-4ff3-a5ec-b1bcd0d75d5c" %}
Undersstanding Garbage Collection in Microsoft.NET
{% endembed %}

