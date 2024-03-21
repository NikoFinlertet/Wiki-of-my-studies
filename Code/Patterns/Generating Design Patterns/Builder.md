Main page: [patterns](Patterns.md)
Parent page: [Generating Design Patterns](GeneratingDesignPatterns.md)
# Description

 Позволяет создавать различные варианты объекта, избегая загрязнения конструктора. Полезно, когда может быть несколько вариантов объекта. Или когда в создании объекта задействовано много шагов.

Строитель (англ. Builder) — порождающий шаблон проектирования предоставляет способ создания составного объекта.

## Когда использовать?

Когда может быть несколько вариантов объекта и чтобы избежать телескопирования конструктора. Ключевое отличие от шаблона factory заключается в том, что шаблон factory следует использовать, когда создание является одно-этапным процессом, в то время как шаблон builder следует использовать, когда создание является многоступенчатым процессом.

# Code

Разумной альтернативой является использование шаблона builder. Прежде всего, у нас есть наш бургер, который мы хотим приготовить

```php
class Burger{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

И тогда у нас есть строитель

```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```

И затем, мы это можем использовать так:
```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```