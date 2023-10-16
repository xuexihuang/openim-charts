# openim-charts

# 前提条件
用户要自己安装了k8s，并且最少有两个域名（一个域名用来做minio的api访问，一个接口用来做openimserver的api访问），
配置了好了storageclass（本实例用nfs-client为例）。ps，下个版本会推出居于一个域名的访问和居于ip的url访问。
说明：如果用户k8s系统ingress-control前节点配置了loadbalance，所有*-config.yaml的域名信息都不需要配置tls项
# 安装中间件
helm add im-infra https://xxxxx.xxx
helm install im-mysql im-infra/mysql -f mysql-config.yaml
helm install im-kafka im-infra/kafka -f kafka-config.yaml
helm install im-minio im-infra/minio -f minio-config.yaml
helm install im-mongodb im-infra/mongodb -f mongodb-config.yaml
helm install im-redis im-infra/redis -f redis-config.yaml
说明：这五个配置文件，都配置了账户信息，其中minio-config.yaml还配置了域名信息

# 安装openimserver服务
helm install openimserver -f k8s-open-im-server-config.yaml -f config-imserver.yaml -f notification.yaml ./open-im-server/
说明：要在文件k8s-open-im-server-config.yaml配置域名信息，config-imserver.yaml中账户信息默认和中间件的*-config.yaml是同步的，
如果前面安装了中间件时候修改了*-config.yaml请同步修改config-imserver.yaml

# 安装openimchat服务
helm install openimchat -f k8s-chat-server-config.yaml -f config-chatserver.yaml ./chat-server/
说明：要在文件k8s-chat-server-config.yaml配置域名信息，config-chatserver.yaml中账户信息默认和中间件的*-config.yaml是同步的，
如果前面安装了中间件时候修改了*-config.yaml请同步修改config-chatserver.yaml

# 安装webfront
helm install imwebfront -f k8s-webfront-config.yaml ./webfront/
说明：要在k8s-webfront-config.yaml配置域名信息

# 安装adminfront
helm install imadminfront -f k8s-adminfront-config.yaml ./adminfront/
说明：要在k8s-adminfront-config.yaml.yaml配置域名信息

