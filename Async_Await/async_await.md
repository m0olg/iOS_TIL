## SwiftData and Keychain

<br>
 
# 01 `SwiftData`란?
 
→ SwiftData는 Swift 객체를 앱 안에 저장하고 불러올 수 있게 해주는 Apple의 데이터 저장 프레임워크
 
앱을 껐다 켜도 데이터가 남아 있으려면 데이터를 기기에 저장해야 함
 
SwiftData를 사용하면 별도의 데이터베이스 코드 없이 Swift 객체를 그대로 저장할 수 있음
 
> Swift 객체 → SwiftData → 기기에 저장
 
예를 들어 먼작귀 캐릭터 시사의 정보를 앱 안에 저장해두는 상황이라고 해보자
 
```text
시사 객체
    ↓ SwiftData
기기에 저장
    ↓ 앱 재실행
저장된 시사 객체 불러오기
```
 
 <br>
  <br>
 
 
 
# 02 `@Model`
 
→ `@Model`은 클래스를 SwiftData가 저장할 수 있는 모델로 만들어주는 매크로
 
```swift
import SwiftData
 
@Model
class Character {
    var name: String
    var favoriteFood: String
 
    init(name: String, favoriteFood: String) {
        self.name = name
        self.favoriteFood = favoriteFood
    }
}
```
 
`@Model`은 `struct`가 아니라 `class`에만 붙일 수 있음
 
이 매크로가 붙은 클래스의 프로퍼티들이 저장 대상이 됨
 
저장하고 싶지 않은 프로퍼티가 있다면 `@Transient`를 붙여서 제외할 수 있음
 
 
 <br>
 <br>
 
 
# 03 `ModelContainer`와 `ModelContext`
 
→ `ModelContainer`는 저장소 자체이고 `ModelContext`는 그 저장소에서 데이터를 다루는 작업 공간임
 
```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        .modelContainer(for: Character.self)
    }
}
```
 
`.modelContainer(for:)`를 앱 진입점에 붙이면 저장소가 만들어지고 하위 뷰에서 사용할 수 있게 됨
 
뷰에서는 `modelContext`를 환경에서 꺼내 사용함
 
```swift
@Environment(\.modelContext) private var modelContext
```
 
데이터를 추가하거나 삭제하는 작업은 모두 `modelContext`를 통해 이루어짐

 <br>
 <br>

