# 查看sql耗时
#### php
```php
//显示目录
public function index(){
    $model_dir = dirname(dirname(dirname(__FILE__))).'/Runtime/Logs/';
    $model_list = scandir($model_dir);
    if($model_list){
        foreach ($model_list as $k=>$v){
            if($v == '.' || $v == '..' || strpos($v,'.') === 0){
                unset($model_list[$k]);
            }
        }
    }
    $this->assign('dir_list',$model_list);
    $this->display('Test/index');
}
//显示文件
public function getFilesList(){
    $files_name = I('files_name');
    $model_dir = dirname(dirname(dirname(__FILE__))).'/Runtime/Logs/'.$files_name;
    $model_list = scandir($model_dir);
    if($model_list){
        foreach ($model_list as $k=>$v){
            if($v == '.' || $v == '..' || strpos($v,'.') === 0){
                unset($model_list[$k]);
            }
        }
    }
    $this->ajaxReturn($model_list);
}
//显示文件内容
public function getLogDetail(){
    if(IS_AJAX){
        $name = I('name');
        $files_name = I('files_name');
        $model_dir = dirname(dirname(dirname(__FILE__))).'/Runtime/Logs/'.$files_name.'/'.$name;
        if(file_exists($model_dir)) {
            $file_arr = file($model_dir);
            $array = [];
            for ($i = 0; $i < count($file_arr); $i++) {
                $str = $file_arr[$i];
                $preg= "/^SQL([\s\S]*)/";
                if(preg_match($preg,$str,$spl)){
                    $preg= "/RunTime\:(\d\.\d+)s/";
                    if(preg_match($preg,$str,$arr)){
                        array_push($array,array(
                            'index'=>$i+1,
                            'time'=>$arr[1],
                            'sql'=>$spl[1],
                        ));
                    }
                }
            }
            $array_data = arrayOrderBy($array,'SORT_DESC','time');//时间排序
            $res = array_slice($array_data,0,10);//前十条
            $this->ajaxReturn($res);
        }
    }
}
```
#### html
```html
<select name="dir_list" class="form-control width-200" size="10" title="" onchange="getFilesList(this.value)">
    <foreach name="dir_list" item="value">
        <option value="{$value}">{$value}</option>
    </foreach>
</select>
<select name="files" class="form-control width-200" size="10" title="" onchange="getLogDetail(this.value)"></select>
<table class="table table-bordered m-t-10">
    <thead>
    <tr>
        <th>行号</th>
        <th>时长s</th>
        <th>sql</th>
    </tr>
    </thead>
    <tbody></tbody>
</table>
```
#### javascript
```javascript
function getFilesList(name) {
    $.ajax({
        "url":"{:U('Test/getFilesList')}",
        "type":"post",
        "data":{files_name:name},
        success:function (msg) {
            var html = '';
            if(msg){
                for (var i in msg){
                    html += '<option value="'+msg[i]+'">'+msg[i]+'</option>';
                }
            }
            $('select[name="files"]').html(html);
        }
    })
}
function getLogDetail(name) {
    $.ajax({
        "url":"{:U('Test/getLogDetail')}",
        "type":"post",
        "data":{
            files_name:$('select[name="dir_list"]').val(),
            name:name
        },
        success:function (msg) {
            var html = '';
            if(msg){
                var length = msg.length;
                for (var i = 0;i<length;i++){
                    html += '<tr><td>'+msg[i]['index']+'</td><td>'+msg[i]['time']+'</td><td>'+msg[i]['sql']+'</td></tr>';
                }
            }
            $('table tbody').html(html);
        }
    })
}
```
