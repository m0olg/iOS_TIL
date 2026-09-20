## DI
# 01 `DI`란?
→ Dependency Injection의 약자로 클래스 내부에서 필요한 객체의 인스턴스를, 클래스 내부에서 생성하는 게 아니라 외부에서 생성한 뒤 이니셜라이저 또는 setter를 통해 내부로 주입받는 것 \
이 때 이니셜 라이저의 타입은 프로토콜을 활용해서 내부에선 프로토콜 메서드를 사용

DI는 의존성을 클래스에 **주입**시키는 것이고 **의존성 분리**의 조건을 만족해야함

# 02 의존성
클래스 A, B가 있다고 하고 클래스 B의 값이 바뀔 때 클래스 A의 값도 함께 바뀌게 된다면 이 때 **클래스 A는 B에게 의존성을 갖는다**고 말함

> 발로란트를 예시로 들면 ⬇️⬇️
```swift
class Agent {
    let name : String
    init (name : String) {
        self.name = name
    }
}

// 에이전트 클래스
class Agent {
    let name : String
    init (name : String) {
        self.name = name
    }
}

// 플레이어 클래스
class Player {
    let xqPoint : XqPoint
    let xpPoint : XpPoint

    // 클래스 내부에서 XqPoint, XpPoint 인스턴스를 생성
    init() {
        self.xqPoint = XqPoint (name : "클로브")
        self.xpPoint = XpPoint (name : "제트")
    }
}

let 
```


# 03 주입
# 04 의존성 분리
# 05 `IOC COntainer`