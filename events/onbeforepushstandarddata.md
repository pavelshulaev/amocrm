# onBeforePushStandardData (с версии 2.1.11)
Используется вместо устаревшего [beforePushStandardData](./beforepushstandarddata.md).
Вызывается перед началом отправления данных, позволяет включить/отключить создание контактов, компаний, сделок и задач, а также отменить отправку.

## Параметры
0. `Rover\AmoCRM\Event` — объект события интеграции;
1. `Rover\AmoCRM\Helper\TransportProvider` — объект, содержащий все значимые объекты процесса интеграции (с версии 2.2.11).

## Обработка события
Аналогично обработке [onBeforeAmoPush](./onbeforeamopush.md).