## 안전 영역

<br>

# 01 Safe Area란?

→ Safe Area는 UI가 노치, 상태 표시줄, 홈 인디케이터 같은 화면 요소에 가려지지 않도록 배치할 때 기준으로 삼는 영역 !

<br>
<br>

# 02 Safe Area 사용하기

`safeAreaLayoutGuide`를 기준으로 제약 조건을 설정하면 안전 영역 안에 뷰를 배치할 수 있음

```swift
let titleLabel = UILabel()
titleLabel.translatesAutoresizingMaskIntoConstraints = false
view.addSubview(titleLabel)

NSLayoutConstraint.activate([
    titleLabel.topAnchor.constraint(
        equalTo: view.safeAreaLayoutGuide.topAnchor
    ),
    titleLabel.leadingAnchor.constraint(
        equalTo: view.safeAreaLayoutGuide.leadingAnchor
    )
])
```

- `safeAreaLayoutGuide.topAnchor`: 안전 영역의 위쪽
- `safeAreaLayoutGuide.leadingAnchor`: 안전 영역의 시작 가장자리

<br>
<br>

# 03 Safe Area와 View의 차이