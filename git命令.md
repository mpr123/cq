### git推送

…or create a new repository on the command line
echo "# a" >> README.md


…or push an existing repository from the command line

```shell
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/mpr123/cq.git
git push -u origin main
```


​                
…or push an existing repository from the command line

```shell
git remote add origin https://github.com/mpr123/a.git
git branch -M main
git push -u origin main
```

### xxl-job

从gitee上clone项目：

```shell
git clone https://gitee.com/xuxueli0323/xxl-job.git
```

查看仓库的分支：

切换到2.2.0正式版分支：
git checkout origin/2.2.0

```shell
git branch -a
```

切换到2.2.0正式版分支：

```shell
git checkout origin/2.2.0
```