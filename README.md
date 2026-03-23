# 📊 Crypto Analyst Pro

> **Análise diária automática de cripto com debate Bull vs Bear e decisão de trading — powered by OpenClaw multi-agent.**

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-OpenClaw-black.svg)
![Models](https://img.shields.io/badge/models-Claude%20Haiku%20%2B%20Sonnet-orange.svg)
![APIs](https://img.shields.io/badge/APIs-100%25%20gratuitas-green.svg)

---

## 🎯 O que é

**Crypto Analyst Pro** é uma skill para [OpenClaw](https://openclaw.ai) que orquestra múltiplos agentes de IA para gerar análises diárias de criptoativos — automaticamente, no horário que você definir, entregues direto no seu Telegram ou WhatsApp.

Inspirado na arquitetura do [TradingAgents](https://github.com/tauricResearch/TradingAgents), mas construído 100% nativo para cripto, sem dependências externas de dados pagos.

---

## 🚀 Demo

```
📊 ANÁLISE BTC — 23/03 | 08:00 BRT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💰 PREÇO: $70.743 | 24h: +2.82% | Semana: -3.87%

📊 TÉCNICO
Tendência 4h: Altista (acima EMA20 + EMA50)
RSI: 56.52 | Funding: -0.00003 (short squeeze)
Suporte: $70.000 | Resistência: $71.500

🌍 CONTEXTO
Fear & Greed: 8 — Extreme Fear 🔴
Strategy comprou 1.031 BTC ($76.6M) nesta semana
Fed sem corte, petróleo >$100, tensão Iran-Israel

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🐂 BULL — alvo: $75.000
Short squeeze ativo + MACD cruzamento altista
• Funding negativo: shorts pagando longs
• Volume 2-3x acima da média confirma bounce
• Smart money: Strategy + ETFs +$230M semanal

🐻 BEAR — alvo: $67.000
Dead cat bounce em downtrend macro (-44% ATH)
• Squeeze já acabou: funding quase neutro
• ETF outflows $405M pós-FOMC > acumulação
• Correlação BTC/S&P positiva historicamente bearish

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 DECISÃO: AGUARDAR
⭐3/5 | Confiança: 62%

Upside até $72k (+1.8%) vs downside a $67-68k (-4%)
Assimetria negativa — aguardar confirmação acima de $72k

⚠️ Não é conselho financeiro.
```

---

## ⚡ Instalação

### Pré-requisito
[OpenClaw](https://openclaw.ai) instalado e configurado com Claude.

### Opção 1 — ClawHub *(em breve)*
```
/skill install crypto-analyst-pro
```

### Opção 2 — Manual
```bash
# Clone o repositório
git clone https://github.com/spanevello0x/crypto-analyst-pro
cd crypto-analyst-pro

# Copie para sua pasta de skills do OpenClaw
cp -r . ~/.openclaw/workspace/skills/crypto-analyst-pro/
```

---

## 🧙 Setup (4 perguntas, 2 minutos)

Após instalar, diga ao seu assistente OpenClaw:

> **"setup crypto analyst"**

O assistente vai te guiar por 4 perguntas:

| # | Pergunta | Exemplo |
|---|----------|---------|
| 1 | Quais ativos monitorar? | BTC, ETH, SOL |
| 2 | Que horas receber? | 08:00 |
| 3 | Qual perfil? | Swing (3-14 dias) |
| 4 | Onde receber? | Telegram ou WhatsApp |

Após confirmar → **cron configurado automaticamente**. Pronto.

---

## 🏗️ Como funciona

Ver [workflow.html](./workflow.html) para visualização completa do fluxo.

### Arquitetura multi-agente

```
⏰ CRON (08:00 BRT)
        │
        ▼
┌───────────────────────────────────┐
│         ASPIRA (Orquestrador)     │
│         Claude Sonnet             │
└───────────┬───────────────────────┘
            │
     ┌──────┴──────┐  ← PARALELO
     ▼             ▼
 ┌───────┐    ┌────────┐
 │ NERD  │    │ TRADER │
 │ Haiku │    │ Haiku  │
 └───┬───┘    └───┬────┘
     │             │
  Notícias      Técnico
  Macro         RSI/MACD
  Fear&Greed    Funding
  On-chain      Suportes
     │             │
     └──────┬───────┘
            ▼
     ┌─────────────┐
     │ TRADER Bull │  ← Sonnet
     │ (argumento  │
     │  comprador) │
     └──────┬──────┘
            ▼
     ┌─────────────┐
     │ TRADER Bear │  ← Sonnet
     │ (refuta     │
     │  o bull)    │
     └──────┬──────┘
            ▼
     ┌─────────────┐
     │   SÍNTESE   │  ← Aspira
     │  + DECISÃO  │
     └──────┬──────┘
            ▼
     📱 Telegram / WhatsApp
```

### 4 etapas por ativo

1. **Coleta paralela** — NERD (macro/sentimento) + TRADER (técnico) simultâneos
2. **Bull Case** — TRADER argumenta compra com os dados reais
3. **Bear Case** — TRADER refuta o bull ponto a ponto
4. **Síntese** — ASPIRA decide: COMPRAR | AGUARDAR | EVITAR

Para múltiplos ativos: execução sequencial (BTC → ETH → SOL).

---

## 📡 APIs utilizadas (100% gratuitas, sem chave)

| API | Dados | Endpoint |
|-----|-------|----------|
| CoinGecko | Preço, variação 24h/7d | `/simple/price` |
| Binance | Candles 4h (OHLCV) | `/api/v3/klines` |
| Binance Futures | Funding rate | `/fapi/v1/fundingRate` |
| Binance Futures | Open interest | `/fapi/v1/openInterest` |
| alternative.me | Fear & Greed Index | `/fng/` |

**Zero API keys necessárias.**

---

## ⚙️ Configuração

Após o setup, suas preferências ficam em `CONFIG.md`:

```yaml
ativos:
  - BTC
  - ETH
  - SOL

horario: "08:00"
timezone: "America/Sao_Paulo"
perfil: "swing"        # conservador | moderado | agressivo  
foco: "swing"          # holder | swing | day
canal: "telegram:-1001234567890:450"
idioma: "pt"           # pt | en | es
detalhe: "completo"    # resumido | completo
```

Edite diretamente ou diga: **"reconfigure crypto analyst"**

---

## 💬 Comandos disponíveis

| Comando | Ação |
|---------|------|
| `setup crypto analyst` | Configuração inicial (wizard) |
| `analisar BTC` | Análise imediata de BTC |
| `analisar ETH, SOL` | Análise de múltiplos ativos |
| `config crypto analyst` | Ver configuração atual |
| `reconfigure crypto analyst` | Refazer configuração |
| `pausar crypto analyst` | Desativar cron (mantém config) |
| `retomar crypto analyst` | Reativar cron |
| `adicionar SOL` | Adicionar ativo sem reconfigurar |
| `remover ETH` | Remover ativo da lista |
| `mudar horário para 7h` | Atualizar só o horário |

---

## 💰 Custo estimado

| Componente | Custo por análise | Por dia (BTC+ETH) |
|------------|-------------------|-------------------|
| Coleta (Haiku) | ~$0.01 | ~$0.04 |
| Bull/Bear (Sonnet) | ~$0.05 | ~$0.20 |
| **Total** | **~$0.08** | **~$0.30** |

*Valores aproximados. Sem custos de dados (APIs gratuitas).*

---

## 🔒 Segurança

- **Nenhuma chave de API** necessária para dados de mercado
- **Nenhuma ordem executada automaticamente** — análise apenas
- O cron roda no seu próprio OpenClaw, em infraestrutura sua
- Relatórios sempre apresentam Bull E Bear — sem viés único

---

## 📂 Estrutura do projeto

```
crypto-analyst-pro/
├── SKILL.md         ← Instruções para o agente OpenClaw
├── CONFIG.md        ← Configuração do usuário (gerada no setup)
├── README.md        ← Este arquivo
└── workflow.html    ← Visualização interativa do fluxo
```

---

## 🗺️ Roadmap

- [ ] Suporte a mais ativos (BNB, XRP, AVAX, MATIC)
- [ ] Análise semanal consolidada (domingo)
- [ ] Backtesting das decisões passadas
- [ ] Versão em inglês e espanhol
- [ ] Publicação no ClawHub

---

## 📄 Licença

MIT — use, modifique e distribua livremente.

---

## 👤 Autor

**Diego Spanevello** — [@IntusCripto](https://instagram.com/intuscripto)

Fundador do Intus Cripto Club. 5+ anos criando conteúdo educacional sobre DeFi, criptoativos e macroeconomia.

---

*Não é conselho financeiro. Use por sua conta e risco.*
