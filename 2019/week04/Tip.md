# Charles抓包

客户端开发，离不开API请求。调试服务器端API、moc数据，离不开抓包工具。在macOS上，用的比较多的是Charles。

电脑和手机，处于同一个局域网内。

## 1. 设置代理

1. Mac端设置，Proxy -> Proxy Settings，设置代理端口
2. 手机端设置，设置 -> 无线局域网 -> 连接的网络 -> 配置代理 -> 手动 -> 服务器及端口

## 2. 抓取HTTPS

1. 电脑安装证书，Help -> SSL Proxying -> Install Charles Root Certificate
2. 手机安装证书，Help -> SSL Proxying -> Install Charles Root Certificate on a Mobile Device or Remote Browser
	* 设置手机代理
	* 下载证书，chls.pro/ssl
	* iOS以上的系统，信任证书，Settings -> General -> About -> Certificate Trust。把下载的证书，设置为信任状态
3. Mac端，设置 SSL Proxying 代理。Proxy -> SSL Proxy Settings -> SSL Proxying，添加 `*:443`。可以在Mac端过滤抓取的域名，Proxy -> Recording Settings -> Include，添加要抓取的地址。
	
## 3. 修改返回数据

1. 保存 Response。选择需要保存的请求，右键 Save Response，进行保存。
2. 修改 Response。选择需要修改的请求，右键 Map Local，选择上一步保存的文件，将请求映射本地文件。
3. 修改数据。修改第一步保存的数据，可修改请求的返回数据。

## 4. map remote

映射到远程环境。使用场景，测试App，需要验证线上App的bug。一种方式是，在App中，设置可切换环境的入口；另一种方式，就是使用map remote，将测试的url map 为 remote url。
