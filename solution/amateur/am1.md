# am1 -- 在本地使用git连接多个github账号

### 前言

    应用场景 => 本地git服务连接多个github账户，同时与多个github仓库进行连接

### 步骤

    本地git账户连接两个账户,分别为 one@qq.com 和 two@qq.com . 此处git安装省略，自行百度

- step1 --- 生成对应的SSH key

        code: $ ssh-keygen -t rsa -C "one@qq.com"
        code: $ ssh-keygen -t rsa -C "two@qq.com"

    注意不要一路回车，需要对生成的文件夹重命名，如id_rsa_one 和 id_rsa_two,这样会生成四个文件，分别为id_rsa_one、id_rsa_one.pub、id_rsa_two、id_rsa_two.pub(公钥和私钥)



    备注 => code为linux命令  //  在~/.ssh路径下git bash或者运行命令行，生成文件于该目录。关于.ssh文件夹的生成，自行百度。

- step2 --- 添加私钥

    step2-1: 打开ssh-agent

            (1) => 官方bash -- $ ssh-agent -s

            (2) => 其他 --- $ eval $(ssh-agent -s)
    
    step2-2: 分别添加私钥

            code: $ ssh-add ~/.ssh/id_rsa_one

            code: $ ssh-add ~/.ssh/id_rsa_two

- step3 --- 创建config文件

    step3-1: 新建config文件

        code： $ touch config / 在.ssh文件夹下新建config文件

    step3-2: 配置config文件(尤其重要)

        code: 
            # one(one@gmail.com)

            Host one.github.com

        　　HostName github.com

        　　PreferredAuthentications publickey

        　　IdentityFile ~/.ssh/id_rsa_one

        　　User one



            # two(two@ gmail.com)

                Host two.github.com

            　　HostName github.com

            　　PreferredAuthentications publickey

            　　IdentityFile ~/.ssh/id_rsa_two

            　　User two

    *配置文件可能有格式问题，所以在配置的时候要尤其注意，文件中会附上本机.ssh 文件配置*

- step4 部署SSH key

        登陆两个github账户，进入Personal settings –> SSH and GPG keys，点击"new SSH key"，将对应的公钥内容(.pub文件)添加到相应的github账户中

- step5 远程测试

        code: $ ssh -T one.github.com

        code: $ ssh -T two.github.com
    
    *验证成功后，会在.ssh文件中生成known_hostwen文件*

备注 => 如果测试出现问题,请仔细检查config文件，大概率为格式文件，后面会附上文件

- step6 使用

    step6-1: clone到本地

        code(原写法) => git clone 官网提供SSH地址

        code(现写法) => git clone git@ one 官网提供SSH地址 //唯一不同就是这个one，此为config里面配置的host

        code(现写法) => git clone git@ two 官网提供SSH地址 //唯一不同就是这个two，此为config里面配置的host

    step6-2: 设置局部的用户名和邮箱

        code => $ git config user.name "one_name" ; git config user.email "one_email"

        code => $ git config user.name "two_name" ; git config user.email "two_email"

    step6-3: 上述完成后，github上钥匙变亮，即完成
