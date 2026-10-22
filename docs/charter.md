# Project Charter — Sistema de Retención Temprana (SRT)

> Se inicia en el **Reto 00** (secciones 1–5) y se completa en el **Reto 01** (secciones 6–8).
> Nombres, roles y decisiones según el *contrato del sistema* del curso (§6.1, §9, §11).

## 1. Identificación

- Proyecto: Sistema de Retención Temprana (SRT).
- Organización: Conecta Andina S.A.
- Variante asignada: V2.
- Segmento: Hogar Pereira y Dosquebradas.
- Equipo: GJJJ, conformado por cuatro integrantes.
- Repositorio: https://github.com/juanm0119/srt-v2-gjjj
- Fuente de los supuestos: Contrato del sistema del curso, §11.

### Supuestos de la variante V2

Los valores monetarios se expresan en miles de pesos colombianos.

| Supuesto | Valor V2 | Unidad o interpretación | Dónde se configura o registra |
|---|---|---|---|
| Valor estimado de conservar un cliente | 450 | Miles de COP por cliente retenido | `params.yaml`: `business.customer_value` |
| Costo de contacto | 20 | Miles de COP por cliente contactado | `params.yaml`: `business.contact_cost` |
| Éxito supuesto de la intervención de retención | 0.25 | Probabilidad: 25 % | `params.yaml`: `business.retention_success_rate` |
| Capacidad semanal del call center | 900 | Llamadas por semana | `params.yaml`: `business.weekly_capacity` |
| Brecha máxima inicial de recall entre grupos de `senior_citizen` | 0.10 | Diferencia absoluta máxima de 10 puntos porcentuales | `params.yaml`: `fairness.max_recall_gap_senior_citizen` |
| Semilla del extracto | 2027 | Número utilizado para reproducir el extracto asignado | Registrada en este charter como referencia para el extracto del Reto 02 |

Estos valores corresponden a las condiciones oficiales de V2. Su configuración en `params.yaml` se verificará durante el Reto 00.

### Integrantes y roles

| Integrante | Rol principal | Rol secundario |
|---|---|---|
| Juan Manuel Adarve Cifuentes | Líder de proyecto / Product Owner | Pendiente de acuerdo del equipo |
| Jorge Eliecer Leighton Romero | MLOps Engineer | Pendiente de acuerdo del equipo |
| Jorge Andrés Sanchez | Data Scientist | Pendiente de acuerdo del equipo |
| Gloria Abimeleth Ardila Guerrero | Data Steward | Pendiente de acuerdo del equipo |

Las funciones de Data Scientist y Data Steward se distribuyen entre dos integrantes. Los roles secundarios se completarán cuando el equipo acuerde las responsabilidades de apoyo.


## 2. Problema de negocio y línea base

## 3. ML Canvas (razonamiento de cada bloque)
| Bloque | Canvas canónico del SRT | Razonamiento del equipo | Adaptación a la variante |
|---|---|---|---|
| Propuesta de valor | | | |
| Tarea de predicción | | | |
| Decisiones | | | |
| Haciendo predicciones | | | |
| Recolección de datos | | | |
| Fuentes de datos | | | |
| Simulación de impacto | | | |
| Construyendo modelos | | | |
| Features | | | |
| Monitoreo | | | |

**Bloque peor justificado y por qué** ("todos están bien justificados" no cuenta):

## 4. KPIs
| Tipo | KPI | Línea base | Meta | Cómo se mide |
|---|---|---|---|---|
| Técnico | | | | |
| Negocio (estimado) | | | | |
| Operación | | | | |
| Gestión | | | | |

## 5. Alcance y nivel de madurez MLOps objetivo

## 6. Roles, habilidades y RACI
(Las filas de umbral de decisión, umbrales del gate, brecha de equidad, rollback, cambio del contrato
de datos y envío de datos a un LLM externo tienen el **A** que fija el contrato §9.2.)

## 7. Backlog estimado, Definición de Terminado y cronograma

## 8. Riesgos principales
Los cinco riesgos de mayor exposición. El registro completo y vivo está en [`riesgos.md`](riesgos.md).
