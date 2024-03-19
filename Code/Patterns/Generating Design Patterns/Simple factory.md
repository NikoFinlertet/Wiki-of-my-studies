/[patterns](Patterns.md)
# Description

Простая фабрика просто создает экземпляр для клиента не предоставляя клиенту какой либо логики создания.

## Когда использовать?

_Если создание объекта не ограничивается парой присвоений и требует вовлечения определенной логики, имеет смысл вынести ее в отдельную фабрику вместо постоянного повторения одного и того же кода._


# Code
```Java
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```

Затем фабрика DoorFactory создает двери и возвращает их.
```
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```
И затем, мы это можем использовать так
```
$door = DoorFactory::makeDoor(100, 200);
echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();
```


