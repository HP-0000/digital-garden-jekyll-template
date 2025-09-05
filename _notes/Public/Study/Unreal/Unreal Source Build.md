---
---


1. Git 에서 Release 브랜치의 코드를 받는다
https://github.com/EpicGames/UnrealEngine

[5.6.1 release]

아래 배치 파일을 순서대로 실행한다.
- Setup.bat 
- GenerateProjectFiles.bat

2. 설치 빌드

개발자가 엔진 코드를 수정한 뒤 설치 빌드를 만들어 팀원에게 배포한다.

https://dev.epicgames.com/documentation/ko-kr/unreal-engine/installed-build-reference-guide-for-unreal-engine

![](/Resource/20250905102704.png)

솔루션 파일을 열어 AutomationTool을 빌드한다.

나의 경우 위와 같은 취약성 패키지및 아래와 같은 유형의 경고 에러가 많이 생겼다.
```
39>C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\amd64\Microsoft.Common.CurrentVersion.targets(2433,5): warning MSB3277: "MySql.Data, Version=6.10.9.0, Culture=neutral, PublicKeyToken=c5687fc88969c44d"과(와) "MySql.Data, Version=9.4.0.0, Culture=neutral, PublicKeyToken=c5687fc88969c44d" 사이에 충돌이 발생했습니다.
```

솔루션용 Neget 패키지 관리의 통합 탭에서 .net8.0의 지원 수준에 맞게 종속성을 일일이 업데이트 해주었다.

```
해결할 수 없는 "System.Text.Json"의 다른 버전 간 충돌이 발견되었습니다.
35>C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\amd64\Microsoft.Common.CurrentVersion.targets(2433,5): warning MSB3277: "System.Text.Json, Version=8.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51"과(와) "System.Text.Json, Version=9.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51" 사이에 충돌이 발생했습니다.

35>C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\amd64\Microsoft.Common.CurrentVersion.targets(2433,5): warning MSB3277:     "System.Text.Json, Version=8.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51"이(가) 기본 항목이므로 선택되었습니다. "System.Text.Json, Version=9.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51"은(는) 기본 항목이 아닙니다.

35>C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\amd64\Microsoft.Common.CurrentVersion.targets(2433,5): warning MSB3277:     "System.Text.Json, Version=8.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51"[C:\Program Files\dotnet\packs\Microsoft.NETCore.App.Ref\8.0.19\ref\net8.0\System.Text.Json.dll]에 종속되는 참조입니다.

```

읽어보면 System.Text.Json 이 Net8.0 에서 8.0으로 제공되는 반면 다른 전이된 패키지에서 
9.0으로 제공되어 생긴 일인 것같다.


![](/Resource/20250905120015.png)

저 두 프로젝트를 한번 체크해서 System.Text.Json 을 전이시키는 패키지 버전을 낮춰야 한다.

![](/Resource/20250905120218.png)
잘 보니 MySql 버전을 적절히 낮춰야 할 듯 하다.  

빌드가 완료 된 후 아래 배치파일을 실행했다.

```
@echo off
cd Engine\Build\BatchFiles RunUAT.bat BuildGraph 
-target="Make Installed Build Win64" 
-script=Engine/Build/InstalledEngineBuild.xml 
-set:HostPlatformOnly=true 
-set:WithFullDebugInfo=true 
-set:WithDDC=false
pause
```

[[DDC(Derived Data Cache)]]

![](/Resource/20250905151454.png)

오류 하나를 마주했다.
https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/
Debugging Tools for Windows 를 설치해주자

![](/Resource/20250905153633.png)

끝! 만들어진 LocalBuilds 폴더를 공유하면 된다.