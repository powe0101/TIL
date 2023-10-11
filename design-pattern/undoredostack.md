# UndoRedoStack

## 개요

UndoRedoStack은 실행 취소와 다시 실행을 하기 위해 구현한 디자인 패턴이에요.

다양한 소프트웨어에서 각자의 방법으로 구현 되어 있지만, 저는 조금 간단하게 필요한 기능만 구현하여 각 기능들을 액션(Action)으로 나누어 보았어요.

그리고 각 Action을 묶은 ActionBlock 클래스를 만들어 Sequence를 구현하도록 했어요.

이를 통해 복잡한 Undo 또는 Redo 작업을 한번에 처리 할 수 있어요.

#### IUndoableAction

```csharp
public interface IUndoableAction
{
    void Do();
    void Undo();
}
```

#### ActionBlock

```csharp
public class ActionBlock : IUndoableAction
{
    private readonly IUndoableAction[] actionSequence;

    public ActionBlock(params IUndoableAction[] actionSequence)
    {
        this.actionSequence = actionSequence;
    }

    public void Do()
    {
        foreach (var action in actionSequence)
            action.Do();
    }

    public void Undo()
    {
        foreach (var action in actionSequence.Reverse())
            action.Undo();
    }

    public override string ToString()
    {
        return string.Join(",", actionSequence.Select(a => a.ToString()));
    }
}
```



#### UndoRedoStack

{% code overflow="wrap" lineNumbers="true" %}
```csharp
public class UndoRedoStack
{
    private readonly Stack<IUndoableAction> undoStack = new Stack<IUndoableAction>();
    private readonly Stack<IUndoableAction> redoStack = new Stack<IUndoableAction>();

    /// <summary>
    /// 새로운 액션을 입력받았을때 각 IUndoableAction을 
    /// Implement한 클래스의 액션을 수행하고 기존의 Redo 스택은 지워진다.
    /// Undo 및 Redo 메서드 수행 시 Implement 된 클래스의 내용을 수행한다.
    /// </summary>
    /// <param name="action"></param>
    public void Do(IUndoableAction action)
    {
        action.Do();
        undoStack.Push(action);

        redoStack.Clear();
    }

    public void Undo()
    {
        RunAction(undoStack, redoStack, f => f.Undo());
    }

    public void Redo()
    {
        RunAction(redoStack, undoStack, f => f.Do());
    }

    private static void RunAction(Stack<IUndoableAction> source, Stack<IUndoableAction> target, Action<IUndoableAction> undoRedo)
    {
        if (source.Count <= 0)
            return;

        var action = source.Pop();

        undoRedo(action);
        target.Push(action);
    }
}
```
{% endcode %}
