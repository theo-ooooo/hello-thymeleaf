# 🌿 Thymeleaf 기본 정리 (스프링 MVC 2편)

## 📝 텍스트 출력

### ✔️ 기본 텍스트 출력
```html
<span th:text="${data}">기본 출력</span>
```

### ✔️ `text` vs `utext`
- `th:text` → HTML Escape (XSS 방지)
- `th:utext` → HTML 태그 적용됨 (주의)

---

## 🔍 표준 표현식 구문 (SpringEL)

```html
<span th:text="${user.username}">표현식</span>
```

- `${...}` : 변수 접근
- `#...` : 유틸 객체나 기본 객체 접근

---

## 📦 변수 - SpringEL
```html
<span th:text="${user.username}">user.username</span>
```

---

## ⚙️ 기본 객체들 (Spring Boot 3.x 기준)

> ⚠️ Spring Boot 3.x부터는 일부 Servlet 관련 기본 객체들이 더 이상 자동 제공되지 않습니다.

### ❌ 더 이상 지원되지 않는 기본 객체들

| 객체 이름 | 설명 |
|-----------|------|
| `#request` | `HttpServletRequest` (미지원) |
| `#response` | `HttpServletResponse` (미지원) |
| `#session` | `HttpSession` (미지원) |
| `#servletContext` | `ServletContext` (미지원) |

### ✅ 계속 지원되는 객체들

| 객체 | 설명 |
|------|------|
| `#ctx` | ApplicationContext |
| `#vars` | 변수들 |
| `#locale` | 현재 로케일 |

> Servlet 객체를 사용하고 싶다면 직접 Model에 담아 전달해야 합니다.

```java
@GetMapping("/test")
public String test(HttpServletRequest request, Model model) {
    model.addAttribute("req", request);
    return "test";
}
```

```html
<p th:text="${req.method}">요청 메서드</p>
```


---

## 🛠️ 유틸리티 객체

| 객체 | 설명 |
|------|------|
| `#dates` | 날짜 유틸 |
| `#calendars` | 캘린더 유틸 |
| `#numbers` | 숫자 유틸 |
| `#strings` | 문자열 유틸 |
| `#objects` | 객체 유틸 |
| `#bools` | 불리언 유틸 |
| `#arrays` | 배열 유틸 |
| `#lists` | 리스트 유틸 |
| `#sets` | Set 유틸 |
| `#maps` | Map 유틸 |

---

## 🔗 링크 URL
```html
<a th:href="@{/hello}">hello</a>
<a th:href="@{/item/{id}(id=${item.id})}">item</a>
```

---

## 🔢 리터럴, 연산
- 리터럴: `'hello'`, `10`, `true`, `null`
- 연산자: `+`, `-`, `*`, `/`, `%`, `==`, `!=`, `>`, `<`, `>=`, `<=`, `and`, `or`, `!`, `?:`, `?:`, `??`, `|`, `^`

---

## 🏷️ 속성 설정

```html
<input type="text" th:value="${user.name}" />
<option th:selected="${user.gender == 'M'}">남자</option>
```

---

## 🔁 반복

기본 문법:
```html
<tr th:each="user, userStat : ${users}">
  <td th:text="${user.name}">이름</td>
  <td th:text="${userStat.index}">인덱스</td>
</tr>
```

### 🔄 반복 상태 변수 (`Stat`)

| 속성    | 설명                 |
|---------|----------------------|
| index   | 0부터 시작하는 인덱스 |
| count   | 1부터 시작하는 인덱스 |
| size    | 전체 아이템 수        |
| even    | 짝수 여부             |
| odd     | 홀수 여부             |
| first   | 첫 번째 요소 여부     |
| last    | 마지막 요소 여부     |

---

## 🧩 조건부 평가

```html
<span th:if="${user.active}">활성</span>
<span th:unless="${user.active}">비활성</span>
```

---

## 💬 주석 / 블록

```html
<!--/* 주석 처리됨 */-->
<th:block th:if="${isAdmin}">
  관리자용 콘텐츠
</th:block>
```

---

## 🧠 자바스크립트 인라인

```html
<script th:inline="javascript">
  let user = [[${user}]];
</script>
```

---

## 🧱 템플릿 레이아웃

### 조각 사용 (Fragment)
```html
<div th:replace="fragments/header :: header"></div>
```
---

