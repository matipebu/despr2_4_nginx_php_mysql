# Actividad 2.4 — Despliegue de app web con Nginx como servidor estático y proxy inverso a PHP-MySQL, phpMyAdmin para gestión de base de datos, usando Docker Compose

## Despregamento de aplicacións web
**Autor:** Matías Pérez Bustelo

La tarea consiste en un servidor **Nginx** que recibe peticiones que no son archivos estáticos y las redirige a otro contenedor de **PHP** que procesa la lógica, genera la respuesta y se la devuelve a Nginx.

Esta es la estructura que debemos tener en nuestro proyecto, ahora la vamos a analizar.
<img width="1004" height="565" alt="image" src="https://github.com/user-attachments/assets/eb469548-f537-4c9f-8084-a9a89c8ca0e9" />

---

## 1. Servicios en Docker Compose

### 1.1 Servicio Nginx (contenido estático)
En esta captura vemos el primer servicio **Nginx**, que está desplegado para el contenido estático de la tienda (8081) y el blog (8080). Este se inicia después de que los contenedores indicados en `depends_on` estén levantados.

<img width="808" height="469" alt="image" src="https://github.com/user-attachments/assets/2e253c1d-d13b-4de2-9572-9579bd486fc8" />

### 1.2 Servicio PHP - Tienda
Este servicio es un contenedor PHP construido desde un **Dockerfile**, que depende de que el servicio de **MySQL** esté iniciado y operativo.

<img width="483" height="291" alt="image" src="https://github.com/user-attachments/assets/0cd8b1de-56e8-417a-af65-96ae71d5e7bd" />

### 1.3 Servicio PHP - Blog
Este servicio es un contenedor PHP que pertenece al blog. En este caso no tiene ninguna dependencia en la sección `volumes`; son las rutas a las que tiene acceso este contenedor.

<img width="386" height="178" alt="image" src="https://github.com/user-attachments/assets/4c3ad15f-b5a7-4db4-b3a3-bb928a49a92f" />

### 1.4 Servicio MySQL
Este servicio es la base de datos donde se almacenan los datos de forma persistente según lo indicado en `volumes`. Se configura el entorno con la base de datos, el usuario, contraseña y codificación UTF-8.

Incluye un `healthcheck` que garantiza que el servicio esté operativo antes de que otros servicios dependientes se conecten.

<img width="898" height="499" alt="image" src="https://github.com/user-attachments/assets/43496675-8995-485d-ba95-19541787b890" />

---

## 2. Gestión de contenedores

### 2.1 Parar y eliminar contenedores antiguos
Antes de ejecutar el nuevo despliegue:
<img width="1004" height="158" alt="image" src="https://github.com/user-attachments/assets/49d8dfa5-87e2-4c4e-897b-5637b2d356ca" />


### 2.2 Iniciar contenedores
Iniciamos los contenedores con:

```bash
docker-compose up -d
```
<img width="1004" height="138" alt="image" src="https://github.com/user-attachments/assets/4c9120c2-8103-404e-bdc3-3a75bef0a3df" />


### 2.3 Comprobación de estado
Verificamos que todos los contenedores estén activos y que su estado sea `Healthy`:

```bash
docker ps
```

<img width="1004" height="121" alt="image" src="https://github.com/user-attachments/assets/343402e5-f523-415f-964c-b7a033c08569" />

---

## 3. Acceso a las páginas dinámicas

- **Blog:** [http://localhost:8080](http://localhost:8081)
<img width="682" height="564" alt="image" src="https://github.com/user-attachments/assets/bd478437-9237-4628-b1db-e9f0b3febd67" />

- **Tienda:** [http://localhost:8081](http://localhost:8080)

<img width="688" height="642" alt="image" src="https://github.com/user-attachments/assets/41bebdad-67e5-4921-b092-6c6924f240f4" />

