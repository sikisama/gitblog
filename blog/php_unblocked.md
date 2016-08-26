<!--
author: zhangxuefeng
date: 2016-08-16
title: PHP非阻塞模式
tags: 阻塞模式,php
category:php
status: publish
summary: PHP非阻塞模式
-->
让PHP不再阻塞当PHP作为后端处理需要完成一些长时间处理，为了快速响应页面请求，不作结果返回判断的情况下，可以有如下措施：

### 一、FastCGI模式，使用fastcgi_finish_request()能马上结束会话，但PHP线程继续在跑。

```
echo "program start.";
 
file_put_contents('log.txt','start-time:'.date('Y-m-d H:i:s'), FILE_APPEND);
fastcgi_finish_request();
sleep(1);
echo 'debug...';
file_put_contents('log.txt', 'start-proceed:'.date('Y-m-d H:i:s'), FILE_APPEND);
 
sleep(10);
file_put_contents('log.txt', 'end-time:'.date('Y-m-d H:i:s'), FILE_APPEND);
```
这个例子输出结果可看到输出program start.后会话就返回了，所以debug那个输出浏览器是接收不到的，而log.txt文件能完整接收到三个完成时间。

### 二、使用fsockopen、cUrl的非阻塞模式请求另外的网址

```
$fp = fsockopen("www.example.com", 80, $errno, $errstr, 30);
if (!$fp) die('error fsockopen');
stream_set_blocking($fp,0);
$http = "GET /save.php  / HTTP/1.1\r\n";    
$http .= "Host: www.example.com\r\n";    
$http .= "Connection: Close\r\n\r\n";
fwrite($fp,$http);
fclose($fp);
```
利用cURL中的curl_multi_*函数发送异步请求

```
$cmh = curl_multi_init();
$ch1 = curl_init();
curl_setopt($ch1, CURLOPT_URL, "http://localhost:6666/child.php");
curl_multi_add_handle($cmh, $ch1);
curl_multi_exec($cmh, $active);
echo "End\n";
```
### 三、使用Gearman、Swoole扩展
Gearman是一个具有php扩展的分布式异步处理框架，能处理大批量异步任务；
Swoole最近很火，有很多异步方法，使用简单。

### 四、使用redis等缓存、队列，将数据写入缓存，使用后台计划任务实现数据异步处理。
这个方法在常见的大流量架构中应该很常见吧

### 五、极端的情况下，可以调用系统命令，可以将数据传给后台任务执行，个人感觉不是很高效。

```
$cmd = 'nohup php ./processd.php $someVar >/dev/null  &';
`$cmd`
```
### 六、外国佬的大招，没看懂，php原生支持
http://nikic.github.io/2012/12/22/Cooperative-multitasking-using-coroutines-in-PHP.html

### 七、安装pcntl扩展，使用pcntl_fork生成子进程异步执行任务，个人觉得是最方便的，但也容易出现zombie process。

```
if (($pid = pcntl_fork()) == 0) {
    child_func();    //子进程函数，主进程运行
} else {
    father_func();   //主进程函数
}
 
echo "Process " . getmypid() . " get to the end.\n";
 
function father_func() {
    echo "Father pid is " . getmypid() . "\n";
}
 
function child_func() {
    sleep(6);
    echo "Child process exit pid is " . getmypid() . "\n";
    exit(0);
}
```