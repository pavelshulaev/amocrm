# События
Модуль поддерживает события, основанные на ядре d7 Битрикс.
## onBeforeAmoPush
Вызывается перед началом интеграции с amoCrm. Срабатывает только в том случае, если для веб-формы или почтового события настроено соотвествующее правило интеграции.

### Примеры
#### 1) Замена имени файла на путь к нему, при интеграции с почтовым событием АСПРО
При загрузке файла в решениях от АСПРО, в почтовое событие передается только его имя и стандартными средствами получить путь до него невозможно. Чтобы обойти это ограничение, в файле `/bitrix/php_interface/init.php` можно разместить подобный обработчик:
    
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
    
                if (($source->getType() == Source::SOURCE__EVENT)
                    && ($source->getName() == 'EXAMPLE_EVENT_NAME')) // EXAMPLE_EVENT_NAME = aspro event name with file
                {
                    $params = $event->getParameter('params');
    
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
                            $event->setParameter('params', $params);
                        }
                    }
                }
    
                return new EventResult(EventResult::SUCCESS, $event->getParameters());
            }
        }
    }
    
[на главную](./README.MD)    