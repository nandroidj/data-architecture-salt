# data-architecture-salt


# pgAdmin - Setup

1. Crear el `docker-compose`,

```
version: "3"

services: 
  postgres:
    image: postgres:13
    init: true
    environment: 
      - POSTGRES_DB=${DB_DATABASE}
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - ./postgres:/var/lib/postgresql/data
    ports: 
      - "5432:5432"
    restart: always

  pgadmin:
    image: dpage/pgadmin4
    init: true
    environment: 
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_EMAIL}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_PASWORD}
    ports:
      - "80:80"
    depends_on: 
      - postgres 
```

2. Crear el archivo `.env`,

```
export DB_HOST=localhost 
export DB_DATABASE=postgres

export DB_USER=root
export DB_PASSWORD=root

export PGADMIN_EMAIL="nando@iglesias.com"
export PGADMIN_PASWORD="Password123"
```

3. Lanzar el comando `docker-compose up` donde docker se encargara de descargar las imagenes de postgres y pgadmin definidas y montara los contendores.

4. Tal como se define en el archivo `docker-compose`, dirigirse a la url `localhost:80` donde se encuentra disponible el servicio de pgAdmin.

5. Ingresar a la plataforma con las credenciales definidas en las variables de entorno (`.env`).

6. En la barra izquierda, `Browser`, hacer click derecho sobre el icono **Servers**, y luego realizar los siguientes pasos, `Servers -> Reegister -> Server`. 

  - Solapa general: asignarle un nombre al servidor.

  - Solapa connection:

      * hostname: postgres (es el nombre declarado al servicio de base de datos en docker-compose)

      * username y password: ingresar el valor a las variables de entorno definidas en el `.env

7. En el icono del servidor creado realizar click derecho y luego seguir los siguientes pasos, `Create -> Database`. Een la solapa General asignarle un nombre a la base de datos.

8. Hacer click izquierdo sobre la db creada y luego presionar el boton **Query Tool**. Posteriormente, en la solapa *Query Editor* ingresar el modelo de datos a crear en lenguaje *SQL* y finalmente presionar el boton **Execute/Refresh**. 

9. Dentro de la base de datos creada, en la seccion **Schemas** se puede visualizar el/los schemas creados y dentro de cada uno las tablas confeccionadas en *SQL*, entre otras utilidades.

10. Cada vez que se levanten los contendores se tendr√° que **registrar nuevamente el server** desde la GUI de pgAdmin y en efecto, se podr√°n ver los datos persistidos.


## Fuentes

- [postgresql with docker-compose](https://linuxhint.com/run_postgresql_docker_compose/)

- [docker volumes](https://docs.docker.com/storage/volumes/)
