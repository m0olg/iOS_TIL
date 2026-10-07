## multipart 기본 개념




## 01 `multipart/form-data`란?

→ `multipart/form-data`는 **텍스트와 파일을 한 번의 HTTP 요청에 함께 담아 보내기 위한 데이터 형식**

예를 들어 MoSS에서 리뷰 내용과 사진을 같이 등록할 때 사용할 수 있음

```text
리뷰 내용 + 별점 + 사진 파일
```

JSON은 글자나 숫자 같은 데이터를 주고받을 때 편하고 \
`multipart/form-data`는 이미지·동영상 같은 **파일과 일반 데이터를 함께 보낼 때** 자주 사용함 !






## 02 왜 `multipart`라고 부를까?

요청 본문을 여러 부분(part)으로 나누어 각각의 데이터를 담기 때문임

```text
요청 본문
├── 리뷰 내용
├── 별점
└── 이미지 파일
```

각 부분은 `boundary`라는 구분 문자열로 나뉨 \
실제 HTTP 요청에서는 대략 다음처럼 표현............. 

<sub><sub>어렵다...................나중에다시보기⭐⭐⭐⭐

```text
--boundary123
Content-Disposition: form-data; name="content"

조용하고 공부하기 좋았어욤
--boundary123
Content-Disposition: form-data; name="rating"

5
--boundary123
Content-Disposition: form-data; name="image"; filename="review.jpg"
Content-Type: image/jpeg

(이미지 데이터)
--boundary123--
```

- `name`: 서버가 정한 필드 이름
- `filename`: 파일 이름
- `Content-Type`: 해당 부분의 데이터 종류
- `boundary`: 각 부분을 구분하는 값






## 03 JSON과 `multipart/form-data` 비교

| 형식 | 주로 보내는 데이터 | 예시 |
| :--- | :--- | :--- |
| `application/json` | 글자, 숫자, 배열, 객체 | 리뷰 내용, 별점, 매장 ID |
| `multipart/form-data` | 파일과 일반 데이터를 함께 보낼 때 | 리뷰 내용 + 사진 파일 |

사진 없이 리뷰 텍스트만 보낸다면 JSON으로 충분함. 사진 파일까지 함께 보낼 때는 서버 API가 지원한다면 `multipart/form-data`를 사용






## 04 iOS에서는 어떻게 보내나요?

iOS에서는 `URLSession`으로 요청을 만들고, 요청 본문에 `multipart/form-data` 형식의 데이터를 직접 구성해 담을 수 있음

```swift
import Foundation

func uploadImage(
    imageData: Data,
    to url: URL
) async throws {
    var request = URLRequest(url: url)
    request.httpMethod = "POST"

    let boundary = UUID().uuidString
    request.setValue(
        "multipart/form-data; boundary=\(boundary)",
        forHTTPHeaderField: "Content-Type"
    )

    var body = Data()

    // 파일 데이터 앞에 붙는 헤더
    body.append("--\(boundary)\r\n".data(using: .utf8)!)
    body.append(
        "Content-Disposition: form-data; name=\"image\"; filename=\"review.jpg\"\r\n"
            .data(using: .utf8)!
    )
    body.append("Content-Type: image/jpeg\r\n\r\n".data(using: .utf8)!)

    // 실제 이미지 데이터
    body.append(imageData)
    body.append("\r\n".data(using: .utf8)!)

    // 본문 종료 표시
    body.append("--\(boundary)--\r\n".data(using: .utf8)!)

    request.httpBody = body

    let (_, response) = try await URLSession.shared.data(for: request)

    guard let httpResponse = response as? HTTPURLResponse,
          (200..<300).contains(httpResponse.statusCode) else {
        throw URLError(.badServerResponse)
    }
}
```

위 예시는 이미지 파일 하나만 보내는 형태임 \
리뷰 내용이나 별점도 같이 보낼 땐 이미지 부분과 같은 방식으로 텍스트 부분을 추가하면 됨






