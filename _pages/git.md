## 基本概念

| 英文 | 中文 |
| --- | --- |
| `working tree` | 工作区 |
| `stage/index` | 暂存区 |
| `origin` | 远程、源 |
| `tag` | 标签 |
| `mainline` | 主线 |

## git-commit
> Record changes to the repository

向仓库提交改动

| 命令 | 描述 |
| --- | --- |
| `-n, --no-verify` | 该选项可以避免执行 `pre-commit` 和 `commit-msg` 钩子 |

## git-tag
> Create, list, delete or verify a tag object signed with GPG

创建，列举或者删除标签

| 命令 | 描述 |
| --- | --- |
| `git tag -a [tagname] -m [msg]` | 创建标签并添加信息 |
| `git tag -l` | 列表本地标签 |
| `git tag -d [tagname]` | 删除本地标签 |
| `git push --tags` | 向远程推送所有本地标签 |
| `git push origin :refs/tags/[tagname]` | 删除远程标签 |

## git-clean

> Remove untracked files from the working tree

从工作区中删除未跟踪的文件

`git clean [-d] [-f]`

### -d

递归地删除目录和目录下包含的所有目录和文件

### -f, --force

如果不添加该选项，`git clean`会没有删除文件和目录的权限



## git-revert
> Revert some existing commits

撤销已经存在的提交

**通过创建新的提交，去撤销已经存在的提交**

| 命令 | 描述 |
| --- | --- |
| `git revert [commit]` | 撤销一个已经存在的提交 |
| `-n, --no-commit` | 不创建新的提交，直接把撤销后的代码添加到工作区和暂存区 |

### `git revert [start-commit]..[end-commit]`

撤销指定区间的提交，**前开后闭，不包括`[start-commit]`，包括`[end-commit]`**

### `git revert -m 1 [merge-commit]`

`--mainline parent-number`

通过指定主线撤销合并，`parent-number` 以 `1` 开始，通过`git show [merge-commit]`查看
