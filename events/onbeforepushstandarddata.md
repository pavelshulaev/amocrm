# onBeforePushStandardData
Используется вместо устаревшего [beforePushStandardData](./beforepushstandarddata.md).
Вызывается перед началом отправления данных, позволяет включить/отключить создание контактов, компаний, сделок и задач, а также отменить отправку.

## Параметры
0. `Rover\AmoCRM\Controller\Transport` — объект содержащий все значимые объекты процесса интеграции, в т.ч. `Rover\AmoCRM\Event` и `Rover\AmoCRM\Profile`, а также контроллеры создаваемых объектов.

## Обработка события
Аналогично обработке [onBeforePushData](./onbeforepushdata.md).