# 🎮 eSports Tournament System

> **Laboratorio 06 — Desarrollo de Aplicaciones Web**  
> Universidad Nacional de San Agustín de Arequipa · Escuela Profesional de Ingeniería de Sistemas

---

## 👥 Integrantes

| Estudiante | Código |
|---|---|
| Valdivia Flores, Manuel | — |
| Manrique Supanta, Ronald | — |
| Cahuana Vera, Diego | — |

**Semestre:** 2026-A &nbsp;·&nbsp; **Asignatura:** Desarrollo de Aplicaciones Web (2502117)

---

## 📋 Descripción

Sistema de gestión de torneos de videojuegos competitivos construido con **Django 6** y **PostgreSQL**. Permite administrar organizadores, equipos, jugadores, torneos y registros de participación a través del panel **Django Admin**, con un esquema relacional altamente indexado y optimizado para alta concurrencia.

---

## 🏗️ Arquitectura del Proyecto

```
MyWebApps/
└── MNGTournament/
    ├── models/
    │   ├── __init__.py
    │   ├── Organizer.py
    │   ├── Team.py
    │   ├── Player.py
    │   ├── Tournament.py
    │   └── PlayerTournament.py
    ├── admin.py
    ├── apps.py
    └── migrations/
```

> Los modelos están desacoplados en submódulos independientes dentro del directorio `models/`, siguiendo el principio de responsabilidad única.

---

## 🗂️ Modelo de Datos

```
Organizer ──< Tournament >── PlayerTournament >── Player
                                                      │
                                                    Team
```

| Entidad | Tabla BD | Descripción |
|---|---|---|
| `Organizer` | `organizers` | Entidad que gestiona y alberga torneos |
| `Team` | `teams` | Escuderías o clanes competitivos |
| `Player` | `players` | Jugadores de la plataforma |
| `Tournament` | `tournaments` | Eventos competitivos de videojuegos |
| `PlayerTournament` | `players_tournaments` | Relación N:M entre jugadores y torneos |

### Restricciones de Integridad

| Relación | Restricción |
|---|---|
| `Tournament → Organizer` | `ON DELETE RESTRICT` |
| `Player → Team` | `ON DELETE SET NULL` |
| `PlayerTournament → Player` | `ON DELETE CASCADE` |
| `PlayerTournament → Tournament` | `ON DELETE CASCADE` |
| `(player, tournament)` | `UNIQUE CONSTRAINT` |

---

## ⚙️ Instalación y Configuración

### 1. Clonar el repositorio

```bash
git clone https://github.com/DJGH9Wz/Laboratorio06_DAW.git
cd Laboratorio06_DAW
```

### 2. Crear y activar el entorno virtual

```bash
# Crear entorno virtual
mkdir my_env
cd my_env
virtualenv -p python3 .

# Activar (Windows PowerShell)
.\Scripts\Activate.ps1

# Activar (Linux / macOS)
source bin/activate
```

### 3. Instalar dependencias

```bash
pip install django psycopg2-binary
```

### 4. Configurar la base de datos

En `settings.py`, ajusta la conexión a PostgreSQL:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'esports_db',
        'USER': 'tu_usuario',
        'PASSWORD': 'tu_contraseña',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

### 5. Ejecutar migraciones

```bash
# Generar archivos de migración
python manage.py makemigrations MNGTournament

# Aplicar migraciones a la base de datos
python manage.py migrate
```

### 6. Crear superusuario

```bash
python manage.py createsuperuser
```

### 7. Iniciar servidor de desarrollo

```bash
python manage.py runserver
```

Accede al panel de administración en: **http://127.0.0.1:8000/admin/**

---

## 🔍 Índices de Base de Datos

El sistema declara índices explícitos en PostgreSQL para optimizar consultas frecuentes:

| Índice | Tabla | Campo(s) |
|---|---|---|
| `idx_org_status` | `organizers` | `status` |
| `idx_tea_status` | `teams` | `status` |
| `idx_pla_teamsId` | `players` | `team` |
| `idx_pla_status` | `players` | `status` |
| `idx_tou_organizersId` | `tournaments` | `organizer` |
| `idx_tou_eventDate` | `tournaments` | `eventDate` |
| `idx_tou_gameName` | `tournaments` | `gameName` |
| `idx_tou_status` | `tournaments` | `status` |
| `idx_pt_playersId` | `players_tournaments` | `player` |
| `idx_pt_tournamentsId` | `players_tournaments` | `tournament` |
| `idx_pt_score` | `players_tournaments` | `-score` (DESC) |
| `idx_pt_finalPosition` | `players_tournaments` | `finalPosition` |
| `idx_pt_status` | `players_tournaments` | `status` |

---

## 🛡️ Validaciones Implementadas

- **`validate_future_date`** — Impide registrar torneos con fechas en el pasado.
- **`MinValueValidator(2)`** — El número máximo de participantes debe ser al menos 2.
- **`unique=True`** en `email` de `Organizer` y `Player`, y en `gamertag` de `Player`.
- **`URLField`** en `Organizer.website` y `Team.logoUrl` para validación automática de URLs.

---

## 🖥️ Panel de Administración

Los 5 modelos están registrados en `admin.py` y son gestionables desde Django Admin:

- ✅ Organizers
- ✅ Teams
- ✅ Players
- ✅ Tournaments
- ✅ Player Tournaments

Los campos `created` y `modified` se automatizan con `auto_now_add` y `auto_now`, permaneciendo ocultos en los formularios.

---

## 🔗 Enlaces

| Recurso | URL |
|---|---|
| 📁 Repositorio GitHub | https://github.com/DJGH9Wz/Laboratorio06_DAW.git |
| 🎥 Video Demostrativo | https://youtu.be/nsx5D3lFRIk |

---

## 🛠️ Stack Tecnológico

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat-square&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-6.x-092E20?style=flat-square&logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Latest-4169E1?style=flat-square&logo=postgresql&logoColor=white)

---

<p align="center">
  Laboratorio 06 · Desarrollo de Aplicaciones Web · UNSA 2026-A
</p>
