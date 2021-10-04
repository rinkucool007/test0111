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



To get the latestfile from nexus snapshot

NEXUS_URL=https://your-nexus.com
MAVEN_REPO=maven-snapshots
GROUP_ID=...
ARTIFACT_ID=...
VERSION=2.0.1-SNAPSHOT
FILE_EXTENSION=jar

download_url=$(curl -X GET "${NEXUS_URL}/service/rest/v1/search/assets?repository=${MAVEN_REPO}&maven.groupId=${GROUP_ID}&maven.artifactId=${ARTIFACT_ID}&maven.baseVersion=${VERSION}&maven.extension=${FILE_EXTENSION}" -H  "accept: application/json"  | jq -rc '.items | .[].downloadUrl' | sort | tail -n 1)

echo $download_url

wget $download_url
