#### 部署
##### 第一步：准备环境
安装Java(Java 8 或者 11，8以上版本均可)，并设置JAVA_HOME

##### 第二步：下载Sea Tunnel
进入下载页（https://seatunnel.apache.org/download/），下载最新版本seatunnel-<version>-bin.tar.gz。

> 快速下载，关注公众号：飞桨PPDB，回复st,获取百度网盘下载链接。

或者直接通过在终端中执行命令
```sh
export version="2.3.1"
wget "https://archive.apache.org/dist/incubator/seatunnel/${version}/apache-seatunnel-incubating-${version}-bin.tar.gz"
tar -xzvf "apache-seatunnel-incubating-${version}-bin.tar.gz"
```

##### 第三步：安装连接器插件
进入安装根目录，执行
```sh
sh bin/install-plugin.sh
```
也可以指定版本，如2.3.0-beta
```sh
sh bin/install-plugin.sh 2.3.0-beta
```

通常我们并不需要所有的插件，这时我们可以修改`config/plugin_config`文件，例如我们只需要`connector-console`插件，我们修改为：
```sh
--connectors-v2--
connector-console
--end--
```
如果想要跑通示例的话，需要改为：
```sh
--connectors-v2--
connector-fake
connector-console
--end--
```
默认会安装所有插件，建议按需添加。

关注我，带你深入了解SeaTunnel技术及应用。
![飞桨PPDB](https://ai-studio-static-online.cdn.bcebos.com/e939f12ab7034a069fb4581dec21bb233473ed75fdd543d683982921ddb69167)