# DevOps Technical Interview Suite

Repositorio de evaluaciones técnicas estructuradas para tres niveles de seniority DevOps.
Cada carpeta es una prueba auto-contenida con instrucciones, tareas, plantillas y rúbrica de scoring.

---

## Niveles de evaluación

| Carpeta | Nivel | Stack principal |
|---------|-------|----------------|
| [`01-principiante/`](./01-principiante/) | Principiante | Azure · GitHub Actions · Docker · Minikube · Java/Quarkus |
| [`02-competente/`](./02-competente/) | Competente | Todo lo anterior + Seguridad · Helm · Tipos de Actions · Observabilidad |
| [`03-proficiente/`](./03-proficiente/) | Proficiente | Todo lo anterior + Terraform · GitOps · Liderazgo · ADRs · SLOs |

---

## Proceso de evaluación

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       FLUJO COMPLETO DE EVALUACIÓN                         │
└─────────────────────────────────────────────────────────────────────────────┘

  1. ASIGNACIÓN          2. EJECUCIÓN           3. ENTREGA              4. REVISIÓN
  ──────────────         ────────────           ──────────              ───────────
  El evaluador           El candidato           El candidato            Sesión en vivo
  comparte la            trabaja de             comparte el             con el evaluador
  carpeta del            forma autónoma         repositorio             (30–60 min)
  nivel asignado    →    en su entorno     →    antes de la       →    Preguntas técnicas
                         local                  sesión                  + scoring
                         (ver README)
```

### Paso 1 — Asignación del nivel

El evaluador determina el nivel a evaluar según la experiencia declarada del candidato:

| Experiencia declarada | Nivel sugerido |
|-----------------------|---------------|
| 0–2 años, perfil junior | Principiante |
| 2–4 años, trabajo con pipelines y nube | Competente |
| 4+ años, liderazgo técnico o arquitectura | Proficiente |

> Un candidato puede ser evaluado en un nivel superior al sugerido si así lo solicita.
> Si no supera un nivel, no se avanza al siguiente en el mismo proceso.

### Paso 2 — Ejecución de la prueba

El candidato:
1. Lee el `README.md` de la carpeta asignada **completo** antes de comenzar
2. Configura su entorno local según la sección _Requisitos previos_
3. Crea un repositorio Git personal (GitHub recomendado)
4. Resuelve cada tarea en el tiempo estimado indicado
5. Documenta decisiones técnicas en su propio `README.md`

> **Regla:** el candidato trabaja de forma **completamente autónoma**. No se permiten
> consultas al evaluador durante la fase de ejecución. Puede usar documentación oficial,
> internet y herramientas de IA — lo que importa es que pueda explicar y defender su solución.

### Paso 3 — Entrega

El candidato comparte:
- **Link al repositorio** (público o con acceso al evaluador)
- **Link a la ejecución del workflow** en GitHub Actions (si aplica)
- Cualquier nota o aclaración en el `README.md` de su solución

La entrega debe ocurrir **al menos 1 hora antes** de la sesión de revisión.

### Paso 4 — Sesión de revisión y scoring

El evaluador:
1. Revisa el repositorio usando la [`RUBRICA-EVALUACION.md`](./01-principiante/RUBRICA-EVALUACION.md) del nivel correspondiente
2. Durante la sesión, hace preguntas técnicas sobre las decisiones tomadas
3. Puntúa cada sección y calcula el score final
4. Comparte el resultado al finalizar la sesión

---

## Sistema de scoring

### Fórmula

El score final es el **promedio ponderado** de todas las secciones:

```
Score Final = Σ (puntaje_sección × peso_sección)

Donde:
  puntaje_sección ∈ [0, 100]   (asignado por el evaluador)
  peso_sección    ∈ [0, 1]     (definido en la rúbrica de cada nivel)
  Σ pesos = 1.0                (los pesos siempre suman 100%)
```

**Ejemplo de cálculo — Nivel Principiante:**

| Sección | Peso | Puntaje obtenido | Aporte al score |
|---------|------|-----------------|-----------------|
| Containerización | 20% | 85 | 85 × 0.20 = **17.0** |
| Kubernetes | 25% | 70 | 70 × 0.25 = **17.5** |
| GitHub Actions | 35% | 90 | 90 × 0.35 = **31.5** |
| Documentación | 20% | 60 | 60 × 0.20 = **12.0** |
| **TOTAL** | **100%** | — | **78.0 / 100** ✅ |

> Resultado: **78 puntos → Aprobado** (umbral ≥ 70)

---

## Tabla de pesos por nivel

### Nivel Principiante

| Sección | Peso |
|---------|------|
| Containerización (Dockerfile multi-stage, non-root, healthcheck) | **20%** |
| Despliegue Kubernetes en Minikube (manifiestos, probes, configmap) | **25%** |
| Pipeline CI/CD con GitHub Actions (triggers, jobs encadenados, deploy Azure) | **35%** |
| Documentación (diagrama, instrucciones, decisiones técnicas) | **20%** |
| **Total** | **100%** |

### Nivel Competente

| Sección | Peso |
|---------|------|
| Seguridad en el pipeline (Trivy, SBOM, Cosign, OIDC federation) | **25%** |
| Tipos de GitHub Actions (Composite Action, Reusable Workflow, Matrix) | **20%** |
| Helm Chart + Kubernetes avanzado (NetworkPolicy, HPA, values override) | **25%** |
| Observabilidad (métricas Prometheus, alertas AlertManager) | **15%** |
| Documentación de seguridad (threat model, checklist, justificaciones) | **15%** |
| **Total** | **100%** |

### Nivel Proficiente

| Sección | Peso |
|---------|------|
| Infraestructura como Código — Terraform módulos + entornos (AKS, ACR, Key Vault) | **20%** |
| GitOps con ArgoCD (App of Apps, sync policy, ADR argumentado) | **15%** |
| Pipeline de infraestructura IaC (plan en PR, detección de cambios destructivos) | **15%** |
| Gestión de proyectos y liderazgo (roadmap 90 días, RACI, backlog) | **20%** |
| SLO + Postmortem + Runbooks operacionales | **15%** |
| Architecture Decision Records — 3 ADRs con MADR | **10%** |
| Presentación ejecutiva (FinOps, riesgos, métricas de éxito) | **5%** |
| **Total** | **100%** |

---

## Tabla de resultados y tabulación

### Escala de puntuación (aplica a todos los niveles)

| Rango | Calificación | Significado |
|-------|-------------|-------------|
| 90 – 100 | ⭐ Sobresaliente | Supera ampliamente lo esperado; candidato destacado |
| 70 – 89 | ✅ Aprobado | Cumple el nivel evaluado; apto para el rol |
| 50 – 69 | 🔄 En desarrollo | Conoce los conceptos pero la implementación es incompleta |
| < 50 | ❌ No aprobado | Brechas significativas; requiere más preparación |

### Hoja de tabulación rápida (para el evaluador)

```
┌────────────────────────────────────────────────────────────────────────────────┐
│  CANDIDATO: ___________________________   NIVEL: ___________________________  │
│  FECHA: _______________________________   EVALUADOR: ______________________   │
├───────────────────────────────────────┬────────┬───────────┬──────────────────┤
│  SECCIÓN                              │  PESO  │  PUNTAJE  │  APORTE          │
├───────────────────────────────────────┼────────┼───────────┼──────────────────┤
│  1.                                   │  ___%  │   __ /100 │  __ × 0.__ = __  │
│  2.                                   │  ___%  │   __ /100 │  __ × 0.__ = __  │
│  3.                                   │  ___%  │   __ /100 │  __ × 0.__ = __  │
│  4.                                   │  ___%  │   __ /100 │  __ × 0.__ = __  │
│  5. (si aplica)                       │  ___%  │   __ /100 │  __ × 0.__ = __  │
│  6. (si aplica)                       │  ___%  │   __ /100 │  __ × 0.__ = __  │
│  7. (si aplica)                       │  ___%  │   __ /100 │  __ × 0.__ = __  │
├───────────────────────────────────────┼────────┼───────────┼──────────────────┤
│  SCORE FINAL                          │  100%  │     —     │       __ / 100   │
└───────────────────────────────────────┴────────┴───────────┴──────────────────┘

  RESULTADO:  [ ] Sobresaliente (≥90)   [ ] Aprobado (≥70)
              [ ] En desarrollo (≥50)   [ ] No aprobado (<50)

  RECOMENDACIÓN: _______________________________________________________________
```

---

## Árbol de decisión del proceso

```
¿El candidato aprueba (≥ 70)?
│
├── SÍ ──→ ¿Es el nivel máximo evaluado?
│           │
│           ├── SÍ ──→ ✅ Apto para el rol — Continuar con HR
│           │
│           └── NO ──→ ¿Se requiere evaluar el siguiente nivel?
│                       │
│                       ├── SÍ ──→ Asignar siguiente nivel (plazo: 1 semana)
│                       └── NO ──→ ✅ Apto para el rol del nivel aprobado
│
└── NO ──→ ¿Puntaje ≥ 50?
            │
            ├── SÍ ──→ 🔄 Plan de desarrollo (re-evaluar en 30 días)
            └── NO ──→ ❌ No apto — Cerrar proceso
```

---

## Estructura del repositorio

```
.
├── README.md                          ← Este archivo (proceso + scoring)
│
├── 01-principiante/
│   ├── README.md                      ← Instrucciones + tareas + entregables
│   ├── RUBRICA-EVALUACION.md          ← Hoja de scoring para el evaluador
│   └── templates/
│       ├── Dockerfile                 ← Referencia multi-stage Java/Quarkus
│       ├── ci-cd.yml                  ← Pipeline GitHub Actions de referencia
│       └── k8s/
│           ├── deployment.yaml
│           ├── service.yaml
│           └── configmap.yaml
│
├── 02-competente/
│   ├── README.md                      ← Instrucciones + glosario de seguridad + tareas
│   ├── RUBRICA-EVALUACION.md          ← Hoja de scoring + preguntas conceptuales
│   └── templates/
│       ├── ci-cd.yml                  ← Matrix strategy + Composite Action + OIDC
│       ├── security-scan.yml          ← SAST · SCA · Trivy · SBOM · Cosign
│       ├── reusable-deploy.yml        ← workflow_call con OIDC federation
│       └── composite-action.yml      ← Composite Action reutilizable
│
└── 03-proficiente/
    ├── README.md                      ← Contexto empresarial + 7 tareas + liderazgo
    ├── RUBRICA-EVALUACION.md          ← Scoring + bonus sesión de revisión
    └── templates/
        ├── platform-deploy.yml        ← Terraform IaC pipeline con PR comments
        ├── gitops/
        │   ├── app-of-apps.yaml       ← ArgoCD App of Apps
        │   └── devops-app.yaml        ← ArgoCD Application + selfHeal
        └── docs/
            └── ADR-001-gitops-vs-push.md ← Plantilla MADR de ejemplo
```

---

> **Preguntas sobre el proceso de evaluación?** Contactar al equipo de contratación.
