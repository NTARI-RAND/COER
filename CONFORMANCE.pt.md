> Tradução comunitária (rascunho) — Política NTARI P2-002, Transmissão Multilíngue Global (Global Multilingual Broadcast). Fonte: CONFORMANCE.md (original em inglês, snapshot de 2026-10-05). Rascunho comunitário assistido por máquina, pendente de revisão por mantenedor regional conforme P2-002 §3.1. As especificações técnicas centrais permanecem em inglês conforme §2.2.
>
> Notou algum erro nesta tradução? Correções de tradução são contribuições
> valiosas e muito bem-vindas: faça um fork do repositório e abra um pull
> request em https://github.com/NTARI-RAND/COER.

> O original em inglês é normativo. Esta tradução é oferecida para leitura, não para interpretação: onde ela divergir de `CONFORMANCE.md`, prevalece o inglês. As palavras-chave normativas (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY), os nomes de campos, os rótulos e os identificadores são mantidos em inglês porque constituem o contrato de conformidade.

# COER — Autodescrição de conformidade

*Autoatestada; pré-construção. IDs do registro: `JFA/jfa-conformance-suite.py`.*

**Postura:** o COER é uma **instância da JFA cuja forma de economia-educação se flexiona** — registros de benefício público e alertas, em vez de um mercado de crédito. A forma pode se flexionar; os pisos não: o substrato soberano, o registro testemunhado e o pacto vinculam de maneira uniforme, herdados à medida que o COER é construído, e todo vínculo ainda não construído é uma **solução provisória nomeada em um caminho comprometido**, nunca uma isenção (veja OPEN-QUESTIONS #1 para a sequência):

| ID / disciplina | Status |
|---|---|
| GOV-9 documento vivo de questões em aberto | **Atendido** — `OPEN-QUESTIONS.md` acompanha esta semente |
| GOV-10 commons copyleft | **Atendido** — AGPL-3.0 adotada em 2026-07-22; `LICENSE` na raiz, declarada no cabeçalho da especificação (OQ-3 resolvida) |
| SUB-1 nenhum ponto de estrangulamento de hospedagem | **Tensão, nomeada** — estado final SaaS multilocatário vs. a tese da portabilidade (OQ-2) |
| Disciplina de registro (espírito de REC-1) | **Atendido por design** — AuditEvent imutável para toda mudança de estado; hash de evidências e cadeia de custódia |
| Sinalizar, nunca sentenciar | **Atendido por design** — "a IA classifica e sinaliza; humanos decidem"; conteúdo não verificado nunca é retransmitido automaticamente |
| Disciplina de PII (espírito de REC-3) | **Atendido por design** — nenhuma PII nos logs; nenhum rastreamento individual persistente; consentimento, tags de retenção, expurgo |
| Legibilidade | **Parcial** — padrões como contratos e testes de conformidade para payloads NERIS/NEMSIS/CAP estão nas convenções; recibos de decisão ainda não especificados |

**Linha de honestidade:** toda alegação crítica para a segurança aqui é texto de especificação, não comportamento testado; nada é testemunhado nem verificado de forma independente. A salvaguarda da própria especificação — nenhum merge por agente autônomo em caminhos §5/§10 — permanece.
