
# Anonymous Classes

익명 클래스를 사용하면 코드를 더욱 간결하게 만들 수 있습니다. 이를 통해 클래스를 선언하고 동시에 인스턴스화할 수 있습니다. 이름이 없다는 점을 제외하면 로컬 클래스와 같습니다. 로컬 클래스를 한 번만 사용해야 하는 경우 이를 사용하십시오.

이 섹션에서는 다음 주제를 다룹니다.

- 익명 클래스 선언
- 익명 클래스의 Syntax
- 바깥쪽 범위의 지역 변수에 액세스하고 익명 클래스의 멤버 선언 및 액세스
- 익명 클래스의 예

## Declaring Anonymous Classes

로컬 클래스는 클래스 선언인 반면 익명 클래스는 표현식입니다. 즉, 클래스를 다른 표현식으로 정의한다는 의미입니다. 다음 예제 `HelloWorldAnonymousClasses`는 지역 변수 `frenchGreeting` 및 `spanishGreeting`의 초기화 문에서 익명 클래스를 사용하지만, `englishGreeting` 변수의 초기화에는 지역 클래스를 사용합니다.

```java

public class HelloWorldAnonymousClasses {
    
    interface HelloWorld {
        public void greet();
        public void greetSomeone(String someone);
    }
    
    public void sayHello() {
        
        class EnglishGreeting implements HelloWorld {
            String name = "world";
            public void greet() {
                greetSomeone("world");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Hello " + name);
            }
        }
        
        HelloWorld englishGreeting = new EnglishGreeting();
        
        HelloWorld frenchGreeting = new HelloWorld() {
            String name = "tout le monde";
            public void greet() {
                greetSomeone("tout le monde");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Salut " + name);
            }
        };
        
        HelloWorld spanishGreeting = new HelloWorld() {
            String name = "mundo";
            public void greet() {
                greetingSomeone("mundo");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Hola, " + name);
            }
        };
        
        englishGreeting.greet();
        frenchGreeting.greetSomeone("Fred");
        spanishGreeting.greet();
    }
    
    public static void main(String... args) {
        HelloWoldAnonumousClasses myApp = new HelloWolrdAnonymousClasses();
        myApp.sayHello();
    }
}

```

## Syntax of Anonymous Classes

앞서 언급했듯이 익명 클래스는 표현식입니다. 익명 클래스 식의 구문은 코드 블록에 클래스 정의가 포함되어 있다는 점을 제외하면 생성자의 호출과 유사합니다.

FrenchGreeting 객체의 인스턴스화를 고려해보세요.
```java
HelloWorld frenchGreeting = new HelloWorld() {
    String name = "tout le monde";
    public void greet() {
        greetSomeone("tout le monde");
    }
    public void greetSomeone(String someone) {
        name = someone;
        System.out.println("Salut " + name);
    }
};

```

익명 클래스 표현식은 다음으로 구성됩니다.

- `new` 연산자

- 구현할 인터페이스 또는 확장할 클래스의 이름입니다. 이 예에서 익명 클래스는 `HelloWorld` 인터페이스를 구현합니다.

- 일반 클래스 인스턴스 생성 표현식과 마찬가지로 생성자에 대한 인수를 포함하는 괄호입니다. 참고: 인터페이스를 구현할 때는 생성자가 없으므로 이 예제와 같이 빈 괄호 쌍을 사용합니다.

- 클래스 선언 본문인 본문입니다. 보다 구체적으로 말하면 본문에서는 메서드 선언이 허용되지만 문은 허용되지 않습니다.

익명 클래스 정의는 표현식이므로 statement의 일부여야 합니다. 이 예에서 익명 클래스 표현식은 `frenchGreeting` 객체를 인스턴스화하는 문의 일부입니다. (닫는 중괄호 뒤에 세미콜론이 있는 이유가 설명됩니다.)

## Accessing Local Variables of the Enclosing Scope, and Declaring and Accessing Members of the Anonymous Class

로컬 클래스와 마찬가지로 익명 클래스도 변수를 캡처할 수 있습니다. 그들은 둘러싸는 범위의 지역 변수에 대해 동일한 액세스 권한을 갖습니다.

- 익명 클래스는 자신을 둘러싸는 클래스의 멤버에 액세스할 수 있습니다.

- 익명 클래스는 `final` 또는 `사실상 final`으로 선언되지 않은 바깥쪽 범위의 지역 변수에 액세스할 수 없습니다.

- 중첩 클래스와 마찬가지로 익명 클래스의 type 선언(예: 변수)은 바깥쪽 범위에서 동일한 이름을 가진 다른 모든 선언을 숨깁니다. 자세한 내용은 Shadowing을 참조하세요.

익명 클래스에는 멤버와 관련하여 로컬 클래스와 동일한 제한 사항이 있습니다.

- 익명 클래스에서는 정적 초기화 코드나 멤버 인터페이스를 선언할 수 없습니다.

- 익명 클래스는 상수 변수인 경우 정적 멤버를 가질 수 있습니다.

익명 클래스에서는 다음을 선언할 수 있습니다.

- 필드

- 추가 메소드(상위 유형의 메소드를 구현하지 않더라도)

- 인스턴스 초기화 코드

- 로컬 클래스 

그러나 익명 클래스에서는 생성자를 선언할 수 없습니다.

## Examples of Anonymous Classes

익명 클래스는 GUI(그래픽 사용자 인터페이스) 애플리케이션에서 자주 사용됩니다.

JavaFX 예제 `HelloWorld.java`를 고려하십시오(JavaFX 시작하기의 `Hello World`, JavaFX 스타일 섹션에서 참조). 이 샘플은 'Hello World' 버튼이 포함된 프레임을 만듭니다. 익명 클래스 표현식이 강조표시됩니다.

```java
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;
 
public class HelloWorld extends Application {
    public static void main(String[] args) {
        launch(args);
    }
    
    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Hello World!");
        Button btn = new Button();
        btn.setText("Say 'Hello World'");
        btn.setOnAction(new EventHandler<ActionEvent>() {
 
            @Override
            public void handle(ActionEvent event) {
                System.out.println("Hello World!");
            }
        });
        
        StackPane root = new StackPane();
        root.getChildren().add(btn);
        primaryStage.setScene(new Scene(root, 300, 250));
        primaryStage.show();
    }
}

```

이 예에서 메소드 호출 `btn.setOnAction`은 'Hello World' 버튼을 선택할 때 발생하는 상황을 지정합니다. 이 메서드에는 `EventHandler<ActionEvent>` 타입의 객체가 필요합니다. `EventHandler<ActionEvent>` 인터페이스에는 `handle`이라는 하나의 메서드만 포함되어 있습니다. 이 예제에서는 새 클래스로 이 메서드를 구현하는 대신 익명 클래스 식을 사용합니다. 이 표현식은 `btn.setOnAction` 메소드에 전달된 인수입니다.

`EventHandler<ActionEvent>` 인터페이스에는 메서드가 하나만 포함되어 있으므로 익명 클래스 식 대신 람다 식을 사용할 수 있습니다. 자세한 내용은 람다 표현식 섹션을 참조하세요.

익명 클래스는 두 개 이상의 메서드가 포함된 인터페이스를 구현하는 데 이상적입니다. 다음 JavaFX 예제는 UI 컨트롤 사용자 정의 섹션에서 가져온 것입니다. 강조 표시된 코드는 숫자 값만 허용하는 `TextField`를 만듭니다. `TextInputControl` 클래스에서 상속된 `replaceText` 및 `replaceSelection` 메서드를 재정의하여 익명 클래스로 `TextField` 클래스의 기본 구현을 재정의합니다.

```java
import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.geometry.Insets;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.stage.Stage;

public class CustomTextFieldSample extends Application {
    
    final static Label label = new Label();
 
    @Override
    public void start(Stage stage) {
        Group root = new Group();
        Scene scene = new Scene(root, 300, 150);
        stage.setScene(scene);
        stage.setTitle("Text Field Sample");
 
        GridPane grid = new GridPane();
        grid.setPadding(new Insets(10, 10, 10, 10));
        grid.setVgap(5);
        grid.setHgap(5);
 
        scene.setRoot(grid);
        final Label dollar = new Label("$");
        GridPane.setConstraints(dollar, 0, 0);
        grid.getChildren().add(dollar);
        
        final TextField sum = new TextField() {
            @Override
            public void replaceText(int start, int end, String text) {
                if (!text.matches("[a-z, A-Z]")) {
                    super.replaceText(start, end, text);                     
                }
                label.setText("Enter a numeric value");
            }
 
            @Override
            public void replaceSelection(String text) {
                if (!text.matches("[a-z, A-Z]")) {
                    super.replaceSelection(text);
                }
            }
        };
 
        sum.setPromptText("Enter the total");
        sum.setPrefColumnCount(10);
        GridPane.setConstraints(sum, 1, 0);
        grid.getChildren().add(sum);
        
        Button submit = new Button("Submit");
        GridPane.setConstraints(submit, 2, 0);
        grid.getChildren().add(submit);
        
        submit.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent e) {
                label.setText(null);
            }
        });
        
        GridPane.setConstraints(label, 0, 1);
        GridPane.setColumnSpan(label, 3);
        grid.getChildren().add(label);
        
        scene.setRoot(grid);
        stage.show();
    }
 
    public static void main(String[] args) {
        launch(args);
    }
}

```