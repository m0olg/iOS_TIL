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

# 05 