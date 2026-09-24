# Hoja de evaluación — Nivel Competente

## Candidato
- **Nombre:**
- **Fecha:**
- **Evaluador:**
- **Tiempo utilizado:**

---

## Sección 1 — Seguridad en el pipeline (Peso: 25%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Trivy integrado y falla en CRITICAL/HIGH | 20 | | |
| SARIF subido a GitHub Security tab | 15 | | |
| SBOM generado en formato CycloneDX | 15 | | |
| Cosign keyless signing implementado | 20 | | |
| OIDC Federation (sin secreto de larga vida) | 20 | | |
| Secret Scanning habilitado (screenshot) | 10 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 1:** _____ / 100  
**Puntuación ponderada (×0.25):** _____

---

## Sección 2 — Tipos de GitHub Actions (Peso: 20%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Composite Action correctamente definida con inputs/outputs | 30 | | |
| Composite Action reutilizada desde el workflow principal | 15 | | |
| Reusable Workflow con `workflow_call` y inputs/secrets | 30 | | |
| Matrix strategy (2 Java versions × 2 OS) | 25 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 2:** _____ / 100  
**Puntuación ponderada (×0.20):** _____

---

## Sección 3 — Helm Chart + Kubernetes avanzado (Peso: 25%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| `Chart.yaml` correcto | 10 | | |
| `values.yaml` con todas las variables parametrizadas | 20 | | |
| `values-prod.yaml` con overrides de producción | 15 | | |
| `networkpolicy.yaml` con ingress/egress correctos | 25 | | |
| `hpa.yaml` con autoscaling definido | 15 | | |
| `helm upgrade --install` funcional (evidencia) | 15 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 3:** _____ / 100  
**Puntuación ponderada (×0.25):** _____

---

## Sección 4 — Observabilidad (Peso: 15%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Endpoint `/q/metrics` expuesto correctamente | 25 | | |
| Anotaciones Prometheus en el Service K8s | 25 | | |
| Métricas de seguridad identificadas y justificadas | 25 | | |
| 2 alertas en formato AlertManager YAML | 25 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 4:** _____ / 100  
**Puntuación ponderada (×0.15):** _____

---

## Sección 5 — Documentación de seguridad (Peso: 15%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Threat model del pipeline CI/CD | 35 | | |
| Justificación de cada decisión de seguridad | 35 | | |
| Checklist DevOps (mínimo 10 ítems) | 30 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 5:** _____ / 100  
**Puntuación ponderada (×0.15):** _____

---

## Preguntas de conocimiento conceptual (Bonus — hasta 10 pts extra)

El evaluador puede hacer hasta 5 preguntas orales del glosario. Máx 2 pts por respuesta correcta.

| Pregunta | Concepto evaluado | Puntaje |
|----------|-------------------|---------|
| 1 | | /2 |
| 2 | | /2 |
| 3 | | /2 |
| 4 | | /2 |
| 5 | | /2 |
| **Bonus total** | | /10 |

---

## Resultado final

| Sección | Peso | Puntaje (0–100) | Puntaje ponderado |
|---------|------|-----------------|-------------------|
| 1 — Seguridad pipeline | 25% | | |
| 2 — Tipos GitHub Actions | 20% | | |
| 3 — Helm + K8s avanzado | 25% | | |
| 4 — Observabilidad | 15% | | |
| 5 — Documentación seguridad | 15% | | |
| **TOTAL** | **100%** | — | **___ / 100** |
| Bonus conceptual | — | — | + ___ |
| **TOTAL FINAL** | | | **___ / 100** |

### Nivel alcanzado

- [ ] **No aprobado** — Puntaje < 50
- [ ] **En desarrollo** — Puntaje 50–69
- [ ] **Aprobado (Competente)** — Puntaje ≥ 70

---

## Observaciones del evaluador

```
[Agregar comentarios cualitativos aquí]
```

## Recomendación

- [ ] Continuar a nivel Proficiente
- [ ] Repetir evaluación Competente en 30 días
- [ ] No apto para el rol
