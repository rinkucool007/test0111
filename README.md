# With Security Feature
docker run -d --restart=unless-stopped --name elasticsearch --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e "http.host=0.0.0.0" -e "transport.host=127.0.0.1" -e "xpack.security.enabled=true" elasticsearch:7.4.2
bin/elasticsearch-setup-passwords interactive

docker run -d --restart=unless-stopped -e XPACK_MONITORING_UI_ENABLED=false --name kibana --net elastic -p 5601:5601 kibana:7.4.2
elasticsearch.username: "elastic"
elasticsearch.password: "elastic"



LogStash :
output {
    elasticsearch {
       hosts => ['https://my.es.server.com:443']
       user => 'elastic'
       password => 'elastic'
       proxy => 'http://my.proxy:80'
       index => "my-index-%{+YYYY.MM.dd}"
    }
}
