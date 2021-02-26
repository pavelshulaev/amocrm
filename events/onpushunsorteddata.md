# onPushUnsortedData
Вызывается перед непосредственным отправлением «неразобранного», позволяет внести изменения в объект «неразобранного» (`Rover\AmoCRM\Entity\Handler\Unsorted`), а также отменить отправку.

## Параметры
0. `Rover\AmoCRM\Controller\Transport` — объект содержащий все значимые объекты процесса интеграции, в т.ч. `Rover\AmoCRM\Event` и `Rover\AmoCRM\Profile`, а также контроллеры создаваемых объектов.

## Обработка события
Аналогично обработке [onBeforePushData](./onbeforepushdata.md).