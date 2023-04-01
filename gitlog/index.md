# Gitlog

#  Git Changelog

原理/思路其实很简单，通过git log指令生成的日志。筛选出提交记录，提交时间。

把这些内容提取出来，按照md格式写入发布文件中（changelog中）


具体的筛选操作需要借助 format指令取操作。



## gitlog Format

>默认git log 出来的格式并不是特别直观，很多时候想要更简便的输出更多或者更少的信息，这里列出几个git log的format。可以根据自己的需要定制。

git log命令可一接受一个--pretty选项，来确定输出的格式.

比如 ：

如果我们只想输出hash.

```bash
git log --pretty=format:"%h" 
#也可以直接使用format

#e.g.
git log --format=%B%aD

Sat, 10 Sep 2022 16:03:16 +0800
修改文章排序
Sat, 10 Sep 2022 02:23:41 +0800
修改aidraw图片
Sat, 10 Sep 2022 01:08:54 +0800
修改aidraw
Sat, 10 Sep 2022 00:43:50 +0800
添加作者链接


```

详细 命令 ：

- %H: commit hash

- %h: 缩短的commit hash
- %T: tree hash
- %t: 缩短的 tree hash
- %P: parent hashes
- %p: 缩短的 parent hashes
- %an: 作者名字
- %aN: mailmap的作者名字 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
- %ae: 作者邮箱
- %aE: 作者邮箱 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
- %ad: 日期 (--date= 制定的格式)
- %aD: 日期, RFC2822格式
- %ar: 日期, 相对格式(1 day ago)
- %at: 日期, UNIX timestamp
- %ai: 日期, ISO 8601 格式
- %cn: 提交者名字
- %cN: 提交者名字 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
- %ce: 提交者 email
- %cE: 提交者 email (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
- %cd: 提交日期 (--date= 制定的格式)
- %cD: 提交日期, RFC2822格式
- %cr: 提交日期, 相对格式(1 day ago)
- %ct: 提交日期, UNIX timestamp
- %ci: 提交日期, ISO 8601 格式
- %d: ref名称
- %e: encoding
- %s: commit信息标题
- %f: sanitized subject line, suitable for a filename
- %b: commit信息内容
- %N: commit notes
- %gD: reflog selector, e.g., refs/stash@{1}
- %gd: shortened reflog selector, e.g., stash@{1}
- %gs: reflog subject
- %Cred: 切换到红色
- %Cgreen: 切换到绿色
- %Cblue: 切换到蓝色
- %Creset: 重设颜色
- %C(...): 制定颜色, as described in color.branch.* config option
- %m: left, right or boundary mark
- %n: 换行
- %%: a raw %
- %x00: print a byte from a hex code
- %w([[,[,]]]): switch line wrapping, like the -w option of git-shortlog(1)







## 其他内容的嵌入思路

### version

可以通过项目的配置文件，配置版本号。在生成changelog脚本读取配置文件的版本号或者其他当前版本信息。



### link to change

```bash
git log --format=%B%H%aD 

504b9f726d20343a59624ebefa8b50bd7897fd1dSat, 10 Sep 2022 16:03:16 +0800
修改文章排序
eab956775923e7297300f6728f97ef2d1b9ccaa6Sat, 10 Sep 2022 02:23:41 +0800
修改aidraw图片
1fee031181cd00e6d3ef87b1f9a2f525aacecdf3Sat, 10 Sep 2022 01:08:54 +0800
修改aidraw
```

修改输出格式，获取提交的hash值，通过链接拼接，可以直接定位到修改位置。



### 其他信息

一样的思路，修改--format的思路，通过拼接不同的信息指令。

案例：https://github.com/Ellioben/generate-gitlog


