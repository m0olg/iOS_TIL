## Async/Await

<br>
 
# 01 `비동기 처리`란?
 
→ 비동기 처리는 시간이 걸리는 작업을 기다리는 동안 앱이 멈추지 않도록 다른 작업을 계속 진행하는 방식
 
네트워크 통신이나 파일 읽기처럼 결과가 바로 나오지 않는 작업은 완료될 때까지 시간이 걸림
 
이 작업을 기다리느라 화면이 멈추면 사용자는 앱이 먹통이 된 것처럼 느끼게 됨
 
예를 들어 서버에서 먼작귀 캐릭터 시사의 정보를 가져오는 상황이라고 해보자
 
```text
시사 정보 요청
    ↓ 응답을 기다리는 동안에도
앱 화면은 계속 동작
    ↓ 응답 도착
시사 정보 화면에 표시
```
 
이처럼 오래 걸리는 작업을 기다리면서도 앱이 멈추지 않게 해주는 것이 비동기 처리임

<br>
<br>

# 02 `async/await`란?
 
→ `async/await`는 비동기 코드를 위에서 아래로 읽히는 일반 코드처럼 작성할 수 있게 해주는 Swift 문법
 
> `async` : 이 함수는 비동기로 동작함 \
> `await` : 결과가 나올 때까지 여기서 기다림
 
기존에는 클로저(completion handler)로 결과를 받아서 코드가 중첩되기 쉬웠음
 
`async/await`를 사용하면 순서대로 읽히는 코드로 바꿀 수 있음
 
```swift
// 클로저 방식
fetchCharacter { result in
    // 결과 처리
}
 
// async/await 방식
let sisa = try await fetchCharacter()
```
 
`await`가 붙은 줄에서 함수가 잠시 멈추고 결과를 기다림
 
이때 스레드가 막히는 것이 아니라 다른 작업이 그 스레드를 사용할 수 있음
 
 
 <br>
<br>
 
 
 
# 03 `async` 함수 만들기
 
→ 함수 선언부의 `)` 뒤에 `async`를 붙이면 비동기 함수가 됨
 
```swift
struct Character: Decodable {
    let name: String
    let favoriteFood: String
}
 
func fetchCharacter() async throws -> Character {
    let url = URL(string: "https://example.com/characters/sisa")!
    let (data, _) = try await URLSession.shared.data(from: url)
    return try JSONDecoder().decode(Character.self, from: data)
}
```
 
`URLSession.shared.data(from:)`도 `async` 함수이기 때문에 `await`를 붙여서 호출해야 함
 
오류가 발생할 수 있다면 `async throws`로 함께 선언함
 
이때 순서는 항상 `async` 다음에 `throws`임
 
 
 <br>
<br>
 
 
 
# 04 `await`로 호출하기
 
→ `async` 함수는 `await`와 함께 호출해야 하고 `await`는 `async` 컨텍스트 안에서만 쓸 수 있음
 
```swift
func loadSisa() async {
    do {
        let sisa = try await fetchCharacter()
        print(sisa.name)
    } catch {
        print(error)
    }
}
```
 
`try await`는 오류도 던질 수 있고 기다려야 하는 함수에 사용함
 
일반 함수 안에서는 `await`를 바로 쓸 수 없음
 
```swift
func normalFunction() {
    let sisa = try await fetchCharacter()  // XXXXXX 컴파일 오류
}
```
 
이럴 때 사용하는 것이 `Task`임
 
 
 <br>
<br>
 
 
 
# 05 `Task`
 
→ `Task`는 동기 코드에서 비동기 작업을 시작할 수 있게 해주는 타입
 
```swift
func buttonTapped() {
    Task {
        do {
            let sisa = try await fetchCharacter()
            print(sisa.name)
        } catch {
            print(error)
        }
    }
}
```
 
버튼 액션처럼 `async`가 아닌 곳에서 비동기 함수를 부르고 싶을 때 `Task { }` 안에서 호출하면 됨
 
SwiftUI에서는 `.task` 수정자를 사용해 화면이 나타날 때 비동기 작업을 시작할 수도 있음
 
```swift
struct SisaView: View {
    @State private var sisa: Character?
 
    var body: some View {
        Text(sisa?.name ?? "불러오는 중")
            .task {
                sisa = try? await fetchCharacter()
            }
    }
}
```
 
`.task`는 화면이 사라지면 작업을 자동으로 취소해주는 장점이 있음



<br>
<br>



# 06 `async let`
 
→ `async let`은 서로 관련 없는 비동기 작업을 동시에 실행하고 싶을 때 사용함
 
`await`를 순서대로 쓰면 앞의 작업이 끝나야 다음 작업이 시작됨
 
```swift
// 순서대로 실행 : 총 시간 = 두 작업 시간의 합
let sisa = try await fetchCharacter(id: 1)
let usagi = try await fetchCharacter(id: 2)
```
 
`async let`을 사용하면 두 작업이 동시에 시작됨
 
```swift
// 동시에 실행 : 총 시간 = 더 오래 걸리는 작업 시간
async let sisa = fetchCharacter(id: 1)
async let usagi = fetchCharacter(id: 2)
 
let characters = try await [sisa, usagi]
```
 
결과가 필요한 시점에 `await`로 한 번에 받으면 됨
 
 
 
 <br>
<br>
 
 
# 07 `@MainActor`
 
→ `@MainActor`는 해당 코드가 반드시 메인 스레드에서 실행되도록 보장함
 
UI를 바꾸는 코드는 메인 스레드에서 실행되어야 함
 
`await` 이후에는 코드가 다른 스레드에서 이어질 수 있기 때문에 UI 관련 타입에는 `@MainActor`를 붙여줌
 
```swift
@MainActor
final class CharacterViewModel: ObservableObject {
    @Published var sisa: Character?
 
    func load() async {
        sisa = try? await fetchCharacter()
    }
}
```
 
`@MainActor`가 붙은 타입의 프로퍼티는 항상 메인 스레드에서 변경됨
 
그래서 `DispatchQueue.main.async`를 직접 쓰지 않아도 됨


<br>
<br>