# High Frequency Heap

### Static Variable

우리는 C\#에서 static \(정적\) 변수가 있다는걸 알고 있습니다.  
그런데,  이 static 변수가 어느 메모리 영역에 저장되는지는 잘 모릅니다.  
일반적으론 힙에 저장된다 라는 이야기가 많죠.  
그런데, 정확한 사실은 static 변수는 High-Frequency Heap 이라는 곳에 저장\(Allocate\)됩니다.

그래서 저는 이 새로운 힙에 대해서 말해보려고 합니다.

### High Frequency heap

![](../../.gitbook/assets/image%20%2822%29.png)





