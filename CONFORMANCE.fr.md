> Traduction communautaire (brouillon) — politique NTARI P2-002, Diffusion multilingue mondiale. Source : CONFORMANCE.md (original anglais, instantané du 2026-10-05). Brouillon communautaire assisté par machine, en attente de relecture par le mainteneur régional conformément à P2-002 §3.1. Les spécifications techniques essentielles restent en anglais conformément au §2.2.
>
> Vous avez remarqué une erreur de traduction ? N'hésitez pas à la corriger
> vous-même : forkez le dépôt https://github.com/NTARI-RAND/COER et
> ouvrez une pull request. Les corrections de traduction sont des
> contributions précieuses, tout autant que le code.

> L'original anglais est normatif. Cette traduction est proposée pour la lecture, non pour l'interprétation : là où elle diverge de `CONFORMANCE.md`, l'anglais prévaut. Les mots-clés normatifs (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY), les noms de champs, les libellés et les identifiants sont conservés en anglais, car ils constituent le contrat de conformité.

# COER — Autodescription de conformité

*Auto-attestée ; avant construction. Identifiants du registre : `JFA/jfa-conformance-suite.py`.*

**Posture :** COER est une **instance JFA dont la forme économie-éducation fléchit** — des registres d'intérêt public et des alertes plutôt qu'un marché du crédit. La forme peut fléchir ; les planchers, non : le substrat souverain, le registre attesté par témoins et le pacte s'imposent uniformément, hérités à mesure que COER se construit, et chaque obligation non encore construite est une **mesure provisoire nommée sur une trajectoire engagée**, jamais une exemption (voir OPEN-QUESTIONS n° 1 pour l'ordonnancement) :

| ID / discipline | Statut |
|---|---|
| GOV-9 document vivant des questions ouvertes | **Respecté** — `OPEN-QUESTIONS.md` est livré avec ce document fondateur |
| GOV-10 communs copyleft | **Respecté** — AGPL-3.0 adoptée le 2026-07-22 ; `LICENSE` à la racine, déclarée dans l'en-tête de la spécification (OQ-3 résolue) |
| SUB-1 aucun point d'étranglement d'hébergement | **Tension, nommée** — état final SaaS multilocataire contre la thèse de la portabilité (OQ-2) |
| Discipline du registre (esprit de REC-1) | **Respecté par conception** — AuditEvent immuable pour chaque changement d'état ; hachage des preuves et chaîne de possession |
| Signaler, jamais constater | **Respecté par conception** — « L'IA classe et signale ; les humains décident » ; un contenu non vérifié n'est jamais rediffusé automatiquement |
| Discipline des données personnelles (esprit de REC-3) | **Respecté par conception** — aucune donnée personnelle (PII) dans les journaux ; aucun suivi individuel persistant ; consentement, étiquettes de conservation, purge |
| Lisibilité | **Partiel** — les normes en tant que contrats et les tests de conformité pour les charges utiles NERIS/NEMSIS/CAP figurent dans les conventions ; les reçus de décision ne sont pas encore spécifiés |

**Ligne d'honnêteté :** chaque affirmation critique pour la sécurité formulée ici est un texte de spécification, et non un comportement testé ; rien n'est attesté par témoins ni vérifié de manière indépendante. Le garde-fou propre à la spécification — aucune fusion par un agent autonome dans les chemins §5/§10 — demeure.
