**1.- Crear el archivo de compose**

```plaintext
version: '2'
services:
  web:
    image: nicopaez/jobvacancy-ruby:1.3.0
    links:
      - db
    ports:
      - "3000:3000"
    environment:
      PORT: "3000"
      RACK_ENV: "production"
      DATABASE_URL: "postgres://postgres:Passw0rd!@db:5432/postgres"
    depends_on:
      - db
  db:
    image: postgres:14.4-alpine
    environment:
      POSTGRES_PASSWORD: Passw0rd!
```

**2.- Ejecutar los contenedores**

```plaintext
docker-compose -f docker-compose-jobvacancy.yml up
```

**3.- Contestar las siguientes preguntas**

- ¿Cuántos contenedores se están ejecutando? (pueden verlo en el archivo docker-compose-jobvacancy.yml y también ejecutando docker ps)

2 contenedores, lo que se puede constatar en el resultado del docker ps

PS C:\Users\elugo> docker ps
CONTAINER ID   IMAGE                            COMMAND                  CREATED              STATUS              PORTS                    NAMES
8c4e4143d751   nicopaez/jobvacancy-ruby:1.3.0   "/jobvacancy/start_a…"   About a minute ago   Up About a minute   0.0.0.0:3000->3000/tcp   compose_web_1
eec1bc94c115   postgres:14.4-alpine             "docker-entrypoint.s…"   About a minute ago   Up About a minute   5432/tcp                 compose_db_1

- ¿Cuales son las imágenes en las que están basados los mencionados contenedores?

Las imagenes de los contenedores son nicopaez/jobvacancy-ruby:1.3.0 y postgres:14.4-alpine

- ¿Puedes leer el docker-compose-jobvacancy.yml y describir lo que hace cada una de sus lineas?

```plaintext
version: '2'    -- Se indica que la version de sintaxis utilizada para el compose es la 2
services:       -- Inicio de sección de declaración de servicios 
  web:          -- Etiqueta para iniciar la descripcion del servicio llamado "web"
    image: nicopaez/jobvacancy-ruby:1.3.0   -- Imagen base a utilizar para este servicio
    links:                                  -- Definición de enlace a otros servicios
      - db                                  -- Nombre del alias del otro servicio para conectar a traves de la network
    ports:                                  -- Definicion de mapeo de puertos 
      - "3000:3000"                         -- mapeo de puerto local y del contenedor.
    environment:                            -- Definición de variables y parametros del entorno 
      PORT: "3000"
      RACK_ENV: "production"
      DATABASE_URL: "postgres://postgres:Passw0rd!@db:5432/postgres"
    depends_on:  -- Permite indicar el orden y prioridad de las imagenes.
      - db
  db:          -- Etiqueta para iniciar la descripcion del servicio "db"
    image: postgres:14.4-alpine
    environment:
      POSTGRES_PASSWORD: Passw0rd!
```

- Dado que cada contenedor corre en forma aislada ¿Cómo es posible que esos contenedores se vean entre sí?

Porque fue definido un link entre ellos y adicionalmente si se realiza un docker inspect compose_default se tiene el resultado en donde se visualiza en la sección de containers los dos contenedores que se estan ejecutando para el jobvacancy.

```plaintext
"Containers": {
            "8c4e4143d7511cc06eb7929253a2e2c5916388a0963a22e9b56258e2fbc34746": {
                "Name": "compose_web_1",
                "EndpointID": "4202ed9645a7ba7b931c7907ef4771dc3e3fceef134ea1629d3c0f459d9fcca9",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "eec1bc94c115dfe3755529e582f37bc6c2b50e2fcdb31f1d1f776f6710aad580": {
                "Name": "compose_db_1",
                "EndpointID": "27c9670cee43880020dbfff00ba80a9bf209ff477f372a04663bc268f8121840",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
```
