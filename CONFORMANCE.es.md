> Traducción comunitaria (borrador) — Política P2-002 de NTARI, Difusión Multilingüe Global. Fuente: CONFORMANCE.md (original en inglés, instantánea del 2026-10-05). Borrador comunitario asistido por máquina, pendiente de revisión por mantenedores regionales conforme a P2-002 §3.1. Las especificaciones técnicas centrales permanecen en inglés conforme a §2.2.
>
> ¿Encontraste un error en esta traducción? Tu corrección es una contribución
> bienvenida y valorada: haz un fork del repositorio y abre un pull request en
> https://github.com/NTARI-RAND/COER.

> El original en inglés es normativo. Esta traducción se ofrece para su lectura, no para su interpretación: donde difiera de `CONFORMANCE.md`, prevalece el inglés. Las palabras clave normativas (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY), los nombres de campos, las etiquetas y los identificadores se mantienen en inglés porque constituyen el contrato de conformidad.

# COER — Autodescripción de conformidad

*Autodeclarada; previa a la construcción. IDs del registro: `JFA/jfa-conformance-suite.py`.*

**Postura:** COER es una **instancia de JFA cuya forma de economía-educación se adapta** — registros de beneficio público y alertas, en lugar de un mercado de crédito. La forma puede adaptarse; los pisos no: el sustrato soberano, el registro atestiguado y el pacto vinculan de manera uniforme, se heredan a medida que COER se construye, y cada vinculación aún no construida es una **medida provisional nombrada en una ruta comprometida**, nunca una exención (véase OPEN-QUESTIONS #1 para la secuencia):

| ID / disciplina | Estado |
|---|---|
| GOV-9 documento vivo de preguntas abiertas | **Cumplido** — `OPEN-QUESTIONS.md` se publica con esta semilla |
| GOV-10 bien común copyleft | **Cumplido** — AGPL-3.0 adoptada el 2026-07-22; `LICENSE` en la raíz, declarada en el encabezado de la especificación (OQ-3 resuelta) |
| SUB-1 sin punto de estrangulamiento de alojamiento | **Tensión, nombrada** — estado final de SaaS multiinquilino frente a la tesis de portabilidad (OQ-2) |
| Disciplina de registro (espíritu de REC-1) | **Cumplido por diseño** — AuditEvent inmutable para cada cambio de estado; hashing de evidencias y cadena de custodia |
| Señalar, nunca dictaminar | **Cumplido por diseño** — "la IA clasifica y señala; los humanos deciden"; el contenido no verificado nunca se retransmite automáticamente |
| Disciplina de PII (espíritu de REC-3) | **Cumplido por diseño** — sin PII en los registros (logs); sin seguimiento individual persistente; consentimiento, etiquetas de retención, purga |
| Legibilidad | **Parcial** — los estándares como contratos y las pruebas de conformidad para las cargas útiles de NERIS/NEMSIS/CAP están en las convenciones; los recibos de decisión aún no están especificados |

**Línea de honestidad:** cada afirmación crítica para la seguridad aquí es texto de la especificación, no comportamiento probado; nada está atestiguado ni verificado de forma independiente. La salvaguarda propia de la especificación — ningún agente autónomo fusiona cambios en las rutas de §5/§10 — se mantiene.
