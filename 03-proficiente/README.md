# Nivel 3 — Proficiente DevOps

## Descripción general

El nivel Proficiente va más allá de las habilidades técnicas. Evalúa la capacidad del
candidato para **diseñar plataformas completas**, tomar **decisiones de arquitectura**
bajo restricciones reales, liderar equipos, gestionar proyectos, y comunicar decisiones
técnicas a audiencias no técnicas.

Se espera que el candidato no solo implemente sino también **justifique**, **diseñe**
y **lidere** el proceso.

---

## Requisitos previos

Todo lo del nivel Competente, más experiencia comprobable en:

| Área | Expectativa mínima |
|------|-------------------|
| Gestión de proyectos | Experiencia con Azure Boards, Jira, o similar |
| Arquitectura de plataformas | Decisiones de diseño documentadas (ADRs) |
| Liderazgo técnico | Haber guiado al menos un equipo pequeño (2–5 personas) |
| GitOps | Conocimiento de ArgoCD o Flux |
| FinOps | Nociones de costeo de infraestructura cloud |
| Incident Management | Haber respondido o liderado un postmortem |
| SLO/SLA/SLI | Definición y medición en producción |

---

## Contexto del ejercicio

> Eres el **Lead DevOps Engineer** de una startup fintech que está migrando de una
> arquitectura monolítica (Java EE en bare-metal) a **microservicios en Azure Kubernetes
> Service (AKS)**. Tienes un equipo de 3 DevOps engineers junior/mid y un plazo de
> **90 días** para la migración. El CTO quiere una presentación ejecutiva en la semana 4.

Este contexto atraviesa todas las tareas. Tus decisiones deben ser coherentes con él.

---

## Estructura esperada del repositorio entregable

```
mi-solucion/
├── platform/
│   ├── terraform/
│   │   ├── modules/
│   │   │   ├── aks/
│   │   │   ├── acr/
│   │   │   └── key-vault/
│   │   ├── environments/
│   │   │   ├── dev/
│   │   │   └── prod/
│   │   └── backend.tf
│   └── gitops/
│       └── argocd/
│           ├── app-of-apps.yaml
│           └── apps/
│               └── devops-app.yaml
├── .github/
│   └── workflows/
│       ├── platform-deploy.yml        # IaC pipeline
│       ├── ci-cd.yml                  # App pipeline (multi-env)
│       └── incident-runbook.yml       # Automated incident response
├── docs/
│   ├── architecture-decision-records/ # ADRs
│   │   ├── ADR-001-gitops-vs-push.md
│   │   ├── ADR-002-aks-vs-aci.md
│   │   └── ADR-003-secrets-strategy.md
│   ├── runbooks/
│   │   ├── deployment-runbook.md
│   │   └── incident-response-runbook.md
│   ├── slo-definition.md
│   ├── postmortem-template.md
│   └── executive-presentation.md     # Para el CTO
├── project/
│   ├── migration-roadmap.md          # Plan de 90 días
│   ├── team-raci.md                  # Matriz RACI
│   └── sprint-backlog-sample.md      # Backlog de ejemplo
└── README.md
```

---

## Tareas

### Tarea 1 — Infraestructura como Código con Terraform (20 pts)

#### 1.1 — Módulos Terraform para Azure (12 pts)

Crea módulos Terraform reutilizables en `platform/terraform/modules/`:

**Módulo `aks/`**:
- AKS cluster con system nodepool + user nodepool
- Azure AD integration (RBAC habilitado)
- Network policy: `azure` o `calico`
- Managed Identity para el cluster
- Private cluster habilitado para prod

**Módulo `acr/`**:
- Azure Container Registry en SKU `Standard`
- Geo-replication desactivada en dev, activada en prod
- Role assignment: AKS → ACR pull permissions

**Módulo `key-vault/`**:
- Azure Key Vault con purge protection
- Access policy basada en Managed Identity del AKS
- Secrets rotados automáticamente (al menos un secret con `expiration_date`)

#### 1.2 — Entornos separados (8 pts)

Crea `environments/dev/` y `environments/prod/` con:
- `main.tf` que consuma los módulos
- `terraform.tfvars` con valores diferenciados por entorno
- `backend.tf` con remote state en Azure Blob Storage
- Workspaces Terraform o carpetas separadas (justifica tu elección en un ADR)

**Entregable:** código Terraform + `terraform plan` output (puede ser texto en README) +
ADR sobre la estrategia de entornos elegida.

---

### Tarea 2 — GitOps con ArgoCD (15 pts)

Implementa un modelo **App of Apps** con ArgoCD en `platform/gitops/argocd/`:

1. **`app-of-apps.yaml`**: Application raíz que gestiona las demás (5 pts)
2. **`apps/devops-app.yaml`**: Application para el microservicio, apuntando al Helm Chart (5 pts)
3. Configura **sync policy** con `selfHeal: true` y `prune: true` (3 pts)
4. Documenta en un ADR (`ADR-001-gitops-vs-push.md`) la decisión de usar GitOps
   vs. push-based deployment: trade-offs, cuándo uno es mejor que el otro (7 pts)

> **Nota:** No se require un cluster ArgoCD funcionando; los manifiestos correctos
> + el ADR bien argumentado son suficientes.

---

### Tarea 3 — Pipeline de infraestructura (IaC Pipeline) (15 pts)

Crea `.github/workflows/platform-deploy.yml` que:

1. Se ejecute en push a `main` solo si hay cambios en `platform/terraform/**` (3 pts)
2. Ejecute `terraform fmt -check`, `terraform validate`, `terraform plan` (5 pts)
3. En PR: publique el plan como comentario en el Pull Request (4 pts)
4. En push a `main`: ejecute `terraform apply -auto-approve` solo si el plan no tiene
   cambios destructivos (detectado con `grep "0 to destroy"`) (3 pts)

```yaml
# Tip: usar paths filter para evitar ejecuciones innecesarias
on:
  push:
    paths:
      - "platform/terraform/**"
  pull_request:
    paths:
      - "platform/terraform/**"
```

---

### Tarea 4 — Gestión de proyectos y liderazgo (20 pts)

#### 4.1 — Plan de migración 90 días (8 pts)

Crea `project/migration-roadmap.md` con:
- Plan dividido en **3 sprints de 2 semanas** + hitos intermedios
- Criterios de "Done" para cada sprint
- Dependencias entre tareas críticas
- Plan de rollback si algo falla en semana 6+

#### 4.2 — Matriz RACI (5 pts)

Crea `project/team-raci.md` con una matriz RACI para las actividades clave:
- Diseño de arquitectura
- Implementación de pipelines
- Revisión de seguridad
- Deploy a producción
- Gestión de incidentes
- Comunicación con stakeholders

Roles: Lead DevOps (tú), DevOps Junior 1, DevOps Junior 2, DevOps Mid, CTO, Security Team.

#### 4.3 — Backlog de ejemplo (7 pts)

Crea `project/sprint-backlog-sample.md` con el backlog del Sprint 1 en formato:
```
## Sprint 1 — Fundamentos de plataforma
**Goal:** AKS operativo con pipeline básico funcionando

### Historias de usuario / tareas técnicas
| ID | Título | Tipo | Story Points | Responsable | Criterio de aceptación |
|----|--------|------|-------------|-------------|------------------------|
| T-01 | ... | Task | 3 | ... | ... |
```
Mínimo 8 ítems, incluyendo al menos 2 tareas de habilitación (enablers) y 1 de deuda técnica.

---

### Tarea 5 — SLO, Postmortem y Runbooks (15 pts)

#### 5.1 — Definición de SLO (5 pts)

Crea `docs/slo-definition.md` con:
- **SLI** (Service Level Indicator) para cada SLO definido
- **SLO** (Service Level Objective): al menos 3 (disponibilidad, latencia, tasa de error)
- **Error budget** calculado para cada SLO
- Herramientas para medirlos (Azure Monitor / Prometheus)

#### 5.2 — Plantilla de Postmortem (5 pts)

Crea `docs/postmortem-template.md` siguiendo el modelo blameless postmortem con:
- Timeline del incidente
- Impacto (usuarios afectados, duración, SLO impactado)
- Root Cause Analysis (5 Whys)
- Action items con dueño y fecha

#### 5.3 — Runbook de respuesta a incidentes (5 pts)

Crea `docs/runbooks/incident-response-runbook.md` con:
- Árbol de decisión: ¿cuándo escalar?
- Pasos para los 3 incidentes más comunes en el contexto del ejercicio
  (pod crashloop, pipeline roto, secret expirado)
- Comandos kubectl/az específicos para diagnóstico rápido

---

### Tarea 6 — Architecture Decision Records (ADRs) (10 pts)

Crea **mínimo 3 ADRs** en `docs/architecture-decision-records/` usando la plantilla
MADR (Markdown Architectural Decision Records):

```markdown
# ADR-XXX — Título

## Status
[Proposed | Accepted | Deprecated | Superseded]

## Context
[Describe el problema y las restricciones]

## Decision
[Describe la decisión tomada]

## Consequences
[Trade-offs: qué se gana, qué se pierde]

## Alternatives considered
[Qué otras opciones se evaluaron y por qué se descartaron]
```

ADRs obligatorios:
- `ADR-001-gitops-vs-push.md` — GitOps (ArgoCD) vs push-based deploy
- `ADR-002-aks-vs-aci.md` — AKS vs Azure Container Instances para el workload
- `ADR-003-secrets-strategy.md` — Estrategia de gestión de secretos

---

### Tarea 7 — Presentación ejecutiva (5 pts)

Crea `docs/executive-presentation.md` simulando la presentación al CTO en la semana 4.
Debe incluir:
- Estado actual vs. estado objetivo (antes / después)
- Riesgos identificados y planes de mitigación
- Métricas de éxito del proyecto
- Costos estimados (FinOps: costo mensual estimado de AKS + ACR + Key Vault en Azure)
- Timeline hasta go-live

> El evaluador puede pedirte que "presentes" esta sección verbalmente en 5 minutos.

---

## Rúbrica de evaluación

| Sección | Peso | Criterios clave |
|---------|------|-----------------|
| Tarea 1 — Terraform IaC | 20% | Módulos, entornos, remote state, ADR de estrategia |
| Tarea 2 — GitOps ArgoCD | 15% | App of Apps, sync policy, ADR argumentado |
| Tarea 3 — IaC Pipeline | 15% | Paths filter, plan en PR, apply seguro |
| Tarea 4 — Liderazgo / PM | 20% | Roadmap 90 días, RACI, backlog Sprint 1 |
| Tarea 5 — SLO + Postmortem + Runbooks | 15% | SLIs/SLOs, error budget, runbooks operacionales |
| Tarea 6 — ADRs | 10% | 3 ADRs con contexto, decisión y consecuencias |
| Tarea 7 — Presentación ejecutiva | 5% | Comunicación clara, costos estimados, roadmap |
| **Total** | **100%** | **Aprobado ≥ 70%** |

### Escala de puntuación

| Puntaje | Descripción |
|---------|-------------|
| 90–100 | Pensamiento sistémico, decisiones bien argumentadas, excelente comunicación |
| 70–89 | Sólido técnicamente y organizacionalmente, áreas menores por madurar |
| 50–69 | Competente técnicamente pero con brechas en liderazgo o visión arquitectural |
| < 50 | No apto para un rol de liderazgo DevOps en este momento |

---

## Tiempo estimado

| Tarea | Tiempo sugerido |
|-------|-----------------|
| Tarea 1 — Terraform | 120 min |
| Tarea 2 — GitOps | 60 min |
| Tarea 3 — IaC Pipeline | 60 min |
| Tarea 4 — PM / Liderazgo | 90 min |
| Tarea 5 — SLO + Runbooks | 75 min |
| Tarea 6 — ADRs | 60 min |
| Tarea 7 — Presentación | 45 min |
| **Total** | **~9 horas** |

> Este nivel puede realizarse en 2 sesiones de trabajo. Coordina con el evaluador.

---

## Entregable final

1. **Repositorio GitHub** con toda la estructura
2. **Links a ejecuciones de workflows** (platform-deploy + ci-cd)
3. **`docs/`** completo con ADRs, SLOs, runbooks y presentación ejecutiva
4. **`project/`** con roadmap, RACI y backlog
5. Disponibilidad para una sesión de revisión de **60 minutos** donde presentarás
   tus decisiones de arquitectura y responderás preguntas del evaluador

> El evaluador hará preguntas tipo _"¿Por qué elegiste X sobre Y?"_, _"¿Cómo escalarías esto?"_,
> _"¿Qué harías diferente con más tiempo?"_
