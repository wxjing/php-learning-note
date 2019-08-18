#Thinkphp 生成Model文件
***
```php
public function createModelFile(){
        //获取所有表名
        $sql = 'show tables';
        $tables = M()->query($sql);
        $tables = array_column_fun($tables,'tables_in_lawyer_sys');
        //获取Model目录及所有文件名
        $model_dir = dirname(dirname(__FILE__)).'/Model/';
        $model_list = scandir($model_dir);
        $create_model_file = array();
        foreach ($tables as $k=>$v){
            $name = substr($v,3);
            $names = explode('_',$name);
            $data = array();
            foreach ($names as $kk=>$vv){
                array_push($data,ucfirst($vv));
            }
            $model_name = implode('',$data);
            $model_file_name = implode('',$data).'Model.class.php';
            if(!in_array($model_file_name,$model_list)){
                $model_str = <<<AAA
<?php
namespace Admin\Model;
use Think\Model;
class {$model_name}Model extends Model
{
    protected \$tableName = '{$name}';
}
AAA;
                if(file_put_contents($model_dir.$model_file_name,$model_str)) {
                    array_push($create_model_file,$model_file_name);
                }
            }
        }
        $this->ajaxReturn($create_model_file);
    }
```
