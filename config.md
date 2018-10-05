## config/logging.php

```php
<?php
    // [...]

    'channels' => [
        'stack' => [
            'driver' => 'stack',
            'channels' => ['mysql'],
        ],

        // [...]

        'mysql' => [
            'driver' => 'custom',
            'via' => App\Logging\CreateMySQLLogger::class,
        ],
    ],
```
## app/Logging/CreateMySQLLogger.php
```php
<?php
namespace App\Logging;

use Exception;
use Monolog\Logger;
use Logger\Monolog\Handler\MysqlHandler;

class CreateMySQLLogger
{
    /**
     * Create a custom Monolog instance.
     *
     * @param  array $config
     * @return Logger
     * @throws Exception
     */
    public function __invoke(array $config)
    {
        $channel = $config['name'] ?? env('APP_ENV');
        $monolog = new Logger($channel);
        $monolog->pushHandler(new MysqlHandler());
        return $monolog;
    }
}
```
