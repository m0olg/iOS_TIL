## Encoding

# 01 `Encoding`이란?

→ 인코딩은 Swift 객체를 JSON이나 `Data` 같은 외부 데이터 형식으로 변환하는 과정

앱에서 사용하는 데이터는 Swift 객체 형태로 관리하지만, 서버에 보내거나 파일에 저장하려면 외부에서 사용할 수 있는 데이터 형식으로 바꿔야 함

> Swift 객체 → JSON/Data

예를 들어 먼작귀 캐릭터 시사의 이름과 좋아하는 음식을 담은 객체가 있다고 해보자

```swift
struct Character {
    let name: String
    let favoriteFood: String
}

let sisa = Character(
    name: "시사",
    favoriteFood: "라멘"
)
```

이 객체를 JSON 형식으로 표현하면 다음과 같음

```json
{
  "name": "시사",
  "favoriteFood": "라멘"
}
```

`sisa` 객체를 JSON으로 변환하는 과정이 `Encoding`임






# 02 `JSONEncoder`란?

→ `JSONEncoder`는 Swift 객체를 JSON 형식의 `Data`로 변환하는 타입

> Swift 객체 → JSONEncoder → JSON Data

```swift
struct Character: Encodable {
    let name: String
    let favoriteFood: String
}

let sisa = Character(
    name: "시사",
    favoriteFood: "라멘"
)

let encoder = JSONEncoder()
let data = try encoder.encode(sisa)
```

`encode(sisa)`를 실행하면 `sisa` 객체가 JSON으로 표현될 수 있는 `Data`로 변환됨

```swift
print(String(data: data, encoding: .utf8)!)
```

출력 결과

```json
{"name":"시사","favoriteFood":"라멘"}
```

여기서 `data`는 JSON 문자열 자체가 아니라 JSON 내용을 바이트 형태로 담고 있는 `Data` 타입임






# 03 `Data`란?

→ `Data`는 바이트 단위의 데이터를 저장하는 Swift 타입

네트워크 통신이나 파일 처리에서는 데이터를 `Data` 형태로 주고받는 경우가 많음

`JSONEncoder`로 인코딩한 결과도 `Data` 타입임

```swift
let data: Data = try JSONEncoder().encode(sisa)
```

예를 들어 앱에서 시사의 정보를 서버에 저장한다면 다음과 같은 흐름으로 전달됨

```text
시사 객체
    ↓ JSONEncoder
Data
    ↓ 네트워크 전송
서버
```

`Data`는 사람이 직접 읽기 위한 형태라기보다 앱과 서버가 데이터를 주고받거나 저장하기 위한 형태에 가까움






# 04 네트워크 통신에서의 `Encoding`

서버에 데이터를 보낼 때는 Swift 객체를 JSON `Data`로 변환한 뒤 HTTP 요청의 Body에 담음

```swift
struct Character: Encodable {
    let name: String
    let favoriteFood: String
}

let sisa = Character(
    name: "시사",
    favoriteFood: "라멘"
)

var request = URLRequest(
    url: URL(string: "https://example.com/characters")!
)

request.httpMethod = "POST"

request.setValue(
    "application/json",
    forHTTPHeaderField: "Content-Type"
)

request.httpBody = try JSONEncoder().encode(sisa)
```

`request.httpBody`는 `Data` 타입을 받음

따라서 `Character` 객체를 그대로 넣는 것이 아니라 `JSONEncoder`로 `Data`로 변환한 뒤 넣어야 함

```text
시사 객체
    ↓ JSONEncoder
JSON Data
    ↓ httpBody
서버로 전송
```

즉, 인코딩은 앱에 있는 시사의 정보를 서버가 전달받을 수 있는 형태로 준비하는 과정 !!






# 05 `camelCase`와 `snake_case`

Swift에서는 프로퍼티 이름을 주로 `camelCase`로 작성함

```swift
struct Character: Encodable {
    let favoriteFood: String
}
```

하지만 서버에서는 JSON 키를 `snake_case`로 요구할 수 있음

```json
{
  "favorite_food": "라멘"
}
```

이처럼 Swift 프로퍼티 이름과 서버가 요구하는 JSON 키 이름이 다를 수 있음






# 06 `keyEncodingStrategy`

Swift의 `camelCase` 프로퍼티 이름을 JSON의 `snake_case` 키 이름으로 바꿀 때 `keyEncodingStrategy`를 사용할 수 있음

```swift
struct Character: Encodable {
    let favoriteFood: String
}

let sisa = Character(favoriteFood: "라멘")

let encoder = JSONEncoder()
encoder.keyEncodingStrategy = .convertToSnakeCase

let data = try encoder.encode(sisa)
```

이 설정을 사용하면 `favoriteFood`가 JSON에서 `favorite_food`로 변환됨

```json
{
  "favorite_food": "라멘"
}
```

서버가 요구하는 JSON 키 형식에 맞춰 변환 전략을 설정하면 됨






# 07 `Encoding` 오류

`Encoding`은 Swift 객체를 외부 데이터 형식으로 바꾸는 과정에서 실패할 수 있음

`JSONEncoder`의 `encode` 메서드는 오류가 발생할 수 있으므로 `try`와 함께 사용함

```swift
let data = try JSONEncoder().encode(sisa)
```

오류를 처리하려면 `do-catch`를 사용할 수 있음

```swift
do {
    let data = try JSONEncoder().encode(sisa)
    print(data)
} catch {
    print(error)
}
```

인코딩할 타입은 `JSONEncoder`가 처리할 수 있도록 설정되어 있어야 됨






# 08 `Encoding` 정리


### [Encoding] ⬇️⬇️
```
시사 객체
    ↓ JSONEncoder
JSON Data
    ↓
서버 전송 또는 파일 저장
```

| 요소 | 역할 |
| :--- | :--- |
| `Encoding` | Swift 객체를 외부 데이터 형식으로 변환 |
| `JSONEncoder` | Swift 객체를 JSON `Data`로 변환 |
| `Data` | 네트워크 통신이나 파일 처리에 사용하는 데이터 타입 |
| `keyEncodingStrategy` | Swift 프로퍼티 이름을 JSON 키 형식에 맞게 변환 |

* `Encoding`은 Swift 객체를 JSON이나 `Data`로 변환하는 과정임
* `JSONEncoder`를 사용해 객체를 JSON `Data`로 만들 수 있음
* 인코딩한 `Data`는 서버로 보내거나 파일에 저장할 수 있음
* JSON 키 이름을 서버 형식에 맞춰야 할 때 `keyEncodingStrategy`를 사용할 수 있음
* 인코딩 과정에서 오류가 발생할 수 있으므로 오류 처리가 필요함

> ### ⇒ `Encoding`은 시사처럼 앱에서 사용하는 Swift 객체를 서버나 저장소에서 사용할 수 있는 JSON `Data`로 변환하는 과정임 ✨