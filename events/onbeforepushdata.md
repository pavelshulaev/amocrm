# onBeforePushData (с версии 2.1.11)
Используется вместо устаревших [onBeforeAmoPush](./onbeforeamopush.md) и [pushData](./pushdata.md).
Вызывается непосредственно перед началом передачи информации в amoCrm, можно изменить параметры создаваемых сущностей в классе `Rover\AmoCRM\Event` (поля, тип («неразобранное» или нет), а также включить/отключить создание контактов, компаний, сделок и задач).

## Параметры
0. `Rover\AmoCRM\Event` — объект события интеграции;
1. `Rover\AmoCRM\Helper\TransportProvider` — объект, содержащий все значимые объекты процесса интеграции (с версии 2.2.11).

## Обработка события

### 1) Замена имени файла на путь к нему, при интеграции с почтовым событием АСПРО
При загрузке файла в решениях от АСПРО, в почтовое событие передается только его имя и стандартными средствами получить путь до него невозможно. Чтобы обойти это ограничение, в файле `/bitrix/php_interface/init.php` можно разместить подобный обработчик:

```php
use Bitrix\Main\Application;
use Bitrix\Main\EventManager;
use Bitrix\Main\Loader;
use Rover\AmoCRM\Controller\Transport;
use Rover\AmoCRM\Model\Source\PostEvent;
use Rover\AmoCRM\Options;
use Bitrix\Main\Event;
use Bitrix\Main\EventResult;
use \Bitrix\Main\ArgumentOutOfRangeException;

if (Loader::includeModule('rover.amocrm')){
    EventManager::getInstance()->addEventHandler(
        Options::MODULE_ID,
        'onBeforePushData',
        ["FixAspro", "onBeforePushDataHandler"]
    );

    /**
     * Class FixAspro
     *
     * @author Pavel Shulaev (https://rover-it.me)
     */
    class FixAspro
    {
        /**
         * @param Event $event
         * @return EventResult
         * @throws ArgumentOutOfRangeException
         * @author Pavel Shulaev (https://rover-it.me)
         */
        public static function onBeforePushDataHandler(Event $event)
        {
            /** @var Transport $transport */
            $transport = $event->getParameter(0);
            try{
                $source = $transport->getEvent()->getSource();
            } catch (\Exception $e) {
                return new EventResult(EventResult::ERROR, $event->getParameters());
            }

            if (($source->getType() == PostEvent::getType())
                && ($source->getName() == 'EXAMPLE_EVENT_NAME')) // EXAMPLE_EVENT_NAME = aspro event name with file
            {
                $params = $transport->getEvent()->getEventParams();

                $fileName = trim($params['FILE_NAME']); // FILE_NAME = example field with file name
                if (strlen($fileName))
                {
                    $file = \CFile::getList(['ID' => 'DESC'], ['ORIGINAL_NAME' => $fileName])->Fetch();
                    if ($file)
                    {
                        $src = \CFile::GetFileSRC($file);
                        // if file not on external storage
                        if (strpos($src, 'http') === false)
                        {
                            $protocol = Application::getInstance()->getContext()->getRequest()->isHttps()
                                ? 'https://'
                                : 'http://';

                            $src = $protocol . str_replace(DIRECTORY_SEPARATOR . DIRECTORY_SEPARATOR,
                                    DIRECTORY_SEPARATOR, $_SERVER['HTTP_HOST'] . $src);
                        }

                        $params['FILE_NAME'] .= ' (' . $src . ')';
                        $transport->getEvent()->setEventParams($params);
                    }
                }
            }

            return new EventResult(EventResult::SUCCESS, $event->getParameters());
        }
    }
}
```
* `EXAMPLE_EVENT_NAME` - замените на имя почтового события, в котором надо передать файл
* `FILE_NAME` - замените на поле, в котором имя файла передаётся в событие.
