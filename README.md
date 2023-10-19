# openim-charts

# 前提条件
1，用户要自己安装了k8s，并且最少有一个域名（如果还需要开启minio的管理web访问，需要准备两个域名，可以是子域名），请替换这五个配置文件（minio-config.yaml，k8s-adminfront-config.yaml，k8s-chat-server-config.yaml，
k8s-open-im-server-config.yaml，k8s-webfront-config.yaml）的域名信息和证书信息。如果用户k8s系统ingress-control前节点配置了loadbalance，所有5个配置文件的域名信息都不需要配置tls项（改成tls: []）。
2，配置了好了storageclass（本实例用nfs-client为例）。
ps，下个版本会推出居于一个域名的访问和居于ip的url访问。
# 安装中间件
helm install im-mysql infra/mysql/ -f mysql-config.yaml
helm install im-kafka infra/kafka/ -f kafka-config.yaml
helm install im-minio infra/minio/ -f minio-config.yaml
helm install im-mongodb infra/mongodb/ -f mongodb-config.yaml
helm install im-redis infra/redis/ -f redis-config.yaml
说明：这五个配置文件，都配置了账户信息，其中minio-config.yaml还配置了域名信息,请修改成你真实的域名和tls名称。默认情况不开启minio的控制台web访问，如果要开启，请填写子域名名称的和对应的tls。
请不要修改chart名称:im-mysql,im-kafka,im-minio,im-mongodb,im-redis，否则要同步中间件的serviceName信息到config-imserver.yaml和config-chatserver.yaml。
请不要修改五个配置文件的账户信息，否则要同步中间件的账户信息到config-imserver.yaml和config-chatserver.yaml中。

# 安装openimserver服务
helm install openimserver -f k8s-open-im-server-config.yaml -f config-imserver.yaml -f notification.yaml ./open-im-server/
说明：要在文件k8s-open-im-server-config.yaml配置域名信息，请修改成你真实的域名和tls名称。config-imserver.yaml中账户信息默认和中间件的*-config.yaml是同步的，
如果前面安装了中间件时候修改了*-config.yaml请同步修改config-imserver.yaml

# 安装openimchat服务
helm install openimchat -f k8s-chat-server-config.yaml -f config-chatserver.yaml ./chat-server/
说明：要在文件k8s-chat-server-config.yaml配置域名信息，请修改成你真实的域名和tls名称。config-chatserver.yaml中账户信息默认和中间件的*-config.yaml是同步的，
如果前面安装了中间件时候修改了*-config.yaml请同步修改config-chatserver.yaml

# 安装webfront
helm install imwebfront -f k8s-webfront-config.yaml ./webfront/
说明：要在k8s-webfront-config.yaml配置域名信息，请修改成你真实的域名和tls名称

# 安装adminfront
helm install imadminfront -f k8s-adminfront-config.yaml ./adminfront/
说明：要在k8s-adminfront-config.yaml.yaml配置域名信息，请修改成你真实的域名和tls名称

