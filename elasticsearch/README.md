# Download and install the .zip package
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.zip
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.zip.sha512
    shasum -a 512 -c elasticsearch-6.2.2.zip.sha512 
    unzip elasticsearch-6.2.2.zip
    cd elasticsearch-6.2.2/
# Running Elasticsearch from the command line
    ./bin/elasticsearch
# Checking that Elasticsearch is running, You can test that your Elasticsearch node is running by sending an HTTP request to port 9200 on localhost:
    localhost:9200

# 创建索引
    PUT localhost:9200/people
    {
        "settings": {
            "number_of_shards": 5,
            "number_of_replicas": 1
        },
        "mappings": {
            "man": {
                "properties": {
                    "name": {
                        "type": "text"
                    },
                    "country": {
                        "type": "keyword"
                    },
                    "age": {
                        "type": "integer"
                    },
                    "date": {
                        "type": "date",
                        "format": "yyyy-MM-dd HH:mm:ss"
                    }
                }
            }
        }
    }

# 插入文档
    PUT localhost:9200/people/man/1
    {
        "name": "antony",
        "age": 18,
        "country": "CN",
        "date": "2018-03-13 00:00:00"
    }

# 自动生成ID
    POST localhost:9200/people/man
    {
        "name": "iris",
        "age": 19,
        "date": "1992-02-18 12:00:06",
        "country": "CN"
    }

# 查询文档
    GET localhost:9200/people/man/1

# 删除文档
    DELETE localhost:9200/people/man/1

# 更新文档
    POST localhost:9200/people/man/1/_update
    {
        "doc": {
            "country": "CN"
        }
    }

# 条件查询
    POST localhost:9200/people/_search
    {
        "query": {
            "match_all": {
            }
        },
        "sort": [{
            "date": {
                "order": "asc"
            }
        }]
    }

# 聚合查询
    POST localhost:9200/people/_search
    {
        "aggs": {
            "country_of_cn": {
                "terms": {
                    "field": "country"
                }
            }
        }
    }