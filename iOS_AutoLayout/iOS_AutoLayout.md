# AutoLayout

## 가변, 고정값에 대해서....

> App UI / UX 디자인 할때, 앱 전체의 가변,고정 영역에 대해서 충분한 고려가 되어야 합니다. 
> 
> 아래 그림과 같이 iOS 375x667로 디자인 한 것을 개발자에게 전달하면, 개발자가 전부
> 
> **고정값**으로 진행한다면 사이즈가 다른 단말기에서는 빈공간이 나타나게 됩니다. 

### 1. 전부 고정값으로 아무런 고려 하지 않는 경우

![auto_01.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/23-16-35-44-auto_01.png)iPhone 8 사이즈 기준으로 개발자가 고정값으로 진행 한다면 위 그림과 같은 결과물이 나타나게 됩니다.  
(극단적 예시)  

### 2. 고정된 크기의 가운데 컨텐츠을 고정하고, 나머지 부분을 가변으로 한 경우

![auto_02.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/23-16-35-48-auto_02.png)

### 3. 컨텐츠와 상단을 고정으로 하고 나머지 부분을 가변으로 한 경우

![auto_03.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/23-16-35-52-auto_03.png)

### 4. 상단,하단 값 고정하고, 버튼, 텍스트 이미지 크기를 가변으로 한 경우.

![auto_04.png](https://raw.githubusercontent.com/CMJunghoon/Resume/master/2019/10/23-16-36-13-auto_04.png)
