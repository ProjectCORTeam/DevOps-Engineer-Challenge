# DevOps Challenge - Node.js Application

Una aplicación simple en Node.js/TypeScript diseñada para evaluar habilidades de DevOps en Kubernetes.

## 🚀 Quick Start

### Prerrequisitos

- Node.js 20+ 
- Docker
- **OrbStack** con Kubernetes habilitado (recomendado)
- Make

> **Nota**: Esta aplicación está optimizada para OrbStack. Las imágenes Docker locales están disponibles inmediatamente en Kubernetes sin necesidad de registry.

### Despliegue Rápido

```bash
# Instalar, construir y desplegar todo de una vez
make quick-start
```

Este comando ejecutará:
1. Instalación de dependencias
2. Build de la aplicación
3. Construcción de imagen Docker
4. Despliegue en Kubernetes
5. Port forwarding automático

## 📋 Comandos Disponibles

### Desarrollo Local
```bash
make install      # Instalar dependencias
make dev          # Ejecutar en modo desarrollo
make build        # Compilar TypeScript
make test         # Ejecutar tests
make lint         # Ejecutar linter
make type-check   # Verificar tipos
```

### Docker
```bash
make docker-build # Construir imagen Docker
make docker-run   # Ejecutar contenedor localmente
```

### Kubernetes (OrbStack)
```bash
make orbstack-check    # Verificar que OrbStack esté corriendo
make k8s-deploy        # Desplegar en Kubernetes con OrbStack
make k8s-delete        # Eliminar recursos
make k8s-logs          # Ver logs
make k8s-port-forward  # Port forwarding (localhost:8080)
make k8s-status        # Estado de recursos
make k8s-describe      # Información detallada para debug
make restart           # Reiniciar aplicación
```

> **OrbStack**: No necesitas port-forward! Los servicios son accesibles directamente desde Mac. Usa `*.k8s.orb.local` para LoadBalancer services.

### CI/CD
```bash
make ci           # Pipeline completo (install, lint, test, build)
make clean        # Limpiar artefactos
```

## 🔗 Endpoints API

Una vez desplegada, la aplicación expone:

- **GET /health** - Health check
  ```json
  {
    "status": "ok",
    "uptimeMs": 12345,
    "timestamp": "2024-01-01T12:00:00.000Z"
  }
  ```

- **GET /version** - Información de versión
  ```json
  {
    "version": "1.0.0",
    "commit": "abc123"
  }
  ```

- **POST /echo** - Echo del payload JSON (máx 64KB)
  ```json
  {
    "data": { "tu": "payload" }
  }
  ```

- **GET /metrics** - Métricas Prometheus

## 🏗️ Arquitectura

```
src/
├── index.ts              # Aplicación principal
├── config.ts             # Configuración
└── handlers/
    ├── health.ts         # Health check
    ├── version.ts        # Información de versión
    ├── echo.ts           # Echo endpoint
    └── metrics.ts        # Métricas Prometheus

k8s/
├── configmap.yaml        # Variables de entorno
├── deployment.yaml       # Despliegue de la aplicación
└── service.yaml          # Servicio ClusterIP
```

## 🔧 Configuración

Variables de entorno disponibles:

| Variable | Default | Descripción |
|----------|---------|-------------|
| `PORT` | 8080 | Puerto del servidor |
| `NODE_ENV` | development | Entorno de ejecución |
| `LOG_LEVEL` | info | Nivel de logging |
| `APP_NAME` | devops-challenge-app | Nombre de la aplicación |
| `APP_VERSION` | 1.0.0 | Versión de la aplicación |
| `GIT_COMMIT` | - | Hash del commit (opcional) |

## 🐛 Troubleshooting

### Verificar estado de Kubernetes
```bash
make k8s-status
```

### Ver logs detallados
```bash
make k8s-logs
```

### Debug completo
```bash
make k8s-describe
```

### Reiniciar aplicación
```bash
make restart
```

## 🎯 Para Candidatos DevOps

Esta aplicación está diseñada para evaluar:

1. **Diagnóstico**: Identificar problemas en manifiestos K8s
2. **Resolución**: Corregir configuraciones incorrectas
3. **Observabilidad**: Usar logs y métricas para debug
4. **Kubernetes**: Manejo de recursos y troubleshooting

### Escenarios Típicos de Evaluación

- Problemas de configuración en manifiestos
- Issues con probes de salud
- Problemas de recursos (CPU/memoria)
- Configuración incorrecta de variables de entorno
- Problemas de red y conectividad

## 📊 Métricas y Observabilidad

- **Logs estructurados** en JSON con request ID
- **Métricas Prometheus** en `/metrics`
- **Health checks** para liveness/readiness probes
- **Graceful shutdown** con timeout de 5s
- **Request tracing** con correlation IDs

## 🔒 Seguridad

- Contenedor ejecuta como usuario no-root (UID 1001)
- Security context restrictivo
- Headers de seguridad con Helmet
- Validación de input con Zod
- Límites de payload (64KB)

---

## 🆘 Ayuda

Para ver todos los comandos disponibles:
```bash
make help
```
