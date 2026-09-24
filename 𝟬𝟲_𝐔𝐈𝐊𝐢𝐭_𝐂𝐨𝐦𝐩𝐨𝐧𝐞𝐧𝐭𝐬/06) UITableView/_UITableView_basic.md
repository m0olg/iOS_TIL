## 테이블 뷰




# 01 `UITableView`란?

`UITableView`는 데이터를 세로 목록 형태로 보여주는 UI 컴포넌트임

연락처, 게시글, 설정, 할 일 목록 등을 만들 때 사용 가능함

```swift
let tableView = UITableView()
```

각 항목은 `UITableViewCell`이라는 셀에 표시함






# 02 `UITableView`의 주요 구성 요소

| 구성 요소 | 설명 |
|---|---|
| `UITableView` | 전체 목록을 관리함 |
| `UITableViewCell` | 목록의 한 행을 표시함 |
| `Section` | 관련된 행을 그룹으로 묶음 |
| `Data Source` | 데이터와 셀을 제공함 |
| `Delegate` | 선택, 높이 등 동작을 처리함 |






# 03 `Data Source`와 `Delegate` 연결하기

```swift
tableView.dataSource = self
tableView.delegate = self
```

`UITableView`에 데이터를 표시하고 사용자의 동작을 처리하려면 연결해야 함

```swift
final class ViewController: UIViewController {

    @IBOutlet weak var tableView: UITableView!

    override func viewDidLoad() {
        super.viewDidLoad()

        tableView.dataSource = self
        tableView.delegate = self
    }
}
```

```swift
extension ViewController: UITableViewDataSource, UITableViewDelegate {
}
```






# 04 행 개수 설정하기

```swift
func tableView(
    _ tableView: UITableView,
    numberOfRowsInSection section: Int
) -> Int {
    return items.count
}
```

`numberOfRowsInSection`은 테이블 뷰에 표시할 행의 개수를 반환함

데이터 배열의 개수와 행의 개수를 맞춰야 함






# 05 셀 생성하기

```swift
func tableView(
    _ tableView: UITableView,
    cellForRowAt indexPath: IndexPath
) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(
        withIdentifier: "Cell",
        for: indexPath
    )

    cell.textLabel?.text = items[indexPath.row]

    return cell
}
```

`cellForRowAt`은 각 행에 표시할 셀을 반환함

- `indexPath.row`: 현재 행의 위치
- `dequeueReusableCell`: 기존 셀을 재사용함






# 06 셀 재사용하기

```swift
let cell = tableView.dequeueReusableCell(
    withIdentifier: "Cell",
    for: indexPath
)
```

화면에서 사라진 셀을 재사용해 메모리를 절약함

스토리보드의 `Reuse Identifier`와 코드의 식별자가 같아야 함

```text
스토리보드: Cell
코드:       Cell
```






# 07 셀 선택 처리하기

```swift
func tableView(
    _ tableView: UITableView,
    didSelectRowAt indexPath: IndexPath
) {
    print("\(items[indexPath.row]) 선택됨")

    tableView.deselectRow(
        at: indexPath,
        animated: true
    )
}
```

사용자가 셀을 선택했을 때 `didSelectRowAt`이 호출됨

`deselectRow`를 사용하면 선택된 셀의 표시를 해제할 수 있음






# 08 섹션 제목 설정하기

```swift
func tableView(
    _ tableView: UITableView,
    titleForHeaderInSection section: Int
) -> String? {
    return "목록"
}
```

`titleForHeaderInSection`을 사용해 섹션 제목을 설정할 수 있음






# 09 셀 높이 설정하기

```swift
func tableView(
    _ tableView: UITableView,
    heightForRowAt indexPath: IndexPath
) -> CGFloat {
    return 60
}
```

모든 셀의 높이를 일정하게 설정할 수 있음

자동 높이를 사용하려면 다음과 같이 설정함

```swift
tableView.rowHeight = UITableView.automaticDimension
```






# 10 기본 예제

```swift
import UIKit

final class ViewController: UIViewController {

    @IBOutlet weak var tableView: UITableView!

    private let items = [
        "첫 번째 항목",
        "두 번째 항목",
        "세 번째 항목"
    ]

    override func viewDidLoad() {
        super.viewDidLoad()

        tableView.dataSource = self
        tableView.delegate = self
    }
}

extension ViewController: UITableViewDataSource, UITableViewDelegate {

    func tableView(
        _ tableView: UITableView,
        numberOfRowsInSection section: Int
    ) -> Int {
        return items.count
    }

    func tableView(
        _ tableView: UITableView,
        cellForRowAt indexPath: IndexPath
    ) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(
            withIdentifier: "Cell",
            for: indexPath
        )

        cell.textLabel?.text = items[indexPath.row]

        return cell
    }

    func tableView(
        _ tableView: UITableView,
        didSelectRowAt indexPath: IndexPath
    ) {
        print(items[indexPath.row])
    }
}
```

### 💻 코드 흐름

1. `UITableView`를 화면에 추가함
2. `dataSource`와 `delegate`를 연결함
3. 표시할 행의 개수를 반환함
4. 셀을 재사용해 데이터를 표시함
5. 셀 선택 이벤트를 처리함






# 11 자주 발생하는 오류

- `dataSource`와 `delegate`가 연결되지 않은 경우
- `Reuse Identifier`가 일치하지 않는 경우
- 행 개수와 배열의 개수가 다른 경우
- `cellForRowAt`에서 셀을 반환하지 않은 경우
- 셀을 재사용하면서 이전 내용이 남아 있는 경우






# 12 `UITableView` 정리

- `UITableView`는 데이터를 세로 목록으로 보여주는 UI 컴포넌트
- 한 줄의 항목은 `UITableViewCell`에 표시함
- `dataSource`는 데이터와 셀을 제공함
- `delegate`는 셀 선택과 높이 등을 처리함
- `numberOfRowsInSection`에서 행 개수를 반환함
- `cellForRowAt`에서 셀을 생성하고 데이터를 표시함
- `dequeueReusableCell`로 셀을 재사용함
- `didSelectRowAt`에서 셀 선택 이벤트를 처리함

> ⇒ `UITableView`는 여러 데이터를 세로 목록으로 보여주고, 사용자의 셀 선택까지 처리할 수 있는 UI 컴포넌트임 ✨