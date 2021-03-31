# События
Модуль поддерживает события, основанные на ядре d7 Битрикс.

## Общие
* [onBeforeWebhook](./events/onbeforewebhook.md) — перед срабатыванием вебхука. Позволяет добавить свою логику на изменения со стороны амо, а также отменить синхронизацию статусов и флагов амо -> магазин;
* [onBeforePushData](./events/onbeforepushdata.md) — непосредственно перед началом передачи информации в amoCrm, позволяет модифицировать поля создаваемых сущностей, а также оменить или, наоборот, включить их отправку;
* [onAfterPushData](./events/onafterpushdata.md) — после завершения передачи информации в amoCrm.

#### Обычное добавление контактов, компаний, сделок и задач
* [onBeforePushStandardData](./events/onbeforepushstandarddata.md) — перед началом отправления данных, позволяет включить/отключить создание контактов, компаний, сделок и задач, а также отменить отправку;
* [onAfterPushStandardData](./events/onafterpushstandarddata.md) — после отправления данных.

#### Неразобранное
* [onBeforePushUnsortedData](./events/onbeforepushunsorteddata.md) — перед началом отправления «неразобранного», позволяет включить/отключить создание контакта и компании, а также отменить отправку;
* [onPushUnsortedData](./events/onpushunsorteddata.md) — перед непосредственным отправлением «неразобранного», позволяет внести изменения в создаваемые сущности («неразобранное», контакт, сделка, компания), а также отменить отправку;
* [onAfterPushUnsortedData](./events/onafterpushunsorteddata.md) — после отправления «неразобранного»;
  
#### Товары
* [beforeSearchOneByBasketItem](./events/beforesearchonebybasketitem.md) — перед поиском товара в amoCRM на основе товара в корзине;
* [afterSearchOneByBasketItem](./events/aftersearchonebybasketitem.md) — после поиска товара в amoCRM на основе товара в корзине;
* [beforeMakeCatalogElementByBasketItem](./events/beforemakecatalogelementbybasketitem.md) — перед созданием товара для amoCRM на основе товара в корзине;
* [afterMakeCatalogElementByBasketItem](./events/aftermakecatalogelementbybasketitem.md) — после создания товара для amoCRM на основе товара в корзине.
  
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

### Порядок следования параметров в событиях rest-запросов  
Для событий, начинающегося с `before[тип сущности]GetList` (например `beforeNoteGetList`):

0. url запроса
1. тело запроса (массив)
2. запрашиваемые поля (массив)

Для остальных, начинающихся с `before`

0. url запроса
1. тело запроса (массив)

С `after[тип сущности]GetList` (например `afterNoteGetList`)::

0. url запроса
1. тело запроса (массив)
2. запрашиваемые поля (массив)
3. тело ответа (массив)
4. код ответа
5. сообщение об ошибке (если есть)

Для остальных, начинающихся с `after`

0. url запроса
1. тело запроса (массив)
2. тело ответа (массив)
3. код ответа
4. сообщение об ошибке (если есть)

> Если в обработчике события, начинающегося на `before` (кроме `beforeRestRequest`) вернуть результат с типом `\Bitrix\Main\EventResult::ERROR`, то соотвествующий запрос выполнен не будет.

### Пример
Назначение обработчика на событие:
```php
$eventManager = \Bitrix\Main\EventManager::getInstance();
$eventManager->addEventHandler('rover.amocrm', 'afterContactGetList', ['\Rover\AmoCRM\Event', 'onAfterContactGetList']);
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