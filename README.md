# anti-captcha.shuosc.org

```
     _          _   _                       _       _           
    / \   _ __ | |_(_)       ___ __ _ _ __ | |_ ___| |__   __ _ 
   / _ \ | '_ \| __| |_____ / __/ _` | '_ \| __/ __| '_ \ / _` |
  / ___ \| | | | |_| |_____| (_| (_| | |_) | || (__| | | | (_| |
 /_/   \_\_| |_|\__|_|      \___\__,_| .__/ \__\___|_| |_|\__,_|
                                     |_|                        
```

> 针对上海大学校内站的验证码识别服务

## 部署

可采用 Docker 进行快速部署。

```
$ docker pull stormyyd/shu-anti-captcha
```

注意：采用 Docker 部署时还需再部署一个 HTTP 服务器软件，比如 nginx，并通过 wsgi 协议来访问 Docker 容器的 9000 端口。以 nginx 为例，在将 nginx 容器与本服务的容器连接在一起后，在 nginx 的配置文件中加入下面的代码：

```
uwsgi_pass anti-captcha:9000; #anti-captcha 为此服务的容器名称
```

关于容器互联：[容器互联 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/content/network/linking.html)

## 使用方法

基本上也是校内的人用吧我就不用英文了233333

~~服务入口是  https://anti-captcha.shuosc.org （不接收 http ）。~~

请求接口：https://anti-captcha.stormyyd.xyz

PS：原作者给的接口挂掉了，我部署了一个，服务器在国外，速度略慢。

以`captcha`作为 key，用`POST`方法上传验证码图片数据就行，要求不大于 2M，且仅限 `jpg` 格式。

### 请求

~~Host: https://anti-captcha.shuosc.org  # 不接受 http~~
```
Host: https://anti-captcha.stormyyd.xyz
Method: POST
Body:
	captcha: captcha image data		  # JPG image (< 2M) 
```

由于不同站点的验证码形式不同，用户需要根据自己的目标站点调用对应的接口：

+ `/jwc` - 教务处以及选课网站
+ `/xgb` - 成就系统与学工办网站
+ `/phylab` - 物理实验网站

## 响应

服务以 JSON 格式返回结果。

#### 成功

```json
{
    "result": "1045",
  	"succeed": 1
}
```

#### 失败

```json
{
    "reason": "this file type is no supported.",
  	"succeed": 0
}
```



## 关于本服务（ry

没什么技术含量（捂脸），教务处和物理实验是自己打标签训练的卷积神经网络，学工办那一套太听话了直接tesseract摸了。

应该会找时间写个博文，训练集可以群里找我要，不过你直接爬虫爬一堆问这个系统要结果准确率应该也都在95%以上（溜了）