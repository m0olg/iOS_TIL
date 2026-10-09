## 콘텐츠 고유 크기

<br>

# 01 Compression Resistance Priority란?

→ Compression Resistance Priority는 뷰가 콘텐츠 크기보다 **작아지는 것을 얼마나 피하려는지** 나타내는 우선순위

공간이 부족할 때 어떤 뷰의 크기가 먼저 줄어들지 결정하는 데 영향을 줌

<br>
<br>

# 02 우선순위에 따른 차이

- 우선순위가 높은 뷰: 콘텐츠 크기보다 작아지는 것을 더 강하게 피함
- 우선순위가 낮은 뷰: 공간이 부족할 때 더 쉽게 작아질 수 있음

```text
우선순위가 높은 뷰 → 작아지는 것을 더 강하게 피함
우선순위가 낮은 뷰 → 공간이 부족하면 더 쉽게 작아짐
```

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


> Content Hugging → 뷰가 콘텐츠보다 커지는 것을 피함 \
> Compression Resistance → 뷰가 콘텐츠보다 작아지는 것을 피함

<br>
<br>

# 05 정리

- Compression Resistance Priority는 뷰가 콘텐츠 크기보다 작아지는 것을 피하려는 정도를 나타냄
- 우선순위가 높을수록 크기가 줄어드는 것을 더 강하게 거부함
- 공간이 부족할 때 어떤 뷰가 먼저 작아질지에 영향을 줌
- 가로와 세로 방향별로 설정할 수 있음
