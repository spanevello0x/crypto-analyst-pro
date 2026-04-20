# SKILL.md — BTC Analyst (repo legado: crypto-analyst-pro)

## Objetivo

Gerar uma análise diária de **BTC** com dados públicos, leitura curta e acionável, e saída padronizada em:

- `Forças`
- `Riscos`
- `Postura`

## Escopo atual

- **Ativo único:** BTC
- **Sem subagentes**
- **Sem debate Bull vs Bear**
- **Sem setup wizard multiativo**
- **Sem decisão aberta em texto livre**

## Fluxo operacional atual

1. Coletar dados via `curl`
   - CoinGecko preço/variação
   - Binance klines 4h
   - Binance funding
   - Fear & Greed
   - CoinGecko global
2. Calcular com `python3`
   - RSI(14)
   - suporte
   - resistência
   - tendência
   - volume relativo
3. Buscar contexto via `web_search`
4. Se faltar contexto, usar fallback `tavily_search`
5. Classificar o setup em uma destas opções:
   - `BREAKOUT`
   - `RECLAIM`
   - `PULLBACK`
   - `AUSENTE`
6. Montar a mensagem final no formato exato
7. Entregar em Telegram e WhatsApp

## Regras fixas de saída

### Estrutura

- `*🟢 Forças*`
- `*🔴 Riscos*`
- `*🎯 Postura:*`

### Postura final permitida

Usar **exatamente uma** destas frases:

- `E interessante estar Comprado.`
- `Faz sentido manter postura Neutra.`
- `E melhor estar Menos Exposto.`

## Entregas atuais documentadas

- Telegram tópico 450 às 08:00 BRT
- WhatsApp Club Avisos às 08:05 BRT

## Modelo documentado

- `openai-codex/gpt-5.4`

## Fonte interna de verdade

- `repos/aspira-workflows-hub/BTC-ANALYST-WORKFLOW.html`
- `repos/aspira-manual-site/BTC-ANALYST-WORKFLOW.html`

## Observação

O nome do repositório continua `crypto-analyst-pro`, mas a capability viva documentada aqui é o **BTC Analyst** que está rodando hoje no Aspira.
