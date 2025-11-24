<h1>
  <p align="center" width="100%">
    Socket-Proxy
  </p> 
</h1>

<h2> 
  <p align="center" width="100%">
    Un contenedor para añadir una capa de seguridad al Socket de Docker
  </p>
</h2>

<h3>
  <p align="left" width="100%">
    Gracias a la imagen del equipo de <a href="https://github.com/Tecnativa">Tecnativa</a>: <a href="https://github.com/Tecnativa/docker-socket-proxy">docker-socket-proxy</a>
  </p>
</h3>

<!-- [![Static Badge](https://img.shields.io/badge/lang-%F0%9F%87%AC%F0%9F%87%A7_en-blue?style=plastic)](README.md) -->

## Estructura:

    socket-proxy/
     ├─ docker-compose.yml              → archivo docker
     ├─ .env                            → variables de entorno
     └─ haproxy.cfg.template (Opcional) → configuración interna (uso avanzado)

### ¡Importante! `haproxy.cfg.template`

El uso de `haproxy.cfg.template` es **totalmente opcional**, pero si se va a hacer uso del mismo **no** hay que quitar el `.template` del nombre de archivo. Aunque pueda parecer que es una plantilla por la extensión, es el archivo que realmente usa el contenedor.

Se puede usar por ejemplo para retocar los tiempos de espera o cambiar el puerto de escucha interno; ésto último se usa en casos muy específicos.

### Otras observaciones (comentarios completos dentro de cada archivo):

```dockerfile

----
    privileged: true  ← Necesario
    ports:
      - 127.0.0.1:2375:2375  ← Este puerto debe ser únicamente expuesto a la red interna
----
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - $DOCKERDIR/socket-proxy/haproxy.cfg.template:/usr/local/etc/haproxy/haproxy.cfg.template
                      ↖ Comentar o borrar si no se va a usar
----
```

## Arranque del contenedor

```bash
docker compose up -d          → arrancamos socket-proxy en segundo plano

docker logs socket-proxy -f   → examinamos los registros para ver si hay algún problema (CTRL+c para salir)
```
</br>

<h3>
¡Listo! Ya tenemos una capa de seguridad extra en nuestro socket de Docker.
</h3>