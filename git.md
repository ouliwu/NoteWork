git

分布式的版本管理系统

`git init`可以把当前目录初始化为一个git仓库，当初始化完成之后，在目录下就有一个叫`.git`的隐藏目录，这个目录咱们一般不会去多操作它，这个目录一旦删除，那就相当于当前目录就是一个普通的目录，而不再是一个git仓库。

 

初次运行git的时候可能需要配置全局的用户名和邮箱：

```sh
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

只要全局配置了之后，在你的用户根目录下，就有一个`.gitconfig`的文件，直接修改这个文件和命令行修改，结果一样。



[git的语义化commit](<https://seesparkbox.com/foundry/semantic_commit_messages>)



"密钥对" 保存位置
用户目录/.ssh/

`$ ssh-keygen` 只需要通过这个命令回答问题的方式，就可以生成密钥对。 读法： ssh-key   gen => generator
  别看其它的方法了，这个方法最简单。

Generating public/private rsa key pair.
Enter file in which to save the key (/Users/Leo/.ssh/id_rsa):  这里一定是写带路径的名字/Users/Leo/.ssh/id_rsa_coding
在ssh目录下就会出现这样的
id_rsa_coding     id_rsa_coding.pub 公钥，用于放在coding上的

可以添加公钥到git平台

新建一个  (这里没有点)config(这里没有扩展名)  文件， 这个文件关联服务器和密钥对

Host git.dev.tencent.com（这里的地址是你的git地址）
User Leo （这里的名字随便写，别把我的名字也拷贝过去，虽然不犯法）
PreferredAuthentications publickey （注意一个字母都不能少，而且区分大小写）
IdentityFile ~/.ssh/id_rsa_coding (这个就是你要使用哪一个密钥对, windows上也是全路径)





git init: 初始化一个git仓库，如果做错了，显示隐藏文件，删除.git目录
git status: 查看状态

git add 文件名: 添加某个文件
git add .(-A): 添加所有修改
git commit -m '写你的消息'


git log 查看提交历史  按字母 q（uit）退出
git diff(erence) 可以查看没add的不同

git remote add origin (git@git.dev.tencent.com:leochow/renzaoge.git 仓库地址)

git clone: 克隆远程仓库到本地

git branch 查看本地分支

git branch -r 查看远程分支

git branch -D 分支名 删除某个分支

git checkout 分支名： 切换到已有的分支

git checkout -b 分支名: 新建一个分支，并且切换到该分支

git push origin 远程分支名: 如果远程已经存在同名分支，则会有冲突或者合并，如果没有，就会创建一个远程分支，并且和当前分支是关联的

pull request(pr), 新建 合并请求



rebase 流程

pre-1: 在基准分支（dev）上接取最新的代码 git pull origin dev
pre-2: 切换到自己的分支上 git checkout Leo/home

1. git rebase dev
2. 解决冲突
3. git add -A
4. git rebase --continue
5. 重复2，3，4
6. 至到reabase完成

7. 有可能本地自己的分支和远程分支还有冲突，这时候需要git pull ……  之后，解决冲突再push