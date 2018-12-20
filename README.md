# How to build nodeos 如何搭建Nodeos-EOS节点 
## 前置条件(Options)
### 主机
- CentOS 7.4 64位
- 内存 16-32G
- 单核/多核高主频CPU 3.2Hz
- 硬盘 500GB
### Docker
```bash
# Install
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
# Add your user to docker group
sudo usermod -aG docker your-user
# 
yum install docker-compose
```

## 初始数据(Options)
[eosnode](https://eosnode.tools)网站提供了昨日最新的备份数据，不需要Nodeos从其它节点慢慢同步...
当然也可以跳过此步骤

多线程下载工具
```bash
yum install epel-release
yum update
yum install axel
```

下载[最新数据](https://eosnode.tools/blocks)
```bash
axel -n 30 $(wget --quiet "https://eosnode.tools/api/blocks?limit=1" -O- | jq -r '.data[0].s3') -o blocks_backup.tar.gz

# Uncompress to ./blocks
mkdir ./nodeos-data-dir && tar xvzf blocks_backup.tar.gz -C ./nodeos-data-dir
```

## 启动Nodeos
```bash
git clone https://github.com/TamirTian/how-to-build-nodeos.git nodeos
cd nodeos
# 如果有备份数据执行。注意：后期维护启动请用start
./nodeos replay
# 无备份数据执行
./nodeos start
```

## 日常维护
```bash
# 启动
./nodeos start
# 重放数据
./nodeos replay
# 停机
./nodeos stop
# 重置
./nodeos init
```

附录：
Nodeos = Node + EOS
