# 小书匠
小书匠编写的文章的github图床和文章

## 小书匠绑定设置

### 创建一个仓库并初始化
- Github右上角点击"+"->"New repository"

![1](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542530371232.png)
- "Repository name"->"xiaoshujiang"(自定义),"Description (optional)"->"小书匠编写的文章的github图床和文章"
- 勾选"Initialize this repository with a README",然后点击"Create repository"

![2](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542530848951.png)

### 申请一个Github token
- 点击[申请 Github token](https://github.com/settings/tokens/new)进入,输入"Token description"->"小书匠github图床"，勾选"public_repo",点击"Generate token"
![1](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542530058206.png)
- 复制生成的token
![2](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542532117577.png)
### 设置图床
- 小书匠上点击"绑定"->"githubimg"->"图床服务"->"+"
![1](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542530043611.png)
- 输入 github token相关信息

``` html
图床服务->githubimg
请输入一个自定义名称：github图床
请输入一个有效的token值：上面生成的token复制过来
仓库名称：xiaoshujiang
文件名称生成规则：/img/{{year}}{{month}}{{date}}/{{filename}}
```
![4](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542531983629.png)


### 设置数据存储
- 绑定->数据存储->github

![1](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542532276971.pn)