descargar e instalar docker: 
https://www.docker.com/products/personal/

pagina con contenedores ya predefinidos: 
https://hub.docker.com/

#### clonar un contenedor:
- entrar a https://hub.docker.com/
- poner en el buscador postgres
- seleccionar la imagen oficial
- clonar la ultima version: docker pull postgres
- especificar una version: docker pull postgres:9.6
- ejecutar el contenedor: docker run postgres:9.6
- si marca error ejecutarlo: docker run -e POSTGRES_PASSWORD=password postgres:9.6


#### Instrucciones mas usadas:
	docker run -d nginx: ejecutar contenedor en background
	docker images: ver imagenes descargadas
	docker ps: los contenedores que se estan ejecutando
	docker ps -a: los contenedores que se ejecutaron anteriormente
	docker start CONTAINER_ID: reiniciar un contenedor que se cayo (docker ps -a)
	docker log NAMES: ver el log de un contenedor en especifico de (docker ps)
	docker log -f: va leer el log y se va quedar esperando si hay cambios
	docker exec -it CONTAINER_ID sh: ejecutar shell dentro del contenedor
	docker stop CONTAINER_ID: detener un contenedor

	
#### Crear un docker file para una aplicacion de nodejs:
	crear fichero de configuracion: notepad++.exe Dockerfile
	Estructura del archivo:
		# instala un contenedor de nodejs
		FROM node:12.22.1-alpine3.11
		
		# asignamos a /app como directorio de trabajo
		WORKDIR /app
		
		# copiamos todo lo que se encuentre en el directorio al nuevo contenedor
		COPY . .
		
		# compilar todo lo que tenemos en nodejs
		RUN yarn install --production
		
		# especificar que hacer cuando se ejecuta el comando node
		CMD ["node", "/app/src/index.js"]
		
- construir el contenedor: docker build -t getting-started .
- ejecutar imagen en background y exponiendo el puerto 3000: docker run -dp 3000:3000 getting-started
- ejecutar imagen pero con persistencia de datos aunque se reinicie el servicio: docker run -d -v c:\ContainerData:c:\data:RW -p 3000:3000 getting-started	     
- comprobar: docker ps
- sincronizar cambios locales con el contenedor para hacer ajustes: docker run -d -v c:\ContainerData:c:\data:RW -p 3000:3000 -v /Users/kbs/ejemplo-docker/app/src:/app/src getting-started
- reconstruir imagen ya finalizada: docker build -t getting-started:v2


#### Publicar imagen:
- crear cuenta en docker-hub
- loguearse: docker login
- ver imagenes: docker images
- docker tag IMAGE_ID pablokbvs/getting-started:v2  
- docker push pablokbvs/getting-started:v2

#### Dockerizar proyecto nodejs
- inicializar proyecto nodejs: npm init -y
- instalar express: npm i express
- crear archivo de inicio: index.js
		const express = require('express');
		const app = express();

		app.get('/', (req,res) => {
			res.json({
				"message": "server running"
			})
		})

		app.listen(80, () => {
			console.log("server running");
		})
- probar aplicacion: node index.js
	

- crear archivo para configuracion docker: Dockerfile
		FROM node:alpine
		
		WORKDIR /var/www
		
		COPY package.json
		
		RUN npm install
		
		COPY . .
		
		EXPOSE 8080
		
		CMD ["node", "index.js"]
	
- Generar imagen: docker build -t image .
	
- Correr la imagen: docker run -p 8080:8080 image