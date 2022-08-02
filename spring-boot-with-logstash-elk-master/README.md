command to pull the docker images :--------------------------------------------------------------------------------------------

	docker pull docker.elastic.co/beats/filebeat:7.14.1

	docker pull docker.elastic.co/kibana/kibana:7.14.1

	docker pull docker.elastic.co/logstash/logstash:7.14.1

	docker pull docker.elastic.co/elasticsearch/elasticsearch:7.14.1



command to create network bridge:----------------------------------------------------------------------------------------------

	docker network create -d bridge my-bridge

	docker network ls


command to run elk with filebeat docker image via bridge without logstash:-------------------------------------------------------

	docker run -p 9200:9200 -p 9300:9300 --name elasticsearch -itd --network my-bridge -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.14.1

	docker run --name kibana -itd --network my-bridge -p 5601:5601 docker.elastic.co/kibana/kibana:7.14.1

  Command to create index pattern for filebeat:
  
	docker run -itd --network my-bridge docker.elastic.co/beats/filebeat:7.14.1 setup -E setup.kibana.host=kibana:5601 -E output.elasticsearch.hosts=["elasticsearch:9200"]

curl -L -O  https://raw.githubusercontent.com/elastic/beats/8.3/deploy/docker/filebeat.docker.yml

	docker run -it --network my-bridge --name=filebeat --user=root --volume="${HOME}/filebeat.docker.yml:/usr/share/filebeat/filebeat.yml:ro" --volume="/var/lib/docker/containers:/var/lib/docker/containers:ro" --volume="/var/run/docker.sock:/var/run/docker.sock:ro" docker.elastic.co/beats/filebeat:7.14.1 filebeat -e --strict.perms=false -E output.elasticsearch.hosts=["elasticsearch:9200"]

command to build application docker image and run:-------------------------------------------------------------------------

		docker build -t springboot-with-elk .

		docker run --name springboot-with-elk -p 8081:8081 -itd --network my-bridge springboot-with-elk



command to run elk with filebeat docker image via bridge with logstash:------------------------------------------------------

	docker run -p 9200:9200 -p 9300:9300 --name elasticsearch -itd --network my-bridge -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.14.1

	docker run --name kibana -itd --network my-bridge -p 5601:5601 docker.elastic.co/kibana/kibana:7.14.1

	docker run -p 5044:5044 -p 5000:5000  -h logstash --name logstash -it --network my-bridge -v ${HOME}/config-dir:/config-dir docker.elastic.co/logstash/logstash:7.14.1 -f /config-dir/logstash.conf

  Command to create index pattern for filebeat:
  
	docker run -itd --network my-bridge docker.elastic.co/beats/filebeat:7.14.1 setup -E setup.kibana.host=kibana:5601 -E output.elasticsearch.hosts=["elasticsearch:9200"]

curl -L -O  https://raw.githubusercontent.com/elastic/beats/8.3/deploy/docker/filebeat.docker.yml

	docker run -it --network my-bridge --name=filebeat --user=root --volume="${HOME}/filebeat.docker.yml:/usr/share/filebeat/filebeat.yml:ro" --volume="/var/lib/docker/containers:/var/lib/docker/containers:ro" --volume="/var/run/docker.sock:/var/run/docker.sock:ro" docker.elastic.co/beats/filebeat:7.14.1 filebeat -e --strict.perms=false -E output.logstash.hosts=["logstash:5044"]








