## 콘텐츠 허깅 우선순위

<br>

# 01 Content Hugging Priority란?

→ <mark>Content Hugging Priority는 뷰가 콘텐츠 크기보다 커지는 것을 얼마나 피하려는지 나타내는 우선순위

<br>
<br>

# 02 기본 개념

Content Hugging Priority가 높은 뷰는 자신의 콘텐츠 크기를 유지하려는 성향이 강함

Content Hugging Priority가 낮은 뷰는 남는 공간을 더 차지하기 쉬움


> 우선순위가 높은 뷰 → 콘텐츠 크기보다 커지는 것을 더 피함 \
>우선순위가 낮은 뷰 → 남는 공간을 더 차지하기 쉬움

<br>
<br>

# 03 우선순위 설정하기

가로 방향의 Content Hugging Priority를 설정하는 예시

```swift
label.setContentHuggingPriority(
    .defaultHigh,
    for: .horizontal
)
```

- `.defaultHigh`: 기본 높은 우선순위
- `.horizontal`: 가로 방향
- `.vertical`: 세로 방향

숫자로 우선순위를 직접 지정할 수도 있움

```swift
label.setContentHuggingPriority(
    UILayoutPriority(251),
    for: .horizontal
)
```

<br>
<br>

# 04 Compression Resistance와의 차이

| 우선순위 | 뷰가 피하려는 상황 |
|:---|:---|
| Content Hugging | 콘텐츠 크기보다 커지는 것 |
| Compression Resistance | 콘텐츠 크기보다 작아지는 것 |

<br>
<br>

# 05 정리

- Content Hugging Priority는 뷰가 콘텐츠보다 커지는 것을 피하려는 정도를 나타냄
- 우선순위가 높을수록 콘텐츠 크기를 유지하려는 성향이 강함
- 여러 뷰 사이에서 남는 공간을 어떻게 나눌지에 영향을 줌
- 가로와 세로 방향별로 우선순위를 설정할 수 있음