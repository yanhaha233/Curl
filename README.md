## CURL

### CURL 整理 （根据阮一峰的网络日志整理）

##### 一、查看网页源码

curl 直接加上网址，可以看到网页源码。

```shell
$ curl www.baidu.com
```

如果要保存网页，可以使用`-o`参数，相当于 wget 命令。

```shell
$ curl -o [文件名] www.baidu.com
```

##### 二、自动跳转

有的网址是自动跳转的。使用`-L`参数，curl 就会跳转到新的网址。

```shell
$ curl -L www.sina.com
```

##### 三、显示头信息

`-i`参数可以显示 http response 头信息，连同网页代码一起。

```shell
$ curl -i www.baidu.com
```

`-I`则是只显示头信息

##### 四、显示通信过程

`-v`参数可以显示一次 http 通信的整个过程，包括端口连接和 http request 头信息。

```shell
$ curl -v www.baidu.com
```

如果上面的信息还不够，那么下面的命令可以查看更详细的通信过程。

```shell
$ curl --trace output.txt www.baidu.com
#或者
$ curl --trace-ascii output.txt www.baidu.com
```

运行之后打开 output.txt 文件查看

##### 五、发送表单信息

发送表单信息有 GET 和 POST 两种方法。GET 方法相对简单，只要把数据附在网址后面即可。

```shell
$ curl example.com/form.cgi?data=xxx
```

POST 方法必须把数据和网址分开，curl 就要用到--data 参数。

```shell
$ curl -X POST --data "data=xxx" example.com/form.cgi
```

如果你的数据没有经过表单编码，还可以让 curl 为你编码，参数是`--data-urlencode`。

```shell
$ curl -X POST--data-urlencode "date=April 1" example.com/form.cgi
```

##### 六、HTTP 动词

curl 默认的 HTTP 动词是 GET，使用`-X`参数可以支持其他动词。

```shell
$ curl -X POST www.example.com
```

```shell
$ curl -X DELETE www.example.com
```

##### 七、文件上传

假定文件上传的表单是下面这样的:

```html
<form method="POST" enctype="multipart/form-data" action="upload.cgi">
  <input type="file" name="upload" />
  <input type="submit" name="press" value="OK" />
</form>
```

你可以用 curl 这样上传文件：

```shell
$ curl --form upload=@localfilename --form press=OK [URL]
```

##### 八、Referer 字段

```shell
$ curl --referer http://www.example.com http://www.example.com
```

##### 九、User Agent 字段

这个字段是用来表示客户端的设备信息。服务器有时会根据这个字段，针对不同设备，返回不同格式的网页，比如手机版和桌面版。
iPhone4 的 User Agent 是

```
Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_0 like Mac OS X; en-us)
AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8A293 Safari/6531.22.7
```

curl 可以这样模拟：

```shell
$ curl --user-agent "[User Agent]" [URL]
```

##### 十、cookie

使用`--cookie`参数，可以让 curl 发送 cookie。

```shell
$ curl --cookie "name=xxx" www.example.com
```

至于具体的 cookie 的值，可以从 http response 头信息的`Set-Cookie`字段中得到。
`-c cookie-file`可以保存服务器返回的 cookie 到文件
`-b cookie-file`可以使用这个文件作为 cookie 信息，进行后续的请求

```shell
$ curl -c cookies http://example.com
$ curl -b cookies http://example.com
```

##### 十一、添加头信息

有时需要在 http request 之中，自行增加一个头信息。`--header`参数就可以起到这个作用。

```shell
$ curl --header "Content-Type:application/json" http://example.com
```

##### 十二、HTTP 认证

有些网域需要 HTTP 认证，这时 curl 需要用到`--user`参数。

```shell
$ curl --user name:password example.com
```
