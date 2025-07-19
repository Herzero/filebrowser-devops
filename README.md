# 📁 FileBrowser + Jenkins Pipeline (DevOps)

Este proyecto contiene una imagen personalizada de [FileBrowser] (https://filebrowser.org/), configurada con Docker, versionado, volúmenes persistentes y un pipeline automatizado con Jenkins.

---

## 🚀 Características del Proyecto

✅ Imagen Docker construida desde un `Dockerfile` personalizado  
✅ Tag de versión (`1.0.0`)   
✅ Volúmenes para configuración, base de datos y archivos  
✅ Pipeline automatizado con Jenkins que:
- Construye la imagen
- Ejecuta el contenedor
- Realiza pruebas automáticas
- Sube la imagen a DockerHub

---

## 🧰 Requisitos

- Docker y Docker Compose
- Jenkins
- Cuenta en DockerHub
- Git

---

## 🐳 Cómo levantar FileBrowser localmente

```bash
docker compose up -d
```

Acceder desde el navegador:

📍 http://localhost:80
👤 Usuario: admin
🔑 Contraseña: admin

## Cómo probar el pipeline en Jenkins

1. Clonar este repositorio

```bash
Editar
git clone https://github.com/tu_usuario/filebrowser-devops.git
cd filebrowser-devops
```

2. Crear un nuevo pipeline en Jenkins
Elegí: "Pipeline script from SCM"

Repositorio: https://github.com/tu_usuario/filebrowser-devops.git

Branch: main

3. Agregar credencial de DockerHub (si no está)
Tipo: Username + Password

ID sugerido: dockerhub_id

Usuario: tu usuario de DockerHub

Password: tu contraseña/token

4. Ejecutar el pipeline
Se ejecutarán las siguientes etapas:

Construcción de la imagen

Eliminación de contenedores previos

Ejecución del contenedor

Test automatizado (puerto 80, existencia de archivo config)

Push a DockerHub con tag 1.0.0

🐙 DockerHub
Imagen publicada en:
🔗 https://hub.docker.com/r/murasame11/filebrowser/tags

📄 Licencia
Este proyecto se entrega como parte de los desafios solicitados en el Bootcamp de DevOps de EducacionIT.

✍️ Autor
Hernán Taboada
💼 Desarrollado para la carrera de DevOps
📅 Año: 2025
