
# Lambda Expressions

익명 클래스의 한 가지 문제는 익명 클래스의 구현이 메서드가 하나만 포함된 인터페이스와 같이 매우 간단한 경우 익명 클래스의 syntax가 다루기 힘들고 명확하지 않게 보일 수 있다는 것입니다. 이러한 경우 일반적으로 누군가가 버튼을 클릭할 때 어떤 작업을 수행해야 하는지와 같은 기능을 다른 메서드에 인수로 전달하려고 합니다. 람다 표현식을 사용하면 기능을 메서드 인수로 처리하거나 코드를 데이터로 처리할 수 있습니다.

이전 섹션인 익명 클래스에서는 이름을 지정하지 않고 기본 클래스를 구현하는 방법을 보여줍니다. 이는 명명된 클래스보다 더 간결한 경우가 많지만 메서드가 하나만 있는 클래스의 경우 익명 클래스라도 다소 과도하고 번거로워 보입니다. 람다 표현식을 사용하면 단일 메서드 클래스의 인스턴스를 더 간결하게 표현할 수 있습니다.

이 섹션에서는 다음 주제를 다룹니다.

- 람다 표현식의 이상적인 사용 사례
  - 접근 방식 1: 하나의 특성과 일치하는 멤버를 검색하는 메서드 만들기
  - 접근 방식 2: 더욱 일반화된 검색 방법 만들기
  - 접근 방식 3: 로컬 클래스에 검색 기준 코드 지정
  - 접근 방식 4: 익명 클래스에 검색 기준 코드 지정
  - 접근 방식 5: 람다 표현식을 사용하여 검색 기준 코드 지정
  - 접근 방식 6: 람다 표현식과 함께 표준 기능 인터페이스 사용
  - 접근 방식 7: 애플리케이션 전체에서 람다 표현식 사용
  - 접근법 8: 제네릭을 보다 광범위하게 사용
  - 접근 방식 9: 람다 표현식을 매개변수로 허용하는 집계 작업 사용

- GUI 애플리케이션의 람다 표현식

- 람다 표현식의 Syntax

- 둘러싸는 범위의 지역 변수에 액세스

- 대상 입력
  - 대상 type 및 메서드 인수

- 직렬화

## Ideal UseCase for Lambda Expressions

소셜 네트워킹 애플리케이션을 만들고 있다고 가정해 보겠습니다. 관리자가 특정 기준을 충족하는 소셜 네트워킹 애플리케이션의 구성원에 대해 메시지 보내기와 같은 모든 종류의 작업을 수행할 수 있는 기능을 만들고 싶습니다. 다음 표에서는 이 사용 사례를 자세히 설명합니다.

<table border="1" summary="소셜 네트워킹 애플리케이션의 사용 사례">
  <tbody><tr>
    <th id="h1">필드</th>
    <th id="h2">설명</th>
  </tr>

  <tr>
    <td headers="h1">이름</td>
    <td headers="h2">선택된 회원에 대한 작업 수행</td>
  </tr>

  <tr>
    <td headers="h1">주요 수행자</td>
    <td headers="h2">관리자</td>
  </tr>

  <tr>
     <td headers="h1">사전 조건</td>
     <td headers="h2">관리자가 시스템에 로그인되어 있어야 합니다.</td>
  </tr>

  <tr>
    <td headers="h1">사후 조건</td>
    <td headers="h2">지정된 기준에 부합하는 회원에 대해서만 작업이 수행됩니다.</td>
  </tr>

  <tr>
    <td headers="h1">주요 성공 시나리오</td>
    <td headers="h2">
      <ol>
        <li>관리자는 특정 작업을 수행할 회원의 기준을 지정합니다.</li>
        <li>관리자는 선택된 회원에 대해 수행할 작업을 지정합니다.</li>
        <li>관리자는 <strong>제출</strong> 버튼을 선택합니다.</li>
        <li>시스템은 지정된 기준과 일치하는 모든 회원을 찾습니다.</li>
        <li>시스템은 일치하는 모든 회원에 대해 지정된 작업을 수행합니다.</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td headers="h1">확장</td>
    <td headers="h2">
      <p>1a. 관리자는 지정된 기준과 일치하는 회원을 미리보기할 수 있으며, 이를 통해 수행할 작업을 지정하거나 <strong>제출</strong> 버튼을 선택하기 전에 미리 확인할 수 있습니다.</p>
    </td>
  </tr>

  <tr>
    <td headers="h1">발생 빈도</td>
    <td headers="h2">하루에 여러 번 발생합니다.</td>
  </tr>
</tbody></table>

이 소셜 네트워킹 애플리케이션의 구성원이 다음 Person 클래스로 표시된다고 가정합니다.

```java
public class Person {
    
    public enum Sex {
        MALE, FEMALE
    }
    
    String name;
    LocalDate birthday;
    Sex gender;
    String emailAddress;
    
    public int getAge() {
        // ...
    }
    
    public void printPerson() {
        // ...
    }
}
```

소셜 네트워킹 애플리케이션의 구성원이 `List<Person>` 인스턴스에 저장되어 있다고 가정합니다.

이 섹션은 이 사용 사례에 대한 단순한 접근 방식으로 시작됩니다. 로컬 및 익명 클래스를 사용하여 이 접근 방식을 개선한 다음 람다 식을 사용하여 효율적이고 간결한 접근 방식으로 마무리합니다. 
`RosterTest` 예제에서 이 섹션에 설명된 코드 발췌문을 찾아보세요.

## Approach 1: Create Methods That Search for Members That Match One Characteristic

한 가지 단순한 접근 방식은 여러 메서드를 만드는 것입니다. 각 방법은 성별, 연령 등 하나의 특성과 일치하는 회원을 검색합니다. 다음 메소드는 지정된 연령보다 오래된 구성원을 인쇄합니다.

```java
public static void printPersonOlderThan(List<Person> roster, int age) {
    for (Person p : roster) {
        if (p.getAge() >= age) {
            p.printPerson();
        }
    }
}
```
참고: `List`은 순서가 지정된 `Collection`입니다. 컬렉션은 여러 요소를 하나의 단위로 그룹화하는 개체입니다. 컬렉션은 집계 데이터를 저장, 검색, 조작 및 전달하는 데 사용됩니다. 컬렉션에 대한 자세한 내용은 컬렉션 트레일을 참조하세요.

이 접근 방식은 잠재적으로 애플리케이션을 취약하게 만들 수 있으며, 이는 업데이트(예: 최신 데이터 type) 도입으로 인해 애플리케이션이 작동하지 않을 가능성이 있습니다. 애플리케이션을 업그레이드하고 다른 멤버 변수를 포함하도록 `Person` 클래스의 구조를 변경한다고 가정해 보겠습니다. 아마도 class에서는 다른 데이터 type이나 알고리즘을 사용하여 연령을 기록하고 측정할 수도 있습니다. 이러한 변경 사항을 수용하려면 많은 API를 다시 작성해야 합니다. 게다가 이 접근 방식은 불필요하게 제한적입니다. 예를 들어, 특정 연령보다 어린 회원을 인쇄하고 싶다면 어떻게 해야 할까요?

## Approach 2: Create More Generalized Search Methods

다음 메소드는 `printPersonsOlderThan`보다 더 일반적(more generic)입니다. 지정된 연령 범위 내의 구성원을 인쇄합니다.

```java
public static void printPersonsWithAgeRange(
    List<Person> roster, int low, int high) {
    for (Person p: roster) {
        if (low <=p.getAge() && p.getAge() < high) {
            p.printPerson();
        }
    }
}
```

특정 성별의 회원 또는 특정 성별과 연령 범위의 조합을 인쇄하려면 어떻게 해야 합니까? `Person` 클래스를 변경하고 관계 상태나 지리적 위치와 같은 다른 속성을 추가하기로 결정했다면 어떻게 될까요? 이 메서드는 `printPersonsOlderThan`보다 더 일반적이지만 가능한 각 검색 쿼리에 대해 별도의 메서드를 만들려고 하면 여전히 취약한 코드가 발생할 수 있습니다. 대신 다른 클래스에서 검색하려는 기준을 지정하는 코드를 분리할 수 있습니다.

## Approach 3: Specify Search Criteria Code in a Local Class

다음 메소드는 지정한 검색 기준과 일치하는 멤버를 인쇄합니다.
```java
public static void printPersons(
    List<Person> roster, CheckPerson tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
}

```
이 메소드는 `tester.test` 메소드를 호출하여 `List` 매개변수 목록에 포함된 각 `Person` 인스턴스가 `CheckPerson` 매개변수 테스터에 지정된 검색 기준을 만족하는지 여부를 확인합니다. `tester.test` 메소드가 참값을 반환하면 `printPersons` 메소드가 `Person` 인스턴스에서 호출됩니다.

검색 기준을 지정하려면 `CheckPerson` 인터페이스를 구현합니다.
```java
interface CheckPerson {
    boolean test(Person p);
}
```
다음 클래스는 테스트 메서드에 대한 구현을 지정하여 `CheckPerson` 인터페이스를 구현합니다. 이 메소드는 미국에서 징병 대상이 되는 구성원을 필터링합니다. `Person` 매개변수가 남성이고 18세에서 25세 사이인 경우 참값을 반환합니다.

```java
class CheckPersonEligibleForSelectiveService implements CheckPerson {
    public boolean test(Person p) {
        return p.gender == Person.Sex.MALE && p.getAge() >= 18 && p.getAge() <= 25;
    }
}
```
이 클래스를 사용하려면 새 인스턴스를 만들고 `printPersons` 메서드를 호출합니다.

```java
printPersons(
    roster, 
    new CheckPersonEligibleForSelectiveService());
```
이 접근 방식은 덜 불안정하지만(`Person`의 구조를 변경하는 경우 메서드를 다시 작성할 필요가 없음) 여전히 추가 코드가 있습니다. 즉, 애플리케이션에서 수행하려는 각 검색에 대한 새 인터페이스와 로컬 클래스가 있습니다. `CheckPersonEligibleForSelectiveService`는 인터페이스를 구현하므로 로컬 클래스 대신 익명 클래스를 사용할 수 있으며 각 검색에 대해 새 클래스를 선언할 필요가 없습니다.

## Approach 4: Specify Search Criteria Code in an Anonymous Class

`printPersons` 메소드의 다음 호출에 대한 인수 중 하나는 미국에서 선택적 병역에 적합한 구성원, 즉 18세에서 25세 사이의 남성을 필터링하는 익명 클래스입니다.
```java
printPersons(
    roster, 
    new CheckPerson() {
        public boolean test(Person p){
            return p.getGender() == Person.Sex.MALE && p.getAge() >= 18 && p.getAge() <= 25;
    }
  }
);
```
이 접근 방식을 사용하면 수행하려는 각 검색에 대해 새 클래스를 만들 필요가 없기 때문에 필요한 코드 양이 줄어듭니다. 그러나 `CheckPerson` 인터페이스에 메서드가 하나만 포함되어 있다는 점을 고려하면 익명 클래스의 구문은 부피가 큽니다. 이 경우 다음 섹션에 설명된 대로 익명 클래스 대신 람다 식을 사용할 수 있습니다.

## Approach 5: Specify Search Criteria Code with a Lambda Expression

`CheckPerson` 인터페이스는 기능적 인터페이스입니다. functional 인터페이스는 하나의 추상 메서드만 포함하는 인터페이스입니다. (함수형(functional) 인터페이스에는 하나 이상의 default 메서드 또는 정적 메서드가 포함될 수 있습니다.) functional 인터페이스에는 추상 메서드가 하나만 포함되어 있으므로 구현할 때 해당 메서드의 이름을 생략할 수 있습니다. 이렇게 하려면 익명 클래스 식을 사용하는 대신 다음 메서드 호출에서 강조 표시된 람다 식을 사용합니다.
```java
printPersons(
        roster,
        (Person p) -> p.getGender() == Person.Sex.MALE && p.getAge() >= 18 && p.getAge() <= 25
);
```
람다 식을 정의하는 방법에 대한 자세한 내용은 람다 식 구문을 참조하세요.

`CheckPerson` 인터페이스 대신 표준 기능 인터페이스를 사용할 수 있으며, 이는 필요한 코드 양을 더욱 줄여줍니다.

## Approach 6: Use Standard Functional Interfaces with Lambda Expressions

CheckPerson 인터페이스를 다시 생각해 보세요:
```java
interface CheckPerson {
    boolean test(Person p);
}

```
이것은 매우 간단한 인터페이스입니다. 하나의 추상 메소드만 포함하므로 기능적 인터페이스입니다. 이 메서드는 하나의 매개 변수를 사용하고 부울 값을 반환합니다. 이 방법은 너무 간단해서 애플리케이션에서 정의하는 것이 가치가 없을 수도 있습니다. 결과적으로 JDK는 `java.util.function` 패키지에서 찾을 수 있는 여러 표준 기능 인터페이스를 정의합니다.

예를 들어 `CheckPerson` 대신 `Predicate<T>` 인터페이스를 사용할 수 있습니다. 이 인터페이스에는 `boolean test(T t)` 메소드가 포함되어 있습니다.

```java
interface Predicate<Person> {
    boolean test(Person t);
}

```
`CheckPerson.boolean test(Person p)`와 동일한 반환 유형 및 매개변수를 갖는 메소드가 포함되어 있습니다. 결과적으로 다음 메서드에서 보여 주는 것처럼 `CheckPerson` 대신 `Predicate<T>`를 사용할 수 있습니다.
```java
public static void printPersonsWithPredicate(
    List<Person> roster, Predicate<Person> tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
    
}

```
결과적으로 다음 메소드 호출은 접근 방식 3에서 `printPersons`를 호출할 때와 동일합니다. 선택적 서비스에 적합한 구성원을 얻기 위해 로컬 클래스에 검색 기준 코드를 지정합니다.
```java
printPersonWithPredicate(
    roster,
    p -> p.getGender() == Person.Sex.MALE && p.getAge() >= 18 && p.getAge() <= 25 
);
```
이 메서드에서 람다 식을 사용할 수 있는 유일한 위치는 아닙니다. 다음 접근 방식은 람다 식을 사용하는 다른 방법을 제안합니다.

## Approach 7: Use Lambda Expressions Throughout Your Application

람다 표현식을 사용할 수 있는 다른 곳을 알아보려면 `printPersonsWithPredicate` 메소드를 다시 생각해 보세요.
```java
public static void printPersonsWithPredicate(
    List<Person> roster, Predicate<Person> tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
 }
```
이 메소드는 `List` 매개변수 목록에 포함된 각 `Person` 인스턴스가 `Predicate` 매개변수 `tester`에 지정된 기준을 충족하는지 확인합니다. `Person` 인스턴스가 `tester`가 지정한 기준을 충족하면 `printPerson` 메소드가 `Person` 인스턴스에서 호출됩니다.

`printPerson` 메소드를 호출하는 대신 `tester`가 지정한 기준을 충족하는 `Person` 인스턴스에 대해 수행할 다른 작업을 지정할 수 있습니다. 람다 식을 사용하여 이 작업을 지정할 수 있습니다. 하나의 인수(`Person` type의 객체)를 취하고 void를 반환하는 `printPerson`과 유사한 람다 표현식을 원한다고 가정해 보겠습니다. 람다 식을 사용하려면 기능적 인터페이스를 구현해야 한다는 점을 기억하세요. 이 경우 `Person` 유형의 인수 하나를 취하고 void를 반환할 수 있는 추상 메서드가 포함된 기능적 인터페이스가 필요합니다. `Consumer<T>` 인터페이스에는 이러한 특성을 갖는 `void accept(T t)` 메서드가 포함되어 있습니다. 다음 메소드는 `p.printPerson()` 호출을 `accept` 메소드를 호출하는 `Consumer<Person>` 인스턴스로 대체합니다.

```java
public static void processPersons(
    List<Person> roster,
    Predicate<Person> tester,
    Consumer<Person> block) {
        for (Person p : roster) {
            if (tester.test(p)) {
                block.accepet(p);
            }
        }
}
```
결과적으로 다음 메소드 호출은 접근법 3에서 `printPersons`를 호출할 때와 동일합니다. 로컬 클래스에 검색 기준 코드를 지정하여 선택적 복무에 적합한 구성원을 얻습니다. 멤버를 인쇄하는 데 사용되는 람다 표현식이 강조 표시됩니다.

```java
processPersons(
        roster,
        p -> p.getGender() == Person.Sex.MALE && p.getAge() >= 18 && p.getAge() <= 25, 
        p -> p.printPerson()
);
```

회원의 프로필을 인쇄하는 것보다 더 많은 작업을 수행하고 싶다면 어떻게 해야 합니까? 회원의 프로필을 확인하거나 연락처 정보를 검색하고 싶다고 가정해 보세요. 이 경우 값을 반환하는 추상 메서드가 포함된 functional 인터페이스가 필요합니다. `Function<T,R>` 인터페이스에는 `R apply(T t)` 메서드가 포함되어 있습니다. 다음 메서드는 매개변수 `mapper`에 의해 지정된 데이터를 검색한 다음 매개변수 `block`에 지정된 데이터에 대해 작업을 수행합니다.
```java
public static void processPersonsWithFunction(
    List<Person> roster,
    Predicate<Person> tester,
    Function<Person, String> mapper,
    Consumer<String> block) {
    for (Person p : roster) {
        if (tester.test(p)) {
            String data = mapper.apply(p);
            block.accept(data);
        }
    }
}
```
다음 방법은 선발 복무 대상자 명단에 포함된 각 구성원의 이메일 주소를 검색한 후 인쇄합니다.

```java
processPersonsWithFunction(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);
```

## Approach 8: Use Generics More Extensively

processPersonsWithFunction 메소드를 다시 생각해 보세요. 다음은 모든 데이터 유형의 요소를 포함하는 컬렉션을 매개변수로 허용하는 일반 버전입니다.

```java
public static <X, Y> void processElements(
    Iterable<X> source,
    Predicate<X> tester,
    Function <X, Y> mapper,
    Consumer<Y> block) {
    for (X p : source) {
        if (tester.test(p)) {
            Y data = mapper.apply(p);
            block.accept(data);
        }
    }
}
```
징병 대상 회원의 이메일 주소를 출력하려면 다음과 같이 `processElements` 메소드를 호출하십시오.
```java
processElements(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);
```

이 메소드 호출은 다음 작업을 수행합니다.

1. 컬렉션 `source`에서 개체의 소스를 가져옵니다. 이 예에서는 컬렉션 목록에서 `Person` 개체의 소스를 얻습니다. `List` 유형의 컬렉션인 `roster`, `List`은 `Iterable` 유형의 객체이기도 합니다.
2. `Predicate` 개체 `tester`와 일치하는 개체를 필터링합니다. 이 예에서 `Predicate` 개체는 Selective Service에 적합한 구성원을 지정하는 람다 식입니다.
3. 필터링된 각 개체를 `Function` 개체 매퍼에서 지정한 값에 매핑합니다. 이 예제에서 `Function` 개체는 구성원의 전자 메일 주소를 반환하는 람다 식입니다.
4. `Consumer` 개체 `block`에 지정된 대로 매핑된 각 개체에 대해 작업을 수행합니다. 이 예제에서 `Consumer` 개체는 `Function` 개체가 반환한 전자 메일 주소인 문자열을 인쇄하는 람다 식입니다.

이러한 각 작업을 집계 작업으로 대체할 수 있습니다.

## Approach 9: Use Aggregate Operations That Accept Lambda Expressions as Parameters

다음 예에서는 집계 작업을 사용하여 징병 대상 자격이 있는 수집 `roster`에 포함된 구성원의 전자 메일 주소를 인쇄합니다.

```java
roster
    .stream()
    .filter(
        p -> p.getGender() == Person.Sex.MALE 
            && p.getAge() >= 18
            && p.getAge() <= 25)
    .map(p -> p.getEmailAddress())
    .forEach(email -> System.out.println(email));

```

다음 표는 processElements 메서드가 해당 집계 작업을 통해 수행하는 각 작업을 매핑합니다.

<table border="1" summary="메서드 processElements가 수행하는 각 동작에 대한 대응되는 집계 작업">
<tbody><tr>
  <th id="h101"><code>processElements</code> 동작</th>
  <th id="h102">집계 작업</th>
</tr>

<tr>
  <td headers="h101">객체 소스 획득</td>

  <td headers="h102"><code>Stream&lt;E&gt; <strong>stream</strong>()</code></td>
</tr>

<tr>
  <td headers="h101"><code>Predicate</code> 객체와 일치하는 객체 필터링</td>
  <td headers="h102"><code>Stream&lt;T&gt; <strong>filter</strong>(Predicate&lt;? super T&gt; predicate)</code></td>
</tr>

<tr>

  <td headers="h101"><code>Function</code> 객체에 의해 지정된 다른 값으로 객체 매핑</td>

  <td headers="h102"><code>&lt;R&gt; Stream&lt;R&gt; <strong>map</strong>(Function&lt;? super T,? extends R&gt; mapper)</code></td>

</tr>

<tr>
  <td headers="h101"><code>Consumer</code> 객체에 의해 지정된 동작 수행</td>
  <td headers="h102"><code>void <strong>forEach</strong>(Consumer&lt;? super T&gt; action)</code></td>
</tr>

</tbody></table>

작업 `filter`, `map` 및 `forEach`는 집계 작업입니다. 집계 작업은 컬렉션에서 직접 요소를 처리하는 것이 아니라 `stream`에서 요소를 처리합니다. 이것이 바로 이 예제에서 호출된 첫 번째 메서드가 `stream`인 이유입니다. 스트림은 일련의 요소입니다. 컬렉션과 달리 요소를 저장하는 데이터 구조가 아닙니다. 대신 스트림은 파이프라인을 통해 컬렉션과 같은 소스의 값을 전달합니다. 파이프라인은 일련의 스트림 작업이며, 이 예에서는 `filter`-`map`-`forEach`입니다. 또한 집계 작업은 일반적으로 람다 식을 매개 변수로 허용하므로 동작 방식을 사용자 지정할 수 있습니다.

집계 작업에 대한 자세한 내용은 집계 작업 강의를 참조하세요.

## Syntax of Lambda Expressions

람다 식은 다음으로 구성됩니다.

- 괄호 안에 쉼표로 구분된 형식 매개변수 목록입니다. `CheckPerson.test` 메서드에는 `Person` 클래스의 인스턴스를 나타내는 하나의 매개 변수 `p`가 포함되어 있습니다.

- 참고: 람다 식에서 매개 변수의 데이터 형식을 생략할 수 있습니다. 또한, 매개변수가 하나만 있는 경우 괄호를 생략할 수 있습니다. 예를 들어 다음 람다 식도 유효합니다.

```java
p -> p.getGender() == Person.Sex.MALE 
    && p.getAge() >= 18
    && p.getAge() <= 25
```
- 화살 토큰 `->`
- 단일 표현식 또는 명령문 블록으로 구성된 본문입니다. 이 예에서는 다음 표현식을 사용합니다.
```java
p.getGender() == Person.Sex.MALE 
    && p.getAge() >= 18
    && p.getAge() <= 25
```
단일 표현식을 지정하면 Java 런타임은 표현식을 평가한 다음 해당 값을 반환합니다. 또는 return 문을 사용할 수 있습니다.
```java
p -> {
    return p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25;
}
```
return 문은 표현식이 아닙니다. 람다 식에서는 명령문을 중괄호(`{}`)로 묶어야 합니다. 그러나 void 메서드 호출을 중괄호로 묶을 필요는 없습니다. 예를 들어 다음은 유효한 람다 식입니다.
```java
email -> System.out.println(email)

```
람다 식은 메서드 선언과 매우 유사합니다. 람다 식을 익명 메서드, 즉 이름이 없는 메서드로 간주할 수 있습니다

다음 예인 `Calculator`는 둘 이상의 형식 매개 변수를 사용하는 람다 식의 예입니다.

```java
public class Calculator {
    interface IntegerMath {
        int operation(int a, int b);
    }
    
    public int operateBinary(int a, int b, IntegerMath op){
        return op.operation(a, b);
    }
    
    public static void main(String... args) {
        Calculator myApp = new Calculator();
        IntegerMath addition = (a, b) -> a + b;
        IntegerMath subtraction = (a, b) -> a - b;
        System.out.println("40 + 2 = " + myApp.operateBinary(40, 2, addition));
        System.out.println("20 - 10 = " + myApp.operateBinary(20, 10, subtraction));
    }
}
```
`OperateBinary` 메소드는 두 개의 정수 피연산자에 대해 수학 연산을 수행합니다. 연산 자체는 `IntegerMath`의 인스턴스에 의해 지정됩니다. 이 예제에서는 람다 식, `addition` 및 `subtraction`를 사용하여 두 가지 연산을 정의합니다. 예제에서는 다음을 인쇄합니다.
```
40 + 2 = 42
20 - 10 = 10
```

## Accessing Local Variables of the Enclosing Scope

로컬 및 익명 클래스와 마찬가지로 람다 식은 변수를 캡처할 수 있습니다. 그들은 둘러싸는 범위의 지역 변수에 대해 동일한 액세스 권한을 갖습니다. 그러나 로컬 및 익명 클래스와 달리 람다 식에는 섀도잉 문제가 없습니다(자세한 내용은 섀도잉 참조). 람다 식은 lexical scope가 지정됩니다. 이는 supertype에서 이름을 상속받지 않거나 새로운 범위 지정 수준을 도입하지 않음을 의미합니다. 람다 식의 선언은 바깥쪽 환경에서와 마찬가지로 해석됩니다. 다음 예제인 `LambdaScopeTest`는 이를 보여줍니다.

```java
import java.util.function.Consumer;

public class LambdaScopeTest {
    
    public int x = 0;
    
    class FirstLevel {
    
        public int x = 1;
    
        void methodInFirstLevel(int x) {
            
            int z = 2;
            
            Consumer<Integer> myConsumer = (y) -> {
              // The following statement causes the compiler to generate
              // the error "Local variable z defined in an enclosing scope
              // must be final or effectively final" 
              //
              // z = 99;
              System.out.println("x = " + x);
              System.out.println("y = " + y);
              System.out.println("z = " + z);
              System.out.println("this.x = " + this.x);
              System.out.println("LambdaScopeTest.this.x = " +
                      LambdaScopeTest.this.x);
            };
            
            myConsumer.accept(x);
        
        }
    }
    
    public static void main(String... args) {
        LambdaScopeTest st = new LambdaScopeTest();
        LambdaScopeTest.FirstLevel fl = st.new FirstLevel();
        fl.methodFirstLevel(23);
    }
}

```
이 예에서는 다음 출력을 생성합니다.
```
x = 23
y = 23
z = 2
this.x = 1
LambdaScopeTest.this.x = 0
```
람다 식 `myConsumer`의 선언에서 `y` 대신 매개 변수 `x`로 대체하면 컴파일러에서 오류가 생성됩니다.
```java
Consumer<Integer> myConsumer = (x) -> {
    // ...
}
```
람다 식이 새로운 수준의 범위 지정을 도입하지 않기 때문에 컴파일러는 "람다 식의 매개 변수 `x`는 바깥쪽 범위에 정의된 다른 지역 변수를 다시 선언할 수 없습니다"라는 오류를 생성합니다. 결과적으로, 포함 범위의 필드, 메소드 및 지역 변수에 직접 액세스할 수 있습니다. 예를 들어 람다 식은 `methodInFirstLevel` 메서드의 매개 변수 `x`에 직접 액세스합니다. 바깥쪽 클래스의 변수에 액세스하려면 `this` 키워드를 사용하세요. 이 예에서 `this.x`는 멤버 변수 `FirstLevel.x`를 참조합니다.

그러나 로컬 및 익명 클래스와 마찬가지로 람다 식은 `final` 또는 `사실상 final`인 바깥쪽 블록의 로컬 변수 및 매개 변수에만 액세스할 수 있습니다. 이 예에서 변수 `z`는 `사실상 final` 변수입니다. 초기화된 후에는 해당 값이 변경되지 않습니다. 그러나 람다 식 `myConsumer`에 다음 할당 문을 추가한다고 가정해 보겠습니다.

```java
Consumer<Integer> myConsumer = (y) -> {
    z = 99;
    // ...
}
```
이 할당문으로 인해 변수 `z`는 더 이상 `사실상 final` 변수가 아닙니다. 결과적으로 Java 컴파일러는 "외부 범위에 정의된 로컬 변수 `z`는 `final`이거나 `사실상 final`이어야 합니다."와 유사한 오류 메시지를 생성합니다.

## Target Typing

람다 식의 type을 어떻게 결정합니까? 18세에서 25세 사이의 남성 구성원을 선택한 람다 표현식을 떠올려 보세요.

```java
p -> p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25
```
이 람다 식은 다음 두 가지 메서드에 사용되었습니다.

- 접근 방식 3: 로컬 클래스에 검색 기준 코드 지정의 `public static void printPersons(List<Person> roster, CheckPerson tester)`

- 접근 방식 6: 람다 표현식과 함께 표준 기능 인터페이스 사용의 `public void printPersonsWithPredicate(List<Person> roster, Predicate<Person> tester)`

Java 런타임이 `printPersons` 메소드를 호출할 때 `CheckPerson`의 데이터 유형을 예상하므로 람다 표현식은 이 유형입니다. 그러나 Java 런타임이 `printPersonsWithPredicate` 메소드를 호출할 때 `Predicate<Person>`의 데이터 유형을 예상하므로 람다 표현식은 이 타입입니다. 이러한 메소드가 기대하는 데이터 유형을 대상 타입이라고 합니다. 람다 표현식의 타입을 결정하기 위해 Java 컴파일러는 람다 표현식이 발견된 컨텍스트 또는 상황의 대상 타입을 사용합니다. 따라서 Java 컴파일러가 대상 타입을 결정할 수 있는 상황에서만 람다 식을 사용할 수 있습니다.

 - 변수 선언

 - 할당

 - 반환문

 - 배열 초기화 코드

 - 메서드 또는 생성자 인수

 - 람다 표현 본문

 - 조건식, `?:`

 - 캐스트 표현

### Target Types and Method Arguments

메소드 인수의 경우 Java 컴파일러는 오버로드 resolution 및 타입 인수 추론이라는 두 가지 다른 언어 기능을 사용하여 대상 타입을 결정합니다.

다음 두 가지 함수형 인터페이스(`java.lang.Runnable` 및 `java.util.concurrent.Callable<V>`)를 고려하십시오.

```java
public interface Runnable {
    void run();
}

public interface Callable<V> {
    V call();
}
```
`Runnable.run` 메서드는 값을 반환하지 않지만 `Callable<V>.call`은 값을 반환합니다. 다음과 같이 메소드 `invoke`을 오버로드했다고 가정합니다(메소드 오버로드에 대한 자세한 내용은 메소드 정의 참조).
```java
void invoke(Runnable r) {
    r.run();
}

<T> T invoke(Callable<T> c) {
    return c.call();
}
```
다음 명령문에서는 어떤 메소드가 호출됩니까?
```java
String s = invoke(() -> "done");
```
`invoke(Callable<T>)` 메소드는 값을 반환하기 때문에 호출됩니다. `invoke(Runnable)` 메소드는 그렇지 않습니다. 이 경우 람다 식 `() -> "done"`의 유형은 `Callable<T>`입니다.

## Serialization

대상 유형과 캡처된 인수가 직렬화 가능한 경우 람다 식을 직렬화할 수 있습니다. 그러나 내부 클래스와 마찬가지로 람다 식의 직렬화는 권장되지 않습니다.
