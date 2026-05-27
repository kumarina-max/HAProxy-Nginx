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
