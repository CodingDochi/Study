
# Nested Classes

중첩 클래스는 `static`과 non-static으로 분류 가능. non-static 중첩 클래스는 `inner class` , `static`클래스는 `static nested class`라고 함.

```java
class OuterClass {
    
    class InnerClass {
        //...      
    }

    static class StaticNestedClass {
        // ...
    }
}
```

중첩 클래스는 해당 클래스를 둘러싸는 클래스의 멤버입니다. non-static 중첩 클래스(inner 클래스)는 private으로 선언된 경우에도 바깥쪽 클래스의 다른 멤버에 액세스할 수 있습니다.
static 중첩 클래스는 바깥쪽 클래스의 다른 멤버에 액세스할 수 없습니다.
OuterClass의 멤버로서 중첩 클래스는 `private`, `public`, `protected` 또는 `package private`로 선언될 수 있습니다. (외부 클래스는 `public` 또는 `package private`로만 선언될 수 있다는 점을 기억하세요.

## Why Use Nested Classes?

중첩 클래스를 사용하는 중요한 이유는 다음과 같습니다.

이는 한 곳에서만 사용되는 클래스를 논리적으로 그룹화하는 방법입니다. 클래스가 다른 하나의 클래스에만 유용하다면 해당 클래스에 해당 클래스를 포함하고 두 클래스를 함께 유지하는 것이 논리적입니다. 이러한 "도우미 클래스"를 중첩하면 패키지가 더욱 간소화됩니다.

캡슐화를 증가시킵니다. B가 `private`로 선언될 A의 멤버에 액세스해야 하는, 두 개의 최상위 클래스 A와 B를 생각해 보세요. 클래스 A 내에 클래스 B를 숨김으로써 A의 멤버를 비공개로 선언하고 B가 해당 멤버에 액세스할 수 있습니다. 또한 B 자체도 외부 세계로부터 숨겨질 수 있습니다.

코드를 더 읽기 쉽고 유지 관리하기 쉽게 만들 수 있습니다. 최상위 클래스 내에 작은 클래스를 중첩하면 코드가 사용되는 위치에 더 가깝게 배치됩니다.

## Inner Classes

인스턴스 메서드 및 변수와 마찬가지로 내부 클래스는 바깥쪽 클래스의 인스턴스와 연결되며 해당 개체의 메서드 및 필드에 직접 액세스할 수 있습니다. 또한 내부 클래스는 인스턴스와 연결되어 있으므로 정적 멤버 자체를 정의할 수 없습니다.

내부 클래스의 인스턴스인 객체는 외부 클래스의 인스턴스 내에 존재합니다. 다음 클래스를 고려하십시오.

```java
class OuterClass {
    //...
    class InnerClass {
       // ...
    }
}
```

`InnerClass`의 인스턴스는 `OuterClass`의 인스턴스 내에만 존재할 수 있으며 해당 인스턴스의 메서드 및 필드에 직접 액세스할 수 있습니다.

내부 클래스를 인스턴스화하려면 먼저 외부 클래스를 인스턴스화해야 합니다. 그런 다음 아래 구문을 사용하여 외부 개체 내에 내부 개체를 만듭니다.

```java
OuterClas outerObject = new OuterClass();
OuterClas.InnerClass innerObject = outerObject.new InnerClass();

```
내부 클래스에는 로컬 클래스와 익명 클래스라는 두 가지 특별한 종류가 있습니다.

## Static Nested Classes

클래스 메서드 및 변수와 마찬가지로 정적 중첩 클래스는 외부 클래스와 연결됩니다. 그리고 정적 클래스 메서드와 마찬가지로 정적 중첩 클래스는 해당 클래스를 둘러싸는 클래스에 정의된 인스턴스 변수나 메서드를 직접 참조할 수 없습니다. 객체 참조를 통해서만 사용할 수 있습니다. 내부 클래스 및 중첩 정적 클래스 예제에서는 이를 보여줍니다.

참고: 정적 중첩 클래스는 다른 최상위 클래스와 마찬가지로 외부 클래스(및 기타 클래스)의 인스턴스 멤버와 상호 작용합니다. 실제로 정적 중첩 클래스는 패키징 편의를 위해 다른 최상위 클래스에 중첩된 동작상 최상위 클래스입니다. 내부 클래스 및 중첩 정적 클래스 예제에서도 이를 보여줍니다.

최상위 클래스와 동일한 방식으로 정적 중첩 클래스를 인스턴스화합니다.

```java
StaticNestedClass staticNestedObject = new StaticNestedClass();
```

## Inner Class and Nested Static Class Example

다음 예제인 `OuterClass`는 `TopLevelClass`와 함께 내부 클래스(`InnerClass`), 중첩된 정적 클래스(`StaticNestedClass`) 및 최상위 클래스(`TopLevelClass`)가 액세스할 수 있는 `OuterClass`의 클래스 멤버를 보여줍니다.

### OuterClass.java
```java
public class OuterClass {
    String outerField = "Outer field";
    static String staticOuterField = "Static outer field";
    
    class InnerClass {
        void accessMembers() {
            System.out.println(outerField);
            System.out.println(staticOOuterField);
        }
    }
    
    static class StaticNestedClass {
        void accessMembers(OuterClass outer) {
            // Compiler error: Cannnot make a static reference to the non-static field outerField
            // System.out.println(outerField);
            System.out.println(outer.outerField);
            System.out.println(staticOuterField);
            
        }
    }
    
    public static void main(String[] args) {
        System.out.println("Inner class:");
        System.out.println("------------");
        OuterClass outerObject = new OuterClass();
        OuterClass.InnerClass innerObject = outerObject.new InnerClass();
        innerObject.accessMembers();

        System.out.println("\nStatic nested class:");
        System.out.println("--------------------");
        StaticNestedClass staticNestedObject = new StaticNestedClass();
        staticNestedObject.accessMembers(outerObject);

        System.out.println("\nTop-level class:");
        System.out.println("--------------------");
        TopLevelClass topLevelObject = new TopLevelClass();
        topLevelObject.accessMembers(outerObject);
        
    }
}

```
### TopLevelClass.java

```java

public class TopLevelClass {
    
    void accessMembers(OuterClass outer) {
        // Compiler error: Cannot make a static reference to the non-static
        //     field OuterClass.outerField
        // System.out.println(OuterClass.outerField);
        System.out.println(outer.outerField);
        System.out.println(OuterClass.staticOuterField);
    }
}
```
이 예에서는 다음 출력을 인쇄합니다.

```
Inner class:
------------
Outer field
Static outer field

Static nested class:
--------------------
Outer field
Static outer field

Top-level class:
--------------------
Outer field
Static outer field
```

정적 중첩 클래스는 다른 최상위 클래스와 마찬가지로 외부 클래스의 인스턴스 멤버와 상호 작용합니다. 정적 중첩 클래스인 `StaticNestedClass`는 바깥쪽 클래스인 `OuterClass`의 인스턴스 변수이기 때문에 `outerField`에 직접 액세스할 수 없습니다. Java 컴파일러는 강조 표시된 명령문에서 오류를 생성합니다.
```java
static class StaticNestedClass {
    void accessMembers(OuterClass outer) {
        // Compiler error: Cannot make a static reference to the non-static
        //     field outerField
        System.out.println(outerField); // <- 여기
    }
}
```
오류를 해결하려면 객체 참조를 통해 `outerField`에 액세스하세요.
```java
    System.out.println(outer.outerField);
```
마찬가지로 최상위 클래스 `TopLevelClass`도 `outerField`에 직접 액세스할 수 없습니다.

## Shadowing

특정 범위(예: 내부 클래스 또는 메서드 정의)에 있는 타입(예: 멤버 변수 또는 매개 변수 이름) 선언이 바깥쪽 범위에 있는 다른 선언과 동일한 이름을 갖는 경우 선언은 둘러싸는 범위의 선언을 숨깁니다(shadows).  이름만으로는 숨겨진 선언을 참조할 수 없습니다. 다음 예제인 `ShadowTest`는 이를 보여줍니다.

```java
public class ShadowTest {
    
    public int x = 0;
    
    class FirstLevel {
    
        public int x = 1;
    
        void methodInFirstLevel(int x) {
            System.out.println("x = " + x);
            System.out.println("this.x = " + this.x);
            System.out.println("ShadowTest.this.x = " + ShadowTest.this.x);
        }
        
        public static void main(String... args) {
            ShadowTest st = new ShadowTest();
            ShadowTest.FirstLevel fl = st.new FirstLevel();
            fl.methodFirstLevel(23);
        }
    }
}

```
다음은 이 예제의 출력입니다.

```
x = 23
this.x = 1
ShadowTest.this.x = 0

```
이 예제에서는 x라는 세 가지 변수, 즉 `ShadowTest` 클래스의 멤버 변수, `FirstLevel` 내부 클래스의 멤버 변수, `methodInFirstLevel` 메서드의 매개 변수를 정의합니다. `methodInFirstLevel` 메소드의 매개변수로 정의된 변수 `x`는 내부 클래스 `FirstLevel`의 변수를 숨깁니다. 결과적으로, `methodInFirstLevel` 메소드에서 `x` 변수를 사용하면 이는 메소드 매개변수를 참조합니다. 내부 클래스 `FirstLevel`의 멤버 변수를 참조하려면 `this` 키워드를 사용하여 둘러싸는 범위를 나타냅니다.

```java
System.out.println("this.x = " + this.x);
```
더 큰 범위를 자신이 속한 클래스 이름으로 묶는 멤버 변수를 참조하세요. 예를 들어, 다음 문은 `methodInFirstLevel` 메서드에서 `ShadowTest` 클래스의 멤버 변수에 액세스합니다.

```java
System.out.println("ShadowTest.this.x = " + ShadowTest.this.x);
```

## Serialization

로컬 및 익명 클래스를 포함한 내부 클래스의 직렬화는 권장되지 않습니다. Java 컴파일러는 내부 클래스와 같은 특정 counstructs을 컴파일할 때 synthetic construucts을 생성합니다. 이는 소스 코드에 해당 construct가 없는 클래스, 메서드, 필드 및 기타 construct입니다. Synthetic construct을 사용하면 Java 컴파일러가 JVM을 변경하지 않고도 새로운 Java 언어 기능을 구현할 수 있습니다. 그러나 synthetic construct은 Java 컴파일러 구현마다 다를 수 있습니다. 즉, `.class` 파일도 구현마다 다를 수 있습니다. 따라서 내부 클래스를 직렬화한 다음 다른 JRE 구현으로 역직렬화하면 호환성 문제가 발생할 수 있습니다. 내부 클래스가 컴파일될 때 생성되는 synthetic construct에 대한 자세한 내용은 메서드 매개 변수 이름 얻기 섹션의 Implicit and Synthetic Parameters 섹션을 참조하세요.