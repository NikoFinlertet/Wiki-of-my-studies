/[patterns](Patterns.md)
# Description

 Фабрика, которая группирует индивидуальные, но связанные или зависимые фабрики вместе без указания их конкретных классов.

 Абстрактная фабрика (англ. Abstract factory) — порождающий шаблон проектирования, предоставляет интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов, не специфицируя их конкретных классов. Шаблон реализуется созданием абстрактного класса Factory, который представляет собой интерфейс для создания компонентов системы (например, для оконного интерфейса он может создавать окна и кнопки). Затем пишутся классы, реализующие этот интерфейс.

## Когда использовать?

Когда существуют взаимодействующие зависимости с не-совсем-простой логикой создания.


# Code
Реализуем описанный пример с дверьми. Прежде всего у нас будет интерфейс `Door` и несколько его имплементаций.

```html
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo 'I am a wooden door';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo 'I am an iron door';
    }
}
```

Затем у нас будут несколько соответствующих экспертов для каждого вида двери

```html
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit iron doors';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit wooden doors';
    }
}
```

Теперь создадим абстрактную фабрику, которая бы позволяла создавать семейство взаимосвязанных объектов, например, фабрика деревянных дверей создает деревянную двурь и эксперта по деревянным дверям, а фабрика железных дверей должна создавать железные двери и экспертов по ним.

```html
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// Wooden factory to return carpenter and wooden door
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// Iron door factory to get iron door and the relevant fitting expert
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```

затем это может быть использованно следующим образом

```html
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // Output: I am a wooden door
$expert->getDescription(); // Output: I can only fit wooden doors

// Same for Iron Factory
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // Output: I am an iron door
$expert->getDescription(); // Output: I can only fit iron doors
```

Как вы можете видеть, фабрика деревянных дверей инкапсулирует `carpenter` и `wooden door`, в то время как фабрика железных дверей инкапсулирует `iron door` и `welder`. И таким образом, это дает нам уверенность, что для каждой созданной двери мы не перепутаем соответствующего эксперта.