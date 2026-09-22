## UIImageView

<br>

# 01 `UIImageView`란?

→ `UIImageView`는 앱 화면에 **이미지를 표시하는 UI 컴포넌트**임

캐릭터 이미지, 프로필 사진, 배경 이미지, 아이콘 등을 화면에 표시할 때 사용 가능함

```swift
let imageView = UIImageView()
```

<br>
<br>

# 02 이미지 설정하기

```swift
let imageView = UIImageView()

imageView.image = UIImage(named: "chiikawa")
```

`image` 프로퍼티를 사용해 이미지 설정 가능함

`UIImage(named:)`를 사용하려면 이미지 파일이 프로젝트의 Assets에 추가되어 있어야 함

<br>
<br>

# 03 이미지 추가하기

> 이미지 파일을 프로젝트에 추가한 후 Assets에 넣어야 함

### ① 이미지 파일 추가함

이미지 파일을 Xcode 프로젝트로 가져옴

### ② Assets에 이미지 등록함

이미지 파일을 Assets에 드래그하여 추가함

### ③ 이미지 이름으로 불러옴

```swift
let image = UIImage(named: "chiikawa")
imageView.image = image
```

`UIImage(named:)` 안에는 Assets에 등록된 이미지 이름을 작성해야 함

<br>
<br>

# 04 이미지 크기 설정하기

```swift
imageView.frame = CGRect(
    x: 0,
    y: 0,
    width: 200,
    height: 200
)
```

`frame`을 사용해 이미지 뷰의 위치와 크기를 설정할 수 있음

```swift
imageView.frame = CGRect(
    x: 20,
    y: 100,
    width: 150,
    height: 150
)
```

이미지 뷰를 화면의 $x = 20$, $y = 100$ 위치에 배치하고 크기를 $150 \times 150$으로 설정함

<br>
<br>

# 05 이미지 크기 비율 설정하기

이미지와 이미지 뷰의 크기가 다르면 이미지가 늘어나거나 잘릴 수 있음

`contentMode`를 사용해 이미지 표시 방식을 설정 가능함

```swift
imageView.contentMode = .scaleAspectFit
```

이미지의 비율을 유지하면서 이미지 뷰 안에 전체 이미지를 표시함

```swift
imageView.contentMode = .scaleAspectFill
```

이미지의 비율을 유지하면서 이미지 뷰를 가득 채움

이미지 일부가 잘릴 수 있음

```swift
imageView.contentMode = .scaleToFill
```

이미지를 이미지 뷰 크기에 맞게 늘림

이미지 비율이 달라질 수 있음

| 값 | 설명 |
|---|---|
| `.scaleAspectFit` | 이미지 전체를 표시하고 빈 공간이 생길 수 있음 |
| `.scaleAspectFill` | 이미지 뷰를 가득 채우고 일부가 잘릴 수 있음 |
| `.scaleToFill` | 이미지 뷰 크기에 맞게 이미지를 늘림 |
| `.center` | 이미지 크기를 유지한 채 가운데 배치함 |

<br>
<br>

# 06 이미지 모서리 설정하기

```swift
imageView.layer.cornerRadius = 20
```

`cornerRadius`를 사용해 이미지 모서리를 둥글게 설정 가능함

```swift
imageView.layer.cornerRadius = 20
imageView.clipsToBounds = true
```

`clipsToBounds`를 `true`로 설정해야 이미지가 둥근 모서리 영역을 벗어나지 않음

프로필 사진처럼 원형 이미지로 만들 수도 있음

```swift
imageView.layer.cornerRadius = 50
imageView.clipsToBounds = true
```

이미지 뷰의 크기가 $100 \times 100$이라면 모서리 반지름을 $50$으로 설정해 원형으로 만들 수 있음

<br>
<br>

# 07 이미지 투명도와 숨김 설정하기

```swift
imageView.alpha = 0.5
```

`alpha`를 사용해 이미지의 투명도 설정 가능함

```swift
imageView.isHidden = true
```

이미지 뷰를 화면에서 숨길 수 있음

```swift
imageView.isHidden = false
```

숨긴 이미지 뷰를 다시 표시 가능함

<br>
<br>

# 08 이미지에 접근성 설명 추가하기

```swift
imageView.accessibilityLabel = "치이카와 캐릭터 이미지"
```

`accessibilityLabel`을 사용해 이미지에 대한 설명을 제공할 수 있음

시각 보조 기능을 사용하는 사용자에게 이미지 내용을 전달할 때 사용 가능함

<br>
<br>

# 09 코드로 이미지 뷰 화면에 추가하기

```swift
import UIKit

class MainViewController: UIViewController {
    
    let characterImageView: UIImageView = {
        let imageView = UIImageView()
        
        imageView.image = UIImage(named: "chiikawa")
        imageView.contentMode = .scaleAspectFit
        imageView.layer.cornerRadius = 20
        imageView.clipsToBounds = true
        
        return imageView
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .systemYellow
        
        view.addSubview(characterImageView)
        
        characterImageView.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            characterImageView.centerXAnchor.constraint(
                equalTo: view.centerXAnchor
            ),
            characterImageView.centerYAnchor.constraint(
                equalTo: view.centerYAnchor
            ),
            characterImageView.widthAnchor.constraint(
                equalToConstant: 200
            ),
            characterImageView.heightAnchor.constraint(
                equalToConstant: 200
            )
        ])
    }
}
```

### 💻 코드 흐름
1. `UIImageView` 생성함
2. 이미지 설정함
3. 이미지 비율 설정함
4. 이미지 모서리 설정함
5. `view.addSubview()`로 화면에 추가함
6. 오토 레이아웃으로 이미지 위치와 크기 설정함

<br>
<br>

# 10 이미지가 없을 때 처리하기

이미지 이름이 잘못되었거나 Assets에 이미지가 없으면 이미지가 표시되지 않음

```swift
imageView.image = UIImage(named: "wrongImageName")
```

이미지가 제대로 불러와졌는지 확인할 수 있음

```swift
if let image = UIImage(named: "chiikawa") {
    imageView.image = image
} else {
    print("이미지를 찾을 수 없습니다.")
}
```

이미지가 없을 때 기본 이미지를 표시할 수도 있음

```swift
imageView.image = UIImage(
    named: "chiikawa"
) ?? UIImage(
    systemName: "photo"
)
```

<br>
<br>

# 11 `UIImageView`와 `UIButton`의 차이

| 구분 | `UIImageView` | `UIButton` |
|---|---|---|
| 역할 | 이미지를 표시함 | 사용자의 동작을 실행함 |
| 터치 이벤트 | 기본적으로 처리하지 않음 | 처리 가능함 |
| 사용 예시 | 치이카와 이미지, 카페 사진 표시함 | 이미지 버튼, 검색 버튼 제작함 |
| 주요 목적 | 이미지 전달함 | 사용자와 상호작용함 |

이미지만 표시할 때는 `UIImageView`를 사용하고, 이미지를 눌렀을 때 동작을 실행해야 한다면 `UIButton`을 사용함

<br>
<br>

# 12 정리

- `UIImageView`는 화면에 이미지를 표시하는 UI 컴포넌트
- `image`로 표시할 이미지 설정 가능
- `UIImage(named:)`로 Assets의 이미지 불러오기 가능
- `contentMode`로 이미지 표시 방식 설정 가능
- `.scaleAspectFit`은 이미지 전체를 표시함
- `.scaleAspectFill`은 이미지 뷰를 가득 채우지만 일부가 잘릴 수 있음ㅠㅠㅠㅠ
- `cornerRadius`로 이미지 모서리를 둥글게 설정 가능
- `clipsToBounds`로 이미지가 영역 밖으로 나가지 않도록 설정 가능함
- `alpha`로 이미지 투명도 설정 가능
- `isHidden`으로 이미지 뷰를 숨기거나 표시 가능
- 이미지를 눌러 동작을 실행해야 한다면 `UIButton`을 사용

> ### ⇒ `UIImageView`는 치이카와 같은 캐릭터 이미지나 카페 사진을 화면에 표시하고 이미지 크기·비율·모서리·투명도 등을 설정할 수 있는 UI 컴포넌트임 ✨