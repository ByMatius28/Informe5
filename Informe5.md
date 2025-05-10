# Implementación de un sitio WordPress utilizando Docker
 
## 2. Tiempo de duración  
**25 minutos**

## 3. Fundamentos

Para entender esta práctica es importante conocer cómo funciona la virtualización a nivel de contenedores usando Docker. Docker es una herramienta que permite crear, desplegar y ejecutar aplicaciones dentro de contenedores. Un **contenedor** es una unidad ligera y portátil que incluye todo lo necesario para ejecutar una aplicación: el código, las dependencias, librerías y configuraciones.

En esta práctica se utilizaron los siguientes conceptos clave:

- **Docker Network**: Permite que contenedores se comuniquen entre sí como si estuvieran conectados en una red local.
- **Docker Volumes**: Se usan para persistir datos que no deben perderse cuando un contenedor se reinicia o elimina.
- **Contenedores de Servicios**: Se usaron contenedores específicos para MySQL, phpMyAdmin y WordPress, que se comunican entre sí mediante una red personalizada.
- **Variables de entorno**: Permiten configurar los contenedores de forma dinámica (como las credenciales de la base de datos).
  
Estas tecnologías permiten una solución ágil para el despliegue de aplicaciones web como WordPress, facilitando el mantenimiento, escalabilidad y portabilidad del entorno.

![Figura 3-1. ](/net%20y%20volumenes.png)

![Figura 3-2. ](/wordpress%20run.png)

![Figura 3-3. ](/crearphp.png)

![Figura 3-4. ](/mysql.png)

## 4. Conocimientos previos

Para realizar esta práctica se  necesita tener claros los siguientes temas:

- Manejo de la línea de comandos en Windows (PowerShell).
- Fundamentos de virtualización con Docker.
- Comandos básicos de Docker (`docker run`, `docker volume`, `docker network`, `docker ps`).
- Conceptos básicos de redes y puertos.
- Manejo básico de navegador web.
- Conocimientos básicos sobre WordPress.

## 5. Objetivos a alcanzar

- Implementar contenedores usando Docker para levantar un sitio WordPress funcional.
- Utilizar volúmenes para persistencia de datos de WordPress y MySQL.
- Administrar una base de datos MySQL desde phpMyAdmin.
- Comprender el uso de variables de entorno y redes Docker.

## 6. Equipo necesario

- Computador con sistema operativo Windows 10/11 (también aplicable en Linux/Mac)
- Docker Desktop instalado y configurado (versión recomendada: Docker v24+)
- Acceso a PowerShell o CMD
- Conexión a internet
- Navegador web (Chrome, Firefox, etc.)

## 7. Material de apoyo

- [Documentación oficial de Docker](https://docs.docker.com/)
- Guía de la asignatura
- Docker Cheat Sheet: https://github.com/wsargent/docker-cheat-sheet
- Sitio oficial de WordPress: https://wordpress.org
- Documentación phpMyAdmin: https://docs.phpmyadmin.net

## 8. Procedimiento

**Paso 1: Crear una red Docker personalizada**

```powershell
docker network create wordpress-net
````

**Paso 2: Crear volúmenes para persistencia**

```powershell
docker volume create wordpress-data
docker volume create mysql-data
```

**Paso 3: Crear contenedor MySQL**

```powershell
docker run -d `
--name wordpress-mysql `
--network wordpress-net `
-e MYSQL_ROOT_PASSWORD=rootpassword `
-e MYSQL_DATABASE=wordpress `
-e MYSQL_USER=wpuser `
-e MYSQL_PASSWORD=wppassword `
-v mysql-data:/var/lib/mysql `
mysql:5.7
```

**Paso 4: Crear contenedor phpMyAdmin**

```powershell
docker run -d `
--name wordpress-phpmyadmin `
--network wordpress-net `
-e PMA_HOST=wordpress-mysql `
-p 8080:80 `
phpmyadmin/phpmyadmin
```

**Paso 5: Crear contenedor WordPress**

```powershell
docker run -d `
--name wordpress-site `
--network wordpress-net `
-e WORDPRESS_DB_HOST=wordpress-mysql:3306 `
-e WORDPRESS_DB_USER=wpuser `
-e WORDPRESS_DB_PASSWORD=wppassword `
-e WORDPRESS_DB_NAME=wordpress `
-v wordpress-data:/var/www/html `
-p 8000:80 `
wordpress
```

**Figura 8-1. Diagrama de contenedores implementados**

![Figura 8-1. Diagrama de contenedores](/diagrama.png)

## 9. Resultados esperados

Al completar esta práctica, se debe poder ingresar desde el navegador a:

* [http://localhost:8000](http://localhost:8000): Página de instalación de WordPress
* [http://localhost:8080](http://localhost:8080): Panel de control de phpMyAdmin

Capturas de pantalla del resultado final:

**Figura9-1 Dokcer ps**

![Figura 9-1. Docker ps](/docker%20ps.png)

**Figura 9-2. Instalador de WordPress desde navegador**

![Figura 9-2. Instalador de WordPress](/wordpress.png)

**Figura 9-3. phpMyAdmin accediendo a la base de datos MySQL**

![Figura 9-3. phpMyAdmin conectado](/phpmyadmin.png)

## 10. Bibliografía

Docker Inc. (s.f.). *Docker Documentation*. [https://docs.docker.com/](https://docs.docker.com/)

WordPress.org. (s.f.). *WordPress – Blog Tool, Publishing Platform, and CMS*. [https://wordpress.org](https://wordpress.org)

phpMyAdmin. (s.f.). *phpMyAdmin documentation*. [https://docs.phpmyadmin.net/](https://docs.phpmyadmin.net/)

Waldo, S. (2022). *Docker Cheat Sheet*. GitHub. [https://github.com/wsargent/docker-cheat-sheet](https://github.com/wsargent/docker-cheat-sheet)

## Audio link

[link audio](https://drive.google.com/file/d/1KrMdc1S1MfOIhDI-7qfPzBfNV26_L24-/view?usp=sharing)
