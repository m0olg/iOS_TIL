# 01 `UILabel`이란?

`UILabel`은 화면에 **텍스트를 표시하는 UI 컴포넌트**임

먼작귀 캐릭터 이름, 대사, 상태 메시지 등을 화면에 표시할 때 사용 가능함

```swift
let label = UILabel()
```

<br>
<br>

# 02 기본 텍스트 표시

```swift
let characterLabel = UILabel()

characterLabel.text = "치이카와"
```

`text` 프로퍼티를 사용해 라벨에 문자열을 표시할 수 있음

```swift
print(characterLabel.text ?? "")
```

<br>
<br>

# 03 글자 색상 설정

```swift
characterLabel.textColor = .systemBlue
```

`textColor`를 사용해 텍스트 색상 변경 가능함

```swift
characterLabel.text = "하치와레"
characterLabel.textColor = .systemBlue
```

먼작귀 캐릭터별로 다른 색상을 적용할 때 사용 가능

<br>
<br>

# 04 글자 크기와 굵기 설정 ⭐⭐

```swift
characterLabel.font = .systemFont(ofSize: 24)
```

글자 크기를 설정할 수 있음

```swift
characterLabel.font = .boldSystemFont(ofSize: 24)
```

**굵은 글씨**로 설정 가능함

먼작귀 제목이나 캐릭터 이름을 강조할 때 사용 가능함

<br>
<br>

# 05 텍스트 정렬

```swift
characterLabel.textAlignment = .center
```

텍스트를 가운데 정렬할 수 있음

```swift
characterLabel.textAlignment = .left
characterLabel.textAlignment = .right
```

왼쪽 또는 오른쪽 정렬도 가능함

| 값 | 설명 |
|---|---|
| `.left` | 왼쪽 정렬함 |
| `.center` | 가운데 정렬함 |
| `.right` | 오른쪽 정렬함 |
| `.natural` | 기본 방향에 맞춰 정렬함 |

<br>
<br>

# 06 여러 줄 표시

기본적으로 `UILabel`은 한 줄만 표시함

```swift
characterLabel.numberOfLines = 0
```

`numberOfLines`를 `0`으로 설정하면 여러 줄 표시 가능함

```swift
characterLabel.text = """
치이카와와 하치와레가
즐거운 하루를 보내고 있어요.
"""

characterLabel.numberOfLines = 0
```

먼작귀 캐릭터의 긴 대사나 설명을 표시할 때 사용

<br>
<br>

# 07 긴 텍스트 처리

텍스트가 라벨의 범위를 넘어갈 때 처리 방법 설정 가능함

```swift
characterLabel.lineBreakMode = .byTruncatingTail
```

텍스트 뒷부분을 `...`으로 줄여 표시할 수 있음

```swift
characterLabel.lineBreakMode = .byWordWrapping
```

단어 단위로 줄바꿈 가능함

| 값 | 설명 |
|---|---|
| `.byWordWrapping` | 단어 단위로 줄바꿈함 |
| `.byCharWrapping` | 글자 단위로 줄바꿈함 |
| `.byTruncatingTail` | 뒷부분을 `...`으로 생략함 |
| `.byTruncatingMiddle` | 중간 부분을 `...`으로 생략함 |
| `.byTruncatingHead` | 앞부분을 `...`으로 생략함 |

<br>
<br>

# 08 화면에 `UILabel` 추가

```swift
import UIKit

class MainViewController: UIViewController {
    
    let characterLabel: UILabel = {
        let label = UILabel()
        label.text = "치이카와"
        label.textColor = .systemBlue
        label.font = .boldSystemFont(ofSize: 24)
        label.textAlignment = .center
        return label
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .systemYellow
        view.addSubview(characterLabel)
        
        characterLabel.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            characterLabel.centerXAnchor.constraint(
                equalTo: view.centerXAnchor
            ),
            characterLabel.centerYAnchor.constraint(
                equalTo: view.centerYAnchor
            )
        ])
    }
}
```

### 🍀 코드 흐름
1. `UILabel` 생성함
2. 치이카와 텍스트 설정함
3. 텍스트 색상과 크기 설정함
4. `view.addSubview()`로 화면에 추가함
5. 오토 레이아웃으로 위치 설정함

<br>
<br>

# 09 `attributedText` 사용

텍스트 일부만 다른 색상이나 굵기로 설정 가능함

```swift
let text = NSMutableAttributedString(
    string: "치이카와와 하치와레"
)

text.addAttribute(
    .foregroundColor,
    value: UIColor.systemBlue,
    range: NSRange(location: 0, length: 4)
)

text.addAttribute(
    .font,
    value: UIFont.boldSystemFont(ofSize: 22),
    range: NSRange(location: 0, length: 4)
)

characterLabel.attributedText = text
```

하나의 라벨 안에서 캐릭터 이름마다 다른 스타일을 적용할 때 사용 가능함

<br>
<br>

# 10 `UILabel` 숨기기

```swift
characterLabel.isHidden = true
```

라벨을 화면에서 숨길 수 있음

```swift
characterLabel.isHidden = false
```

숨긴 라벨을 다시 표시 가능함

```swift
characterLabel.alpha = 0.5
```

라벨의 투명도 조절 가능함

<br>
<br>

# 11 `UILabel`과 `UIButton` 차이

| 구분 | `UILabel` | `UIButton` |
|---|---|---|
| 역할 | 텍스트 표시함 | 버튼 표시함 |
| 사용자 터치 | 기본적으로 처리하지 않음 | 터치 이벤트 처리 가능함 |
| 먼작귀 예시 | 치이카와 이름과 대사 표시함 | 캐릭터 선택 버튼 제작함 |
| 주요 용도 | 정보 전달함 | 동작 실행함 |

먼작귀 캐릭터 <mark>이름이나 대사를 보여줄 때는 `UILabel`을 사용하고 캐릭터를 선택하거나 다음 화면으로 이동할 때는 `UIButton`을 사용</mark>함

<br>
<br>

# 12 정리

- `UILabel`은 화면에 텍스트를 표시하는 컴포넌트임
- `text`로 치이카와, 하치와레 같은 문자열 설정 가능
- `textColor`로 글자 색상 변경 가능
- `font`로 글자 크기와 굵기 설정 가능
- `textAlignment`로 텍스트 정렬 가능함
- `numberOfLines`로 여러 줄 표시 가능
- `lineBreakMode`로 긴 텍스트 처리 가능
- `attributedText`로 텍스트 일부의 스타일 변경 가능함
- `UILabel`은 주로 정보를 표시하며, 버튼 동작은 기본적으로 처리 ❌

> ### ⇒ `UILabel`은 먼작귀 캐릭터 이름이나 대사를 화면에 표시하고, 텍스트의 색상·크기·정렬·줄바꿈 등을 설정할 수 있는 UI 컴포넌트임 ✨