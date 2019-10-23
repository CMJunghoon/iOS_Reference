# Framework 파일 생성

## 1. Xcode에서 파일 생성 방법

아래 그림과 같이 원하는 Framework Target을 선택하고, Edit Scheme 선택하여 Build config`Debug` / `Release` 선택을 해줍니다. 

> **시뮬레이터 또는 실제 디바이스 선택 해야 됩니다.**
> 
> 1. 시뮬레이터 빌드 -> Framework 내부에 Architecture가 i386, x86_64만 포함하기 때문에, 다른 프로젝트에 Framework을 전달 할 경우 실제 단말기에서 빌드을 할 수 없습니다. 
> 
> 2. 실제 디바이스 빌드 ->  Framework 내부에 Architecture가 armv7 armv7s arm64와 같은 Architecture로 빌드가 됩니다. 이것 또한 마찬가지로 실제 단말기로 빌드가 가능하고, 시뮬레이터에서는 빌드가 되지 않습니다. 
> 
> 3. Framework가 시뮬레이터 / 실제 디바이스 전부 빌드가 되어야 한다면, 2가지 전부 빌드 후 Architecture 합치기을 하면 됩니다. ( `lipo` 추후 설명 하겠습니다. )

![framework_Build_03.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/15-17-01-40-framework_Build_03.png)

![framework_Build_02.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/15-17-01-37-framework_Build_02.png)

아래와 같이 셋팅을 하고 시뮬레이터 또는 실제 디바이스 선택 후 `CMD`+`B` 빌드합니다.

Products 폴더에 Framework 파일이 생성이 됩니다. 

![framework_Build_01.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/15-17-01-32-framework_Build_01.png)

## 2. 터미널 및 XcodeBuild 을 이용한 방법

아래 명령어로 터미널 실행

```bash
$ xcodebuild -workspace ${workspaceFilePath} -scheme ${target_CocoaPods} -configuration Release -sdk iphoneos ONLY_ACTIVE_ARCH=NO clean build
```

> `xcodebuild`: xcode에서 제공하는 터미널 빌드 명령어
> 
> `-workspace ${workspaceFilePath}`: xcworkspace파일이 있는 위치
> 
> 예) "/Users/junghoon/Develop/GitLab/BGFSelfPayment_iOS/SelfPayment.xcworkspace"
> 
> `-scheme ${target_CocoaPods}` : Target 이름 
> 
> 예) "BuySelfSDK"
> 
> `-configuration Release` : 빌드 타입 
> 
> `-sdk` : Framework을 만들 것이다.
> 
> `iphoneos ONLY_ACTIVE_ARCH=NO` : 실제 단말기의 arch으로
> 
> `clean` 깨끗하게..
> 
> `build` 빌드을 하고
> 
> `-quiet` 조용하게 

아래 그림과 같이 성공적으로 빌드가 됩니다. 

![framework_Build_04.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/15-17-35-53-framework_Build_04.png)

맨 밑에 있는 경로을 꼭 복사 해야됩니다. 

```shell
/Users/junghoon/Library/Developer/Xcode/DerivedData/SelfPayment-fuuxcveogcoknhdgutmodgfgasbh/Build/Products/Release-iphoneos/BuySelfSDK.framework
```

cp 명령어을 통해서 framework파일을 원하는 위치로 이동 합니다. 

```bash
cp -R {위 주소} {복사할 폴더위치}
```

## 3. Architecture 합치기

시뮬레이터로 빌드와 단말기 빌드 각각 Framework파일을 준비 합니다. 

아래와 같이 `lipo`명령어로 합치기을 합니다.

```
# lipo -create -output {합치기 후 파일 생성 경로} {시뮬레이터 Framework 파일경로} {단말기 Framework 파일 경로}
```

## 4. sh 파일 만들기.

완전한 자동을 빌드을 하기 위해서 사용해던  sh 파일의 내용입니다 

1. 기존 SDK 파일 및 폴더 삭제

2. 디렉토리 생성

3. 시뮬레이터로 빌드 후 원하는 폴더에 Copy

4. 단말기 빌드 후 원하는 폴더에 Copy

5. 아키텍쳐 합치기

6. 다시 원하는 위치로 최종 SDK파일 복사

```shell

# 변수
# 프레임워크 이름
export framework_Name="BuySelfSDK.framework"

# Xcode 타켓 이름
export target_CocoaPods="BuySelfSDK"
export target_CocoaPods_X="BuySelfSDK_Library"

# 프레임워크 저장위치
export frameworkFolder="/Users/junghoon/Develop/GitLab/BuySelf_Framework/BuySelf_CocoaPods"

# 워크스페이즈 위치
export workspaceFilePath="/Users/junghoon/Develop/GitLab/BGFSelfPayment_iOS/SelfPayment.xcworkspace"

# 빌드 주소 ( 뒤 주소는 변경 될 수 있습니다. )
export build_DIR_Root="/Users/junghoon/Library/Developer/Xcode/DerivedData/SelfPayment-fuuxcveogcoknhdgutmodgfgasbh"

# 시뮬레이터 빌드 주소
export build_simulator_Path="Build/Products/Release-iphonesimulator"

# 디바이스 빌드 주소
export build_iphoneos_Path="Build/Products/Release-iphoneos"


# 기존 디렉토리 삭제
rm -rf ${frameworkFolder}

# 디렉토리 생성
mkdir -p ${frameworkFolder}/iphoneos
mkdir -p ${frameworkFolder}/iphonesimulator


# CocoaPods Target 빌드 ( 시뮬레이터 빌드 )
# xcodebuild -workspace ${workspaceFilePath} -scheme ${target_CocoaPods} -configuration Release -sdk iphonesimulator ONLY_ACTIVE_ARCH=NO clean build -quiet

# 시뮬레이터 빌드 Framework 파일을 원하는 폴더로 Copy
# cp -R ${build_DIR_Root}/${build_simulator_Path}/${framework_Name} ${frameworkFolder}/iphonesimulator/

# CocoaPods Target 빌드 ( 디바이스 빌드 )
xcodebuild -workspace ${workspaceFilePath} -scheme ${target_CocoaPods} -configuration Release -sdk iphoneos ONLY_ACTIVE_ARCH=NO clean build -quiet

# 디바이스 빌드한 Framework 파일을 원하는 폴더로 Copy
# cp -R ${build_DIR_Root}/${build_iphoneos_Path}/${framework_Name} ${frameworkFolder}/iphoneos/

# 디바이스 빌드한 Framework 파일을 원하는 폴더로 Copy
cp -R ${build_DIR_Root}/${build_iphoneos_Path}/${framework_Name} ${frameworkFolder}/

# 아키텍쳐 합치기 ( 시뮬레이터 / 디바이스 )
# lipo -create -output ${frameworkFolder}/${framework_Name}/BuySelfSDK ${frameworkFolder}/iphonesimulator/${framework_Name}/BuySelfSDK ${frameworkFolder}/iphoneos/${framework_Name}/BuySelfSDK


# 원하는 위치로 파일 복사.
export SDK_File_Path="/Users/junghoon/Develop/GitLab/BuySelfSDK_Sample/SDK_File"

rm -rf ${SDK_File_Path}/${framework_Name}/


# 디렉토리 생성
mkdir -p ${SDK_File_Path}

# CocoaPods용 SDK 파일을 원하는 위치에 복사
cp -R ${frameworkFolder}/${framework_Name} ${SDK_File_Path}
```
