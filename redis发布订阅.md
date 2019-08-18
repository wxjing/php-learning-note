# 发布端
```PHP
$redis = new \Redis();
$redis->connect('127.0.0.1', 6379, 3600);
$redis->auth('123456'); //设置密码
$message = '测试一下';
$ret = $redis->publish('test', $message);
print_r($ret);
echo PHP_EOL;

```
# 订阅端
```PHP
ini_set('default_socket_timeout', -1);  //不超时
$redis = new Redis();
$redis->connect('127.0.01', 6379, 3600);
$redis->auth('123456'); //设置密码
$redis->subscribe(['test'], 'callback');

function callback($instance, $channelName, $message)
{
    echo '----------回调开始--------------' . PHP_EOL;
    print_r($message);//消息
    echo PHP_EOL;
    print_r($instance);//redis实例
    echo PHP_EOL;
    print_r($channelName);//管道名称
    echo PHP_EOL;
    echo '----------回调结束--------------' . PHP_EOL;
}

```
