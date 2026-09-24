# Hoja de evaluación — Nivel Proficiente

## Candidato
- **Nombre:**
- **Fecha:**
- **Evaluador:**
- **Tiempo utilizado:**
- **Modalidad:** Tarea + sesión de revisión de 60 min

---

## Sección 1 — Terraform IaC (Peso: 20%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Módulo `aks/` con RBAC, Managed Identity, Network Policy | 20 | | |
| Módulo `acr/` con role assignment AKS→ACR | 15 | | |
| Módulo `key-vault/` con purge protection y expiración de secretos | 15 | | |
| `environments/dev` y `environments/prod` con variables diferenciadas | 20 | | |
| Remote state en Azure Blob Storage configurado | 15 | | |
| ADR sobre estrategia de entornos (workspaces vs. carpetas) | 15 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 1:** _____ / 100  
**Puntuación ponderada (×0.20):** _____

---

## Sección 2 — GitOps ArgoCD (Peso: 15%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| `app-of-apps.yaml` correcto con finalizers | 25 | | |
| `devops-app.yaml` apuntando al Helm Chart con values override | 25 | | |
| `selfHeal: true` y `prune: true` configurados | 20 | | |
| ADR-001 bien argumentado (contexto, decisión, alternativas) | 30 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 2:** _____ / 100  
**Puntuación ponderada (×0.15):** _____

---

## Sección 3 — IaC Pipeline (Peso: 15%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Paths filter correctamente configurado | 15 | | |
| `terraform fmt`, `validate`, `plan` ejecutados | 25 | | |
| Plan publicado como comentario en PR | 25 | | |
| Detección de cambios destructivos antes de apply | 20 | | |
| Apply solo en `main` y sin cambios destructivos | 15 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 3:** _____ / 100  
**Puntuación ponderada (×0.15):** _____

---

## Sección 4 — Gestión de proyectos y liderazgo (Peso: 20%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Plan de 90 días con 3 sprints y criterios de Done | 30 | | |
| Plan de rollback documentado | 15 | | |
| Matriz RACI con todos los roles y actividades | 25 | | |
| Sprint 1 backlog con mínimo 8 ítems (incl. enablers y deuda técnica) | 30 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 4:** _____ / 100  
**Puntuación ponderada (×0.20):** _____

---

## Sección 5 — SLO + Postmortem + Runbooks (Peso: 15%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| 3 SLOs con SLIs y error budget calculado | 35 | | |
| Postmortem template con 5 Whys y action items | 30 | | |
| Runbook con árbol de decisión y 3 escenarios de incidente | 35 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 5:** _____ / 100  
**Puntuación ponderada (×0.15):** _____

---

## Sección 6 — Architecture Decision Records (Peso: 10%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| ADR-001 (GitOps vs push): contexto claro, consecuencias reales | 35 | | |
| ADR-002 (AKS vs ACI): trade-offs técnicos y económicos | 30 | | |
| ADR-003 (Secrets strategy): OIDC, Key Vault, CSI driver | 35 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 6:** _____ / 100  
**Puntuación ponderada (×0.10):** _____

---

## Sección 7 — Presentación ejecutiva (Peso: 5%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Estado actual vs. objetivo claro | 20 | | |
| Riesgos y mitigaciones identificados | 25 | | |
| Métricas de éxito definidas | 25 | | |
| Estimación de costos (FinOps) incluida | 15 | | |
| Comunicación clara para audiencia no técnica | 15 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 7:** _____ / 100  
**Puntuación ponderada (×0.05):** _____

---

## Evaluación de la sesión de revisión (Bonus — hasta 15 pts extra)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Justificó decisiones de arquitectura con claridad | 5 | | |
| Respondió preguntas de escalabilidad con criterio | 5 | | |
| Demostró pensamiento crítico ("haría X diferente") | 5 | | |
| **Bonus total** | **15** | | |

---

## Resultado final

| Sección | Peso | Puntaje (0–100) | Puntaje ponderado |
|---------|------|-----------------|-------------------|
| 1 — Terraform IaC | 20% | | |
| 2 — GitOps ArgoCD | 15% | | |
| 3 — IaC Pipeline | 15% | | |
| 4 — Liderazgo / PM | 20% | | |
| 5 — SLO + Runbooks | 15% | | |
| 6 — ADRs | 10% | | |
| 7 — Presentación ejecutiva | 5% | | |
| **TOTAL** | **100%** | — | **___ / 100** |
| Bonus sesión de revisión | — | — | + ___ |
| **TOTAL FINAL** | | | **___ / 100** |

### Nivel alcanzado

- [ ] **No aprobado** — Puntaje < 50
- [ ] **En desarrollo** — Puntaje 50–69
- [ ] **Aprobado (Proficiente)** — Puntaje ≥ 70
- [ ] **Sobresaliente** — Puntaje ≥ 90

---

## Observaciones del evaluador

```
[Agregar comentarios cualitativos aquí — especialmente sobre habilidades de liderazgo,
pensamiento sistémico y comunicación]
```

## Recomendación

- [ ] Apto para rol Senior DevOps / Lead DevOps Engineer
- [ ] Apto con plan de desarrollo en áreas específicas: _______________
- [ ] Repetir evaluación Proficiente en 60 días
- [ ] No apto para rol de liderazgo en este momento
