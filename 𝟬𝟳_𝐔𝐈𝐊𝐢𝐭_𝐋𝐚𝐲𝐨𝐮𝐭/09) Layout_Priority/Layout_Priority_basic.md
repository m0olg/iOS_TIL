## 콘텐츠 고유 크기

<br>

# 01 Intrinsic Content Size란?

→ Intrinsic Content Size는 뷰가 가진 콘텐츠를 표시하기 위해 필요한 기본 크기

예를 들어 `UILabel`은 글자와 글꼴에 따라 필요한 너비와 높이가 정해짐

<br>
<br>

# 02 기본 크기가 있는 뷰

일부 UIKit 뷰는 콘텐츠를 기준으로 기본 크기를 가짐

예를 들어 `UILabel`은 표시할 문자열과 글꼴을 바탕으로 크기가 계산됨

```swift
let label = UILabel()
label.text = "ㅎㅇ"
label.font = .systemFont(ofSize: 20)
```

이 레이블의 고유 크기는 텍스트와 글꼴에 따라 달라짐

<br>
<br>

# 03 Auto Layout에서의 역할

Auto Layout은 제약 조건과 Intrinsic Content Size를 이용해 뷰의 크기를 계산

너비와 높이를 별도로 지정하지 않아도 콘텐츠에 맞는 크기로 표시될 수 있음

```swift
label.translatesAutoresizingMaskIntoConstraints = false
label.text = "ㅎㅇ"
label.font = .systemFont(ofSize: 20)

NSLayoutConstraint.activate([
    label.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20),
    label.topAnchor.constraint(equalTo: view.topAnchor, constant: 100)
])
```

위 예시에서는 위치만 제약 조건으로 설정하고, 레이블의 크기는 콘텐츠를 기준으로 정해짐

<br>
<br>