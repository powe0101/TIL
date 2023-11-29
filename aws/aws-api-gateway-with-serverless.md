---
description: Serverless를 사용할 때 발생할 수 있는 문제들을 정리하고 해결하기
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# AWS API Gateway with Serverless

### AWS API Gateway

aws에서 제공하는 api gateway에서는 RestAPI를 주로 쓰게 되는데, 일반적으로 복잡한 작업을l하기a위해서 lambda 연결을 하는 경우가 많아요.

그런데 이 작업에서는 오픈된 환경은 상관 없지만, token(jwt)를 받아야 하는 경우에 인증 과정에 대한 Process를 잘 설계하고 AWS Serverless에 대한 이해도가 있어야 해요.

특히 Cognito를 사용한다면 IDP 를 연결하고, IAM Role에 대한 지식을 공부해야합니다.



#### Introduction.

최근 개발중인  프로젝트에서는 Cloud와 Client를 연동하고, 회원가입과 로그인, 그리고 계정에 맞는 DB와 Data를 저장하는 기능이 필요했어요.

이를 구현하기 위해  EC2를사용하거나, On-Promise 를 구축하거나, Serverless를 사용하는 방법이 있었고, 저는 AWS의 Serverless를 사용하기로 했어요.







