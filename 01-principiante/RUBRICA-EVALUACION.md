# Hoja de evaluación — Nivel Principiante

## Candidato
- **Nombre:**
- **Fecha:**
- **Evaluador:**
- **Tiempo utilizado:**

---

## Sección 1 — Containerización (Peso: 20%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Dockerfile multi-stage presente y correcto | 25 | | |
| Imagen final < 250 MB (demostrado con `docker images`) | 20 | | |
| Proceso corre como usuario no-root | 20 | | |
| HEALTHCHECK definido en el Dockerfile | 15 | | |
| Los 2 endpoints REST responden correctamente | 20 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 1:** _____ / 100  
**Puntuación ponderada (×0.20):** _____

---

## Sección 2 — Despliegue Minikube/Kubernetes (Peso: 25%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| `deployment.yaml` con 2 réplicas | 15 | | |
| `liveness` y `readiness` probes definidas | 20 | | |
| Resource requests y limits definidos | 15 | | |
| `service.yaml` correcto y accesible | 15 | | |
| `configmap.yaml` con variables externalizadas | 15 | | |
| `securityContext` con runAsNonRoot | 10 | | |
| Evidencia de pods Running (screenshot/log) | 10 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 2:** _____ / 100  
**Puntuación ponderada (×0.25):** _____

---

## Sección 3 — GitHub Actions CI/CD (Peso: 35%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Triggers correctos (push main, feature/**, PR) | 10 | | |
| Job `build-and-test` con Java 21 y mvn verify | 15 | | |
| Test reports publicados como artifact | 10 | | |
| Job `build-image` encadenado con `needs` | 15 | | |
| Push a GHCR con SHA como tag | 15 | | |
| Job `deploy-azure` con autenticación por Service Principal | 20 | | |
| Condición `if: github.ref == 'refs/heads/main'` en jobs de despliegue | 10 | | |
| Link a ejecución exitosa del workflow | 5 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 3:** _____ / 100  
**Puntuación ponderada (×0.35):** _____

---

## Sección 4 — Documentación (Peso: 20%)

| Criterio | Máx | Obtenido | Notas |
|----------|-----|----------|-------|
| Diagrama de flujo del pipeline | 25 | | |
| Instrucciones de ejecución local paso a paso | 25 | | |
| Variables de entorno listadas y descritas | 25 | | |
| Justificación de decisiones técnicas | 25 | | |
| **Subtotal** | **100** | | |

**Puntuación Sección 4:** _____ / 100  
**Puntuación ponderada (×0.20):** _____

---

## Resultado final

| Sección | Peso | Puntaje (0–100) | Puntaje ponderado |
|---------|------|-----------------|-------------------|
| 1 — Containerización | 20% | | |
| 2 — Kubernetes | 25% | | |
| 3 — GitHub Actions | 35% | | |
| 4 — Documentación | 20% | | |
| **TOTAL** | **100%** | — | **___ / 100** |

### Nivel alcanzado

- [ ] **No aprobado** — Puntaje < 50
- [ ] **En desarrollo** — Puntaje 50–69
- [ ] **Aprobado (Principiante)** — Puntaje ≥ 70

---

## Observaciones del evaluador

```
[Agregar comentarios cualitativos aquí]
```

## Recomendación

- [ ] Continuar a nivel Competente
- [ ] Repetir evaluación Principiante en 30 días
- [ ] No apto para el rol
