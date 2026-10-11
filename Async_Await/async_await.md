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

# 04 저장 · 조회 · 삭제
 
→ 저장은 `insert`, 조회는 `@Query`, 삭제는 `delete`를 사용함
 
```swift
struct ContentView: View {
    @Environment(\.modelContext) private var modelContext
    @Query private var characters: [Character]
 
    var body: some View {
        List(characters) { character in
            Text(character.name)
        }
 
        Button("시사 추가") {
            let sisa = Character(name: "시사", favoriteFood: "라멘")
            modelContext.insert(sisa)
        }
    }
}
```
 
`@Query`는 저장된 데이터를 자동으로 가져오고 데이터가 바뀌면 뷰도 함께 갱신해줌 !!
 
삭제할 때는 객체를 `delete`에 전달하면 됨
 
```swift
modelContext.delete(sisa)
```
 
SwiftData는 기본적으로 변경 사항을 자동 저장하지만 직접 저장하고 싶다면 `save`를 사용할 수 있음
 
```swift
try modelContext.save()
```
 
 
 
 
 
 
# 05 `Keychain`이란?
 
→ Keychain은 비밀번호나 토큰 같은 민감한 정보를 암호화해서 안전하게 보관하는 iOS의 저장소
 
로그인 후 받은 액세스 토큰을 일반 파일이나 `UserDefaults`에 저장하면 탈취당할 위험이 있음
 
Keychain은 시스템이 암호화해서 관리하기 때문에 민감한 정보를 저장하기에 적합함
 
```text
액세스 토큰
    ↓ Keychain
암호화되어 안전하게 저장
    ↓ 필요할 때
꺼내서 서버 요청에 사용
```
 
또한 앱을 삭제했다가 다시 설치해도 Keychain에 저장한 값이 남아 있을 수 있음
 
 
 
 
 
 
# 06 `Keychain` 사용하기
 
→ Keychain은 딕셔너리 형태의 query를 만들어 `SecItem` 함수로 저장 · 조회 · 삭제함
 
### 저장
 
```swift
let token = "sisa-access-token".data(using: .utf8)!
 
let query: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,
    kSecAttrAccount as String: "accessToken",
    kSecValueData as String: token
]
 
let status = SecItemAdd(query as CFDictionary, nil)
```
 
### 조회
 
```swift
let query: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,
    kSecAttrAccount as String: "accessToken",
    kSecReturnData as String: true,
    kSecMatchLimit as String: kSecMatchLimitOne
]
 
var result: AnyObject?
let status = SecItemCopyMatching(query as CFDictionary, &result)
 
if status == errSecSuccess, let data = result as? Data {
    let token = String(data: data, encoding: .utf8)
}
```
 
### 삭제
 
```swift
let query: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,
    kSecAttrAccount as String: "accessToken"
]
 
let status = SecItemDelete(query as CFDictionary)
```
 
`kSecAttrAccount`가 값을 구분하는 이름표 역할을 함
 
결과는 `OSStatus`로 돌아오며 `errSecSuccess`면 성공임
 
같은 이름표로 이미 저장된 값이 있으면 `SecItemAdd`가 실패하므로 값을 바꿀 때는 삭제 후 다시 저장하거나 `SecItemUpdate`를 사용함
 
 
 
 
 
 
# 07 `SwiftData`와 `Keychain`의 차이
 
→ 일반 데이터는 SwiftData에 민감한 정보는 Keychain에 저장함
 
| 구분 | `SwiftData` | `Keychain` |
| :--- | :--- | :--- |
| 용도 | 앱의 일반 데이터 저장 | 비밀번호 · 토큰 같은 민감 정보 저장 |
| 저장 단위 | Swift 객체 (`@Model`) | 작은 `Data` 값 |
| 보안 | 일반 저장 | 시스템이 암호화해서 관리 |
| 예시 | 시사의 이름 · 좋아하는 음식 | 로그인 액세스 토큰 |
 
시사의 정보는 SwiftData에 저장하고 시사로 로그인하기 위한 토큰은 Keychain에 저장하는 식으로 나눠서 사용함