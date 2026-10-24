\# Bitácora del Reto 00 — Equipo GJJJ



\## PS-2 — Prueba de humo



\- Fecha: 17 de octubre de 2026.

\- Persona que ejecutó la verificación: Gloria Abimeleth Ardila Guerrero.

\- Repositorio: https://github.com/juanm0119/srt-v2-gjjj

\- Variante asignada: V2.

\- Entorno utilizado: Windows, terminal PowerShell.

\- Configuración de parámetros utilizada: pendiente de verificar.



\### 1. Preparación del entorno



Se ejecutó:



```powershell

uv sync

```



Salida obtenida:



```text

Resolved 215 packages in 1ms

Checked 207 packages in 26ms

```



El comando finalizó sin errores visibles.



\### 2. Verificación del pipeline



Se ejecutó:



```powershell

uv run dvc repro

```



Salida obtenida:



```text

Stage 'ingest' didn't change, skipping

Stage 'validate' didn't change, skipping

Stage 'prepare' didn't change, skipping

Stage 'train' didn't change, skipping

Stage 'evaluate' didn't change, skipping

Data and pipelines are up to date.

```



DVC informó que el pipeline estaba actualizado. Las etapas se

omitieron porque no detectó cambios y se conservaron los

resultados existentes.



\### 3. Consulta de las métricas



Como evaluate no volvió a ejecutarse, las métricas se consultaron

en el archivo metrics.json mediante:



```powershell

Get-Content .\\metrics.json

```



Resultados consultados:



```json

{

&#x20; "roc\_auc": 0.8404,

&#x20; "pr\_auc": 0.6627,

&#x20; "f1": 0.6445,

&#x20; "recall\_at\_eval": 0.7604,

&#x20; "precision\_at\_eval": 0.5573,

&#x20; "recall\_gap\_senior\_citizen": 0.1444,

&#x20; "customers\_contacted": 457,

&#x20; "net\_savings\_model": 37355.0,

&#x20; "net\_savings\_random": 11137.74,

&#x20; "net\_savings\_vs\_random": 26217.26

}

```



Estas métricas corresponden al archivo existente; no fueron

generadas nuevamente durante esta ejecución de dvc repro.

### 4. Pruebas automatizadas

Se ejecutó el comando:

```powershell
uv run pytest
```

Salida final obtenida:

```text
29 passed in 3.76s
```

- Número de pruebas aprobadas: 29.
- Número de pruebas fallidas: 0.
- Tiempo de ejecución: 3,76 segundos.

### 5. Errores y soluciones

No se observaron errores en uv sync, uv run dvc repro ni
uv run pytest.

DVC omitió las etapas porque el pipeline estaba actualizado.
Las métricas se consultaron en el archivo metrics.json.

Al consultar la bitácora en PowerShell, algunas tildes se
mostraron incorrectamente. Se visualizaron correctamente
al utilizar Get-Content con el parámetro -Encoding UTF8.

### 6. Estado de la verificación

Se completaron la preparación del entorno, la comprobación
del estado del pipeline y la ejecución de las pruebas.

DVC reutilizó los resultados existentes y las 29 pruebas
automatizadas aprobaron. El roc_auc consultado fue 0.8404
y el pr_auc fue 0.6627.


3 — Adaptación de los supuestos a la variante V2

Responsable: Leighton (usuario de GitHub: Leighton8807).

Rama de trabajo: leighton.

Fuente de los valores: Contrato del sistema, §11.

Segmento: Hogar Pereira y Dosquebradas.

Parámetros modificados

Se actualizaron los siguientes valores en params.yaml:

Parámetro

Valor anterior

Valor V2

business.customer_value

600.0

450.0

business.contact_cost

25.0

20.0

business.retention_success_rate

0.30

0.25

business.weekly_capacity

600

900

Parámetro

Valor anterior

Valor V2

business.customer_value

600.0

450.0

business.contact_cost

25.0

20.0

business.retention_success_rate

0.30

0.25

business.weekly_capacity

600

900

Los valores monetarios se expresan en miles de COP. La política fairness.max_recall_gap_senior_citizen se conservó en 0.10, porque ya coincide con V2. No se modificó el umbral de decisión.

Verificación de las etapas afectadas

Después de recuperar del respaldo local dvc.lock y models/model_meta.json, se ejecutó uv run dvc status y se obtuvo:

evaluate:
        changed deps:
                params.yaml:
                        modified:           business

Se ejecutó uv run dvc params diff:



Path         Param                            HEAD    workspace
params.yaml  business.contact_cost            25.0    20.0
params.yaml  business.customer_value           600.0   450.0
params.yaml  business.retention_success_rate   0.3     0.25
params.yaml  business.weekly_capacity          600     900

Ejecución y comparación de métricas

Se ejecutó uv run dvc repro. DVC omitió ingest, validate, prepare y train, y ejecutó únicamente evaluate.

Después se ejecutó uv run dvc metrics diff:

Path          Metric                 HEAD      workspace  Change
metrics.json  net_savings_model       37355.0   21347.5   -16007.5
metrics.json  net_savings_random      11137.74  4961.71   -6176.03
metrics.json  net_savings_vs_random   26217.26  16385.79  -9831.47

Las métricas técnicas permanecieron iguales: roc_auc = 0.8404, pr_auc = 0.6627, precision = 0.593 y recall = 0.7057. No se reentrenó el modelo.

El impacto económico estimado cambió al aplicar los supuestos de V2. La ventaja estimada frente a contactar al azar pasó de 26217.26 a 16385.79 miles de COP. 