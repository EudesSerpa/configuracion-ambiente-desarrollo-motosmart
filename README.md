# Guía de Configuración del Entorno de Desarrollo para MotoSmart

¡Bienvenido a la guía de configuración del entorno de trabajo para el desarrollo de software en MotoSmart! Aquí encontrarás todos los pasos necesarios para configurar tu entorno y comenzar a trabajar en emocionantes soluciones. Sigamos adelante y contribuyamos juntos al éxito de MotoSmart.

## Prerrequisitos

Antes de comenzar con la instalación de las herramientas y tecnologías necesarias, asegúrate de haber completado lo siguiente:

1. **Instalar GIT:** 
[Descarga GIT](https://git-scm.com/downloads) e instálalo en tu sistema.

2. **Clonar el Repositorio del Proyecto:** 
Utiliza el siguiente comando para clonar el repositorio de MotoSmart:
   ```bash
   git clone [repositorio-url]
   ```

## Paso 1: Instalación de Dependencias Generales

En este paso, instalaremos herramientas y tecnologías esenciales para nuestro entorno de desarrollo.

1. Ejecuta el siguiente comando en la terminal para instalar dependencias generales:
   
   ```bash
   sudo apt-get -y install git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev imagemagick postgresql postgresql-contrib libpq-dev nodejs npm
   ```

## Paso 2: Instalación de Docker

Instalaremos Docker para manejar los contenedores necesarios en el proyecto.

1. Actualiza los paquetes del sistema:
   ```bash
   sudo apt update
   ```

2. Instala paquetes necesarios para permitir el uso de HTTPS con apt:
   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
   ```

3. Agrega la clave GPG de Docker:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add
   ```

4. Agrega el repositorio de Docker a apt:
   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
   ```

5. Actualiza los paquetes del sistema con los paquetes de Docker agregados:
   ```bash
   sudo apt update
   ```
6. Cambia el punto de instalación de Docker: 
    ```bash
    apt-cache policy docker-ce
    ```

7. Instala Docker:
   ```bash
   sudo apt install docker-ce
   ```

8. Verifica la instalación:
   ```bash
   sudo systemctl status docker
   ```

   La salida resultante es:
   ```bash
    ● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2020-05-19 17:00:41 UTC; 17s ago
    TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 24321 (dockerd)
      Tasks: 8
     Memory: 46.4M
     CGroup: /system.slice/docker.service
             └─24321 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
   ```

## Paso 3: Instalación de Ruby con rbenv

En esta sección, instalaremos Ruby utilizando Rbenv.

1. Actualiza los paquetes del sistema:
   ```bash
   sudo apt update
   ```

2. Instala las dependencias requeridas:
   ```bash
   sudo apt install git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev
   ```

3. Instala Rbenv:
   ```bash
  curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
   ```

   Si obtienes un error, puedes intentar instalarlo mediante apt:
    ```bash
    sudo apt install rbenv
    ```
    
4. Agrega rbenv a las variables de entorno:
   ```bash
   echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
   ```

5. Permite que Rbenv se cargue automáticamente:
   ```bash
   echo ‘eval “$(rbenv init -)”’ >> ~/.bashrc
   ```

   En caso de error, no te preocupe adelante verificaremos que rbenv esté funcionando.

6. Guarda los cambios de las variables de entorno:
   ```bash
   source ~/.bashrc
   ```

7. Verifica la instalación de Rbenv:
   ```bash
   type rbenv
   ```

   Como salida debes obtener algo similar a esto: 
   ```bash
   rbenv is a function
    rbenv ()
    {
        local command;
        command="${1:-}";
        if [ "$#" -gt 0 ]; then
            shift;
        fi;
        case "$command" in
            rehash | shell)
                eval "$(rbenv "sh-$command" "$@")"
            ;;
            *)
                command rbenv "$command" "$@"
            ;;
        esac
    }
   ```

8. Instala Ruby en la versión 2.7.1:
   ```bash
   rbenv install 2.7.1
   ```

   Este proceso puede tomar varios minutos, espera hasta su finalizacion.

9. Establece la versión de Ruby por defecto:
   ```bash
   rbenv global 2.7.1
   ```

10. Verifica la instalación de Ruby:
    ```bash
    ruby -v
    ```

11. Navega a la carpeta del proyecto clonado:
    ```bash
    cd /ruta/a/la/carpeta_del_proyecto
    ```

12. Instala las gemas del proyecto:
    ```bash
    bundle install
    ```
    
    En caso de error con PG:
        12.1. Instalamos la dependencia libpq:
        ```bash
        sudo apt install libpq-dev
        ```
        12.2. Instalamos PG versión 1.5.3:
        ```bash
        gem install pg -v 1.5.3
        ```

    Volvemos a instalar las gemas del proyecto.

13. Instala Rails en la versión 6.1.4.1:
    ```bash
    gem install rails -v 6.1.4.1
    ```

## Paso 4: Configuración de la Base de Datos

En esta etapa, configuraremos la base de datos PostgreSQL.

1. Instala PostgreSQL:
   ```bash
   sudo apt-get install postgresql postgresql-contrib nodejs -y
   ```

2. Verifica la instalación de PostgreSQL:
   ```bash
   psql --version
   ```

3. Verifica el que el servicio de PostgreSQL esté activo:
   ```bash
   sudo service postgresql status
   ```

4. Crea y cambia al usuario "postgres":
   ```bash
   sudo -i -u postgres
   ```

5. Ingresa a la consola de PostgreSQL:
   ```bash
   psql
   ```

6. Crea un nuevo usuario en PostgreSQL (reemplaza `[role_name]` y `[role_password]`):
   ```sql
   CREATE ROLE [role_name] WITH LOGIN SUPERUSER CREATEDB CREATEROLE PASSWORD '[role_password]';
   ```

    Recuerda las credenciales, las necesitarás.

7. Crea la base de datos:
   ```sql
   CREATE DATABASE MotoSmart_development;
   ```
8. Lista las bases de datos:
    ```sql
    \l
    ```

    Verifica el nombre de la base de datos creado anteriormente. En caso de que el nombre se haya formateado a minúsculas, sigue estos pasos:
    7.1. Sal de la consola de PostgreSQL:
        ```sql
        \q
        ```

    7.2. Sal del usuario "postgres":
        ```bash
        exit
        ```

    7.3. Modifica el archivo de configuración de la base de datos del proyecto:
        ```bash
        vim config/database.yml
        ```

        7.3.1. Cambia al modo "Insert":
            ```
            (Presiona la tecla) i
            ```

        7.3.2. Modifica el nombre de la base de datos en la sección "development" de forma que refleje el obtenido en la lista de bases de datos previamente vista.

        7.3.3. Cambia al modo "Commands":
            ```
            (Presiona la tecla) Escape
            ```

        7.3.4. Guarda los cambios y sal del editor:
            ```bash
            :wq
            ```

8. Importa los datos de la base de datos del proyecto (reemplaza `[role_name]` y `[db_name]`):
    ```bash
    pg_restore --verbose --clean --no-acl --no-owner -h localhost -U [role_name] -d [bd_name] db/motosmart_test-18-05-2023.dump
    ```

8. Realiza migraciones y crea la base de datos:
    ```bash
    rails db:create db:migrate
    ```

## Paso 5: Ejecución del Proyecto

Finalmente, ejecutaremos el proyecto MotoSmart.

1. Crea la red "elastic" en Docker:
   ```bash
   docker network create elastic
   ```

2. Ejecuta el contenedor de Docker para Elasticsearch:
   ```bash
   docker run --name elasticsearch --net elastic -p 9200:9200 -e discovery.type=single-node -e ES_JAVA_OPTS="-Xms1g -Xmx1g" -e xpack.security.enabled=false -it docker.elastic.co/elasticsearch/elasticsearch:8.2.2
   ```

3. Ejecuta el proyecto:
   ```bash
   rails s
   ```

4. Abre tu navegador web y navega al proyecto:
   ```
   http://localhost:3000/api/v1/market?user_uuid=-D1aMpbDNTnqEiqfH9fMpacBs1DQUPY6BeXyRDsx934&user_token=Wz89AwayrRweyZUEiAG9
   ```

¡Felicidades! Ahora tienes un entorno de desarrollo completamente configurado para contribuir al emocionante proyecto MotoSmart. ¡Desarrolla soluciones innovadoras y contribuye al éxito de MotoSmart!


## Authors

- [@EudesSerpa](https://github.com/EudesSerpa)
- [@EstebanGVX](https://github.com/EstebanGVX)
