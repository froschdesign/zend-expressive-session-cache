# Manual usage

The following details the constructor of the `Zend\Expressive\Session\Cache\CacheSessionPersistence` class:

```php
/**
 * Prepare session cache and default HTTP caching headers.
 *
 * The cache limiter setting is used to determine how to send HTTP
 * client-side caching headers. Those headers will be added
 * programmatically to the response along with the session set-cookie
 * header when the session data is persisted.
 *
 * @param int $cacheExpire Number of seconds until the session cookie
 *     should expire; defaults to 180 minutes (180m * 60s/m = 10800s),
 *     which is the default of the PHP session.cache_expire setting. This
 *     is also used to set the TTL for session data.
 * @param null|int $lastModified Timestamp when the application was last
 *     modified. If not provided, this will look for each of
 *     public/index.php, index.php, and finally the current working
 *     directory, using the filemtime() of the first found.
 */
public function __construct(
    \Psr\Cache\CacheItemPoolInterface $cache,
    string $cookieName,
    string $cookiePath = '/',
    string $cacheLimiter = 'nocache',
    int $cacheExpire = 10800,
    ?int $lastModified = null
) {
```

Pass all required values and any optional values when creating an instance:

```php
use Cache\Adapter\Predis\PredisCachePool;
use Zend\Expressive\Session\Cache\CacheSessionPersistence;
use Zend\Expressive\Session\SessionMiddleware;

$cachePool = new PredisCachePool('tcp://localhost:6379');
$persistence = new CacheSessionPersistence(
    $cachePool,
    'MYSITE',
    '/',
    'public',
    60 * 60 * 24 * 30 // 30 days
);
$middleware = new SessionMiddleware($persistence);
```
