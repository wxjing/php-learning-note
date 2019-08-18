# [aes加解密](./aes加解密.php)
# [composer命令](./composer.md)
# [Modern PHP笔记](./ModernPHP.md)
# 跨域
```php
/**
 * 跨域请求
 * 设置哪些域名可以访问
 */
$origin = isset($_SERVER['HTTP_ORIGIN'])? $_SERVER['HTTP_ORIGIN'] : '';

$allow_origin = array(
    'http://192.168.1.132',
);
if(in_array($origin, $allow_origin)){
    header('Access-Control-Allow-Origin:'.$origin);
    header('Access-Control-Allow-Methods:GET, PUT, POST');
    header('Access-Control-Allow-Headers:Origin, No-Cache, X-Requested-With, If-Modified-Since, Pragma, Last-Modified, Cache-Control, Expires, Content-Type, X-E4M-With');
}
```
