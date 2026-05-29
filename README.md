# HAProxy-Nginx
## Домашнее задание к занятию 2 «Кластеризация и балансировка нагрузки» Кукушкина Марина

## Задание 1

## Конфигурационный файл HAProxy

```bash
global
    daemon
    maxconn 256

defaults
    mode tcp
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

listen stats
    bind :8081
    mode http
    stats enable
    stats uri /stats
    stats realm HAProxy\ Statistics
    stats auth admin:admin

frontend web_frontend
    bind *:8080
    mode tcp
    default_backend web_servers

backend web_servers
    mode tcp
    balance roundrobin
    server s1 127.0.0.1:8888 check inter 3s
    server s2 127.0.0.1:9999 check inter 3s
```
## Cкриншот с перенаправление запросов на разные серверы
![haproxy_roundrobin](screenshots/haproxy_roundrobin.png)

## Задание 2: HAProxy + Weighted Round Robin (7 уровень)

## Конфигурация HAProxy

```bash
global
    daemon
    maxconn 256

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

listen stats
    bind :8081
    mode http
    stats enable
    stats uri /stats
    stats realm HAProxy\ Statistics
    stats auth admin:admin

frontend web_frontend
    bind *:8080
    mode http
    acl is_example_local hdr(host) -i example.local
    http-request deny if !is_example_local
    use_backend web_servers if is_example_local

backend web_servers
    mode http
    balance roundrobin
    server s1 127.0.0.1:8888 check inter 3s weight 2
    server s2 127.0.0.1:8889 check inter 3s weight 3
    server s3 127.0.0.1:8890 check inter 3s weight 4
```
![example_host](screenshots/example_host.png)

![no_example_host](screenshots/no_example_host.png)
