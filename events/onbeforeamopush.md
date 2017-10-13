# onBeforeAmoPush (актуально с версии 1.11.3)
Вызывается перед началом интеграции с amoCrm. Срабатывает только в том случае, если для веб-формы или почтового события настроено соотвествующее правило интеграции. 

Событие может быть использовано как для модификации значений передаваемых полей, так и для отмены интеграции, если его обработчик вернёт результат, отличный от `new EventResult(EventResult::SUCCESS, ...)`.

## Примеры
### 1) Замена имени файла на путь к нему, при интеграции с почтовым событием АСПРО
При загрузке файла в решениях от АСПРО, в почтовое событие передается только его имя и стандартными средствами получить путь до него невозможно. Чтобы обойти это ограничение, в файле `/bitrix/php_interface/init.php` можно разместить подобный обработчик:

```php
use Bitrix\Main\EventManager;
use Bitrix\Main\Loader;
use Rover\AmoCRM\Config\Options;
use Bitrix\Main\Event;
use Bitrix\Main\EventResult;
use \Rover\AmoCRM\Model\Source;
use \Rover\AmoCRM\Helper\Event as EventHelper;
use \Bitrix\Main\ArgumentOutOfRangeException;

if (Loader::includeModule('rover.amocrm')){
    EventManager::getInstance()->addEventHandler(
        Options::MODULE_ID,
        EventHelper::ON_BEFORE_AMO_PUSH,
        ["FixAspro", "onBeforeAmoPush"]
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
        public static function onBeforeAmoPush(Event $event)
        {
            $source = $event->getParameter('source');
            if (!$source instanceof Source)
                throw new ArgumentOutOfRangeException('source');

            if (($source->getType() == Source::TYPE__EVENT)
                && ($source->getName() == 'EXAMPLE_EVENT_NAME')) // EXAMPLE_EVENT_NAME = aspro event name with file
            {
                /**
                 * @var Rover\AmoCRM\Entity\Result $result
                 */
                $result = $event->getParameter('result');
                $params = $result->getEventParams();

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
                            $protocol = \CMain::IsHTTPS()
                                ? 'https://'
                                : 'http://';

                            $src = $protocol . str_replace(DIRECTORY_SEPARATOR . DIRECTORY_SEPARATOR,
                                    DIRECTORY_SEPARATOR, $_SERVER['HTTP_HOST'] . $src);
                        }

                        $params['FILE_NAME'] .= ' (' . $src . ')';
                        $result->setEventParams($params);
                        $event->setParameter('result', $result);
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