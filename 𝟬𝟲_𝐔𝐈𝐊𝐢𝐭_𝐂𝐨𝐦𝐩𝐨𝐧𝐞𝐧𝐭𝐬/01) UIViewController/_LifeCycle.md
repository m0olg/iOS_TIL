## 뷰컨트롤러

<br>

# 01 `UIViewController`

→ `UIViewController`는 iOS 앱에서 **하나의 화면을 관리하는 객체**

화면에 보이는 뷰를 관리하고 사용자의 입력에 반응하며, 화면 전환과 생명주기를 처리

```swift
import UIKit

class MainViewController: UIViewController {

}
```

#### ⬆️⬆️ 위 코드는 `UIViewController`를 상속받은 새로운 화면을 만드는 코드

<br>
<br>

# 02 화면 구성

`UIViewController`는 기본적으로 `view`라는 화면을 가지고 있음

```swift
class MainViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .white
    }
}
```

```swift
let viewController = MainViewController()
```

`view.backgroundColor`를 이용해 화면의 배경색을 설정할 수 있음

<br>
<br>

# 03 `UIViewController`의 주요 역할

### 🍀`UIViewController`는 주로 다음과 같은 역할을 함
- 화면의 UI 구성
- 버튼, 레이블 같은 뷰 관리
- 사용자 입력 처리
- 데이터 표시
- 화면 전환
- 화면 생명주기 관리

단, 너무 많은 기능을 하나의 `UIViewController`에 넣으면 코드가 복잡해질 수 있음 ㅠㅅㅠ

<br>
<br>

# 04 기본적인 화면 구성

```swift
import UIKit

class MainViewController: UIViewController {

    let titleLabel: UILabel = {
        let label = UILabel()
        label.text = "메인 화면"
        label.textColor = .black
        label.font = .boldSystemFont(ofSize: 어쩌구)
        return label
    }()

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .white

        view.addSubview(titleLabel)

        titleLabel.translatesAutoresizingMaskIntoConstraints = false

        NSLayoutConstraint.activate([
            titleLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            titleLabel.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }
}
```

### 💻 코드 흐름
1. `UILabel`을 생성
2. 레이블의 텍스트와 스타일을 설정
3. `viewDidLoad()`에서 화면에 추가
4. 오토 레이아웃으로 위치를 설정

<br>
<br>

# 05 `UIViewController` 생명주기

화면은 생성되고, 나타나고, 사라지는 과정을 거침) (예전에 멘토링할 때 쓴 벨로그인데 좀 내리다보면 생명주기 잇음 비슷해서 참고하면 좋을 것 같음)

```swift
class MainViewController: UIViewController {

    override func loadView() {
        super.loadView()
        print("View가 메모리에 로딩되는 단계")
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        print("View가 메모리에 로딩된 직후")
    }

    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        print("화면이 나타나기 직전")
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        print("화면이 나타난 직후")
    }

    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        print("화면이 사라지기 직전")
    }

    override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(animated)
        print("화면이 사라진 직후")
    }

    deinit {
        print("ViewController가 메모리에서 해제")
    }
}
```

### 🍀 주요 생명주기 메서드

| 메서드 | 호출 시점 | 주로 사용하는 작업 |
| --- | --- | --- |
| `loadView()` | View를 직접 생성할 때 | 코드로 View 생성 |
| `viewDidLoad()` | View가 메모리에 로드된 직후 | 초기 UI 설정 |
| `viewWillAppear()` | 화면이 나타나기 직전 | 최신 데이터 갱신 |
| ⭐⭐`viewDidAppear()` | 화면이 나타난 직후 | 애니메이션, 화면 노출 처리 |
| `viewWillDisappear()` | 화면이 사라지기 직전 | 입력 종료, 상태 저장 |
| `viewDidDisappear()` | 화면이 사라진 직후 | 작업 정리 |

# 06 `viewDidLoad()`에서 하는 일

`viewDidLoad()`는 **화면이 메모리에 올라온 직후 한 번 호출**됨

보통 다음 작업함

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    configureUI()
    configureLayout()
    bind()
}
```

각 기능을 메서드로 분리하면 코드가 읽기 쉬워짐

```swift
private func configureUI() {
    view.backgroundColor = .white
}

private func configureLayout() {
    view.addSubview(titleLabel)
}

private func bind() {
    // 버튼 이벤트나 데이터 연결
}
```

# 07 화면 전환

### ①`present`

현재 화면 위에 새로운 화면을 띄움

```swift
let nextViewController = NextViewController()

present(
    nextViewController,
    animated: true
)
```

주로 모달 화면이나 팝업을 띄울 때 사용

### ②`dismiss`

`present`로 띄운 화면을 닫음

```swift
dismiss(animated: true)
```

### ③ `UINavigationController`의 `push`

다음 화면으로 이동

```swift
let nextViewController = NextViewController()

navigationController?.pushViewController(
    nextViewController,
    animated: true
)
```

뒤로 가고 싶을 때는 다음과 같이 작성

```swift
navigationController?.popViewController(animated: true)
```

### 🍀 화면 전환 방식 비교

| 메서드 | 용도 |
| --- | --- |
| `present` | 화면 위에 새로운 화면을 띄움 |
| `dismiss` | `present`된 화면을 닫음 |
| `pushViewController` | 내비게이션 스택에 화면을 추가함 |
| `popViewController` | 이전 화면으로 돌아감 |

# 08 버튼 이벤트 처리

```swift
class MainViewController: UIViewController {

    private let nextButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("다음 화면", for: .normal)
        return button
    }()

    override func viewDidLoad() {
        super.viewDidLoad()

        view.addSubview(nextButton)
        nextButton.addTarget(
            self,
            action: #selector(didTapNextButton),
            for: .touchUpInside
        )
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

`addTarget`을 사용하면 버튼을 눌렀을 때 특정 메서드를 실행할 수 있음

# 09 `UIViewController`와 `UIView`의 차이

| 구분 | `UIView` | `UIViewController` |
| --- | --- | --- |
| 역할 | 화면의 개별 UI 요소 | 하나의 화면 관리 |
| 예시 | 버튼, 레이블, 이미지 뷰 | 로그인 화면, 홈 화면 |
| 담당 | 크기, 위치, 모양 | 생명주기, 전환, 데이터 처리 |
| 관계 | 화면을 구성하는 요소 | View들을 관리하는 객체 |

간단히 말하면 다음과 같음 ⬇️⬇️

> **`UIView`는 화면을 구성하고, `UIViewController`는 화면 전체를 관리함 !!!!!**
> 

# 10 정리

- `UIViewController`는 iOS 앱의 하나의 화면을 관리
- `viewDidLoad()`에서 초기 UI와 레이아웃을 설정
- `viewWillAppear()`와 `viewDidAppear()`는 화면이 나타날 때 호출
- `present`와 `dismiss`는 모달 화면 전환에 사용
- `push`와 `pop`은 내비게이션 기반 화면 전환에 사용
- `UIView`는 화면의 구성 요소이고, `UIViewController`는 화면과 생명주기를 관리하는 객체

> ### ⇒ `UIViewController`는 화면의 UI, 생명주기, 사용자 입력, 화면 전환을 관리하는 iOS의 핵심 클래스 ✨
>