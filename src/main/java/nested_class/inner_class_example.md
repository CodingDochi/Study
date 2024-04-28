
# Inner Class Example

사용 중인 내부 클래스를 보려면 먼저 배열을 고려하십시오. 다음 예에서는 배열을 만들고 정수 값으로 채운 다음 배열의 짝수 인덱스 값만 오름차순으로 출력합니다.

다음 `DataStructure.java` 예제는 다음으로 구성됩니다.

- 연속된 정수 값(0, 1, 2, 3 등)으로 채워진 배열을 포함하는 `DataStructure` 인스턴스를 생성하기 위한 생성자와 짝수 인덱스가 있는 배열 요소를 인쇄하는 메서드를 포함하는 `DataStructure` 외부 클래스.
- `Iterator< Integer>` 인터페이스를 확장하는 `DataStructureIterator` 인터페이스를 구현하는 `EvenIterator` 내부 클래스입니다. 반복자는 데이터 구조를 단계별로 실행하는 데 사용되며 일반적으로 마지막 요소를 테스트하고, 현재 요소를 검색하고, 다음 요소로 이동하는 메서드를 갖습니다.
- `DataStructure` 객체(`ds`)를 인스턴스화한 다음 `printEven` 메소드를 호출하여 짝수 인덱스 값을 갖는 `arrayOfInts` 배열의 요소를 인쇄하는 `main` 메소드입니다.

```java

public class DataStructure {
    
    // Create an array
    private final static int SIZE = 15;
    private int[] arrayOfInts = new int[SIZE];
    
    public DataStructure() {
        //fill the array with ascending integer values
        for (int i = 0; i < SIZE; i++) {
            arrayOfInts[i] = i;
        }
    }
    
    public void printEven() {
        
        // Print out values of even indices of the array
        DataStructureIterator iterator = this.new EvenIterator();
        while(iterator.hasNext()) {
            System.out.println(iterator.next() + " ");
        }
        System.out.println();
    }
    
    interface DataStructureIterator extends java.util.Iterator<Integer> {
    }
    
    // Inner class implements the DataStructureIterator interface, 
    // which extends the Iterator<Integer> interface
    
    private class EvenIterator implements DataStructureIterator {
        
        // Start stepping through the array from the beginning
        private int nextIndex = 0;
        
        public boolean hasNext() {
            
            // Check if the current element is the last in the array
            return (nextIndex <= SIZE -1);
        }
        
        public Integer next() {
            
            // Record a value an even index of the array
            Integer retValue = Integer.valueOf(arrayOfInts[nextIndex]);
            
            // Get the next even element
            nextIndex += 2;
            return retValue;
        }
    }
    
    public static void main(String s[]) {
        
        // Fill the array with integer values and print out only
        // values of even indices
        DataStructure ds = new DataStructure();
        ds.printEven();
    }
}

```
출력은 다음과 같습니다
```
0 2 4 6 8 10 12 14 
```

`EvenIterator` 클래스는 `DataStructure` 객체의 `arrayOfInts` 인스턴스 변수를 직접 참조합니다.

내부 클래스를 사용하여 이 예제에 표시된 것과 같은 도우미 클래스를 구현할 수 있습니다. 사용자 인터페이스 이벤트를 처리하려면 이벤트 처리 메커니즘에서 내부 클래스를 광범위하게 사용하므로 내부 클래스를 사용하는 방법을 알아야 합니다.

## Local and Anonumous Classes

내부 클래스에는 두 가지 추가 유형이 있습니다. 메서드 본문 내에서 내부 클래스를 선언할 수 있습니다. 이러한 클래스를 로컬 클래스라고 합니다. 클래스 이름을 지정하지 않고 메서드 본문 내에서 내부 클래스를 선언할 수도 있습니다. 이러한 클래스를 익명 클래스라고 합니다.

## Modifiers

외부 클래스의 다른 멤버에 사용하는 것과 동일한 수정자를 내부 클래스에 사용할 수 있습니다. 예를 들어, 다른 클래스 멤버에 대한 액세스를 제한하는 것과 마찬가지로 액세스 지정자 `private`, `public` 및 `protected`를 사용하여 내부 클래스에 대한 액세스를 제한할 수 있습니다.

