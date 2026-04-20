# ₿ Crypto Analyst Pro

> Repositório público do workflow real de análise diária de BTC que roda hoje no Aspira.

**Nota importante:** o nome do repositório ficou legado (`crypto-analyst-pro`), mas o workflow operacional atual aqui espelhado é o **BTC Analyst** em produção.

---

## O que este repositório representa hoje

Este repo não descreve mais um produto genérico multiativo com debate Bull vs Bear.
Ele passa a espelhar o fluxo real e atual do **BTC Analyst diário** que está operando no ecossistema do Aspira.

### Perfil operacional atual

- **Ativo:** BTC apenas
- **Entrega 1:** Telegram, tópico 450 (08:00 BRT)
- **Entrega 2:** WhatsApp, Club Avisos (08:05 BRT)
- **Execução:** jobs isolados do OpenClaw
- **Modelo documentado:** `openai-codex/gpt-5.4`
- **Coleta:** APIs públicas via `curl`
- **Cálculo:** `python3`
- **Contexto:** `web_search` com fallback `tavily_search`
- **Formato final:** `Forças / Riscos / Postura`
- **Classificação do setup:** `BREAKOUT`, `RECLAIM`, `PULLBACK` ou `AUSENTE`

---

## Formato atual da saída

```md
*📊 BTC — DD/MM | HH:MM BRT*
$X | 24h: X% | 😱 Medo: X/100 — [label]
Dom: X% | RSI: X | Funding: X%
Tend: X | Suporte: $X | Resist: $X | Vol: Xx média
Setup: BREAKOUT | RECLAIM | PULLBACK | AUSENTE

*🟢 Forças*
• ...
• ...

*🔴 Riscos*
• ...
• ...

*🎯 Postura:*
E interessante estar Comprado.
OU
Faz sentido manter postura Neutra.
OU
E melhor estar Menos Exposto.

⚠️ Não é conselho financeiro.
```

A postura final deve ser **exatamente uma** das três frases fixas acima.

---

## Arquivos principais

- `workflow.html` → workflow operacional atual espelhado do sistema do Aspira
- `SKILL.md` → spec operacional curta da versão viva do BTC Analyst
- `CONFIG.md` → snapshot público da configuração operacional atual
- `skill-spec.html` → spec genérica/portável da capability

---

## Fonte de verdade interna

Hoje a fonte real do BTC Analyst está dentro do repositório principal do Aspira:

- `openclaw-dashboard-pro`
- caminho canônico: `repos/aspira-workflows-hub/BTC-ANALYST-WORKFLOW.html`
- espelho interno: `repos/aspira-manual-site/BTC-ANALYST-WORKFLOW.html`

Em runtime, a operação é sustentada pelos jobs do OpenClaw que fazem as entregas para Telegram e WhatsApp.

---

## O que mudou em relação à versão antiga

A versão antiga deste repositório falava em:

- debate `Bull vs Bear`
- análise multiativo
- wizard de setup em 4 perguntas
- decisão `COMPRAR / AGUARDAR / EVITAR`
- arquitetura multiagente como fluxo principal

Isso estava **desalinhado** com a operação real.

A partir desta atualização, o repo passa a documentar o fluxo que está **de fato rodando**.

---

## Publicação canônica

Hub público do Aspira:

- https://aspira-workflows.pages.dev/BTC-ANALYST-WORKFLOW.html

Repositório público:

- https://github.com/spanevello0x/crypto-analyst-pro
