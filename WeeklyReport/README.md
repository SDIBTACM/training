# Readme
+ 周报每周六提交，每个人以“姓名拼音.md”提交文件。eg：liuxinyi.md。
+ 存放目录为本周最后一天（周六）的日期，如果提交时还没有该目录，自己建一个以便后面人提交，不要提交到主目录下。
+ 注意 [markdown](https://guides.github.com/features/mastering-markdown/) 的语法格式，提交以后 check 一下显示有没有问题。
+ 周报请按格式要求填写，要求有“本周进展”和“下周计划”。具体格式可参考 [template.md](template.md) 和 [中文文案指北](https://github.com/sparanoid/chinese-copywriting-guidelines/blob/master/README.zh-CN.md)。
+ 周赛的解题报告放到ContestReport对应的目录中。
+ 一般认为周六是每周的最后一天。
+ 每次提交前请先执行 `git pull`。



# 周报 PR 提交流程
1. 使用 `git clone` 到本地.
2. 首先使用 `git pull --rebase` 拉取最新仓库内容. (切记使用 rebase)
3. 使用 `git checkout -b 你分支的名字, 如: jk192/fyj-weekly-report` 新建一个 git 分支. 
4. 对当前分支新增内容 (你写的周报) 进行提交
5. 使用 `git push orign` 提交到本远程仓库
6. 在 github 提交 PR, 将你自己的周报分支, 如: `jk192-fyj-weekly-report` 分支创建合并进 master 的 PR, 等待合并即可.
7. 备注: 如果对 git 命令不熟悉, 可以使用 [soureTree](https://www.sourcetreeapp.com/) 进行 git 分支的管理
