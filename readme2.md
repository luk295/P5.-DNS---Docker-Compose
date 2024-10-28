services:
  bind: 					--> el servicio creado
    image: internetsystemsconsortium/bind9:9.18 --> la imagen 
    container_name: bind_imagen  		--> el nombre del contenedor	
    ports:   					--> puertos en los que se conecta la maquina local con la maquina virtual
      - 54:53/udp
      - 54:53/tcp
      - 127.0.0.1:953:953/tcp
    #volumes:  					--> Volumenes que se montan de la maquina local a la virtual
    #  - ./configuracionOpciones:/etc/bind
    networks:   				--> red a la que se conecta
      - frontend 				--> nombre de red
networks: 					--> redes que se crean cuando se lanza el docker compose
  frontend:  nombre de red
    driver: bridge 
