# ”старевшие событи€

## ќбщие
* [onBeforeAmoPush](./events/onbeforeamopush.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onBeforePushData](./onbeforepushdata.md)) Ч срабатывает перед началом передачи данных в amoCRM
* [pushData](./events/pushdata.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onBeforePushData](./onbeforepushdata.md)) Ч непосредственно перед началом передачи информации в amoCrm, можно изменить входные параметры, тип (Ђнеразобранноеї или нет), а также включить/отключить создание контактов, компаний, сделок и задач;
* [afterPushData](./events/afterpushdata.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onAfterPushData](./onafterpushdata.md)) Ч после передачи информации в amoCrm, в событие передаютс€ входные параметры, тип (Ђнеразобранноеї или нет), а также флаги создани€ контактов, компаний, сделок и задач.

## ќбычное добавление контактов, компаний, сделок и задач
* [beforePushStandardData](./events/beforepushstandarddata.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onBeforePushStandardData](./onbeforepushstandarddata.md)) Ч перед началом отправлени€ данных, позвол€ет включить/отключить создание контактов, компаний, сделок и задач, а также отменить отправку;
* [afterPushStandardData](./events/afterpushstandarddata.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onBeforePushStandardData](./onafterpushstandarddata.md)) Ч после отправлени€ данных, в событие передаютс€ созданные объекты сущностей.

## Ќеразобранное
* [beforePushUnsortedData](./events/beforepushunsorteddata.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onBeforePushUnsortedData](./onbeforepushunsorteddata.md)) Ч перед началом отправлени€ Ђнеразобранногої, позвол€ет включить/отключить создание сделок и задач, а также отменить отправку;
* [pushUnsortedData](./events/pushunsorteddata.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onPushUnsortedData](./onpushunsorteddata.md)) Ч перед непосредственным отправлением Ђнеразобранногої, позвол€ет внести изменени€ в объект Ђнеразобранногої (`Rover\AmoCRM\Entity\Handler\Unsorted`), а также отменить отправку;
* [afterPushUnsortedData](./events/afterpushunsorteddata.md) (устарело с версии 2.1.11, удалено с 2.4.0. »спользуйте [onAfterPushUnsortedData](./onafterpushunsorteddata.md)) Ч после отправлени€ Ђнеразобранногої;
  
---
[—обыти€](../events.md)
[на главную](../README.MD)    