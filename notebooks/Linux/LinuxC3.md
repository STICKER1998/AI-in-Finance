Chapter 3 Linux 用户和权限
=================================
## Section 3.1 root用户
### 1.root用户 vs 普通用户
Windows，MacOS，Linux等系统均采用多用户的管理模式进行权限管理。在Linux系统中，拥有最大权限的账户名为root（超级管理员），root用户拥有最大的系统操作权限，而普通用户在很多地方的权限受到限制。

**例子**：普通用户无法在根目录下创建文件，但是root用户可以；
- 普通用户的权限一般在其home目录内是不受限的；
- 一旦出了home目录，普通用户基本只有**只读**和**执行**权限，没有修改权限；

### 2.切换用户的命令

#### (1) 切换到[用户名]用户
```
su [-] [用户名]
```
#### (2) 回退到上一个用户
```
exit
```
- 使用普通用户切换到root用户，需要输入密码，反之不需要；
- 如果不指定用户名，则默认向root用户切换；

### 3.`sudo`用户
#### (1) `sudo`语法
在我们得知root密码的时候，可以通过su命令切换到root用户得到最大权限，但是不建议长期使用root用户，避免给系统带来破坏。此时，可以使用`sudo`命令，使得普通用户临时以root身份运行。
```
sudo 其他命令
```

- 在其他命令之前带上`sudo`，即可为这一条命令临时赋予root授权；
- 但并不是所有用户都可以使用`sudo`，我们需要为普通用户设置`sudo`认证；


#### (2) 为普通用户配置`sudo`认证
- 切换到root用户，执行`visodu`命令，会自动通过vi编辑器打开：`/etc/sudoers`
- 在文件的最后一行添加
  ```
  普通用户用户名 ALL = (ALL) NOPASSPORT: ALL
  ```
- 通过`:wq`保存
- 切换回普通用户
- 执行的命令均已root执行
  