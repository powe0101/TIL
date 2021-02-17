# Boyer Moore Horspool Algorithm

{% embed url="https://youtu.be/PHXAOKQk2dw" caption="보이어 무어 호스풀 알고리즘 참고 영상" %}

일반적인 문자열 검색은 다음과 같아요.

```text

string factMessage = "Extension methods have all the capabilities of regular static methods.";

// This search returns the substring between two strings, so
// the first index is moved to the character just after the first string.
int first = factMessage.IndexOf("methods") + "methods".Length;
int last = factMessage.LastIndexOf("methods");

Console.WriteLine(first);
Console.WriteLine(last);
```

> 결과값 : 17, 62





{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/how-to/search-strings" %}



