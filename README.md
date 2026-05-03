# Proyecto Univy - Plataforma de Gestión Universitaria

## 📋 Descripción General

Plataforma integral para la gestión universitaria desarrollada como proyecto de seminario DevOps y Docker. El proyecto consta de una arquitectura de microservicios containerizada con un backend REST API, una aplicación web para estudiantes y un panel de administración.

## 👥 Equipo - Grupo 5

- Laura Manso
- Jan Nogueria
- Víctor Pintado
- Omar Boury
- Júlia Martínez

---

## 🏗️ Arquitectura de Servicios

El proyecto está organizado en una arquitectura de servicios containerizada con Docker Compose:

```mermaid
graph TB
    Client["👥 Cliente Web"]
    Admin["👤 Administrador"]
    
    subgraph "Frontend"
        Web["🌐 Web App<br/>(React + Vite)<br/>Puerto 80"]
        Backoffice["⚙️ Backoffice<br/>(Angular)<br/>Puerto 9000"]
    end
    
    subgraph "Backend"
        Backend["🔧 Backend API<br/>(Node.js + Express)<br/>Puerto 1337"]
    end
    
    subgraph "Data"
        MongoDB["🗄️ MongoDB<br/>Base de Datos<br/>Puerto 27017"]
    end
    
    Client -->|HTTP/REST| Web
    Admin -->|HTTP/REST| Backoffice
    Web -->|API REST| Backend
    Backoffice -->|API REST| Backend
    Backend -->|Query/Update| MongoDB
    
    style Web fill:#61dafb,stroke:#333,stroke-width:2px,color:#000
    style Backoffice fill:#dd0031,stroke:#333,stroke-width:2px,color:#fff
    style Backend fill:#68a063,stroke:#333,stroke-width:2px,color:#fff
    style MongoDB fill:#13aa52,stroke:#333,stroke-width:2px,color:#fff
    style Client fill:#f0f0f0,stroke:#333,stroke-width:2px
    style Admin fill:#f0f0f0,stroke:#333,stroke-width:2px
```

---

## 📦 Servicios

### 1. **MongoDB** 🗄️
- **Contenedor**: `univy_db`
- **Puerto**: `27017`
- **Descripción**: Base de datos NoSQL que almacena toda la información del sistema
- **Volumen**: `mongo_data` (persistencia de datos)
- **Imagen**: `mongo:latest`

### 2. **Backend API** 🔧
- **Contenedor**: `univy_backend`
- **Puerto**: `1337`
- **Descripción**: API REST desarrollada con Node.js + Express + TypeScript
- **Stack**: TypeScript, Express.js, MongoDB
- **Carpeta**: `EA_BACKEND_G5/`
- **Imagen**: `bictorp/backend-ea:v1.4`
- **Dependencias**: MongoDB

**Módulos principales:**
- `controllers/` - Lógica de controladores
- `routes/` - Definición de rutas API
- `services/` - Lógica de negocio
- `models/` - Esquemas de datos
- `middleware/` - Middleware (autenticación, validación)
- `utils/` - Utilidades (JWT, etc.)

**Funcionalidades:**
- Autenticación y autorización con JWT
- Gestión de universidades, grados y cursos
- Gestión de usuarios
- Sistema de posts y comentarios
- Sistema de reportes
- Estadísticas

### 3. **Web App** 🌐
- **Contenedor**: `univy_web`
- **Puerto**: `80`
- **Descripción**: Aplicación web para estudiantes
- **Stack**: React + TypeScript + Vite
- **Carpeta**: `EA_WEB_G5/`
- **Imagen**: `bictorp/web-ea:v1`
- **Dependencias**: Backend API

**Características:**
- Interface responsiva para estudiantes
- Consumo de API REST del backend
- Gestión de autenticación
- Visualización de posts y comentarios

### 4. **Backoffice** ⚙️
- **Contenedor**: `univy_backoffice`
- **Puerto**: `9000`
- **Descripción**: Panel de administración
- **Stack**: Angular + TypeScript
- **Carpeta**: `EA_BACKOFFICE_G5/`
- **Imagen**: `bictorp/backoffice-ea:v1.2`
- **Dependencias**: Backend API

**Funcionalidades:**
- Panel administrativo
- Gestión de usuarios y permisos
- Reportes y estadísticas
- Administración de universidades y programas académicos

---

## 🚀 Quick Start con Docker

### Requisitos Previos
- Docker
- Docker Compose
- (Opcional) Node.js 18+ para desarrollo local

### Instalación y Ejecución

1. **Clonar el repositorio**
```bash
git clone <repository-url>
cd "SEMINARIO DevOps y Docker"
```

2. **Configurar variables de entorno**
```bash
# Copiar archivo .env en EA_BACKEND_G5
cp EA_BACKEND_G5/.env.example EA_BACKEND_G5/.env
```

3. **Iniciar todos los servicios**
```bash
docker-compose up -d
```

4. **Verificar que los servicios estén corriendo**
```bash
docker-compose ps
```

5. **Acceder a las aplicaciones**
- 🌐 Web App: http://localhost
- ⚙️ Backoffice: http://localhost:9000
- 🔧 Backend API: http://localhost:1337
- 🗄️ MongoDB: localhost:27017

### Comandos Útiles

```bash
# Ver logs de un servicio
docker-compose logs -f backend

# Detener todos los servicios
docker-compose down

# Detener y eliminar volúmenes (⚠️ Borra datos)
docker-compose down -v

# Reconstruir imágenes
docker-compose build

# Ejecutar un comando en un contenedor
docker-compose exec backend npm run dev
```

---

## 🛠️ Desarrollo Local

### Backend

```bash
cd EA_BACKEND_G5

# Instalar dependencias
npm install

# Desarrollo con hot reload
npm run dev

# Build para producción
npm run build

# Ver documentación Swagger
# Disponible en http://localhost:1337/api/docs
```

### Web App

```bash
cd EA_WEB_G5

# Instalar dependencias
npm install

# Servidor de desarrollo
npm run dev

# Build para producción
npm run build
```

### Backoffice

```bash
cd EA_BACKOFFICE_G5

# Instalar dependencias
npm install

# Servidor de desarrollo
npm run start

# Build para producción
npm run build
```

---

## 📚 Endpoints API Principales

### Autenticación
- `POST /api/auth/register` - Registro de usuario
- `POST /api/auth/login` - Inicio de sesión

### Universidades
- `GET /api/universidad` - Listar universidades
- `POST /api/universidad` - Crear universidad (Admin)

### Usuarios
- `GET /api/usuario` - Listar usuarios
- `GET /api/usuario/:id` - Obtener usuario
- `PUT /api/usuario/:id` - Actualizar usuario

### Posts y Comentarios
- `GET /api/post` - Listar posts
- `POST /api/post` - Crear post
- `GET /api/comment` - Listar comentarios
- `POST /api/comment` - Crear comentario

### Reportes
- `GET /api/report` - Listar reportes
- `POST /api/report` - Crear reporte

### Estadísticas
- `GET /api/stats` - Obtener estadísticas

Para documentación completa de la API, consulta el Swagger en `/api/docs` una vez que el backend esté corriendo.

---

## 🐳 Estructura Docker

```
docker-compose.yml
├── mongodb (mongo:latest)
├── backend (bictorp/backend-ea:v1.4)
├── web (bictorp/web-ea:v1)
└── backoffice (bictorp/backoffice-ea:v1.2)
```

### Volumes
- `mongo_data` - Persistencia de datos de MongoDB

### Network
- Todas los servicios se comunican a través de la red Docker predefinida

---

## 📋 Variables de Entorno

### Backend (`EA_BACKEND_G5/.env`)
```
NODE_ENV=production
PORT=1337
MONGODB_URI=mongodb://mongodb:27017/univy
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRATION=24h
```

---

## 🔒 Consideraciones de Seguridad

⚠️ **IMPORTANTE** - En producción:
1. Cambiar puertos expuestos de MongoDB
2. Usar variables de entorno seguras
3. Implementar HTTPS
4. Usar autenticación con Docker Registry
5. Limitar recursos de contenedores
6. Implementar rate limiting

---

## 📖 Documentación Adicional

- [Backend README](./EA_BACKEND_G5/README.md)
- [Web App README](./EA_WEB_G5/README.md)
- [Backoffice README](./EA_BACKOFFICE_G5/README.md)

---

## 🤝 Contribuciones

Este es un proyecto educativo del seminario "DevOps y Docker". Para cambios, por favor contacta con los integrantes del equipo.

---

## 📄 Licencia

Proyecto educativo - Universidad [Institución]

---

**Última actualización**: Mayo 2026
