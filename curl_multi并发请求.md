[curl_multi并发请求](https://www.php.cn/php-weizijiaocheng-409819.html)

```php
// 简单demo，默认支持为GET请求
function multiRequest($urls)
{
    $mh = curl_multi_init();
    $urlHandlers = [];
    $urlData = [];
    // 初始化多个请求句柄为一个
    foreach ($urls as $value) {
        $ch = curl_init();
        $url = $value['url'];
        $url .= strpos($url, '?') ? '&' : '?';
        $params = $value['params'];
        $url .= is_array($params) ? http_build_query($params) : $params;
        curl_setopt($ch, CURLOPT_URL, $url);
        // 设置数据通过字符串返回，而不是直接输出
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        $urlHandlers[] = $ch;
        curl_multi_add_handle($mh, $ch);
    }
    $active = null;
    // 检测操作的初始状态是否OK，CURLM_CALL_MULTI_PERFORM为常量值-1
    do {
        // 返回的$active是活跃连接的数量，$mrc是返回值，正常为0，异常为-1
        $mrc = curl_multi_exec($mh, $active);
    } while ($mrc == CURLM_CALL_MULTI_PERFORM);
    // 如果还有活动的请求，同时操作状态OK，CURLM_OK为常量值0
    while ($active && $mrc == CURLM_OK) {
        // 持续查询状态并不利于处理任务，每50ms检查一次，此时释放CPU，降低机器负载
        usleep(50000);
        // 如果批处理句柄OK，重复检查操作状态直至OK。select返回值异常时为-1，正常为1（因为只有1个批处理句柄）
        if (curl_multi_select($mh) != -1) {
            do {
                $mrc = curl_multi_exec($mh, $active);
            } while ($mrc == CURLM_CALL_MULTI_PERFORM);
        }
    }
    // 获取返回结果
    foreach ($urlHandlers as $index => $ch) {
        $urlData[$index] = curl_multi_getcontent($ch);
        // 移除单个curl句柄
        curl_multi_remove_handle($mh, $ch);
    }
    curl_multi_close($mh);
    return $urlData;
}

```
