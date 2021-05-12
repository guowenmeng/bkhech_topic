docker elasticsearch kibana安装:

- elasticsearch
1. docker pull docker.elastic.co/elasticsearch/elasticsearch:6.8.9
2. docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.8.9

- 安装IK分词
1. docker exec -it elasticsearch /bin/bash
2. cd /usr/share/elasticsearch/plugins/
3. elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.8.6/elasticsearch-analysis-ik-6.8.9.zip
4. docker restart elasticsearch 　　

- kibana
1. docker pull docker.elastic.co/kibana/kibana:6.8.9
2. docker run -d --name kibana --link=elasticsearch:es  -p 5601:5601 kibana:6.8.9





- network
docker network create elk

- elasticsearch
1. docker pull docker.elastic.co/elasticsearch/elasticsearch:7.9.3
2. docker run -d --name elasticsearch-7.9.3 --network elk  -p 9201:9200 -p 9301:9300 -e "discovery.type=single-node" elasticsearch:7.9.3

- 安装IK分词
1. docker exec -it elasticsearch-7.9.3 /bin/bash
2. cd /usr/share/elasticsearch/plugins/
3. elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.9.3/elasticsearch-analysis-ik-7.9.3.zip
4. docker restart elasticsearch-7.9.3 　　

- kibana
1. docker pull docker.elastic.co/kibana/kibana:7.9.3
2. docker run -d --name kibana-7.9.3 --network elk -e ELASTICSEARCH_HOSTS=http://elasticsearch-7.9.3:9201 -p 5602:5601 kibana:7.9.3

- test
docker exec kibana-7.9.3 ping elasticsearch-7.9.3

docker exec -it kibana-7.9.3 /bin/bash


