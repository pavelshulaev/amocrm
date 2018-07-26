# События
Модуль поддерживает события, основанные на ядре d7 Битрикс.

* [onBeforeAmoPush](./events/onbeforeamopush.md) - срабатывает перед началом передачи данных в amoCRM
    
## `Rover\AmoCRM\Model\Rest`
* `beforeRestRequest` — перед выполнением любого rest-запроса
* `afterRestRequest` — после выполнения любого rest-запроса
* `beforeRestAuth` — перед запросом авторизации
* `afterRestAuth` — после запроса авторизации

## Контакт/Комания `Rover\AmoCRM\Model\Rest\Contact`
* `beforeContactAdd` — перед запросом добавления контакта/компании
* `afterContactAdd` — после запроса добавления контакта/компании
* `beforeContactUpdate` — перед запросом обновления контакта/компании
* `afterContactUpdate` — после запроса обновления контакта/компании
* `beforeContactGetList` — перед запросом получения списка контактов/компаний
* `afterContactGetList` — после запроса получения списка контактов/компаний
    
## Сделка `Rover\AmoCRM\Model\Rest\Lead`
* `beforeLeadAdd` — перед запросом добавления сделки
* `afterLeadAdd` — после запроса добавления сделки
* `beforeLeadUpdate` — перед запросом обновления сделки
* `afterLeadUpdate` — после запроса обновления сделки
* `beforeLeadGetList` — перед запросом получения списка сделок
* `afterLeadGetList` — после запроса получения списка сделок

## Задача `Rover\AmoCRM\Model\Rest\Task`
* `beforeTaskAdd` — перед запросом добавления задачи
* `afterTaskAdd` — после запроса добавления задачи

## Задача `Rover\AmoCRM\Model\Rest\Unsorted`
* `beforeUnsortedAdd` — перед запросом добавления «неразобранного»
* `afterUnsortedAdd` — после запроса добавления «неразобранного»
* `beforeUnsortedGetList` — перед запросом получения списка «неразобранного»
* `afterUnsortedGetList` — после запроса получения списка «неразобранного»
    
## Примечание `Rover\AmoCRM\Model\Rest\Note`
* `beforeNoteAdd` — перед запросом добавления примечания
* `afterNoteAdd` — после запроса добавления примечания
* `beforeNoteGetList` — перед запросом получения списка примечаний
* `afterNoteGetList` — после запроса получения списка примечаний
    
Все события поддерживают одинаковую стуктуру параметров.
Для события, начинающегося с `before` — это:
* url запроса
* тело запроса (массив)

С `after`:
* url запроса
* тело запроса (массив)
* тело ответа (массив)
* код ответа
* сообщение об ошибке (если есть)

Если в обработчике события, начинающегося на `before` (кроме `beforeRestRequest`) вернуть результат с типом `\Bitrix\Main\EventResult::ERROR`, то соотвествующий запрос выполнен не будет.

См. пример [onBeforeAmoPush](./events/onbeforeamopush.md).

---
[на главную](./README.MD)    