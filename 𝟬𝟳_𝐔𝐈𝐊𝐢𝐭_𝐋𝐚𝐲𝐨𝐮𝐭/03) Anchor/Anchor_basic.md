## 앵커?

<br>

# 01 Anchor란?

→ Anchor는 Auto Layout 제약 조건을 코드로 표현하는 방법

> 앵커를 사용하면 뷰의 위치, 크기, 중심을 다른 뷰와의 관계로 설정할 수 있음

<br>
<br>

# 02 주요 Anchor


| Anchor | 의미 |
| --- | :---: |
| `leadingAnchor` | 읽는 방향 기준 시작 가장자리 (왼쪽) |
| `trailingAnchor` | 읽는 방향 기준 끝 가장자리 (오른쪽) |
| `topAnchor` | 위쪽 가장자리 |
| `bottomAnchor` | 아래쪽 가장자리 |
| `centerXAnchor` | 가로 중심 |
| `centerYAnchor` | 세로 중심 |
| `widthAnchor` | 너비 |
| `heightAnchor` | 높이 |

<br>
<br>

# 03 Anchor로 뷰 배치하기

아래 코드는 `boxView`를 화면 가운데에 배치하고 크기를 설정함

```swift
let boxView = UIView()
boxView.translatesAutoresizingMaskIntoConstraints = false
view.addSubview(boxView)

NSLayoutConstraint.activate([
    boxView.centerXAnchor.constraint(equalTo: view.centerXAnchor),
    boxView.centerYAnchor.constraint(equalTo: view.centerYAnchor),
    boxView.widthAnchor.constraint(equalToConstant: 200),
    boxView.heightAnchor.constraint(equalToConstant: 100)
])
```

- `centerXAnchor`: 가로 중심을 맞춤
- `centerYAnchor`: 세로 중심을 맞춤
- `widthAnchor`: 너비를 설정
- `heightAnchor`: 높이를 설정

<br>
<br>

# 04 Anchor 사이의 간격 설정하기

`constant`를 사용해 Anchor 사이의 간격을 설정

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

이 경우 `boxView`는 부모 뷰의 왼쪽에서 `20`, 위쪽에서 `100`만큼 떨어져 배치

<br>
<br>

# 05 정리

- Anchor는 Auto Layout 제약 조건을 코드로 표현하는 방법
- 위치, 중심, 너비, 높이를 나타내는 Anchor가 있음
- `constant`로 Anchor 사이의 간격을 설정함
- 코드로 제약 조건을 설정할 땐 `translatesAutoresizingMaskIntoConstraints`를 `false`로 설정