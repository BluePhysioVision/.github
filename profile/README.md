# 🫐 BluePhysioVision

> **Sistema IoT-Edge de Análisis Fisiológico Automatizado en Plantas de Arándano**
> Grupo de Investigación GISTFA — Universidad de Cundinamarca

BluePhysioVision es un ecosistema distribuido que combina **IoT**, **Edge Computing**, **Computer Vision (ONNX)** e **Inteligencia Artificial Generativa (RAG/LLM)** para monitorear el crecimiento y diagnosticar el estado fisiológico de cultivos de arándano de forma automatizada y no invasiva.

---

## 🏗️ Arquitectura del Sistema

```
┌─────────────────────────────────────────────────────────────────────┐
│                        USUARIO / NAVEGADOR                         │
│                  bp-frontend-web (React 19 + Vite)                 │
└──────────────────────────┬──────────────────────────────────────────┘
                           │  HTTP (JWT)
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    bp-backend-web (FastAPI)                         │
│           API Central · Auth · RBAC · Orquestador                  │
│                         PostgreSQL                                 │
├────────────┬──────────────────────────────┬─────────────────────────┤
│            │  X-Internal-Key (HTTP)       │  X-Internal-Key (HTTP)  │
│            ▼                              ▼                         │
│  ┌──────────────────────┐   ┌───────────────────────────┐          │
│  │  bp-ml-microservice  │   │    bp-llm-service         │          │
│  │  ONNX Runtime (CV)   │   │  Gemini / Ollama + Redis  │          │
│  │  Inferencia + IoU    │   │  RAG Agronómico + Cache   │          │
│  └──────────────────────┘   └───────────────────────────┘          │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                   EDGE COMPUTING (Finca / Cultivo)                  │
│          bp-Raspberry_server (FastAPI + Mosquitto MQTT)             │
│     Recibe telemetría + fotos de ESP32-CAM, sincroniza a la nube   │
├─────────────────────────────────────────────────────────────────────┤
│   ESP32-CAM #1 ──(MQTT/HTTP)──►  Raspberry Pi 5  ──(HTTP)──► API  │
│   ESP32-CAM #2 ──(MQTT/HTTP)──►                                    │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🗺️ Mapa de Repositorios

### 🧠 Backend & Cloud Services

| Repositorio | Descripción | Stack |
| :--- | :--- | :--- |
| **[bp-backend-web](https://github.com/BluePhysioVision/bp-backend-web)** | API Central: autenticación JWT + Supabase, RBAC, orquestación de microservicios, gestión de usuarios/plantaciones/capturas. | FastAPI, PostgreSQL, SQLAlchemy 2.0 Async, Alembic, Pydantic v2 |
| **[bp-ml-microservice](https://github.com/BluePhysioVision/bp-ml-microservice)** | Motor de Computer Vision: inferencia ONNX, métricas fisiológicas (área foliar, índice de verdor), sandbox de validación con IoU (Shapely). | FastAPI, ONNX Runtime, Pillow, Shapely |
| **[bp-llm-service](https://github.com/BluePhysioVision/bp-llm-service)** | Recomendaciones inteligentes: RAG agronómico sobre base de conocimiento JSON, caché semántico Redis, fallback multi-proveedor (Gemini → Ollama → reglas estáticas). | FastAPI, Redis 7+, Google Gemini 1.5, httpx |

### 📱 Frontend

| Repositorio | Descripción | Stack |
| :--- | :--- | :--- |
| **[bp-frontend-web](https://github.com/BluePhysioVision/bp-frontend-web)** | Dashboard interactivo: gestión de plantaciones, cámaras, análisis, soporte y administración de usuarios. | React 19, Vite, TailwindCSS, shadcn/ui, TanStack Query, Recharts |

### 🔌 Edge Computing & Hardware

| Repositorio | Descripción | Stack |
| :--- | :--- | :--- |
| **[bp-Raspberry_server](https://github.com/BluePhysioVision/bp-Raspberry_server)** | Edge Gateway IoT: servidor en Raspberry Pi 5 que recopila telemetría MQTT e imágenes HTTP de nodos ESP32-CAM, con aceleración FFMPEG y sincronización a la nube. | FastAPI, Mosquitto (MQTT), SQLite, Docker, OpenCV |
| **[bp-firmware-esp32](https://github.com/BluePhysioVision/bp-firmware-esp32)** | Firmware de los nodos sensores: lógica de captura de imagen, gestión de red, aprovisionamiento remoto vía MQTT y ahorro de energía para ESP32-CAM. | PlatformIO, C++, Arduino Core, PubSubClient |
| **[bp-raspberry-ui](https://github.com/BluePhysioVision/bp-raspberry-ui)** | Dashboard local de monitoreo: interfaz gráfica táctil para la Raspberry Pi instalada en la finca para visualización en tiempo real sin internet. | React 19, Vite, TailwindCSS, Shadcn/UI |

### 🤖 Agentic Orchestration & Memory

| Repositorio | Descripción | Stack |
| :--- | :--- | :--- |
| **[bp-memory](https://github.com/BluePhysioVision/bp-memory)** | Sistema de memoria persistente: almacén de observaciones, decisiones arquitectónicas y contextos para orquestación de agentes de IA (Antigravity Protocol). | Python, SQLite (FTS5), Conda, JSON |

### 📚 Documentación Central

| Repositorio | Descripción | Contenido |
| :--- | :--- | :--- |
| **[bp-documentation](https://github.com/BluePhysioVision/bp-documentation)** | 🛑 **LEER PRIMERO**. Estándares de código, commits, onboarding, diseños e investigación. | PDFs, Diagramas, Guías |

---

## 🔒 Comunicación entre Servicios

| Origen | Destino | Mecanismo | Autenticación |
| :--- | :--- | :--- | :--- |
| Frontend → Backend | HTTP REST | Bearer JWT (access + refresh) |
| Backend → ML Service | HTTP interno (Docker) | `X-Internal-Key` header |
| Backend → LLM Service | HTTP interno (Docker) | `X-Internal-Key` header |
| ESP32-CAM → Raspberry Pi | MQTT (telemetría) + HTTP (fotos) | Credenciales MQTT / API Key |
| Raspberry Pi → Backend | HTTP REST (sincronización) | Bearer JWT / API Key |

---

## 🛠️ Prácticas de Desarrollo

### Convenciones de Código

- **Backend (Python)**: PEP 8 — `snake_case` para archivos/funciones/variables, `PascalCase` para clases.
- **Frontend (React/TS)**: Airbnb Style Guide — `PascalCase` componentes/interfaces, `camelCase` funciones/hooks.
- **Linting**: Ruff (Python), ESLint (TypeScript).
- **Testing**: Pytest + httpx (backend), Vitest (frontend).

### Protocolo de Commits

Usamos **[Conventional Commits](https://www.conventionalcommits.org/)**: `<tipo>(<alcance>): <descripción>`

| Tipo | Uso | Ejemplo |
| :--- | :--- | :--- |
| `feat` | Nueva funcionalidad | `feat(auth): add login with google` |
| `fix` | Corrección de errores | `fix(nav): resolve menu collapse issue` |
| `docs` | Cambios en documentación | `docs(readme): update setup steps` |
| `refactor` | Refactorización sin cambio funcional | `refactor(api): extract base client` |
| `chore` | Mantenimiento (build, deps) | `chore(deps): upgrade fastapi to 0.115` |
| `test` | Tests nuevos o corregidos | `test(auth): add jwt expiration tests` |
| `ci` | Cambios en CI/CD | `ci(actions): add security scan workflow` |

### Flujo de Trabajo (Git Flow)

1. **`main` protegida**: Nunca hagas commit directo en `main`.
2. **Ramas de trabajo**: Crea una rama por tarea vinculada a un issue.
   - `feature/RF-07-nueva-camara`, `fix/12-login-error`, `docs/update-readme`
3. **Pull Request**: Abre PR hacia `main` con referencia al issue (`Closes #7`).
4. **Automatización**: Los workflows de proyecto mueven automáticamente los issues en el tablero Kanban (Backlog → Ready → In Progress → Code Review → Done).

> **Onboarding**: Antes de empezar, clona `bp-documentation` y lee `ONBOARDING.md`.

---

## ⚙️ CI/CD y Automatización

Cada repositorio contiene sus propios workflows de GitHub Actions:

| Workflow | Repositorios | Propósito |
| :--- | :--- | :--- |
| `project-automation.yml` | backend, frontend | Automación del tablero de proyecto (mover issues/PRs por columnas). |
| `ci.yml` | backend, raspberry | Lint + tests en cada PR. |
| `ci_validation.yml` | frontend | Build + lint en cada PR. |
| `security.yml` | backend, raspberry | Escaneo de dependencias y secretos. |
| `deploy.yml` | raspberry | Despliegue automatizado al Edge. |

---

## 🛡️ Estado del Proyecto

![Status](https://img.shields.io/badge/Status-Development-yellow)
![License](https://img.shields.io/badge/License-MIT-blue)
![Python](https://img.shields.io/badge/Python-3.11+-blue)
![React](https://img.shields.io/badge/React-19-61DAFB)
![FastAPI](https://img.shields.io/badge/FastAPI-0.109+-green)

**Universidad de Cundinamarca** · Grupo GISTFA · © 2026
