## Swinject

<br>

# 01 `Swinject`란?
→ **Swinject**는 Swift에서 사용하는 **의존성 주입(DI) 프레임워크**

객체를 직접 생성하고 연결하는 대신 Swinject가 객체의 생성과 의존성 주입을 관리해줌 (교체와 테스트가 쉬워짐 !!!!)

> Swinject = Swift에서 사용하는 IoC Container

<br>
<br>

# 02 Swinject를 사용하는 이유

직접 객체를 생성하면 코드가 다음처럼 복잡해질 수 있음

```swift
let agent = Agent(name: "체임버")
let player = Player(agent: agent)
```

객체가 많아지면 생성과 연결을 한 곳에서 관리하기 어려워짐

↳ `Swinject`를 사용하면 객체를 등록하고 필요한 곳에서 자동으로 주입받을 수 있음

<br>
<br>

# 03 설치

`[Swift Package Manager]`에서 다음 저장소를 추가

```text
https://github.com/Swinject/Swinject.git
```

설치 후 코드에서 `import`하기

```swift
import Swinject
```

<br>
<br>

# 04 예시

먼저 프로토콜과 클래스를 정의

```swift
protocol AgentType {
    var name: String { get }
}

class Agent: AgentType {
    let name: String

    init(name: String) {
        self.name = name
    }
}

class Player {
    let agent: AgentType

    init(agent: AgentType) {
        self.agent = agent
    }
}
```

`Player`는 구체적인 `Agent`가 아니라 `AgentType`에 의존하고 있음

<br>
<br>

# 05 객체 등록

Swinject의 `Container`에 객체를 등록함

```swift
import Swinject

let container = Container()

container.register(AgentType.self) { _ in
    Agent(name: "체임버")
}
```

이 코드는 다음을 의미 ⬇️⬇️

> `AgentType`이 필요하면 `Agent(name: "체임버")`를 만들어서 제공 ㄱㄱ

<br>
<br>

# 06 의존성 주입 등록

`Player`도 컨테이너에 등록해야함

```swift
container.register(Player.self) { resolver in
    let agent = resolver.resolve(AgentType.self)!
    return Player(agent: agent)
}
```

여기서 `resolver`는 컨테이너에 등록된 객체를 찾아주는 역할

```swift
resolver.resolve(AgentType.self)
```

이 코드는 등록된 `AgentType` 객체를 가져옴

<br>
<br>

# 07 객체 꺼내기

이제 `Player`를 직접 생성하지 않고 컨테이너에서 가져옴

```swift
let player = container.resolve(Player.self)!

print(player.agent.name) // 체임버
```

### 🍀 `Swinject`가 내부적으로 다음 과정을 처리
1. Player가 필요함
2. Player의 생성자에 AgentType이 필요함
3. 등록된 Agent를 찾음
4. Agent를 Player에 주입함
5. 완성된 Player를 반환함

<br>
<br>

# 08 전체 코드

```swift
import Swinject

protocol AgentType {
    var name: String { get }
}

class Agent: AgentType {
    let name: String

    init(name: String) {
        self.name = name
    }
}

class Player {
    let agent: AgentType

    init(agent: AgentType) {
        self.agent = agent
    }
}

let container = Container()

container.register(AgentType.self) { _ in
    Agent(name: "체임버")
}

container.register(Player.self) { resolver in
    let agent = resolver.resolve(AgentType.self)!
    return Player(agent: agent)
}

let player = container.resolve(Player.self)!

print(player.agent.name) // 체임버
```

<br>
<br>

# 09 객체 생명주기 설정

> `Swinject`는 객체를 어떻게 보관할지도 설정할 수 있음

### ① 매번 새로 생성

```swift
container.register(AgentType.self) { _ in
    Agent(name: "체임버")
}
```

등록된 객체를 요청할 때마다 새로운 객체를 생성

### ② 하나의 객체를 계속 사용

```swift
container.register(AgentType.self) { _ in
    Agent(name: "체임버")
}
.inObjectScope(.container)
```

컨테이너가 살아 있는 동안 같은 객체를 사용

### 🐔 이 방식은 보통 다음과 같은 객체에 사용 !
- 네트워크 매니저
- 데이터베이스 매니저
- 로그인 정보 관리 객체
- 앱 전체에서 하나만 필요한 객체

<br>
<br>

# 10 `Swinject`와 `DIP`

`Swinject`는 `DIP`와 함께 사용하면 효과적

```swift
protocol AgentType {
    var name: String { get }
}
```

```swift
class Player {
    let agent: AgentType

    init(agent: AgentType) {
        self.agent = agent
    }
}
```

`Player`는 구체적인 `Agent`가 아니라 `AgentType`이라는 추상화에 의존

Swinject는 실제로 어떤 객체를 넣을지 관리

```swift
container.register(AgentType.self) { _ in
    Agent(name: "체임버")
}
```

### 정리하면 ⬇️⬇️

- `Player`가 `AgentType`에 의존함 → **DIP**
- `Agent`를 외부에서 `Player`에 넣음 → **DI**
- Swinject가 객체 생성과 연결을 관리함 → **IoC Container**

<br>
<br>

# 11 테스트용 객체 교체

Swinject를 사용하면 실제 객체 대신 테스트용 객체를 등록할 수 있음

```swift
class MockAgent: AgentType {
    let name = "테스트 에이전트"
}
```

테스트할 때 등록 내용을 바꿈

```swift
container.register(AgentType.self) { _ in
    MockAgent()
}

container.register(Player.self) { resolver in
    let agent = resolver.resolve(AgentType.self)!
    return Player(agent: agent)
}

let testPlayer = container.resolve(Player.self)!

print(testPlayer.agent.name) // 테스트 에이전트
```

`Player`의 <mark>코드를 수정하지 않고도 실제 에이전트를 테스트용 에이전트로 교체 가능</mark>

<br>
<br>

# 12 핵심 정리

- **Swinject**는 Swift의 DI 프레임워크
- 내부적으로 **IoC Container** 역할을 함
- `register`로 객체 생성 방법을 등록
- `resolve`로 필요한 객체를 가져옴
- 프로토콜과 함께 사용하면 DIP를 적용 가능
- 테스트할 때 실제 객체를 Mock 객체로 쉽게 교체 가능

> ### ⇒ **Swinject는 객체 생성과 의존성 연결을 대신 관리해주는 Swift용 DI 프레임워크 ✨**