# Git操作指南

标签（空格分隔）：git github

---
![git流程图][1]

`git init`:初始化git环境，文件夹生成一个.git隐藏文件;

`$ git config --global user.name "username"` ：配置用户信息

`$ git config --global user.email "email"` ：配置用户信息

`git status`：查看文件夹中文件状态

`git add` : 工作区添加到暂存区
`git add .`:上传全部文件

`git rm --cached 文件名` ：从暂存区删除

`git commit`:进入界面添加commit信息（esc+: 输入wq 退出）；
`git commit -m 输入项目信息`：直接添加信息；

//忽略一些文件的上传：
1. 新建文件：.gitignore；
2. 在文件夹内写入需要忽略上传的文件名或文件夹（/dir）


//创建分支：
`git branch 文件名`:创建分支；
`git checkout 分支文件名`：选择进入此分支；
//合并分支：
1.首先进入主干环境
2.`git merge 分支名字`


`git push` : 存入远程仓库
`git clone` : 从远程仓库克隆到本地































  [1]: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1555063574478&di=ef2872a5ac1b2e3b074b56ba95e7483e&imgtype=0&src=http%3A%2F%2Fpic002.cnblogs.com%2Fimages%2F2012%2F132622%2F2012020615581335.png