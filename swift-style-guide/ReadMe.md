# iOS Swift Coding Style Guide

> 꼭 지키도록 하자.

### Code Layout

띄어쓰기

* 타입은 한칸 공백을 둡니다.

* 콜론 `(:)`뒤는 공백을 둡니다.

* `Array<String>` / `Dictionary<String: Any>` 보다는
  
  `[String]` /  `[String: Any]` 를 사용합니다.
  
  ```swift
  let name: String
  let age: Bool
  let info: [String: Any]
  ```

줄바꿈

* 함수의 코드가 길어지면 파라미터 기준으로 줄바꿈합니다.
  
  ```swift
  let alert = UIAlertAction(
       title: "title",
       style: .default,
       handler: { action in
       // action        
  })
  ```
  
  ```swift
  func doSomthing(
      start: Date,
      end: Date,
      time: TimeInterval) {
     // action    
  }
  ```
  
  ```swift
  func doSomthing(
      start: Date,
      end: Date,
      time: TimeInterval
  ) -> String {
    return "string"
  }
  ```

* 파라미터에 클로저가 2개 이상일 경우 무조건 내려쓰기 입니다.
  
  ```swift
  UIView.animate(
     withDuration: 0.3,
     animations: {
      // animation        
  },
     completion: { isCompleted in
     // completed
  })
  ```

주석 & Color 값

* UIColor을 확장하여 명확하게 컬러 이름을 구분합니다.

* RGB 값을 주석에 꼭 표기하여 사용합니다.
  
  ```swift
  extension UIColor {
    convenience init(red: Int, green: Int, blue: Int, alpha: CGFloat = 1.0) {
      self.init(
        red: CGFloat(red) / 255.0,
        green: CGFloat(red) / 255.0,
        blue: CGFloat(red) / 255.0,
        alpha: alpha
      )
    }
  
    /// red: 30, green: 20, blue: 20
    static var RNRed: UIColor { return UIColor(red: 30, green: 20, blue: 20) }
  
    /// red: 100, green: 120, blue: 120
    static var RNYellow: UIColor { return UIColor(red: 100, green: 120, blue: 120) }
  
    /// red: 200, green: 220, blue: 220
    static var RNDarkgary: UIColor { return UIColor(red: 200, green: 220, blue: 220) }
  
    static var darkLightGray: UIColor { return UIColor(red: 200, green: 220, blue: 220) }
  
    static var panTone281: UIColor { return UIColor(red: 200, green: 220, blue: 220) }
    static var titleDarkGray: UIColor { return UIColor(red: 200, green: 220, blue: 220) }
  }
  ```

* 사용
  
  ```swift
  backgroundColor = .RNRed
  backgroundColor = .darkLightGray
  ```

Enum / Codable

* `Codable` 에서 `enum`을 사용할 때, 아래와 같이 사용한다.
  
  ```swift
  enum UserStatus: String, Codable {
    case revoke = "revoke"
    case good = "good"
    case stop = "stop"
    case reStart = "restart"
    
    func text() -> String {
      switch self {
      case .revoke:
        return "삭제됨"
      case .good:
        return "정상"
      case .stop:
        return "일시정지"
      case .reStart:
        return "재가입"
      }
    }
    
    func backgroundColor() -> UIColor {
      switch self {
      case .revoke:
        return .RNRed
      case .good:
        return .darkLightGray
      case .stop:
        return .titleDarkGray
      case .reStart:
        return .panTone281
      }
    }
  }
  ```

* 사용법
  
  ```swift
  func configre(item: UserStatus) {
      titleLabel.text = item.text()
      titleLabel.backgroudColor = item.backgroundColor()
  }
  ```


