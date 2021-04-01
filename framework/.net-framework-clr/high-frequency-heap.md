# High Frequency Heap

### Static Variable

우리는 C\#에서 static \(정적\) 변수가 있다는걸 알고 있습니다.  
그런데,  이 static 변수가 어느 메모리 영역에 저장되는지는 잘 모릅니다.  
일반적으론 힙에 저장된다 라는 이야기가 많죠.  
그런데, 정확한 사실은 static 변수는 High-Frequency Heap 이라는 곳에 저장\(Allocate\)됩니다.

그래서 저는 이 새로운 힙에 대해서 말해보려고 합니다.

### High Frequency heap

![](../../.gitbook/assets/image%20%2822%29.png)

매니지드 프로세스는 세가지의 도메인으로 나뉩니다.

* System Domain
* Shared Domain
* Default AppDomain

이 중 High Frequency Heap은 System Domain에 속해있구요.  
보통 말하는 힙이라는것은 GC heap 을 말하는데 이 친구는 appdomain에서 동작하는 친구에요.  
  
**Loader Heap:** contains CLR structures and the type system  
**High Frequency Heap:** statics, MethodTables, FieldDescs, interface map  
**Low Frequency Heap:** EEClass, ClassLoader and lookup tables  
**Stub Heap:** stubs for CAS, COM wrappers, PInvoke  
**Large Object Heap:** memory allocations that require more than 85k bytes  
**GC Heap**: user allocated heap memory private to the app  
**JIT Code Heap:** memory allocated by mscoreee \(Execution Engine\) and the JIT compiler for managed code  
**Process/Base Heap:** interop/unmanaged allocations, native memory, etc





### 



### 

