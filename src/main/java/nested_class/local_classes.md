
# Local Classes

로컬 클래스는 균형 괄호(`{}`) 사이에 있는 0개 이상의 명령문 그룹인 block에 정의된 클래스입니다. 일반적으로 메서드 본문에 정의된 로컬 클래스가 있습니다.

이 섹션에서는 다음 주제를 다룹니다.

- local 클래스 선언
- 포함하는 클래스의 멤버에 액세스 
  - 섀도잉 및 로컬 클래스
- 로컬 클래스는 내부 클래스와 유사합니다

## Declaring Local Classes

모든 블록 내에 로컬 클래스를 정의할 수 있습니다(자세한 내용은 [표현식, 명령문 및 블록](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/expressions.html) 참조). 예를 들어 메서드 본문, `for` 루프 또는 `if` 절에 로컬 클래스를 정의할 수 있습니다.

다음 예인 `LocalClassExample`은 두 개의 전화번호를 확인합니다. 이는 `verifyPhoneNumber` 메소드에 로컬 클래스 `PhoneNumber`를 정의합니다.

```java

public class LocalClassExample {

    static String regularExpression = "[^0-9]";

    public static void validatePhoneNumber(
            String phoneNumber1, String phoneNumber2) {

        final int numberLength = 10;

        // Valid in JDK8 and later:

        // int numberLength = 10;

        class PhoneNumber {

            String formattedPhoneNumber = null;

            PhoneNumber(String phoneNumber) {
                // numberLength = 7;
                String currentNumber = phoneNumber.replaceAll(regularExpression, "");
                if (currentNumber.length() == numberLength) {
                    formattedPhoneNumber = currentNumber;
                } else {
                    formattedPhoneNumber = null;
                }
            }

            public String getNumber() {
                return formattedPhoneNumber;
            }
            // Valid in JDK 8 and later:
//            public void printOriginalNumbers() {
//                System.out.println("Original numbers are " + phoneNumber1 +
//                    " and " + phoneNumber2);
//            }
        }
        PhoneNumber myNumber1 = new PhoneNumber(phoneNumber1);
        PhoneNumber myNumber2 = new PhoneNumber(phoneNumber2);

        // Valid in JDK8 and later:

//      myNumber1.printOriginalNumbers();

        if (muNumber1.getNumber() == null) {
            System.out.println("First number is invalid");
        } else {
            System.out.println("Fist number is " + myNumber1.getNumber());
        }
        if (myNumber2.getNumber() == null) {
            System.out.println("Second number is invalid");
        } else {
            System.out.println("Second number is " + myNumber2.getNumber());
        }

    }

    public static void main(String... args) {
        validatePhoneNumber("123-456-7890", "456-7890");
    }
}

```

이 예에서는 먼저 전화번호에서 숫자 0~9를 제외한 모든 문자를 제거하여 전화번호의 유효성을 검사합니다. 그런 다음 전화번호에 정확히 10자리(북미의 전화번호 길이)가 포함되어 있는지 확인합니다. 이 예에서는 다음을 인쇄합니다.
```
First number is 1234567890
Second number is invalid
```

## Accessing Members of an Enclosing Class

로컬 클래스는 자신을 둘러싸는 클래스의 멤버에 액세스할 수 있습니다. 이전 예제에서 `PhoneNumber` 생성자는 `LocalClassExample.regularExpression` 멤버에 액세스합니다.

또한 지역 클래스는 지역 변수에 접근할 수 있습니다. 그러나 로컬 클래스는 `final`로 선언된 로컬 변수에만 액세스할 수 있습니다. 로컬 클래스는 둘러싸는 블록의 로컬 변수나 매개변수에 액세스할 때 해당 변수나 매개변수를 `캡처`합니다. 예를 들어 `PhoneNumber` 생성자는 `final`로 선언된 지역 변수 `numberLength`에 액세스할 수 있습니다. `numberLength`는 `캡처된 변수`입니다.

그러나 Java SE 8부터 로컬 클래스는 `final` 또는 `사실상 final`인 둘러싸는 블록의 로컬 변수 및 매개변수에 액세스할 수 있습니다. 초기화된 후에 값이 변경되지 않는 변수나 매개변수는 사실상 `final`입니다. 예를 들어 변수 `numberLength`가 `final`로 선언되지 않았고 `PhoneNumber` 생성자에 강조 표시된 할당 문을 추가하여 유효한 전화번호 길이를 7자리로 변경한다고 가정합니다.

```java
PhoneNumber(String phoneNumber) {
    numberLength = 7;
    String currentNumber = phoneNumber.replaceAll(regularExpression, "");
    if (currentNumber.length()==numberLength) {
        formattedPhoneNumber = currentNumber;
    } else {
        formattedPhoneNumber = null;
    }
}

```
이 할당문으로 인해 `numberLength` 변수는 더 이상 `사실상 final` 변수가 아닙니다. 결과적으로 Java 컴파일러는 내부 클래스 `PhoneNumber`가 `numberLength` 변수에 액세스하려고 시도하는 경우 "내부 클래스에서 참조되는 로컬 변수는 `final`이거나 `사실상 final`이어야 합니다"와 유사한 오류 메시지를 생성합니다.

```java
if (currentNumber.length() == numberLength) {}
```
Java SE 8부터는 메소드에서 로컬 클래스를 선언하면 해당 메소드의 매개변수에 액세스할 수 있습니다. 예를 들어 `PhoneNumber` 로컬 클래스에서 다음 메서드를 정의할 수 있습니다.

```java
public void printOriginalNumbers() {
    System.out.println("Original numbers are " + phoneNumber1 +
        " and " + phoneNumber2);
}
```
`printOriginalNumbers` 메소드는 `verifyPhoneNumber` 메소드의 `phoneNumber1` 및 `phoneNumber2` 매개변수에 액세스합니다.

### Shadowing and Local Classes

동일한 이름을 가진 바깥쪽 범위의 로컬 클래스 섀도우 선언에 있는 유형(예: 변수)의 선언입니다. 자세한 내용은 Shadowing을 참조하세요.


## Local Classes Are Similar To Inner Classes

로컬 클래스는 정적 멤버를 정의하거나 선언할 수 없다는 점에서 내부 클래스와 유사합니다. 정적 메서드인 `verifyPhoneNumber`에 정의된 `PhoneNumber` 클래스와 같은 정적 메서드의 로컬 클래스는 바깥쪽 클래스의 정적 멤버만 참조할 수 있습니다. 예를 들어 멤버 변수 `regularExpression`을 정적으로 정의하지 않으면 Java 컴파일러는 "비정적 변수 `regularExpression`은 정적 컨텍스트에서 참조할 수 없습니다."와 유사한 오류를 생성합니다.

로컬 클래스는 둘러싸는 블록의 인스턴스 멤버에 액세스할 수 있으므로 정적이 아닙니다. 결과적으로 대부분의 정적 선언을 포함할 수 없습니다.

블록 내부에서는 인터페이스를 선언할 수 없습니다. 인터페이스는 본질적으로 정적입니다. 예를 들어 다음 코드 발췌 부분은 `HelloThere` 인터페이스가 `greetingInEnglish` 메서드 본문 내부에 정의되어 있으므로 컴파일되지 않습니다.

```java
    public void greetInEnglish() {
        interface HelloThere {
           public void greet();
        }
        class EnglishHelloThere implements HelloThere {
            public void greet() {
                System.out.println("Hello " + name);
            }
        }
        HelloThere myGreeting = new EnglishHelloThere();
        myGreeting.greet();
    }

```
로컬 클래스에서는 정적 초기화 프로그램이나 멤버 인터페이스를 선언할 수 없습니다. `EnglishGoodbye.sayGoodbye` 메서드가 정적으로 선언되었기 때문에 다음 코드 발췌 부분은 컴파일되지 않습니다. 컴파일러는 이 메서드 정의를 발견할 때 "'static' 수정자는 상수 변수 선언에서만 허용됩니다."와 유사한 오류를 생성합니다.
```java
    public void sayGoodbyeInEnglish() {
        class EnglishGoodbye {
            public static void sayGoodbye() {
                System.out.println("Bye bye");
            }
        }
        EnglishGoodbye.sayGoodbye();
    }
```
로컬 클래스는 상수 변수인 경우 정적 멤버를 가질 수 있습니다. (상수 변수는 `final`로 선언되고 컴파일 타임 상수 식으로 초기화되는 primitive 형식 또는 `String` 형식의 변수입니다. 컴파일 타임 상수 식은 일반적으로 컴파일 타임에 평가할 수 있는 문자열 또는 산술 식입니다. 자세한 내용은 클래스 멤버 이해를 참조하세요.) 다음 코드 발췌는 정적 멤버 `EnglishGoodbye.farewell`이 상수 변수이기 때문에 컴파일됩니다.
```java
public void sayGoodbyeInEnglish() {
    class EnglishGoodbye {
        public static final String farewell = "Bye bye";
        public void sayGoodbye() {
            System.out.println(farewell);
        }
    }
    EnglishGoodbye myEnglishGoodbye = new EnglishGoodbye();
    myEnglishGoodbye.sayGoodbye();
}

```