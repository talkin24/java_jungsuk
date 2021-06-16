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



### 비교연산자

문자열 비교에는 == 대신 equals()를 사용해야 함



### 조건 연산자

조건식의 결과에 따라 연산결과를 달리한다. (if문을 간단하게 나타내기 위해 사용)

유일한 3항 연산자

ex) `result = (x>y) ? x : y;`



## Ch4. 조건문과 반복문(제어문; flow control statement)

### if문

`if (조건식) {`

// 조건식이 참일 때 수행될 문장들을 적는다.

`}`

### 블럭

: 여러문장을 하나로 묶어주는 것. if문에 속한 문장이 하나라면 블럭 생략

# 프로그래밍 Refactoring 관점에서 else문은 사용하지 않는게 좋다.



### Switch문

: 처리해야 하는 경우의 수가 많을 때 유용한 조건문

```java
switch (조건식) {
	case 값1 :
			// 조건식의 결과가 값1과 같을 경우 수행될 문장들
			// ...
			break; // switch문을 벗어난다
	case 값2 :
			// 조건식의 결과가 값2와 같을 경우 수행될 문장들
			// ...
			break; // switch문을 벗어난다			
	default :
			// 	조건식의 결과와 일치하는 case문이 없을 때 수행될 문장들
			// ...
}
```

default 문은 생략 가능

### Switch문 제약 조건

1. 조건식 결과는 정수 또는 문자열이어야 한다.
2. case문의 값은 정수(상수(문자포함)), 문자열만 가능. 중복 허용 안됨

### 임의의 정수(난수) 만들기

`Math.random()`- 0.0과 1.0 사이의 임의의 double 값을 반환

형변환, 덧셈으로 원하는 정수 범위 설정 가능



### for 문

: 반복횟수를 알 때 적합. 조건식이 True인 한 수행됨

```java
for(초기화;조건식;증감식){
	수행될 문장
}
```



# 실행해 보기 전에 예측하는 습관을 가져야 함



### 중첩for문

### while문

: 반복횟수를 모를 때(보통), 조건식이 True인 한 수행

for문과 while문은 100% 호환 가능

- 무한반복문

  ```java
  # for문 - 조건식 비우면 됨
  for(;;){
  
  }
  
  # while문 - 조건식 비우면 에러 발생
  while(true) {
  
  }
  ```

  

### do-while문

: 블럭을 최소한 한 번 이상 반복 - 사용자 입력 받을 때 유용

```java
do {
	
} while (조건식);
```

### break문

: 자신이 포함된 하나의 반복문을 벗어난다

### continue문

: 자신이 포함된 반복문의 끝으로 이동 - 다음 반복으로 넘어감

### 이름붙은 반복문

: 반복문에 이름을 붙여서 하나 이상의 반복문을 벗어날 수 있다.

```java
class Ex4_19
{
	public static void main(String[] args)
  {
    Loop1 : for(int i=2;i<=9;i++) {
      for(int j=1;j<=9;j++) {
        if(j==5)
          break Loop1;
        
				System.out.println(i+"*"+j+"="+i*j);        
      }
      System.out.println();
    }
  }
}
```



# Ch5. 배열

: 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것



배열의 선언 - 배열을 다루기 위한 참조변수의 선언

| 선언방법           | 선언 예                              |
| ------------------ | ------------------------------------ |
| `타입[] 변수이름;` | `int[] score;`<br />`String[] name;` |
| `타입 변수이름[];` | `int score[];`<br />`String name[];` |

```java
int[] score; 				// int타입의 배열을 다루기 위한 참조변수 score 선언
score = new int[5]; // int타입의 값 5개를 저장할 수 있는 배열 생성

score[3] = 100;      // 배열 score의 4번째 요소에 100을 저장
int value = score[3]; // 배열 score의 4번째 요소 값을 읽어서 value에 저장
```



배열의 인덱스 - 각 요소에 자동으로 붙는 번호



배열의 길이 - 배열은 한번 생성하면 실행동안 그 길이를 바꿀 수 없다.

: 배열이름.length (int형 상수)



배열의 초기화: 배열의 각 요소에 처음으로 값을 저장하는 것 

```java
int[] score = new int[] {50, 60, 70, 80, 90};
int[] score = {50, 60, 70, 80, 90}; // new int []를 생략할 수 있음, 두문장으로 나누면 에러남

int[] score;
score = {50, 60, 70, 80, 90}; 두문장으로 나눠서 에러 발생
```



배열의 출력

```java
int[] iArr = {100, 95, 90, 80, 70};
System.out.println(iArr); // [I@14318와 같은 형식의 문자열이 출력된다. Char배열인 경우엔 제대로 찍어줌
```

for문을 쓰거나, Arrays 클래스의 toString사용

```java
import java.util.Arrays; // ctrl+shift+o 자동으로 import문 생성

for(int i=0, i< iArr.length; i++) {
		System.out.println(iArr[i]);
}

System.out.println(Arrays.toString(iArr));
```



배열의 활용

 : 총합, 평균, 최댓값, 최솟값, 섞기



String 배열

```java
String[] name = new String[3]; // 3개의 문자열을 담을 수 있는 배열을 생성한다.
```

* 참조형의 기본값은 null 
* 실제로는 값이 들어가는게 아니고, 주소가 배열에 입력됨.(그러나 실제 들어간다고 보아도 큰 문제 없음)



### 커맨드 라인을 통해 입력받기



### 2차원 배열

: 테이블 형태의 데이터를 저장하기 위한 배열

```java
int[][] score = new int[4][3]; // 4행 3열의 2차원 배열 생성
```

2차원 배열의 초기화

```java
int[][] arr = {
                {1, 2, 3}, 
                {4, 5, 6}
                };
```

2차원배열 = 1차원 배열의 배열



## String클래스

String 클래스는 char[]와 메서드를 결합한 것

String 클래스는 내용을 변경할 수 없다.(read only) => 문자열 변경 시 새로운 문자열이 만들어질뿐 기존 문자열이 바뀌는 것이 아니다

### 주요 메서드

| 메서드                             | 설명                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| char charAt(int index)             | 문자열에서 해당 위치(index)에 있는 문자를 반환한다           |
| int length()                       | 문자열의 길이를 반환한다                                     |
| String substring(int from, int to) | 문자열에서 해당 범위의 문자열을 반환한다(to는 포함안됨)      |
| boolean equals(Object obj)         | 문자열의 내용이 같은지 확인한다. 같으면 결과는 true, 다르면 false |
| char[] toCharArray()               | 문자열을 문자배열(char[])로 변환해서 반환한다                |

## Arrays클래스

### 배열의 출력과 비교 - equals(), toString()

```java
## 출력
int[] arr = {0, 1, 2, 3, 4};
int[][] arr2D = {{11, 12}, {21, 22}};
System.out.println(Arrays.toString(arr)); // [0, 1, 2, 3, 4]
System.out.println(Arrays.deepToString(arr2D)); // [[11, 12], [21, 22]]
## 비교
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA". "BBB"}}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA". "BBB"}}};
System.out.println(Arrays.equals(str2D, str2D2)); // false; 1차원 배열 비교
System.out.println(Arrays.deepequals(str2D, str2D2)); // true; 2차원 이상
```

### 배열의 복사 - copyOf(), copyOfRange()

```java
int[] arr = {0, 1, 2, 3, 4};
int[] arr2 = Arrays.copyOf(arr, arr.length); // arr2 = [0, 1, 2, 3, 4]
int[] arr3 = Arrays.copyOf(arr, 3);					 // arr3 = [0, 1, 2]
int[] arr4 = Arrays.copyOf(arr, 7);	         // arr4 = [0, 1, 2, 3, 4, 0, 0]
int[] arr5 = Arrays.copyOfRange(arr, 2, 4);  // arr5 = [2, 3]
int[] arr6 = Arrays.copyOfRange(arr, 0, 7);  // arr6 = [0, 1, 2, 3, 4, 0, 0]
```

### 배열의 정렬 - sort()

```java
int[] arr = {3, 2, 0, 1, 4};
Arrays.sort(arr); // 배열을 오름차순 정렬한다.
System.out.println(Arrays.toString(arr)); // [0, 1, 2, 3, 4]
```



# Ch6. 객체지향개념(OOP, object-oriented programming)

1. 캡슐화
2. 상속
3. 추상화
4. 다형성****

객체지향 언어 = 프로그래밍 언어 + 객체지향개념(규칙)

객체 = 속성(변수) + 기능(메서드)

객체: 모든 인스턴스를 대표하는 일반적 용어

인스턴스: 특정 클래스로부터 생성된 객체

인스턴스화: 클래스 -> 인스턴스(객체)

Q. 클래스가 왜 필요한가?
A. 객체를 생성하기 위해

Q. 객체가 왜 필요한가?
A. 객체를 사용하기 위해

Q. 객체를 사용한다는 것은?
A. 객체가 가진 속성과 기능을 사용하려고



### 하나의 소스파일에 여러 클래스 작성

| 올바른 작성 예                                               | 설 명                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Hello2.java<br />public class Hello2{ }<br />           class Hello3 { } | public class가 있는 경우, 소스파일의 이름은 반드시 public class의 이름과 일치해야 한다 |
| Hello2.java<br />class Hello2 { }<br />class Hello3 { }      | public class가 하나도 없는 경우, 소스파일의 이름은 'Hello2.java', 'Hello3.java' 둘다 가능하다 |

- 하나의 소스파일에 둘 이상의 public class가 존재하면 안된다.
- 대소문자 구분해야 한다
- public class가 존재하면, public class이름과 파일명이 동일해야 한다.
- 가능하면 하나의 소스파일에는 하나의 클래스만 작성하는 것이 바람직하다.



### 객체의 생성과 사용

```java
클래스명 변수명;					// 클래스의 객체를 참조하기 위한 참조변수를 선언
변수명 = new 클래스명(); // 클래스의 객체를 생성 후, 객체의 주소를 참조변수에 저장

Tv t;								 // Tv클래스 타입의 참조변수 t를 선언
t = new Tv();				 // Tv인스턴스를 생성한 후, 생성된 Tv 인스턴스의 주소를 t에 저장

Tv t = new Tv(); 		 //	 한줄로 표현


# 객체의 사용
t.channel = 7;       // 	Tv인스턴스의 멤버변수 channel의 값을 7로 한다.
t.channelDown(); 		 // 	Tv인스턴스의 메서드 channelDown()을 호출한다.
```

- 참조변수는 인스턴스를 다루는 리모컨이라고 생각하면됨
- 리모컨 없이 객체를 사용할 수 없음
- 사용할 수 없는 객체들은 garbage collector가 주기적으로 삭제해줌
- 하나의 인스턴스를 여러 개의 참조변수가 가리키는 것은 가능
- 여러 인스턴스를 하나의 참조변수가 가리키는 것은 불가능



### 객체 배열

객체 배열 == 참조변수 배열

객체배열을 만들었다고 해서 그 안에 객체가 자동으로 생성되는 것이 아님. 객체배열 생성 후 각 인덱스에 객체 생성해주어야 함

```java
Tv[] tvArr = new Tv[3]; // 객체배열 생성
tvArr[0] = new Tv();		// 객체배열에 객체 생성
tvArr[1] = new Tv();
tvArr[2] = new Tv();
```



### 클래스

(1) 설계도

(2) 데이터 + 함수

- 구조체: 서로 관련된 여러 데이터(종류 관계 없이)를 하나로 저장할 수 있는 공간

- 클래스: 데이터와 함수의 결합(구조체 + 함수)(구조체에 관련있는 함수까지 함께 저장)

(3) 사용자 정의

- 사용자 정의 타입 - 원하는 타입을 직접 만들 수 있음



### 선언위치에 따른 변수의 종류

```java
class Variables
{																															//
	int iv;					// 인스턴스 변수															 //		클래스영역	
	static int cv;  // 클래스 변수(static변수, 공유변수) 						 //   : 선언문만 가능, 순서 X
																															//     iv, cv
	void method()																								//  
	{																     		// 메서드영역					//
		int lv = 0;		// 지역변수						 		// : lv							//	
	}																														//
}
```

|   변수의 종류    |        선언위치         |                     생성시기                     |
| :--------------: | :---------------------: | :----------------------------------------------: |
|  클래스변수(cv)  |       클래스영역        | 클래스가 메모리에 올라갈 때(객체 생성 안해도 됨) |
| 인스턴스변수(iv) |       클래스영역        |            인스턴스가 생성 되었을 때             |
|   지역변수(lv)   | 클래스 영역 이외의 영역 |           변수 선언문이 수행되었을 때            |



### cv와 iv의 차이

객체의 속성중 개별적인 속성: 인스턴스변수

객체 모두 공통적으로 유지되어야 하는것: cv(static)

```java
class Card{
	String kind;
	int number;
	
	static int width = 100;
	static int height = 250;
}


Card c = new Card();
c.kind = "Heart";
c.number = 5;
c.width = 200;     // 가능하나 권장하지 않음
c.height = 300;    // 가능하나 권장하지 않음
Card.width = 200;  // 권장
Card.height = 300; // 권장
```

- cv는 객체 생성 없이 사용가능!!!
- cv는 iv처럼 바꾸어도 전체가 바뀜. 즉 공유하는 모든 값이 변화됨



### 메서드란?

문장들을 묶어 놓은 것 - 작업 단위로 문장들을 묶어서 이름 붙인 것

값을 받아서 처리하고, 결과를 반환

메서드와 함수는 거의 비슷. 메서드는 반드시 클래스 안에 있어야 한다는 제약이 있을 뿐

**하나의 메서드는 한가지 기능만 수행하도록 작성**

메서드 = 선언부 + 구현부

```java
반환타입 메서드이름 (타입 변수명, 타입 변수명, ...)	// 선언부
{                   											// 구현부
	// 메소드 호출시 수행될 코드               
}
```

**반환할 타입이 없을 때는 void라고 적는다**

지역변수: 메서드 내에 선언된 변수. 그래서 이름이 다른 메소드의 변수랑 겹쳐도 상관 없음. 매개변수 역시 지역변수



### 메서드의 호출

### return문

: 실행 중인 메서드를 종료하고 호출한 곳으로 되돌아 간다

- 반환 타입이 void가 아닌 경우, 반드시 return문 필요



### 호출 스택(call stack)

스택: 밑이 막힌 상자, 위에 차곡차곡 쌓인다

**호출스택: 메서드 수행에 필요한 메모리가 제공되는 공간. 메서드가 호출되면 호출스택에 메모리 할당, 종료되면 해제**

-> 아래 있는 메서드가 위의 메서드를 호출. 맨 위의 메서드 하나만 실행 중, 나머지는 대기중



### 기본형 매개변수

read only



### 참조형 매개변수

read & write

객체의 주소 자체를 알고 있기 때문에 읽기 쓰기 전부 가능

```java
class Data2 {int x;}

class Ex6_7 {
  public static void main(String[] args) {
    Data2 d = new Data2();
    d.x = 10;
    System.out.println("main() : x = " + d.x);
    
    change(d);
    System.out.println("After change(d)");
    System.out.println("main() : x = " + d.x);
  }
  
  static void change(Data2 d) {
    d.x = 1000;
    System.out.println("change(): x = " + d.x);
  }
}
// main() : x = 10
// change() : x = 1000
// After change(d)
// main() : x = 1000
```

반환타입이 참조형인 경우엔 복사한 객체의 주소를 반환(=객체를 반환)

```java
class Data3 { int x; }

class Ex6_8 {
  public static void main(String[] args) {
    Data3 d = new Data3();
    d.x = 10;
    
    Data3 d2 = copy(d);
    System.out.println("d.x = " + d.x);
    System.out.pringln("d2.x = " + d2.x);
  }
  
  static Data3 copy(Data3 d) {
    Data3 tmp = new Data3();
    
    tmp.x = d.x;
    
    return tmp;
  }
}
// d.x = 10
// d2.x = 10
```





### 인스턴스 메서드

- 인스턴스 생성 후, '참조변수.메서드이름()'으로 호출
- 인스턴스 멤버(iv, im)와 관련된 작업을 하는 메서드
- 메서드 내에서 인스턴스 변수(iv) 사용 가능

### static메서드

- **객체 생성 없이** '클래스이름.메서드이름()'으로 호출 (객체생성 안하니까 참조변수 없음. 클래스이름 사용)
- 인스턴스 멤버(iv, im)와 관련없는 작업을 하는 메서드
- 메서드 내에서 인스턴스 변수(iv) 사용 불가

=> iv 사용 여부로 구분!

**객체: iv 묶음** => 인스턴스 메서드는 iv로 작업하기 때문에 객체를 생성해야함!

```java
class MyMath2 {
	long a, b; // iv
  
  long add() { // 인스턴스 메서드, iv
    return a + b; // iv
  }
  
  static long add(long a, long b) { // 클래스메서드(static메서드), lv
    return a + b;  // lv
  }
}


class MyMathTest2 {
  public static void main(String args[]) {
    System.out.println(MyMath2.add(200L, 100L)); // 클래스메서드 호출
    MyMath2 mm = new MyMath2 (); // 인스턴스 생성
    mm.a = 200L;
    mm.b = 100L;
    System.out.println(mm.add()); // 	인스턴스메서드 호출
  }
}
```

static을 언제 붙여야 할까?

- 속성(멤버 변수) 중에서 공통 속성에 static을 붙인다.
- 인스턴스 멤버(iv, im)을 사용하지 않는 메서드에 static을 붙인다.



인스턴스메서드/static메서드 모두 클래스 변수는 사용가능. 단 static메서드의 경우, 호출 시 인스턴스가 존재한다는 보장이 없기 때문에 iv는 사용하면 안됨

인스턴스 메서드는 다른 인스턴스 메서드 호출 가능(이미 인스턴스가 생성되었으니까). 단 static메서드는 인스턴스 메서드를 호출할 수 없다. static끼리는 서로 호출 가능

