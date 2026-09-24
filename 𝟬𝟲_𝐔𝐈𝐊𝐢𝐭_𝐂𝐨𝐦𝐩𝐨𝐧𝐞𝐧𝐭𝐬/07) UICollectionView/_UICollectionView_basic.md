## 컬렉션 뷰




# 01 `UICollectionView`란?

`UICollectionView`는 데이터를 다양한 형태의 레이아웃으로 보여주는 UI 컴포넌트임

사진 갤러리, 상품 목록, 카드 목록 등을 만들 때 사용 가능함

```swift
let collectionView = UICollectionView(
    frame: .zero,
    collectionViewLayout: UICollectionViewFlowLayout()
)
```

`UITableView`와 달리 세로 목록뿐만 아니라 격자, 가로 목록 등 다양한 형태로 배치할 수 있음






# 02 `UICollectionView`의 주요 구성 요소

| 구성 요소 | 설명 |
|---|---|
| `UICollectionView` | 전체 목록을 관리함 |
| `UICollectionViewCell` | 한 개의 항목을 표시함 |
| `UICollectionViewLayout` | 셀의 배치 방식을 결정함 |
| `Data Source` | 데이터와 셀을 제공함 |
| `Delegate` | 선택, 크기 등 동작을 처리함 |






# 03 레이아웃 설정하기

```swift
let layout = UICollectionViewFlowLayout()

let collectionView = UICollectionView(
    frame: .zero,
    collectionViewLayout: layout
)
```

`UICollectionView`는 생성할 때 레이아웃을 반드시 지정해야 함

가장 기본적인 레이아웃은 `UICollectionViewFlowLayout`임






# 04 `Data Source`와 `Delegate` 연결하기

```swift
collectionView.dataSource = self
collectionView.delegate = self
```

```swift
extension ViewController:
    UICollectionViewDataSource,
    UICollectionViewDelegate {
}
```

`dataSource`는 셀과 데이터를 제공하고, `delegate`는 셀 선택 등의 동작을 처리함






# 05 셀 등록하기

코드로 셀을 사용할 때는 먼저 셀을 등록해야 함

```swift
collectionView.register(
    UICollectionViewCell.self,
    forCellWithReuseIdentifier: "Cell"
)
```

등록할 때 사용한 식별자와 셀을 가져올 때 사용하는 식별자가 같아야 함






# 06 셀 개수 설정하기

```swift
func collectionView(
    _ collectionView: UICollectionView,
    numberOfItemsInSection section: Int
) -> Int {
    return items.count
}
```

`numberOfItemsInSection`은 컬렉션 뷰에 표시할 셀의 개수를 반환함






# 07 셀 생성하기

```swift
func collectionView(
    _ collectionView: UICollectionView,
    cellForItemAt indexPath: IndexPath
) -> UICollectionViewCell {
    let cell = collectionView.dequeueReusableCell(
        withReuseIdentifier: "Cell",
        for: indexPath
    )

    cell.backgroundColor = .systemBlue

    return cell
}
```

`cellForItemAt`은 각 위치에 표시할 셀을 반환함

- `indexPath.item`: 현재 셀의 위치
- `dequeueReusableCell`: 기존 셀을 재사용함






# 08 셀 크기 설정하기

```swift
func collectionView(
    _ collectionView: UICollectionView,
    layout collectionViewLayout: UICollectionViewLayout,
    sizeForItemAt indexPath: IndexPath
) -> CGSize {
    return CGSize(width: 100, height: 100)
}
```

각 셀의 너비와 높이를 설정할 수 있음






# 09 셀 선택 처리하기

```swift
func collectionView(
    _ collectionView: UICollectionView,
    didSelectItemAt indexPath: IndexPath
) {
    print("\(items[indexPath.item]) 선택됨")
}
```

사용자가 셀을 선택하면 `didSelectItemAt`이 호출됨






# 10 셀 간격 설정하기

```swift
let layout = UICollectionViewFlowLayout()

layout.minimumLineSpacing = 10
layout.minimumInteritemSpacing = 10
```

| 속성 | 설명 |
|---|---|
| `minimumLineSpacing` | 줄 사이의 간격 |
| `minimumInteritemSpacing` | 같은 줄에 있는 셀 사이의 간격 |






# 11 기본 예제

```swift
import UIKit

final class ViewController: UIViewController {

    private let items = [
        "첫 번째",
        "두 번째",
        "세 번째"
    ]

    private let collectionView: UICollectionView = {
        let layout = UICollectionViewFlowLayout()
        layout.itemSize = CGSize(width: 100, height: 100)
        layout.minimumLineSpacing = 10
        layout.minimumInteritemSpacing = 10

        return UICollectionView(
            frame: .zero,
            collectionViewLayout: layout
        )
    }()

    override func viewDidLoad() {
        super.viewDidLoad()

        collectionView.dataSource = self
        collectionView.delegate = self

        collectionView.register(
            UICollectionViewCell.self,
            forCellWithReuseIdentifier: "Cell"
        )

        view.addSubview(collectionView)
        collectionView.translatesAutoresizingMaskIntoConstraints = false

        NSLayoutConstraint.activate([
            collectionView.topAnchor.constraint(
                equalTo: view.safeAreaLayoutGuide.topAnchor
            ),
            collectionView.leadingAnchor.constraint(
                equalTo: view.leadingAnchor
            ),
            collectionView.trailingAnchor.constraint(
                equalTo: view.trailingAnchor
            ),
            collectionView.bottomAnchor.constraint(
                equalTo: view.bottomAnchor
            )
        ])
    }
}

extension ViewController:
    UICollectionViewDataSource,
    UICollectionViewDelegate {

    func collectionView(
        _ collectionView: UICollectionView,
        numberOfItemsInSection section: Int
    ) -> Int {
        return items.count
    }

    func collectionView(
        _ collectionView: UICollectionView,
        cellForItemAt indexPath: IndexPath
    ) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(
            withReuseIdentifier: "Cell",
            for: indexPath
        )

        cell.backgroundColor = .systemBlue

        return cell
    }

    func collectionView(
        _ collectionView: UICollectionView,
        didSelectItemAt indexPath: IndexPath
    ) {
        print(items[indexPath.item])
    }
}
```

### 💻 코드 흐름

1. `UICollectionViewFlowLayout`을 생성함
2. 컬렉션 뷰를 생성하고 레이아웃을 연결함
3. 셀을 등록함
4. `dataSource`와 `delegate`를 연결함
5. 셀 개수와 셀 내용을 설정함
6. 셀 선택 이벤트를 처리함






# 12 `UITableView`와의 차이

| 구분 | `UITableView` | `UICollectionView` |
|---|---|---|
| 기본 형태 | 세로 목록 | 격자, 가로·세로 목록 |
| 셀 메서드 | `cellForRowAt` | `cellForItemAt` |
| 위치 값 | `indexPath.row` | `indexPath.item` |
| 레이아웃 | 기본 목록 형태 | 직접 설정 가능 |
| 사용 예시 | 설정, 연락처 | 갤러리, 상품 목록 |






# 13 자주 발생하는 오류

- `collectionViewLayout`을 지정하지 않은 경우
- 셀을 등록하지 않은 경우
- `Reuse Identifier`가 일치하지 않는 경우
- `dataSource`와 `delegate`를 연결하지 않은 경우
- `numberOfItemsInSection`에서 잘못된 개수를 반환한 경우






# 14 `UICollectionView` 정리

- `UICollectionView`는 데이터를 다양한 형태로 배치함
- `UICollectionViewCell`에 각 항목을 표시함
- `UICollectionViewFlowLayout`으로 셀 배치를 설정함
- `dataSource`는 셀과 데이터를 제공함
- `delegate`는 셀 선택과 크기 등을 처리함
- 셀은 반드시 등록한 뒤 재사용해야 함
- `cellForItemAt`에서 셀을 생성함
- `didSelectItemAt`에서 셀 선택을 처리함

> ⇒ `UICollectionView`는 데이터를 격자나 다양한 레이아웃으로 보여줄 수 있는 UI 컴포넌트 ✨