Github 访问时出现Permission denied (public key)

原文地址: http://www.cnblogs.com/gr-nick/p/3406235.html


一. 发现问题：

　　使用　git clone　命令时出现Permission denied (public key) 。

二. 解决问题：

　　１、首先尝试重新添加以前生成的key,添加多次，仍然不起作用。

　　２、使用命令 ssh -v git@github.com测试，最后几行结果如下：　　　

　　　　debug1: Authentications that can continue: publickey
　　　　debug1: Next authentication method: publickey
　　　　debug1: Trying private key: /home/gr/.ssh/id_rsa
　　　　debug1: Trying private key: /home/gr/.ssh/id_dsa
　　　　debug1: Trying private key: /home/gr/.ssh/id_ecdsa
　　　　debug1: No more authentication methods to try.
　　　　Permission denied (publickey).

　　３、分析：尝试了３个private key,但都没有成功，最后是导致Permission denied.

　　４、查看我的密钥, ls ~/.ssh/ :

　　　　bajie  bajie.pub  known_hosts　　　　

　　5、发现我的id_rsa文件我命令为bajie,　所以根本没有使用它。同时可以使用如下命令查看密钥列表：

　　　　ssh-add -l

　　6、上面命令的密钥列表为空，所以要添加我的密钥，使用命令：

　　　　gr@grpc:~/workspace/git/home$ ssh-add ~/.ssh/bajie

　　　　Enter passphrase for /home/gr/.ssh/bajie: 
　　　　Identity added: /home/gr/.ssh/bajie (/home/gr/.ssh/bajie)

　　7、再次查看，如下，添加成功：

　　　　gr@grpc:~/workspace/git/home$ ssh-add -l

　　　　2048 63:c5:d8:6c:a0:0c:a8:9c:26:d8:f8:95:de:29:04:eb /home/gr/.ssh/bajie (RSA)

　　８、再使用ssh -v git@github.com测试连接，可以看到验证通过：

　　　　debug1: Authentications that can continue: publickey
　　　　debug1: Next authentication method: publickey
　　　　debug1: Offering RSA public key: /home/gr/.ssh/bajie
　　　　debug1: Server accepts key: pkalg ssh-rsa blen 279
　　　　debug1: Authentication succeeded (publickey).
　　　　Authenticated to github.com ([192.30.252.129]:22).

　　９、最后git clone项目成功。