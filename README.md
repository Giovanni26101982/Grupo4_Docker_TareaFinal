# Grupo4_Docker_TareaFinal

# Despliegue de Flowise con PostgreSQL en Docker Compose

Este proyecto despliega **Flowise** con una base de datos **PostgreSQL** dedicada, utilizando **Docker Compose**.

---

## 🚀 Requisitos previos
- Docker instalado
- Docker Compose instalado

---

## 📂 Estructura
```bash
flowise-postgres/
│── docker-compose.yml
│── .env
└── README.md
```

---

## 🛠 Desarrollo - Procedimiento

--- 
1. **Ver espacio disponible**

   Antes de ejecutar, verificar que existe espacio en disco (opcional se pueden remover los volúmenes que ya no se utilizan)

```bash
docker system df
```
<img width="886" height="170" alt="image" src="https://github.com/user-attachments/assets/76604ba6-2ab3-44c0-aac6-3a2e5d7c0bc7" />

--- 

2. **Liberar espacio de imágenes/volúmenes huérfanos**

```bash
docker system prune -af –volumes
```
<img width="886" height="789" alt="image" src="https://github.com/user-attachments/assets/ed539125-df58-4177-84b1-f838522829ef" />
  
  - Se eliminará:
  
    - contenedores detenidos
    - imágenes no usadas
    - volúmenes no usados
    - redes no usadas

--- 

3. **Verificar imágenes grandes**

```bash
docker images --digests --no-trunc --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
```
<img width="886" height="70" alt="image" src="https://github.com/user-attachments/assets/06fb3f4c-e57b-4171-af7a-577a08e41db3" />

--- 

4. **Eliminarlas si existen:**

```bash
docker rmi <image_id>
```

--- 

## ⚙️ Configuración
Editar el archivo `.env` para personalizar puertos, credenciales y nombres de base de datos.

Ejemplo:
```ini
FLOWISE_PORT=3000
POSTGRES_USER=flowise_user
POSTGRES_PASSWORD=flowise_pass
POSTGRES_DB=flowise
```
## ▶️ Levantar servicios

```bash
docker compose up -d
```
## ⏹️ Detener servicios
```bash
docker compose down
```
⏹️ Detener y eliminar volúmenes
```bash
docker compose down -v
```
## 🌐 Acceso
- Flowise: http://localhost:3000

## 🛠️ Buenas prácticas aplicadas
- Variables sensibles en .env

- Servicios separados (flowise y flowise_db)

- Red personalizada (flowise_network)

- Persistencia con volúmenes (flowise_db_data)

- Sin bind mounts → aplicación portable
