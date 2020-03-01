## 예제 파일


```
name: Swift

on:
  push:
    branches:
      - master

jobs:
  build:

    runs-on: macOS-latest

    steps:
    - uses: actions/checkout@v2
    - name: Build
      run: xcodebuild clean -project MintWallet.xcodeproj -scheme MintWallet -destination "platform=iOS Simulator,OS=13.3,name=iPhone 8" CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO ONLY_ACTIVE_ARCH=NO
    - name: Bundle Install
      run: bundle install
    - name: fastlane
      run: fastlane test
```
