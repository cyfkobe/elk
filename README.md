## es集群启动命令
```
docker run -d --name es-node1 --restart=always -p 9201:9201 -p 9301:9301 -v /cyf/elk/es1/config/:/usr/share/elasticsearch/config -v /cyf/elk/es1/data/:/usr/share/elasticsearch/data cuiyf/elasticsearch:7.2.0
docker run -d --name es-node2 --restart=always -p 9202:9202 -p 9302:9302 -v /cyf/elk/es2/config/:/usr/share/elasticsearch/config -v /cyf/elk/es2/data/:/usr/share/elasticsearch/data cuiyf/elasticsearch:7.2.0
docker run -d --name es-node3 --restart=always -p 9203:9203 -p 9303:9303 -v /cyf/elk/es3/config/:/usr/share/elasticsearch/config -v /cyf/elk/es3/data/:/usr/share/elasticsearch/data cuiyf/elasticsearch:7.2.0
```


