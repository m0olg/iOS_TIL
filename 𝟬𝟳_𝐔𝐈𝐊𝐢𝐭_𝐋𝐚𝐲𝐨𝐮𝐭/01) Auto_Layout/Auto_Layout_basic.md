## 오토 레이아웃

<br>

# 01 Auto Layout이란?

→ Auto Layout은 뷰의 위치와 크기를 **제약 조건**으로 정하는 UIKit의 레이아웃 시스템

화면 크기나 기기 방향이 달라져도 뷰 사이의 관계를 기준으로 위치와 크기를 계산함

<br>
<br>

# 02 Frame 방식과의 차이

### ① Frame 방식

뷰의 위치와 크기를 직접 설정함

```swift
let boxView = UIView()
boxView.frame = CGRect(x: 20, y: 100, width: 200, height: 100)
```

- `x`, `y`: 뷰의 위치
- `width`, `height`: 뷰의 크기

화면 크기가 달라지면 원하는 위치에 표시되지 않을 수 있음

### ② Auto Layout 방식

뷰와 다른 뷰 또는 부모 뷰 사이의 관계를 설정함

```swift
boxView.translatesAutoresizingMaskIntoConstraints = false

NSLayoutConstraint.activate([
    boxView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20),
    boxView.topAnchor.constraint(equalTo: view.topAnchor, constant: 100),
    boxView.widthAnchor.constraint(equalToConstant: 200),
    boxView.heightAnchor.constraint(equalToConstant: 100)
])
```

<br>
<br>

