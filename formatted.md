# formatted() 메서드

`formatted` 는 **Java 15부터 정식 추가된 문자열 포맷팅 메서드**입니다.
역할은 **`String.format()`의 인스턴스 메서드 버전**이라고 보면 됩니다.

---

## 1. 기본 개념

```java
String result = "%d번 회원이 생성되었습니다.".formatted(10);
```

⬇️ 결과

```text
10번 회원이 생성되었습니다.
```

즉,

```java
"%d번 회원이 생성되었습니다.".formatted(member.getId())
```

은 내부적으로 아래와 **완전히 같은 동작**을 합니다.

```java
String.format("%d번 회원이 생성되었습니다.", member.getId())
```

---

## 2. 왜 나온 메서드인가?

기존 방식:

```java
String msg = String.format("%d번 회원이 생성되었습니다.", member.getId());
```

`formatted()` 방식:

```java
String msg = "%d번 회원이 생성되었습니다.".formatted(member.getId());
```

👉 **문자열이 주체가 되어서 읽기 좋아짐**
👉 메서드 체이닝에 유리
👉 가독성 개선 목적

---

## 3. 네가 쓴 코드 해석

```java
return new RsData<>(
    "201-1",
    "%d번 회원이 생성되었습니다.".formatted(member.getId()),
    member
);
```

정확히 의미하면:

* `%d` → `member.getId()` 값으로 치환
* 최종 메시지 예:

  ```
  3번 회원이 생성되었습니다.
  ```

---

## 4. 포맷 문자 정리 (핵심만)

| 포맷     | 의미      | 예시      |
| ------ | ------- | ------- |
| `%d`   | 정수      | `10`    |
| `%s`   | 문자열     | `"kim"` |
| `%f`   | 실수      | `3.14`  |
| `%.2f` | 소수점 2자리 | `3.14`  |

```java
"%s님 나이는 %d세입니다.".formatted("철수", 20);
```

---

## 5. 주의사항 (중요)

### ❗ 개수 불일치 시 런타임 에러

```java
"%d %d".formatted(1); // ❌ IllegalFormatException
```

### ❗ 타입 불일치도 에러

```java
"%d".formatted("abc"); // ❌
```

컴파일 타임이 아니라 **런타임 에러**라서 주의 필요

---

## 6. 언제 쓰는 게 좋나?

✅ 이런 경우 추천

* 응답 메시지, 로그 메시지
* RsData / ApiResponse 메시지
* 문자열이 주체가 되는 코드

❌ 이런 경우는 비추

* 단순 문자열 결합만 필요할 때

```java
"회원 ID: " + member.getId(); // 이게 더 단순
```

---

## 7. 한 줄 요약

> `formatted()`는
> **String.format()을 더 읽기 좋게 만든 자바 15+ 문자열 포맷 메서드**다.
