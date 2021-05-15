# Java의 정석 TIL



## Ch1. 셋팅

### 이클립스 단축키

```
     command + shift + L 단축키 전체 목록보기
		 command + +, - 폰트크기 증가/감
		 command + D 행 삭제
		 command + option + down 행단위 복사
		 command + option + a 멀티컬럼 편집
		 option + up, down 행단위 이동
		 command + i 자동 들여쓰기
		 command + / 주석(토글)
```



## Ch2. 변수

변수: 하나의 값을 저장할 수 있는 메모리 공간

변수 선언 방법

​	`변수타입 변수이름 ;`

상수(constant): 한 번만 값을 저장 가능한 변수

```java
final int MAX = 100;
					MAX = 200; Error
```

리터럴(literal): 우리가 알고 있는 원래 상수

리터럴의 접두사와 접미사	

|  종류  |            리터럴            |     접미사     |
| :----: | :--------------------------: | :------------: |
| 논리형 |         false, true          |                |
| 정수형 | 123, 0b0101, 077, 0xFF, 100L |       L        |
| 실수형 | 3.14, 3.0e8, 1.4f, 0x1.op-1  | f, d(생략가능) |
| 문자형 |        'A', '1', '\n'        |                |
| 문자열 |  "ABC", "123", "A", "true"   |                |

- 정수형 접두사
  - 0: 8진수
  - 0x: 16진수
  - 0b: 2진수
- 정수형 접미사
  - 20억 이상일 시, L 붙여야 에러 안남
  - `long l = 10_000_000_000L;` 헷갈리지 말라고 쉽표 대신 underscore



변수 > 리터럴 => OK
변수 < 리터럴 => ERROR



- 기본형(Primitive type); 값의 타입 8개

  - 값

    - 문자 -char
    - 숫자
      - 정수 - byte, short, int, long
      - 실수 - float, double
    - 논리 - boolean

  - **실제 값을 저장**

  - Char는 변수 당 하나의 문자만을 저장 가능

  - 정수형: byte는 이진데이터, short는 c언어와의 호환을 위해 추가

    | 종류/크기 | 1byte   | 2byte          | 4byte        | 8byte              |
    | --------- | ------- | -------------- | ------------ | ------------------ |
    | 논리형    | boolean |                |              |                    |
    | 문자형    |         | char(유니코드) |              |                    |
    | 정수형    | byte    | short          | int(default) | long               |
    | 실수형    |         |                | float        | double(정밀도 2배) |

  - java에서 정수는 항상 부호 있음

    - n bit로 표현할 수 있는 값의 범위: -2^(n-1) ~ 2^(n-1) - 1
    - byte타입의 경우, 부호(sign)bit + 7bit
    - byte: -128 ~ 127
    - short: -32768 ~ 32767
    - char: 0 ~ 65535(부호 없음)
    - int; -20억 ~ 20억
    - long: -800경 ~ 800경
    - 800경 이상일 시 BigInteger Class 사용

  - float 타입은 4byte지만 int보다 훨씬 큰 범위 저장 가능

    - 부호/지수/가수
      S/E(8)/M(23)
    - -(3.4E38 ~ 1.4E-45) & 1.4E-45 ~ 3.4E38; 사이 값은 표현할 수 없음
    - 원래 저장하려던 값과 실제 값의 오차 발생 가능 => 정밀도 중요
    - 정밀도: 오차 없는 자릿수
      - float는 7자리
      - double은 15자리
        - S/E(11)/M(52)
      - 단순히 값의 범위가 아니라 정밀도가 더 중요한 경우가 많음. 따라서 정밀도가 더 높은 double이 default

- 참조형 (Reference type)
  - 기본형을 제외한 나머지(String, System 등)
  - **메모리 주소를 저장(4byte OR 8byte)**



println() 단점 - 출력형식 지정 불가

​	1) 실수의 자릿수 조절 불가

​	2) 10진수로만 출력된다

=> printf()로 출력형식 지정가능

```java
System.out.printf("%.2f", 10.0/3);  // 3.33
System.out.printf("%d", 0x1A); // 26
System.out.printf("%x", 0x1A); //1A
```

- printf()의 지시자

  - | 지시자 | 설명        |
    | ------ | ----------- |
    | %b     | boolean     |
    | %d     | 10진 정수   |
    | %o     | 8진 정수    |
    | %x, %X | 16진 정수   |
    | %f     | 부동소숫점  |
    | %e, %E | 지수 표현식 |
    | %c     | 문자        |
    | %s     | 문자열      |

    정수를 10진수, 8진수, 16진수, 2진수로 출력

    ```java
    System.out.printf("%d", 15); // 15
    System.out.printf("%o", 15); // 17
    System.out.printf("%x", 15); // f
    System.out.printf("%s", Integer.toBinaryString(15)); // 1111
    ```

  - 8진수와 16진수에 접두사 붙이기

    ```java
    System.out.printf("%#o", 15) // 017
    System.out.printf("%#x", 15) // 0xf
    System.out.printf("%#X", 15) // 0Xf  
    ```

  - 실수 출력을 위한 지시자 %f, 지수형식 %e, 간략한 형식 %g

    ```java
    float f = 123.4567890f;
    System.out.printf("%f", f) // 123.456787 (정밀도 7자리이므로 7자리보다 크면 의미 없음)
    System.out.printf("%e", f) // 1.234568e+02
      
    System.out.printf("%g", 123.456789) // 123.457
    System.out.printf("%g", 0.00000001) // 1.00000e-8
    ```

  - 출력 자리수 지정

    ```java
    // 정수
    System.out.printf("[%5d]%n", 10)   // [   10]
    System.out.printf("[%-5d]%n", 10)  // [10   ]
    System.out.printf("[%05d]%n", 10)  // [00010]
    // 소수
    System.out.printf("d=%14.10f%n", d) // 공백공백1.2345678900 /전체자리 14, 소수점아래자리 10
    // 문자열
    System.out.printf("[%s]%n", url)    // [www.codechobo.com]
    System.out.printf("[%20s]%n", url)  // [   www.codechobo.com]
    System.out.printf("[%-20s]%n", url) // [www.codechobo.com   ]
    System.out.printf("[%.8s]%n", url)  // [www.code]
    ```

    



Scanner: 화면으로부터 데이터를 입력받는 기능을 제공하는 클래스

1. import문 추가

   `import java.util.*;`

2. Scanner 객체의 생성
   `Scanner scanner = new Scanner(System.in);`

3. Scanner 객체를 사용

   `int num = scanner.nextInt(); // 화면에서 입력받은 정수를 num에 저장`

   `String input = scanner.nextLine(); // 화면에서 입력받은 내용을 input에 저장`

   `int num = Integer.parseInt(input); // 문자열(input)을 숫자(num)로 변환`

 

오버플로우: 표현가능한 범위를 넘는 것

​	최댓값 + 1  => 최솟값
​	최솟값 - 1  => 최댓값



### 타입 간 변환방법

1. 문자와 숫자간의 변환

   - 문자 '0' 더하거나 뺌

2. 문자열로의 변환

   - 빈문자열 더해주면 문자열로 바뀜

3. 문자열을 숫자로 변환

   - `Integer.parseInt("3")`
   - `Double.parseDouble("3.4")`

4. 문자열을 문자로 변환

   - `"3".charAt(0)`

   

   

   단항연산자가 이항연산자 보다 우선 순위가 높다

   (단항 > 이항 > 삼항)

   산술연산자가 비교연산자 보다 우선 순위가 높다

   비교연산자가 논리연산자 보다 우선 순위가 높다

   대입 연산자는 연산자 중 제일 우선 순위가 낮다

   (산술 > 비교 > 논리 > 대입)

   **단항 연산자와 대입 연산자를 제외한 모든 연산의 진행방향은 왼쪽에서 오른쪽이다**





### 증감연산자

전위형: 값이 참조되기 전에 증가시킨다; `j = ++i;`

후위형: 값이 참조되기 전에 증가시킨다; `j = i++;`



### 형변환 연산자

변수 또는 상수의 타입을 다른 타입으로 변환하는 것

타입이 맞지 않는 경우 compiler가 자동으로 형변환하기도 함(표현범위가 작은 타입을 큰 타입에 저장할 때, but 반대의 경우엔 에러 발생. 이 경우 수동 형변환 해야 함)[But 예외적인 케이스도 있음. byte범위 안의 숫자를 int리터럴로 대입할 시 자동형변환]
"기존의 값을 최대한 보존할 수 있는 타입으로 자동 형변환된다."



### 사칙 연산자

컴퓨터는 원래 다른 타입은 계산 못한다

산술변환
	: 연산 전에 피연산자의 타입을 일치시키는 것

 1. 두 피연산자의 타입을 같게 일치시킨다.(보다 큰 타입으로 일치)

    long + int -> long + long -> long

    float + int -> float + float -> float

    double + float -> double + double -> double

 2. 피연산자의 타입이 int보다 작은 타입이면 int로 변환된다.

    byte + short -> int + int -> int

    char + short -> int + int -> int



### 반올림 - Math.round()

: 소숫점 첫째자리에서 반올림 -> 다른 자리에서 반올림하고 싶으면 10^n을 곱하고 다시 10^n으로 나누는 과정 필요

`long result = Math.round(4.52)`

버림: 형변환 이용(int)



### 나머지 연산자  %

나누는 피연산자는 0이 아닌 정수만 가능(부호는 무시됨)