## Encoding

<br>


# 01 `Encoding`이란?

→ `Encoding`은 Swift 객체를 JSON이나 Data 같은 외부 데이터 형식으로 변환하는 과정

즉, 앱에서 사용하는 데이터를 서버에 보내거나 파일에 저장할 수 있는 형태로 바꾸는 것

```text
Swift 객체 → JSON/Data
```

예를 들어 다음과 같은 Swift 객체가 있다고 해보자

```swift
struct User {
    let name: String
    let age: Int
}
```

이 객체를 JSON 형태로 변환하면 다음과 같은 데이터가 됨

```json
{
  "name": "Kim",
  "age": 20
}
```

<mark>`User` 객체를 JSON으로 변환하는 과정이 `Encoding`임


<br>

<br>




# 02 `Decoding`이란?

→ `Decoding`은 JSON이나 Data 같은 외부 데이터를 Swift 객체로 변환하는 과정

```text
JSON/Data → Swift 객체
```

서버에서 다음과 같은 JSON을 받았다고 해보자

```json
{
  "name": "Kim",
  "age": 20
}
```

이 JSON을 앱에서 사용할 수 있는 `User` 객체로 변환하는 것이 `Decoding`

```swift
struct User {
    let name: String
    let age: Int
}
```

서버에서 받은 JSON을 Swift 객체로 변환하면 앱에서 다음과 같이 사용할 수 있음

```swift
print(user.name)
print(user.age)
```


<br>
<br>





# 03 `Encoding`과 `Decoding`의 차이

| 개념 | 변환 방향 | 사용 목적 |
| :--- | :--- | :--- |
| **Encoding** | Swift 객체 → JSON/Data | 서버에 데이터를 보내거나 저장 |
| **Decoding** | JSON/Data → Swift 객체 | 서버에서 받은 데이터를 앱에서 사용 |

전체 흐름은 다음과 같음

```text
[Encoding]

Swift 객체
    ↓
JSON/Data
    ↓
서버에 전송


[Decoding]

서버에서 JSON 수신
    ↓
JSON/Data
    ↓
Swift 객체로 변환
```



<br>
<br>




# 04 `Encoder`와 `Decoder`

`Encoder`와 `Decoder`는 데이터를 특정 형식으로 변환하거나 복원하는 역할을 함

## 4-1 `Encoder`

→ Swift 객체를 외부 데이터 형식으로 변환하는 객체

```text
Swift 객체 → 외부 데이터
```

대표적인 Encoder는 `JSONEncoder`임

```swift
let encoder = JSONEncoder()
```

## 4-2 `Decoder`

→ 외부 데이터를 Swift 객체로 변환하는 객체

```text
외부 데이터 → Swift 객체
```

대표적인 Decoder는 `JSONDecoder`임

```swift
let decoder = JSONDecoder()
```

정리하면 다음과 같음

| 개념 | 역할 |
| :--- | :--- |
| **Encoder** | Swift 객체를 외부 데이터로 변환 |
| **Decoder** | 외부 데이터를 Swift 객체로 변환 |
| **JSONEncoder** | Swift 객체를 JSON Data로 변환 |
| **JSONDecoder** | JSON Data를 Swift 객체로 변환 |




<br>
<br>



# 05 `JSONEncoder`란?

→ `JSONEncoder`는 Swift 객체를 JSON Data로 변환하는 객체

```text
Swift 객체 → JSON Data
```

```swift
struct User: Encodable {
    let name: String
    let age: Int
}
```

```swift
let user = User(
    name: "Kim",
    age: 20
)

let encoder = JSONEncoder()
let data = try encoder.encode(user)
```

`JSONEncoder`가 `user` 객체를 JSON 형태의 `Data`로 변환함

```swift
print(String(data: data, encoding: .utf8)!)
```

출력 결과

```json
{"name":"Kim","age":20}
```

> 단 `JSONEncoder`를 사용하려면 해당 타입이 `Encodable`을 채택하고 있어야 함



<br>
<br>




# 06 `JSONDecoder`란?

→ `JSONDecoder`는 JSON Data를 Swift 객체로 변환하는 객체

```text
JSON Data → Swift 객체
```

```swift
struct User: Decodable {
    let name: String
    let age: Int
}
```

```swift
let decoder = JSONDecoder()

let user = try decoder.decode(
    User.self,
    from: data
)
```

`JSONDecoder`가 JSON Data를 `User` 객체로 변환함

이제 변환된 객체를 Swift 코드에서 사용할 수 있음

```swift
print(user.name)
print(user.age)
```

단, `JSONDecoder`를 사용하려면 해당 타입이 `Decodable`을 채택하고 있어야 함



<br>
<br>




# 07 `Data`란?

→ `Data`는 바이트 단위의 데이터를 저장하는 Swift 타입

iOS에서 네트워크 통신이나 파일 처리 시 데이터를 주고받는 기본 형태로 자주 사용

`JSONEncoder`로 변환한 결과도 `Data` 타입임 !!

```swift
let data: Data = try JSONEncoder().encode(user)
```

문자열로 표현되는 JSON도 실제 네트워크 통신에서는 `Data` 형태로 변환되어 전달됨

```
Swift 객체
    ↓ JSONEncoder
Data
    ↓ 네트워크 전송
서버
```

`Data`는 사람이 읽기 위한 형태라기보다는 앱과 서버가 데이터를 주고받기 위한 형태에 가까움

<br>
<br>

# 08 `Encoding`과 `Decoding`의 전체 흐름

## 8-1 `Encoding` 흐름

```swift
struct User: Encodable {
    let name: String
    let age: Int
}

let user = User(
    name: "Kim",
    age: 20
)

let data = try JSONEncoder().encode(user)
```

```text
User 객체
    ↓
JSONEncoder
    ↓
JSON Data
```

## 8-2 `Decoding` 흐름

```swift
let user = try JSONDecoder().decode(
    User.self,
    from: data
)
```

```text
JSON Data
    ↓
JSONDecoder
    ↓
User 객체
```

전체 과정은 다음과 같음

```text
[Encoding]

Swift 객체
    ↓ JSONEncoder
JSON Data
    ↓ 네트워크 전송
서버


[Decoding]

서버
    ↓ JSON Data 응답
JSON Data
    ↓ JSONDecoder
Swift 객체
```



<br>
<br>





# 09 네트워크 통신에서의 `Encoding`

서버에 데이터를 보내려면 Swift 객체를 JSON Data로 변환한 뒤 HTTP Body에 넣어야 함

```swift
struct User: Encodable {
    let name: String
    let age: Int
}

let user = User(
    name: "Kim",
    age: 20
)

var request = URLRequest(
    url: URL(string: "https://example.com/users")!
)

request.httpMethod = "POST"

request.setValue(
    "application/json",
    forHTTPHeaderField: "Content-Type"
)

request.httpBody = try JSONEncoder().encode(user)
```

여기서 `httpBody`는 `Data` 타입을 받음

따라서 `User` 객체를 `JSONEncoder`를 통해 `Data`로 변환해야 함

```text
User 객체
    ↓ JSONEncoder
JSON Data
    ↓ httpBody
서버로 전송
```

<br>
<br>

# 10 네트워크 통신에서의 `Decoding`

서버에서 받은 응답 데이터는 JSON Data 형태임

이를 앱에서 사용할 수 있는 Swift 객체로 변환해야 함

```swift
struct User: Decodable {
    let name: String
    let age: Int
}
```

```swift
let (data, _) = try await URLSession.shared.data(
    for: request
)

let user = try JSONDecoder().decode(
    User.self,
    from: data
)

print(user.name)
```

```text
서버 응답 Data
    ↓
JSONDecoder
    ↓
User 객체
```

이렇게 변환된 `User` 객체를 화면에 표시하거나 앱의 다른 로직에서 사용할 수 있음



<br>
<br>




# 11 `camelCase`와 `snake_case`

Swift에서는 일반적으로 프로퍼티 이름을 `camelCase`로 작성함

```swift
struct User {
    let userName: String
}
```

하지만 서버에서는 JSON 키를 `snake_case`로 보내는 경우가 있음

```json
{
  "user_name": "Kim"
}
```

Swift 프로퍼티와 JSON 키의 이름이 다르면 Decoding에 실패할 수 있음

이때 `keyDecodingStrategy`를 사용할 수 있음

```swift
struct User: Decodable {
    let userName: String
}

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

let user = try decoder.decode(
    User.self,
    from: data
)
```

`user_name`을 Swift 프로퍼티인 `userName`으로 자동 변환해줌



<br>
<br>




# 12 `keyEncodingStrategy`

Swift 프로퍼티를 JSON으로 변환할 때도 이름 변환 방식을 설정할 수 있음

```swift
struct User: Encodable {
    let userName: String
}
```

기본적으로는 다음과 같은 JSON이 만들어짐

```json
{
  "userName": "Kim"
}
```

서버가 `snake_case` 형식을 요구한다면 다음과 같이 설정할 수 있음

```swift
let encoder = JSONEncoder()

encoder.keyEncodingStrategy = .convertToSnakeCase

let data = try encoder.encode(user)
```

변환 결과

```json
{
  "user_name": "Kim"
}
```

`camelCase` 프로퍼티를 `snake_case` JSON 키로 변환해줌



<br>
<br>




# 13 `keyDecodingStrategy`

서버에서 `snake_case` 형식의 JSON을 받을 때 사용할 수 있음

서버 JSON

```json
{
  "user_name": "Kim"
}
```

Swift 모델

```swift
struct User: Decodable {
    let userName: String
}
```

```swift
let decoder = JSONDecoder()

decoder.keyDecodingStrategy = .convertFromSnakeCase

let user = try decoder.decode(
    User.self,
    from: data
)
```

`user_name`을 Swift 프로퍼티인 `userName`으로 자동 변환해줌

정리하면 다음과 같음

| 전략 | 역할 |
| :--- | :--- |
| `.convertToSnakeCase` | Swift의 `camelCase`를 JSON의 `snake_case`로 변환 |
| `.convertFromSnakeCase` | JSON의 `snake_case`를 Swift의 `camelCase`로 변환 |



<br>
<br>

# 14 `Encoding`과 `Decoding`에서 발생하는 오류

`Encoding`과 `Decoding`은 실패할 수 있기 때문에 `try`를 사용해야 함

```swift
let data = try JSONEncoder().encode(user)
```

```swift
let user = try JSONDecoder().decode(
    User.self,
    from: data
)
```

오류를 처리하려면 `do-catch`를 사용할 수 있음

```swift
do {
    let user = try JSONDecoder().decode(
        User.self,
        from: data
    )

    print(user.name)
} catch {
    print(error)
}
```

Decoding 오류의 주요 원인

* JSON 키 이름과 Swift 프로퍼티 이름이 다름
* JSON 타입과 Swift 타입이 다름
* 필수 프로퍼티가 JSON에 없음
* JSON 형식이 올바르지 않음
* 중첩된 JSON 구조와 Swift 모델 구조가 다름


<br>
<br>





# 15 `Encoding` 오류

<mark>Encoding은 Swift 객체를 JSON으로 변환하는 과정에서 발생하는 오류 !!!!!!!!!!!!!!!!!!!!

```swift
let data = try JSONEncoder().encode(user)
```

대부분의 기본 타입은 자동으로 Encoding할 수 있음

하지만 직접 만든 타입이 `Encodable`을 지원하지 않거나, 커스텀 Encoding 과정에서 문제가 발생하면 오류가 생길 수 있음

```swift
struct User {
    let name: String
}
```

위 타입은 `Encodable`을 채택하지 않았기 때문에 다음 코드를 사용할 수 없음

```swift
let data = try JSONEncoder().encode(user)
```

다음처럼 `Encodable`을 채택해야 함

```swift
struct User: Encodable {
    let name: String
}
```


<br>
<br>





# 16 `Decoding` 오류

Decoding 오류는 JSON Data를 Swift 객체로 변환하지 못했을 때 발생함

예를 들어 JSON이 다음과 같다고 해보자

```json
{
  "name": "Kim",
  "age": "20"
}
```

그런데 Swift 모델에서 `age`를 `Int`로 선언하면 오류가 발생함

```swift
struct User: Decodable {
    let name: String
    let age: Int
}
```

JSON의 `age`는 문자열인 `"20"`이고 Swift에서는 `Int`를 기대하기 때문임

JSON과 Swift 타입이 일치하도록 수정해야 함

```json
{
  "name": "Kim",
  "age": 20
}
```



<br>
<br>




# 17 `Encoding`과 `Decoding` 정리

```text
[Encoding]

Swift 객체
    ↓ JSONEncoder
JSON Data
    ↓
서버로 전송


[Decoding]

서버에서 JSON Data 수신
    ↓ JSONDecoder
Swift 객체
    ↓
앱에서 사용
```

각 요소의 역할

| 요소 | 역할 |
| :--- | :--- |
| `Encodable` | Swift 객체를 외부 데이터로 변환할 수 있도록 함 |
| `Decodable` | 외부 데이터를 Swift 객체로 변환할 수 있도록 함 |
| `JSONEncoder` | Swift 객체를 JSON Data로 변환 |
| `JSONDecoder` | JSON Data를 Swift 객체로 변환 |
| `Data` | 네트워크나 파일 처리에 사용되는 데이터 형태 |
| `CodingKeys` | Swift 프로퍼티와 JSON 키를 연결 |

<br>
<br>

# 18 정리

* `Encoding`은 Swift 객체를 JSON이나 Data로 변환하는 과정
* `Decoding`은 JSON이나 Data를 Swift 객체로 변환하는 과정
* `JSONEncoder`는 Encoding을 담당함
* `JSONDecoder`는 Decoding을 담당함
* `Data`는 네트워크 통신이나 파일 저장에 사용되는 데이터 형태
* 서버에 데이터를 보낼 때는 Swift 객체를 JSON Data로 변환해야 함
* 서버에서 데이터를 받을 때는 JSON Data를 Swift 객체로 변환해야 함
* `keyEncodingStrategy`는 Swift 키를 JSON 키로 변환하는 방식을 설정함
* `keyDecodingStrategy`는 JSON 키를 Swift 키로 변환하는 방식을 설정함
* Encoding과 Decoding은 실패할 수 있으므로 오류 처리가 필요함
* JSON의 키와 타입이 Swift 모델과 일치해야 Decoding할 수 있음

> ### ⇒ `Encoding`은 앱의 Swift 객체를 서버나 저장소에서 사용할 수 있는 데이터로 변환하는 과정이고, `Decoding`은 외부 데이터를 다시 앱에서 사용할 수 있는 Swift 객체로 변환하는 과정 🍀
