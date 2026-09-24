# Nivel 2 — Competente DevOps

## Descripción general

Este nivel asume que dominas el nivel Principiante y evalúa competencias **intermedias**:
seguridad en pipelines y contenedores, tipología avanzada de GitHub Actions (reusable
workflows, composite actions, matrix strategies), gestión de secretos, y observabilidad básica.

El candidato debe demostrar no solo que "hace funcionar" el pipeline, sino que entiende
**por qué** cada decisión técnica importa y cuáles son los riesgos de hacerlo mal.

---

## Requisitos previos (tu entorno)

Todo lo del nivel Principiante, más:

| Herramienta | Versión mínima | Verificación |
|-------------|----------------|--------------|
| Trivy (scanner de vulnerabilidades) | latest | `trivy --version` |
| Azure CLI | 2.50+ | `az version` |
| Helm | 3.x | `helm version` |
| Cosign (firma de imágenes) | 2.x | `cosign version` |
| OWASP Dependency-Check o Snyk CLI | latest | `dependency-check --version` |

---

## Glosario de términos clave (debes dominar estos conceptos)

El evaluador puede preguntarte sobre cualquiera de los siguientes durante la sesión de revisión:

| Término | Categoría |
|---------|-----------|
| SAST / DAST | Seguridad |
| SCA (Software Composition Analysis) | Seguridad |
| CVE / CVSS Score | Seguridad |
| SBOM (Software Bill of Materials) | Seguridad |
| Image signing / Cosign / Sigstore | Seguridad |
| Secrets scanning | Seguridad |
| Least privilege / OIDC federation | Seguridad |
| Reusable Workflow (`workflow_call`) | GitHub Actions |
| Composite Action | GitHub Actions |
| Matrix Strategy | GitHub Actions |
| Environment Protection Rules | GitHub Actions |
| GITHUB_TOKEN vs PAT vs OIDC | GitHub Actions |
| Workload Identity Federation | Azure / GitHub |
| Azure Key Vault + CSI driver | Azure Secrets |
| Helm Chart values override | Kubernetes/Helm |
| Network Policy en Kubernetes | Seguridad K8s |
| Pod Security Admission | Seguridad K8s |

---

## Estructura esperada del repositorio entregable

```
mi-solucion/
├── app/
│   ├── src/
│   └── pom.xml
├── docker/
│   └── Dockerfile
├── helm/
│   └── devops-app/
│       ├── Chart.yaml
│       ├── values.yaml
│       ├── values-prod.yaml
│       └── templates/
│           ├── deployment.yaml
│           ├── service.yaml
│           ├── configmap.yaml
│           ├── networkpolicy.yaml
│           └── hpa.yaml
├── .github/
│   ├── actions/
│   │   └── build-java/
│   │       └── action.yml          # Composite Action
│   └── workflows/
│       ├── ci-cd.yml               # Pipeline principal
│       ├── reusable-deploy.yml     # Reusable Workflow
│       └── security-scan.yml      # Pipeline de seguridad
├── docs/
│   └── security-posture.md        # Análisis de postura de seguridad
└── README.md
```

---

## Tareas

### Tarea 1 — Seguridad en el pipeline (25 pts)

#### 1.1 — Escaneo de imagen con Trivy (10 pts)

Agrega un job `security-scan` en `.github/workflows/security-scan.yml` que:

1. Ejecute **Trivy** sobre la imagen Docker construida
2. Falle el pipeline si existen vulnerabilidades **CRITICAL** o **HIGH**
3. Genere un reporte SARIF y lo suba a GitHub Security tab
4. Genere un **SBOM** en formato CycloneDX JSON

```yaml
# Ejemplo de integración Trivy:
- name: Run Trivy vulnerability scanner
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: ghcr.io/${{ github.repository }}/devops-app:${{ github.sha }}
    format: sarif
    output: trivy-results.sarif
    severity: CRITICAL,HIGH
    exit-code: '1'
```

**Entregable:** workflow `security-scan.yml` + screenshot del Security tab en GitHub.

#### 1.2 — Firma de imágenes con Cosign (8 pts)

Firma la imagen Docker usando `cosign` con **keyless signing** (Sigstore/OIDC):

```yaml
- name: Sign container image
  run: |
    cosign sign --yes \
      ghcr.io/${{ github.repository }}/devops-app:${{ github.sha }}
  env:
    COSIGN_EXPERIMENTAL: "true"
```

Documenta en `docs/security-posture.md`:
- ¿Qué garantiza la firma de una imagen?
- ¿Cómo verificarías la firma antes de un despliegue?

#### 1.3 — Secrets scanning y OIDC federation (7 pts)

1. Habilita **Secret Scanning** en el repositorio GitHub (screenshot)
2. Reemplaza el `AZURE_CREDENTIALS` (Service Principal con secreto) por
   **Workload Identity Federation** (OIDC) con Azure:

```yaml
permissions:
  id-token: write     # Necesario para OIDC
  contents: read

- name: Azure Login (OIDC — no secrets needed)
  uses: azure/login@v2
  with:
    client-id: ${{ secrets.AZURE_CLIENT_ID }}
    tenant-id: ${{ secrets.AZURE_TENANT_ID }}
    subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
```

Explica en tu README la diferencia entre SP con secreto y OIDC Federation.

---

### Tarea 2 — Tipos de GitHub Actions (20 pts)

#### 2.1 — Composite Action (8 pts)

Crea `.github/actions/build-java/action.yml` como una **Composite Action** que:
- Reciba como inputs: `java-version` (default: `21`), `working-directory` (default: `app`)
- Encapsule: checkout → setup-java → mvn verify → upload artifacts
- Sea reutilizable desde cualquier workflow con `uses: ./.github/actions/build-java`

#### 2.2 — Reusable Workflow (7 pts)

Crea `.github/workflows/reusable-deploy.yml` como un **Reusable Workflow** (`workflow_call`) que:
- Reciba como inputs: `environment` (string), `image-tag` (string)
- Reciba como secrets: `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`
- Realice el despliegue en el entorno indicado
- Sea invocado desde `ci-cd.yml` con:
  ```yaml
  uses: ./.github/workflows/reusable-deploy.yml
  with:
    environment: production
    image-tag: ${{ needs.build-image.outputs.image-tag }}
  ```

#### 2.3 — Matrix Strategy (5 pts)

Agrega un job en `ci-cd.yml` que use **matrix strategy** para:
- Testear la aplicación contra Java 17 y Java 21
- Testear en Linux y Windows (`ubuntu-latest`, `windows-latest`)

```yaml
strategy:
  matrix:
    java: [17, 21]
    os: [ubuntu-latest, windows-latest]
```

---

### Tarea 3 — Helm Chart y Kubernetes avanzado (25 pts)

Migra los manifiestos K8s a un **Helm Chart** en `helm/devops-app/`:

1. **`Chart.yaml`**: nombre, versión, appVersion (10 pts)
2. **`values.yaml`**: `replicaCount`, `image.repository`, `image.tag`, `resources`, `autoscaling.enabled` (5 pts)
3. **`values-prod.yaml`**: overrides para producción (2 réplicas, límites más altos, autoscaling activado) (5 pts)
4. **`templates/networkpolicy.yaml`**: ingress solo desde el namespace `ingress-nginx`, egress solo a DNS (5 pts)

Despliega en Minikube usando Helm:
```bash
helm upgrade --install devops-app helm/devops-app \
  -f helm/devops-app/values.yaml \
  --set image.tag=$(git rev-parse --short HEAD)
```

**Entregable:** directorio `helm/` completo + salida de `helm list` y `kubectl get pods`.

---

### Tarea 4 — Observabilidad básica (15 pts)

1. Agrega el **Micrometer** extension a Quarkus para exponer métricas Prometheus en `/q/metrics` (5 pts)
2. Configura el Service de Kubernetes con anotaciones para scraping de Prometheus:
   ```yaml
   annotations:
     prometheus.io/scrape: "true"
     prometheus.io/port: "8080"
     prometheus.io/path: "/q/metrics"
   ```
3. Documenta en `docs/security-posture.md` qué métricas considera críticas monitorear
   para detectar un ataque (ej. tasa de errores 4xx/5xx, latencia anómala, CPU spike) (5 pts)
4. Define al menos **2 alertas** (en YAML, formato Prometheus AlertManager) para las métricas seleccionadas (5 pts)

---

### Tarea 5 — Documentación y análisis de seguridad (15 pts)

Crea `docs/security-posture.md` con:

1. **Threat model simplificado** del pipeline CI/CD: ¿Qué podría salir mal en cada stage? (5 pts)
2. **Explicación de cada decisión de seguridad** tomada (OIDC vs SP, firma de imagen, network policy) (5 pts)
3. **Checklist de seguridad DevOps** que aplicarías en un proyecto real (mínimo 10 ítems) (5 pts)

---

## Rúbrica de evaluación

| Sección | Peso | Criterios clave |
|---------|------|-----------------|
| Tarea 1 — Seguridad en pipeline | 25% | Trivy + SBOM, Cosign keyless, OIDC federation |
| Tarea 2 — Tipos de GitHub Actions | 20% | Composite Action, Reusable Workflow, Matrix |
| Tarea 3 — Helm Chart + Network Policy | 25% | Chart completo, values override, NetworkPolicy |
| Tarea 4 — Observabilidad | 15% | Métricas Prometheus, alertas definidas |
| Tarea 5 — Documentación seguridad | 15% | Threat model, justificaciones, checklist |
| **Total** | **100%** | **Aprobado ≥ 70%** |

### Escala de puntuación

| Puntaje | Descripción |
|---------|-------------|
| 90–100 | Dominio sólido, implementación segura y bien razonada |
| 70–89 | Cumple los requisitos, algún área de mejora |
| 50–69 | Conoce los conceptos pero la implementación es incompleta |
| < 50 | Brechas significativas en seguridad o comprensión conceptual |

---

## Tiempo estimado

| Tarea | Tiempo sugerido |
|-------|-----------------|
| Tarea 1 | 90 min |
| Tarea 2 | 75 min |
| Tarea 3 | 90 min |
| Tarea 4 | 45 min |
| Tarea 5 | 45 min |
| **Total** | **~6 horas** |

---

## Entregable final

1. **Repositorio GitHub** con todo el código
2. **Link a ejecución del workflow** (incluyendo Security tab con resultados Trivy)
3. **Screenshot** de `helm list` y pods corriendo
4. `docs/security-posture.md` completo

> Comparte el repositorio **antes** de la sesión de revisión técnica.
> El evaluador puede hacerte preguntas directas sobre cualquier término del glosario.
