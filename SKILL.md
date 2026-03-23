# SKILL.md — Crypto Analyst Pro

**Análise diária automática de cripto com debate Bull vs Bear.**
**Por Diego Spanevello / Intus Cripto Club.**

---

## 🎯 QUANDO ESTA SKILL É ATIVADA

Ativar quando o usuário disser:
- "setup crypto analyst"
- "configurar análise cripto"
- "instalar crypto analyst"
- "reconfigure crypto analyst"
- "analisar [ATIVO]" (ex: "analisar BTC", "analisa ETH")
- Cron job automático diário (disparo interno)

---

## 🧙 FLUXO DE SETUP (primeira vez ou reconfiguração)

### PASSO 0 — Verificar se já existe config

```bash
cat ~/.openclaw/workspace/skills/crypto-analyst-pro/CONFIG.md
```

Se já existir config com `cron_job_id` preenchido → perguntar se quer **reconfigurar** ou só **rodar análise agora**.

---

### PASSO 1 — Boas-vindas

Enviar esta mensagem exata:

```
📊 *Crypto Analyst Pro* — Setup

Vou configurar suas análises diárias em 4 perguntas rápidas.

*Pergunta 1 de 4:*
Quais ativos você quer monitorar?

Exemplos: BTC, ETH, SOL, BNB, XRP, AVAX
_(pode escolher 1 ou vários, separados por vírgula)_
```

---

### PASSO 2 — Horário

Após receber os ativos, perguntar:

```
✅ Ativos: [ATIVOS ESCOLHIDOS]

*Pergunta 2 de 4:*
Que horas você quer receber a análise?

Digite no formato HH:MM — ex: 08:00, 07:30, 09:00
_(horário local do seu fuso)_
```

---

### PASSO 3 — Perfil

```
✅ Horário: [HORARIO]

*Pergunta 3 de 4:*
Qual é o seu perfil de trading?

1️⃣ *Holder* — foco em longo prazo, não se preocupa com movimentos curtos
2️⃣ *Swing* — busca oportunidades de 3 a 14 dias
3️⃣ *Day Trade* — opera intraday, quer análise técnica mais detalhada

Responda: 1, 2 ou 3
```

---

### PASSO 4 — Canal de entrega

```
✅ Perfil: [PERFIL]

*Pergunta 4 de 4 (última!):*
Onde você quer receber os relatórios?

1️⃣ *Telegram* — neste chat (aqui mesmo)
2️⃣ *Telegram* — em outro grupo/tópico (me diga o ID)
3️⃣ *WhatsApp* — me diga o número com DDI

Responda: 1, 2 ou 3
```

Se responder 1: usar o chat_id e thread_id atual da conversa.
Se responder 2: pedir o ID do grupo e tópico.
Se responder 3: pedir o número de WhatsApp.

---

### PASSO 5 — Confirmação e ativação

Após todas as respostas:

```
🎯 *Resumo da sua configuração:*

📈 Ativos: [ATIVOS]
⏰ Horário: [HORARIO] (horário local)
👤 Perfil: [PERFIL]
📱 Entrega: [CANAL]

Confirma? Responda *sim* para ativar ou *não* para ajustar.
```

---

### PASSO 6 — Salvar config e criar cron

Após confirmação:

1. **Detectar timezone** do usuário (perguntar se não souber, ou usar America/Sao_Paulo como padrão)
2. **Converter horário** para UTC
3. **Atualizar CONFIG.md** com todos os valores
4. **Criar cron job** usando a estrutura abaixo
5. **Confirmar ativação**

#### Estrutura do cron job:

```javascript
cron.add({
  name: `Crypto Analyst Pro — ${ativos.join('+')} ${horario}`,
  schedule: {
    kind: "cron",
    expr: `${minutosUTC} ${horaUTC} * * *`,
    tz: "UTC"
  },
  sessionTarget: "isolated",
  payload: {
    kind: "agentTurn",
    model: "anthropic/claude-sonnet-4-6",
    timeoutSeconds: 1800,
    message: GERAR_TASK(ativos, canal, perfil, foco, idioma, detalhe)
  },
  delivery: { mode: "none" },
  enabled: true
})
```

#### Mensagem de confirmação:

```
✅ *Crypto Analyst Pro ativado!*

Sua primeira análise chegará amanhã às [HORARIO].

💡 Dicas:
• Para análise imediata: "analisar BTC"
• Para reconfigurar: "reconfigure crypto analyst"
• Para pausar: "pausar crypto analyst"
• Para ver config: "config crypto analyst"
```

---

## 📊 FLUXO DE ANÁLISE (execução manual ou cron)

### Quando ativado para análise (via cron ou pedido manual):

Ler `CONFIG.md` para obter: ativos, perfil, foco, canal, idioma, detalhe.

Para **cada ativo** na lista, executar sequencialmente:

---

#### ETAPA 1 — Coleta paralela (NERD + TRADER simultâneos)

**NERD** (`agentId: nerd`, `model: anthropic/claude-haiku-4-5`):

```
Colete dados de mercado de [ATIVO] agora:

1. NOTÍCIAS (últimas 24h): headlines principais, eventos macro, impacto
2. SENTIMENTO: Fear & Greed via https://api.alternative.me/fng/, tom social
3. ON-CHAIN: flows de exchanges, movimentos de whales, métricas de rede
4. MACRO: DXY tendência, S&P500 recente, expectativas Fed
5. MOMENTUM: dominância BTC, market cap cripto total, performance vs semana

Retorne JSON estruturado com esses 5 blocos.
```

**TRADER** (`agentId: trader`, `model: anthropic/claude-haiku-4-5`):

```
Análise técnica de [ATIVO] agora.

APIs gratuitas a usar:
- Preço: https://api.coingecko.com/api/v3/simple/price?ids=[COINGECKO_ID]&vs_currencies=usd&include_24hr_change=true
- Candles 4h: https://api.binance.com/api/v3/klines?symbol=[SYMBOL]USDT&interval=4h&limit=60
- Funding: https://fapi.binance.com/fapi/v1/fundingRate?symbol=[SYMBOL]USDT&limit=1

Calcule e retorne JSON: preço atual, tendência 1h/4h/1D, suportes, resistências,
RSI 14, MACD, volume vs média, funding rate, bias de mercado.
```

IDs CoinGecko de referência:
- BTC → `bitcoin` | Binance: `BTCUSDT`
- ETH → `ethereum` | Binance: `ETHUSDT`
- SOL → `solana` | Binance: `SOLUSDT`
- BNB → `binancecoin` | Binance: `BNBUSDT`
- XRP → `ripple` | Binance: `XRPUSDT`
- AVAX → `avalanche-2` | Binance: `AVAXUSDT`

---

#### ETAPA 2 — Bull Case

**TRADER** em modo Bull (`model: anthropic/claude-sonnet-4-6`):

```
Com os dados coletados [incluir JSONs], construa o argumento COMPRADOR mais sólido para [ATIVO].

Adaptar conforme perfil: [PERFIL] | foco: [FOCO]

Estrutura:
1. Tese principal (1 parágrafo)
2. 3 argumentos com números reais
3. Gatilho de alta
4. Alvo de preço (prazo adequado ao perfil)
5. Invalidação (nível de preço)
Máx 300 palavras.
```

---

#### ETAPA 3 — Bear Case

**TRADER** em modo Bear (`model: anthropic/claude-sonnet-4-6`):

```
Com os dados + bull case, construa o argumento VENDEDOR para [ATIVO].
Refute o bull ponto a ponto.

Estrutura:
1. Tese principal
2. Refutação dos 3 argumentos bull
3. 3 riscos concretos com números
4. Gatilho de baixa
5. Alvo de queda
Máx 300 palavras.
```

---

#### ETAPA 4 — Síntese e entrega

ASPIRA sintetiza e envia via `message tool` para o canal configurado:

**Formato do relatório (idioma: pt):**

```
📊 ANÁLISE [ATIVO] — [DD/MM] | [HH:MM] BRT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💰 PREÇO: $X.XXX | 24h: X% | Semana: X%

📊 TÉCNICO
Tendência 4h: [alta/baixa/neutro]
RSI: XX | Funding: X.XX%
Suporte: $X.XXX | Resistência: $X.XXX

🌍 CONTEXTO
Fear & Greed: XX — [label]
[2-3 linhas macro + headline principal]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🐂 BULL — [alvo: $X.XXX]
[Tese em 1 linha]
• [arg 1 com número]
• [arg 2 com número]
• [arg 3 com número]

🐻 BEAR — [alvo: $X.XXX]
[Tese em 1 linha]
• [risco 1 com número]
• [risco 2 com número]
• [risco 3 com número]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 DECISÃO: [COMPRAR | AGUARDAR | EVITAR]
⭐[X]/5 | Confiança: X%

[Raciocínio em 2 linhas]
[Se COMPRAR: Entry $X | Stop $X | Alvo $X]

⚠️ Análise informativa. Não é conselho financeiro.
```

**Ajustes por perfil:**
- `conservador`: recomenda AGUARDAR em cenários mistos, stop mais conservador
- `moderado`: equilibrado, padrão acima
- `agressivo`: menciona oportunidades mesmo em cenários incertos

**Ajustes por detalhe:**
- `resumido`: remover seções de contexto detalhado, manter apenas preço, decisão, 1 bullet por lado
- `completo`: formato completo acima

---

## 🔧 COMANDOS DE GESTÃO

| Comando do usuário | Ação |
|---|---|
| "analisar BTC" | Roda análise imediata de BTC |
| "analisar ETH, SOL" | Roda análise de múltiplos ativos |
| "config crypto analyst" | Mostra configuração atual |
| "reconfigure crypto analyst" | Reinicia fluxo de setup |
| "pausar crypto analyst" | Desativa o cron (mantém config) |
| "retomar crypto analyst" | Reativa o cron |
| "adicionar SOL" | Adiciona ativo à lista sem reconfigurar tudo |
| "remover ETH" | Remove ativo da lista |
| "mudar horário para 7h" | Atualiza só o horário |

---

## 📦 ESTRUTURA DE ARQUIVOS

```
skills/crypto-analyst-pro/
├── SKILL.md      ← Este arquivo (instruções para o agente)
├── CONFIG.md     ← Configuração do usuário (gerada no setup)
└── README.md     ← Documentação para o usuário
```

---

## 🔑 DEPENDÊNCIAS

- **APIs gratuitas:** CoinGecko, Binance, alternative.me (Fear & Greed) — sem chave
- **LLM:** Claude (configurado no OpenClaw do usuário)
- **Ferramentas OpenClaw:** sessions_spawn, message, cron, web_fetch — nativas

**Não requer instalação de dependências externas.**

---

## 📌 NOTAS IMPORTANTES

- Nunca executar ordens de compra/venda automaticamente — análise apenas
- Sempre apresentar Bull E Bear — nunca relatório com viés único
- Se APIs retornarem erro, tentar fallback (web_search com mesmo query)
- Relatórios sempre em horário BRT independente do timezone do servidor
- Máximo 5 ativos por instância para não estourar timeout do cron (1800s)
