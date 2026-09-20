## 킷 기본개념

<br>

## 01 UIKit이란?
→ UIKit은 iOS 앱의 화면과 사용자 이벤트를 구현하기 위한 프레임워크

```swift
import UIKit
```

UIKit에서는 화면을 구성할 때 `UIView`, `UIViewController` 같은 객체를 직접 만들고 설정함

그래서 스유와 비교할 때 킷은 스유보다 귀찮고 코드가 긺 ㅠㅠㅠㅠㅠ (근데그만큼자세히구현가능해서조음)

<br>
<br>

## 02 UIKit 클래스 구조

### 🍀 UIKit의 주요 객체는 다음과 같은 상속 관계를 가짐

| 클래스 | 상속하는 클래스 | 역할 |
| :--- | :--- | :--- |
| **UILabel** | UIView | 텍스트 표시 |
| **UIButton** | UIView | 버튼 표시 및 사용자 입력 처리 |
| **UIImageView** | UIView | 이미지 표시 |
| **UITextField** | UIView | 텍스트 입력 |
| **UIView** | UIResponder | 화면에 표시되는 UI 요소의 기본 클래스 |
| **UIViewController** | UIResponder | 하나의 화면 관리 |

- `UIView`: 화면에 표시되는 UI 요소의 기본 클래스
- `UIViewController`: 하나의 화면을 관리하는 클래스
- `UIResponder`: 터치와 키보드 입력 같은 이벤트를 받을 수 있는 객체

요기 위에 써둔건 기억해두기 !!

<br>
<br>

## 03 `UIView`
> `UIView`는 UIKit UI 요소의 기본 클래스

```swift
let boxView = UIView()
boxView.backgroundColor = .systemBlue
```

`UILabel`, `UIButton`, `UIImageView` 같은 요소도 모두 `UIView`를 기반으로 동작

<br>
<br>

## 04 `UIViewController`

> `UIViewController`는 하나의 화면을 관리함

```swift
import UIKit

final class MainViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .systemBackground
    }
}
```

화면에 필요한 UI를 만들고 사용자 이벤트를 처리하고 다른 화면과 데이터를 연결하는 역할을 함

<br>
<br>

## 05 `UIViewController` 생명주기

UIKit에서는 <mark>**화면의 상태에 따라 특정 메서드가 호출**</mark>됨

```text
viewDidLoad
↓
viewWillAppear
↓
viewDidAppear
↓
viewWillDisappear
↓
viewDidDisappear
```

### (0) `viewDidLoad`

화면이 메모리에 처음 로드될 때 한 번 호출됨

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    view.backgroundColor = .systemBackground
}
```

### (1) `viewWillAppear`

화면이 나타나기 직전에 호출

### (2) `viewDidAppear`

화면이 나타난 직후 호출

### (3) `viewWillDisappear`

화면이 사라지기 직전에 호출

### (4) `viewDidDisappear`

화면이 사라진 직후 호출

<br>
<br>

## 06 `UIResponder`
> `UIResponder`는 사용자 이벤트를 받을 수 있는 객체임

### 대표적인 이벤트 ⬇️⬇️
- 터치
- 키보드 입력
- 모션 이벤트

`UIView`와 `UIViewController`는 `UIResponder`를 상속받음

```swift
override func touchesBegan(
    _ touches: Set<UITouch>,
    with event: UIEvent?
) {
    print("화면이 터치됨")
}
```

<br>
<br>

## 07 View 계층 구조

> UIKit의 화면은 뷰를 부모와 자식 관계로 구성함

뷰 안에 다른 뷰를 추가하려면 `addSubview`를 사용

```swift
let label = UILabel()

view.addSubview(label)
```

- `view`: 부모 뷰
- `label`: 자식 뷰 또는 서브뷰

<br>
<br>

# 08 정리

- UIKit은 iOS 화면을 객체 기반으로 구성하는 프레임워크
- `UIView`는 UIKit UI 요소의 기본 클래스
- `UIViewController`는 하나의 화면을 관리함
- `UIResponder`는 터치와 키보드 같은 이벤트를 처리
- UIKit 화면은 부모 뷰와 자식 뷰의 계층 구조로 구성
- 화면 생명주기는 `UIViewController`의 메서드로 관리함