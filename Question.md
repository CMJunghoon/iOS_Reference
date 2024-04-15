## 동기(Synchronous) vs 비동기(Asynchronous) 작업 

![image1](https://miro.medium.com/v2/resize:fit:828/format:webp/1*iOPsNyddAorJAZlhVguwJg.png)

### Swift에서 동기 작업과 비동기 작업의 차이점을 설명하세요. 각각은 언제 사용하나요?

동기식 작업과 비동기식 작업은 각각 고유한 특성과 사용 사례를 가진 서로 다른 코드 실행 방식을 의미합니다.  
  
메인 스레드가 차단되면 앱이 응답하지 않거나 멈추는 등 사용자 환경이 저하될 수 있으므로 메인 스레드의 응답성을 유지하는 것이 중요합니다. 따라서 다소 시간이 걸릴 수 있는 작업은 비동기적으로 수행하여 메인 스레드가 사용자 상호 작용을 계속 처리하고 사용자 인터페이스를 원활하게 업데이트할 수 있도록 해야 합니다.

## 동기 작업( Synchronous )
동기식 작업에서는 프로그램이 다음 코드 줄로 넘어가기 전에 작업이 완료될 때까지 기다립니다. 작업이 완료될 때까지 실행 흐름이 차단됩니다. 동기식 작업은 순차적으로 차례로 실행되기 때문에 추론하기가 간단합니다.  
  
일반적으로 빠르고, 차단되지 않으며, 네트워크 요청과 같이 잠재적으로 시간이 많이 걸리는 작업이 포함되지 않는 작업에 동기 작업을 사용합니다. 동기식 작업은 간단한 계산, 인메모리 데이터 조작 또는 눈에 띄는 지연이나 사용자 인터페이스의 정지 없이 빠르게 완료할 수 있는 기타 작업에 적합합니다. 
<div class="page-break" style="page-break-before: always;"></div>

예)
``` swift
func synchronousTask() {  
	print("Swiftable")  
	print("iOS")  
	print("Community")  
}  
  
synchronousTask()

// Swiftable  
// iOS  
// Community

```
각 코드 줄이 실행되기 전에 이전 코드 줄이 완료될 때까지 기다리기 때문에 모든 인쇄 함수가 차례로 실행됩니다. 
  
다른 예시를 통해 이해해 봅시다. `sort()` 메서드를 사용하여 문자열의 배열을 제자리에서 정렬하는 함수를 정의해 보겠습니다. `sort()` 메서드는 문자열의 기본 정렬 기준(알파벳 순서)에 따라 배열의 요소를 오름차순으로 재정렬하는 동기식 연산입니다.

예)
``` swift
var words = ["swiftable", "developer", "community"]  
func sortWords() {  
	words.sort() // synchronous in-memory sorting  
}  
  
sortWords()  
print("words: \(words)")  
// words: ["community", "developer", "swiftable"]

```

`sortWords()` 함수를 호출하면 현재 스레드에서 `words.sort()` 줄을 동기적으로 실행합니다. 즉, 현재 스레드(이 경우 메인 스레드)는 다음 코드 줄로 이동하기 전에 정렬 작업이 완료될 때까지 차단하고 대기합니다.  
  
이 경우 인메모리 배열을 정렬하는 데 동기식 연산을 사용하면 작업이 빠를 가능성이 높고 사용자 인터페이스가 눈에 띄게 지연되거나 멈추지 않으므로 허용됩니다.
<div class="page-break" style="page-break-before: always;"></div>

## 비동기 작업( Asynchronous )
비동기 작업에서는 프로그램이 작업이 완료될 때까지 기다리지 않습니다. 대신 비동기 작업이 완료될 때까지 기다리는 동안 다른 작업을 계속 실행합니다. 비동기 작업은 일반적으로 네트워크 요청, 파일 I/O 또는 애니메이션과 같이 완료하는 데 다소 시간이 걸릴 수 있는 작업에 사용됩니다.

예)
``` swift
let imageURL = URL(string: "https://example.com/image.jpg")!  
  
let task = URLSession.shared.dataTask(with: imageURL) { data, response, error in 
	if let data = data, let image = UIImage(data: data) {  
		DispatchQueue.main.async {  
		// update downloaded image to UI on the main queue  
		}  
	}  
}  
  
task.resume() // asynchronous image download
```

위의 예에서 이미지 다운로드 작업은 백그라운드 대기열에서 비동기적으로 수행되는 반면, 메인 대기열은 응답성을 유지하며 사용자 상호작용이나 다른 작업을 처리할 수 있습니다. 이미지 데이터가 다운로드되면 완료 핸들러가 호출되고 메인 대기열에 이미지를 할당하여 원활한 UI 업데이트를 보장합니다.  
  
비동기 이미지 로딩을 사용하면 시간이 오래 걸릴 수 있는 다운로드 프로세스 중에 메인 스레드가 차단되어 사용자 인터페이스가 응답하지 않거나 정지되는 것을 방지할 수 있습니다. 이 접근 방식은 네트워크 운영이나 기타 장기 실행 작업을 처리할 때에도 원활하고 반응이 빠른 사용자 경험을 보장합니다

각각의 사용 시기:  
동기 작업: 
특정 작업이 순차적으로 완료되도록 해야 하고 다음 코드 줄이 이전 작업의 결과에 따라 달라지는 경우 동기화 작업을 사용하세요. 동기식 작업은 실행 흐름을 차단하는 것이 허용되는 단순하고 수명이 짧은 작업에 적합합니다. 

비동기 작업: 
사용자 입력을 기다리는 네트워크 요청 작업과 같이 실행 흐름을 차단해서는 안 되는 장기 실행 작업을 수행해야 하는 경우 비동기 작업을 사용하세요. 비동기 작업은 UI의 반응성을 유지하고 작업을 동시에 실행하여 전반적인 성능을 향상시키는 데 필수적입니다.
<div class="page-break" style="page-break-before: always;"></div>

>Swift는 Grand Central Dispatch(GCD), Operations, 최신 async/await 구문 등 비동기 작업 작업을 위한 여러 메커니즘을 제공합니다. 이러한 메커니즘을 사용하면 백그라운드 대기열이나 컨텍스트에 작업을 디스패치하고 해당 작업의 결과 또는 완료를 비동기적으로 처리하여 메인 스레드가 계속 응답할 수 있도록 할 수 있습니다.