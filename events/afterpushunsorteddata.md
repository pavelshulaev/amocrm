# afterPushUnsortedData (с версии 2.0.0, устарело с версии 2.1.11)
Вместо него используется [onAfterPushUnsortedData](./onafterpushunsorteddata.md).
Вызывается после отправления «неразобранного».

## Параметры
0. `Rover\AmoCRM\Entity\Source` — источник (почтовое событие/веб-форма);
1. `Rover\AmoCRM\Entity\Result` — результат (почтового события/веб-формы);
2. `Rover\AmoCRM\Entity\Handler\Unsorted` — объект, содержащий все параметры создаваемого «неразобранного».

## Обработка события
Аналогично обработке [onBeforeAmoPush](./onbeforeamopush.md), за исключением того, что изменения параметров уже ни на что не влияют.