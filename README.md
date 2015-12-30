#HttpSQS (基于HTTP协议的简单队列服务) 使用示例

## 初始化操作 

    include "path/HttpSQS.php"

    /**
     *@param $host 默认 '192.168.1.36'
     *@param $port 默认 '1218'
     *@param $auth 默认 'wlb_ylb.sqs'
     *@param $auth 默认 'utf-8'
     *
     */
    $httpsqs = new HttpSQS($host, $port, $auth, $charset);

    //也可以使用默认值
    $httpsqs = new HttpSQS();


## 入队操作

    /**
     *  将文本字符串'example1' 放入 队列 'newiep'中
     *
     *  @return bool
     */

    $httpsqs->put('newiep','example1');


## 出队操作

    /**
     * 出队操作 两种方式 get 和 gets
     */

    //get: 如果队列为空 返回 "HTTPSQS_GET_END"
    $result = $httpsqs->get('newiep'); //example1

    //gets: 队列为空返回 array('pos'=>null, 'data'=> "HTTPSQS_GET_END" );
    $result = $httpsqs->gets('newiep'); // array('pos'=>1, 'data'=>'example1');


## 查看队列状态


    //字符串形式
     HTTP Simple Queue Service v1.7 
    ------------------------------ 
    Queue Name: newiep 
    Maximum number of queues: 1000000 
    Put position of queue (1st lap): 13 
    Get position of queue (1st lap): 12 
    Number of unread queue: 1 

    $result = $httpsqs->status('newiep');

    //json 格式
    {"name":"xoyo","maxqueue":1000000,"putpos":45,"putlap":1,"getpos":6,"getlap":1,"unread":39}
    $result = $httpsqs->status('newiep', 'status_json');


## 查看队列指定位置
    
    //查看队列 **newiep** 下标为 **1** 的值
    $result = $httpsqs->view('newiep', 1);

## 重置指定队列

    //将 **newiep** 队列重置，从开始位置重新写入
    $result = $httpsqs->reset('newiep'); //true

## 更改指定队列的最大队列数量,默认 100万条

    //修改队列 **newiep** 最大数量（$num >=10 && $num <= 1000000000）

    $result = $httpsqs->maxqueue('newiep', 1000); //true

    $result = $httpsqs->maxqueue('newiep', 1); //false

## 不停止服务的情况下，修改定时刷新内存缓冲区内容到磁盘的间隔时间
    
    //　默认间隔时间：5秒

    $result = $foo->synctime(10); //true