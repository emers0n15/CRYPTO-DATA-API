# ⚡ CryptoPulse API

> **Autor:** Emerson Covane (emers0n15)  
> **Versão:** 1.0.0  
> **Descrição:**  
> API backend construída com **Express.js**, **Axios** e **Socket.io**, que fornece dados em tempo real sobre criptomoedas através da **CoinGecko API**.  
> Hospedada na **Vercel**, com suporte a WebSocket e cache inteligente.

---

## 🌍 URL de Produção

**https://cryptodataapi.vercel.app**

Endpoint principal:
```
https://cryptodataapi.vercel.app/api
```

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

## ⚙️ Variáveis de Ambiente

`.env` no desenvolvimento:

```bash
PORT=3001
NODE_ENV=development
COINGECKO_API_KEY=
FRONTEND_URL=http://localhost:5173
```

> No Vercel, defina estas variáveis em **Project Settings → Environment Variables**.

---

## 🚀 Endpoints

### ✅ GET `/api`
Retorna top moedas ou pesquisa opcional.

Exemplos:

```
GET https://cryptodataapi.vercel.app/api
GET https://cryptodataapi.vercel.app/api?search=solana
```

Resposta esperada:

```json
{
  "id": "bitcoin",
  "symbol": "btc",
  "current_price": 97300,
  "market_cap": 1800000000000
}
```

---

### ✅ GET `/api/coin/:id`
Detalhes completos + histórico de 24h

```
GET https://cryptodataapi.vercel.app/api/coin/bitcoin
```

---

### ✅ GET `/api/suggestions?q=<nome>`
Sugestões para autocomplete

```
GET https://cryptodataapi.vercel.app/api/suggestions?q=eth
```

---

## 📡 WebSocket

### URL WebSocket
```
wss://cryptodataapi.vercel.app
```

Eventos disponíveis:

| Evento | Ação |
|--------|------|
| `requestCoins` | Carrega top moedas |
| `searchCoins` | Pesquisa moedas |
| `coinsData` | Retorna dados |
| `coinsError` | Mensagens de erro |

Exemplo de uso:

```js
import { io } from "socket.io-client";
const socket = io("https://cryptodataapi.vercel.app");

socket.emit("requestCoins");
socket.on("coinsData", data => console.log(data));
```

---

## 🧪 Rodar Localmente

```bash
cd backend
npm install
npm run dev
```

API local:
```
http://localhost:3001/api
```

---

## ✅ Deploy

A API já está deployada em:
```
https://cryptodataapi.vercel.app
```

Configurar no Vercel:
- Root Directory: `backend`
- Build: `@vercel/node`
- Output: automático via `vercel.json`

---

## 👨‍💻 Autor

**Emerson Covane**  
GitHub: https://github.com/emers0n15

---

## 📜 Licença

MIT — Livre para uso educacional.
CoinGecko © Dados sujeitos aos termos oficiais.

