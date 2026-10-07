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






## 05 일반 텍스트 필드도 함께 보내기

사진과 리뷰 내용을 한 요청으로 보내는 경우, 서버와 필드 이름을 먼저 맞춰야 함

#### 예를 들어 서버 명세가 아래와 같다고 가정 !!

| 필드 이름 | 값 |
| :--- | :--- |
| `content` | 리뷰 내용 |
| `rating` | 별점 |
| `image` | 이미지 파일 |

이때 `content`와 `rating`도 `boundary`로 구분된 각각의 파트로 본문에 추가

```text
Content-Disposition: form-data; name="content"

조용하고 공부하기 좋았어욤
```

필드 이름은 예시일 뿐이므로 실제 구현에서는 **백엔드 API 명세에 적힌 이름과 파일 개수·형식·크기 제한**을 따라야 한다!!!!!






## 06 구현할 때 주의할 점

- `Content-Type` 헤더에 **boundary를 포함**해야 함
- 각 파트 사이에 `\r\n` 줄바꿈을 넣어야 함
- 마지막 파트 뒤에는 종료용 boundary를 추가해야 함
- 필드 이름과 파일 이름은 서버 명세와 정확히 맞춰야 함
- 이미지 형식이 JPEG인지 PNG인지 확인하고 그에 맞는 `Content-Type`을 설정
- 이미지 크기 제한이나 업로드 가능한 파일 개수는 서버 정책을 확인해야됨
- 인증이 필요하면 서버에서 정한 인증 헤더도 추가해야 함
- 실제 서비스 코드에서는 `!`로 강제 언래핑하기보다 데이터 변환 오류도 처리하는 편이 안전






## 07 언제 쓸까?

예를 들어 내가 지금 하는 프젝인 MoSS에 리뷰 사진 기능이 들어간다면 이런 데이터를 업로드할 때 사용할 수 있음

```text
리뷰 내용 + 별점 + 소음 체감 + 사진
```

즉, 현재는 **사진 업로드 기능이 포함될 경우 필요한 방식**으로 알아두면 됨 \
텍스트 리뷰만 구현한다면 `multipart/form-data`가 필요하지 않을 수 있음






# 08 정리

- `multipart/form-data`는 한 요청에 여러 데이터 파트를 담는 형식
- 텍스트뿐 아니라 이미지 같은 파일도 함께 보낼 수 있음
- 요청의 각 파트는 `boundary`로 구분함
- iOS에서는 `URLSession` 요청에 multipart 본문을 구성해 전송할 수 있음
- 필드 이름, 파일 크기·형식, 인증 방식은 백엔드 API 명세를 따라야 함
- MoSS에서는 리뷰 사진 업로드 등에 활용할 수 있음