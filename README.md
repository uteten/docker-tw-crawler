Twiter Crawler
====
The system visualize tweets including specific words by logstash + elasticsearch + kibana.

# Image
Secondlife,セカンドライフの単語を含むツイートの可視化した例
![Secondlife,セカンドライフの単語を含むツイートの可視化](https://github.com/uteten/tw-crawler/wiki/demo.png)

# Requirement
docker on Linux or MacOS  
(if you use CentOS7, you simply exec `$ sudo yum install docker `.)

# Install & Usage
use only docker almost all are docker 

1. Run ElasticSearch  
    `$ sudo docker run --name es -d  -p 9200:9200 -p 9300:9300  -e "http.host=0.0.0.0" -e "transport.host=127.0.0.1" elasticsearch:6.6.1`

2. Run Kibana  
    `$ sudo docker run --name kiba --link es:elasticsearch:6.6.1 -p 5601:5601 -d kibana`

3. Edit logstash.conf  
    `$ vi logstash.conf`

    fill in consumer_key,consumer_secret,oauth_token,oauth_token_secret
    these are you can create from http://apps.twitter.com


4. Run logstash  
    ``$ sudo docker run -d --rm -v "$PWD":/config-dir logstash:6.6.1  -e "`sed -e "s/ES_ADDR/\`docker inspect es -f "{{ .NetworkSettings.IPAddress }}"\`/" logstash.conf`"``

    (alternative if you want to debug)  
    ``$ sudo docker run -it --rm -v "$PWD":/config-dir logstash:6.6.1  -e "`sed -e "s/ES_ADDR/\`docker inspect es -f "{{ .NetworkSettings.IPAddress }}"\`/" logstash.conf`"``

5. Use  
    wait a few minutes and open http://localhost:5601/ by browser.


