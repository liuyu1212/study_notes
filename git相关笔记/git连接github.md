#Permission denied (publickey). fatal: Could not read from remote repository
===================
1.在执行git clone git@github.com:test/git
	当在git bash命令窗口执行完这条命令后，出现错误：
	Permissiondenied (publickey).

　　fatal:Could not read from remote repository.

　　Pleasemake sure you have the correct access rights

　　and the repository exists.

出现这个错误是由于本第（或服务器）上没有生成ssh key，可以执行 cd ~/.ssh ls来查看是否有文件id_rsa以及文件id_rsa.pub，如下图所示。

2.解决办法：
	1》如果没有ssh key 的话，在git bash 下输入命令：ssh-keygen -t rsa -C "yy@example.com"， yy@example.com改为自己的邮箱即可，途中会让你输入密码啥的，不需要管，一路回车即可，会生成你的ssh key。（如果重新生成的话会覆盖之前的ssh key。）
如下：
+---[RSA 2048]----+
|      . . .=+++oO|
|     . . .. o+O.O|
|      o      o Oo|
|     . o      =.o|
|      . S    +.oE|
|            . .=.|
|           ...*++|
|          . .=Bo |
|           . .oBo|
+----[SHA256]-----+

	2》然后在git bash 下执行命令：
	ssh -v git@github.com
	在最后会出现：
	No more authentication methods to try.  

　　Permission denied (publickey).
	3》在执行：
	ssh-agent -s
	然后会提示：
	SSH_AUTH_SOCK=/tmp/ssh-n1Y7wvwhA1Y1/agent.5444; export SSH_AUTH_SOCK;
SSH_AGENT_PID=2072; export SSH_AGENT_PID;
echo Agent pid 2072;
	4》在执行：
	ssh-add ~/.ssh/id_rsa

　　这时候应该会提示：
　　Identity added: ...（这里是一些sshkey文件路径的信息）
　　（注意）如果出现错误提示：
　　Could not open a connection to your authentication agent.

　　请执行命令：eval `ssh-agent -s`后继续执行命令 ssh-add ~/.ssh/id_rsa，这时候一般没问题啦。
	5》打开你刚刚生成的id_rsa.pub，将里面的内容复制，进入你的github账号，在settings下，SSH and GPG keys下new SSH key，title随便取一个名字，然后将id_rsa.pub里的内容复制到Key中，完成后Add SSH Key。

　　6》最后一步，验证Key
　　在ternimal下输入命令：
　　ssh -T git@github.com

　　提示：Hi xxx! You've successfully authenticated, but GitHub does not provide shell  access.

　　这时候你的问题就解决啦，可以使用命令 git clone yy git@github.com:test.git 去下载你的代码

	7>在git上添加文件夹 ：git add +文件名称
	执行：git commit -m "first commit"
	添加备注。
	执行：git push -u origin master
	提交文件。
