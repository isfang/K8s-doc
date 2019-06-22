1.添加公钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


2.添加源
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

将在 /etc/apt/sources.list中 添加官方源

3.可执行 apt-get update 更新下本地包索引

4.可执行 apt-get install docker-ce=17.03.2~ce-0~ubuntu-xenial


