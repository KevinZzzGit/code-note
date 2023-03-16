# Git 的基础使用

[官方文档](https://git-scm.com/book/zh)

[高阶原理](https://www.open-open.com/lib/view/open1328070620202.html)

![img](https://img-blog.csdn.net/20140417113336421)

## 安装与环境配置

- 下载

  去官网下载器下载吧！！



## 查看与修改配置信息

- 查看版本

  ```bash
   git --version
  ```

- 查看配置信息

  ```bash
  // 查看当前配置信息
  git config --list
  
  // 查看全局配置信息
  git config --global --list
  ```

- 添加配置信息

  ```bash
  // 查看当前配置信息
  git config --list
  
  // 查看全局配置信息
  git config --global --list
  ```

- 删除配置信息

  ```bash
  // 删除当前配置信息
  git config --unset 'keyName'
  
  // 删除全局配置信息
  git config --global --unset 'keyName'
  ```

- 编辑配置文件

  ```bash
  // 编辑当前配置信息
  git config --edit
  
  // 编辑全局配置信息
  git config --global --edit
  ```

- 获取帮助

  ```bash
  // 全部帮助
  git help
  
  // 简要帮助 -h
  git help -h
  ```



## 绑定远程仓库

1. 在``github`` ``gitee`` ``gitlab`` 上创建仓库，再``clone``到本地。

2. 在本地项目初始化仓库并绑定到远程仓库。

   1. 创建远程空仓库

   2. 初始化本地仓库

      ```bash
      git init
      ```

   3. 绑定远程仓库

      ```bash
      // shortName 别名，一般会用 origin
      git remote add [shortName] '远程仓库地址'
      ```

   4. 拉取远程仓库代码

      - ``git fetch`` + ``git merge``(推荐)

        ```bash
        git fetch origin '远程分支名'
        git merge 'origin/远程分支名'
        
        // checkout 创建新的分支并绑定到远程分支上
        git checkout -b 'newBranch' 'origin/远程分支名'
        ```

      - ``git pull``



## 提交commit

- 添加到暂存区

```bash
// 空文件夹不会提交
// 提交路径下的文件
git add path
// 提交全部修改的文件 
git add .
```

- 取消添加

```bash
// 取消添加
git reset HEAD
```

- 查看当前文件状态

``` bash
git status
// ?? 新添加的未跟踪文件
// A 新添加到暂存区的
// M 已修改并暂存并暂存
// MM 暂存后又修改了
```

- 提交到本地仓库
```bash
git commit -m “提交描述信息”
```

- 查看操作日志
```bash
git log
```



## 推送

- 绑定远程仓库 并同步(https)

  ```bash
  // 绑定本地库和远程仓库https
  git push -u origin(别名、或仓库地址) master(分支名称)
  ```

- 绑定远程仓库 并同步(ssh)

  ```bash
  // 绑定本地库和远程仓库SSH
  // 创建密钥，并存放在C盘指定文件目录.ssh下。
  ssh-keygen -t rsa -C "邮箱地址"
  // 拷贝公钥，并在git线上仓库的setting中添加公钥
  ```



## Git版本控制

- 回跳一个版本

  ```bash
  // 多少个 ^ 往前回跳多少个版本
  git reset --hard HEAD^
  ```

- 回跳多个版本

  ```bash
  // 版本回跳3个版本
  git reset --hard HEAD~3
  ```

- 借助``reflog`` 或者``log``回跳到指定版本

  ```bash
  git reflog
  git reset --hard "版本的唯一标识符"
  ```



## Git指令

### git文件的三种状态和三种仓库

| 状态             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| 已提交(commited) | 已提交表示数据已经安全的保存在本地数据库中。                 |
| 已修改(modified) | 已修改表示修改了文件，但还未保存在数据库。                   |
| 已暂存(staged)   | 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。 |

### Git分支操作

| 命令                                   | 描述                     |
| -------------------------------------- | ------------------------ |
| git checkout branch                    | 切换到指定分支           |
| git checkout -b new_branch             | 新建分支并切换到新建分支 |
| git branch -d branch                   | 删除指定分支             |
| git branch                             | 查看所有分支             |
| git merge branch                       | 合并分支                 |
| git branch -m \|-M oldbranch newbranch | 重命名分支               |

### Push和Pull操作

| 命令                                             | 描述                             |
| ------------------------------------------------ | -------------------------------- |
| git branch -a                                    | 查看本地与远程分支               |
| git push origin branch_name                      | 推送本地分支到远程               |
| git push origin :remote_branch                   | 删除远程分支(本地分支还保留)     |
| git checkout -b local_branch orgin/remote_branch | 拉取远程指定分支并在本地创建分支 |
| git fetch                                        | 读取远程仓库最新信息             |

### 配置忽略文件

```bash
// 创建.gitignore 文件
cat .gitignore
```



