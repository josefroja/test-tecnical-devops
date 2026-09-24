# Nivel 1 — Principiante DevOps

## Descripción general

Esta prueba evalúa competencias básicas de un profesional DevOps que trabaja
con el ecosistema **Azure + GitHub + Docker + Minikube + Java/Quarkus**.

El objetivo no es que todo funcione perfectamente al primer intento, sino evaluar
que el candidato entiende los conceptos, sabe cómo estructurar una solución y
puede razonar sobre sus decisiones.

---

## Requisitos previos (tu entorno)

Antes de comenzar asegúrate de tener instalado y funcionando:

| Herramienta | Versión mínima | Verificación |
|-------------|----------------|--------------|
| Git | 2.x | `git --version` |
| Docker Desktop | 24.x | `docker version` |
| Minikube | 1.32+ | `minikube version` |
| kubectl | 1.28+ | `kubectl version --client` |
| Java JDK | 17 o 21 | `java -version` |
| Maven o Quarkus CLI | 3.x / latest | `mvn -v` o `quarkus version` |
| Cuenta GitHub | — | Repositorio público o privado compartido |
| Cuenta Azure | Free tier suficiente | Azure Portal accesible |

> **Nota:** Si no tienes acceso a Azure, documenta los pasos que seguirías
> y usa Docker Hub o GitHub Container Registry como alternativa.

---

## Estructura esperada del repositorio entregable

```
mi-solucion/
├── app/                        # Código fuente Quarkus
│   ├── src/
│   └── pom.xml
├── docker/
│   └── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── configmap.yaml
├── .github/
│   └── workflows/
│       └── ci-cd.yml           # Pipeline principal
└── README.md                   # Instrucciones para ejecutar tu solución
```

---

## Tareas

### Tarea 1 — Containerización de la aplicación (20 pts)

1. Crea una aplicación **Quarkus** mínima con un endpoint REST:
   - `GET /health` → retorna `{ "status": "ok", "version": "1.0.0" }`
   - `GET /greet/{name}` → retorna `{ "message": "Hello, {name}!" }`

2. Escribe un [`Dockerfile`](../01-principiante/docker/Dockerfile) multi-stage que:
   - Use `maven:3.9-eclipse-temurin-21` para compilar
   - Use `eclipse-temurin:21-jre-alpine` para la imagen final
   - La imagen final no debe superar **250 MB**
   - El proceso corra como usuario **no-root**

3. Construye la imagen localmente y demuestra que los endpoints responden:
   ```bash
   docker build -t devops-app:1.0.0 .
   docker run -p 8080:8080 devops-app:1.0.0
   curl http://localhost:8080/health
   ```

**Entregable:** `docker/Dockerfile` + evidencia (screenshot o log) en tu README.

---

### Tarea 2 — Despliegue local con Minikube (25 pts)

1. Inicia un cluster Minikube:
   ```bash
   minikube start --cpus=2 --memory=2048
   ```

2. Carga tu imagen en Minikube:
   ```bash
   minikube image load devops-app:1.0.0
   ```

3. Crea los manifiestos Kubernetes en `k8s/`:
   - **`deployment.yaml`**: 2 réplicas, liveness probe en `/health`, resource limits definidos
   - **`service.yaml`**: tipo `NodePort` o `ClusterIP` con port-forward documentado
   - **`configmap.yaml`**: al menos una variable de entorno externalizada (ej. `APP_ENV=dev`)

4. Aplica los manifiestos y verifica:
   ```bash
   kubectl apply -f k8s/
   kubectl get pods
   kubectl get services
   kubectl port-forward svc/devops-app 8080:8080
   ```

**Entregable:** carpeta `k8s/` con los 3 manifiestos + salida de `kubectl get pods` en tu README.

---

### Tarea 3 — Pipeline CI/CD con GitHub Actions (35 pts)

Crea el archivo `.github/workflows/ci-cd.yml` que realice lo siguiente:

#### 3.1 — Trigger (5 pts)
El workflow debe ejecutarse en:
- `push` a la rama `main`
- `push` a ramas `feature/**`
- `pull_request` hacia `main`

#### 3.2 — Job: `build-and-test` (10 pts)
```yaml
# El job debe:
# - Usar ubuntu-latest
# - Hacer checkout del código
# - Configurar Java 21
# - Ejecutar: mvn clean verify
# - Publicar el reporte de tests como artifact
```

#### 3.3 — Job: `build-image` (10 pts)
```yaml
# El job debe (solo en push a main):
# - Depender de build-and-test
# - Construir la imagen Docker
# - Hacer push a GitHub Container Registry (ghcr.io)
# - Usar el SHA del commit como tag de la imagen
```

#### 3.4 — Job: `deploy-azure` (10 pts)
```yaml
# El job debe (solo en push a main):
# - Depender de build-image
# - Autenticarse en Azure usando un Service Principal (secret en GitHub)
# - Desplegar en Azure Container Instances o Azure Web App for Containers
# - Mostrar la URL del recurso desplegado como output
```

> Si no tienes acceso a Azure, implementa `deploy-azure` comentado y explica
> en tu README qué secrets necesitarías y por qué.

**Entregable:** archivo `.github/workflows/ci-cd.yml` funcional + link a una ejecución exitosa del workflow.

---

### Tarea 4 — Documentación y criterios de calidad (20 pts)

Tu `README.md` debe incluir:

1. **Diagrama de flujo** del pipeline (puede ser texto ASCII o Mermaid) (5 pts)
2. **Instrucciones de ejecución local** paso a paso (5 pts)
3. **Variables de entorno requeridas** y su propósito (5 pts)
4. **Decisiones técnicas tomadas**: ¿Por qué usaste ese base image? ¿Por qué esa cantidad de réplicas? (5 pts)

---

## Rúbrica de evaluación

| Sección | Peso | Criterios clave |
|---------|------|-----------------|
| Tarea 1 — Containerización | 20% | Dockerfile multi-stage, imagen < 250MB, usuario no-root |
| Tarea 2 — Minikube / K8s | 25% | Manifiestos correctos, probes definidas, configmap usado |
| Tarea 3 — GitHub Actions CI/CD | 35% | Triggers correctos, jobs encadenados, push a registry, deploy |
| Tarea 4 — Documentación | 20% | Diagrama, instrucciones claras, justificación de decisiones |
| **Total** | **100%** | **Aprobado ≥ 70%** |

### Puntuación detallada por sección

Cada sección se evalúa con esta escala:

| Puntaje | Descripción |
|---------|-------------|
| 90–100 | Supera lo esperado, implementación robusta y bien documentada |
| 70–89 | Cumple los requisitos con mínimos detalles por pulir |
| 50–69 | Implementación parcial, conceptos comprendidos pero incompleto |
| < 50 | No cumple los requisitos mínimos o hay errores conceptuales graves |

---

## Tiempo estimado

| Tarea | Tiempo sugerido |
|-------|-----------------|
| Tarea 1 | 45 min |
| Tarea 2 | 60 min |
| Tarea 3 | 90 min |
| Tarea 4 | 30 min |
| **Total** | **~3.5 horas** |

---

## Entregable final

1. **Repositorio GitHub** con todo el código (público o compartido con el evaluador)
2. **Link a la ejecución del workflow** en GitHub Actions
3. **README.md** completo en la raíz del repositorio

> Comparte el repositorio **antes** de la sesión de revisión técnica.
