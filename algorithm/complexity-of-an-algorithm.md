---
description: Notation and SOTA with Asymptotic
---

# Complexity of an Algorithm

### First of all

우리가 풀고, 구현하고, 사용하는 알고리즘은 어떻게 쓰는게 가장 효율적일까요?\
저는 이 질문의 답을 시간 복잡도와 공간 복잡도가 어떻게 될지를 아는것에서 시작한다고 생각해요.\
그리고 이 복잡도를 위해서는 자바스크립트나 C++, C# 등 사용하는 언어에 따라 알고리즘이 어떻게 변화하는게 효율적인지 알아야 합니다.\
그렇다면 시간 복잡도는 어떤 기준으로, 그리고 어떻게 측정 할 수 있을까요?

### Asymptotic notation

알고리즘을 구현하고 이를 최적화 하는 것은 다양한 고려사항이 존재합니다.\
예를 들면 알고리즘을 컴퓨터에서 연산 할 때와 노트북에서 연산 할 때의 성능 차이가 있겠죠. 이런 부분들 때문에 계산 복잡성을 표현 하기 위해 컴퓨터 성능에 관계 없이 독립인 방식으로 표현하게 됩니다.\
그걸 조금 더 쉽게 하기 위해 알고리즘 실행시간에서 필요한 가장 큰 부분들만 계산해서 이를 속도로 표기한것을 **점근적 표기법(Asymptotic notation)** 이라고 해요.

그러니까, 점근적 표기법은 알고리즘을 만들고 그 알고리즘이 잘 만들어 졌는지 평가하는 방법인거죠. 그리고 그걸 평가하는 기준은 주어진 데이터 기준으로 수행시간 이나 사용 공간이 얼마나 사용되는지가 되는거구요.

점근적 표기법에는 여러가지가 있지만 저는 빅 오, 오메가, 세타 이 3개를 먼저 이야기 해볼꺼에요.

### Basic notation operation

알고리즘의 효율성을 계산 할때는 데이터 개수 n 이 주어졌을 때 필요한 기본 연산 횟수로 표현 해요.\
그리고 점근 표기법에선 로직을 단순화 해서 최고차항의 차수를 기준으로 삼아요.\
예를 들어 $$n^2+n+9$$ 의 계산 복잡성을 가진 알고리즘이 있다고 한다면 그 알고리즘의 계산복잡도는 **** $$n^2$$ 이 되는거죠. 이렇게 해서 구한 계산복잡도로 같은 결과를 내는 알고리즘을 서로 비교하게 됩니다.



![](<../.gitbook/assets/image (26).png>)

추가로, 입력 크기 (데이터 개수)에 따라 알고리즘 실행 시간을 표현할 때 점근적 표기법을 사용한다면 자주 보게 되는 함수의 표현식은 위와 같아요.

{% hint style="info" %}
### State of the art

알고리즘은 언제나 다양한 이유로 변경되고 개선돼요. 보통은 환경이 바뀌거나 정확도를 올려할 필요가 있을 때 개선 작업을 진행하죠. 그리고 그렇게한 새 알고리즘이 기존 알고리즘의 성능보다 개선됐다면 이를 현재 최고 수준(SOTA) 라고 합니다.&#x20;

SOTA는 AI나 딥러닝 분야에서 주로 쓰는 용어라고 알고 있어요.\
그렇지만 유용한 표현이라고 생각해서 참고사항으로 넣어보았어요.
{% endhint %}

### Asymptotic tighter bound (Theta Notation)

![](<../.gitbook/assets/image (24).png>)

새로 만든 알고리즘이 "아무리 나쁘거나 좋더라도 $$n_0$$ 을 기준으로 오른쪽의 수치는 기존에 존재하는 알고리즘의 속도안에 있다" 라는 뜻이에요.\
그러니까, 위 그림에서처럼 $$c_1g(n)$$과 $$c_2g(n)$$의 사이에 f(n)이 있다는 말은 기존에 존재하는 알고리즘의 점근적 상한선(Big-O Nation)와 점근적 하한선(Omega Notation) 속도 사이에 있다는 말이죠.

### Asymptotic lower bound (Omega Notation)

![](<../.gitbook/assets/image (25).png>)

오메가 표기법은 알고리즘이 "최소한 기존의 비교하는 속도와과 같거나 혹은 좋지 않다" 를 의미해요. \
즉 어떤 알고리즘을 만들었을 때 알고리즘의 계산 복잡도가 $$n^2$$ 이라면 아무리 빨라도 $$n^2$$을 넘지 못하거나 더 느리다는 말이죠.&#x20;

### Asymptotic upper bound (Big-O Notation)

빅오 표기법은 한 줄로 말하면 "아무리 나빠도 이정도 속도가 소요된다." 라고 할 수 있어요.

알고리즘 수행 시간은 매번 수행할 때마다 데이나 하드웨어의 차이로 인해 변동이 됩니다.\
그런데 만약 알고리즘 수행 중 백번중에 한번 나온 속도로 표기 한다면 객관적인 알고리즘의 속도가 될 수 없겠죠?

그렇기 때문에 "제가 만든 알고리즘은 아무리 느려도 이만큼은 나와요!" 라고 말하는것이 가장 편한거죠.\
그래서 알고리즘 표기법중에 가장 많이 쓰입니다.

![Big-O Notation Graph](<../.gitbook/assets/image (23).png>)

### Data Structures Table

이건 일반적으로 구현하는 데이터 구조들의 속도를 표로 정리해둔 것이에요.\
만약 임의로 새로 구현해야 한다면 이 표를 참고해서 최소한 이정도 속도는 나오도록 구현하는 것이 좋아요.

| Data Structures    | Average Case |          |          | Worst Case |          |          |
| ------------------ | ------------ | -------- | -------- | ---------- | -------- | -------- |
|                    | Search       | Insert   | Delete   | Search     | Insert   | Delete   |
| Array              | O(n)         | N/A      | N/A      | O(n)       | N/A      | N/A      |
| Sorted Array       | O(log n)     | O(n)     | O(n)     | O(log n)   | O(n)     | O(n)     |
| Linked List        | O(n)         | O(1)     | O(1)     | O(n)       | O(1)     | O(1)     |
| Doubly Linked List | O(n)         | O(1)     | O(1)     | O(n)       | O(1)     | O(1)     |
| Stack              | O(n)         | O(1)     | O(1)     | O(n)       | O(1)     | O(1)     |
| Hash table         | O(1)         | O(1)     | O(1)     | O(n)       | O(n)     | O(n)     |
| Binary Search Tree | O(log n)     | O(log n) | O(log n) | O(n)       | O(n)     | O(n)     |
| B-Tree             | O(log n)     | O(log n) | O(log n) | O(log n)   | O(log n) | O(log n) |
| Red-Black tree     | O(log n)     | O(log n) | O(log n) | O(log n)   | O(log n) | O(log n) |
| AVL Tree           | O(log n)     | O(log n) | O(log n) | O(log n)   | O(log n) | O(log n) |

### Reference

{% embed url="https://ko.khanacademy.org/computing/computer-science/algorithms/asymptotic-notation/a/asymptotic-notation" %}

{% hint style="info" %}
[https://www.bowdoin.edu/\~ltoma/teaching/cs231/spring16/Lectures/01-analysis/analysis.pdf](https://www.bowdoin.edu/\~ltoma/teaching/cs231/spring16/Lectures/01-analysis/analysis.pdf)
{% endhint %}

{% embed url="https://www.mathway.com/ko/popular-problems/Algebra/716931" %}

{% embed url="https://meherchilakalapudi.wordpress.com/category/data-structures-1asymptotic-analysis/" %}



