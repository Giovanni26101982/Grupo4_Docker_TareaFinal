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

5. **Ejecutar docker compose:**

```bash
docker compose up --build -d
```
<img width="886" height="783" alt="image" src="https://github.com/user-attachments/assets/129cef40-9929-4eda-885d-4307f5f0d347" />

--- 

6. **Verificar el estado de los contenedores:**

```bash
docker ps -a
```
<img width="886" height="102" alt="image" src="https://github.com/user-attachments/assets/3c72451e-be44-42ad-ac7b-02f621ceacc9" />

   - En este caso la base de datos (flowise-db) está Up (healthy), pero el contenedor de Flowise aparece en: Restarting (0) 23 secongs ago
---

7. **Verificar el Log**

```bash
docker logs -f flowise
```
<img width="886" height="39" alt="image" src="https://github.com/user-attachments/assets/e0524bae-5f5a-426c-902b-7fb7f136b87f" />

<img width="886" height="760" alt="image" src="https://github.com/user-attachments/assets/eefe7c5a-f518-42bb-8115-903d95cff636" />

   - Se repite en bucle
```bash
Flowiseai Server
VERSION
  flowise/1.6.3 linux-x64 node-v18.20.1

USAGE
  $ flowise [COMMAND]
COMMANDS
  start
Flowiseai Server
VERSION
  flowise/1.6.3 linux-x64 node-v18.20.1
USAGE
  $ flowise [COMMAND]
COMMANDS
  Start
.
.
.
```
   - Eso significa que la imagen oficial de Flowise no arranca el servidor automáticamente, sino que requiere que le pases explícitamente el comando start.
---

8. **Se modifica el .yml para solucionar el error**

```bash
Antes -> command: ["flowise"]
Nuevo -> command: ["flowise", "start"]
```
---

9. **Volver a crear el contenerdo**

```bash
docker compose down
```
<img width="886" height="105" alt="image" src="https://github.com/user-attachments/assets/a8795069-4d2c-4d74-a4e3-16bb55252814" />

```bash
docker compose up -d –build
```
<img width="886" height="121" alt="image" src="https://github.com/user-attachments/assets/d497b4e0-af40-40fb-be4c-ba1b259d9071" />

```bash
docker ps -a
```
<img width="886" height="103" alt="image" src="https://github.com/user-attachments/assets/3486b1b1-1ec6-4ac3-8daa-ab88ec282de7" />

---

10. **Validar la navegación a la dirección local del contenedor en el host**

```bash
[Antes -> command: ["flowise"]
Nuevo -> command: ["flowise", "start"]](http://localhost:3000/)
```
<img width="886" height="716" alt="image" src="https://github.com/user-attachments/assets/8b069289-49ef-47db-a526-4144849234bd" />

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
