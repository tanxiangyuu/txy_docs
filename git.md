# GIT

- `git branch`:

  git branch -f main HEAD~3: 可以直接使用 `-f` 选项让分支指向另一个提交

- `git rebase `: 合并分支的方法是 `git rebase`。Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。Rebase 的优势就是可以创造更线性的提交历史。

- `HEAD` : HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录。通常情况下是指向分支名的（如 bugFix， 但也可以进行分离。分离的 HEAD 就是让其指向了某个具体的提交记录而不是分支名。

  <img src="C:\Users\河马\AppData\Roaming\Typora\typora-user-images\image-20230314204656602.png" alt="image-20230314204656602" style="zoom: 50%;" />

- 相对引用：

  - 使用 `^` 向上移动 1 个提交记录 , ^2是选择分支的方向。
  - 使用 `~<num>` 向上移动多个提交记录，如 `~3`

### 撤销变更

- `git reset`: 通过把分支记录回退几个提交记录来实现撤销改动。你可以将这想象成“改写历史”。`git reset` 向上移动分支，原来指向的提交记录就跟从来没有提交过一样。但是这种方法对远程分支无效。
- `git revert`:  撤销更改并**分享**给别人，

### 整理提交记录

-  `git cherry-pick`: 将一些提交复制到当前所在的位置（`HEAD`）下面的话， Cherry-pick 是最直接的方式了
- ` git rebase -i`: 交互式的 rebase, 如果你不清楚你想要的提交记录的哈希值, 用交互式的rebase

### 永远指向

- `git tag`: 可以（在某种程度上 —— 因为标签可以被删除后重新在另外一个位置创建同名的标签）永久地将某个特定的提交命名为里程碑，然后就可以像分支一样引用了。更难得的是，它们并不会随着新的提交而移动。你也不能切换到某个标签上面进行修改提交，它就像是提交树上的一个锚点，标识了某个特定的位置。

### 远程操作

- `git clone`：在真实的环境下的作用是在**本地**创建一个远程仓库的拷贝（比如从 github.com）。

Git 远程仓库相当的操作实际可以归纳为两点：向远程仓库传输数据以及从远程仓库获取数据。

- `git fetch`: 从远程仓库下载本地仓库中缺失的提交记录; 更新远程分支指针(如 `o/main`) 实际上将本地仓库中的远程分支更新成了远程仓库相应分支最新的状态。
-  `git pull`: 
- `git push`: 

## github使用过程

github联系本地文件时，url需要加上token，token保存在本地文件中。

1. 报错：push过程出现：[rejected] master -> master (fetch first)(non-fast forward)

   解决：原因是没有同步远程的master，所以我们需要先同步一下。

   ```
     git pull origin master
   ```

   也可以git push -f origin master强制提交。

2.  报错：git commit 过程中Changes not staged for commit:

   需要先git add 后在commit 然后 push

3. 



