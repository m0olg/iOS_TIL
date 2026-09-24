## 스크롤 뷰



# 01 `UIScrollView`란?

`UIScrollView`는 화면보다 큰 콘텐츠를 스크롤할 수 있도록 하는 UI 컴포넌트임

긴 글, 큰 이미지, 여러 입력창이 있는 화면 등을 만들 때 사용 가능함

```swift
let scrollView = UIScrollView()
```






# 02 콘텐츠 크기 설정

```swift
scrollView.contentSize = CGSize(
    width: 0,
    height: 1000
)
```

`contentSize`는 스크롤할 콘텐츠의 전체 크기를 설정함

화면보다 콘텐츠의 높이가 크면 세로 스크롤이 가능함






# 03 스크롤 방향 설정하기

```swift
scrollView.alwaysBounceVertical = true
scrollView.alwaysBounceHorizontal = false
```

| 속성 | 설명 |
|---|---|
| `alwaysBounceVertical` | 세로 스크롤 허용함 |
| `alwaysBounceHorizontal` | 가로 스크롤 허용함 |

기본적으로 콘텐츠 크기에 따라 스크롤 방향이 결정됨






# 04 스크롤 위치와 여백 설정하기

```swift
scrollView.contentInset = UIEdgeInsets(
    top: 20,
    left: 0,
    bottom: 20,
    right: 0
)
```

`contentInset`을 사용해 콘텐츠 주변에 여백을 설정할 수 있음

```swift
scrollView.contentOffset = CGPoint(
    x: 0,
    y: 100
)
```

`contentOffset`으로 현재 스크롤 위치를 설정할 수 있음






# 05 오토 레이아웃으로 사용하기

오토 레이아웃에서는 `contentLayoutGuide`와 `frameLayoutGuide`를 활용함

```swift
let scrollView = UIScrollView()
let contentView = UIView()

view.addSubview(scrollView)
scrollView.addSubview(contentView)

scrollView.translatesAutoresizingMaskIntoConstraints = false
contentView.translatesAutoresizingMaskIntoConstraints = false

NSLayoutConstraint.activate([
    scrollView.topAnchor.constraint(
        equalTo: view.safeAreaLayoutGuide.topAnchor
    ),
    scrollView.leadingAnchor.constraint(
        equalTo: view.leadingAnchor
    ),
    scrollView.trailingAnchor.constraint(
        equalTo: view.trailingAnchor
    ),
    scrollView.bottomAnchor.constraint(
        equalTo: view.bottomAnchor
    ),

    contentView.topAnchor.constraint(
        equalTo: scrollView.contentLayoutGuide.topAnchor
    ),
    contentView.leadingAnchor.constraint(
        equalTo: scrollView.contentLayoutGuide.leadingAnchor
    ),
    contentView.trailingAnchor.constraint(
        equalTo: scrollView.contentLayoutGuide.trailingAnchor
    ),
    contentView.bottomAnchor.constraint(
        equalTo: scrollView.contentLayoutGuide.bottomAnchor
    ),
    contentView.widthAnchor.constraint(
        equalTo: scrollView.frameLayoutGuide.widthAnchor
    )
])
```

세로 스크롤에서는 `contentView`의 높이를 내부 콘텐츠에 맞게 설정해야 함






# 06 스크롤바 설정하기

```swift
scrollView.showsVerticalScrollIndicator = true
scrollView.showsHorizontalScrollIndicator = false
```

스크롤바 표시 여부를 설정할 수 있음

| 속성 | 설명 |
|---|---|
| `showsVerticalScrollIndicator` | 세로 스크롤바 표시 여부 |
| `showsHorizontalScrollIndicator` | 가로 스크롤바 표시 여부 |






# 07 키보드가 올라올 때 처리하기

입력창이 있는 화면에서는 키보드가 콘텐츠를 가릴 수 있음

```swift
scrollView.keyboardDismissMode = .onDrag
```

스크롤하면 키보드가 자동으로 내려가도록 설정할 수 있음

```swift
.onDrag
```

스크롤을 시작하면 키보드가 내려감

```swift
.interactive
```

스크롤 동작에 따라 키보드가 내려감






# 08 확대·축소 설정하기

이미지 등을 확대하거나 축소하려면 `minimumZoomScale`과 `maximumZoomScale`을 설정함

```swift
scrollView.minimumZoomScale = 1.0
scrollView.maximumZoomScale = 3.0
scrollView.delegate = self
```

확대할 뷰를 반환해야 함

```swift
func viewForZooming(
    in scrollView: UIScrollView
) -> UIView? {
    return imageView
}
```






# 09 스크롤 이벤트 처리하기

```swift
func scrollViewDidScroll(
    _ scrollView: UIScrollView
) {
    print("스크롤주우우ㅜ우우ㅜㅇㅇ")
}
```

스크롤 위치가 변경될 때마다 호출됨

무한 스크롤, 특정 위치에서 버튼 표시 등의 기능을 만들 때 사용할 수 있음






# 10 기본 예제

```swift
import UIKit

final class ViewController: UIViewController {

    private let scrollView = UIScrollView()
    private let contentView = UIView()

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .white

        view.addSubview(scrollView)
        scrollView.addSubview(contentView)

        scrollView.translatesAutoresizingMaskIntoConstraints = false
        contentView.translatesAutoresizingMaskIntoConstraints = false

        NSLayoutConstraint.activate([
            scrollView.topAnchor.constraint(
                equalTo: view.safeAreaLayoutGuide.topAnchor
            ),
            scrollView.leadingAnchor.constraint(
                equalTo: view.leadingAnchor
            ),
            scrollView.trailingAnchor.constraint(
                equalTo: view.trailingAnchor
            ),
            scrollView.bottomAnchor.constraint(
                equalTo: view.bottomAnchor
            ),

            contentView.topAnchor.constraint(
                equalTo: scrollView.contentLayoutGuide.topAnchor
            ),
            contentView.leadingAnchor.constraint(
                equalTo: scrollView.contentLayoutGuide.leadingAnchor
            ),
            contentView.trailingAnchor.constraint(
                equalTo: scrollView.contentLayoutGuide.trailingAnchor
            ),
            contentView.bottomAnchor.constraint(
                equalTo: scrollView.contentLayoutGuide.bottomAnchor
            ),
            contentView.widthAnchor.constraint(
                equalTo: scrollView.frameLayoutGuide.widthAnchor
            ),
            contentView.heightAnchor.constraint(
                equalToConstant: 1000
            )
        ])
    }
}
```

### 💻 코드 흐름

1. `UIScrollView`를 생성함
2. 콘텐츠를 담을 `contentView`를 추가함
3. 스크롤 뷰를 화면에 배치함
4. `contentLayoutGuide`로 콘텐츠 영역을 설정함
5. `frameLayoutGuide`로 화면 너비를 고정함
6. 콘텐츠 높이를 화면보다 크게 설정함






# 11 `UIScrollView` 정리

- `UIScrollView`는 화면보다 큰 콘텐츠를 스크롤하게 해줌
- `contentSize`로 스크롤할 전체 콘텐츠 크기를 설정함
- `contentOffset`으로 현재 스크롤 위치를 설정함
- `contentInset`으로 콘텐츠 여백을 설정함
- 오토 레이아웃에서는 `contentLayoutGuide`와 `frameLayoutGuide`를 사용함
- `keyboardDismissMode`로 스크롤 시 키보드를 내릴 수 있음
- `viewForZooming`으로 확대·축소할 뷰를 지정함
- `scrollViewDidScroll`로 스크롤 이벤트를 처리함

> ⇒ `UIScrollView`는 화면보다 큰 콘텐츠를 사용자가 스크롤할 수 있도록 해주는 UI 컴포넌트임 ✨