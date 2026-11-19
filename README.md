# ALUMGLASS

Sistema web para la gestión de una empresa de aluminio y vidrio (**Alum Glass**).
El repositorio está organizado como un monorepo con dos aplicaciones independientes:

- **`backend/`** — API y panel de administración con **Django** (+ Django REST Framework).
- **`frontend/`** — interfaz de usuario con **Next.js** (App Router) + React + TypeScript.

> Estado actual: proyecto en fase inicial. El backend solo expone el admin de Django
> (`/admin/`) y el frontend muestra una página de bienvenida.

---

## Tabla de contenido

1. [Stack y versiones](#stack-y-versiones)
2. [Estructura de carpetas](#estructura-de-carpetas)
3. [Requisitos previos](#requisitos-previos)
4. [Instalación](#instalación)
5. [Ejecución en desarrollo](#ejecución-en-desarrollo)
6. [Variables de entorno](#variables-de-entorno)
7. [Scripts disponibles](#scripts-disponibles)
8. [Convenciones y notas](#convenciones-y-notas)

---

## Stack y versiones

Las versiones se obtuvieron del entorno real del proyecto (entorno virtual `venv/`
y `frontend/package.json` / `package-lock.json`).

### Backend

| Herramienta | Versión | Uso |
| --- | --- | --- |
| Python | `3.14.10` | Lenguaje del backend (venv creado con uv) |
| Django | `6.1.3` | Framework web principal |
| Django REST Framework | `3.18.3` | API REST (instalado, aún no registrado en `INSTALLED_APPS`) |
| django-cors-headers | `4.9.0` | CORS para el frontend (instalado, aún no configurado) |
| psycopg | `3.3.9` | Driver PostgreSQL (psycopg 3, con `psycopg-binary`) |
| python-dotenv | `1.2.4` | Carga de variables desde `.env` |
| tzdata | `2026.6` | Base de datos de zonas horarias |
| pip | `26.2.2` | Gestor de paquetes del venv |
| SQLite | incluido en Python | Motor de BD actual (`backend/db.sqlite3`) |

### Frontend

| Herramienta | Versión | Uso |
| --- | --- | --- |
| Node.js | `22.22.2` | Runtime de JavaScript |
| npm | `11.7.0` | Gestor de paquetes |
| Next.js | `16.4.6` | Framework (App Router + Turbopack) |
| React | `19.3.0` | Librería de UI |
| React DOM | `19.3.0` | Renderizador web |
| TypeScript | `^5` | Tipado estático |
| Tailwind CSS | `4.3.3` | Utilidades CSS (instalado, aún sin configurar en `globals.css`) |
| @tailwindcss/turbopack | `4.3.3` | Integración de Tailwind con Turbopack |
| ESLint | `^9` | Linter |
| eslint-config-next | `16.4.6` | Reglas de ESLint para Next.js |
| @types/node | `^20` | Tipos de Node.js |
| @types/react | `^19` | Tipos de React |
| @types/react-dom | `^19` | Tipos de React DOM |

### Configuración relevante de Next.js

`frontend/next.config.ts` habilita:

- `cacheComponents: true`
- `partialPrefetching: true`
- `experimental.agentFeedback: true`
- Regla de Turbopack para compilar CSS con `@tailwindcss/turbopack`.

---

## Estructura de carpetas

```text
ALUMGLASS/
├── .gitignore                     # Ignorados globales del monorepo
├── README.md                      # Este archivo
│
├── backend/                       # Aplicación Django
│   ├── manage.py                  # Utilidad de línea de comandos de Django
│   ├── requirements.txt           # Dependencias de Python (versiones fijadas)
│   ├── db.sqlite3                 # Base de datos local (no se versiona)
│   └── mysite/                    # Proyecto/paquete Django
│       ├── __init__.py
│       ├── settings.py            # Configuración (BD, apps, i18n, email)
│       ├── urls.py                # Rutas raíz (solo admin/ por ahora)
│       ├── asgi.py                # Punto de entrada ASGI
│       └── wsgi.py                # Punto de entrada WSGI
│
├── frontend/                      # Aplicación Next.js
│   ├── app/                       # App Router
│   │   ├── globals.css            # Estilos globales (actualmente vacío)
│   │   ├── layout.tsx             # Layout raíz + fuentes Geist
│   │   └── page.tsx               # Página principal ("Alum Glass")
│   ├── public/                    # Recursos estáticos (SVG de ejemplo)
│   ├── next.config.ts             # Configuración de Next.js
│   ├── tsconfig.json              # Configuración de TypeScript (alias @/*)
│   ├── eslint.config.mjs          # Configuración plana de ESLint
│   ├── next-env.d.ts              # Tipos generados por Next.js
│   ├── package.json               # Dependencias y scripts
│   ├── package-lock.json          # Lockfile de npm
│   ├── AGENTS.md                  # Reglas para agentes de IA (generado por Next)
│   ├── README.md                  # README original de create-next-app
│   ├── .gitignore                 # Ignorados propios del frontend
│   ├── node_modules/              # Dependencias instaladas (no se versiona)
│   └── .next/                     # Build/caché de Next.js (no se versiona)
│
└── venv/                          # Entorno virtual de Python (no se versiona)
    ├── Include/
    ├── Lib/                       # Paquetes instalados (Django, DRF, etc.)
    └── Scripts/                   # python, pip, django-admin, activate...
```

### Capas y responsabilidades

| Carpeta | Responsabilidad |
| --- | --- |
| `backend/mysite/` | Configuración global de Django. Aquí se registran apps, middleware, BD y rutas. |
| `backend/` (raíz) | Punto de entrada (`manage.py`) y base de datos de desarrollo. |
| `frontend/app/` | Rutas y componentes con el App Router de Next.js. Cada carpeta es un segmento de URL. |
| `frontend/public/` | Archivos servidos tal cual desde `/`. |
| `venv/` | Aislamiento de dependencias de Python. |

---

## Requisitos previos

- **Python 3.14+** (probado con 3.14.10)
- **Node.js 22+** (probado con 22.22.2) y npm 11+
- Git

---

## Instalación

### 1. Clonar el repositorio

```bash
git clone <URL_DEL_REPOSITORIO>
cd ALUMGLASS
```

### 2. Backend

El entorno virtual `venv/` ya existe en el proyecto. Si necesitas recrearlo:

```bash
python -m venv venv
```

Activarlo (Windows / PowerShell):

```powershell
.\venv\Scripts\Activate.ps1
```

Activarlo (Windows / CMD):

```bat
venv\Scripts\activate.bat
```

Activarlo (Linux / macOS):

```bash
source venv/bin/activate
```

Instalar dependencias y preparar la base de datos:

```bash
pip install -r backend/requirements.txt

cd backend
python manage.py migrate
python manage.py createsuperuser   # opcional, para el admin
```

Contenido de `backend/requirements.txt` (versiones fijadas):

```text
Django==6.1.3
djangorestframework==3.18.3
django-cors-headers==4.9.0
psycopg[binary]==3.3.9
python-dotenv==1.2.4
tzdata==2026.6
```

> Para actualizar el archivo tras instalar algo nuevo:
> `pip freeze > backend/requirements.txt` (con el venv activo).

### 3. Frontend

```bash
cd frontend
npm install
```

---

## Ejecución en desarrollo

Se necesitan **dos terminales**.

**Backend** (por defecto en `http://127.0.0.1:8000`):

```bash
# con el venv activo, desde la raíz del repo
cd backend
python manage.py runserver
```

- Admin de Django: <http://127.0.0.1:8000/admin/>

**Frontend** (por defecto en `http://localhost:3000`):

```bash
cd frontend
npm run dev
```

---

## Variables de entorno

La raíz `.gitignore` excluye los archivos `.env*` (salvo `.env.example`), por lo que
las credenciales nunca se versionan.

Sugerencia de archivos:

- `backend/.env` — usado con `python-dotenv` (`SECRET_KEY`, `DEBUG`, `ALLOWED_HOSTS`,
  `DATABASE_URL`, etc.).
- `frontend/.env.local` — variables públicas (`NEXT_PUBLIC_*`) y secretos de servidor.

> Actualmente `settings.py` tiene `SECRET_KEY` y `DEBUG = True` escritos en duro.
> Antes de desplegar, muévelos a variables de entorno y establece
> `ALLOWED_HOSTS` correctamente.

---

## Scripts disponibles

### Frontend (`frontend/package.json`)

| Comando | Descripción |
| --- | --- |
| `npm run dev` | Servidor de desarrollo (`next dev`, Turbopack) |
| `npm run build` | Build de producción |
| `npm run start` | Sirve el build de producción |
| `npm run lint` | Ejecuta ESLint |

### Backend

| Comando | Descripción |
| --- | --- |
| `python manage.py runserver` | Servidor de desarrollo |
| `python manage.py migrate` | Aplica migraciones |
| `python manage.py makemigrations` | Genera migraciones |
| `python manage.py createsuperuser` | Crea usuario administrador |
| `python manage.py shell` | Consola interactiva de Django |

---

## Convenciones y notas

- **App Router**: en `frontend/app/`, `page.tsx` define la vista de un segmento de URL
  y `layout.tsx` el layout compartido. Los alias de importación usan `@/*` → `frontend/*`.
- **Estilos**: `app/globals.css` está vacío; Tailwind CSS 4.3.3 ya está instalado pero
  todavía no se importa (`@import "tailwindcss";`). La configuración usa el plugin de
  Turbopack, no `postcss.config.mjs`.
- **Idioma/zona horaria del backend**: `LANGUAGE_CODE = 'en-us'`, `TIME_ZONE = 'UTC'`.
  Ajustar a `es-*` y la zona local cuando se definan los requerimientos.
- **Apps Django**: todavía no hay apps de dominio creadas. Cuando se agreguen, usar
  `python manage.py startapp <nombre>` dentro de `backend/` y registrarlas en
  `INSTALLED_APPS`.
- **REST Framework y CORS**: están instalados pero no registrados en `INSTALLED_APPS`
  ni en `MIDDLEWARE`. Se deben habilitar antes de exponer la API al frontend.
- **Repositorio Git anidado**: `frontend/.git` existe porque `create-next-app` inicializó
  su propio repositorio. Si el monorepo se versiona desde la raíz, conviene eliminarlo:
  `Remove-Item -Recurse -Force frontend/.git` (PowerShell) y luego `git init` en la raíz.
- **Ignorados**: el `.gitignore` de la raíz cubre Python/venv, Django, Node/Next.js,
  variables de entorno, logs y archivos del sistema operativo/IDE.
