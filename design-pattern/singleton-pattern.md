# Singleton Pattern

### First

개발 하면서 한번쯤은 들어본 싱글턴 패턴. 한번쯤은 정리할 필요가 있을듯해서 기록을 남겨요.  
싱글턴은 짧게 말하면 **"프로그램 내에서 사용자에게 단 하나의 인스턴스만을 생성해서 사용하도록 강제한 것"** 이에요.

개발자 중에는 싱글턴을 좋아하는 사람도 있고, 싫어 하는 사람도 있어요. 그런데 중요한 것은 이게 왜 필요하고, 어떨 때 사용해야 하는지, 또 사용하지 않아야 할 상황은 어떤 때인지를 아는 것이라고 생각해요.  
"static은 되도록이면 사용하지 말라" 라는 말도 있는데, 이것 또한 상황에 따르다고 생각하구요.  


### Not thread-safe version

```text
public class SingleManager
{
    private static SingleManager instance = null;
    private SingleManager()
    {
        //TODO:
    }
    
    public static SingleManager Instance
    {
        get
        {
            if(instance == null)
                instance = new SingleManager();
            return instance;
        }
    }
}
```

### Traditional Thread-safe version

```text
public class SingleManager
{
    private static SingleManager instance = null;//default(SingleManager);
    private static readonly object lockObj = new object();
    
    private SingleManager()
    {
        //TODO:
    };
    
    public static SingleManager Instance
    {
        get
        {
            lock(lockObj)
            {
                if(instance == null)
                    instance = new SingleManager();
                return instance;
            }
        }
    }
}
```

### Lazy Thread-safe version

