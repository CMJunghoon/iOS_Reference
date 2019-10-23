# 파일 Access 권한

## Objecticve-C

#### Framework Header File

![](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/14-17-37-02-framework_13.png)

`RenoSDK.h` 파일은  Framework의 Header 파일입니다. 

Header File의 권한은 `Public`권한을 가지고 있습니다. 

#### 파일 Access 권한

![framework_11.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/15-10-03-30-framework_11.png)

`Objecticve-C` 의 모든 `h`파일에 그림과 같이 파일의 권한 선택이 가능합니다. 

`project` `public` `private`  3가지로 선택이 가능합니다.

`project`는 프로젝트에서만 노출이 됩니다. Framework로 빌드 파일 자체가 안보입니다. 

`public` 는 Framework 빌드시에도 파일명이 노출이 되고, `Code 접근이 가능`한 파일입니다. 

`private`는 Framewok 빌드시에도 파일명이 노출이 되고, `Code 접근이 불가능` 합니다. 

## Swift

Framework Header File

Swift에서는 `RenoSDK.h`파일이 필요가 없습니다. 

`Objective-C` 처럼 파일 Access 는 아래 그림과 같은 위치에서 처리 됩니다. 

SDK Project -> Targets -> Build Phases -> Headers

![framework_14.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/15-10-34-08-framework_14.png)

원하는 위치에 파일을 이동 시키면 됩니다. 
