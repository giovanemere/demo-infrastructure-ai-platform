# 🤖 Infrastructure AI Platform

**Análisis automático de arquitecturas AWS con IA → Catálogo Backstage**

Plataforma que analiza descripciones de texto e imágenes de arquitecturas AWS usando Gemini AI, genera templates de Backstage automáticamente y los integra con GitHub para crear proyectos completos.

## 🚀 Replicación del Proyecto

### 📋 Prerrequisitos del Sistema

**Sistema Operativo:**
- Ubuntu/Debian Linux (recomendado)
- macOS (compatible)
- Windows con WSL2

**Software Requerido:**
```bash
# Node.js y Yarn
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install -g yarn

# Python y pip
sudo apt install python3 python3-pip python3-venv

# Git y herramientas básicas
sudo apt install git curl wget netcat-openbsd

# PostgreSQL
sudo apt install postgresql postgresql-contrib

# Docker (opcional para MinIO)
sudo apt install docker.io docker-compose
```

**MkDocs (REQUERIDO):**
```bash
sudo apt install -y pipx
pipx install mkdocs
pipx inject mkdocs mkdocs-material mkdocs-techdocs-core
sudo ln -sf ~/.local/bin/mkdocs /usr/local/bin/mkdocs
```

### 🔧 Configuración Inicial

#### 1. Clonar Repositorios

```bash
# Crear workspace
mkdir ~/infrastructure-ai-platform
cd ~/infrastructure-ai-platform

# Clonar repositorio principal
git clone https://github.com/tu-usuario/demos.git
cd demos

# Clonar repositorios relacionados
git clone https://github.com/tu-usuario/backstage-idp.git
git clone https://github.com/tu-usuario/infra-ai-agent.git
git clone https://github.com/tu-usuario/catalog-repo.git
git clone https://github.com/tu-usuario/templates-repo.git
```

#### 2. Configurar Variables de Entorno

**AI Agent (.env):**
```bash
cd infra-ai-agent
cp .env.example .env
# Editar .env con tus credenciales:
# GEMINI_API_KEY=tu_api_key_de_gemini
# GITHUB_TOKEN=tu_github_token
# GITHUB_CLIENT_ID=tu_github_client_id
```

**Backstage (.env):**
```bash
cd ../backstage-idp/infra-ai-backstage
cp .env.example .env
# Editar .env con tus credenciales:
# GITHUB_TOKEN=tu_github_token
# GITHUB_CLIENT_ID=tu_github_client_id
# GITHUB_CLIENT_SECRET=tu_github_client_secret
```

#### 3. Configurar Base de Datos

```bash
# Iniciar PostgreSQL
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Crear usuario y base de datos
sudo -u postgres psql -c "CREATE USER backstage WITH PASSWORD 'backstage123';"
sudo -u postgres psql -c "CREATE DATABASE backstage OWNER backstage;"
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE backstage TO backstage;"
```

#### 4. Instalar Dependencias

**AI Agent:**
```bash
cd infra-ai-agent
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**Backstage:**
```bash
cd ../backstage-idp/infra-ai-backstage
yarn install
```

### 🚀 Iniciar la Plataforma

```bash
cd ~/infrastructure-ai-platform/demos

# Verificar prerequisitos
./task-runner.sh check

# Iniciar plataforma completa
./task-runner.sh start

# Verificar estado
./task-runner.sh status
```

### 🌐 URLs de Acceso

- **AI Agent API**: http://localhost:8000
- **AI Agent Docs**: http://localhost:8000/docs
- **Backstage**: http://localhost:3000
- **Infra AI Page**: http://localhost:3000/infra-ai
- **MinIO**: http://localhost:9000
- **PostgreSQL**: localhost:5432

## 🏗️ Arquitectura del Sistema

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Frontend      │    │    AI Agent      │    │   Gemini AI     │
│   (Backstage)   │◄──►│   (FastAPI)      │◄──►│   (Google)      │
│   :3000         │    │   :8000          │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │                       │                       │
         │                       ▼                       │
         │              ┌──────────────────┐             │
         │              │   PostgreSQL     │             │
         │              │   :5432          │             │
         │              └──────────────────┘             │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   GitHub        │    │     MinIO        │    │   Templates     │
│   (Repos)       │    │   (Storage)      │    │   (Generated)   │
│                 │    │   :9000          │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

## 🔧 Comandos Principales

### Sistema de Tareas Dinámico
```bash
./task-runner.sh                    # Ver todas las tareas disponibles
./task-runner.sh start              # Iniciar plataforma completa
./task-runner.sh stop               # Detener todos los servicios
./task-runner.sh restart            # Reiniciar plataforma
./task-runner.sh status             # Verificar estado de servicios
./task-runner.sh test               # Probar conectividad
./task-runner.sh update             # Actualizar repositorios
./task-runner.sh commit             # Commit interactivo
```

### Servicios Individuales
```bash
./task-runner.sh start-ai           # Solo AI Agent
./task-runner.sh start-backstage    # Solo Backstage
./task-runner.sh restart-ai         # Reiniciar AI Agent
./task-runner.sh stop-backstage     # Detener Backstage
```

## 🧪 Uso de la Plataforma

### 1. Procesamiento de Texto
```bash
curl -X POST "http://localhost:8000/process-text" \
  -F "description=Aplicación web con S3, CloudFront y Lambda para análisis de imágenes"
```

### 2. Análisis de Imágenes
```bash
curl -X POST "http://localhost:8000/process-image" \
  -F "file=@architecture-diagram.png"
```

### 3. Interfaz Web
1. Ir a http://localhost:3000/infra-ai
2. **Procesar Arquitectura**: Ingresar descripción de texto
3. **Analizar Imagen**: Subir imagen de diagrama de arquitectura
4. Ver resultados y templates generados en Backstage

## 📁 Estructura del Proyecto

```
infrastructure-ai-platform/
├── demos/                          # Repositorio principal
│   ├── task-runner.sh              # Sistema de tareas
│   ├── start-platform.sh           # Script de inicio
│   ├── stop-platform.sh            # Script de parada
│   └── docs/                       # Documentación
├── backstage-idp/                  # Backstage IDP
│   └── infra-ai-backstage/         # Aplicación Backstage
│       ├── packages/app/src/       # Frontend personalizado
│       └── app-config.yaml         # Configuración
├── infra-ai-agent/                 # AI Agent
│   ├── agent/                      # Código principal
│   │   ├── main.py                 # API FastAPI
│   │   ├── processors/             # Procesadores IA
│   │   ├── generators/             # Generadores de templates
│   │   └── validators/             # Validadores
│   ├── venv/                       # Entorno virtual Python
│   └── requirements.txt            # Dependencias Python
├── catalog-repo/                   # Catálogo Backstage
│   ├── components/                 # Componentes
│   ├── resources/                  # Recursos
│   └── groups/                     # Grupos de usuarios
└── templates-repo/                 # Templates generados
    └── templates/                  # Templates de Scaffolder
        ├── aws-web-app/            # Template ejemplo
        │   ├── content/            # Archivos del template
        │   └── template.yaml       # Definición Scaffolder
        └── ...
```

## 🔐 Configuración de Seguridad

### GitHub Integration
1. **Crear GitHub App** en tu organización
2. **Generar tokens** con permisos de repositorio
3. **Configurar SSH keys** para acceso a repositorios
4. **Actualizar .env** con credenciales

### Gemini AI
1. **Crear proyecto** en Google Cloud Console
2. **Habilitar Gemini API**
3. **Generar API key**
4. **Configurar en .env**

## 🐛 Troubleshooting

### Problemas Comunes

**Error: Puerto ocupado**
```bash
./task-runner.sh stop
sudo lsof -ti:8000 | xargs kill -9
sudo lsof -ti:3000 | xargs kill -9
```

**Error: Base de datos**
```bash
sudo systemctl restart postgresql
./task-runner.sh restart
```

**Error: Dependencias**
```bash
cd infra-ai-agent && source venv/bin/activate && pip install -r requirements.txt
cd ../backstage-idp/infra-ai-backstage && yarn install
```

### Logs de Debugging
```bash
# AI Agent
tail -f infra-ai-agent/ai-agent.log

# Backstage
tail -f backstage.log

# PostgreSQL
sudo journalctl -u postgresql -f
```

## 📚 Documentación Adicional

- **[Arquitectura Detallada](docs/ai/architecture.md)**
- **[Guía de Desarrollo](docs/ai/components.md)**
- **[Workflows](docs/ai/workflows.md)**
- **[Troubleshooting](docs/ai/troubleshooting.md)**

## 🤝 Contribución

1. Fork el repositorio
2. Crear rama feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit cambios (`git commit -am 'Agregar nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Crear Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para detalles.

## 🆘 Soporte

- **Issues**: Reportar problemas en GitHub Issues
- **Documentación**: Ver carpeta `docs/`
- **Logs**: Usar `./task-runner.sh status` para diagnósticos

---

**Versión**: v1.2.0  
**Última actualización**: Enero 2026  
**Mantenido por**: Infrastructure AI Team
