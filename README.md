# Helm Chart para Metabase

Este es un chart de Helm para desplegar Metabase en Kubernetes con soporte para PostgreSQL.

## Características

- ✅ **Compatible con ArgoCD**: Usa versiones semver válidas
- ✅ **Configuración de Base de Datos**: Soporte para PostgreSQL externo
- ✅ **Seguridad**: Gestión de credenciales mediante Secrets
- ✅ **Escalabilidad**: Configuración de recursos y autoscaling
- ✅ **Ingress**: Soporte para exposición externa

## Instalación

### Usando Helm directamente

```bash
helm install metabase . \
  --set database.host="tu-postgresql-host" \
  --set database.password="tu-password"
```

### Usando ArgoCD

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: metabase
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/Yolabn/helm-metabase.git
    targetRevision: HEAD
    path: .
    helm:
      parameters:
        - name: database.host
          value: "postgresql.default.svc.cluster.local"
  destination:
    server: https://kubernetes.default.svc
    namespace: metabase
```

## Configuración

### Valores Principales

| Parámetro | Descripción | Valor por Defecto |
|-----------|-------------|-------------------|
| `image.tag` | Versión de Metabase | `v0.50.0` |
| `database.host` | Host de PostgreSQL | `postgresql.default.svc.cluster.local` |
| `database.existingSecret` | Secret con credenciales | `metabase-database` |
| `ingress.enabled` | Habilitar Ingress | `false` |

### Secret de Base de Datos

Crea un Secret con las credenciales de PostgreSQL:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: metabase-database
type: Opaque
stringData:
  user: "postgres"
  password: "tu-password"
```

## Versión

- **Chart Version**: 1.0.0
- **App Version**: 0.50.0
- **Última Actualización**: Octubre 2025

## Soporte

Para problemas o preguntas, abre un issue en este repositorio.
