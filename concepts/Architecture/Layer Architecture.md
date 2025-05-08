# Layered Architecture

## **개요**

Layered Architecture는 소프트웨어 시스템을 여러 계층으로 나누고, 각 계층이 **명확한 역할과 책임**을 갖도록 설계하는 아키텍처입니다.

---

## **1. 개념 정의**

- **Layered Architecture**는 애플리케이션을 기능별로 분리된 계층으로 구성하여, **관심사의 분리(Separation of Concerns)** 를 실현하는 아키텍처 패턴입니다.
- 각 계층은 자신보다 **하위 계층에만 의존**하며, 계층 간 **역할과 책임을 명확히 구분**함으로써 **모듈화**, **테스트 용이성**, **유지보수성**을 향상시킵니다.
- 일반적으로 MVC 또는 4계층 구조(Presentation, Application, Domain, Infrastructure)로 구성됩니다.
- **소프트웨어 아키텍처 패턴** 중 하나로, DDD나 클린 아키텍처와도 밀접한 관련이 있습니다.

---

## **2. 동작 원리 (How It Works)**

Layered Architecture는 다음과 같은 계층으로 구성되며, 각 계층은 **하위 계층의 기능만을 사용**하고, **상위 계층에는 의존하지 않습니다**:

1. **Presentation Layer (UI/Controller)**
   - 사용자 요청을 수신하고 Application Layer에 전달
   - 요청/응답 데이터를 DTO로 처리
2. **Application Layer (Service)**
   - 유즈케이스 로직을 구현
   - 도메인 객체를 호출하여 비즈니스 로직 수행
3. **Domain Layer (Model)**
   - 핵심 비즈니스 로직을 정의
   - Entity, Value Object, Domain Service 포함
4. **Infrastructure Layer**
   - 데이터베이스, 외부 API, 메시징 등 기술 구현 담당
   - Repository 구현체 및 외부 시스템 Adapter 포함

> 예시 흐름: Controller → Service → Domain Model → Repository → DB

---

## **3. 장점 및 한계**

### **✅ 장점**

- 계층 간 **관심사 분리**가 명확함
- 특정 계층의 변경이 다른 계층에 미치는 영향이 적어 **유지보수가 쉬움**
- **계층 단위 테스트 가능** → 테스트 용이성 향상
- 도메인 로직과 기술 코드가 분리되어 **가독성 증가**

### **⚠️ 한계**

- 소규모 프로젝트에서는 **과도한 설계(오버엔지니어링)** 가 될 수 있음
- 계층 간 DTO ↔ Entity 변환 등으로 인해 **보일러플레이트 코드 증가**
- 모든 요청이 위→아래→위로 흐르기 때문에 **복잡한 트랜잭션 처리에 비효율적일 수 있음**
- 계층 분리가 개발 속도를 저하시킬 수 있음

---

## **4. 관련 아키텍처와의 비교 (선택 사항)**

| **항목**      | **Layered Architecture** | **Clean Architecture** | **Hexagonal Architecture** |
| ------------- | ------------------------ | ---------------------- | -------------------------- |
| 중심 구조     | 계층 중심                | 도메인 중심            | 포트와 어댑터 중심         |
| 의존성 방향   | 위 → 아래                | 외부 → 내부            | 외부 → 내부                |
| 도메인 분리   | 선택적                   | 필수                   | 필수                       |
| 테스트 용이성 | 보통                     | 높음                   | 높음                       |
| 적용 난이도   | 낮음                     | 중간~높음              | 중간                       |

---

## **5. 사용 예 및 적용 시점**

- 전통적인 웹 애플리케이션 (Spring MVC, Laravel, Django 등)
- **복잡한 비즈니스 로직이 필요한 B2B 서비스**
- 유지보수와 협업이 중요한 **중대형 프로젝트**
- DDD 기반 설계를 위한 **기반 아키텍처로 활용 가능**

### 실제 사용 예시 (Spring 기준)

```
com.example.user
├── controller      ← UI Layer (REST API 진입점)
├── service         ← Application Layer (유즈케이스 처리)
├── model           ← Domain Layer (Entity, VO, 도메인 로직)
├── repository
│   ├── UserRepository      ← 인터페이스 (도메인 계층)
│   └── JpaUserRepository   ← 구현체 (인프라 계층)
└── external        ← 외부 시스템 연동
```

## 6. 참고 자료

- [Microsoft – Layered Architecture Guide](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/layered)
- [[DDD 시작하기 - https://domain-driven-design.org/]](https://domain-driven-design.org/)
