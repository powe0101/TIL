---
description: Serverless를 사용할 때 발생할 수 있는 문제들을 정리하고 해결하기
---

# AWS API Gateway with Serverless

{% hint style="info" %}
AWS API Gateway

aws에서 제공하는 api gateway에서는 RestAPI를 주로 쓰게 되는데, 일반적으로 복잡한 작업을하기 위해서 lambda 연결을 하는 경우가 많아요.

그런데 이 작업에서는 오픈된 환경은 상관 없지만, token(jwt)를 받아야 하는 경우에 인증 과정에 대한 Process를 잘 설계하고 AWS Serverless에 대한 이해도가 있어야 해요.

특히 Cognito를 사용한다면 IDP 를 연결하고, IAM Role에 대한 지식을 공부해야합니다.
{% endhint %}

### Introduction.

최근 개발중인  프로젝트에서는 Cloud와 Client를 연동하고, 회원가입과 로그인, 그리고 계정에 맞는 DB와 Data를 저장하는 기능이 필요했어요.

이를 구현하기 위해  EC2를 사용하거나, On-Promise를 구축하거나, Serverless를 사용하는 방법이 있었고, 저는 그 중에 AWS의 Serverless를 사용하기로 했어요.

### Cognito

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

Cognito를 만들고 사용하는 방법은 단순하지만, 실제로 사용하는 핵심 기능은 자격 증명 풀(idp)이에요.

idp 중 사용자 엑세스 탭을 보면 인증된 엑세스, 자격 증명 공급자, 게스트 엑세스로 나뉘어져 있어요.

우리는 이를 사용하여 사용자 별로 권한을 부여하고 비인증 접근을 차단할 수 있어요.

#### Auth Access









### Context.

우선 API Gateway를 설정해야해요. API의 종류는 Rest로 선택하고 진행합니다.

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

권한이 필요한 경우와 권한이 필요하지 않은 리소스의 종류 두 가지 예시로 설명하려고 해요.

#### Authorizers



<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

먼저, 권한이 필요한 경우엔 권한 부여자를 먼저 설정해야해요.

미리 만들어둔 cognito를 사용하여 api gateway의 api를 호출 하면  해당 cognito의 계정을 인증에 사용하도록 할꺼에요.



<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

&#x20;부여를 위해 권

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>
