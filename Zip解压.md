```PHP
function uploadZipFileHandle($fileObj=null) {
    if($fileObj!=null && $fileObj['error']==0){
        $upload = new \Think\Upload();// 实例化上传类
        $upload->maxSize = 1024*1024*20 ;// 设置附件上传大小
        $upload->exts = array('zip');// 设置附件上传类型
        $upload->subName = "";
        $upload->saveName = $fileObj["name"];
        $file_all = explode('.', $fileObj["name"]);
        $storePath = './Pages/zips/';
        if(!is_dir($storePath)){
            mkdir($storePath, 0777, true);
        }$upload->rootPath = $storePath; // 设置附件上传根目录
        $upload->savePath = ''; // 设置附件上传（子）目录
        $info = $upload->uploadOne($fileObj);
        sleep(2);


        $zip = new ZipArchive;
        $res = $zip->open($storePath.$info['savepath'].$info['savename']);
        if ($res === TRUE) {
            $zip->extractTo('./Pages/');
            $zip->close();
            sleep(2);
            @unlink($storePath.$info['savepath'].$info['savename']);
        }


        return $file_all[0];
    }
}

```
