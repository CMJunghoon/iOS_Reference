# Sign in Apple ID

> iPhone에서 사용하고 있는 AppleID을 활용하여 Login을 한다.
> 
> Apple에서 제공하는 ASAuthorizationAppleIDButton 객체을 이용하지 않아도 권한을 받는 것은 가능하다. 하지만, AppleReview 할때, 리젝 사유가 될 수 있다. 
> 
> [참고사이트](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)

#### Step1

```swift
import AuthenticationServices
```

* Apple Login Button 생성 및 권한 체크
  
  ```swift
  func viewDidLoad() {
      let loginButton = ASAuthorizationAppleIDButton()
      loginButton.addTarget(self, action: #selector(getCredentialState), for: .touchUpInside)
  }
  
  func getCredentialState() {
      // AppleID 권한 체크 먼저 진행.
      let appleIDProvider = ASAuthorizationAppleIDProvider()
      appleIDProvider.getCredentialState(forUserID: userIDs) { (credentialState, error) in
        switch credentialState {
        case .authorized:
          print("UserIdentifier가 승인된 상태") 
          print("UserIdentifier을 가지고 keychain에 저장된 정보을 가져와서 서버통신 진행.")
          break
        case .notFound:
          print("UserIdentifier가 없음")
          print("새롭게 AppleID을 받아 올 수 있도록 권한 요청을 해야됨.")
          break
        case .revoked:
          print("사용 거부됨.")
          print("AppleID을 더이상 사용하지 않도록 설정에서 로그아웃을 진행함.")
          break
        case .transferred:
          print("transferred:")
          break
        @unknown default:
          print("default:")
          break
        }
      }
  }
  ```

* AppleID 로그인 요청하기.
  
  ```swift
  func requestAppleID() {
      let request = ASAuthorizationAppleIDProvider().createRequest()
      // 2가지만 요청 할 수 있다.
      request.requestedScopes = [.email, .fullName]
      let controller = ASAuthorizationController(authorizationRequests: [request])
      controller.delegate = self
      controller.presentationContextProvider = self
      controller.performRequests()
  }
  ```

* AppleID 관련 Delegate 
  
  ```swift
  extension ViewController: ASAuthorizationControllerPresentationContextProviding {
    func presentationAnchor(for controller: ASAuthorizationController) -> ASPresentationAnchor {
      // 어느 뷰에 사용자 정보을 전달 해줘야 하는지 알려줘야 한다.
      return self.view.window!
    }
  }
  ```
  
  ```swift
  extension ViewController: ASAuthorizationControllerDelegate {
    func authorizationController(controller: ASAuthorizationController, didCompleteWithAuthorization authorization: ASAuthorization) {
      guard let authCredential = authorization.credential as? ASAuthorizationAppleIDCredential else { return }
      let userUnqieID = authCredential.user
      if let useremail = authCredential.email, let userName = authCredential.fullName {
        // 최초 로그인 성공
        // 관리하는 서버로 전송하여 권리 한다.
        // useremail : 유저가 선택한 이메일 주소 ( Private 또는 Public )
        // userName: 유저 이름
      } else {
        // userID을 통해서 키체인 또는 관리하고 있는 서버을 통해서 가입여부 확인
      }
  
    }
    func authorizationController(controller: ASAuthorizationController, didCompleteWithError error: Error) {
      // 로그인 실패
    }
  }
  ```
