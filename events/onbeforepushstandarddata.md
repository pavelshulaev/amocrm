# onBeforePushStandardData (с версии 2.1.11)
Используется вместо устаревшего [beforePushStandardData](./beforepushstandarddata.md).
Вызывается перед началом отправления данных, позволяет включить/отключить создание контактов, компаний, сделок и задач, а также отменить отправку.

## Параметры
0. `Rover\AmoCRM\Event` — объект события интеграции;

## Обработка события
Аналогично обработке [onBeforeAmoPush](./onbeforeamopush.md).