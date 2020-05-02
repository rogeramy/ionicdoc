##### 从本地上传项目到 github

1. 在github创建空仓
2. git init
3. git add README.md，本地没有直接命令创建会提示错误，使用touch README.md 替代
4. git add .
5. git commit -m "first commit"
6. git remote add origin https://用户名:密码@github.com/rogeramy/mips2020.git
7. git push -u origin master

第五步，私有项目需要输入用户名和密码



