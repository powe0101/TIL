# Boyer Moore Horspool Algorithm

{% embed url="https://youtu.be/PHXAOKQk2dw" caption="보이어 무어 호스풀 알고리즘 참고 영상" %}

## Find Pattern

## Naive string search

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

결과값을 얻는 가장 전통적인 방식이에요. 하지만 이런 글자가 수십억개가 된다면?  
하나하나 일일히 비교하려면 굉장히 많은 시간이 걸릴꺼에요.   
그래서 저는 새로운 방법을 소개해보려고 해요.

## Boyer-Moore-Horspool search



## Reference.







{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/how-to/search-strings" %}



