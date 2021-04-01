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


### Managed Process

#### System Domain

The SystemDomain is responsible for creating and initializing the SharedDomain and the default AppDomain. It loads the system library mscorlib.dll into SharedDomain. It also keeps process-wide string literals interned implicitly or explicitly.

String interning is an optimization feature that's a little bit heavy-handed in the .NET Framework 1.1, as the CLR does not give assemblies the opportunity to opt out of the feature. Nonetheless, it saves memory by having only a single instance of the string for a given literal across all the application domains.

SystemDomain is also responsible for generating process-wide interface IDs, which are used in creating InterfaceVtableMaps in each AppDomain. SystemDomain keeps track of all the domains in the process and implements functionality for loading and unloading the AppDomains.

#### Shared Domain

All of the domain-neutral code is loaded into SharedDomain. Mscorlib, the system library, is needed by the user code in all the AppDomains. It is automatically loaded into SharedDomain. Fundamental types from the System namespace like Object, ValueType, Array, Enum, String, and Delegate get preloaded into this domain during the CLR bootstrapping process. User code can also be loaded into this domain, using LoaderOptimization attributes specified by the CLR hosting app while calling CorBindToRuntimeEx. Console programs can load code into SharedDomain by annotating the app's Main method with a System.LoaderOptimizationAttribute. SharedDomain also manages an assembly map indexed by the base address, which acts as a lookup table for managing shared dependencies of assemblies being loaded into DefaultDomain and of other AppDomains created in managed code. DefaultDomain is where non-shared user code is loaded.

#### Default AppDomain

DefaultDomain is an instance of AppDomain within which application code is typically executed. While some applications require additional AppDomains to be created at runtime \(such as apps that have plug-in architectures or apps doing a significant amount of run-time code generation\), most applications create one domain during their lifetime. All code that executes in this domain is context-bound at the domain level. If an application has multiple AppDomains, any cross-domain access will occur through .NET Remoting proxies. Additional intra-domain context boundaries can be created using types inherited from System.ContextBoundObject. Each AppDomain has its own SecurityDescriptor, SecurityContext, and DefaultContext, as well as its own loader heaps \(High-Frequency Heap, Low-Frequency Heap, and Stub Heap\), Handle Tables \(Handle Table, Large Object Heap Handle Table\), Interface Vtable Map Manager, and Assembly Cache.

### 

### Summary

**Loader Heap:** contains CLR structures and the type system  
**High Frequency Heap:** statics, MethodTables, FieldDescs, interface map  
**Low Frequency Heap:** EEClass, ClassLoader and lookup tables  
**Stub Heap:** stubs for CAS, COM wrappers, PInvoke  
**Large Object Heap:** memory allocations that require more than 85k bytes  
**GC Heap**: user allocated heap memory private to the app  
**JIT Code Heap:** memory allocated by mscoreee \(Execution Engine\) and the JIT compiler for managed code  
**Process/Base Heap:** interop/unmanaged allocations, native memory, etc

### 

### Reference

{% embed url="https://docs.microsoft.com/en-us/archive/msdn-magazine/2005/may/net-framework-internals-how-the-clr-creates-runtime-objects" %}





### 



### 

