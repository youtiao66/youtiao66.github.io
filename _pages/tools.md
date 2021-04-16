## Git

### git log
**以图表和单行的形式展示所有提交日志** `git log --graph --oneline --all`

### git reset
#### 将 指定分支`[branchName]` 所指向的 commit id 设置为当前 HEAD 所指向的 commit id
```
git checkout [branchName]
git reset --hard [<commit>]
```

### git rebase
#### 合并多个commit为一个完整commit
`git rebase -i  [startpoint]  [endpoint]`

- `-i`即`--interactive`，即弹出交互式的界面让用户编辑完成合并操作
- `[startpoint] [endpoint]`则指定了一个编辑区间，如果不指定`[endpoint]`，则该区间的终点默认是当前分支`HEAD`所指向的commit（注：该区间指定的是一个前开后闭的区间）

#### 将某一部分 commit 复制到另一个分支
`git rebase [startpoint] [endpoint] --onto [branchName]`

### git update-index
**假定指定的文件为未更改的状态** `git update-index [--[no-]assume-unchanged] [--] [<file>...]`

**查看被假定为未更改状态的文件** `git ls-files -v`

### 统计
**指定作者的提交次数** `git log --author=username --oneline | wc -l`

**指定作者的代码量**
```
  git log --author=username --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }'
```

## VIM
**全局替换** `%s/[old]/[new]/g`

## pandoc
### 将`markdown`转换成`docx`
`pandoc -f markdown -t docx ./test.md -o test.docx`

## Chrome
### Chrome Cookies
```
  chrome://flags/
  => SameSite by default cookies
  => Cookies without SameSite must be secure
```

### ServiceWorker
如何停用 Service Worker 缓存

```
  chrome://serviceworker-internals/
  => Open DevTools window and pause JavaScript execution on Service Worker startup for debugging.
  => 选择需要取消缓存对应的网址，点击 unregister 取消缓存
```
