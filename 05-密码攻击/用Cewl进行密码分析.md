# 用Cewl进行密码分析

![](https://gitee.com/itlab/images/raw/master/img/导出图片Wed Jul 15 2020 11_03_33 GMT+0800 (中国标准时间).png)

### 引言

信息安全的对抗，就是获取权限的对抗；使用密码是系统对特殊权限的角色进行身份验证的一种方式。接下来几天，浪哥会连续输出和密码有关的文章。加密和破解是一对欢喜冤家，破解时有一个好用的字典，能让破解更easy!暴力破解考验更多的是计算机的算力和每个人运气。一个有针对性的字典，对密码破解尤为重要。

![](https://gitee.com/itlab/images/raw/master/img/unnamed (1).jpg)

### Cewl介绍

Cewl是一个通过指定url及深度，使用爬虫技术，生成字典的一个工具。Cewl是通过ruby编写，通过爬取网站并提取独立的单词保存为字典，可以和John the Ripper等工具配合使用。 [github链接](https://github.com/digininja/CeWL) 

你可以安装在任意平台上安装Cewl，Kali linux上是预装好的，我们可以开箱即用。在Kali中可以通过Terminal输入cewl，打开软件，也可以通过Application--05-密码攻击--Cewl。

Cewl是一个命令行工具，使用语法是 cewl [选项] <站点url>

首先输入cewl -h,查看帮助，看看这个小工具能干写什么。

```shell
选项：
  -h，--help：显示帮助。
  -k，-keep：保留下载的文件。
  -d <x>，-depth <x>：到蜘蛛的深度，默认为2。
  -m，--min_word_length：最小字长，默认为3。
  -o，--offsite：让蜘蛛访问其他站点。
  -w，--write：将输出写入文件。
  -u，--ua <agent>：要发送的用户代理。
  -n，--no-words：不输出单词表。
  --with-numbers：接受带有数字以及字母
  -a，--meta：包括元数据的单词。
  --meta_file文件：元数据的输出文件。
  -e，--email：包括电子邮件地址。
  --email_file <文件>：电子邮件地址的输出文件。
  --meta-temp-dir <dir>：exiftool解析文件时使用的临时目录，默认为/ tmp。
  -c，--count：显示找到的每个单词的计数。
  -v，--verbose：详细。
  --debug：额外的调试信息。

  身份验证
  --auth_type：Digest or basic
  --auth_user：身份验证用户名。
  --auth_pass：验证密码。

  代理支持
  --proxy_host：代理主机。
  --proxy_port：代理端口，默认为8080
  。--proxy_username：代理的用户名（如果需要）。
  --proxy_password：代理的密码（如果需要）。

  标头
  --header，-H：格式为name：value-可以传递多个。

  <url>：蜘蛛站点。
```



### Cewl 示例

+ 默认输出

  ```shell
  cewl http://www.xyz.com/
  ```



+ 保存字典文件

  ``` shell
  cewl http://www.xyz.com/ -w dict.txt	
  ```

  指定字典的最小长度

  ```shell
  cewl http://www.xyz.com/ -m9 -w dict.txt
  ```

+ 获取网站的Email地址

  通过-e来开启Email参数，并通过-n隐藏生成的密码字典，可以发现email地址。

  ```shell
  cewl http://www.xyz.com/ -n -e
  ```

+ 开启爬取结果中单词出现的次数
	```shell
	cewl http://www.xyz.com/ -c
	```

+ 增加爬取深度

  默认爬取深度是2，可以增加爬取深度获取更大的字典。

  ```shell
  cewl http://www.xyz.com/ -d 3
  ```

+ 提取调试信息

  在爬取的过程中可能会出现一个报错，通过开启调试模式，可以看到错误和一些Html元数据，从而获取更详细信息以供判单。

  ```shell
  cewl http://www.xyz.com/ --debug
  ```

+ 生成包含数字和字符的字典

  ```shell
  cewl http://www.xyz.com/ --with-numbers
  ```

+ 开启认证来爬取

  ```
  cewl http://www.xyz.com/ --auth_type Digest --auth_uer admin --auth_pass password -v
  ```

+ 开启代理

  如果目标网站对IP设置了黑名单，或者想隐藏真实IP去获取信息，可以开启代理。

  ```shell
  cewl --proxy_host 127.0.0.1 --proxy_poxy 10808 -w dirct.txt -d 3 http://www.xyz.com/
  ```



### 最后

信息安全的会设计网络、系统和编程三个方面，从安全的顶层来看还会涉及更多的知识。浪哥今后会持续输出更多的干货，使自己处于一个持续学习的状态，有兴趣的朋友可以关注我的公众号。






