---
description: 시놀로지 도커를 이용해 실시간 파일 수정이 가능한 닷넷코어 3.1 LTS 서버를 구축하는 법에 대하여.
---

# Setting for synology docker with .Net Core 3.1

### Summary

처음 닷넷코어를 이용해 서버를 만드려고 했을 때 다른 블로그들을 많이 참조했어요.  
그런데 대부분의 블로그 글들은 닷넷코어 2.1을 사용하거나, 이미지를 만들어서 배포하는 글들이 많았어요.  
하지만 제가 원하는건 **도커를 이용해 실시간으로 수정하고, 이를 서버에 바로 반영할 수 있는 파일 시스템을 이용한, 닷넷코어 3.1 베이 서버 구축**이었거든요.

{% hint style="warning" %}
작성일 기준\(2021/03\) 닷넷코어가 닷넷 5.0으로 통합되긴 하지만,  
아직은 닷넷5가 안정적이라고 확신하지 못해서 닷넷코어 3.1을 사용하려고 해요.  
이후에 마이그레이션도 가능하니 보시는 분들은 참고해 주세요.
{% endhint %}

이런 부분에서 블로그를 찾고 계시거나 시행착오를 겪는 분들이 많을 듯해서  
이걸 정리해서 공유하려고 해요. 이를 위한 제가 사용한 환경은 다음과 같아요.

{% hint style="info" %}
IDE : Visual Studio 2019 \(vs2019\)  
Server : Synology 720+  
Base version : .Net Core 3.1 \(LTS\)
{% endhint %}

### Visual Studio Project Setting

![Create new project](../../.gitbook/assets/image%20%2814%29.png)

먼저 vs2019를 실행하고 닷넷코어의 새 웹 애플리케이션을 만들어요. 설명의 편의를 위해 기본 템플릿을 그대로 사용할꺼에요.

![Setting new project](../../.gitbook/assets/image%20%287%29.png)

그리고 적절한 위치에 프로젝트를 생성하고 만들기 버튼을 눌러요

![Preference for docker](../../.gitbook/assets/image%20%284%29.png)

우리는 도커를 이용해 서버를 호스팅할 예정이기때문에 Docker 지원 사용에 체크해야 해요.  
또 사용자 환경에 따라 HTTPS 구성에도 체크할 수 있어요. 저는 체크하고 넘어갈께요.  
아, 그리고 **ASP .Net Core 3.1** 과 **Linux** 로 선택하는것도 잊지 마세요.

이렇게 하면 기본적인 프로젝트 설정이 끝났어요.

### Make Release files

![Publish to server](../../.gitbook/assets/image%20%283%29.png)

![Select target server](../../.gitbook/assets/image%20%2811%29.png)

![](../../.gitbook/assets/image%20%2812%29.png)

  
먼저 시놀로지에 릴리즈한 파일을 보내기 위해 배포과정을 진행해요. 원격지의 웹서버를 사용할 것이기 때문에 IIS,FTP 등을 선택해요. 그리고 파일로 배포하기 때문에 게시 방법은 파일 시스템을 선택해요.

![](../../.gitbook/assets/image%20%289%29.png)

파일 시스템을 선택하면 설정을 잡아줘야 해요. 대상 프레임워크는 **netcoreapp3.1**로,  대상 런타임은 **이동 가능**으로 설정해요.

![](../../.gitbook/assets/image%20%2815%29.png)

이 작업이 끝나면 이제 게시\(Publish\)를 할 수 있어요. 버튼을 누르면 빌드가 되면서 릴리즈된 최종 결과물이 지정 폴더에 생성될꺼에요.  
  
생성된 파일들은 아래 사진처럼 시놀로지에 미리 옮겨두어요.

![](../../.gitbook/assets/image%20%2817%29.png)

### Publish to synology

이제 파일 준비가 끝났으니 도커에 옮길 준비를 해야해요.  
먼저 프로젝트 내에 자동으로 생성된 Dockerfile을 보면 이런 소스들이 보일꺼에요.

```text
#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Demo/Demo.csproj", "Demo/"]
RUN dotnet restore "Demo/Demo.csproj"
COPY . .
WORKDIR "/src/Demo"
RUN dotnet build "Demo.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Demo.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Demo.dll"]
```

우리가 봐야 할 건 3 line과 8 line의 FROM 부분이에요.  
각각 제공하고 있는 닷넷의 sdk 버전과 core 버전을 알려주고 있는데, 우리는 시놀로지 도커에 해당되는 레지스트리를 받아 설치할 꺼에요.

{% embed url="https://hub.docker.com/\_/microsoft-dotnet-aspnet/" caption="Microsoft official aspnet registry" %}

위의 허브 링크를 통해 마이크로소프트는, 오피셜한 이미지를 제공하고 있어요.

```text
docker pull mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim
```

그리고 제공된 Dockerfile 의 버전을 맞춰주기 위해 텔넷을 이용해 위의 명령어를 입력하고 도커 이미지를 다운로드 받아요. \(만약 정상적으로 다운로드가 되지 않으면 sudo -i 명령어를 이용해 먼저 root 계정으로 로그인 해요.\)

![Download aspnet docker image](../../.gitbook/assets/image%20%288%29.png)

정상적으로 다운로드를 받으면 아래 사진 처럼 dotnet/aspnet:3.1-buster-slim 이미지가 시놀로지 도커상에 존재하게 될꺼에요.

![](../../.gitbook/assets/image%20%2813%29.png)

그리고 환경에 맞는 도커 네트워크 포트를 구성하고 ASPNETCORE\_URLS에도 포트를 변경해줘요.

아 그리고 위 단계에서 복사했던 프로젝트 폴더도 마운트 해주도록 해요.

![](../../.gitbook/assets/image%20%286%29.png)

### Running .net core server

위 과정이 모두 끝나고 컨테이너를 실행하면 터미널을 컨트롤 할 수 있어요.

![](../../.gitbook/assets/image%20%2810%29.png)

```text
dotnet --info
```

설치가 정상적으로 됐다면 위 이미지처럼 dotnet이 동작하고 있는걸 확인할 수 있을꺼에요.

```text
cd demo
dotnet Demo.dll
```

그리고 나서 위 명령어를 터미널에 입력하면, 결과적으로 잘 동작하는 서버를 볼 수 있어요.  


![](../../.gitbook/assets/image%20%2816%29.png)

### Reference.

{% embed url="https://dotnet.microsoft.com/download" %}

{% embed url="https://hub.docker.com/\_/microsoft-dotnet-aspnet/" %}



