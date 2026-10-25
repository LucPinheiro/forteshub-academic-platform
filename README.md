# forteshub-academic-platform

# 🔬 FortesHub

### Red científica, currículos académicos y colaboración para investigadores

**FortesHub** es una plataforma web en desarrollo dirigida a investigadores, científicos de datos, equipos de investigación e instituciones académicas, con especial atención al ecosistema científico brasileño.

Reúne perfiles profesionales, currículos académicos, publicaciones, proyectos, materiales científicos y herramientas de colaboración en una interfaz moderna.

El proyecto incluye una **API REST**, persistencia en PostgreSQL, un **frontend React** y funcionalidades asistidas por inteligencia artificial local mediante **Ollama**.

> **Estado:** proyecto en desarrollo / demostración funcional. Algunas funcionalidades de la interfaz todavía utilizan datos de ejemplo o simulaciones locales.

---

## ✨ Funcionalidades

- **Perfiles académicos:** información del investigador, institución, áreas de investigación y trayectoria profesional.
- **Currículos científicos:** formación, publicaciones, proyectos, colaboraciones, materiales y citas.
- **Importación de Lattes:** procesamiento de archivos de currículo Lattes mediante la API.
- **Red científica:** exploración de investigadores, proyectos, publicaciones y materiales.
- **Proyectos y colaboración:** gestión visual de proyectos, participantes y solicitudes de colaboración.
- **Repositorios:** creación y consulta de repositorios asociados a investigadores.
- **Exportación:** generación de documentos PDF e información curricular.
- **Inteligencia artificial:** asistencia y generación de biografías académicas con Ollama.
- **Comunidad:** interfaces de mensajería, notificaciones y contactos.
- **Diseño responsive:** interfaz adaptable a ordenadores, tablets y dispositivos móviles.
- **Personalización:** temas visuales y componentes reutilizables.

> Algunas experiencias de comunidad, autenticación y mensajería son demostrativas y todavía no cuentan con persistencia multiusuario completa.

## 🧰 Stack tecnológico

| Capa | Tecnologías |
| --- | --- |
| Frontend | React 19, TypeScript, Vite 8 |
| Interfaz | Tailwind CSS 4, Lucide React |
| Backend | Node.js 22+, Express 5, TypeScript |
| Base de datos | PostgreSQL 16 |
| ORM | Prisma 6 |
| Validación | Zod |
| Inteligencia artificial | Ollama, llama3.2:3b |
| Procesamiento de archivos | Multer, fast-xml-parser, pdfjs-dist |
| Generación PDF | jsPDF |
| Infraestructura | Docker, Docker Compose |

## 🏗️ Estructura del proyecto

```text
forteshub/
├── Backend/
│   ├── prisma/
│   │   ├── migrations/
│   │   └── schema.prisma
│   ├── src/
│   │   ├── config/
│   │   ├── controllers/
│   │   ├── lib/
│   │   ├── middlewares/
│   │   ├── routes/
│   │   ├── schemas/
│   │   ├── services/
│   │   ├── types/
│   │   ├── app.ts
│   │   └── server.ts
│   ├── .env.example
│   ├── docker-compose.yml
│   ├── Dockerfile
│   └── package.json
├── Frontend/
│   ├── public/
│   ├── src/
│   │   ├── api/
│   │   ├── auth/
│   │   ├── billing/
│   │   ├── chat/
│   │   ├── collab/
│   │   ├── community/
│   │   ├── components/
│   │   ├── cv/
│   │   ├── export/
│   │   ├── pages/
│   │   └── theme/
│   ├── .env.example
│   ├── package.json
│   └── vite.config.ts
└── README.md
```

## 🚀 Instalación

### Requisitos

- Node.js 22 o superior.
- npm.
- Docker y Docker Compose.
- Ollama (opcional, para las funciones de IA).

### 1. Clonar el repositorio

```bash
git clone https://github.com/LucPinheiro/forteshub.git
cd forteshub
```

### 2. Configurar el backend

```bash
cd Backend
cp .env.example .env
```

En Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Configura las variables de entorno, incluyendo las credenciales de PostgreSQL.

### 3. Iniciar backend y base de datos

```bash
docker compose up -d --build
```

Servicios predeterminados:

| Servicio | URL |
| --- | --- |
| API REST | http://localhost:4601 |
| Health check | http://localhost:4601/health |
| PostgreSQL | localhost:5442 |

Comprobar el estado:

```bash
docker compose ps
docker compose logs -f api
```

El contenedor aplica las migraciones Prisma al arrancar.

### 4. Iniciar el frontend

Desde otra terminal, en la raíz del repositorio:

```bash
cd Frontend
cp .env.example .env
npm ci
npm run dev
```

En Windows PowerShell, utiliza `Copy-Item .env.example .env`.

**Acceso:** http://localhost:5173

### 5. Configurar Ollama (opcional)

Descarga el modelo de IA:

```bash
ollama pull llama3.2:3b
```

Configuración predeterminada:

```env
OLLAMA_BASE_URL=http://host.docker.internal:11434
OLLAMA_MODEL=llama3.2:3b
```

Para el frontend:

```env
VITE_OLLAMA_URL=http://localhost:11434
VITE_OLLAMA_MODEL=llama3.2:3b
```

El backend puede funcionar sin Ollama, pero las funcionalidades dependientes de IA no estarán disponibles.

## ⚙️ Variables de entorno

### Backend

| Variable | Descripción |
| --- | --- |
| `NODE_ENV` | Entorno de ejecución |
| `PORT` | Puerto interno de la API |
| `DATABASE_URL` | Conexión PostgreSQL |
| `POSTGRES_USER` | Usuario PostgreSQL |
| `POSTGRES_PASSWORD` | Contraseña PostgreSQL |
| `POSTGRES_DB` | Nombre de base de datos |
| `POSTGRES_PORT` | Puerto PostgreSQL |
| `CORS_ORIGIN` | Origen autorizado del frontend |
| `OLLAMA_BASE_URL` | URL del servicio Ollama |
| `OLLAMA_MODEL` | Modelo de IA |
| `OLLAMA_TIMEOUT_MS` | Tiempo máximo de espera |
| `LATTES_MAX_UPLOAD_MB` | Tamaño máximo de archivos Lattes |

### Frontend

| Variable | Descripción |
| --- | --- |
| `VITE_API_URL` | Dirección de la API |
| `VITE_GOOGLE_CLIENT_ID` | Cliente de Google para acceso |
| `VITE_OLLAMA_URL` | Dirección de Ollama |
| `VITE_OLLAMA_MODEL` | Modelo de IA |

> Nunca publiques archivos .env con credenciales reales.

## 🔌 API REST

Endpoints identificados en el backend:

| Método | Endpoint | Funcionalidad |
| --- | --- | --- |
| GET | `/health` | Estado de los servicios |
| POST | `/api/v1/researchers/import-lattes` | Importación de currículos Lattes |
| GET | `/api/v1/researchers/:id` | Consultar investigador |
| POST | `/api/v1/researchers/:id/bio` | Generar biografía con IA |
| POST | `/api/v1/repositories` | Crear repositorio |
| GET | `/api/v1/repositories/researcher/:researcherId` | Repositorios de investigador |
| GET | `/api/v1/repositories/:username/:slug` | Consultar repositorio |

### Comprobar la API

```bash
curl http://localhost:4601/health
```

Los estados posibles incluyen `ok`, `degraded` y `down`.

## 🛠️ Desarrollo local sin Docker

Con PostgreSQL previamente instalado y configurado:

```bash
cd Backend
cp .env.example .env
npm ci
npm run prisma:generate
npm run prisma:migrate
npm run dev
```

En este modo el puerto predeterminado de la API es 4600. Ajusta `VITE_API_URL` si es necesario.

### Comandos de desarrollo

Backend:

```bash
npm run typecheck
npm run build
npm run prisma:studio
```

Frontend:

```bash
npm run lint
npm run build
npm run preview
```

## 🗺️ Estado del proyecto

FortesHub está en fase de desarrollo y mejora continua.

### Componentes presentes

- API de investigadores y repositorios.
- Importación de currículos Lattes.
- PostgreSQL y migraciones Prisma.
- Generación de biografías asistida por Ollama.
- Interfaces de perfiles, currículos y publicaciones.
- Exportación de información académica.
- Experiencias visuales de colaboración y comunidad.

### Próximas mejoras

- [ ] Autenticación y autorización completas en backend.
- [ ] Persistencia multiusuario de mensajería y conexiones.
- [ ] Integración completa de funcionalidades frontend y backend.
- [ ] Pruebas automatizadas.
- [ ] Documentación OpenAPI.
- [ ] Mejoras de seguridad y despliegue en producción.
- [ ] Gestión avanzada de privacidad y datos personales.

## 🔐 Seguridad y privacidad

FortesHub trabaja con información académica y profesional que puede incluir datos personales.

Antes de realizar un despliegue público, será necesario revisar los requisitos de protección de datos, control de acceso, almacenamiento y consentimiento, incluida la **LGPD de Brasil** cuando corresponda.

El proyecto debe considerarse en desarrollo y no como una plataforma preparada para producción sin revisiones adicionales de seguridad.

## 👩‍💻 Autora

**Lu Pinheiro**

Desarrolladora Full Stack · Integración de Sistemas · Inteligencia Artificial

**GitHub:** https://github.com/LucPinheiro

---

### 🔬 FortesHub

*Conectando conocimiento, investigación y colaboración.*
  
