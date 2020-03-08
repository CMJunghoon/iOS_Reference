# Cocoapod으로 Library 배포 하기.

### 1. Sample 코드을 작성 할 프로젝트 생성.

> 일반적으로 Xcode 프로젝트을 생성하면 됩니다.

```textile
예) 
RenoLibrary
└── iOSExample
     ├── iOSExample
     │   ├── AppDelegate.swift
     │   ├── Assets.xcassets
     │   ├── Base.lproj
     │   ├── Info.plist
     │   ├── SceneDelegate.swift
     │   └── ViewController.swift
     └── iOSExample.xcodeproj
```

### 2. cocoapods 배포에 필요한  spec 파일 생성.

> RenoLibrary 폴더에서 아래 명령어을 치면
> 
> `RenoLibrary.podspec` 이 생성이 됩니다.

```shell
$ pod spec create RenoLibrary
```

### 3. Spec 파일 작성

> 아래와 같이 기본적으로 작성하시면 됩니다.
> 
> `cocoapod`버전은 `github` 의 `tag`에 버전명으로 `push`가 되고,
> 
> `spec` 을 이용해서 `cocoapod`에 `push`을 해야 됩니다.

```ruby
Pod::Spec.new do |spec|
  spec.name         = "RenoLibrary"
  spec.version      = "0.0.1"
  spec.summary      = "A short description of RenoLibrary."
  spec.homepage     = "https://www.renomedia.co.kr"
  # spec.screenshots  = "www.example.com/screenshots_1.gif", "www.example.com/screenshots_2.gif"

  spec.license      = { :type => "MIT", :file => "LICENSE" } # 라이센스 종류 및 파일 이름
  spec.author       = { "Reno" => "dev@RenoMedia.co.kr" } 
  spec.source       = { :git => "https://github.com/Reno/RenoLibrary.git", :tag => "#{spec.version}" }
  spec.source_files  = "Source/**/*"
  
  # spec.resource  = "icon.png"
  # spec.resources = "Resources/*.png"

  # spec.dependency "JSONKit", "~> 1.4" # 

end
```

### 4. pod으로 배포할 폴더 생성

> Source 폴더 생성

```shell
예) 
RenoLibrary
├── RenoLibrary.podspec # 생성된 pod
├── Source # 생성된 podspec에 source_files에 있는 path
└── iOSExample
     ├── iOSExample
     │   ├── AppDelegate.swift
     │   ├── Assets.xcassets
     │   ├── Base.lproj
     │   ├── Info.plist
     │   ├── SceneDelegate.swift
     │   └── ViewController.swift
     └── iOSExample.xcodeproj

```

### 5. 개발용 pod 생성.

```shell
iOSExample: $ pod init
```

```shell
podfile 수정

target 'iOSExample' do
  use_frameworks!
  pod 'RenoLibrary', :path => '../'
end
```

```shell
$ pod install
```
### 6. spec 파일 점검  

```shell
$ pod spec lint 
```

> 아래와 같이 `error / warning`에 대해서 수정이 필요합니다.

```shell
-> RenoLibrary (0.0.1)
    - ERROR | license: Sample license type.
    - WARN  | homepage: The homepage has not been updated from default
    - ERROR | source: The Git source still contains the example URL.
    - WARN  | summary: The summary is not meaningful.
    - ERROR | description: The description is empty.
    - WARN  | url: There was a problem validating the URL http://EXAMPLE/RenoLibrary.
    - ERROR | [OSX] unknown: Encountered an unknown error (The `RenoLibrary` pod failed to validate due to 3 errors:
    - ERROR | license: Sample license type.
    - WARN  | homepage: The homepage has not been updated from default
    - ERROR | source: The Git source still contains the example URL.
    - WARN  | summary: The summary is not meaningful.
    - ERROR | description: The description is empty.

) during validation.

Analyzed 1 podspec.

[!] The spec did not pass validation, due to 4 errors and 3 warnings.
```
