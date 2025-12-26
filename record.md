# Record

자바의 **`record` 클래스**는
**데이터만 담는 객체(불변 데이터 캐리어)**를 간결하고 안전하게 만들기 위해 도입된 문법이다.

---

## 1. record가 해결하려는 문제

기존에 DTO, VO를 만들 때 항상 반복하던 코드들:

* `private final` 필드
* 생성자
* `getter`
* `equals()`, `hashCode()`, `toString()`

➡ **record는 이걸 한 줄로 자동 생성**해준다.

---

## 2. 기본 문법

```java
public record Member(Long id, String username, int age) {}
```

이 한 줄은 아래를 **자동으로 생성**한다.

```java
public final class Member {
    private final Long id;
    private final String username;
    private final int age;

    public Member(Long id, String username, int age) { ... }

    public Long id() { return id; }
    public String username() { return username; }
    public int age() { return age; }

    // equals, hashCode, toString
}
```

---

## 3. record의 핵심 특징 (중요)

### ✅ 1) **불변(Immutable)**

* 모든 필드는 `private final`
* setter 존재 불가
* 생성 후 값 변경 불가

👉 값 객체(VO), 응답 DTO에 적합

---

### ✅ 2) **상속 불가**

* `record`는 자동으로 `final`
* 다른 클래스를 상속할 수 없음
* 단, **인터페이스 구현은 가능**

---

### ✅ 3) **접근자 이름이 getter 아님**

```java
member.id();        // O
member.getId();     // X
```

---

### ✅ 4) 생성자 커스터마이징 가능

```java
public record Member(Long id, String username) {
    public Member {
        if (id == null) {
            throw new IllegalArgumentException("id는 필수");
        }
    }
}
```

➡ **검증 로직만 추가 가능**, 필드 재할당은 불가

---

## 4. 언제 record를 쓰는 게 맞나?

### 👍 record가 적합한 경우

* **DTO / Response 객체**
* 값 그 자체가 의미인 객체
* 상태 변경이 없어야 하는 객체
* `equals/hashCode`가 값 기준인 객체

```java
public record LoginRequest(String username, String password) {}
public record MemberResponse(Long id, String nickname) {}
```

---

### ❌ record가 부적합한 경우

* **JPA Entity**
* 상태 변경이 필요한 도메인 객체
* 비즈니스 로직이 많은 클래스

```java
@Entity
public record Member(...) // ❌ JPA와 궁합 안 좋음
```

---

## 5. record vs class 한 줄 요약

| 구분          | class | record        |
| ----------- | ----- | ------------- |
| 목적          | 일반 객체 | **불변 데이터 전달** |
| boilerplate | 많음    | **거의 없음**     |
| 변경 가능       | 가능    | ❌ 불가          |
| 상속          | 가능    | ❌ 불가          |

---

## 6. 실무 기준 결론

* **API 요청/응답 DTO → record 적극 사용**
* **도메인 / 엔티티 → 일반 class**
* “값”이 중심이면 record,
  “행위 + 상태 변경”이 중심이면 class