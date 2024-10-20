# java-calculator-precourse

## 프로젝트 소개

- - - - - -
+이 프로젝트는 우아한테크코스 7기 프리코스 1주차 과제로서 계산기를 구현하는 프로그램입니다.

+사용자가 쉼표(,) 또는 콜론(:)을 구분자로 가지는 문자열을 전달하는 경우 구분자를 기준으로 분리한 숫자의 합을 반환합니다.
+예시: "" => 0, "1,2" => 3, "1,2:3" => 6

+사용자가 기본 구분자(쉼표(,), 콜론(:)) 외에 커스텀 구분자를 지정할 수 있으며, 커스텀 구분자는 문자열 앞부분에 "//"와 "\n" 사이에 위치시켜 사용할 수 있습니다.
+예시: "//;\n1;2;3" => 커스텀구분자는 세미콜론(;)이며, 결과값으로 6이 반환됩니다.

## 기능 명세

- - - - - -

1. [사용자 입력처리]

   1.1 [문자열 입력받기] - View
    + 스크립트 출력 : "덧셈할 문자열을 입력해 주세요."
    + camp.nextstep.edu.missionutil의 readline() 메서드를 통해 사용자 입력값 받아서 반환

   1.2 [입력된 문자열 분리] - Model
    + (예외처리) 첫 시작이 // 또는 숫자로 시작하지 않거나 아예 빈문장열도 아닌 경우,
        + IllegalArgumentException 발생시키고 애플리케이션 종료

    + case1: 커스텀 구분자가 있는 경우:
        + 1.4 메서드 호출 -> 구분자 인덱스 반환받기
        + 1.5 메서드 호출(구분자인덱스 입력) -> 커스텀 구분자 반환받기
        + 커스텀 구분자를 가지고 case 2 진행
    + case2: 커스텀 구분자가 없는 경우(""포함)
        + 입력된 양의 정수를(피연산자, Operand) 구분자로 분리한뒤 배열로 저장하여 Controller로 반환
            + 구분할때는 split 메서드 사용(인자로 , 또는 : 넣어주기 - 정규표현식 사용 )
        + 1.3 메서드 실행히야 Long으로 변환된 배열 받기
        + 1.3에서 반환한 배열에 대해 2번 메서드 실행(계산기 로직)

    + Controller로 Long타입의 숫자 배열(피연산자 배열)을 반환

   1.3 [String 배열을 Long 배열로 변환]

    + 1.2에서 split으로 분리해서 String으로 저장된 배열을 다시 Long 배열로 변환하여 저장
        + case1: 빈문자열인 경우:
        + case2: 숫자인 경우: parseLong으로 변환
        + case3: 숫자가 아닌 문자인 경우("123abc", "abc123" 포함)
          +numberformatexception이 아닌 IllegalArgumentException이 발생하도록 try catch문 작성

   1.4 [커스텀구분자 끝지점 인덱스 반환] - Model

    + // 와 \n 사이에 있는 문자열을 추출하여 호출한 위치로 반환
        + 예외처리 : // 다음에 \n이 바로 오지 않는지
        + while문
        + 시작: int i = 2
        + 끝: for(;i<String.length();i++)
        + if(String.charat(i) == '\n')
        + if(i+1<String.length())
        + if(String.charat(i+1) != '\n')
        + return i
        + else
        + 예외처리: \n 뒤에 아무것도 안나오는 경우
        + 예외처리: 끝까지 \n이 안 나온 경우

   1.5 [커스텀구분자 반환] - model

    + 예외처리 :
        + String.substring(2,i)이 빈문자열 인 경우 - 구분자가 없는 경우
        + i+1>=String.length()인 경우 - \n 다음에 아무런 문자열도 없는 경우
        + Controller를 통해 1.4에서 반환한 인덱스 i를 전달 받음
        + 구분자 Controller로 반환 : String.substring(2, i)

2. [로직 구현] - model
   2.1 [계산기 합산 로직]
    + Controller에서 전달받은 배열안에 있는 양의 정수를 합산하여 int sum 변수에 저장
    + sum을 Controller로 반환

3. [처리값 출력] - view
   3.1 [결과 출력]
    + controller에서 2.1에서 반환한 sum을 입력값으로 해서 3.1메서드 실행
    + "결과 : " + sum 을 출력

4. [예외 처리]
    + 구분자를 받는 경우, 받지 않는 경우 공통
        + 첫 시작이 // 또는 숫자로 시작하지 않거나 아예 빈문장열이 아닌 경우 - 처리 완료
    + 구분자를 받는 경우
        + // 바로 다음에 \n이 나오는 경우(구분자가 없는 경우)  - 처리 완료
        + // 구분자 \n 다음에 아무 숫자도 안 나오는 경우 - 처리 완료
          (+ 문자열이 // \n\n\n\n 으로 끝나는 경우)
        + 문자열이 끝날때까지 \n이 나오지 않는 경우 - 처리 완료
        
        