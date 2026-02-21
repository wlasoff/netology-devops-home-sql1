# Домашнее задание к занятию 2 «Кластеризация и балансировка нагрузки» - Шумихин Кирилл

### Задание 1

Цели:

1. `Запустите два simple python сервера на своей виртуальной машине на разных портах`
2. `Установите и настройте HAProxy, воспользуйтесь материалами к лекции`
3. `Настройте балансировку Round-robin на 4 уровне.`
4. `На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.`

Запуск сервера - ВМ на Yandex Cloud
```
# Server 1
cd server1
echo "Server 1 Port 8888" > index.html
python3 -m http.server 8888

# Server 2
cd server2
echo "Server 2 Port 9999" > index.html
python3 -m http.server 9999
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 1](ссылка на скриншот 1)`

Конфигурация HAProxy (L4)
```
listen web_tcp
    bind :8080
    mode tcp
    balance roundrobin

    server s1 127.0.0.1:8888 check
    server s2 127.0.0.1:9999 check
```

Проверка
```
curl http://localhost:8080
```
Результат (чередование):
```
Server 1 Port 8888
Server 2 Port 9999
Server 1 Port 8888
Server 2 Port 9999
```
`При необходимости прикрепитe сюда скриншоты
![Название скриншота 1](ссылка на скриншот 1)`

---

### Задание 2

Цели:

1. `Запустите три simple python сервера на своей виртуальной машине на разных портах`
2. `Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4`
3. `HAproxy должен балансировать только тот http-трафик, который адресован домену example.local`
4. `На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.`

Запуск сервера - ВМ на Yandex Cloud
```
# Server 1
python3 -m http.server 8888

# Server 2
python3 -m http.server 9999

# Server 3
python3 -m http.server 7777
```

Конфигурация HAProxy (L7)
```
frontend http_front
    bind :8080
    mode http

    acl host_example hdr(host) -i example.local
    use_backend example_backend if host_example

backend example_backend
    mode http
    balance roundrobin

    server s1 127.0.0.1:8888 weight 2 check
    server s2 127.0.0.1:9999 weight 3 check
    server s3 127.0.0.1:7777 weight 4 check
```

Проверка

`Без домена`
```
curl http://localhost:8080
```
Ответ
```
503 Service Unavailable
```
`При необходимости прикрепитe сюда скриншоты
![Название скриншота 1](ссылка на скриншот 1)`

`С доменом example.local`
```
curl -H "Host: example.local" http://localhost:8080
```
Ответ

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 1](ссылка на скриншот 1)`
