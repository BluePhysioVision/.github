# 🫐 BluePhysioVision

> **Sistema de Análisis Automatizado de Salud Vegetal (Arándanos)**
> Grupo de Investigación GISTFA - Universidad de Cundinamarca

Bienvenido a la organización oficial del proyecto BluePhysioVision. Este sistema utiliza IoT, Edge Computing e Inteligencia Artificial para monitorear el crecimiento y fisiología de plantas de arándano.

---

## 🗺️ Mapa de Navegación del Proyecto

El código está modularizado para mantener el orden. Por favor, dirígete al repositorio correspondiente según el área de trabajo:

### 📱 Frontend & Web
| Repositorio | Descripción | Tecnologías |
| :--- | :--- | :--- |
| **[bp-frontend-web](https://github.com/BluePhysioVision/bp-frontend-web)** | Dashboard interactivo y gestión de usuarios. | React, TypeScript, Tailwind |
| **[bp-raspberry-ui](https://github.com/BluePhysioVision/bp-raspberry-ui)** | Interfaz para el dispositivo Raspberry Pi local. | React, Vite, ShadcnUI |

### 🧠 Backend & Cloud
| Repositorio | Descripción | Tecnologías |
| :--- | :--- | :--- |
| **[bp-backend-cloud](https://github.com/BluePhysioVision/bp-backend-web)** | API Central, Base de datos y Procesamiento Pesado. | Python (FastAPI), Supabase |
| **[bp-edge-computing](https://github.com/BluePhysioVision/bp-Raspberry_server)** | Servidor local (Raspberry Pi) y orquestación. | Python, Docker, MQTT |

### 🔌 Hardware & Firmware
| Repositorio | Descripción | Tecnologías |
| :--- | :--- | :--- |
| **[bp-firmware-esp32](https://github.com/BluePhysioVision/bp-firmware-esp32)** | Código embebido para cámaras de captura. | C++, PlatformIO |

### 📚 Documentación Central
| Repositorio | Descripción | Contenido |
| :--- | :--- | :--- |
| **[bp-documentation](https://github.com/BluePhysioVision/bp-documentation)** | 🛑 **LEER PRIMERO**. Manuales, investigación y guías. | PDFs, Diseños, Docs |

---

## �️ Resumen de Prácticas de Desarrollo

Para garantizar la calidad y homogeneidad del código, seguimos estos estándares esenciales:

### 1. Convenciones de Código
- **Backend (Python)**: Seguimos **PEP 8**.
  - `snake_case` para archivos, funciones y variables.
  - `PascalCase` para clases.
- **Frontend (React)**: Seguimos **Airbnb Style Guide**.
  - `PascalCase` para Componentes e Interfaces.
  - `camelCase` para funciones, variables y hooks.
  - Componentes funcionales con TypeScript.

### 2. Protocolo de Commits
Usamos **[Conventional Commits](https://www.conventionalcommits.org/)**: `<tipo>(<alcance>): <descripción>`

| Tipo | Uso Común | Ejemplo |
|:---|:---|:---|
| `feat` | Nueva funcionalidad | `feat(auth): add login with google` |
| `fix` | Corrección de errores | `fix(nav): resolve menu collapse issue` |
| `docs` | Cambios en documentación | `docs(readme): update setup steps` |
| `chore`| Mantenimiento (build, deps)| `chore(package): upgrade react version` |

---

## 🛣️ Rutas Recomendadas (Workflow)

Seguimos una metodología estricta para el control de versiones:

1.  **Rama Principal protegida**: Nunca hagas commit directo en `main`.
2.  **Ramas de Funcionalidad**:
    - Crea una rama para cada tarea: `feature/nombre-tarea` o `fix/nombre-error`.
    - Ejemplo: `git checkout -b feature/nueva-camara`.
3.  **Proceso de Integración**:
    - Haz tus cambios y commits siguiendo la convención.
    - Haz Push de tu rama: `git push origin feature/nueva-camara`.
    - Abre un **Pull Request (PR)** hacia `main` para revisión.

> **Nota**: Antes de empezar, asegúrate de tener clonado el repositorio `bp-documentation` y leer el archivo `ONBOARDING.md` para configurar tu entorno.

---

## 🛡️ Estado del Proyecto

![Status](https://img.shields.io/badge/Status-Development-yellow)
![License](https://img.shields.io/badge/License-MIT-blue)
