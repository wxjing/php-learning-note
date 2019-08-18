# 利用ob系列的函数
ob_start() 打开输出控制缓冲
ob_get_contents() 返回输出缓冲区内容
ob_clean 清空输出缓冲区
ob_end_flush() 冲刷出输出缓冲区内容并关闭缓冲

```php
<?php
date_default_timezone_set('Asia/Shanghai');

$id = !empty($_GET['id']) ? $_GET['id'] : '';

$cache_name = md5(__FILE__).'-'.$id. '.html';
$cache_lifetime = 10;
if( filectime(__FILE__) <= filectime($cache_name) &&
    file_exists($cache_name) &&
    filectime($cache_name) + $cache_lifetime > time())
{
    include $cache_name;
    exit();
}
ob_start();
?>
<b>this is my Script id = <?php echo time(); ?></b>
<?php
$content = ob_get_contents();
ob_end_flush();
$handle = fopen($cache_name,'w');
fwrite($handle,$content);
fclose($handle);
?>
```
