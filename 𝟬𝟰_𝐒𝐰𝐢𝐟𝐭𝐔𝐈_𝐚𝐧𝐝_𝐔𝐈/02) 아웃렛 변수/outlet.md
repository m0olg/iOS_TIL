## 아웃렛 변수

<br>

# 01 아웃렛 변수란?
→ 아웃렛 변수는 **스토리보드에 배치한 UI 객체(버튼, 레이블, 텍스트 필드 등)를 코드에서 접근하고 제어**할 수 있도록 연결해 주는 변수

```swift
@IBOutlet var lblHello : UILable!
```
* `@IBOutlet`으로 정의된 변수를 아울렛 변수라고 함
* IB는 인터페이스 빌더의 약자로 @IB로 시작되는 변수나 함수는 인터페이스 빌더와 관련된 변수나 함수라는 것을 의미
* 객체를 소스코드에서 참조하기 위해 사용하는 키워드이며 주로 색상, 크기, 모양 선의 두께, 텍스트 내용 등 객체의 속석을 제어하는 데 사용

<br>
<br>


# 02 선언하는 방법
* 변수를 선언할 때는 var 키워드를 사용
* 변수를 선언하는 var 뒤에 아웃렛 변수의 입력하여 변수를 선언

<br>
<br>

# 03 `UILable!`
* 선언하고자 하는 변수의 타입을 나타냄
* UILable은 레이블 객체에 대한 변수를 선언하는 것으로 UILabel 클래스 타입을 선택

<br>
<br>

# 04 아웃렛 변수 추가 & 연결 설정
텍스트 필드와 라벨 모듀 동일, 뷰 컨트롤러 클래스 선언문 아래에 드래그앤드롭 해주면 됨 \
설정은 `[Type : UITextField / Storage : Strong]`

<br>
<br>

# 05 아웃렛 변수 사용 방법

스토리보드의 UI 객체를 아웃렛 변수로 연결하면 코드에서 해당 객체의 속성을 변경할 수 있음

```swift
@IBOutlet var lblHello: UILabel!
```

이제 `lblHello`를 사용해 레이블의 텍스트와 색상을 변경할 수 있음

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    lblHello.text = "시사"
    lblHello.textColor = .systemBlue
    lblHello.font = .boldSystemFont(ofSize: 7777777)
}
```

아웃렛 변수로 다음과 같은 속성을 제어 가능함

- 텍스트 내용
- 텍스트 색상
- 글자 크기
- 배경 색상
- 테두리
- 모서리 둥글기
- 숨김 여부
- 활성화 여부

<br>
<br>

# 06 아웃렛 변수 연결 시 주의할 점

### ① 타입 이름 확인

`UILable`이 아니라 `UILabel`이 올바른 클래스 이름임

```swift
@IBOutlet var lblHello: UILabel!
```

`UILabel`처럼 대문자와 철자를 정확하게 작성해야 함

---

### ② 연결 대상 확인

아웃렛 변수의 타입과 스토리보드 객체의 타입이 일치해야 함

```swift
@IBOutlet var nameLabel: UILabel!
```

위 변수는 `UILabel` 객체와 연결해야 함

```swift
@IBOutlet var inputTextField: UITextField!
```

위 변수는 `UITextField` 객체와 연결해야 함

---

### ③ 연결하지 않은 상태에서 사용하지 않기

아웃렛 변수가 스토리보드와 연결되지 않은 상태에서 사용하면 오류가 발생할 수 있음

```swift
lblHello.text = "시사"
```

<mark>스토리보드와 연결되지 않았다면 `lblHello`가 `nil`인 상태일 수 있음

```text
Unexpectedly found nil while implicitly unwrapping an Optional value
```

---

### ④ 연결이 끊어진 경우

스토리보드의 UI 객체를 삭제하거나 이름을 변경하면 아웃렛 연결이 끊어질 수 있음

#### 연결이 끊어진 아웃렛은 연결 목록에서 삭제한 후 다시 연결해야 함 ‼️
1. 연결이 끊어진 아웃렛 삭제함
2. 스토리보드에서 UI 객체 선택함
3. 뷰 컨트롤러 코드로 다시 드래그함
4. 새로운 아웃렛 변수 생성함

<br>
<br>

# 07 아웃렛 변수와 생명주기

아웃렛 변수는 스토리보드의 화면이 메모리에 올라온 이후에 사용할 수 있음

따라서 보통 `viewDidLoad()` 이후에 아웃렛 변수를 사용함

```swift
class ViewController: UIViewController {
    
    @IBOutlet var lblHello: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        lblHello.text = "시사"
    }
}
```

`viewDidLoad()`는 뷰가 메모리에 올라온 직후 호출됨

이 시점에는 스토리보드의 UI 객체와 아웃렛 연결이 완료되어 있으므로 UI 설정이 가능함

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    lblHello.text = "시사자격증화이팅넌할수잇어귀요미시사"
    lblHello.textColor = .systemPink
    lblHello.textAlignment = .center
}
```

<br>
<br>

# 08 아웃렛 변수의 `Strong`과 `Weak`

> 아웃렛 변수의 저장 방식은 `Strong` 또는 `Weak`로 설정 가능함

### (1) `Strong`

```swift
@IBOutlet var lblHello: UILabel!
```

`Strong`은 뷰 객체를 강하게 참조함

스토리보드에서 연결할 때 기본 저장 방식으로 자주 사용함

### (2) `Weak`

```swift
@IBOutlet weak var lblHello: UILabel!
```

`Weak`는 뷰 객체를 약하게 참조함

뷰 계층이 해당 객체를 유지하고 있으므로 뷰가 제거되면 아웃렛 변수도 자동으로 `nil`이 될 수 있음

일반적인 화면에서는 다음과 같이 `weak`를 사용하는 경우도 많음

```swift
@IBOutlet weak var lblHello: UILabel!
```

<br>
<br>

# 09 정리

- 아웃렛 변수는 스토리보드의 UI 객체를 코드와 연결하는 변수임
- `@IBOutlet` 키워드를 사용해 선언함
- `UILabel`, `UIButton`, `UITextField` 등의 UI 객체와 연결 가능함
- 연결된 객체의 텍스트, 색상, 크기, 모양 등을 코드에서 변경 가능함
- 보통 `viewDidLoad()` 이후에 사용함
- 스토리보드 객체와 변수의 타입이 일치해야 함
- 연결이 끊어지거나 타입이 잘못되면 오류가 발생할 수 있음
- `Strong`과 `Weak` 방식으로 객체 참조를 설정 가능함

> ### ⇒ 아웃렛 변수는 스토리보드에 배치한 UI 객체를 코드에서 접근하고 제어하기 위해 사용하는 연결 변수임 ✨
