## 스택 뷰

<br>

# 01 UIStackView란?

→ <mark>`UIStackView`는 여러 뷰를 가로 또는 세로 방향으로 배열하는 UIKit 컨테이너 뷰

안에 넣은 뷰들의 배열 방향과 간격을 관리

<br>
<br>

# 02 Stack View 만들기

`axis`로 뷰가 배열될 방향을 설정

```swift
let stackView = UIStackView()

stackView.axis = .vertical
stackView.spacing = 12
stackView.addArrangedSubview(UILabel())
stackView.addArrangedSubview(UIButton())
```

- `.vertical`: 세로 방향으로 배열
- `.horizontal`: 가로 방향으로 배열
- `spacing`: 뷰 사이의 간격

<br>
<br>

# 03 주요 설정

| 속성 | 역할 |
|:---|:---:|
| `axis` | 뷰를 배열할 방향 |
| `spacing` | 뷰 사이의 간격 |
| `alignment` | Stack View의 축에 수직인 방향으로 정렬 |
| `distribution` | Stack View의 축 방향으로 크기를 배분 |

<br>
<br>

# 04 `addArrangedSubview`와 `addSubview`

`addArrangedSubview`는 뷰를 Stack View의 배열 대상에 추가함

```swift
stackView.addArrangedSubview(titleLabel)
```

`addSubview`는 뷰를 일반적인 하위 뷰로 추가

```swift
stackView.addSubview(titleLabel)
```

Stack View의 배열과 간격 관리를 받으려면 `addArrangedSubview`를 사용

<br>
<br>

# 05 정리

- `UIStackView`는 여러 뷰를 가로 또는 세로로 배열함
- `axis`로 배열 방향을 설정함
- `spacing`으로 뷰 사이의 간격을 설정함
- 배열 대상에 뷰를 추가할 때는 `addArrangedSubview`를 사용함