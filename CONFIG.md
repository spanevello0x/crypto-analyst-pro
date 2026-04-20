# CONFIG.md — Snapshot operacional público

> Este arquivo não é mais um template de wizard multiativo.
> Ele documenta a configuração operacional atual do BTC Analyst vivo no Aspira.

```yaml
workflow_nome: "BTC Analyst Diário"
repo_publico: "https://github.com/spanevello0x/crypto-analyst-pro"
hub_publico: "https://aspira-workflows.pages.dev/BTC-ANALYST-WORKFLOW.html"

ativo:
  - BTC

modelo: "openai-codex/gpt-5.4"

entregas:
  telegram:
    chat_id: "-1003880005285"
    thread_id: "450"
    horario_brt: "08:00"
  whatsapp:
    destino: "Club Avisos"
    horario_brt: "08:05"

coleta:
  preco: "CoinGecko"
  klines_4h: "Binance"
  funding: "Binance Futures"
  sentimento: "alternative.me Fear & Greed"
  dominancia: "CoinGecko Global"
  contexto_principal: "web_search"
  contexto_fallback: "tavily_search"

saida:
  secoes:
    - "Forças"
    - "Riscos"
    - "Postura"
  setups:
    - "BREAKOUT"
    - "RECLAIM"
    - "PULLBACK"
    - "AUSENTE"
  posturas_permitidas:
    - "E interessante estar Comprado."
    - "Faz sentido manter postura Neutra."
    - "E melhor estar Menos Exposto."
```
