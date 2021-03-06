QConf
=====

## 简介 [English](https://github.com/Qihoo360/QConf/blob/master/README.md)
QConf 是一个分布式配置管理工具。
用来替代传统的配置文件，将配置信息与程序代码分离，透明的完成配置信息的多机器同步及高效读取。使工程师从琐碎的配置修改、代码提交、配置上线流程中解放出来，极大的简化了配置管理工作。

## 特点
* 一处修改，所有机器实时同步更新
* 高效读取配置
* 安装部署方便，使用简单
* 服务器宕机、网络中断、集群迁移等异常情况对用户透明
* 支持c/c++、shell、php、python 等语言


## 编译安装
QConf采用CMake进行构建（CMake 版本 2.6及以上）

可以使用以下命令完成QConf的编译安装:
``` shell
mkdir build && cd build
cmake ..
make
make install
```
你也可以在CMake图形界面工具中导入CMakeList.txt 文件

使用如下配置可以指定QConf的安装目录:
``` shell
cmake .. -DCMAKE_INSTALL_PREFIX=/install/prefix
```
## 使用

 - 搭建Zookeeper集群，并通过Zookeeper Client 新建修改配置

	 关于zookeeper使用的更多信息: [ZooKeeper Getting Started Guide](http://zookeeper.apache.org/doc/r3.3.3/zookeeperStarted.html)
	 

 - 在QConf 配置文件中配置Zookeeper集群地址

``` shell
vi QCONF_INSTALL_PREFIX/conf/idc.conf
```
``` php
  #all the zookeeper host configuration.
  #[zookeeper]
  zookeeper.test=127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183 #test机房zookeeper配置
```
 - 在QConf配置文件中指定本地机房
``` shell
echo test > QCONF_INSTALL_PREFIX/conf/localidc #指定本地机房为test
```
 - 启动QConf

``` shell
cd QCONF_INSTALL_PREFIX/bin && sh agent-cmd.sh start
```
 - 编写代码访问QConf
 
 
## 性能
1. 测试策略
 * **测试次数** ： 循环测试1000次，每次循环获取分别获取10000个不同key对应的值，总共取一千万次key
 * **测试数据** ： 每个key对应的value的大小是1k
 * **测试方式** ： 多进程测试时候，多个进程同时运行，然后截取其中一段时间，来记录各个进程运行取一千万次的总耗时
 * **测试机器** ： Intel(R) Xeon(R) CPU E5-2630 0 @ 2.30GHz,  24核；64G memory
 * **测试语言** ： c++
2. 测试结果 
 * ![enter image description here](http://ww1.sinaimg.cn/bmiddle/69a9c739jw1eqgw9ss6nwj20600763yu.jpg "Qconf测试结果")
3. 结论
 *  单进程的延迟是16微秒左右
 *  在多进程的情况下，QPS 能够达到百万

## 使用样例

``` c
	  // Init the qconf env
      ret = qconf_init();
      assert(QCONF_OK == ret);

      // Get Conf value
      char value[QCONF_CONF_BUF_MAX_LEN];
      ret = qconf_get_conf("/demo/node1", value, sizeof(value), NULL);
      assert(QCONF_OK == ret);

      // Destroy qconf env
      qconf_destroy();
```

## 文档
[Getting Started](https://github.com/Qihoo360/QConf/blob/master/doc/QConf%20Getting%20Started%20Guide.md) - QConf 使用说明，包括QConf的安装，运行，API等信息
## 联系方式

* 邮箱: g-qconf@list.qihoo.net
* QQ群: 438042718
