# P5. DNS - Docker Compose
### Configura un contenedor coa imaxe oficial de bind9 usando docker-compose.
Primeiro fíxeno engadindo ao arquivo composo outro arquivo coas características do contenedor. Pero por problemas de versións non me deixa. Polo que o fixen da seginte manera:

```
services:
  bind:
    image: internetsystemsconsortium/bind9:9.18
    container_name: bind_imagen
    ports:
      - 54:53/udp
      - 54:53/tcp
      - 127.0.0.1:953:953/tcp
    volumes:
      - ./configuracionOpciones:/etc/bind
    networks:
      - frontend
networks:
  frontend:
    driver: bridge
```

### Usa os volumes como se explica no setup da imaxe

Creo unha carpeta chamada configuracionOpciones onde se gardará a configuración do DNS.

Para iso monto a carpeta no directorio /etc/bind do DNS.

### Unha comprobado que o contenedor funciona, sube a GitHub o ficheiro nun repo.
O comprobo co comando `dig`.
Fago `dig 127.0.0.1 -p 54 `(a dirección e o porto do DNS do contenedor).

Pero o contenedor créase, co comando `docker compose up`:
```
[+] Running 1/0
 ✔ Container bind_imagen  Created                                                                           0.1s 
Attaching to bind_imagen
```
E podo ver a información do contenedor coas carpetas montadas co comando `docker inspect bind_imagen`
```
[
    {
        "Id": "36af022cb99f3b14bd42b21560a78ed17e9324591a7ce8729b47fe52e3459c2f",
        "Created": "2024-10-25T17:25:43.593999149Z",
        "Path": "/usr/sbin/named",
        "Args": [
            "-u",
            "bind",
            "-f",
            "-c",
            "/etc/bind/named.conf",
            "-L",
            "/var/log/bind/default.log"
        ]

Detalles 


Detalles da rede: 

"NetworkMode": "compose_frontend",
            "PortBindings": {
                "53/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "54"
                    }
                ],
                "53/udp": [
                    {
                        "HostIp": "",
                        "HostPort": "54"
                    }
                ],
                "953/tcp": [
                    {
                        "HostIp": "127.0.0.1",
                        "HostPort": "953"
                    }
```


### Agora unha inspección a esa rede na que está conectada a imaxe bind_imagen :
`docker network inspect compose_fontend`
```
[
    {
        "Name": "compose_frontend",
        "Id": "761a2054dab4c69950061d4733cb960dbb34c2f4939a0058c5885339328f9ace",
        "Created": "2024-10-25T20:07:47.43841314+02:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.21.0.0/16",
                    "Gateway": "172.21.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "91225cb50e3f181c262a368fed43ef522fe6b657a82294120bcbf6bec2c12abd": {
                "Name": "bind_imagen",
                "EndpointID": "183312c50c4cdac9d8c30628041bd6612fdb70b116d9caa1849c673db4a6905e",
                "MacAddress": "02:42:ac:15:00:02",
                "IPv4Address": "172.21.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "frontend",
            "com.docker.compose.project": "compose",
            "com.docker.compose.version": "2.29.2"
        }
    }
]

```
Ahí vese a rede creada, a ip e os contenedores que están conectados á rede. Neste caso o contenedor chamado *bind_imagen*, coa súa ip.

Fago `dig 172.21.0.2`, que é a IP do servidor. 

>[!NOTA]
>Ao principio ó facer ping atopábame o servidor pero non respondía nada xa que tiña as opción do exemplo no arquivo name.conf onde dicía que non respondera a nada.

```

; <<>> DiG 9.18.28-0ubuntu0.22.04.1-Ubuntu <<>> @172.21.0.2
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51109
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 7ee22b294358969901000000671be474a81b056554a8392a (good)
;; QUESTION SECTION:
;.                              IN      NS

;; ANSWER SECTION:
.                       518365  IN      NS      c.root-servers.net.
.                       518365  IN      NS      i.root-servers.net.
.                       518365  IN      NS      f.root-servers.net.
.                       518365  IN      NS      k.root-servers.net.
.                       518365  IN      NS      a.root-servers.net.
.                       518365  IN      NS      m.root-servers.net.
.                       518365  IN      NS      g.root-servers.net.
.                       518365  IN      NS      b.root-servers.net.
.                       518365  IN      NS      d.root-servers.net.
.                       518365  IN      NS      e.root-servers.net.
.                       518365  IN      NS      l.root-servers.net.
.                       518365  IN      NS      j.root-servers.net.
.                       518365  IN      NS      h.root-servers.net.

;; ADDITIONAL SECTION:
a.root-servers.net.     518365  IN      A       198.41.0.4
b.root-servers.net.     518365  IN      A       170.247.170.2
c.root-servers.net.     518365  IN      A       192.33.4.12
d.root-servers.net.     518365  IN      A       199.7.91.13
e.root-servers.net.     518365  IN      A       192.203.230.10
f.root-servers.net.     518365  IN      A       192.5.5.241
g.root-servers.net.     518365  IN      A       192.112.36.4
h.root-servers.net.     518365  IN      A       198.97.190.53
i.root-servers.net.     518365  IN      A       192.36.148.17
j.root-servers.net.     518365  IN      A       192.58.128.30
k.root-servers.net.     518365  IN      A       193.0.14.129
l.root-servers.net.     518365  IN      A       199.7.83.42
m.root-servers.net.     518365  IN      A       202.12.27.33
a.root-servers.net.     518365  IN      AAAA    2001:503:ba3e::2:30
b.root-servers.net.     518365  IN      AAAA    2801:1b8:10::b
c.root-servers.net.     518365  IN      AAAA    2001:500:2::c
d.root-servers.net.     518365  IN      AAAA    2001:500:2d::d
e.root-servers.net.     518365  IN      AAAA    2001:500:a8::e
f.root-servers.net.     518365  IN      AAAA    2001:500:2f::f
g.root-servers.net.     518365  IN      AAAA    2001:500:12::d0d
h.root-servers.net.     518365  IN      AAAA    2001:500:1::53
i.root-servers.net.     518365  IN      AAAA    2001:7fe::53
j.root-servers.net.     518365  IN      AAAA    2001:503:c27::2:30
k.root-servers.net.     518365  IN      AAAA    2001:7fd::1
l.root-servers.net.     518365  IN      AAAA    2001:500:9f::42
m.root-servers.net.     518365  IN      AAAA    2001:dc3::35

;; Query time: 1 msec
;; SERVER: 172.21.0.2#53(172.21.0.2) (UDP)
;; WHEN: Fri Oct 25 20:33:24 CEST 2024
;; MSG SIZE  rcvd: 851



```


### Pega na resposta o enlace ó repo.

### Engade un Readme.md explicando as opcións do docker-compose.yml
