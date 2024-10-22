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
Fago `dig 127.0.0.1:54`(a dirección e o porto do DNS do contenedor).
```
; <<>> DiG 9.18.28-0ubuntu0.22.04.1-Ubuntu <<>> 127.0.0.1:54
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 58945
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;127.0.0.1:54.                  IN      A

;; AUTHORITY SECTION:
.                       86399   IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2024102200 1800 900 604800 86400

;; Query time: 37 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Tue Oct 22 21:21:53 CEST 2024
;; MSG SIZE  rcvd: 116

```
### Pega na resposta o enlace ó repo.

### Engade un Readme.md explicando as opcións do docker-compose.yml
