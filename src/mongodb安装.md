# mongodb
## 安装
* 导入mongodb公钥 -- sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4 -- 提示找不到 dirmngr
* sudo apt install dirmngr
* 添加软件源 -- echo "deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.0 main
* 安装MongoDB
sudo apt-get update
sudo apt-get install -y mongodb-org
* 解决没有/home/mongodb目录的问题
sudo mkdir /home/mongodb
sudo chown -R mongodb:mongodb /home/mongodb

## 启动 && 操作
* sudo systemctl start mongod
* 查看日志
sudo cat /var/log/mongodb/mongod.log
sudo cat /var/log/mongodb/mongod.log | grep port
* 停止服务
sudo systemctl stop mongod
* 重启服务
sudo systemctl restart mongod
* 设置开机启动或禁用 MOngoDB服务
sudo systemctl enable mongod -- 设置开机启动
sudo systemctl disable mongod -- 设置开机禁用

## 链接
[如何安装 mongodb](https://wiki.deepin.org/index.php?title=MongoDB%E6%95%B0%E6%8D%AE%E5%BA%93%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97&language=en)