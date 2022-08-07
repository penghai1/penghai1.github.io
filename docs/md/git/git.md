

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



## IDEA社区版常用插件整理

- Alibaba Java Coding Guidelines 代码规范提醒
- CodeGlance 代码编辑区迷你缩放图，当代码行数过多时可以通过预览图快速定位
- Codota 智能补全代码，很好用
- Database Navigator 可以在idea连接mysql等主流数据库，进行查询等各种操作
- Gerp Console 控制台彩色分类显示
- Lombok 懂得都懂，必备
- Easy Javadoc 自动生成 Javadoc 风格注释
- Material Theme UI 皮肤主题插件，用了之后心情都变好了
- Maven Helper 优化Maven操作
- MybatisX 官方推荐的 Mybatis-Plus 插件，简化操作
- Smart Tomcat 社区版不提供tomcat插件，需要自己下载
- Easy Code 自动生成代码 收费版和破解版可以用一下，社区版好像没法用



## IDEA内存配置优化

