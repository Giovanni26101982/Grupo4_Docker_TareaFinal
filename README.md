# Grupo4_Docker_TareaFinal

# Despliegue de Flowise con PostgreSQL en Docker Compose

Este proyecto despliega **Flowise** con una base de datos **PostgreSQL** dedicada, utilizando **Docker Compose**.

---

## 🚀 Requisitos previos
- Docker instalado
- Docker Compose instalado

---

## 📂 Estructura

flowise-postgres/
│── docker-compose.yml
│── .env
│── README.md


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
##▶️ Levantar servicios

```bash
docker compose up -d
```
⏹️ Detener servicios
```bash
docker compose down
```
⏹️ Detener y eliminar volúmenes
```bash
docker compose down -v
```
🌐 Acceso
- Flowise: http://localhost:3000

🛠️ Buenas prácticas aplicadas
- Variables sensibles en .env

- Servicios separados (flowise y flowise_db)

- Red personalizada (flowise_network)

- Persistencia con volúmenes (flowise_db_data)

- Sin bind mounts → aplicación portable
