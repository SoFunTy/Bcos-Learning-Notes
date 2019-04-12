**Build bcos on docker**
====================
Based on ubuntu18.10 OS
### Let’s start!
#### Install Depends before play
	sudo apt-get install git
	sudoapt-get install -y nodejs 
	sudoapt-get install -y npm
	sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
	sudo cnpm install -g babel-cli babel-preset-es2017
	echo '{ "presets": ["es2017"] }' > ~/.babelrc	sudo npm install babel-cli
	sudo apt-get install cmake
	wget https://github.com/ethereum/solidity/releases/download/v0.4.13/solc-static-linux
	sudo cp solc-static-linux  /usr/bin/solc
	sudo chmod +x /usr/bin/solc
	sudo cnpm install -g ethereum-console

##### Install Docker
##### You can reference https://docs.docker.com/install/linux/docker-ce/ubuntu/
##### use packages repository#
$sudo apt-get install \
apt-transport-https \
ca-certificates \
curl \
gnupg-agent \
software-properties-common
##### Add Docker’s official GPG key#
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add –
##### by searching for the last 8 characters of the fingerprint.#
	$sudo apt-key fingerprint 0EBFCD88      
##### softwware update
	$sudo apt-get update
	$apt list –upgradable
##### set up the stable repository
	$sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    (lsb_release -cs) \
    stable"
##### INSTALL NOW
$sudo apt-get install docker-ce docker-ce-cli containerd.io
##### Add your user to the docker group
	$ sudo groupadd docker
	$ sudo usermod -aG docker $USER
##### PowerBoot
$sudo systemctl enable docker     

##### Check docker status, like this:
##### (if you see anything like “Permission denied”,  maybe you need log back in)


##### Please refer to the official website for more information.
##### Configuring Bcos
##### Cloning file
git clone https://github.com/bcosorg/bcos
cd bcos/docker
##### Set Creation block config file into node-0
./scripts/genConfig.sh

##### launch docker container
$ ./scripts/start_bcos_docker.sh $PWD/node-0
##### Correct state:

$ cd ../systemcontractv2
$ cnpm install
(sudo chown -R $USER:$(id -gn $USER) /home/xubi/.config)
##### You can input again to check

##### modification bcos/docker/node-0/config.js  "Port": 35500”
##### Of course, you can open the file to modify
$ sed -i 's/127.0.0.1:8545/127.0.0.1:35500/' config.js
$ babel-node deploy.js
#####  将输出中，SystemProxy合约地址记下来，例如 SystemProxy合约地址 0xff27dc5cc5144c626b9fdc26b2f292d9df062470
#####  修改docker/node-0/config.json中systemproxyaddress为上一步骤记录的SystemProxy合约地址 
$ sed -i 's/"systemproxyaddress":"0x0"/"systemproxyaddress":"0xff27dc5cc5144c626b9fdc26b2f292d9df062470"/' ../docker/node-0/config.json

#####  检查node-0/config.json中系统合约字段是否正确
$ cat ../docker/node-0/config.json | grep systemproxyaddress

##### 重启node-0
$ docker restart $(docker ps -a | grep bcos-node-0 | awk 'NR==1{print$1}')

##### 创世节点信息写入合约
$ babel-node tool.js NodeAction registerNode ../docker/node-0/node.json

##### We can see $docker ps -a


## New prot into
$ cd bcos/docker
##### genConfig.sh的参数分别是除创世节点外的组网节点数和创世节点配置文件路径
$ ./scripts/genConfig.sh 1 node-0
##### 新节点入网
$ cd bcos/systemcontractv2
##### 新加节点node-1信息写入合约
$ babel-node tool.js NodeAction registerNode ../docker/node-1/node.json 

##### 启动新加入节点node-1，参数为新节点配置文件完整路径
$ cd ../docker & ./script/start_bcos_docker.sh $PWD/node-0
##### 要加入更多节点只需要重复步骤2.2-2.3，启动新节点时参数改为新节点配置文件路径即可
##### 查看工作状态
##### 块高增长说明网络工作正常
$ cd ../systemcontractv2
$ babel-node monitor.js




contract HelloWorld{
    string name;
    constructor() public{
       name="Hi,Welcome!";
    }
    function get() public view returns(string memory){
        return name;
    }
    function set(string memory n) public{
    	name=n;
    }
}
into bcos/tool/HelloWorld.sol
cnpm install
babel-node deploy.sol HelloWorld

xubi@xubi-Standard:~/bcos/tool$ babel-node deploy.js HelloWorld
RPC=http://127.0.0.1:35500
Ouputpath=./output/
deploy.js  ........................Start........................
Soc File :HelloWorld
HelloWorld.sol:1:1: Warning: Source file does not specify required compiler version! Consider adding "pragma solidity ^0.5.7;"
contract HelloWorld{
^ (Relevant source part starts here and spans across multiple lines).
HelloWorld编译成功！
(node:1336) [DEP0005] DeprecationWarning: Buffer() is deprecated due to security and usability issues. Please use the Buffer.alloc(), Buffer.allocUnsafe(), or Buffer.from() methods instead.
HelloWorld合约地址 0x71d4e7f19cd8a24c69c79863258017db589ac8af
HelloWorld部署成功！


Q：If you see your port is busy or other you can (sudo apt-get install net-tools  & netstat -ap | grep $you port 				and kill -9 PID) 


Q：
sudo yum install -y openssl openssl-devel
chmod +x scripts/install_deps.sh
./scripts/install_deps.sh

