# 요약 이미지

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYnl7L%2Fbtr7gQddR7P%2FutjuKtJOKONzCkXIEFVGk0%2Fimg.png)

![](https://user-images.githubusercontent.com/33862991/116250387-a46ec800-a7a8-11eb-8368-8e2df28225fa.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4rSai%2Fbtr2DFsUAzZ%2FVkVOL8JMlT07zS3KiN2CL1%2Fimg.png)

# DAO(Data Access Object)

DAO는 실제로 DB의 data에 접근하기 위한 객체입니다.

- 실제로 DB에 접근하여 data를 삽입, 삭제, 조회, 수정 등 CRUD 기능을 수행합니다.
- Service와 DB를 연결하는 고리 역할을 합니다.
- Repository package가 바로 DAO입니다.

```java
@Repository
@RequiredArgsConstructor
public class MemberRepository {

    private final EntityManager em;

    public void save(Member member) {
        em.persist(member);
    }

    public Member findOne(Long id) {
        return em.find(Member.class, id);
    }

    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }

    public List<Member> findByName(String name) {
        return em.createQuery("select m from Member m where m.name =:name", Member.class)
                .setParameter("name", name)
                .getResultList();
    }
}
```

# Entity
- Entity 클래스는 실제 DataBase의 테이블과 1 : 1로 매핑 되는 클래스
- DB의 테이블내에 존재하는 컬럼만을 속성(필드)으로 가져야 한다.
- Entity 클래스는 상속을 받거나 구현체여서는 안되며, 최대한 외부에서 Entity 클래스의 getter method를 사용하지 않도록 해당 클래스 안에서 필요한 로직 method을 구현 해야하고, Domain Logic만 가지며 Presentation Logic을 가지고 있어서는 안된다.
- Entity를 작성할 때 setter를 무분별하게 사용하면 객체(Entity)의 값을 변경할 수 있으므로 객체의 일관성을 보장할 수 없다.
  객체의 일관성을 유지할 수 있어야 유지 보수성이 올라가기 때문에 setter를 사용해서는 안되며, 객체의 생성자에 값들을 넣어줌으로써 setter 사용을 줄일 수 있다.

```java
@Entity
@Getter
@Setter
public class Member {

    @Id
    @GeneratedValue
    private Long id;

    private String name;
	private int age;

}
```

# DTO(Data Transfer Object)

DTO는 계층 간 데이터 교환을 하기 위해 사용하는 객체로, DTO는 로직을 가지지 않는 순수한 데이터 객체(Java Beans)입니다.

- DTO는 즉, getter/setter 메서드만 가진 클래스를 의미합니다.
- DB에서 데이터를 얻어서 Service나 Controller 등으로 보낼 때 사용합니다.
- 즉 엔티티를 DTO 형태로 변환한 후 사용합니다.
```java
    @Getter
    @Setter
    static class ResponseDto {
        private String name;
        private String result = "결과입니다.";

        public ResponseDto(Member member) {
            name = member.getName();
        }
    }
```

# Controller
```java
    @PostMapping("api/useDTO")
    @ResponseBody
    public ResponseDto useDTO(@RequestParam String name, @RequestParam Long id) {
    	Member findMember = memberService.findOne(id);
        return new ResponseDto(findMember);
    }
    
    @PostMapping("api/notUseDTO")
    @ResponseBody
    public FindMember notUseDTO(@RequestParam String name, @RequestParam Long id) {
        Member findMember = memberService.findOne(id);
        return new FindMember(findMember);
    }
```
useDTO 메서드는 Entitiy의 결과를 DTO로 변환하여 원하는 결과인 name과 result만 클라이언트에게 반환합니다. 하지만 notUseDTO는 Member엔티티의 모든 정보 즉, 엔티티 그 자체를 클라이언트에게 반환합니다.

현재 예시는 DTO를 Response에만 이용했지만 Request역시 적용할 수 있습니다. 

# VO (Value Object)

- VO는 Value Object의 약자로 값 자체를 표현하는 객체.
- DTO와 유사하지만 DTO는 setter를 가지고 있어 값이 변할 수 있다.
- VO는 getter() 메소드를 포함해서, 이외의 비즈니스 로직도 포함할 수 있다.
사용하는 도중에 변경 불가하여 setter() 메소드는 가지지 않아. **오로지, 읽기 기능만 가능하다.(**read-Only)
- VO는 두 객체의 모든 필드 값들이 동일하면 두 객체는 같다! 가 핵심 정의이다. 그렇기 때문에 완전히 값 자체 표현 용도로만 사용하는 게 목적이라면, 두 객체의 모든 필드 값들이 모두 같으면 같은 객체이도록 만드는 것(equals() 와 hashCode()의 오버라이딩)이 중요하지, 메소드는 어떤 메소드가 있든 말든 상관 없다.

```java
// VO예제
// 값을 표현만 하기 때문에 setter()메소드는 사용하지 않는다.
public class VoEx {
    static class Money{
        private final int value;

        Money(int value) {
            this.value = value;
        }

        public int getValue() {
            return value;
        }
				
	// 사용자 생성 메소드이지만 setter형태의 메소드가 아니므로 무관함
        public String printMoney(){
            return this.value + "원";
        }
					
	// 두 객체의 모든 필드 값들이 모두 같으면 같은 객체이도록 만들기 위한 오버라이딩
		@Override
        public int hashCode() {
            return Objects.hash(value);
        }

        @Override
        public boolean equals(Object obj) {
            if(this == obj)
                return true;
            if(obj == null || getClass() != obj.getClass())
                return false;
            Money money = (Money) obj;
            if(value == money.value)
                return true;
            else
                return false;
        }
    }
}
```
