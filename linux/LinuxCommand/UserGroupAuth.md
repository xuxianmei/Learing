## 组操作

相关文件
/etc/group  组账户信息
/etc/gshadow 安全组账户信息
/etc/login.defs Shadow密码套件配置

### groupadd 

创建一个新的组
* 用法

````
groupadd [OPTION]  GROUPNAME
````

### groupmod
修改组信息

* 用法

````
groupmod [OPTION] GROUPNAME
````

* 常用参数

````
-n NEWNAME：将组名修改为NEWNAME
````

### groupdel

删除组，不能删除包含有用户的组。

* 用法

````
groupdel [OPTION] GORUPNAME
````
### 查看组  


* 查看所有组


````
cat  /etc/group
````

* 查看指定用户所在组

````
groups [OPTION]... [USERNAME]...
````

````
groups ：不加用户名参数，显示当前用户所在组。
````




## 用户操作

### useradd
创建一个新用户或更新默认新用户信息

* 用法

````
useradd [OPTION] USERNAME：创建新用户
useradd -D ：创建新用户的默认信息
useradd -D [OPTION]：更改创建新用户时的默认信息
````

* 常用参数

````
-b：设置基目录 BASE_DIR，如果没有使用-d设置用户主目录HOME_DIR，基目录加上账户名就是主目录  
-c：用户描述  
-d HOME_DIR：用户主目录，不指定，使用默认值
-g GROUP:指定用户所属组  ，不指定，使用默认值
-D:默认值  
-e EXPIRE_DATE 禁用日期（YYYY-MM-DD）
-s SHELL：用户的登录SHELL名。

更改默认值： user add -D [OPTION]
有效的更改默认值选项有：
-b  BASE_DIR
-e EXPIRE_DATE
-f INACTIVE:密码过期到账户被禁用之前的天数
-g GROUP
-s SHELL
````

### passwd
  
更改用户密码。

passwd用来更改用户账户的密码，普通用户通常只更改其自己的账户，而超级用户可以更改任何账户的密码。

密码更改
	如果有旧密码，首先提示用户输入旧密码。超级用户可以略过这个步骤，以便更改忘记了的密码。
* 用法

````
passwd [OPTION] [USERNAME]
````
* 常用参数

````
-a：此选项只能和-S一起使用，来显示所有用户的状态。
-d：删除用户密码（让它为空）。这是禁用一个用户快速方法。
-e：让一个账户的密码立即过期。这可以强制一个用户下次登录时更改密码。
-S ：显示账户状态信息。状态信息包含7个字段
		字段1：用户名
		字段2：密码锁定状态（L:密码已锁定，NP：没有密码，P：密码可用）
		字段3：最后一次更改密码的日期
		字段4：密码的最小年龄（天）
		字段5：密码的最大年龄（天）
		字段6：警告期（天）
		字段7：禁用期（天）
-l：锁定指定账户的密码，不允许更改密码。
-U：解锁指定用户的密码

````

### usermod

修改一个用户账户的相关信息
 * 用法

````
usermod [option] USERNAME
````

* 常用参数

````
-a：，将用户添加到附加组，只能和-G一起使用。
-d HOME_DIR:，用户的新主目录，如果和-m一起使用，当前主目录的内容将会移动到新主目录中，如果不存在，则创建。  
-g  GORUP：用户的新初始登录组
-G GROUP1[,GROUP2]...：附加组
-l NEW_NAME:新用户名
-L：锁定用户的密码
-m：将用户的主目录移动到新位置
-s SHELL：用户的默认shell。
-U:解锁用户的密码

````

### userdel

删除用户账户及相关文件

* 用法

````
userdel [option] USER
````

* 常用参数

````
-f：强制删除
-r：删除主目录中的文件
````


### gpasswd
管理/etc/group和/etc/gshadow。

* 用法

````
gpasswd [option] GROUP
````

* 常用参数
除了-A和-M选项，其它选项不能联合使用。不加任何参数，为组设置密码。
````
-a(--add) user：向GROUP中添加用户user
-d user：移除用户user
-A user1,user2：设置有管理权限的用户列表
-M user1,user2：设置组成员列表
````

### newgrp

登录到一个新组，在当前会话当中，可以使用newgrp来登录切换到另一个组。

* 用法

````
newgrp [-] [group]
````

如果使用了 “**``-``**”，用户的环境会重新初始化，或者现有的环境不会改变。
如果没有给定group，会切换回/etc/passwd中此用户的GROUP。

### w

显示登录的用户及它们正在做的事。

* 用法

````
w [options] user [...]
````




### who 
显示已登录的用户

* 用法

````
who [OPTION... [FILE | ARG1 | ARG2]
````

* 常用参数

````
-b:系统上次启动的时间 

````

### whoami

当前用户名/用户ID


## 权限操作

相关命令：umask、chmod、chgrp、chown
````
r：4
w：2
x：1
````
### umask

设置、查看默认权限

* 用法

````
umask [[0-7][0-7][0-7][0-7]]
````

### chgrp


更改文件的归属组。

* 用法

````
chgrp [OPTION]... GROUP FILE...   更改匹配的文件所有组
chgrp [OPTION]... --reference=RFILE FILE...  更改匹配文件的所有组为RFILE的所有组
````

* 常用参数

````
-R ：递归操作
````




### chown

更改文件的所有者及所有组。

* 用法

````
chown [OPTION]... [OWNER][:[GROUP]] FILE
chown [OPTION]... --reference=RFILE FILE
````

* 常用参数

````
-R;递归操作
--from=CURRENT_OWNER:CURRENT_GROUP
````

### chmod
修改文件权限

* 用法

````
chmod [OPTION]... MODE[,MODE]... FILE...
chmod [OPTION]... OCTAL-MODE FILE...
chmod [OPTION]... --reference=RFILE FILE...
````

每一个MODE都是：``[ugoa][+-=][rwxXst]``

u:拥有者
g：拥有者同组用户
o：其它用户
a：所有用户

示例：
```chmod uo+rw,g-x testfile```

OCTAL-MODE:八进制模式
```chmod 644 testfile```
* 常用参数

````
-R:递归操作
````
