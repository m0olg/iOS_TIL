## 텍스트 필드

<br>

# 01 `UITextField`란?

`UITextField`는 사용자가 텍스트를 입력할 수 있도록 하는 UI 컴포넌트임

로그인 아이디, 비밀번호, 검색어, 이름 등을 입력받을 때 사용 가능함

```swift
let textField = UITextField()
```


<br>
<br>



# 02 `UITextField` 생성하기

```swift
let textField = UITextField()
```

`UITextField` 객체를 생성해 입력창을 만들 수 있음

```swift
let searchTextField = UITextField()
```

검색어를 입력받는 텍스트 필드 생성 가능함


<br>
<br>



# 03 입력창 테두리 설정하기

```swift
textField.borderStyle = .roundedRect
```

`borderStyle`을 사용해 텍스트 필드의 기본 테두리 스타일을 설정할 수 있음

| 값 | 설명 |
|---|---|
| `.none` | 테두리 표시하지 않음 |
| `.line` | 기본 선 형태로 표시함 |
| `.bezel` | 입체적인 테두리로 표시함 |
| `.roundedRect` | 모서리가 둥근 테두리로 표시함 |

일반적인 입력창을 만들 때는 `.roundedRect`를 주로 사용함


<br>
<br>



# 04 입력할 텍스트 설정하기

```swift
textField.text = "카페를 검색하세요."
```

`text`를 사용해 텍스트 필드에 기본으로 표시할 문자열을 설정할 수 있음

사용자가 입력한 값도 `text`를 통해 가져올 수 있음

```swift
let keyword = textField.text ?? ""
```

`textField.text`는 `String?` 타입이므로 값이 없을 때를 처리하기 위해 `??`를 사용할 수 있음


<br>
<br>



# 05 플레이스홀더 설정하기

```swift
textField.placeholder = "검색어를 입력하세요."
```

`placeholder`를 사용해 입력 전 안내 문구를 표시할 수 있음

사용자가 텍스트를 입력하면 플레이스홀더는 자동으로 사라짐

```swift
searchTextField.placeholder = "카페 이름을 입력하세요."
```

검색창이나 로그인 입력창에 입력할 내용을 안내할 때 사용 가능함


<br>
<br>



# 06 글자 색상과 크기 설정하기

```swift
textField.textColor = .black
textField.font = .systemFont(ofSize: 16)
```

`textColor`로 입력된 텍스트의 색상을 설정할 수 있음

`font`로 입력된 텍스트의 글자 크기와 글꼴을 설정 가능함

```swift
textField.textColor = .systemBlue
textField.font = .boldSystemFont(ofSize: 18)
```


<br>
<br>



# 07 입력창 배경색 설정하기

```swift
textField.backgroundColor = .systemGray6
```

`backgroundColor`를 사용해 텍스트 필드의 배경 색상을 설정할 수 있음

```swift
textField.backgroundColor = .white
```

입력창의 배경색을 흰색으로 설정 가능






# 08 입력 글자 수 제한하기

> `delegate`를 사용하면 입력 가능한 글자 수를 제한할 수 있음

```swift
class ViewController: UIViewController, UITextFieldDelegate {
    
    @IBOutlet weak var nameTextField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        nameTextField.delegate = self
    }
    
    func textField(
        _ textField: UITextField,
        shouldChangeCharactersIn range: NSRange,
        replacementString string: String
    ) -> Bool {
        
        let currentText = textField.text ?? ""
        let updatedText = currentText + string
        
        return updatedText.count <= 10
    }
}
```

위 코드는 최대 $10$글자까지만 입력 가능하도록 설



<br>
<br>


# 09 키보드 종류 설정하기

```swift
textField.keyboardType = .default
```

`keyboardType`을 사용해 입력할 때 표시되는 키보드 종류를 설정할 수 있음

| 값 | 설명 |
|---|---|
| `.default` | 기본 키보드 표시함 |
| `.numberPad` | 숫자 키보드 표시함 |
| `.decimalPad` | 소수 입력 키보드 표시함 |
| `.emailAddress` | 이메일 입력에 적합한 키보드 표시함 |
| `.phonePad` | 전화번호 입력에 적합한 키보드 표시함 |
| `.URL` | URL 입력에 적합한 키보드 표시함 |

```swift
textField.keyboardType = .numberPad
```

가격이나 전화번호처럼 숫자만 입력받을 때 사용 가능


<br>
<br>



# 10 비밀번호 입력 설정하기

```swift
passwordTextField.isSecureTextEntry = true
```

`isSecureTextEntry`를 `true`로 설정하면 입력한 글자가 숨겨짐

```swift
passwordTextField.placeholder = "비밀번호를 입력하세요."
passwordTextField.isSecureTextEntry = true
```

비밀번호 입력창을 만들 때 사용 가능함



<br>
<br>


# 11 키보드 리턴 버튼 설정하기


* `textField.returnKeyType = .done`


`returnKeyType`을 사용해 키보드의 리턴 버튼 이름을 설정할 수 있음

| 값 | 설명 |
|---|---|
| `.done` | 완료 버튼 표시함 |
| `.search` | 검색 버튼 표시함 |
| `.next` | 다음 버튼 표시함 |
| `.go` | 이동 버튼 표시함 |
| `.send` | 보내기 버튼 표시함 |

검색창에는 `.search`를 사용하는 것이 적절함

```swift
searchTextField.returnKeyType = .search
```






# 12 텍스트 정렬하기


* `textField.textAlignment = .center`

`textAlignment`를 사용해 입력 텍스트의 정렬 방향을 설정할 수 있음

```swift
textField.textAlignment = .left
textField.textAlignment = .right
```

| 값 | 설명 |
|---|---|
| `.left` | 왼쪽 정렬함 |
| `.center` | 가운데 정렬함 |
| `.right` | 오른쪽 정렬함 |
| `.natural` | 기본 방향에 맞춰 정렬함 |


<br>
<br>



# 13 텍스트 필드 이벤트 처리하기

텍스트 필드의 입력이 끝났을 때 실행할 동작을 설정할 수 있음

```swift
textField.addTarget(
    self,
    action: #selector(textFieldDidChange),
    for: .editingChanged
)
```

사용자가 입력할 때마다 메서드가 실행됨

```swift
@objc private func textFieldDidChange() {
    print("텍스트가 변경되었습니다.")
}
```

검색어가 입력되었는지 확인하거나 검색 버튼을 활성화할 때 사용 가능함



<br>
<br>


# 14 키보드 내리기

```swift
textField.resignFirstResponder()
```

현재 텍스트 필드가 사용 중인 키보드를 내릴 수 있음

```swift
override func touchesBegan(
    _ touches: Set<UITouch>,
    with event: UIEvent?
) {
    view.endEditing(true)
}
```

화면의 빈 공간을 눌렀을 때 키보드를 내리도록 설정 가능함



<br>
<br>


# 15 `UITextFieldDelegate` 사용하기

> `UITextFieldDelegate`를 사용하면 키보드 리턴 버튼을 눌렀을 때의 동작 등을 설정할 수 있음

```swift
class ViewController: UIViewController, UITextFieldDelegate {
    
    @IBOutlet weak var searchTextField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        searchTextField.delegate = self
    }
    
    func textFieldShouldReturn(
        _ textField: UITextField
    ) -> Bool {
        print("검색 실행")
        textField.resignFirstResponder()
        return true
    }
}
```

리턴 버튼을 눌렀을 때 검색을 실행하거나 키보드를 내릴 수 있음

<br>
<br>






# 16 코드로 `UITextField` 화면에 추가하기

```swift
import UIKit

class MainViewController: UIViewController {
    
    let searchTextField: UITextField = {
        let textField = UITextField()
        
        textField.placeholder = "카페 이름을 입력하세요."
        textField.borderStyle = .roundedRect
        textField.textColor = .black
        textField.backgroundColor = .systemGray6
        textField.keyboardType = .default
        textField.returnKeyType = .search
        
        return textField
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .white
        view.addSubview(searchTextField)
        
        searchTextField.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            searchTextField.centerXAnchor.constraint(
                equalTo: view.centerXAnchor
            ),
            searchTextField.centerYAnchor.constraint(
                equalTo: view.centerYAnchor
            ),
            searchTextField.leadingAnchor.constraint(
                equalTo: view.leadingAnchor,
                constant: 20
            ),
            searchTextField.trailingAnchor.constraint(
                equalTo: view.trailingAnchor,
                constant: -20
            ),
            searchTextField.heightAnchor.constraint(
                equalToConstant: 45
            )
        ])
    }
}
```

### 💻 코드 흐름

1. `UITextField` 생성함
2. 플레이스홀더 설정함
3. 테두리와 색상 설정함
4. 키보드 종류와 리턴 버튼 설정함
5. `view.addSubview()`로 화면에 추가함
6. 오토 레이아웃으로 위치와 크기 설정함


<br>
<br>



# 17 `UITextField`와 `UILabel`의 차이

| 구분 | `UITextField` | `UILabel` |
|---|---|---|
| 역할 | 사용자의 텍스트 입력을 받음 | 텍스트를 표시함 |
| 사용자 입력 | 가능함 | 기본적으로 불가능함 |
| 사용 예시 | 카페 이름 검색, 비밀번호 입력 | 카페 이름, 혼잡도 표시 |
| 키보드 | 입력 시 표시됨 | 표시되지 않음 |

사용자가 직접 글자를 입력해야 하면 `UITextField`를 사용하고 입력받은 내용을 보여주기만 하면 `UILabel`을 사용 !!


<br>
<br>



# 18 정리

- `UITextField`는 사용자가 텍스트를 입력할 수 있도록 하는 UI 컴포넌트
- `text`로 입력된 텍스트 확인 가능
- `placeholder`로 입력 전 안내 문구 설정 가능
- `borderStyle`로 입력창 테두리 설정 가능
- `textColor`와 `font`로 입력 텍스트 스타일 설정 가능
- `keyboardType`으로 키보드 종류 설정 가능함
- `isSecureTextEntry`로 비밀번호 입력 설정 가능
- `returnKeyType`으로 키보드 리턴 버튼 이름 설정 가능함
- `delegate`로 입력 글자 수 제한과 리턴 버튼 이벤트 처리 가능함
- `resignFirstResponder()`로 키보드 내리기 가능함
- 사용자가 텍스트를 입력해야 하는 경우 `UITextField` 사용

> ⇒ <mark>`UITextField`는 카페 이름이나 검색어 비밀번호처럼 사용자가 직접 텍스트를 입력할 수 있도록 하는 UI 컴포넌트 ✨