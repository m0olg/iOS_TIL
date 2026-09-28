## UIButton
<sup>네이티브 아이디어에 쓴 카페나 스터디 카페 찾는 걸로 예시 코드 작성해봤습니당

<br>

# 01 `UIButton`이란?

`UIButton`은 사용자의 터치 입력을 받아 특정 동작을 실행하는 UI 컴포넌트임

로그인, 다음 화면으로 이동, 데이터 저장, 메뉴 열기 등 다양한 기능을 실행할 때 사용 가능함

```swift
let button = UIButton()
```

<br>
<br>

# 02 `UIButton` 생성하기

```swift
let button = UIButton(type: .system)
```

`UIButton(type:)`을 사용해 버튼을 생성할 수 있음

` .system` 타입은 iOS 기본 버튼 스타일을 사용하는 방식임

```swift
let button = UIButton(type: .system)
```

버튼 타입은 다음과 같이 사용할 수 있음

| 타입 | 설명 |
|---|---|
| `.system` | 기본 시스템 버튼 생성함 |
| `.custom` | 기본 스타일 없이 버튼 생성함 |
| `.detailDisclosure` | 상세 정보 표시 버튼 생성함 |
| `.infoLight` | 밝은 정보 버튼 생성함 |
| `.infoDark` | 어두운 정보 버튼 생성함 |

일반적인 버튼을 만들 때는 주로 `.system`을 사용함

<br>
<br>

# 03 버튼 제목 설정하기

```swift
button.setTitle("다음", for: .normal)
```

`setTitle(_:for:)`를 사용해 버튼에 표시할 텍스트를 설정할 수 있음

`for: .normal`은 버튼이 기본 상태일 때 표시할 제목을 의미함

```swift
button.setTitle("카페 찾기", for: .normal)
```

공부 장소를 검색하거나 다음 화면으로 이동하는 버튼을 만들 때 사용 가능함

<br>
<br>

# 04 버튼 제목 색상 설정하기

```swift
button.setTitleColor(.systemBlue, for: .normal)
```

`setTitleColor(_:for:)`를 사용해 버튼 제목의 색상을 설정할 수 있음

```swift
button.setTitleColor(.systemBlue, for: .normal)
```

버튼 상태마다 다른 색상을 설정할 수도 있음

```swift
button.setTitleColor(.gray, for: .disabled)
```

버튼이 비활성화되었을 때 제목을 회색으로 표시 가능함

<br>
<br>

# 05 버튼 상태란?

버튼은 현재 상태에 따라 다른 제목, 색상, 이미지를 표시할 수 있음

| 상태 | 설명 |
|---|---|
| `.normal` | 기본 상태임 |
| `.highlighted` | 버튼을 누르고 있는 상태임 |
| `.selected` | 버튼이 선택된 상태임 |
| `.disabled` | 버튼을 사용할 수 없는 상태임 |


* `button.setTitle("검색", for: .normal)`
* `button.setTitle("검색 중", for: .highlighted)`
* `button.setTitle("선택됨", for: .selected)`
* `button.setTitle("사용 불가", for: .disabled)`

<br>
<br>

# 06 버튼 배경색 설정하기

```swift
button.backgroundColor = .systemBlue
```

`backgroundColor`를 사용해 버튼의 배경 색상을 설정할 수 있음

```swift
button.backgroundColor = .systemBlue
button.setTitleColor(.white, for: .normal)
```

파란색 배경과 흰색 텍스트를 가진 버튼을 만들 수 있음

<br>
<br>

# 07 버튼 모서리 설정하기

```swift
button.layer.cornerRadius = 10
```

`cornerRadius`를 사용해 버튼 모서리를 둥글게 설정할 수 있음

```swift
button.layer.cornerRadius = 10
button.clipsToBounds = true
```

스터디카페 검색이나 장소 추천 버튼을 둥근 모양으로 만들 때 사용 가능함

버튼을 완전히 둥근 모양으로 만들고 싶다면 버튼 높이의 절반을 설정함

```swift
button.layer.cornerRadius = 25
```

버튼의 높이가 $50$일 때 모서리 반지름을 $25$로 설정하면 둥근 버튼이 됨

<br>
<br>

# 08 버튼 이미지 설정하기

```swift
let image = UIImage(systemName: "magnifyingglass")

button.setImage(image, for: .normal)
```

`setImage(_:for:)`를 사용해 버튼에 이미지를 추가할 수 있음

카페 검색, 위치 확인, 설정 메뉴 등의 버튼을 만들 때 사용 가능함

```swift
button.setImage(
    UIImage(systemName: "mappin"),
    for: .normal
)
```

시스템 이미지 이름을 사용해 기본 아이콘을 표시할 수 있음

<br>
<br>

# 09 버튼 이벤트 연결하기

버튼을 눌렀을 때 특정 메서드가 실행되도록 이벤트를 연결할 수 있음

```swift
button.addTarget(
    self,
    action: #selector(didTapButton),
    for: .touchUpInside
)
```

`addTarget`을 사용해 버튼과 실행할 메서드를 연결함

` .touchUpInside`는 버튼을 눌렀다가 손가락을 뗐을 때 이벤트가 발생하는 것을 의미함

```swift
@objc private func didTapButton() {
    print("버튼이 눌렸습니다.")
}
```

<br>
<br>

# 10 버튼으로 화면 이동하기

```swift
class MainViewController: UIViewController {
    
    let nextButton = UIButton(type: .system)
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        nextButton.setTitle("공부 장소 찾기", for: .normal)
        
        nextButton.addTarget(
            self,
            action: #selector(didTapNextButton),
            for: .touchUpInside
        )
        
        view.addSubview(nextButton)
    }
    
    @objc private func didTapNextButton() {
        let nextViewController = NextViewController()
        
        navigationController?.pushViewController(
            nextViewController,
            animated: true
        )
    }
}
```

버튼을 누르면 `NextViewController` 화면으로 이동 가능함

<br>
<br>

# 11 버튼 활성화와 비활성화

```swift
button.isEnabled = false
```

`isEnabled`를 `false`로 설정하면 버튼을 비활성화할 수 있음

```swift
button.isEnabled = true
```

`isEnabled`를 `true`로 설정하면 버튼을 다시 활성화할 수 있음

예를 들어 검색어를 입력하지 않았을 때 검색 버튼을 비활성화할 수 있음

```swift
searchButton.isEnabled = false
```

검색어가 입력되면 버튼을 활성화할 수 있음

```swift
searchButton.isEnabled = true
```

<br>
<br>

# 12 버튼 숨기기

```swift
button.isHidden = true
```

버튼을 화면에서 숨길 수 있음

```swift
button.isHidden = false
```

숨겨진 버튼을 다시 표시 가능함

```swift
button.alpha = 0.5
```

버튼의 투명도를 조절할 수도 있음

<br>
<br>

# 13 코드로 버튼 화면에 추가하기

```swift
import UIKit

class MainViewController: UIViewController {
    
    let searchButton: UIButton = {
        let button = UIButton(type: .system)
        
        button.setTitle("카페 찾기", for: .normal)
        button.setTitleColor(.white, for: .normal)
        button.backgroundColor = .systemBlue
        button.layer.cornerRadius = 12
        
        return button
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .white
        
        view.addSubview(searchButton)
        
        searchButton.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            searchButton.centerXAnchor.constraint(
                equalTo: view.centerXAnchor
            ),
            searchButton.centerYAnchor.constraint(
                equalTo: view.centerYAnchor
            ),
            searchButton.widthAnchor.constraint(equalToConstant: 200),
            searchButton.heightAnchor.constraint(equalToConstant: 50)
        ])
        
        searchButton.addTarget(
            self,
            action: #selector(didTapSearchButton),
            for: .touchUpInside
        )
    }
    
    @objc private func didTapSearchButton() {
        print("카페 검색")
    }
}
```

### 💻 코드 흐름

1. `UIButton` 생성함
2. 버튼 제목과 색상 설정함
3. 버튼 모서리 설정함
4. `view.addSubview()`로 화면에 추가함
5. 오토 레이아웃으로 버튼 위치와 크기 설정함
6. `addTarget`으로 버튼 이벤트 연결함

<br>
<br>

# 14 `UIButton`과 `UILabel`의 차이

| 구분 | `UIButton` | `UILabel` |
|---|---|---|
| 역할 | 사용자의 동작을 실행함 | 텍스트를 표시함 |
| 터치 이벤트 | 처리 가능함 | 기본적으로 처리하지 않음 |
| 사용 예시 | 카페 검색, 다음 화면 이동 | 카페 이름, 가격 표시 |
| 주요 목적 | 사용자와 상호작용함 | 정보를 전달함 |

카페 이름이나 혼잡도를 보여줄 때는 `UILabel`을 사용하고, 카페를 검색하거나 상세 정보를 확인할 때는 `UIButton`을 사용함

<br>
<br>

# 15 정리

- `UIButton`은 사용자의 터치 입력을 받아 동작을 실행하는 UI 컴포넌트임
- `UIButton(type:)`으로 버튼 생성 가능함
- `setTitle()`로 버튼 제목 설정 가능함
- `setTitleColor()`로 버튼 제목 색상 설정 가능함
- `backgroundColor`로 버튼 배경색 설정 가능함
- `cornerRadius`로 버튼 모서리를 둥글게 설정 가능함
- `setImage()`로 버튼에 이미지 추가 가능함
- `addTarget()`으로 버튼 이벤트 연결 가능함
- `isEnabled`로 버튼 활성화 여부 설정 가능함
- `isHidden`으로 버튼을 숨기거나 표시 가능함
- `UIButton`은 사용자의 동작을 실행할 때 사용함

> ### ⇒ `UIButton`은 사용자의 터치 입력을 받아 화면 이동, 검색, 저장, 메뉴 열기 등의 동작을 실행하는 UI 컴포넌트 ✨