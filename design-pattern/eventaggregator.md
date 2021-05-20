# EventAggregator

### 개요

EventAggregator는 이벤트의 구독\(Subscribe\)과 출판\(Publish\)의 매커니즘을 가지고 있는 구조에요.이 패턴을 사용하면 이벤트들을 관리하는 기능을 제공하여 상대방의 직접적인 참조 없이 이벤트를 처리할 수 있도록 도와줄 수 있을꺼에요.

### 구조 알아보기

```csharp
public interface IEventAggregator;
public interface IApplicationEvent;
public class EventAggregator : IEventAggregator;
```

먼저 많이 사용하는 구조를 알아보기로 해요. 이 패턴은 이루는 세 개의 클래스와 인터페이스로 이루어져있어요. 자세한 사용법은 조금 후에 알아보기로 하고 하나씩 설명해 볼께요.

```csharp
public interface IEventAggregator 
{
        void Subscribe<T>(Action<T> action) where T : IApplicationEvent;
        void Unsubscribe<T>(Action<T> action) where T : IApplicationEvent;
        void Publish<T>(T message) where T : IApplicationEvent;
}
```

IEventAggregator 인터페이스는

* 이벤트를 등록하는 Subscribe
* 등록한 이벤트를 해지 하는 Unsubscribe
* 등록된 이벤트들에게 메세지를 전달하는 Publish 메서드로 이루어져있어요.

```csharp
public interface IApplicationEvent
{
}
```

IApplicationEvent 인터페이스는 실제로 아무 코드도 존재하지 않는데, 위 IEventAggregator 의 [where 제약 조건](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/where-generic-type-constraint)을 걸기 위해 사용했어요.

형식 제약은 EventAggregator를 사용하는 이벤트와 아닌 이벤트를 나누는데 유용해요.

```csharp
public class EventAggregator : IEventAggregator
{
        private readonly ConcurrentDictionary<Type, List<object>> subscriptions =
        new ConcurrentDictionary<Type, List<object>>();

    public static IEventAggregator Instance { get; } = new EventAggregator();

    public void Publish<T>(T message) where T : IApplicationEvent
    {
        List<object> subscribers;
        if (subscriptions.TryGetValue(typeof(T), out subscribers))
        {
            // To Array creates a copy in case someone unsubscribes in their own handler
            foreach (var subscriber in subscribers.ToArray())
            {
                ((Action<T>) subscriber)(message);
            }
        }
    }

    public void Subscribe<T>(Action<T> action) where T : IApplicationEvent
    {
        var subscribers = subscriptions.GetOrAdd(typeof(T), t => new List<object>());
        lock (subscribers)
        {
            subscribers.Add(action);
        }
    }

    public void Unsubscribe<T>(Action<T> action) where T : IApplicationEvent
    {
        List<object> subscribers;
        if (subscriptions.TryGetValue(typeof(T), out subscribers))
        {
            lock (subscribers)
            {
                subscribers.Remove(action);
            }
        }
    }

    public void Dispose()
    {
        subscriptions.Clear();
    }
}
```

실제 클래스는 위와 같이 멀티 스레드에 안전하도록 구현했어요. ConcurrentDictionary는 형식에 따라 이벤트들을 관리하는 List 객체를 가지고 있어요.  
그리고 Aggregator 클래스는 싱글턴 인스턴스로 활용했어요. 이를 통해 어디서든 호출 할 수 있도록 했어요.  
   
마지막 클래스의 사용 해제시엔 Dispose를 통해 Dictionary를 초기화 했어요.

### 이벤트 구독

이벤트의 구독은 먼저 대상이 되는 이벤트의 클래스를 정의해야해요.  
저는 여기에서 예시로 이벤트 클래스와 그 이벤트의 arguments를 다루는 클래스를 하나 만들어 볼께요.

```csharp
public class ProcessCompleteEvent : IApplicationEvent
{
   private ProcessCompleteEventArgs args;
   
   public ProcessCompletedEvent(ProcessCompleteEventArgs args)
   {
      this.args = args;
   }
}

public class ProcessCompleteEventArgs
{
   public int result = default(int);
   public string message = default(string);
   
   public ProcessCompleteEventArgs(int result, string message)
   {
      this.result = result;
      this.message = message;
   }
}
```

대상이 될 클래스와 이벤트를 만들었다면, 이제 구독을 할 차례에요. 구독은 다음과 같은 구조로 이루어져 있어요.

먼저 이벤트의 호출 대상 메서드를 만들고, 그 메서드를 Subscribe 하는 코드를 작성해요.

```csharp
private void OnEventMethod(ProcessCompleteEvent e)
{
    Console.WriteLine(MethodBase.GetCurrentMethod().Name.ToString());
}

EventAggregator.Instance.Subscribe<ProcessCompleteEvent>(OnEventMethod);
```

### 이벤트 전달하기

이렇게 이벤트를 위한 클래스를 만드는 작업이 끝나면 아래와 같은 코드로 이벤트를 publish 합니다.

```csharp
var args = new ProcessCompleteEvent(0, "Test Event Call");

EventAggregator.Instance.Publish<ProcessCompleteEvent>(args);
```

### 



