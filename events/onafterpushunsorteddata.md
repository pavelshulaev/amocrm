# onAfterPushUnsortedData
Вызывается после отправления «неразобранного».

## Параметры
0. `Rover\AmoCRM\Controller\Transport` — объект содержащий все значимые объекты процесса интеграции, в т.ч. `Rover\AmoCRM\Event` и `Rover\AmoCRM\Profile`, а также контроллеры создаваемых объектов.

## Обработка события
Аналогично обработке [onBeforeAmoPush](./onbeforeamopush.md), за исключением того, что изменения параметров уже ни на что не влияют.