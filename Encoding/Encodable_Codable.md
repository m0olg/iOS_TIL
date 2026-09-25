## Encoding & Decoding

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