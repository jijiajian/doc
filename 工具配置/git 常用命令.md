

# 批量操作

- `批量删除本地分支(删掉除了含master|develop以外的分支)`
git branch | grep -v -E 'master|develop' | xargs git branch -D
- `删除含test的本地分支`
git branch | grep 'test' | xargs git branch -D
- `批量删除远程分支(删掉除了含master|develop以外的分支)`
git branch -r | grep -v -E 'master|develop' | sed 's/origin\///g' | xargs -I {} git push origin :{}
- `删除含test的远程分支`
git branch -r | grep 'test' | sed 's/origin\///g' | xargs -I {} git push origin :{}
- git branch | grep -E '^test*' ???????貌似没用