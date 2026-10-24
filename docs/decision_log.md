# Registro de decisiones

Toda decisión relevante (técnica, de datos, de gestión, ética o de operación) y todo uso de IA generativa.

| Fecha | Decisión | Alternativas consideradas | Justificación | Responsable | Evidencia (commit / corrida MLflow) |
|---|---|---|---|---|---|
| AAAA-MM-DD | Métrica principal: ROC-AUC + recall con umbral 0.35 | Accuracy | Clases desbalanceadas; el negocio prioriza no perder clientes en riesgo | Data Scientist | `params.yaml` |
| AAAA-MM-DD | Uso de asistente de IA para generar pruebas unitarias | — | Revisadas y ajustadas por el equipo | MLOps Engineer | PR #… |

| Fecha | Decisión | Alternativas consideradas | Justificación | Responsable | Evidencia |
|---|---|---|---|---|---|
| 2026-10-22 | Aplicar los parámetros de V2: customer_value=450.0, contact_cost=20.0, retention_success_rate=0.25 y weekly_capacity=900. Mantener fairness.max_recall_gap_senior_citizen=0.10. | Conservar los valores V1 de la plantilla. | La variante asignada al equipo es V2 y estos son los valores establecidos en el contrato del sistema, sección 11. | Leighton | params.yaml; metrics.json; docs/bitacora/reto-00.md, sección V3 |
| 2026-10-23 | Utilizar ChatGPT/Codex como apoyo para interpretar los documentos, orientar comandos de Git y DVC, resolver errores del entorno y redactar documentación. | Realizar estas tareas sin asistencia de IA. | Facilitar la comprensión y documentación del trabajo. Los parámetros se contrastaron con el contrato y los resultados con las salidas de la terminal. El equipo debe revisar el contenido antes de integrarlo. | Leighton | docs/bitacora/reto-00.md, sección V3; params.yaml |