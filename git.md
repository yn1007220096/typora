[toc]

# 命令

| 命令                        | 说明                                                         |
| --------------------------- | ------------------------------------------------------------ |
| git reset --soft HEAD~N     | 回滚 commit  N为1 2 3.,,   代表回滚几次commit（代码push不上去，可能是因为git 用户名 邮箱格式不对，即使修改还是不行。因为之前commit的代码用的是老的用户名 邮箱，需要撤回再重新提交） |
| git remote update origin -p | git 更新远程分支列表 （在本地找不到远程新加分支时用）        |
| git log                     | 查看之前提交的历史                                           |
| git config user.name        | 查看当前用户名                                              |
| git config user.email      | 查看邮箱                                            |
| git config --global user.name  xxx      | 修改邮箱用户名                                          |
| git config --global user.email xxx     | 修改邮箱                                            |
|  git push --set-upstream origin xxx     | 新建分支需要用，在远程建立新分支。                                          |


