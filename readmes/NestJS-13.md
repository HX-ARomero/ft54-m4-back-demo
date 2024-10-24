# Nest JS - Deployment

[Volver a Inicio](../README.md)

## MATERIAL PARA LA CLASE

- Slides: https://docs.google.com/presentation/d/1fe1c4cpbZ_NQA_Q7jOSriIulGfcgd5kd/edit#slide=id.p1
- Repositorio:
- Material Extra
  - Nest JS - Documentación: https://nestjs.com/
  - Docker Hub: https://hub.docker.com/
  - Render: https://render.com/
- Copiamos la Carpeta de la Demo "web-ft48" con otro nombre para el Deploy

## 01- PROCESO DE DEPLOYMENT

### 👉 Proceso y etapas del deployment

- Se refiere al cambio de una aplicación de un entorno de desarrollo a lo que se conoce como entorno de producción.
- Todas aquellas aplicaciones de terceros que usamos hoy en día, deben pasar por este proceso para poder llegar hasta nosotros.
- Implica varias etapas y consideraciones que aseguran que la aplicación mantenga un funcionamiento óptimo.
  1.  Preparación del entorno de producción
  2.  Empaquetado y distribución
  3.  Proceso de automatización
  4.  Plan de monitoreo
      - 🎯 Esta etapa implica poseer un Servicio de Pago

🎯 También llamado Despliegue o Implementación

- El es pasaje de la etapa de Desarrollo a la de Producción
- La etapa de desarrollo no termina con el deploy
- Alternativas a Render:
  - Heroku / Amazon Web Services (AWS) / Google Cloud Platform (GCP) / Microsoft Azure / DigitalOcean / Netlify

### 👉 CI/CD y su importancia en la implementación continua y automatizada

#### 1. CI - Integración Continua

- Hace referencia a la técnica en la que los desarrolladores integran su trabajo en un repositorio compartido, a medida que escriben código.
- Cada integración se verifica a través de pruebas automatizadas y otras herramientas.
- Garantiza que el código integrado funcione de manera adecuada.
  🎯 Cada desarrollador posee su Rama de GitHub, al Mergear se realizan pruebas de Integración, ya que en etapa de Producción puede generar errores en el Servidor.

#### 2. CD - Despliegue Continuo

- Es una extensión de CI que se centra en automatizar el proceso de implementación del artefacto en diferentes entornos, como desarrollo y producción.
- Con esta práctica, cada cambio confirmado en el repositorio de código puede desplegarse automáticamente en un entorno de producción.
  🎯 Los cambios implementados pasan a Producción, puede manejarse con dos ramas en GitHub, una de Desarrollo y otra de Producción, que se Despliega en forma automática.
  🎯 Resumen de los pasos que se realizan en forma Semi-Automática:
- GitFlow (GitHub / GitLab / Jenkins)

1. Agregado/Modificado de nueva funcionalidad
2. Integración Continua (CI)
   - Pull Request => Merge a la rama Development
   - Tests Unitarios y de Integración
3. Development Continuo (CD)
   - Merge a la rama Production (Deployada)

## 02- IMPLEMENTACIÓN DE UN PROYECTO

### 👉 CI/CD en la práctica

- Puede ser implementado en proyectos de Docker para mejorar la automatización y eficiencia del ciclo de vida de una aplicación.
- Los contenedores pueden ser integrados y evaluados de forma continua mediante herramientas como Jenkins, Gitlab, GitHub, etc.
- Permiten el trabajo conjunto de un equipo de desarrollo en un entorno controlado.
- Los cambios pueden pasar a la etapa de implementación una vez se cumplan las condiciones requeridas.

🎯 Pasos en IMAGEN: Código => Implementación => Monitoreo => Operatividad => Deploy => Lanzamiento de nueva versión (Release) => Test => Construcción
🎯 Todos estos pasos los realizamos mediante GitHub

### 👉 Github Actions

- Es un  servicio de GitHub.
- Permite definir y controlar el flujo de trabajo dentro de un repositorio.
- Al mismo tiempo se publica el artefacto resultante en la plataforma de Docker Hub.
- Utiliza la herramienta Actions de Github para automatizar el proceso de construcción, prueba y despliegue de la aplicación en Docker Hub.
- Este mismo flujo se repetirá de manera constante y automática cada vez se generen cambios en el repositorio.

🎯 Pasos en IMAGEN:

1. Desarrollador hace cambios en el código
2. Mediante "push" se cargan en repositorio de "GitHub" (GitHub Actions)
3. Mediante "build & push" impactan en el Contenedor de "Docker Hub"
4. Los cambios en el Contenedor se reflejan en el "Deploy"

## 03- DESPLIEGUE DE APLICACIÓN CON RENDER

## Deploy de Proyecto

### 1. Creación de nuevo repositorio en GitHub

- Necesitaremos tener permisos en GitHub

### 2. Subir nuestro proyecto al repo de GitHub

- Cuando se crea un nuevo proyecto en Nest se inicializa Git
- Copiamos el Proyecto en otra CARPETA
- En la CARPETA copia de nuestro proyecto Demo:
  - Borramos carpeta oculta ".git" y carpetas que NO utilizaremos
  - Todo el proyecto estará en la raíz de la nueva carpeta
- En ARCHIVO ".gitignore"
  - Excluimos ARCHIVO ".development.env" y "node_modules"

```.gitignore
# compiled output
/dist
/node_modules
/build

// ----- ----- ----- -----
# dotenv environment variable files
.env
.env.development.local
.env.test.local
.env.production.local
.env.local
.development.env
```

- En la Terminal Integrada
  - Ingresamos los comandos desde la ventana del navegador del nuevo repositorio de GitHub

```bash
# Inicializamos nuevo proyecto en Git, nos ubicará en la rama "master"
git init

# Creamos y nos movemos a rama "main"
git checkout -b main

# Asociamos nuevo repositorio de GitHub a nuestro proyecto
# Copiamos comando desde GitHub
git remote add origin <dirección_del_repo>

# Subimos todos los cambios a GitHub
git add .
git commit -m "first commit"
git push
# git push --set-upstream origin main
# Creamos rama main y pusheamos
git push --set-upstream origin main
```

- Verificamos los cambios en GitHub

### 3. Acceder a Docker Hub y generar Token

- Creamos cuenta en Docker Hub
  - email: nuestro_email
  - username: nuestro_username
- Claves de Acceso
  - Usuario (A) => Account Settings => Security
  - New Access Token => Nombre
    - Access Token Description: "nombre_de_proyecto"
      - => Generate => Copy and Close
      - Se ha generado nuestra IMAGEN en Docker Hub
    - NO PODEMOS VOLVER A OBTENER LAS CREDENCIALES !!!!!
      - Guardar por ejemplo en un ARCHIVO "txt"

```txt
1. Run
docker login  -u <nuestro_username>

2. At the password prompt, enter the personal access token.
  - Aquí nos dará el token...
```

### 4. Registrar Claves en GitHub

- En GitHub

  - Settings (del Repo) => Secrets and Variables => Actions
  - Crearemos Claves que utilizaremos dentro de este Repositorio

- New repository secret

```txt
Name:   DOCKERHUB_TOKEN
Secret: nos_dará_el_token
```

- Add Secret

- New repository secret

```txt
Name:   DOCKERHUB_USERNAME
Secret: # AQUI VA EL NOMBRE DE DESKTOP HUB
```

- Add Secret

### 5. Agregar GitHub Actions en GitHub

- Automatizan la Implementación y Despliegue continuos
  - Se ejecutará cada vez que el Repositorio se modifique
- Sección "Actions"
  - Opción "Skip this and set up a workflow yourself ->"
- Se creará automáticamente ARCHIVO en la ruta:
  - repo/.github/workflows/main.yml

```yml
name: deploy

on:
  push:
    branches:
      - "main"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/<nombre_del_proyecto>:latest
```

🎯 => <nombre_del_proyecto>: Nombre de la Imagen
🎯 DESCRIPCIÓN:

1. `name: deploy`: Define el nombre del flujo de trabajo como "deploy".
2. `on`: Especifica el evento que activará este flujo de trabajo. En este caso, el evento es "push" (cambio en el repositorio remoto) en la rama "main".
3. `jobs`: Define los trabajos que se ejecutarán como parte de este flujo de trabajo. En este caso, solo hay un trabajo llamado "build".
4. `build`: Define las tareas que se ejecutarán en este trabajo.
5. `runs-on: ubuntu-latest`: Indica que este trabajo se ejecutará en una máquina virtual Ubuntu de la versión más reciente disponible.
6. `steps`: Define los pasos individuales que se ejecutarán en este trabajo.
7. `Checkout`: Utiliza la acción `actions/checkout@v4` para clonar el repositorio en la máquina virtual.
8. `Login to Docker Hub`: Utiliza la acción `docker/login-action@v3` para iniciar sesión en Docker Hub utilizando las credenciales almacenadas en los secretos de GitHub.
9. `Set up Docker Buildx`: Utiliza la acción `docker/setup-buildx-action@v3` para configurar Docker Buildx, que es una herramienta para realizar compilaciones y despliegues de imágenes de contenedor Docker.
10. `Build and push`: Utiliza la acción `docker/build-push-action@v5` para construir la imagen de Docker utilizando el archivo `Dockerfile` en el directorio actual (`context: .`) y luego empujar esa imagen a Docker Hub. El tag de la imagen se define como `${{ secrets.DOCKERHUB_USERNAME }}/nest-test:latest`, donde `secrets.DOCKERHUB_USERNAME` es el nombre de usuario de Docker Hub almacenado en los secretos de GitHub.
    🎯 En resumen, este flujo de trabajo automatiza el proceso de construcción y despliegue de una imagen Docker a Docker Hub cada vez que se realiza un push a la rama "main" del repositorio en GitHub.

- Click en "Commit Changes" => "Commit Changes"
- Verificamos en la Sección "Actions" => "build" - Click en "build"
  🎯 Nos muestra los pasos que se ejecutan:

1. Set up job
   - Configuraciones iniciales preparación del entorno
2. Checkout
   - Clonado del repo en la máquina virtual de GitHub Actions
3. Login to Docker Hub: Credenciales de Docker Hub
   - Se inicia sesión en Docker Hub utilizando las credenciales
4. Set up Docker Build:
   - Se configura Docker Buildx en la máquina virtual. Docker Buildx es una herramienta que permite construir imágenes Docker de manera más eficiente y compatible con diferentes plataformas.
5. Build and push
   - Construcción de la imagen Docker utilizando el archivo Dockerfile presente en el directorio actual.
   - Una vez construida, la imagen se envía a Docker Hub.
6. Post Set up Docker Buildx
   - Este paso se ejecuta después de la configuración de Docker Buildx, pero antes de que comience la construcción de la imagen. Puede realizarse alguna tarea adicional relacionada con la configuración de Docker Buildx.
7. Post Login to Docker Hub
   - Este paso se ejecuta después de iniciar sesión en Docker Hub, pero antes de la construcción y empuje de la imagen. Puede realizarse alguna tarea adicional relacionada con la autenticación en Docker Hub
8. Post Checkout
   - Este paso se ejecuta después de clonar el repositorio en la máquina virtual, pero antes de realizar cualquier otra tarea. Puede realizarse alguna tarea adicional relacionada con la verificación del código clonado.
9. Complete job
   - Este es el último paso del trabajo, donde se indica que el trabajo se ha completado correctamente y se prepara para finalizar cualquier recurso utilizado durante la ejecución del flujo de trabajo.

- Revisamos en "Docker Hub", sección "Repositories"
  - Aparece ahora nuestra nueva Imagen
  - Podemos utilizarla en Docker-Compose
    - Click sobre la Imagen
      - Copiamos el nombre de la Imagen

🎯 Nuestra Aplicación posee

- Integración Contínua (CI)
- Deploy Contínuo (CD)

### Utilizamos Imagen de Docker Hub

#### EN DOCKER HUB => NUESTRA IMAGEN

🎯 Podemos utilizar cualquier Imagen de Docker Hub

- Ahora podemos compartir y hacer pull de la IMAGEN en Docker Hub

### 6. Crear Cuenta en Render

- Desplegar un Servidor requiere:
  - Deployar una Base de Datos
  - Deployar la Aplicación
- Creamos Cuenta en Render: https://render.com/
  - Get Started
  - Create an account => Google
  - Dashboard

### 7. Crear Base de Datos

- Botón "New +"
  - PostgreSQL (Única opción de Render para BBDD)
- Para la creación utilizamos los siguientes datos:
  - Name: demo (el mismo de nuestro proyecto)
  - Database: demo
  - User: postgresdb
  - Region: Dejamos la Default
  - PostgreSQL Version: Dejamos la Default
  - Instance Type: Free
    - Si sale cartel => Close
  - CREATE DATABASE
- Status: Available => Ya podemos utilizarla
  - Connections
    - Credenciales de la Base de Datos
  - Access Control
    - Source: 0.0.0.0/0 // Description: everywhere
      - Click en botón "Add source"
      - Click en "Use my IP address"
      - Save

### 8. Modificamos variables en Proyecto

- En ARCHIVO "typeorm.ts"
  - Para asegurarnos de que se creen las tablas:

```ts
const config = {
  // ----- ----- ----- -----
  host: process.env.DB_HOST, ///// ///// ///// /////
  // ----- ----- ----- -----
  synchronize: true,
  dropSchema: false,
};
```

- En ARCHIVO "main.ts" Modificamos Puerto:

```ts
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
const PORT = process.env.PORT || 3000; ///// /////

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(PORT); ///// /////
  // console.log('Server listening on http://localhost:3000');
}
bootstrap();
```

- Pusheamos cambios:

```bash
git pull # traemos archivo .yaml

git add .
git commit -m "define DB_HOST"
git push
```

- Podemos ver actualización en GitHub, sección "Actions"
  - Click sobre "build" para ver proceso

### 8. Crear Aplicación de Nest - Deploy

- En Render

  - Click en botón "New +" => Web Service
  - Deploy an existing image from a registry
  - Next
  - Image URL: Debemos obtener dirección desde Docker Hub
    - En General => Click en Imagen de Docker Creada
      - Repositories
    - Botón "Public View"
      - Copiamos URL desde barra de navegación
    - INGRESAMOS la URL copiada en el paso anterior
      - No es necesario ingresar credencial
    - Next
    - Seleccionamos en sección "Instance Type"
      - Free
    - Sección "Environment Variables" (Al final)
      - Seleccionamos "Add from.env"
      - Pegamos variables de archivo
        - .development.env

- DEBEMOS CHEQUEAR CON LOS DATOS DE NUESTRA BBDD
  - La creada por RENDER

```.env
DB_HOST= ---           Hostname
DB_PORT=5432           Port
DB_NAME= ---           Database
DB_USERNAME= ---       Username
DB_PASSWORD= ---       Password

// ----- ----- ----- -----
TAMBIÉN EL RESTO DE LAS VARIABLES DE ENTORNO !!!!!
- CLOUDINARY
- JWT_SECRET           unaClaveSecreta
- ETC...
```

- Click en "Deploy Web Server"
  - Nos redirige a los "Logs de Creación"

```bash
// ----- ----- ----- -----
==> Your service is live 🎉
```

- Ya podemos ver nuestra aplicación en la Red
- Debajo del nombre nos dará la "URL"

### 🏳️Testeamos en Navegador

- GET https://host/api

## CONEXIÓN CON LA BBDD

- Para conectarnos a la Base de Datos desde SQL Shell (psql) tomaremos las credenciales desde RENDER
  - Sección "Connections"

```txt
Hostname:              <hostname>
Port:                  5432
Database:              <database_name>
Username:              <user_name>
Password:              <password>
Internal Database URL: <internal_database_URL>
External Database URL: <external_database_URL>
// Internal y External Database URL SON IGUALES
PSQL Command:          <PSQL Command>
```

- Ingresamos en SQL Shell (psql):

```sql
Server [localhost]:                  <external_database_URL>
Database [postgres]:                 <external_database_URL>
Port [5432]:                         5432
Username [postgres]:                 <user_name>
Contraseña para usuario <user_name>: <PSQL Command>

demo_2nbp=>
```

- Ingresando a la Base de Datos algunos comandos:

```sql
\dt
SELECT * FROM users;

UPDATE "USERS" SET isAdmin = true WHERE name = 'Nombre Usuario';
```
