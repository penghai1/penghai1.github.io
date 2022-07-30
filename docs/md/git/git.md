

## 批量克隆微服务代码

```sh
@echo off
set microservice=MBDService,UnderlayVPNService
set branch=master
set jobnumber=p0019919
for /d %%i in(%microservice%) do (
    echo ======= now progress %%i ======
    git clone ssh://git@szy-codehb.......%%i.git
    cd %%i
    git fetch "origin"
    git remote add user ssh://git@szy-codehb.......%%i.git
    git checkout -b %branch% remote/origin/%branch%
    cd ..   
)
```



## 批量pull代码

```sh
@echo off
for /d %%i in(*) do if not %%i==[Filter] @cd
    %cd%\%%i && @git pull top
echo %date% %time%
```



## 代码回合

### develop回合master

前提：定成库origin  个人库user

操作：

```sh
git checkout master
git pull origin master
git push user master
git pull origin develop # 此时可能冲突
解决冲突：此时可人为掌握冲突的处理，处理完成后推到远端个人库，然再合入到顶层库
git push -f user develop
```



### develop覆盖master

```sh
git checkout master
git reset --hard origin/develop # 直接覆盖
git push -f user develop        # 推到远端个人库
```

