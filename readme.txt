git常用命令配置用户信息
$ git config --global user.name "zhangyunkang"
$ git config --global user.email "1124773718@qq.com"
$ git init //初始化版本库
$ git add readme.txt //把文件readme.txt添加到暂存区
$ git add . //将当前目录下所有有修改文件添加到暂存区
$ git commit -m "write a readme file" //提交修改
$ git status  //查看仓库当前的状态
$ git diff readme.txt  //查看具体修改了什么内容
$ git log  //查看历史记录可以查看到比较详细的历史记录
$ git log --pretty=oneline //查看只包含commitid的历史记录
HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本HEAD~100
git reset --hard HEAD^ //把当前版本回退到上一个版本
$ git reflog //查看每一次命令
$ git checkout -- file  //丢弃工作区的修改
$ git reset HEAD file  //把暂存区的修改撤销掉（unstage），重新放回工作区
$ ssh-keygen -t rsa -C "youremail@example.com" //创建SSH Key 一路按enter默认
//将已经存在的版本库推送到github上
$ git remote add origin git@github.com:zhangyunkang/test.git 
$ git push -u origin master //第一次提交加上了-u参数Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
$ git push origin master
$ git clone git@github.com:zhangyunkang/newtest.git //从github克隆一个本地库
Git鼓励大量使用分支：
$ git checkout -b zyk //git checkout命令加上-b参数表示创建并切换相当于以下两条$ git branch zyk,$ git checkout zyk
$ git branch //查看分支
$ git merge zyk //把zyk分支的工作成果合并到master分支上,git merge命令用于合并指定分支到当前分支
$ git branch -d zyk //删除分支
$ git push origin --delete zyk //删除远程库上分支
$ git merge --no-ff -m "merge with no-ff" dev  //禁用fast forward模式合并分支请注意--no-ff参数，表示禁用Fast forward
$ git stash //把当前工作现场“储藏”起来，等以后恢复现场后继续工作
$ git stash list //查看工作现场
恢复工作现场
$ git stash apply //工作现场恢复，现场（stash）内容并不删除
$ git stash drop //删除工作现场内容
$ git stash pop  //恢复的同时把stash内容也删了
$ git branch -D feature-vulcan //强行删除分支
$ git remote  //查看远程库的信息
$ git remote -v //查看远程库的详细信息
$ git checkout -b dev origin/de //创建远程origin的dev分支到本地
$ git pull //将远程与本地对应的分支上的内容更新下来
$ git branch --set-upstream dev origin/dev //指定本地dev分支与远程origin/dev分支的链接
$ git tag <name> //打一个新标签,默认标签是打在最新提交的commit上的
$ git tag  //查看所有标签
$ git show <tagname>  //查看标签信息
$ git tag -a v0.1 -m "version 0.1 released" 3eb0627  //创建带有说明的标签，用-a指定标签名，-m指定说明文字
$ git tag -s v0.2 -m "signed version 0.2 released" b95d7f1 //通过-s用私钥签名一个标签必须首先安装gpg（GnuPG）参照：http://blog.csdn.net/sh21_/article/details/71082422
$ git tag -d v0.1   //删除标签
git push origin <tagname>   //推送某个标签到远程
$ git push origin --tags    //一次性推送全部尚未推送到远程的本地标签
删除远程标签
$ git tag -d v0.9   //先本地删除
$ git push origin :refs/tags/v0.9   //再远程删除
$ git config --global alias.lgol 'log --pretty=oneline'   //配置别名 配置 git log --pretty=oneline 命令的别名为git lgol
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"    //非常实用的别名配置用来查看暂存提交之间的关系
$git rm -rf --cached .idea  //删除追踪记录
git rm --cached --force


apacheds使用笔记
1、LDAP服务器默认监听端口10389（未加密或StartTLS）和10636（SSL）
2、ApacheDS Directory Studio登录请注意：Set Bind DN or User to the value uid=admin,ou=system and Bind password to secret.
3、修改后端口：LDAP Server port:389 LDAPS server:636
4、配置文件位置：D:\DevInstall\apacheDS\instances\default\conf\config.ldif
5、管理员账号密码为：账号：uid=admin,ou=system 密码：zykzsl 采用了MD5加密
6、导出本分文件命令：ldapsearch -D "uid=admin,ou=system" -w zykzsl -p 389 -h localhost -b "dc=example,dc=com" -s sub "(ObjectClass=*)" * + > backup.ldif
将在当前命令行所在目录生成一个backup.ldif文件
7、利用ldif文件增加条目：ldapmodify -h localhost -p 389 -D "cn=Horatio Nelson,ou=people,o=sevenSeas" -w pass -a -f captain_hook.ldif
 参数解释：
	-x 使用简单验证方式
        -D 指定管理员DN（与slapd.conf中配置的一致）
        -W 大写W表示回车后根据提示输入密码，可以使用小写的-w password 直接输入密码
        -f 需要导入数据LDIF的文件名
        -h 目录服务器的IP地址（或者别名)
8、查询命令：ldapsearch -h localhost -p 389 -D "cn=Horatio Nelson,ou=people,o=sevenSeas" -w pass -b "o=sevenSeas" -s sub "(cn=James Hook)" +
9、删除条目命令：ldapdel -h localhost -p 389 -D "cn=Horatio Nelson,ou=people,o=sevenSeas" -w pass -a -f captain_hook_delete.ldif
10、列出所有对象类: ldapsearch -h localhost -p 389 -D "uid=admin,ou=system" -w zykzsl -s base -b "cn=schema"   "objectclass=*" objectclasses
11、apacheds问题提交网站：https://issues.apache.org/jira/browse/DIRSERVER
12、利用不同身份查询管理员账号信息所得结果不同，测试请分别运行以下命令：
ldapsearch -h  localhost  -p 389 -D "uid=admin,ou=system" -w  zykzsl -b "ou=system" -s  one "(uid=admin)" dn 
ldapsearch -h  localhost  -p 389 -D "cn=Horatio Hornblower,ou=people,o=sevenSeas" -w  pass -b "ou=system" -s  one "(uid=admin)" dn 

ldapsearch -h localhost -p 389 -D "uid=lxl,ou=User,ou=zsl,dc=taotaosou.com " -w 1234@com -b "dc=taotaosou.com" -s sub "(cn=zhang yun kang)" +
ldapsearch -h localhost -p 389 -D "uid=admin,ou=system" -w zykzsl -b "dc=taotaosou.com" -s sub "(cn=zhang yun kang)" +
13、创建新的节点：Object Classes选择organisationalUnit然后命名ou=name
14、创建用户：Object Classes选择inetOrgPerson然后命名：uid=zyk
15、创建管理员：Object Classes选择account and simpleSecurityObject然后命名：uid=admin
16、创建用户组：Object Classes选择 groupOfNames（groupOfUniqueNames）然后命名：cn=admins
17、创建访问权限设置：
（1）根目录启用权限控制，添加administrativeRole属性是关键
创建administrativeRole属性，选择accessControlSpecificArea
（2）创建访问权限：Object Classes选择accessControlSubentry and subentry
	（2.1）添加匿名读权限
		dn: cn=enableAllUsersRead,dc=taotaosou.com  
		objectClass: subentry  
		objectClass: accessControlSubentry  
		objectClass: top  
		cn: enableAllUsersRead  
		prescriptiveACI: { identificationTag "enableAllUsersRead", precedence 0, aut  
		 henticationLevel none, itemOrUserFirst userFirst: { userClasses { allUsers   
		 }, userPermissions { { protectedItems { entry, allUserAttributeTypesAndValu  
		 es }, grantsAndDenials { grantCompare, grantFilterMatch, grantRead, grantRe  
		 turnDN, grantBrowse } } } } }  
		subtreeSpecification: { }  
	(2.2)添加用户自己修改资料权限
		dn: cn=allowSelfAccessAndModification,dc=taotaosou.com  
		objectClass: subentry  
		objectClass: accessControlSubentry  
		objectClass: top  
		cn: allowSelfAccessAndModification  
		prescriptiveACI: { identificationTag "allowSelfAccessAndModification", prece  
		 dence 10, authenticationLevel simple, itemOrUserFirst userFirst: { userClas  
		 ses { thisEntry }, userPermissions { { protectedItems { entry, allUserAttri  
		 buteTypesAndValues }, grantsAndDenials { grantRemove, grantExport, grantCom  
		 pare, grantImport, grantRead, grantFilterMatch, grantModify, grantInvoke, g  
		 rantDiscloseOnError, grantRename, grantReturnDN, grantBrowse, grantAdd } }   
		 } } }  
		subtreeSpecification: { }  
	(2.3)添加管理员权限
		dn: cn=enableAdminSuper,dc=taotaosou.com  
		objectClass: subentry  
		objectClass: accessControlSubentry  
		objectClass: top  
		cn: enableAdminSuper  
		prescriptiveACI: { identificationTag "enableAdminSuper", precedence 0, authe  
		 nticationLevel strong, itemOrUserFirst userFirst: { userClasses { userGroup  
		  { "cn=administrator,ou=gourp,dc=taotaosou.com" } }, userPermissions { { pr  
		 otectedItems { entry, allUserAttributeTypesAndValues }, grantsAndDenials {   
		 grantRemove, grantExport, grantCompare, grantImport, grantRead, grantFilter  
		 Match, grantModify, grantInvoke, grantDiscloseOnError, grantRename, grantRe  
		 turnDN, grantBrowse, grantAdd } } } } }  
		subtreeSpecification: { }  


		ldapsearch -h 172.16.20.6 -p 10389 -D "uid=admin,ou=system " -w zykzsl "ou=users,o=zsl,dc=jxsltz,dc=com" -s sub "(objectClass=organizationalUnit)" +
		 jndi查询参考：http://yjm199.blog.51cto.com/4408395/1553186


		 ldapsearch -h 172.16.20.6 -p 10389 -D " uid=chenpeiyue,ou=011001,ou=0110,ou=01,o=zsl,dc=jxsltz,dc=com" -w 1234@com "ou=0110,ou=01,o=zsl,dc=jxsltz,dc=com" -s sub "(uid=chenpeiyue)" +
