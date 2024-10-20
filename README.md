# 문자열 덧셈 계산기의 기능 요구사항

> `camp.nextstep.edu.missionutils`에서 제공하는 `Console API`를 사용하여 구현<br>
> 사용자가 입력하는 값은 `camp.nextstep.edu.missionutils.Console`의 `readLine()`을 활용<br>
> **Git의 커밋 단위는 README.md에 정리한 기능 목록 단위**로 추가

- JDK 21 버전에서 실행 가능해야 한다.
- 프로그램 실행의 시작점은 Application의 main()이다.
- build.gradle 파일은 변경할 수 없으며, 제공된 라이브러리 이외의 외부 라이브러리는 사용하지 않는다.
- 프로그램 종료 시 System.exit()를 호출하지 않는다.
- 프로그래밍 요구 사항에서 달리 명시하지 않는 한 파일, 패키지 등의 이름을 바꾸거나 이동하지 않는다.
- 자바 코드 컨벤션을 지키면서 프로그래밍한다.
  - 기본적으로 [Java Style Guide](https://github.com/woowacourse/woowacourse-docs/tree/main/styleguide/java)를 원칙으로 한다.

<br>

## 출력

- [x] 계산기의 실행 문구를 출력하는 기능
  - 포맷: `덧셈할 문자열을 입력해주세요.`
- [x] 계산기의 실행 결과 문구를 출력하는 기능
  - 포맷: `결과 : %s`

- 출력 형식

```
덧셈할 문자열을 입력해 주세요.
<사용자가 입력한 값>
결과 : <덧셈 결과>
```

<br>

## 입력

- [x] 사용자가 텍스트를 입력하는 기능

### 예외처리

- [x] 문자열이 띄어쓰기로만 이루어진 경우 `IllegalArgumentException`을 발생시킨다.
  - 예 1) " "

<br>

## 덧셈 계산

- [x] 입력된 문자열의 숫자가 1개인 경우 그대로 출력
- [x] 구분된 문자열을 통해 합을 계산하는 기능

### 예외처리

- [x] 분리한 후의 값이 자료형의 범위 내에 있는지 확인한다.
  - int: -2,147,483,648 ~ 2,147,483,647
  - long: -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807
  - double: 1.79E-308(-1.79*10^308) ~ 1.79E+308(1.79*10^308) (15digits)
- [x] 덧셈을 하고 난 결과값이 자료형의 범위 내에 존재하지 않는 경우
  - 예 1) 2,147,483,647 + 2,147,483,647

<br>

## 구분자

- [x] 커스텀 구분자가 존재하지 않고, `,`와 `:`로 구분된 문자열일 경우 구분자를 기준으로 합을 계산
  - 예 1) "1,2" -> 1 + 2 -> 3
  - 예 2) "1,2:3" -> 1 + 2 + 3 -> 6
- [x] 커스텀 구분자가 존재하지 않고, `,`과 `:`를 제외한 다른 문자열이 존재할 경우 `IllegalArgumentException`을 발생시킨다.
  - 문자열에 띄어쓰기가 들어갈 경우
    - 예 1) "1, 2" -> `IllegalArgumentException`
- [x] 구분자 및 커스텀 구분자를 저장하는 객체를 선언한다.
  - 객체는 Set으로 선언하여 중복을 제거한다.
- [x] 입력한 문자열을 구분자로 분리한다.
- [ ] 구분자와 양수로 구성된 문자열인지 확인한다.
  - 정규 표현식 사용
- [x] 구분자로 나눈 문자열이 빈 칸일 경우 0으로 저장한다.
- [x] 구분자로 나눈 문자열이 모두 양수인지 확인한다.

## 커스텀 구분자

- 커스텀 구분자는 구분자를 상속받은 자식 객체로 구성한다.
- [x] 입력한 문자열에 커스텀 구분자가 존재할 경우 구분자와 커스텀 구분자로 문자열을 분리한다.
- [x] `//` `\n` 사이에 있는 문자의 경우 커스텀 구분자로 사용한다.
  - 예 1) "//;\n1;2;3" -> 커스텀 구분자 = `;`, 1 + 2 + 3 -> 6
- [x] `//`로 시작해서 `\n`로 닫히지 않고 안의 문자열에 `//`와 `\n`이 포함 되어있는 경우 `IllegalArgumentException`을 발생시킨다.
  - 예 1) "////;\n3\n"
- [ ] 커스텀 구분자는 여러 개 작성할 수 있다.
  - 예 1) "// \n//temp\n1temp2 3" -> 커스텀 구분자: ` `, `temp`, 1 + 2 + 3 -> 6
- [ ] 구분자와 양수로 구성된 문자열인지 확인한다.
  - 정규 표현식 사용

### 예외처리

- [ ] `//`와 `\n`의 개수가 맞지 않는 경우 `IllegalArgumentException`을 발생시킨다.
  - 예 1) `//`나 `\n` 중 하나로만 이루어져 있는 경우 -> "//1,2"
- [ ] `//`와 `\n`이 존재하나 `//`가 `/n`보다 뒤에 존재하는 경우 `IllegalArgumentException`을 발생시킨다.
  - 예 1) "\n;//1;2;3"
- [x] 구분자와 커스텀 구분자 외의 숫자가 아닌 다른 문자가 존재할 경우 `IllegalArgumentException`을 발생시킨다.
- [x] 커스텀 구분자로만 이루어져 있는 경우 0으로 계산한다.
  - 예 1) "//#\n" -> 0
  - 예 2) "//#\n#" -> 0 + 0 -> 0
- [x] 커스텀 구분자가 비어있는 경우 `IllegalArgumentException`을 발생시킨다.
  - 예 1) "//\n1,2,3"
- [ ] 커스텀 구분자가 중복된 경우 중복을 제거한 후 저장한다.
  - 예 1) "//;\n//!\n//;\n" -> 커스텀 구분자: `;`, `!`

<br>


## 예외처리

- [x] `IllegalArgumentException`을 일으키는 기능

## 프로젝트 전체 구조 설계

![8152825D-614D-4B4E-A03A-67BEA0A88872_1_102_a](https://github.com/user-attachments/assets/cfd14b6b-58a5-4c57-b45d-7de7e6169bf7)
