# Montaje ambiente local
> A continuación se indica el paso a paso que se debe realizar para crear localmente los recursos necesarios para ejecución de los microservicios con servicios de la nube AWS

# Getting Started

- Abrir la consola de comandos para descargar la imagen de Postgresql y subir el contenedor en Podman o Docker
    ```
    podman machine start
    podman pull docker.io/library/postgres:16
    ```

- Ejecutar el contenedor
    ```
    podman run --name postgres-vetrik-community -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=123456 -e POSTGRES_DB=db_vetrik_community -p 5432:5432 -d postgres:16
    ```

- Comandos cuando se ha instalado el contenedor
    ```
    podman machine start
    podman start localstack
    podman start postgres-vetrik-community
    ```

- Secretos
    ```
    aws secretsmanager create-secret --name local-postgresql --description "Connection to local PostgreSQL" --secret-string "{\"url\":\"r2dbc:postgresql://localhost:5432/db_vetrik_community\",\"usr\":\"postgres\",\"psw\":\"123456\"}" --endpoint-url=http://localhost:4566
    ```

- Creamos schemas
    ```
    CREATE SCHEMA IF NOT EXISTS community;
    CREATE SCHEMA IF NOT EXISTS business;
    CREATE SCHEMA IF NOT EXISTS commerce;
    ```

- Crear sequences
    ```
    CREATE SEQUENCE community.table_id_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1;
    ```

- Creamos las tablas
    ```
    DROP TABLE IF EXISTS community.table;
    
    CREATE TABLE community.table (
        id BIGINT PRIMARY KEY DEFAULT nextval('community.table_id_seq'),
        short_code VARCHAR(10) NOT NULL,
        name VARCHAR(100) NOT NULL,
        description TEXT,
        status BOOLEAN NOT NULL,
        date_creation TIMESTAMP NOT NULL
    );

    ALTER TABLE worldregion.countries
        ALTER COLUMN id SET DEFAULT nextval('community.table_id_seq');
    ```