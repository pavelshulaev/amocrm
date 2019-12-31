# pushUnsortedData (с версии 2.0.0, устарело с версии 2.1.11)
Вместо него используется [onPushUnsortedData](./onpushunsorteddata.md).
Вызывается перед непосредственным отправлением «неразобранного», позволяет внести изменения в объект «неразобранного» (`Rover\AmoCRM\Entity\Handler\Unsorted`), а также отменить отправку.

## Параметры
0. `Rover\AmoCRM\Entity\Source` — источник (почтовое событие/веб-форма);
1. `Rover\AmoCRM\Entity\Result` — результат (почтового события/веб-формы);
2. `Rover\AmoCRM\Entity\Handler\Unsorted` — объект, содержащий все параметры создаваемого «неразобранного».

## Обработка события
Аналогично обработке [onBeforeAmoPush](./onbeforeamopush.md).