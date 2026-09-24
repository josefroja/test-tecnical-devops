# ADR-001 — GitOps (ArgoCD) vs Push-based Deployment

## Status
Accepted

## Context
<!-- CANDIDATO: Describe aquí el problema de negocio y restricciones que motivaron esta decisión.
     Ejemplo: el equipo necesita auditabilidad, multi-cluster, rollback fácil, etc. -->

El equipo requiere un mecanismo de despliegue para microservicios en AKS que:
- Proporcione un registro de auditoría de todos los cambios desplegados
- Permita rollback rápido ante incidentes
- Escale a múltiples entornos (dev, staging, prod) sin duplicar lógica en pipelines
- Sea operable por engineers con distintos niveles de experiencia

## Decision
<!-- CANDIDATO: Describe la decisión tomada y por qué. -->

Adoptamos **GitOps con ArgoCD** como modelo de despliegue. El cluster sincroniza
su estado deseado desde el repositorio Git; los pipelines CI solo actualizan el
valor de `image.tag` en el repositorio y ArgoCD gestiona la reconciliación.

## Consequences

**Positivo:**
- El estado del cluster siempre está en Git → auditoría completa
- Rollback = revertir un commit → operación familiar para el equipo
- ArgoCD detecta y corrige drift automáticamente (`selfHeal`)
- Separación de concerns: CI produce artefactos, CD gestiona despliegues

**Negativo:**
- Curva de aprendizaje inicial para ArgoCD
- Requiere un componente adicional (ArgoCD) en el cluster
- Latencia de sincronización (puede tardar hasta 3 min en detectar cambios)
- Credenciales del cluster NO en el pipeline CI → menos control granular desde el pipeline

## Alternatives considered

| Alternativa | Motivo de descarte |
|-------------|-------------------|
| Push-based (kubectl/helm en pipeline) | Requiere credentials del cluster en CI; no detecta drift; más difícil de auditar |
| Flux CD | Funcionalidad comparable a ArgoCD pero UI más limitada; menor adopción en el equipo |
| Azure DevOps Release Pipelines | Lock-in con Azure DevOps; menor flexibilidad para multi-cluster |
