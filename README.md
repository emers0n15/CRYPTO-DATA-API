# ⚡ CryptoPulse API

> **Autor:** Emerson Covane (emers0n15)  
> **Versão:** 1.0.0  
> **Descrição:**  
> API backend construída com **Express.js**, **Axios** e **Socket.io**, que fornece dados em tempo real sobre criptomoedas (preço, volume, capitalização de mercado, histórico etc.) através da **CoinGecko API**.  
> Totalmente compatível com **Vercel Serverless** e **modo de desenvolvimento local**.

---

## 🧱 Estrutura do Projeto

```
backend/
├── server.js
├── package.json
├── vercel.json
└── .env
```

---

## ⚙️ Configuração de Ambiente

Cria o ficheiro `.env` dentro da pasta `backend/`:

```bash
PORT=3001
NODE_ENV=development
COINGECKO_API_KEY=
FRONTEND_URL=http://localhost:5173
```

> ⚠️ No Vercel, estas variáveis devem ser configuradas em  
> **Settings → Environment Variables**, com o mesmo nome.

---

## 🧠 Cache Interno

A API mantém um cache em memória para reduzir o número de chamadas à CoinGecko:

| Parâmetro | Valor |
|------------|--------|
| TTL (Time to Live) | 25 segundos |
| Atualização automática | A cada 20 segundos |
| Fallback | Se a CoinGecko falhar, serve o cache anterior |

---

## 🚀 Rotas HTTP

### **1️⃣ GET `/api`**
Retorna as principais criptomoedas (Top 10) ou resultados de busca.

#### 🔹 Parâmetros opcionais
| Parâmetro | Tipo | Descrição |
|------------|------|------------|
| `search` | `string` | Termo de busca (nome ou símbolo). |

#### 🔹 Exemplo
```bash
GET /api
GET /api?search=solana
```

#### 🔹 Resposta
```json
[
  {
    "id": "bitcoin",
    "symbol": "btc",
    "name": "Bitcoin",
    "image": "https://assets.coingecko.com/coins/images/1/large/bitcoin.png",
    "current_price": 97300,
    "market_cap": 1800000000000,
    "total_volume": 35000000000,
    "price_change_percentage_24h": -0.45
  }
]
```

---

### **2️⃣ GET `/api/coin/:id`**
Retorna detalhes de uma moeda específica e histórico de preços das últimas 24h.

#### 🔹 Exemplo
```bash
GET /api/coin/bitcoin
```

#### 🔹 Resposta
```json
{
  "id": "bitcoin",
  "symbol": "btc",
  "name": "Bitcoin",
  "market_data": {
    "current_price": { "usd": 97300 },
    "market_cap": { "usd": 1800000000000 }
  },
  "priceHistory24h": [
    [1730990400000, 97234.12],
    [1730994000000, 97120.54]
  ]
}
```

---

### **3️⃣ GET `/api/suggestions?q=<termo>`**
Retorna sugestões para autocomplete (máx. 10 resultados).

#### 🔹 Exemplo
```bash
GET /api/suggestions?q=eth
```

#### 🔹 Resposta
```json
[
  { "id": "ethereum", "name": "Ethereum", "symbol": "eth", "thumb": "https://assets.coingecko.com/coins/images/279/thumb/ethereum.png" },
  { "id": "ethereum-classic", "name": "Ethereum Classic", "symbol": "etc", "thumb": "https://assets.coingecko.com/coins/images/453/thumb/ethereum-classic.png" }
]
```

---

## ⚡ WebSocket API (Socket.io)

A API suporta atualizações **em tempo real** usando `Socket.io`.

### 📡 Endpoint
- Local: `ws://localhost:3001`
- Produção: `wss://crypto-backend.vercel.app`

### 🔹 Eventos

| Evento | Direção | Descrição |
|--------|----------|------------|
| `requestCoins` | → Servidor | Solicita lista de top coins |
| `searchCoins` | → Servidor | Pesquisa moedas (ex: `"solana"`) |
| `coinsData` | ← Servidor | Retorna dados atualizados |
| `coinsError` | ← Servidor | Retorna mensagens de erro |

#### 🔸 Exemplo de uso
```js
import { io } from "socket.io-client";
const socket = io("https://crypto-backend.vercel.app");

socket.on("coinsData", (data) => console.log(data));
socket.on("coinsError", (err) => console.error(err));

socket.emit("requestCoins");
socket.emit("searchCoins", "solana");
```

---

## 🧾 Códigos de Erro

| Código | Descrição | Exemplo |
|--------|------------|----------|
| 200 | Sucesso | `/api` |
| 404 | Moeda não encontrada | `/api/coin/abc` |
| 500 | Erro interno | Problema de rede ou CoinGecko fora do ar |

---

## 🧪 Testar Localmente

```bash
cd backend
npm install
npm run dev
```

Servidor disponível em:
- [http://localhost:3001/api](http://localhost:3001/api)
- [http://localhost:3001/api/coin/bitcoin](http://localhost:3001/api/coin/bitcoin)
- [http://localhost:3001/api/suggestions?q=eth](http://localhost:3001/api/suggestions?q=eth)

---

## ☁️ Deploy no Vercel

1. Cria o ficheiro `vercel.json`:

```json
{
  "version": 2,
  "builds": [
    { "src": "server.js", "use": "@vercel/node" }
  ],
  "routes": [
    { "src": "/(.*)", "dest": "server.js" }
  ]
}
```

2. Faz push do projeto para o GitHub.

3. No [Vercel Dashboard](https://vercel.com):
   - Cria um novo projeto → define `backend` como Root Directory.  
   - Adiciona as variáveis de ambiente (`PORT`, `NODE_ENV`, `COINGECKO_API_KEY`, `FRONTEND_URL`).
   - Clica em **Deploy**.

4. URL final:
```
https://crypto-backend.vercel.app/api
```

---

## 🧩 Stack Técnica

| Componente | Versão |
|-------------|----------|
| Node.js | 20+ |
| Express | ^5.1.0 |
| Axios | ^1.13.2 |
| Socket.io | ^4.8.1 |
| CORS | ^2.8.5 |

---

## 🔐 Boas Práticas

- Define `FRONTEND_URL` para restringir CORS.
- Usa `COINGECKO_API_KEY` se possuir conta PRO (para limites maiores).
- Evita chamadas em loop inferiores a 15 segundos.
- Prefere WebSocket para dados em tempo real.

---

## 📜 Licença

MIT © 2025 — Emerson Covane  
Uso livre para fins educativos e não comerciais.  
A API CoinGecko é um serviço externo e segue os [Termos de Uso da CoinGecko](https://www.coingecko.com/en/terms).
