# afterPushStandardData (с версии 2.0.0, устарело с версии 2.1.11)
Вместо него используется [onAfterPushStandardData](./onafterpushstandarddata.md).
Вызывается после отправления данных, в событие передаются созданные объекты сущностей.

## Параметры
0. `Rover\AmoCRM\Entity\Source` — источник (почтовое событие/веб-форма);
1. `Rover\AmoCRM\Entity\Result` — результат (почтового события/веб-формы);
2. `Rover\AmoCRM\Entity\Handler\Contact|null` — контакт (`null` - если не создан);
3. `Rover\AmoCRM\Entity\Handler\Company|null` — компания (`null` - если не создана);
3. `Rover\AmoCRM\Entity\Handler\Lead|null` — сделка (`null` - если не создана);
3. `Rover\AmoCRM\Entity\Handler\Task|null` — задача (`null` - если не создана).

## Обработка события
Аналогично обработке [onBeforeAmoPush](./onbeforeamopush.md), за исключением того, что изменения параметров уже ни на что не влияют.