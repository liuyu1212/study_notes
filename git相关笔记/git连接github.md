#Permission denied (publickey). fatal: Could not read from remote repository
===================
1.��ִ��git clone git@github.com:test/git
	����git bash�����ִ������������󣬳��ִ���
	Permissiondenied (publickey).

����fatal:Could not read from remote repository.

����Pleasemake sure you have the correct access rights

����and the repository exists.

����������������ڱ��ڣ������������û������ssh key������ִ�� cd ~/.ssh ls���鿴�Ƿ����ļ�id_rsa�Լ��ļ�id_rsa.pub������ͼ��ʾ��

2.����취��
	1�����û��ssh key �Ļ�����git bash ���������ssh-keygen -t rsa -C "yy@example.com"�� yy@example.com��Ϊ�Լ������伴�ɣ�;�л�������������ɶ�ģ�����Ҫ�ܣ�һ·�س����ɣ����������ssh key��������������ɵĻ��Ḳ��֮ǰ��ssh key����
���£�
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

	2��Ȼ����git bash ��ִ�����
	ssh -v git@github.com
	��������֣�
	No more authentication methods to try.  

����Permission denied (publickey).
	3����ִ�У�
	ssh-agent -s
	Ȼ�����ʾ��
	SSH_AUTH_SOCK=/tmp/ssh-n1Y7wvwhA1Y1/agent.5444; export SSH_AUTH_SOCK;
SSH_AGENT_PID=2072; export SSH_AGENT_PID;
echo Agent pid 2072;
	4����ִ�У�
	ssh-add ~/.ssh/id_rsa

������ʱ��Ӧ�û���ʾ��
����Identity added: ...��������һЩsshkey�ļ�·������Ϣ��
������ע�⣩������ִ�����ʾ��
����Could not open a connection to your authentication agent.

������ִ�����eval `ssh-agent -s`�����ִ������ ssh-add ~/.ssh/id_rsa����ʱ��һ��û��������
	5������ո����ɵ�id_rsa.pub������������ݸ��ƣ��������github�˺ţ���settings�£�SSH and GPG keys��new SSH key��title���ȡһ�����֣�Ȼ��id_rsa.pub������ݸ��Ƶ�Key�У���ɺ�Add SSH Key��

����6�����һ������֤Key
������ternimal���������
����ssh -T git@github.com

������ʾ��Hi xxx! You've successfully authenticated, but GitHub does not provide shell  access.

������ʱ���������ͽ����������ʹ������ git clone yy git@github.com:test.git ȥ������Ĵ���

	7>��git������ļ��� ��git add +�ļ�����
	ִ�У�git commit -m "first commit"
	��ӱ�ע��
	ִ�У�git push -u origin master
	�ύ�ļ���
