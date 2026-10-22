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

### Problema y decisión de negocio

Conecta Andina necesita priorizar sus acciones de retención para el segmento Hogar Pereira y Dosquebradas. El call center tiene una capacidad máxima de 900 llamadas semanales.

La decisión que apoya el SRT es a qué clientes llamar cada semana y con qué argumento, utilizando su probabilidad de abandonar el servicio en los próximos 30 días.

La Gerencia Comercial, representada por el sponsor del curso, define el criterio de decisión. Los agentes del call center utilizan la lista priorizada y las explicaciones para realizar los contactos. La selección deberá considerar rentabilidad, capacidad y política de equidad.

### Situación actual

Según el caso del curso, la empresa utiliza tres reglas informales para priorizar contactos:

1. Clientes que se han quejado.
2. Clientes con contrato mes a mes.
3. Clientes que pagan más.

Estas reglas pueden concentrar llamadas en clientes que permanecerían y dejar por fuera a quienes abandonan el servicio sin presentar señales evidentes.

El SRT propone una priorización basada en el riesgo estimado y en las restricciones del negocio.

### Línea base de comparación

Para el Reto 00 se propone reportar al sponsor el beneficio neto estimado del modelo frente a contactar al azar al mismo número de clientes.

Esta comparación es calculable y reproducible con el pipeline disponible. Su limitación es que mide el desempeño frente a una selección aleatoria; la comparación con las tres reglas del proceso actual requerirá definirlas con criterios medibles y disponer de datos sobre sus resultados.

Con los supuestos de V2 y el umbral provisional de 0.35, se seleccionan 457 clientes del conjunto de prueba.

| Indicador | Resultado en miles de COP |
|---|---:|
| Beneficio neto estimado con el modelo | 21347.50 |
| Beneficio neto estimado con selección aleatoria | 4961.71 |
| Beneficio adicional estimado frente al azar | 16385.79 |

Fuente: `metrics.json`, actualizado mediante la verificación PS-3 registrada en `docs/bitacora/reto-00.md`.

Estos resultados corresponden a datos sintéticos de la plantilla y constituyen una línea base provisional. La actualización con el extracto CRM de la variante comenzará en el Reto 02.

### Costo estimado de no actuar

El lote de referencia contiene 1400 clientes y 384 deserciones simuladas, equivalentes aproximadamente al 27.43 %.

Para ilustrar el valor económico en riesgo, se supone que esas 384 deserciones ocurren sin una intervención de retención y que cada cliente representa el valor homogéneo de V2:

```text
Valor en riesgo = clientes que desertan × customer_value
Valor en riesgo = 384 × 450
Valor en riesgo = 172800 miles de COP = $172.800.000 COP
```

Esta cifra representa valor potencial en riesgo asociado a las deserciones simuladas del lote. El beneficio estimado de una campaña incorpora adicionalmente la probabilidad de éxito de retención del 25 % y el costo de cada contacto.

### Punto de equilibrio del contacto

Con los supuestos de V2, el valor esperado de contactar a un cliente con probabilidad de abandono `p` es:

```text
Valor esperado del contacto = p × retention_success_rate × customer_value − contact_cost
Valor esperado del contacto = p × 0.25 × 450 − 20

p* = contact_cost / (retention_success_rate × customer_value)
p* = 20 / (0.25 × 450)
p* = 0.177777... ≈ 17.78 %
```

En el punto de equilibrio, el valor esperado es cero. Por encima de ese riesgo, el contacto tiene un beneficio esperado positivo bajo los supuestos del caso.

### Comparación entre rentabilidad y capacidad

Se consultaron las probabilidades del modelo guardado en `models/model.joblib` sobre `data/processed/test.csv`, utilizando las mismas variables de entrada que `src/evaluate.py`.

Resultado de la consulta:

```text
Clientes del lote de referencia: 1400
Clientes con churn=1: 384
Punto de equilibrio (%): 17.7778
Clientes con riesgo superior al punto de equilibrio: 738
Capacidad semanal: 900
```

Entre los criterios económico y de capacidad, el económico limita la selección en este lote: los 738 clientes con beneficio esperado positivo caben dentro de las 900 llamadas disponibles. Queda un margen de 162 llamadas.

El análisis económico de los 738 clientes sirve como argumento para discutir el umbral. La configuración provisional continúa en `model.decision_threshold: 0.35`, que selecciona 457 clientes.

El umbral definitivo será aprobado por el sponsor en el Reto 06 con evidencia de rentabilidad, capacidad y equidad. La brecha de recall actual de 0.1444 supera el límite inicial de V2 de 0.10 y queda registrada para ese análisis.



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
