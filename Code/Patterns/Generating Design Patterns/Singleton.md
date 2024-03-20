Main page: [patterns](Patterns.md)
Parent page: [Generating Design Patterns](GeneratingDesignPatterns)
# Description

 Гарантирует, что когда-либо будет создан только один объект определенного класса
 
Одиночка (англ. Singleton) — порождающий шаблон проектирования, гарантирующий, что в однопроцессном приложении будет единственный экземпляр некоторого класса, и предоставляющий глобальную точку доступа к этому экземпляру.

# Code

Чтобы создать синглтон, сделайте конструктор закрытым, отключите клонирование, отключите расширение и создайте статическую переменную для размещения экземпляра

```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // Hide the constructor
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // Disable cloning
    }

    private function __wakeup()
    {
        // Disable unserialize
    }
}
```

Затем для того, чтобы использовать
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```
