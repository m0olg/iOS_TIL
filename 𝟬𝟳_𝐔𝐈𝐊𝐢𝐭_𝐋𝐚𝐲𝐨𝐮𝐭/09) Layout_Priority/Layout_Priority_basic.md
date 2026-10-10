## 레이아웃 우선순위

<br>

# 01 Layout Priority란?

→ <mark>Layout Priority는 여러 제약 조건이 동시에 만족되기 어려울 때 어떤 제약 조건을 우선</mark>할지 정하는 값

Auto Layout은 제약 조건을 가능한 한 만족시키려고 함

<br>
<br>

# 02 우선순위의 범위

제약 조건의 우선순위는 `1`부터 `1000` 사이의 값으로 설정함

- 우선순위가 높을수록 제약 조건이 더 중요하게 적용됨
- 우선순위가 낮은 제약 조건은 다른 제약 조건과 충돌할 때 무시될 수 있음
- `1000`은 필수 제약 조건을 의미함

<br>
<br>

# 03 우선순위 설정하기

제약 조건을 만들 때 우선순위를 설정할 수 있음

```swift
let widthConstraint = boxView.widthAnchor.constraint(
    equalToConstant: 200
)

widthConstraint.priority = UILayoutPriority(750)
widthConstraint.isActive = true
```

`UILayoutPriority`를 사용해 우선순위 값을 지정

<br>
<br>

# 04 우선순위가 다른 제약 조건

아래 예시에서 너비 `200`은 우선순위가 `750`인 제약 조건

더 높은 우선순위의 제약 조건과 충돌하면 너비가 `200`이 아닐 수도 있음

```swift
let widthConstraint = boxView.widthAnchor.constraint(
    equalToConstant: 200
)

widthConstraint.priority = UILayoutPriority(750)
```

반드시 지켜야 하는 조건이라면 기본 우선순위인 `.required`를 사용할 수 있음 !

```swift
widthConstraint.priority = .required
```

<br>
<br>

# 05 정리

- Layout Priority는 제약 조건의 중요도를 나타냄
- 우선순위는 `1`부터 `1000` 사이의 값으로 설정
- 우선순위가 높을수록 제약 조건이 우선 적용
- `1000` 또는 `.required`는 필수 제약 조건을 의미
- 우선순위가 낮은 제약 조건은 더 높은 우선순위의 조건과 충돌할 때 적용되지 않을 수 있음