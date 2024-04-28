
# Method References

람다 식을 사용하여 무명 메서드를 만듭니다. 그러나 경우에 따라 람다 식이 기존 메서드를 호출하는 것 외에는 아무 작업도 수행하지 않습니다. 이러한 경우 기존 메서드를 이름으로 참조하는 것이 더 명확한 경우가 많습니다. 메소드 참조를 사용하면 이를 수행할 수 있습니다. 이는 이미 이름이 있는 메서드에 대한 간결하고 읽기 쉬운 람다 식입니다.

람다 표현식 섹션에서 설명한 `Person` 클래스를 다시 고려해보세요.
```java
public class Person {
    // ...
    LocalDate birthday;
    public int getAge() {
        // ...
    }
    public LocalDate getBirthday() {
        return birthday;
    }
    
    public static int compareByAge(Person a, Person b) {
        return a.birthday.compareTo(b.birthday);
    }
    
    // ...
}
```
소셜 네트워킹 애플리케이션의 구성원이 배열에 포함되어 있고 배열을 연령별로 정렬하려고 한다고 가정합니다. 다음 코드를 사용할 수 있습니다(예제 MethodReferencesTest에서 이 섹션에 설명된 코드 발췌문 찾기).

```java
Person[] rosterAsArray = roster.toArray(new Person[roster.size()]);

class PersonAgeComparator implements Comparator<Person> {
    public int compare(Person a, Person b) {
        return a.getBirthday().compareTo(b.getBirthday());
    }
}

Arrays.sort(rosterAsArray, new PersonAgeComparator());

```
이 `sort` 호출의 메서드 시그니처는 다음과 같습니다.

```java
static <T> void sort(T[] a, Comparator<? super T> c)
```
`Comparator` 인터페이스는 functional 인터페이스라는 점에 유의하세요. 따라서 `Comparator`를 구현하는 클래스의 새 인스턴스를 정의한 다음 만드는 대신 람다 식을 사용할 수 있습니다.
```java
Arrays.sort(rosterAsArray,
    (Person a, Person b) -> {
        return a.getBirthday().compareTo(b.getBirthday());
    }
);
```
그러나 두 `Person` 인스턴스의 생년월일을 비교하는 이 메서드는 이미 `Person.compareByAge`로 존재합니다. 대신 람다 식의 본문에서 이 메서드를 호출할 수 있습니다.
```java
Arrays.sort(rosterAsArray, 
    (a, b) -> Person.compareByAge(a, b)
);

```
이 람다 식은 기존 메서드를 호출하므로 람다 식 대신 메서드 참조를 사용할 수 있습니다.
```java
Arrays.sort(rosterAsArray, Person::compareByAge);
```
메서드 참조 `Person::compareByAge`는 의미상 람다 식 `(a, b) -> Person.compareByAge(a, b)`와 동일합니다. 각각은 다음과 같은 특징을 가지고 있습니다.

- 형식 매개변수 목록은 `(Person, Person)`인 `Comparator<Person>.compare`에서 복사됩니다. 
- 해당 본문에서는 `Person.compareByAge` 메서드를 호출합니다.

## Kinds of Method References

메서드 참조에는 네 가지 종류가 있습니다.

<table summary="메서드 참조의 종류" border="1">
  <tbody><tr>
    <th id="h1">종류</th>
    <th id="h2">구문</th>
    <th id="h3">예제</th>
  </tr>

  <tr>
    <td headers="h1">정적 메서드에 대한 참조</td>
    <td headers="h2"><code><em>포함된클래스</em>::<em>정적메서드명</em></code></td>
    <td headers="h3"><code>Person::compareByAge</code><br>
        <code>MethodReferencesExamples::appendStrings</code></td>
  </tr>

  <tr>
    <td headers="h1">특정 객체의 인스턴스 메서드에 대한 참조</td>
    <td headers="h2"><code><em>포함된객체</em>::<em>인스턴스메서드명</em></code></td>
    <td headers="h3"><code>myComparisonProvider::compareByName</code><br>
        <code>myApp::appendStrings2</code></td>
  </tr>

  <tr>
    <td headers="h1">특정 타입의 임의 객체의 인스턴스 메서드에 대한 참조</td>
    <td headers="h2"><code><em>포함된타입</em>::<em>메서드명</em></code></td>
    <td headers="h3"><code>String::compareToIgnoreCase</code><br>
        <code>String::concat</code></td>
  </tr>

  <tr>
    <td headers="h1">생성자에 대한 참조</td>
    <td headers="h2"><code><em>클래스명</em>::new</code></td>
    <td headers="h3"><code>HashSet::new</code></td>
  </tr>

</tbody></table>


다음 예인 `MethodReferencesExamples`에는 처음 세 가지 유형의 메서드 참조에 대한 예가 포함되어 있습니다.

```java
import java.util.function.BiFunction;

public class MethodReferencesExamples {
    
    public static <T> T mergeThings(T a, T b, BiFunction<T, T, T> merger) {
        return merger.apply(a, b);
    }
    
    public static String appendStrings(String a, String b) {
        return a + b;
    }
    
    public String appendStrings2(String a, String b) {
        return a + b;
    }
    
    public static void main(String[] args) {
        
        MehtodReferencesExamples myApp = new MethodReferencesExamples();
        
        // Calling the method mergeThings with a lambda expression
        System.out.println(MethodReferencesExamples.mergeThings("Hello ", "World!", (a, b) -> a + b));
        
        // Reference to a static method
        System.out.println(MehtodReferencesExamples.mergeThings("Hello ", "World!", MethodReferencesExamples::appendStrings));
        
        // Reference to an instance method a particular object
        System.out.println(MethodReferncesExamples.mergeThings("Hello ", "World!", myApp::appendStrings2));
        
        // References to an instance method of an arbitrary object of a 
        // particular type
        System.out.println(MethodReferencesExamples.mergeThings("Hello ", "World!", String::concat));
    }
}

```
모든 `System.out.println()` 문은 동일한 내용을 인쇄합니다: `Hello World!`

`BiFunction`은 `java.util.function` 패키지의 많은 기능적 인터페이스 중 하나입니다. `BiFunction` 기능적 인터페이스는 두 개의 인수를 받아들이고 결과를 생성하는 람다 식 또는 메서드 참조를 나타낼 수 있습니다.

### Reference to a Static Method

메소드 참조 `Person::compareByAge` 및 `MethodReferencesExamples::appendStrings`는 정적 메소드에 대한 참조입니다.

### Reference to an Instance Method of a Particular Object

다음은 특정 개체의 인스턴스 메서드에 대한 참조의 예입니다.
```java
class ComparisonProvider {
    public int compareByName(Person a, Person b) {
        return a.getName().compareTo(b.getName());
    }
    public int compareByAge(Person a, Person b) {
        return a.getBirthday().compareTo(b.getBirthday());
    }
}
ComparisonProvider myComparisonProvider = new ComparisonProvider();
Arrays.sort(rosterAsArray, myComparisonProvider::compareByName);
```
메소드 참조 `myComparisonProvider::compareByName`은 `myComparisonProvider` 객체의 일부인 `compareByName` 메소드를 호출합니다. JRE는 메소드 유형 인수를 유추하며, 이 경우에는 `(Person, Person)`입니다.

마찬가지로, 메소드 참조 `myApp::appendStrings2`는 `myApp` 객체의 일부인 `appendStrings2` 메소드를 호출합니다. JRE는 메소드 유형 인수를 유추하며, 이 경우에는 `(String, String)`입니다.

### Reference to an Instance Method of an Arbitrary Object of a Particular Type

다음은 특정 유형의 임의 객체의 인스턴스 메서드에 대한 참조의 예입니다.
```java
String[] stringArray = { "Barbara", "James", "Mary", "John",
        "Patricia", "Robert", "Michael", "Linda" };
Arrays.sort(stringArray, String::compareToIgnoreCase);
```
메서드 참조 `String::compareToIgnoreCase`에 해당하는 람다 식에는 형식적인 매개 변수 리스트 `(String a, String b)` 이 있습니다. 여기서 `a`와 `b`는 이 예제를 더 잘 설명하는 데 사용되는 임의의 이름입니다. 메소드 참조는 `a.compareToIgnoreCase(b)` 메소드를 호출합니다.

마찬가지로 메서드 참조 `String::concat`은 `a.concat(b)` 메서드를 호출합니다.

### Reference to a Constructor

`new`라는 이름을 사용하여 정적 메서드와 동일한 방식으로 생성자를 참조할 수 있습니다. 다음 메서드는 한 컬렉션의 요소를 다른 컬렉션으로 복사합니다.

```java
public static <T, SOURCE extends Collection<T>, DEST extends Collection<T>> DEST transferElements(SOURCE sourceCollection, Supplier<DEST> collectionFactory) {
    DEST result = collectionFactory.get();
    for (T t : sourceCollection) {
        result.add(t);
    }
    return result;
}
```
functional 인터페이스 `Supplier`에는 인수를 사용하지 않고 객체를 반환하는 하나의 get 메서드가 포함되어 있습니다. 결과적으로 다음과 같이 람다 식을 사용하여 `transferElements` 메서드를 호출할 수 있습니다.
```java
Set<Person> rosterSetLambda = transferElements(roster, () -> { return new HashSet<>(); });

```
다음과 같이 람다 식 대신 생성자 참조를 사용할 수 있습니다.
```java
Set<Person> rosterSet = transferElements(roster, HashSet::new);

```
Java 컴파일러는 사용자가 `Person` 유형의 요소를 포함하는 `HashSet` 컬렉션을 생성하려고 한다고 추론합니다. 또는 다음과 같이 지정할 수 있습니다.
```java
Set<Person> rosterSet = transferElements(roster, HashSet<Person>::new);

```