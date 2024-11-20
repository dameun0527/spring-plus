# SPRING PLUS WEEKEND ASSIGNMENT

### 과제 수행 내용

#### 필수 기능 Lv 1.

1. 코드 개선 퀴즈 - @Transactional의 이해

   1. 문제 상황: `public ResponseEntity<TodoSaveResponse> saveTodo(){}` 호출 시 에러 발생
   2. 에러 원인: 
      - **[Connection is read-only.]**: 그 핵심 로직 코드인 `saveTodo()`메서드에서 데이터를 저장하는 과정 중 상위 메서드에 적용된 `@Transactional(readOnly=true)`가 하위 메서드로 전파되어
      - **[Queries leading to data modification are not allowed]**:수정이 불가능하다고 하고 있다.
   3. 해결 방안: `saveTodo()`메서드에 직접 `@Transactional`을 추가해줌

    
2. 코드 추가 퀴즈 - JWT의 이해

    - User 정보에 nickname 추가하기(JWT에서 정보를 꺼내어 화면에 보여주기)
      - User 엔티티에 nickname 필드 추가
      - JWT에서 정보를 꺼내야한다 = 토큰에 정보를 담는다로 이해 -> 회원 가입 및 로그인에 쓰이는 메인 로직 `createToken()`에 nickname 추가
      - nickname 추가가 필요한 dto 수정(회원 가입 시 닉네임 입력 -> SignupRequest) + 수정 필요한 곳 객체 및 변수 수정
      - User 조회 메서드`getUser()`가 엔티티에 저장된 닉네임 값을 불러오게 수정
      - 전달하는 dto(UserResponse) nickname 및 생성자 추가
      - Postman으로 테스트 완료


3. 코드 개선 퀴즈 - AOP의 이해

엔드포인트가 잘못 잡혀있었고, `changeUserRole()` 메서드 실행 전에 로그가 찍히도록 `@BEFORE` 어노테이션을 달았다.

4. 테스트 코드 퀴즈 - 컨트롤러 테스트의 이해

'todo_단건_조회_시_todo가_존재하지_않아_예외가_발생한다()'가 의도라면 예외를 일부러 발생시켜야한다.

코드를 보면 when절에서 `InvalidRequestException("Todo not found"))`라고 했으므로 404 Not Found나 400 Bad Request 상태를 기대하는 것이 맞다.

그러므로 then의 isOK()가 아니라 **isNotFound()**가 맞다.



5. 코드 개선 퀴즈 - JPA의 이해

조건 검색을 위해 날씨 외에 시작일과 종료일을 `@RequestParam`으로 넣었다.

조건이 있을 수도 있고 없을 수도 있다고 해서 `required = false`를 추가했다.

시작일과 종료일의 타입은 `LocalDate`지만, 수정일은 `LocalDateTime`이므로 비교가 불가능하다.

`LocalDateTime`을 `LocalDate`로 바꿀 수 없으니 `LocalDate`를 `LocalDateTime`으로 형태를 바꾸고, 시간이 나오도록 했다.

`getTodos()`메서드에 `if`조건을 주가했다.
모든 검색은 수정일을 기준으로 기간 검색이 가능하도록 했다.
- 날씨, 기간 조건이 모두 있을 경우 `findByWeatherAndModifiedDateBetween`
- 날씨 조건만 있을 경우 `findByWeather`
- 기간 조건만 있을 경우 `findByModifiedDateBetween`
- 조건이 없을 경우 `findAllTodos`




#### 필수 기능 Lv 2.

6. JPA Cascade

Todo와 Manager의 관계가 1:N이고, Todo를 새로 저장하면 Manager로 자동 등록되도록 `cascade = CascadeType.PERSIST, orphanRemoval = true`를 추가했다.


7. N+1

`JOIN FETCH`를 사용해서 해결함.

8. QueryDSL

커스텀 repository용 인터페이스(TodoRepositoryCustom)을 추가해서 `findByIdWithUser()`을 작성했다.

JPA 기능으로 `TodoRepositoryImpl`에서 Q클래스를 생성하는 코드를 작성했다.

코드 작성보다 Dependency 적용이 더 어려웠던 것 같다.

9. Spring Security

수정 및 추가한 클래스
- `SecurityConfig`
- `AuthUserArgumentResolver`
- `JwtFilter`
- `JwtUtil`
- `CustomUserDetails`
- `CustomUserDetailsService`

