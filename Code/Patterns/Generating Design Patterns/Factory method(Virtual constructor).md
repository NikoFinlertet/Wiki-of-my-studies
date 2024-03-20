/[patterns](Patterns.md)
# Description

 Обеспечивает возможность передачи логики создания в дочерние классы.
 
 Порождающий шаблон проектирования, предоставляющий подклассам интерфейс для создания экземпляров некоторого класса. В момент создания наследники могут определить, какой класс создавать. Иными словами, Фабрика делегирует создание объектов наследникам родительского класса. Это позволяет использовать в коде программы не специфические классы, а манипулировать абстрактными объектами на более высоком уровне.

## Когда использовать?

Полезно когда в классе присутствуют несколько общих процессов, но требуемый подкласс определяется динамично во время исполнения. Или другими словами, когда клиент не знает какой конкретно подкласс может потребоваться.


# Code
Возьмем пример нашего менеджера по персоналу. Прежде всего создадим интерфейс Interviewer и несколько его реализаций.

```Java
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about design patterns!';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about community building';
    }
}
```

Теперь давайте создадим `HiringManager`

```Java
abstract class HiringManager
{

    // Factory method
    abstract public function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}
```

Теперь любой потомок может наследовать его и предоставить требуемый Interviewer

```java
class DevelopmentManager extends HiringManager
{
    public function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    public function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```

затем это может быть использованно следующим образом

```java
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Output: Asking about design patterns

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Output: Asking about community building.
```

