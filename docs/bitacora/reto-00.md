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

## PS-3 — Configuración y evaluación de la variante V2

- Fecha de ejecución: 2026-10-21.
- Persona que ejecutó la prueba: Gloria Abimeleth Ardila Guerrero.
- Rama de trabajo: `feat/charter-v2-gloria`.
- Variante: V2 — Hogar Pereira y Dosquebradas.
- Datos utilizados: datos sintéticos de la plantilla. Los resultados constituyen una línea base provisional.

### 1. Comparación de los tres supuestos económicos

Primero se modificaron únicamente los tres supuestos económicos, manteniendo temporalmente la capacidad en 600 para realizar la comparación de PS-3.

Comando:

```powershell
uv run python -m dvc status
```

Salida:

```text
evaluate:
        changed deps:
                params.yaml:
                        modified:           business
```

DVC identificó únicamente la etapa `evaluate` como pendiente de actualización.

Comando:

```powershell
uv run python -m dvc params diff
```

Salida:

```text
Path         Param                            HEAD    workspace
params.yaml  business.contact_cost            25.0    20.0
params.yaml  business.customer_value          600.0   450.0
params.yaml  business.retention_success_rate  0.3     0.25
```

Se verificaron exactamente tres cambios económicos.

### 2. Comparación de las métricas

Después de ejecutar `uv run python -m dvc repro`, se consultó:

```powershell
uv run python -m dvc metrics diff
```

Salida:

```text
Path          Metric                 HEAD      workspace    Change
metrics.json  net_savings_model      37355.0   21347.5      -16007.5
metrics.json  net_savings_random     11137.74  4961.71      -6176.03
metrics.json  net_savings_vs_random  26217.26  16385.79     -9831.47
```

Las métricas predictivas se mantuvieron y cambiaron las métricas económicas.

El beneficio estimado disminuyó porque V2 asigna un menor valor a conservar un cliente y una menor probabilidad de éxito a la intervención. El menor costo de contacto compensa parte de esa reducción.

Con V2, el modelo ofrece un beneficio adicional estimado frente a la selección aleatoria de 16385.79 miles de COP, equivalente a $16.385.790 COP. Esta cifra procede de una simulación con datos sintéticos.

### 3. Configuración final de capacidad y evaluación

Después de registrar la comparación anterior, se ajustó `business.weekly_capacity` a 900 llamadas semanales.

La consulta del archivo confirmó:

```text
weekly_capacity: 900         # llamadas por semana del call center (lista del lunes, Reto 04)
```

El límite configurado de equidad se mantuvo en `fairness.max_recall_gap_senior_citizen: 0.10`, correspondiente a V2.

Se volvió a ejecutar:

```powershell
uv run python -m dvc repro
```

Salida final:

```text
Stage 'ingest' didn't change, skipping
Stage 'validate' didn't change, skipping
Stage 'prepare' didn't change, skipping
Stage 'train' didn't change, skipping
Running stage 'evaluate':
> python -m src.evaluate
[evaluate] roc_auc=0.8404, pr_auc=0.6627, precision=0.593, recall=0.7057, f1=0.6445, recall_at_eval=0.7604, precision_at_eval=0.5573, recall_gap_senior_citizen=0.1444, customers_contacted=457, net_savings_model=21347.5, net_savings_random=4961.71, net_savings_vs_random=16385.79
Updating lock file 'dvc.lock'
Use `dvc push` to send your updates to remote storage.
```

Se reutilizaron las etapas de datos y entrenamiento. La evaluación final conservó los resultados económicos obtenidos en la comparación anterior.

### 4. Hallazgo de equidad

La brecha de recall observada fue 0.1444, equivalente a 14,44 puntos porcentuales. Supera el límite inicial de V2 de 0.10, equivalente a 10 puntos porcentuales.

Este resultado queda registrado como un hallazgo pendiente de análisis de equidad. El contrato establece la ratificación de la política por el Comité en el Reto 06.

El umbral actual seleccionó 457 clientes del conjunto de prueba. La capacidad de 900 llamadas es un límite operativo máximo; estos resultados corresponden a una simulación.

### 5. Incidentes y soluciones

**Bloqueo al iniciar DVC**

Al ejecutar `uv run dvc status`, apareció:

```text
error: Failed to spawn: dvc
  cause: Una directiva de Control de aplicaciones bloqueó este archivo. (os error 4551)
```

Se logró ejecutar DVC mediante Python, utilizando `uv run python -m dvc` seguido del subcomando correspondiente. Esta forma permitió consultar el estado, comparar parámetros, reproducir el pipeline y comparar métricas.

**Desajuste con el registro del pipeline**

Antes de modificar los supuestos económicos, DVC mostró diferencias en varias etapas. Previamente se habían guardado `dvc.lock` y `models/model_meta.json` en un stash.

Se identificó y recuperó la copia con:

```powershell
git stash list
git stash apply 'stash@{0}'
```

Después se consultó `uv run python -m dvc status` y se obtuvo:

```text
Data and pipelines are up to date.
```

Así se recuperó una referencia consistente antes de realizar PS-3.

**Ediciones que no aparecían en el archivo guardado**

Al consultar `params.yaml` desde PowerShell, algunos cambios realizados en el editor todavía mostraban los valores anteriores. Se revisaron y corrigieron los tres valores económicos, verificándolos mediante `Get-Content`.

La capacidad semanal se actualizó directamente desde PowerShell y se confirmó que el archivo mostraba `weekly_capacity: 900`.

