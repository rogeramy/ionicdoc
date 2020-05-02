##### 从本地上传项目到 github

1. 在github创建空仓
2. git init
3. git add README.md，本地没有直接命令创建会提示错误，使用touch README.md 替代
4. git add .
5. git commit -m "first commit"
6. git remote add origin https://用户名:密码@github.com/rogeramy/mips2020.git
7. git push -u origin master

第五步，私有项目需要输入用户名和密码



##### git commit 提交的时候报错

Q: husky > pre-commit hook failed (add --no-verify to bypass)

这个问题是因为当你在终端输入git commit -m "XXX",提交代码的时候,pre-commit(客户端)钩子，它会在Git键入提交信息前运行做代码风格检查。如果代码不符合相应规则，则报错，而它的检测规则就是根据.git/hooks/pre-commit文件里面的相关定义

1. 卸载husky。只要把项目的package.json文件中devDependencies节点下的husky库删掉，然后重新npm i 一次即可。或者直接在项目根目录下执行npm uninstall husky --save也可以，再次提交，自动化测试功能就屏蔽掉
2. 进入项目的.git文件夹(文件夹默认隐藏,可先设置显示或者命令ls查找),再进入hooks文件夹,删除pre-commit文件,重新git commit -m 'xxx' git push即可
3. 将git commit -m "XXX" 改为 git commit --no-verify -m "XXX"

目前使用方案2，使用git desktop提交



