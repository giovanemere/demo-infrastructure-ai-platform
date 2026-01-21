# 🔄 Plan de Reajuste: AI Agent ↔ Backstage Integration

## 📊 Análisis de la Estructura Actual vs Requerida

### ❌ Problemas Identificados
1. **Frontend Desconectado**: El frontend actual no se integra con Backstage
2. **Endpoints Incorrectos**: No sigue las convenciones de Backstage
3. **Estructura de Templates**: No genera templates compatibles con Scaffolder
4. **Falta de Validación**: No valida contra el esquema de Backstage
5. **Integración GitHub**: No utiliza la configuración de Backstage

### ✅ Estructura Objetivo

#### 🎯 **Frontend Integrado**
- Eliminar frontend standalone
- Crear endpoints API para Backstage
- Integrar con Scaffolder de Backstage
- Usar TechDocs para documentación

#### 🔧 **Backend Reajustado**
```
agent/
├── main.py                    # FastAPI con endpoints para Backstage
├── processors/
│   ├── text.py               # Procesamiento de texto → Templates
│   └── vision.py             # Procesamiento de imágenes → Templates
├── generators/
│   ├── scaffolder_generator.py  # Genera templates Scaffolder
│   └── catalog_generator.py     # Genera entidades de catálogo
├── integrations/
│   ├── backstage_client.py      # Cliente para APIs de Backstage
│   └── github_integration.py    # Integración GitHub via Backstage
└── validators/
    └── template_validator.py    # Validación de templates
```

## 🎯 Endpoints Requeridos

### 1. **Scaffolder Actions** (Para Backstage)
```
POST /scaffolder/actions/ai-analyze-text
POST /scaffolder/actions/ai-analyze-image
POST /scaffolder/actions/ai-generate-template
```

### 2. **API Endpoints** (Para desarrollo)
```
POST /api/analyze/text
POST /api/analyze/image
GET /api/templates
GET /api/health
```

### 3. **Webhook Endpoints** (Para GitHub)
```
POST /webhooks/github
POST /webhooks/backstage
```

## 🔄 Flujo de Integración Objetivo

```
Usuario en Backstage → Scaffolder → AI Agent API → Gemini AI → Template Generator → GitHub → Backstage Catalog
```

## 📋 Tareas de Reajuste

### Fase 1: Backend API
1. Reestructurar endpoints para Scaffolder
2. Crear generadores de templates compatibles
3. Integrar con configuración de Backstage
4. Validar templates contra esquema

### Fase 2: Integración Backstage
1. Crear Scaffolder Actions
2. Registrar en Backstage como plugin
3. Configurar TechDocs
4. Probar flujo completo

### Fase 3: Frontend (Opcional)
1. Mantener solo para desarrollo/debug
2. Crear dashboard en Backstage
3. Integrar métricas y logs
