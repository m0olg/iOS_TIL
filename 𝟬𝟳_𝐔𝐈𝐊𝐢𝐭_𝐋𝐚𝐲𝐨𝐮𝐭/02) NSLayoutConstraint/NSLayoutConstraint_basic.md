## 레이아웃 제약 조건


<br>

# 01 NSLayoutConstraint란?

→ <mark>`NSLayoutConstraint`는 뷰의 위치와 크기 관계를 표현하는 제약 조건

> 뷰의 너비, 높이, 다른 뷰와의 간격 등을 설정할 수 있음

<br>
<br>

# 02 제약 조건 설정하기

아래 코드는 `boxView`의 너비를 `200`, 높이를 `100`으로 설정함

```swift
let boxView = UIView()
boxView.translatesAutoresizingMaskIntoConstraints = false
view.addSubview(boxView)

NSLayoutConstraint.activate([
    boxView.widthAnchor.constraint(equalToConstant: 200),
    boxView.heightAnchor.constraint(equalToConstant: 100)
])
```

- `widthAnchor`: 너비 제약 조건
- `heightAnchor`: 높이 제약 조건
- `equalToConstant`: 고정된 값으로 크기를 설정

<br>
<br>

# 03 다른 뷰와의 관계 설정하기

아래 코드는 `boxView`를 부모 뷰의 왼쪽에서 `20`, 위쪽에서 `100`만큼 떨어뜨림

```swift
NSLayoutConstraint.activate([
    boxView.leadingAnchor.constraint(
        equalTo: view.leadingAnchor,
        constant: 20
    ),
    boxView.topAnchor.constraint(
        equalTo: view.topAnchor,
        constant: 100
    )
])
```

- `equalTo`: 다른 뷰의 Anchor와 관계를 설정
- `constant`: 두 Anchor 사이의 간격

<br>
<br>

# 04 제약 조건 활성화하기

제약 조건을 만들고 활성화해야 Auto Layout에 적용

```swift
let widthConstraint = boxView.widthAnchor.constraint(
    equalToConstant: 200
)

widthConstraint.isActive = true
```

여러 제약 조건은 한 번에 활성화할 수 있음

```swift
NSLayoutConstraint.activate([
    boxView.widthAnchor.constraint(equalToConstant: 200),
    boxView.heightAnchor.constraint(equalToConstant: 100)
])
```

<br>
<br>

# 05 정리

- `NSLayoutConstraint`는 뷰의 위치와 크기 관계를 나타냄
- `equalToConstant`로 크기를 고정된 값으로 설정가능
- `constant`로 Anchor 사이의 간격을 설정 가능
- 제약 조건은 활성화해야 적용