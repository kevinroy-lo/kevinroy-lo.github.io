---
title: 重学Git
copyright: true
rating: true
related_posts: false
tags:
categories:
---

# 序

公司一直用 SVN，但是平常自己看第三方源码的，或者自己博客都会用到 Git，有时候经常遇到一些问题。比如现在的博客，由于这个版本的 hexo 已经不维护了，但是我又不想切换到最新版本的 hexo(自定义配置什么的都需要重新改，比较麻烦)，于是升级到 hexo 老版本的最新版，但是由于很久没维护，遇到了一些 Aweshme Icon 的问题，找了很久的源码也没有解决，但是重新拉了一下老版本，又发现是好的。于是遇到了一些 merge，版本回退啥的，索性就好好复习一下 Git 的用法吧~

<!-- more -->

## 常用流程

### 版本控制

#### 工作区和暂存区之间

```bash
git checkout -- readme.txt
```

{% note danger%}
意思就是，把 readme.txt 文件在工作区的修改全部撤销，这里有两种情况：
一种是 readme.txt 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是 readme.txt 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

注意要加-- 。
git-checkout - Switch branches or restore working tree files
git checkout [-q][-f] [-m][<branch>]

{%endnote%}

#### 暂存区和版本库之间

```bash
git reset HEAD <file>

```

把暂存区的内容回退到版本库中的内容<舍弃暂存区的修改，回退到 HEAD 的内容>。

{% note danger%}
// 注意和版本回退之间的区别，有无--hard 选项。
git reset --hard HEAD^
{% endnote %}

### add 暂存区

文件保存成二进制对象以后，还需要通知 Git 哪些文件发生了变动。所有变动的文件，Git 都记录在一个区域，叫做"暂存区"（英文叫做 index 或者 stage）。等到变动告一段落，再统一把暂存区里面的文件写入正式的版本历史。

### commit 的概念

暂存区保留本次变动的文件信息，等到修改了差不多了，就要把这些信息写入历史，这就相当于生成了当前项目的一个快照（snapshot）。项目的历史就是由不同时点的快照构成。Git 可以将项目恢复到任意一个快照。快照在 Git 里面有一个专门名词，叫做 commit，生成快照又称为完成一次提交。

所谓快照，就是保存当前的目录结构，以及每个文件对应的二进制对象。上一个操作，目录结构已经保存好了，现在需要将这个目录结构与一些元数据一起写入版本历史。

保存进暂存区以后，只要 git commit 一个命令，就同时提交目录结构和说明，生成快照。

git checkout 命令用于切换到某个快照。

git checkout c9053865e9dff393fd2f7a92a18f9bd7f2caa7fa
git show 命令用于展示某个快照的所有代码变动。

### branch 的概念

git log 命令只显示当前分支的变动，虽然我们前面已经提交了快照，但是还没有记录这个快照属于哪个分支。
所谓分支（branch）就是指向某个快照的指针，分支名就是指针名。哈希值是无法记忆的，分支使得用户可以为快照起别名。而且，分支会自动更新，如果当前分支有**新的快照，指针就会自动指向它**。比如，master 分支就是有一个叫做 master 指针，它指向的快照就是 master 分支的当前快照。

Git 有一个特殊指针 HEAD， 总是指向当前分支的最近一次快照。另外，Git 还提供简写方式，HEAD^指向 HEAD 的前一个快照（父节点），HEAD~6 则是 HEAD 之前的第 6 个快照。

每一个分支指针都是一个文本文件，保存在.git/refs/heads/目录，该文件的内容就是它所指向的快照的二进制对象名（哈希值）。

git log 的运行过程是这样的：

1. 查找 HEAD 指针对应的分支，本例是 master
2. 找到 master 指针指向的快照，本例是 785f188674ef3c6ddc5b516307884e1d551f53ca
3. 找到父节点（前一个快照）c9053865e9dff393fd2f7a92a18f9bd7f2caa7fa
4. 以此类推，显示当前分支的所有快照

前面说过，分支指针是动态的。原因在于，下面三个命令会自动改写分支指针。

1. git commit：当前分支指针移向新创建的快照。
2. git pull：当前分支与远程分支合并后，指针指向新创建的快照。
3. git reset [commit_sha]：当前分支指针重置为指定快照。

```bash
git init // 初始化一个Git项目

// 修改
git add // 添加版本管理的文件，修改的文件将保存在暂存区(添加的也是)


场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

git restore --staged <file> // 撤销提交到暂存区的内容

git commit <file> // 将数据提交到历史中(打包目录树，数据等)，并将HEAD指针指向新的目录
git reset --hard HEAD^ // 将HEAD指针回退到上一个节点(父节点)，HEAD~5，表示回退到父5个节点
git reset --hard 203a  // 也可以指定commit id 的前几位。

```

### Git init

```bash
usage: git init [-q | --quiet] [--bare] [--template=<template-directory>] [--shared[=<permissions>]] [<directory>]

    --template <template-directory> :  directory from which templates will be used
    --bare                create a bare repository
    --shared[=<permissions>]
                          specify that the git repository is to be shared amongst several users
    -q, --quiet           be quiet
    --separate-git-dir <gitdir>
                          separate git dir from working tree
```

### Git status

相当于

```bash
git ls-files --stage
100644 3b18e512dba79e4c8300dd08aeb37f8e728b8dad 0   test.txt
```

上面代码表示，暂存区现在只有一个文件 test.txt，以及它的二进制对象名和权限。知道了二进制对象名，就可以在.git/objects 子目录里面读出这个文件的内容。

```bash

usage: git status [<options>][--] <pathspec>...
-v, --verbose be verbose
-s, --short show status concisely // 简约的形式展示（没有多余提示）
-b, --branch show branch information //分支信息
--show-stash show stash information
--ahead-behind compute full ahead/behind values
--porcelain[=<version>]
machine-readable output
--long show status in long format (default)
-z, --null terminate entries with NUL
-u, --untracked-files[=<mode>]
show untracked files, optional modes: all, normal, no. (Default: all)
--ignored[=<mode>] show ignored files, optional modes: traditional, matching, no. (Default: traditional)
--ignore-submodules[=<when>]
ignore changes to submodules, optional when: all, dirty, untracked. (Default: all)
--column[=<style>] list untracked files in columns
--no-renames do not detect renames
-M, --find-renames[=<n>]
detect renames, optionally set similarity index
--show-ignored-directory
(DEPRECATED: use --ignore=matching instead) Only show directories that match an ignore pattern name.
--no-lock-index (DEPRECATED: use `git --no-optional-locks status` instead) Do not lock the index

```

### Git add

{% note info%}
相当于 git hash-object 命令，把 test.txt 的当前内容压缩成二进制文件，存入 Git。压缩后的二进制文件，称为一个 Git 对象，保存在.git/objects 目录。这个命令还会计算当前内容的 SHA1 哈希值（长度 40 的字符串），作为该对象的文件名。子目录，目录名是哈希值的前 2 个字符，
{% endnote %}

```bash
usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run         dry run
    -v, --verbose         be verbose

    -i, --interactive     interactive picking
    -p, --patch[=<patch-mode>]
                          select hunks interactively
    -e, --edit            edit current diff and apply
    -f, --force           allow adding otherwise ignored files
    -u, --update          update tracked files
    --renormalize         renormalize EOL of tracked files (implies -u)
    -N, --intent-to-add   record only the fact that the path will be added later
    -A, --all             add changes from all tracked and untracked files
    --ignore-removal      ignore paths removed in the working tree (same as --no-all)
    --refresh             don't add, only refresh the index
    --ignore-errors       just skip files which cannot be added because of errors
    --ignore-missing      check if - even missing - files are ignored in dry run
    --chmod (+|-)x        override the executable bit of the listed files
```

### Git diff (diff --git)

{% cq %}
Show changes between commits, commit and working tree, et

git diff 比较的是工作目录中**当前文件和暂存区域快照**之间的差异， 也就是修改之后还没有暂存起来的变化内容；
git diff --cached 比较的是**已暂存的将要添加到下次提交里的内容**和已经提交的内容之间的差异。
{% endcq%}

```bash
Usage: diff [OPTION]... FILES
Compare FILES line by line.

Mandatory arguments to long options are mandatory for short options too.
      --normal                  output a normal diff (the default)
  -q, --brief                   report only when files differ
  -s, --report-identical-files  report when two files are the same
  -c, -C NUM, --context[=NUM]   output NUM (default 3) lines of copied context
  -u, -U NUM, --unified[=NUM]   output NUM (default 3) lines of unified context
  -e, --ed                      output an ed script
  -n, --rcs                     output an RCS format diff
  -y, --side-by-side            output in two columns
  -W, --width=NUM               output at most NUM (default 130) print columns
      --left-column             output only the left column of common lines
      --suppress-common-lines   do not output common lines

  -p, --show-c-function         show which C function each change is in
  -F, --show-function-line=RE   show the most recent line matching RE
      --label LABEL             use LABEL instead of file name and timestamp
                                  (can be repeated)

  -t, --expand-tabs             expand tabs to spaces in output
  -T, --initial-tab             make tabs line up by prepending a tab
      --tabsize=NUM             tab stops every NUM (default 8) print columns
      --suppress-blank-empty    suppress space or tab before empty output lines
  -l, --paginate                pass output through 'pr' to paginate it

  -r, --recursive                 recursively compare any subdirectories found
      --no-dereference            don't follow symbolic links
  -N, --new-file                  treat absent files as empty
      --unidirectional-new-file   treat absent first files as empty
      --ignore-file-name-case     ignore case when comparing file names
      --no-ignore-file-name-case  consider case when comparing file names
  -x, --exclude=PAT               exclude files that match PAT
  -X, --exclude-from=FILE         exclude files that match any pattern in FILE
  -S, --starting-file=FILE        start with FILE when comparing directories
      --from-file=FILE1           compare FILE1 to all operands;
                                    FILE1 can be a directory
      --to-file=FILE2             compare all operands to FILE2;
                                    FILE2 can be a directory

  -i, --ignore-case               ignore case differences in file contents
  -E, --ignore-tab-expansion      ignore changes due to tab expansion
  -Z, --ignore-trailing-space     ignore white space at line end
  -b, --ignore-space-change       ignore changes in the amount of white space
  -w, --ignore-all-space          ignore all white space
  -B, --ignore-blank-lines        ignore changes where lines are all blank
  -I, --ignore-matching-lines=RE  ignore changes where all lines match RE

  -a, --text                      treat all files as text
      --strip-trailing-cr         strip trailing carriage return on input
      --binary                    read and write data in binary mode

  -D, --ifdef=NAME                output merged file with '#ifdef NAME' diffs
      --GTYPE-group-format=GFMT   format GTYPE input groups with GFMT
      --line-format=LFMT          format all input lines with LFMT
      --LTYPE-line-format=LFMT    format LTYPE input lines with LFMT
    These format options provide fine-grained control over the output
      of diff, generalizing -D/--ifdef.
    LTYPE is 'old', 'new', or 'unchanged'.  GTYPE is LTYPE or 'changed'.
    GFMT (only) may contain:
      %<  lines from FILE1
      %>  lines from FILE2
      %=  lines common to FILE1 and FILE2
      %[-][WIDTH][.[PREC]]{doxX}LETTER  printf-style spec for LETTER
        LETTERs are as follows for new group, lower case for old group:
          F  first line number
          L  last line number
          N  number of lines = L-F+1
          E  F-1
          M  L+1
      %(A=B?T:E)  if A equals B then T else E
    LFMT (only) may contain:
      %L  contents of line
      %l  contents of line, excluding any trailing newline
      %[-][WIDTH][.[PREC]]{doxX}n  printf-style spec for input line number
    Both GFMT and LFMT may contain:
      %%  %
      %c'C'  the single character C
      %c'\OOO'  the character with octal code OOO
      C    the character C (other characters represent themselves)

  -d, --minimal            try hard to find a smaller set of changes
      --horizon-lines=NUM  keep NUM lines of the common prefix and suffix
      --speed-large-files  assume large files and many scattered small changes
      --color[=WHEN]       colorize the output; WHEN can be 'never', 'always',
                             or 'auto' (the default)
      --palette=PALETTE    the colors to use when --color is active; PALETTE is
                             a colon-separated list of terminfo capabilities

      --help               display this help and exit
  -v, --version            output version information and exit

```

### git reset

{% cq%}
git-checkout - Switch branches or restore working tree files

git reset 命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用 HEAD 时，表示最新的版本。
{% endcq%}

```bash
git checkout [-q] [-f] [-m] [<branch>]
git checkout [-q] [-f] [-m] --detach [<branch>]
git checkout [-q] [-f] [-m] [--detach] <commit>
git checkout [-q] [-f] [-m] [[-b|-B|--orphan] <new_branch>] [<start_point>]
git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] [--] <paths>…​
git checkout [<tree-ish>] [--] <pathspec>…​
git checkout (-p|--patch) [<tree-ish>] [--] [<paths>…​]
```

## Git 远程库

要关联一个远程库，使用命令 git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令 git push -u origin master 第一次推送 master 分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令 git push origin master 推送最新修改；

## 分支管理

```bash
// 创建分支
git branch dev
// 切换分支
git checkout dev == git switch master
以上两条可以简写为：
 git checkout -b dev == git switch -c dev (checkout由于和之间的用法迷惑)

// 合并dev到当前分支
git merge dev
// 删除分支
git branch -d dev

```

<div class="reference-linking">参考链接</div>

-[Git 原理入门](http://www.ruanyifeng.com/blog/2018/10/git-internals.html) -[廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600)
