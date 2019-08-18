# 查看swoole信息
`php --ri swoole`

# 框架
1. swoft
2. easyswoole
3. fastd

```sh
ps aux | grep simple_route_master | awk '{print $2}' | xargs kill -USR1
```

```php
spl_autoload_register( function($class){
  $baseClassPath = \str_replace('\\',DIRECTORY_SEPARATOR,$class) . '.php';
  $classPath = __DIR__ . '/' . $baseClassPath;
  if(is_file($classPath)){
    require "$classPath";
    return;
  }
})
```

# mac用pecl安装swoole可能出现的报错及解决办法
https://www.bbsmax.com/A/ke5jRRbg5r/
