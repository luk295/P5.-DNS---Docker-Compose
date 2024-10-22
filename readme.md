# P5. DNS - Docker Compose
### Configura un contenedor coa imaxe oficial de bind9 usando docker-compose.
Primeiro fíxeno engadindo ao arquivo composo outro arquivo coas características do contenedor. Pero por problemas de versións non me deixa. Polo que o fixen da seginte manera:
```
version: '3.8'
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

### Unha comprobado que o contenedor funciona, sube a GitHub o ficheiro nun repo.

### Pega na resposta o enlace ó repo.

### Engade un Readme.md explicando as opcións do docker-compose.yml
