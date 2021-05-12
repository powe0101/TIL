---
description: Notation and SOTA with Asymptotic
---

# Complexity of an Algorithm

### First of all

 우리가 풀고, 구현하고, 사용하는 알고리즘은 어떻게 쓰는게 가장 효율적일까요?  
저는 이 질문의 답을 시간 복잡도와 공간 복잡도가 어떻게 될지를 아는것에서 시작한다고 생각해요.  
그리고 이 복잡도를 위해서는 자바스크립트나 C++, C\# 등 사용하는 언어에 따라 알고리즘이 어떻게 변화하는게 효율적인지 알아야 합니다.  
그렇다면 시간 복잡도는 어떤 기준으로, 그리고 어떻게 측정 할 수 있을까요?

### Asymptotic Notation

 우리는 항상 컴퓨터가 빨리 계산해서 결과를 보여주길 원해요. 그래서 알고리즘을 구현하고 이를 최적화 하는 것은 다양한 방식의 패턴이 있을 수 있고 이를 위한 다양한 상수와 계수가 존재합니다.  
예를 들면 알고리즘을 컴퓨터에서 연산 할 때와 노트북에서 연산 할 때에는 속도 면에서 큰 차이를 보일껍니다. 이런 부분들 때문에 수행 환경을 통제하고 계산 복잡성을 표현 할 때 컴퓨터 성능에 관계 없이 독립인 방식으로 표현하게 됩니다.

알고리즘 자체의 효율성은 일반적으로 basic operation, 즉 기본 연산의 횟수로 표현하는게 일반적인데요.  
이를 쉽게 표현하기 위해 알고리즘 실행시간에서 필요한 가장 큰 부분들만 계산해서 이를 속도로 표기한것을 **점근적 표기법\(Asymptotic notation\)** 이라고 해요.

그러니까, 점근적 표기법은 알고리즘을 만들고 그 알고리즘이 잘 만들어 졌는지 평가하는 방법인거죠. 그리고 그걸 평가하는 기준은 주어진 데이터 기준으로 수행시간 이나 사용 공간이 얼마나 사용되는지가 되는거구요.

그런 표기법에는 빅 오, 오메가, 세타 등이 있어요. 이걸 그래프로 표기해보고 알아볼께요.

### Basic Notation Operation

알고리즘의 효율성을 계산 할때는 데이터 개수 n 이 주어졌을 때 필요한 기본 연산 횟수로 표현 해요.  
그리고 점근 표기법에선 로직을 단순화 해서 최고차항의 차수를 기준으로 삼아요.  
그래서 **n^2 + n** 의 계산 복잡성을 가진 알고리즘이 있다고 한다면 그 알고리즘의 계산복잡도는 **n^2**이 되는거죠. 이렇게 해서 구한 계산복잡도로 같은 결과를 내는 알고리즘을 서로 비교하게 됩니다.

### State of the art

알고리즘은 언제나 다양한 이유로 변경되고 개선돼요. 보통은 환경이 바뀌거나 정확도를 올려할 필요가 있을 때 개선 작업을 진행하죠.   


### Asymptotic tighter bound \(Theta Notation\)

새로 만든 알고리즘이 "아무리 나쁘거나 좋더라도 기존의 비교하는 속도안에 있다" 라는 뜻이에요.

### Asymptotic lower bound \(Omega Notation\)

오메가 표기법은 알고리즘이 "아무리 빨라도 기존의 비교하는 속도와과 같거나 혹은 좋지 않다" 를 의미해요.   
즉 어떤 알고리즘을 만들었을 때 알고리즘의 계산 복잡도가 60점이라면 아무리 빨라도 60점을 넘지 못하거나 더 느리다는 말이죠. 

이 부분은 추가해서 차후 업데이트를 할꺼에요.

### Asymptotic upper bound \(Big-O Notation\)

빅오 표기법은 한 줄로 말하면 "아무리 나빠도 이정도 속도가 소요된다." 라고 할 수 있어요.

알고리즘 수행 시간은 매번 수행할 때마다 데이나 하드웨어의 차이로 인해 변동이 됩니다.  
그런데 만약 알고리즘 수행 중 백번중에 한번 나온 속도로 표기 한다면 객관적인 알고리즘의 속도가 될 수 없겠죠?

그렇기 때문에 "제가 만든 알고리즘은 아무리 느려도 100점 만점에 60점 정도의 속도가 나와요!" 라고 말하는것이 가장 바람직합니다. 이를 정리해서 최악의 경우 60점 정도의 속도를 가지는 알고리즘이라고 표기하는거죠.  
그래서 알고리즘 표기법중에 가장 많이 쓰입니다.

![Big-O Notation Graph](../.gitbook/assets/image%20%2823%29.png)

### Data Structures Table

| Data Structures | Average Case |  |  | Worst Case |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  | Search | Insert | Delete | Search | Insert | Delete |
| Array | O\(n\) | N/A | N/A | O\(n\) | N/A | N/A |
| Sorted Array | O\(log n\) | O\(n\) | O\(n\) | O\(log n\) | O\(n\) | O\(n\) |
| Linked List | O\(n\) | O\(1\) | O\(1\) | O\(n\) | O\(1\) | O\(1\) |
| Doubly Linked List | O\(n\) | O\(1\) | O\(1\) | O\(n\) | O\(1\) | O\(1\) |
| Stack | O\(n\) | O\(1\) | O\(1\) | O\(n\) | O\(1\) | O\(1\) |
| Hash table | O\(1\) | O\(1\) | O\(1\) | O\(n\) | O\(n\) | O\(n\) |
| Binary Search Tree | O\(log n\) | O\(log n\) | O\(log n\) | O\(n\) | O\(n\) | O\(n\) |
| B-Tree | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) |
| Red-Black tree | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) |
| AVL Tree | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) | O\(log n\) |

### Reference



{% embed url="https://www.mathway.com/ko/popular-problems/Algebra/716931" %}



