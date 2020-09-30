## Try to be trying ! Gud luck Duc :)) 

```shell
wget -qO - https://packages.fluentbit.io/fluentbit.key | apt-key add -
echo "deb https://packages.fluentbit.io/debian/stretch stretch main" > /etc/apt/sources.list.d/fluentbit.list
apt-get update
apt-get install td-agent-bit
service td-agent-bit start
service td-agent-bit status
service td-agent-bit stop
vim /etc/td-agent-bit/td-agent-bit.conf
```

Push this part to `td-fluent-bit.conf`

```
[OUTPUT]
name  stdout
match *

[OUTPUT]
Name  es
Match *
Host localhost
Port 9200
Index fluentbit
Logstash_Format On
Logstash_Prefix fluentbit-esn
Type docker

COMMENT


[INPUT]
name                    tail
tag                     docker.<container_id>
tag_regex               (?<container_id>[^/]+)-json\.log$
path                    /var/lib/docker/containers/*/*-json.log
db                      /var/log/fluent-bit-docker.pos
parser                  docker
docker_mode             true
buffer_chunk_size       1MB
buffer_max_size         1MB
mem_buf_limit           64MB
skip_long_lines         On

```


Then edit your `parser.conf`

```
[PARSER]
    Name        first_line
    Format      regex
    Regex       ^{"log":"(?!\\u0009)(?<log>\S(?:(\\")|[^"]){9}(?:(\\")|[^"])*)"

```

Let's find! 

https://github.com/fluent/fluent-bit/issues/337
