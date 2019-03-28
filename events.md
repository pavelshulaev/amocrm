# События [с версии 2.0]
Модуль поддерживает события, основанные на ядре d7 Битрикс.

#### Общие
* [onBeforeAmoPush](./events/onbeforeamopush.md) — срабатывает перед началом передачи данных в amoCRM
* [pushData](./events/pushdata.md) — непосредственно перед началом передачи информации в amoCrm, можно изменить входные параметры, тип («неразобранное» или нет), а также включить/отключить создание контактов, компаний, сделок и задач;
* [afterPushData](./events/afterpushdata.md) — после передачи информации в amoCrm, в событие передаются входные параметры, тип («неразобранное» или нет), а также флаги создания контактов, компаний, сделок и задач.

#### Обычное добавление контактов, компаний, сделок и задач
* [beforePushStandardData](./events/beforepushstandarddata.md) — перед началом отправления данных, позволяет включить/отключить создание контактов, компаний, сделок и задач, а также отменить отправку;
* [afterPushStandardData](./events/afterpushstandarddata.md) — после отправления данных, в событие передаются созданные объекты сущностей.

#### Неразобранное
* [beforePushUnsortedData](./events/beforepushunsorteddata.md) — перед началом отправления «неразобранного», позволяет включить/отключить создание сделок и задач, а также отменить отправку;
* [pushUnsortedData](./events/pushunsorteddata.md) — перед непосредственным отправлением «неразобранного», позволяет внести изменения в объект «неразобранного» (`Rover\AmoCRM\Entity\Handler\Unsorted`), а также отменить отправку;
* [afterPushUnsortedData](./events/afterpushunsorteddata.md) — после отправления «неразобранного»;
    
## События rest-запросов
#### Ядро `Rover\AmoCRM\Model\Rest`
* `beforeRestRequest` — перед выполнением любого rest-запроса
* `afterRestRequest` — после выполнения любого rest-запроса
* `beforeRestAuth` — перед запросом авторизации
* `afterRestAuth` — после запроса авторизации

#### Контакт/Компания `Rover\AmoCRM\Model\Rest\Contact`
* `beforeContactAdd` — перед запросом добавления контакта/компании
* `afterContactAdd` — после запроса добавления контакта/компании
* `beforeContactUpdate` — перед запросом обновления контакта/компании
* `afterContactUpdate` — после запроса обновления контакта/компании
* `beforeContactGetList` — перед запросом получения списка контактов/компаний
* `afterContactGetList` — после запроса получения списка контактов/компаний
    
#### Сделка `Rover\AmoCRM\Model\Rest\Lead`
* `beforeLeadAdd` — перед запросом добавления сделки
* `afterLeadAdd` — после запроса добавления сделки
* `beforeLeadUpdate` — перед запросом обновления сделки
* `afterLeadUpdate` — после запроса обновления сделки
* `beforeLeadGetList` — перед запросом получения списка сделок
* `afterLeadGetList` — после запроса получения списка сделок

#### Задача `Rover\AmoCRM\Model\Rest\Task`
* `beforeTaskAdd` — перед запросом добавления задачи
* `afterTaskAdd` — после запроса добавления задачи

#### «Неразобранное» `Rover\AmoCRM\Model\Rest\Unsorted`
* `beforeUnsortedAdd` — перед запросом добавления «неразобранного»
* `afterUnsortedAdd` — после запроса добавления «неразобранного»
* `beforeUnsortedGetList` — перед запросом получения списка «неразобранного»
* `afterUnsortedGetList` — после запроса получения списка «неразобранного»
    
#### Примечание `Rover\AmoCRM\Model\Rest\Note`
* `beforeNoteAdd` — перед запросом добавления примечания
* `afterNoteAdd` — после запроса добавления примечания
* `beforeNoteGetList` — перед запросом получения списка примечаний
* `afterNoteGetList` — после запроса получения списка примечаний

### Порядок следования параметров    
Для событий, начинающегося с `before`:
* url запроса
* тело запроса (массив)
* запрашиваемые поля (массив, только для событий выборки `...GetList`)

С `after`:
* url запроса
* тело запроса (массив)
* запрашиваемые поля (массив, только для событий выборки `...GetList`)
* тело ответа (массив)
* код ответа
* сообщение об ошибке (если есть)

> Если в обработчике события, начинающегося на `before` (кроме `beforeRestRequest`) вернуть результат с типом `\Bitrix\Main\EventResult::ERROR`, то соотвествующий запрос выполнен не будет.

### Пример
Назначение обработчика на событие:
```php
$eventManager = \Bitrix\Main\EventManager::getInstance();
$eventManager->addEventHandler('rover.amocrm', 'afterContactGetList', array('\Rover\AmoCRM\Event', 'onAfterContactGetList'));
```
Сам обработчик:
```php 
namespace Rover\AmoCRM;

use Rover\AmoCRM\Helper\Log;
use \Bitrix\Main;

class Event
{
    public static function onAfterContactGetList(Main\Event $event)
    {
        $result = $event->getParameter(3);

        Log::addNote('Contact::getList results count:',  isset($result['_embedded']['items']) ? count($result['_embedded']['items']) : 0);
    }
}
```

---
[на главную](./README.MD)    