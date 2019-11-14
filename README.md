## es集群启动命令
```
docker run -d --name es --rm -e "discovery.type=single-node" cuiyf/elasticsearch:7.2.0
docker cp es:/usr/share/elasticsearch/config /cyf/elk/es1/
docker stop es
sudo mkdir -p /cyf/elk/es1/data
sudo chmod -R 777 /cyf/elk/es1/data
cp -a /cyf/elk/es1 /cyf/elk/es2
cp -a /cyf/elk/es1 /cyf/elk/es3
docker run -d --name es-node1 -p 9201:9201 -p 9301:9301 -v /cyf/elk/es1/config/:/usr/share/elasticsearch/config -v /cyf/elk/es1/data/:/usr/share/elasticsearch/data cuiyf/elasticsearch:7.2.0
docker run -d --name es-node2 -p 9202:9202 -p 9302:9302 -v /cyf/elk/es2/config/:/usr/share/elasticsearch/config -v /cyf/elk/es2/data/:/usr/share/elasticsearch/data cuiyf/elasticsearch:7.2.0
docker run -d --name es-node3 -p 9203:9203 -p 9303:9303 -v /cyf/elk/es3/config/:/usr/share/elasticsearch/config -v /cyf/elk/es3/data/:/usr/share/elasticsearch/data cuiyf/elasticsearch:7.2.0
```
## head插件安装
```
docker run -d --name es-head -p 9100:9100 mobz/elasticsearch-head:5
```

## kibana启动命令
**注意手动开启监控，监控es，kibana和logstash监控**
```
docker run -d --name kibana -p 5601:5601 -v /cyf/elk/kibana/config/:/usr/share/kibana/config/:ro -v /cyf/elk/kibana/plugins/:/usr/share/kibana/plugins/:ro cuiyf/kibana:7.2.0
```

## logstash启动命令
```
docker run -d -p 5044:5044 -p 9600:9600 --name logstash  -v /cyf/elk/logstash/config/:/usr/share/logstash/config -v /cyf/elk/logstash/pipeline/:/usr/share/logstash/pipeline cuiyf/logstash:7.2.0
```

## redis启动命令
```
docker run -d --name redis -p 6380:6380 -v /cyf/elk/redis/data/:/data redis:5.0 redis-server  redis.conf
```

## filebeat启动命令
```
docker run -d --name filebeat --hostname localhost --user=root -v /cyf/elk/filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml:ro -v /var/lib/docker:/var/lib/docker:ro -v /var/run/docker.sock:/var/run/docker.sock:ro cuiyf/filebeat:7.2.0

docker run -d --name filebeat --hostname localhost --user=root -v /cyf/elk/filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml:ro -v /var/lib/docker:/var/lib/docker:ro -v /var/run/docker.sock:/var/run/docker.sock:ro -v /cyf/elk/nginx/logs:/cyf/elk/nginx/logs cuiyf/filebeat:7.2.0

docker run -d --name filebeat --hostname localhost --user=root -v /cyf/elk/filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml:ro -v /var/lib/docker:/var/lib/docker:ro -v /var/run/docker.sock:/var/run/docker.sock:ro -v /cyf/elk/nginx/logs:/cyf/elk/nginx/logs:ro -v /cyf/elk/mysql/logs/slow.log:/cyf/elk/mysql/logs/slow.log:ro cuiyf/filebeat:7.2.0
```

## metricbeat启动命令
```
docker run -d --name metricbeat --hostname localhost --user=root -v /cyf/elk/metricbeat/metricbeat.yml:/usr/share/metricbeat/metricbeat.yml:ro -v /var/run/docker.sock:/var/run/docker.sock:ro -v /sys/fs/cgroup:/hostfs/sys/fs/cgroup:ro -v /proc:/hostfs/proc:ro -v /:/hostfs:ro cuiyf/metricbeat:7.2.0
```

## nginx启动命令
```
docker run -d --name nginx -p 80:80 -p 443:443 -v /cyf/elk/nginx/config/:/etc/nginx -v /cyf/elk/nginx/logs:/var/log/nginx cuiyf/nginx:1.16
```
## mysql启动命令
```
docker run -d --rm -e MYSQL_ROOT_PASSWORD=123456 --name mysql cuiyf/mysql:5.7
docker cp mysql:/etc/mysql /cyf/elk/mysql
docker stop mysql
mv /cyf/elk/mysql/mysql /cyf/elk/mysql/config
mkdir -p /cyf/elk/mysql/logs
sudo chown 999:999 logs
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=123456 -v /cyf/elk/mysql/config/:/etc/mysql -v /cyf/elk/mysql/data:/var/lib/mysql -v /cyf/elk/mysql/logs:/var/log/mysql cuiyf/mysql:5.7
```
