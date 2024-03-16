# 1. tmux使用命令

## 1. 启动和退出

1. 安装

   ```
   # Ubuntu 或 Debian
   $ sudo apt-get install tmux
   
   # CentOS 或 Fedora
   $ sudo yum install tmux
   
   # Mac
   $ brew install tmux
   ```

1. 进入tmux窗口：**输入 tmux**
   进入tmux窗口后，底部有一个状态栏。状态栏左侧是窗口信息（编号和名称），右侧是系统信息
   
3. 退出 tmux窗口：**按下 Ctrl + d 或者输入exit**

3. 前缀键：**Ctrl + b**（如果使用了配置文件就是修改为了ctrl + a）
   tmux 窗口有大量的快捷键。所有快捷键都要通过前缀键唤起。默认的前缀键是`Ctrl+b`，即先按下`Ctrl+b`，快捷键才会生效。
   举例来说，帮助命令的快捷键是`Ctrl+b ?`。它的用法是，在 tmux 窗口中，先按下`Ctrl+b`，再按下`?`，就会显示帮助信息。
   然后，按下 ESC 键或`q`键，就可以退出帮助。

---

## 2. 会话管理

1. 新建会话
   第一个启动的 Tmux 窗口，编号是`0`，第二个窗口的编号是`1`，以此类推。这些窗口对应的会话，就是 0 号会话、1 号会话。

   使用编号区分会话，不太直观，更好的方法是为会话起名。

   ```
   tmux new -s tom
   ```

   上面命令是新建一个名为 tom 的会话。

2. 分离会话：**ctrl + b  d** 或者输入 **tmux detach**

3. 查看当前所有会话列表：**tmux ls**

4. 接入会话：**tmux attach**

   ```
   # 使用会话编号接入
   tmux attach -t 0
   
   # 使用会话名称接入tom
   tmux attach -t tom
   ```

5. 杀死会话：**tmux kill-session**

   ```
   # 使用会话编号
   tmux kill-session -t 0
   
   # 使用会话名称杀死tom
   tmux kill-session -t tom
   ```

6. 切换会话：**tmux switch**

   ```
   tmux switch -t 0
   ```

7. 重命名会话：**tmux rename-session**

   ```
   # 将编号为0的会话重命名为tom
   tmux rename-session -t 0 tom
   ```

---

## 3. 窗格操作

Tmux 可以将窗口分成多个窗格（pane），每个窗格运行不同的命令。以下命令都是在 Tmux 窗口中执行。

下面是一些窗格操作的快捷键。

```
# 划分左右两个窗格
$ Ctrl+b %

# 划分上下两个窗格
$ Ctrl+b "

# 光标切换到其他窗格。<arrow key>是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键↓
$ Ctrl+b <arrow key>

# 光标切换到上一个窗格
$ Ctrl+b ;

# 光标切换到下一个窗格
$ Ctrl+b o

# 当前窗格与上一个窗格交换位置
$ Ctrl+b {

# 当前窗格与下一个窗格交换位置
$ Ctrl+b }

# 所有窗格向前移动一个位置，第一个窗格变成最后一个窗格
$ Ctrl+b Ctrl+o

# 所有窗格向后移动一个位置，最后一个窗格变成第一个窗格
$ Ctrl+b Alt+o

# 关闭当前窗格
$ Ctrl+b x

# 将当前窗格拆分为一个独立窗口
$ Ctrl+b !

# 当前窗格全屏显示，再使用一次会变回原来大小
$ Ctrl+b z

# 按箭头方向调整窗格大小
$ Ctrl+b Ctrl+<arrow key>

# 显示窗格编号
$ Ctrl+b q
```



---

## 4. 窗口管理

除了将一个窗口划分成多个窗格，Tmux 也允许新建多个窗口。

1. 新建窗口：

   ```
   # 新建一个窗口
   tmux new-window
   
   # 新建一个指定名称为tom的窗口
   tmux new-window -n tom
   ```

2. 切换窗口

   ```
   # 切换到指定的编号为0的窗口
   tmux select-window -t 0
   
   # 切换到指定名称为tom的窗口
   tmux select-window -t tom
   ```

3. 重命名窗口

   ```
   tmux rename-window tom
   ```

4. 窗口快捷键

   ```
   # 创建一个新窗口，状态栏会显示多个窗口的信息。
   $ Ctrl+b c
   
   # 切换到上一个窗口（按照状态栏上的顺序）
   $ Ctrl+b p
   
   # 切换到下一个窗口
   $ Ctrl+b n
   
   # 切换到指定编号的窗口，其中的<number>是状态栏上的窗口编号
   $ Ctrl+b <number>
   
   # 从列表中选择窗口
   $ Ctrl+b w：
   
   # 窗口重命名
   $ Ctrl+b ,
   ```

---

## 5. 其他命令

```
# 列出所有快捷键，及其对应的 Tmux 命令
$ tmux list-keys

# 列出所有 Tmux 命令及其参数
$ tmux list-commands

# 列出当前所有 Tmux 会话的信息
$ tmux info

# 重新加载当前的 Tmux 配置
$ tmux source-file ~/.tmux.conf
```



---



# 2. Docker教程

## 1. 将当前用户添加到docker用户组

为了避免每次使用docker命令都需要加上sudo权限，可以将当前用户加入安装中自动创建的docker用户组(可以参考官方文档)：

```
sudo usermod -aG docker 用户名

sudo usermod -aG docker wxd
```

执行完此操作后，需要退出服务器，再重新登录回来，才可以省去sudo权限。



## 2. 镜像（images）

```shell
# 拉取一个镜像
$ docker pull ubuntu:20.04

# 列出本地所有镜像
$ docker images

# 列出运行的镜像
$ docker ps

# 删除镜像ubuntu:20.04
$ docker image rm ubuntu:20.04 或 docker rmi ubuntu:20.04

# 创建某个container的镜像
$ docker [container] commit CONTAINER IMAGE_NAME:TAG

# 将镜像ubuntu:20.04导出到本地文件ubuntu_20_04.tar中
$ docker save -o ubuntu_20_04.tar ubuntu:20.04

# 将镜像ubuntu:20.04从本地文件ubuntu_20_04.tar中加载出来
$ docker load -i ubuntu_20_04.tar
```



## 3. 容器（container）

![image-20220503221537590](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220503221537590.png)

## 4. 实战

```
# 将镜像上传到自己租的云端服务器
$ scp /var/lib/acwing/docker/images/docker_lesson_1_0.tar server_name: 

# 登录自己的云端服务器
$ ssh server_name  

# 将镜像加载到本地
$ docker load -i docker_lesson_1_0.tar  

# 创建并运行docker_lesson:1.0镜像
$ docker run -p 20000:22 --name my_docker_server -itd docker_lesson:1.0  

# 进入创建的docker容器
$ docker attach my_docker_server  

# 设置root密码
$ passwd  
```

去云平台控制台中修改安全组配置，放行端口20000。

返回AC Terminal，即可通过ssh登录自己的docker容器：

ssh root@xxx.xxx.xxx.xxx -p 20000  # 将xxx.xxx.xxx.xxx替换成自己租的服务器的IP地址
然后创建工作账户acs。

最后，可以参考4. ssh——ssh登录配置docker容器的别名和免密登录。

## 5.docker增加容器的映射端口：80与443

因为使用docker生成好的容器新增端口不方便，所以将原来的容器保存成镜像重新生成一个容器就行。

1. 登录容器，关闭所有运行的服务。

2. 登录运行容器的服务器 ，然后执行：

   ```
   # 1.将容器保存成镜像，将CONTAINER_NAME替换成容器名称(django_lesson:1.1是新的镜像的名称)
   $ docker commit CONTAINER_NAME django_lesson:1.1  
   
   # 2.关闭容器
   $ docker stop CONTAINER_NAME  
   
   # 3.删除容器
   $ docker rm CONTAINER_NAME 
   
   
   # 4.使用保存的镜像重新创建容器
   docker run -p 20000:22 -p 8000:8000 -p 80:80 -p 443:443 --name CONTAINER_NAME -itd django_lesson:1.1
   ```

3. 去云服务器控制台，在安全组配置中开放80和443端口。



# 3. ssh登录

## 1. 基本用法

远程登录服务器：

```
ssh user@Hostname

user：用户名
HostName：IP地址或域名
```

第一次登录时会提示：

```
The authenticity of host 'host (12.18.429.21)' can't be established.
RSA key fingerprint is 98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d.
Are you sure you want to continue connecting (yes/no)?
```

输入yes，然后回车即可，这样会将该服务器的信息记录在~/.ssh/known_hosts文件中。

然后输入密码即可登录到远程服务器中。



默认登录端口号为22.如果想登录某一特定端口：

```
ssh user@hostname -p 22
```



## 2. 配置文件

创建文件 ~/.ssh/config

```
vim config
```

然后在文件中输入：

```
Host myserver1
	HostName IP地址或域名
	User 用户名

Host myserver2
	HostName IP地址或域名
	User 用户名
```

之后再使用服务器是，可以直接使用别名myserver1、myserver2



## 3. 密钥登录

创建密钥：

```
ssh-keygen
```

然后一直回车即可。



执行完成后，~/.ssh/ 目录下会多两个文件：

```
id_rsa : 私钥
id_rsa.pub : 公钥
```

之后想免密登录那个服务器，就将公钥传给哪个服务器即可。

例如：想免密登录myserver服务器。则将公钥中的内容，复制到myserver中的 ~/.ssh/authorized_keys 文件即可。

也可使用如下命令一键添加公钥：

```
ssh-copy-id myserver
```



---

# 4. SCP进行跨机器拷贝

## 1. 将文件复制到本地

```
# 将用户名为wxd的服务器的myubt20_04.tar文件复制到ac terminal
$ scp wxd:myubt20_04.tar .
```



## 2. 上传文件到远程服务器

```
$ scp myubt20_04.tar wxd:

# 然后就可以将myubt20_04.tar镜像提取出来就行
$ docker load -i myubt20_04.tar
```



---

# 5. Django项目

## 1. 创建Django

```
$ django-admin startproject name

# 创建一个名为webapp的aj项目
$ django-admin startproject webapp
```



## 2. 初始化git

```
1. 在django项目中输入 git init
$ git init

会出现下面语句，意思是已经初始化一个空的仓库，说明初始化成功
Initialized empty Git repository in /home/wxd_dj/webapp/.git/


2. 进行Git全局设置：输入以下语句
$ git config --global user.name "wxd"
$ git config --global user.email "1924118115@qq.com"
```

![image-20220504202138506](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220504202138506.png)

```
注意：我们可以配置一下免密登录
将 .ssh/目录下的公钥（id_rsa.pub)里面的内容复制到git的添加密钥中就行了

然后就可以使用以下命令提交项目到git了：
git status
git add . 
git commit -m "" 
git remote add origin https://git.acwing.com/XXXTENTX/webapp.git：只需要第一次提交时写
git push
```



## 3. 运行Django项目

1. 在webapp目录下输入（注意最好在tmux里面启动，这样就算退出终端，项目任然可以运行）： 

   ```
   python3 manage.py runserver 0.0.0.0:8000
   ```

2. 在下面的文件中的ALLOWED_HOSTS中将IP地址填入（首次启动需要）：

   ```
   ~webapp/webapp目录下 vim settings.py
   ```

   

![image-20220504213459916](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220504213459916.png)

![image-20220504213758317](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220504213758317.png)

3. 再次启动项目就行：python3 manage.py runserver 0.0.0.0:8000

4. 可以git提交的时候忽略文件

   ```
   忽略__pycache__文件
   先 vim .gitignore
   后 **/__pycache__
   保存即可,之后只要需要ignore文件在里面添加即可
   ```

5. 运行startapp

   ```
   python3 manage.py startapp game
   game是文件的名字，可以随便取
   ```

6. 更新数据库

   ```
   python3 manage.py migrate
   ```

7. 创建管理员用户

   ```
   python3 manage.py createsuperuser
   输入用户名：admin
   密码：123456
   ```

## 4.Django项目运行模式

1. 项目系统设计：

   ```
   menu：菜单页面
   playground：游戏界面
   settings：设置界面
   ```

2. 项目文件结构：

   ```
   templates目录：管理html文件
   
   urls目录：管理路由，即链接与函数的对应关系
   
   views目录：管理http函数
   
   models目录：管理数据库数据
   
   static目录：管理静态文件，比如：
       css：对象的格式，比如位置、长宽、颜色、背景、字体大小等
       js：对象的逻辑，比如对象的创建与销毁、事件函数、移动、变色等
       image：图片
       audio：声音
       
   consumers目录：管理websocket函数
   ```

   ### 页面展示过程

   1. 先打开网页连接 http://8.142.45.249:8000/ 因为后面是没有路径的，所以会链接到总路由（路径是webapp/webapp/urls.py)中后面路径为空的路由![image-20220507002728939](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220507002728939.png)

      ··如果是在网页链接后面输入 http://8.142.45.249:8000/admin 则会链接到admin页面

   2. 之后根据路由会链接到 game/urls/index.py 这个路由，因为又是链接又是空的，所以会来链接到 game/views/index.py 里面的东西，然后就会根据views里面的函数返回对应页面的html内容。
      ![image-20220507003120099](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220507003120099.png)
      ![image-20220507003324188](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220507003324188.png)

# 5.项目运行
1. nginx：sudo /etc/init.d/nginx start
2. uwsgi：uwsgi --ini scripts/uwsgi.ini
   

