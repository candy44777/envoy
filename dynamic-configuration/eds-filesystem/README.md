# EDS Filesystem demo
环境说明
五个Service:

envoy：Front Proxy,地址为172.17.128.77 
webserver01：第一个后端服务 
webserver01-sidecar：第一个后端服务的Sidecar Proxy,地址为172.17.128.11 
webserver02：第二个后端服务 
webserver02-sidecar：第二个后端服务的Sidecar Proxy,地址为172.17.128.12
运行和测试
创建
docker-compose up
测试
# 查看Cluster中的Endpoint信息
curl 172.17.128.77 /clusters

# 接入front proxy envoy容器的交互式接口，修改eds.conf文件中的内容，将另一个endpoint添加进文件中；
docker exec -it eds-filesystem_envoy_1 /bin/sh
cd /etc/envoy/eds.conf.d/
cat eds.conf.v2 > eds.conf
# 运行下面的命令强制激活文件更改，以便基于inode监视的工作机制可被触发
mv eds.conf temp && mv temp eds.conf

# 再次查看Cluster中的Endpoint信息
curl 172.31.11.2:9901/clusters
停止后清理
docker-compose down

