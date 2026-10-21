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


