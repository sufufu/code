### git 分支

+ master 分支

- 在远程仓库创立的时候会默认创建一个 master 分支，这个分支叫做主干；主干分支上保存的是线上运行的代码，这些代码都是经过测试没有问题的代码

- 基于 master 的开发分支：真实项目中，我们一般不是在 master 分支上开发。而是基于 master 新开一个开发分支，每个分支都有自己的版本库，记录发生在当前分支上的变更

我们新开的分支，一般都是基于 master 开分支，新开的分支相当于在开分支的那一刻的 master 的快照（master 里的代码啥样，分支里面的代码就啥样）接下来所有的开发都是在分支上完成的；卸载分支上的东西如果不合并到 master，master 上是不会有的

+ 合并分支

- 当开发完成后，要把分支合并到 master；因为线上跑的都是 master 的代码，而分支里的代码，master 中没有；但是在分支合并到 master 之前，需要先把 master 上的代码同步到分支上，再把分支上的代码合并到 master

+ 创建一个本地分支（创建分支前同步 master 的代码）

- git checkout -b 分支名（创建分支并且切换到新分支）
- git branch 分支名（新建分支，但是不会切换到分支）

+ git branch -v 查看当前所有的分支，当前带 * 表示单签所处的分支

+ 切换分支

- git checkout 分支名 （从当前分支切换到某个分支）
- git checkout master 切换到 master

+ 删除分支
- git branch -D 分支名
- git merge 分支名  【把指定分支合并到当前分支】

+ 合并分支
- git mege 分支名【把指定的分支合并到当前分支】

+ 分支开发流程
1. 克隆远程仓库到本地：git clone 仓库地址
2. 在远程新建一个分支，例如 feature_0715
3. 在本地仓库新建一个和远程分支同名的分支，git checkout -b feature_0715
4. 同步本地分支和远程分支：git pull origin feature_0715(分支名)
5. 在本地开发（在目录中新建、修改文件）
6.  适时的 add commit，并且要 push 到远程分支；git push origin feature_0715(分支名) 【如果多人协作开发，在 push 到远程分支之前，先 pull 远程分支】
在 pull 分支的时候有可能会冲突，多人修改了同一个文件，就会冲突；冲突后就要解决冲突，谁发现冲突谁解决；所谓解决冲突，就是确定哪些代码要，哪些不要；解决冲突后，需要 commit 到历史区，然后再 push 到远程分支；
7. 重复第6步，直到功能开发完成

+ 分支提测（开发完成后把项目交给QA同事去测试）
1. 开发结束后我们都是用分支提测
2. 一般qa会要求咱们同步master的代码（合一下master提测），就是把master的代码合并到分支上
2.1 本地分支切换到 master
2.2 pull master，使本地的 master 里面的代码和远程 master 同步
2.3 切换到开发分支 feature_0715
2.4 执行合并：git merge master --no-ff 【是把 master 合并到 feature_0715】
2.5 合并 master 以后如果有冲突，需要解决冲突（解决冲突的方式和之前是一样的）；解决完冲突再 commit 然后 push 到远程分支；
2.6 接着就用开发分支提测（如果没有提测文档，需要把分支名发给 QA 的同事）
2.7 在测试的过程中发现了 bug，咱们就在原来的开发分支 feature_0715 上改，然后 add,commit 再 push 到远程分支上；

注意：（所有的 push 之前都要 pull）

```javascript
// 上线（发版）：
// 1. 上线之前还需要再次同步 master 的代码到分支（再次把 master 的代码合并到分支）；
// 2. 把分支合并到 master；但是一般情况下，都是提交 merge request（MR），github 上叫做 pull request；
// 3. 找有权限合代码的人，帮你合并 MR
// 4. 合完代码去找上线的人上线（一般都是运维的同事负责上线）

// 多人协作：
// 1. 如果是 github，把成员添加到项目中 settings -> Collaborators -> 用 github 名，然后再邀请，被邀请的人同意后才会加入到项目中
// 2. gitlab 你的老大会给你开账号，并且把你加到项目中
```

### ssh-key

+ 项目中使用 git 不是每次都需要输入密码，输入密码效率低；而是使用一个 ssh-key 的东西；
+ ssh-key 是建立 ssh 连接时需要的公钥；这个公钥存储在你的机器（电脑）上，通过命令行生成的，生成以后要把这个公钥放到 github 或者 gitlab 上；
+ 等下一次建立 ssh 连接时（pull 和 push），会自动从你的机器上读取这个秘钥，然后带着公钥一起去 github 或者 gitlab 的服务器上，github 或者 gitlab 服务器会比对你带来的秘钥和之前放在 github 上或者 gitlab 上的是否一样；如果一样，就不需要再输入密码了直接就可以进行操作了；

```javascript
// 使用 ssh-key 的步骤：

// 1. 生成 ssh-key
// 1.1 进入家目录，在 git bash 中输入：cd  ~
// 1.2 进入到 .ssh：cd .ssh/ ，如果没有 .ssh ，需要新建 .ssh 文件夹:mkdir .ssh ，然后再 cd .ssh/
// 1.3 生成 ssh-key:  ssh-keygen 一路回车
// 1.4 cat id_rsa.pub 把输出的内容复制
// 1.5 把 ssh-key 添加到 github 或者 gitlab
// 1.6 后面再 clone 项目时改用 SSH 协议，以后所有的操作都不需要密码；
```