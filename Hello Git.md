### Hello Git

## 初始化

1. ``` java
   首先需要初始化本地git设置 
        git config --global user.name "runoob"  用户名
   	 git config --global user.email test@runoob.com  用户邮箱
   ```

2. ``` java
   ssh-keygen -t rsa -C "youremail@example.com"  生成秘钥  随后将秘钥复制到github中 即可无秘登录
   ```

   

---

## git操作

1. ```java
   git init 首先将本地的文件进行git初始化
   ```

2. ``` java
   git add . 将该文件夹中的内容加载到缓存区
   ```

3. ```java
   git commit -m "关于本次更新的说明"  将内容提交到本地仓库
   ```

4. ```java
   git remote add origin github仓库的ssh链接   将github仓库告知本地仓库
   ```

5. ```java
   git push -u origin master 将文件从本地仓库提交到github仓库中
   ```