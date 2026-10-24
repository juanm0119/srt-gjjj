\# Bitácora del Reto 01 — Equipo GJJJ



\## PS-2 — Prueba de humo



\- Fecha: 24 de octubre de 2026.

\- Repositorio: https://github.com/juanm0119/srt-v2-gjjj

\- Variante asignada: V2.

\- Entorno utilizado: Windows, Visual Studio.

\- Generacion de los roles principales y secundarios para los intrgantes del grupo. ver ##6 de charter.md

\- Creacion de los riesgos encontrados. Entre los que se encuentra la habilidad mas debil de equipo y como mitigarla (R-01)

\- El rol de Lider para a Jorge Andres Sanchez

### Reglas para aplicar la RACI

1. **Contrato de datos.** Data Steward prepara los hallazgos y propuestas; el Dueño del dato aprueba los cambios. Las reglas canónicas se conservan.
2. **Quality gate.** Data Scientist sustenta la propuesta; MLOps implementa y verifica la configuración. El Líder / PO conserva la A y debe contar con aprobación del Comité. Se registra la decisión y su evidencia.
3. **Umbral de negocio.** El Sponsor decide `model.decision_threshold` con evidencia del Data Scientist y considerando capacidad, impacto y equidad. Este umbral se distingue del umbral fijo de evaluación del gate.
4. **Equidad.** Data Scientist y Data Steward analizan la brecha y presentan alternativas. El Comité, presidido por el docente, decide la brecha aceptable.
5. **Promoción.** El Líder / PO decide y MLOps ejecuta. Se exige un `reports/gate.json` con `passed: true` antes de promover una versión a `@champion`.
6. **Rollback.** El Líder / PO decide y MLOps ejecuta la reversión. Se registran el momento de la decisión, la versión recuperada y la comprobación del servicio.
7. **LLM externo.** El Sponsor autoriza el envío. Antes de ejecutarlo, se revisan los datos y se aplican las exclusiones: el LLM no recibe `customer_id`, `senior_citizen` ni columnas no canónicas.
8. **Suplencias.** Se registra quién ejerce cada rol durante la sustitución. En promoción y rollback, la persona que decide debe ser distinta de la que ejecuta. Si Juan Manuel ejecuta como MLOps secundario, Jorge Andrés podrá ejercer como Líder / PO secundario mediante una suplencia registrada.
9. **Comunicación y evidencia.** Quien ejecuta documenta el resultado; quien tiene la A confirma la decisión o aceptación y asegura que se informe a los roles I. Las decisiones se registran en `docs/decision_log.md` con evidencia verificable.






