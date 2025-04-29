### 문제 상황

JPA와 Hibernate를 사용하다 보면 다음과 같은 오류 메시지를 마주칠 수 있습니다.

```groovy
org.hibernate.LazyInitializationException: could not initialize proxy - no Session
```

해당 오류는 Lazy Loading이 활성화된 엔티티를 조회할 때, Hibernate 세션이 닫힌 상태에서 프록시 객체를 초기화하려고 할 때 발생합니다. 즉, 데이터베이스와의 연결이 끊어진 후에 지연 로딩을 시도하기 때문에 문제가 발생하는 것입니다.

### 오류 발생 시나리오

이 오류가 발생하는 대표적인 시나리오는 다음과 같습니다.

1. 컨트롤러 또는 서비스 레이어에서 JPA Repository를 통해 엔티티를 조회합니다.
2. 엔티티의 연관 객체가 Lazy Loading으로 설정되어 있습니다.
3. 데이터베이스 세션이 종료된 후, 연관 객체를 접근하려고 할 때 오류가 발생합니다.

예시 코드를 통해 살펴보겠습니다.

```java
@Entity
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    private Team team;
}
```

```java
@Service
public class MemberService {

    @Autowired
    private MemberRepository memberRepository;

    public String getTeamName(Long memberId) {
        Member member = memberRepository.findById(memberId).orElseThrow();
        return member.getTeam().getName();
    }
}
```

1. Member 엔티티의 Team 연관 객체는 **지연 로딩**으로 설정되어 있습니다.
2. getTeam().getName()을 호출할 때, 데이터베이스 세션이 열려 있어야 프록시 객체를 초기화할 수 있습니다.
3. 세션이 이미 닫힌 상태라면 LazyInitializationException이 발생합니다.

### 해결 방법

이 문제를 해결하는 방법은 여러 가지가 있습니다. 상황에 따라 적절한 방법을 선택해야 합니다.

1. **즉시 로딩 (EAGER)으로 변경**
    
    가장 간단한 방법으로 연관 객체의 fetch 전략을 즉시 로딩(EAGER)로 변경하는 것입니다.
    
    ```java
    @ManyToOne(fetch = FetchType.EAGER)
    private Team team;
    ```
    
    하지만 이 방법은 **비효율적인 쿼리 문제**를 일으킬 수 있으므로 주의가 필요합니다. 연관 객체가 많을 경우 **N+1 문제**를 유발할 수 있습니다.
    
2. **Fetch Join 사용**
    
    **JPQL** 또는 **Spring Data JPA**에서 **Fetch Join**을 사용하여 연관 객체를 한 번에 조회하는 방법입니다.
    
    ```java
    @Query("SELECT m FROM Member m JOIN FETCH m.team WHERE m.id = :id")
    Optional<Member> findMemberWithTeam(@Param("id") Long id);
    ```
    
    ```java
    public String getTeamName(Long memberId) {
        Member member = memberRepository.findMemberWithTeam(memberId).orElseThrow();
        return member.getTeam().getName();
    }
    ```
    
    이 방법은 **즉시 로딩(EAGER)**의 단점을 피하면서, **Lazy Loading** 문제를 해결할 수 있습니다.
    
3. **@Transactional 사용**
    
    Hibernate 세션이 **트랜잭션 범위 내**에서 유지되기 때문에, 트랜잭션을 선언해 세션을 유지시키는 방법입니다.
    
    ```java
    @Transactional
    public String getTeamName(Long memberId) {
        // 트랜잭션이 열린 상태에서 데이터 조회
        Member member = memberRepository.findById(memberId).orElseThrow();
        
        // Lazy Loading 초기화 (세션이 열려있으므로 문제없음)
        return member.getTeam().getName();
    }
    ```
    
    **Lazy Loading**이 필요한 연관 객체(member.getTeam())를 트랜잭션 범위 내에서 접근하므로 **Hibernate 세션이 열려있어 프록시 객체가 초기화**됩니다.
    
4. **DTO로 변환하여 반환**
    
    서비스 레이어에서 **DTO(Data Transfer Object)**로 변환하여 필요한 데이터만 반환하는 방식입니다.
    
    ```java
    public class MemberDTO {
        private String name;
        private String teamName;
    
        public MemberDTO(String name, String teamName) {
            this.name = name;
            this.teamName = teamName;
        }
    }
    ```
    
    ```java
    public MemberDTO getMemberWithTeam(Long memberId) {
        Member member = memberRepository.findById(memberId).orElseThrow();
        String teamName = member.getTeam().getName();
        return new MemberDTO(member.getName(), teamName);
    }
    ```
    
    이 방법은 **LazyInitializationException**을 피할 수 있습니다. 또한, 클라이언트에 필요한 데이터만 전달할 수 있습니다.
    

### 정리

| **방법**           | **장점**                            | **단점**                  |
| ---------------- | --------------------------------- | ----------------------- |
| 즉시 로딩(EAGER)     | 간단한 해결 방법                         | N+1 문제 발생 가능            |
| Fetch Join 사용    | 효율적인 쿼리                           | 복잡한 JPQL 작성 필요          |
| DTO 변환           | LazyInitializationException 완벽 해결 | 코드 작성이 다소 번거로움          |
| Transactional 사용 | 간단하고 해결 방법                        | 불필요한 트랜잭션이 열리면 성능 저하 발생 |

### **실제 프로젝트에서의 적용 하기**

1. 일반적으로는 **Fetch Join** 사용하는 것이 좋습니다.
    - **한 번의 쿼리로 필요한 연관 데이터를 모두 가져올 수 있어 성능이 향상**됩니다.
    - **N+1 문제**가 발생할 가능성이 있는 조회 쿼리에 사용합니다.
2. **읽기 전용 쿼리에는** @Transactional(readOnly = true)**를 사용**합니다.
    - 트랜잭션이 열리면서 Hibernate 세션이 유지되므로, **Lazy 로딩 문제를 방지**할 수 있습니다.
    - **쓰기 락 방지** 및 **성능 최적화** 효과가 있습니다.
    - 단, 불필요한 트랜잭션이 열리면 **성능 저하** 발생할 수 있습니다.
3. **복잡한 조회에는 DTO 변환 사용**
    - **복잡한 조건**이 필요한 경우에 사용합니다.
    - 필요한 데이터만 가져와서 **불필요한 데이터 조회를 줄일** 수 있습니다.