## 콘텐츠 압축 저항 우선순위

<br>

# 01 Compression Resistance Priority란?

→ <mark>Compression Resistance Priority는 뷰가 콘텐츠 크기보다 **작아지는 것을 얼마나 피하려는지** 나타내는 우선순위

공간이 부족할 때 어떤 뷰의 크기가 먼저 줄어들지 결정하는 데 영향을 줌

<br>
<br>

# 02 우선순위에 따른 차이

- 우선순위가 높은 뷰: 콘텐츠 크기보다 작아지는 것을 더 강하게 피함
- 우선순위가 낮은 뷰: 공간이 부족할 때 더 쉽게 작아질 수 있음


> 우선순위가 높은 뷰 → 작아지는 것을 더 강하게 피함 /
> 우선순위가 낮은 뷰 → 공간이 부족하면 더 쉽게 작아짐

<br>
<br>

# 03 Compression Resistance Priority 설정하기

가로 방향의 우선순위를 설정하는 예시

```swift
label.setContentCompressionResistancePriority(
    .defaultHigh,
    for: .horizontal
)
```

- `.horizontal`: 가로 방향
- `.vertical`: 세로 방향
- `.defaultHigh`: 기본적으로 높은 우선순위 값

숫자로 우선순위를 직접 설정할 수도 있음

```swift
label.setContentCompressionResistancePriority(
    UILayoutPriority(751),
    for: .horizontal
)
```

<br>
<br>

# 04 Content Hugging과의 차이

두 우선순위는 반대되는 상황에서 작동함

```text
Content Hugging → 뷰가 콘텐츠보다 커지는 것을 피함
Compression Resistance → 뷰가 콘텐츠보다 작아지는 것을 피함
```

<br>
<br>

# 05 정리

- Compression Resistance Priority는 뷰가 콘텐츠 크기보다 작아지는 것을 피하려는 정도를 나타냄
- 우선순위가 높을수록 크기가 줄어드는 것을 더 강하게 거부함
- 공간이 부족할 때 어떤 뷰가 먼저 작아질지에 영향을 줌
- 가로와 세로 방향별로 설정할 수 있음
복사
답변 신고
첨부한 이미지 1
아이거먼저해줘
오후 07:29
Intrinsic_Content_Size_basic.md
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
label.text = "안녕하세요"
label.font = .systemFont(ofSize: 20)
```

이 레이블의 고유 크기는 텍스트와 글꼴에 따라 달라짐

<br>
<br>

# 03 Auto Layout에서의 역할

Auto Layout은 제약 조건과 Intrinsic Content Size를 이용해 뷰의 크기를 계산함

너비와 높이를 별도로 지정하지 않아도 콘텐츠에 맞는 크기로 표시될 수 있음

```swift
label.translatesAutoresizingMaskIntoConstraints = false
label.text = "안녕하세요"
label.font = .systemFont(ofSize: 20)

NSLayoutConstraint.activate([
    label.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20),
    label.topAnchor.constraint(equalTo: view.topAnchor, constant: 100)
])
```

위 예시에서는 위치만 제약 조건으로 설정하고, 레이블의 크기는 콘텐츠를 기준으로 정해짐

<br>
<br>