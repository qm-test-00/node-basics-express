# ESERCIZIO PRATICO Node.js COMPLETO - 10 MINUTI

**Obiettivo:** Creare API REST minimale con validazione, errori e logging  
**Stack:** Express + Zod + UUID

---

## 📋 SPECIFICHE TECNICHE

**Endpoint richiesti:**

- `GET /users?page=1&limit=10` → Lista utenti paginata
- `GET /users/:id` → Utente singolo
- `POST /users` → Crea utente (201 Created)
- `DELETE /users/:id` → Elimina utente (204 No Content)
- `POST /tasks/heavy` → Lancia task pesante su worker thread (202 Accepted)
- `GET /tasks/:taskId` → Stato e risultato del task

**Schema Utente:**

```typescript
interface User {
  id: string; // UUID
  name: string; // min 2 char
  email: string; // valid email
  createdAt: string; // ISO Date
}
```

**Schema Task (Worker Thread):**

```typescript
interface HeavyTaskRequest {
  iterations: number; // min 1, max 1000000
}

interface HeavyTaskResponse {
  taskId: string; // UUID
  status: "processing" | "completed" | "error";
  iterations: number;
  result?: number; // Somma dei numeri primi trovati
  duration?: number; // ms
}
```

---

## 🚀 SETUP INIZIALE

```bash
# Installa dipendenze
npm install

# Avvia server in modalità sviluppo
npm run dev
```

**File di partenza:** `index.ts`

```typescript
import express, { Request, Response, NextFunction } from "express";

const app = express();
app.use(express.json());

const PORT = 3000;

// === IL TUO CODICE QUI ===

app.listen(PORT, () => {
  console.log(`🚀 Server on http://localhost:${PORT}`);
});
```

---

## 🧪 TEST AUTOMATICO

### Test Shell Script

```bash
chmod +x test.sh && npm run test:api
```

### Test Jest (opzionale)

```bash
npm test              # esegue tutti i test
npm run test:watch    # modalità watch
```

I test Jest in `__tests__/api.test.ts` sono esempi da completare dopo l'implementazione.

---

## 📦 DIPENDENZE INCLUSE

### Runtime

- `express` ^4.21.2 - Framework web
- `zod` ^3.23.8 - Validazione schema
- `uuid` ^11.0.3 - Generazione UUID

### Development

- `typescript` ^5.7.2 - TypeScript compiler
- `tsx` ^4.19.2 - TypeScript executor (più veloce di ts-node)
- `jest` ^29.7.0 - Framework testing
- `supertest` ^7.0.0 - Testing HTTP

---

## 🏆 COMANDI RAPIDI

```bash
# Sviluppo
npm run dev

# Build
npm run build
npm start

# Test
npm test
npm run test:api
```

---

## 💡 SUGGERIMENTI

1. **Validazione:** Usa Zod per validare i dati in input
2. **UUID:** Usa `uuid.v4()` per generare ID unici
3. **Paginazione:** Calcola offset = (page - 1) \* limit
4. **Database Simulato:** Usa i dati in `data/users.json` come storage iniziale
   - Puoi leggerli e modificarli in memoria
   - Oppure implementare persistenza su file
5. **Worker Threads:** Usa `worker_threads` per task pesanti
   - Crea un file worker separato (es. `heavy-task.worker.ts`)
   - Usa `new Worker()` per lanciare il worker
   - Comunica con `postMessage` e `on('message')`
   - Memorizza lo stato dei task in una Map/oggetto
6. **Docker:** Crea un Dockerfile per containerizzare l'applicazione
   - Usa immagine base Node.js (es. `node:20-alpine`)
   - COPY package files e RUN npm install
   - COPY source code
   - EXPOSE 3000
   - CMD per avviare il server
7. **Status Code:**
   - 200 OK (GET successo)
   - 201 Created (POST successo)
   - 202 Accepted (Task avviato)
   - 204 No Content (DELETE successo)
   - 400 Bad Request (validazione fallita)
   - 404 Not Found (risorsa non trovata)
   - 409 Conflict (email duplicata)

---

## 🐳 DOCKER

Crea un `Dockerfile` per containerizzare il web server:

```bash
# Build dell'immagine
docker build -t node-api-server .

# Run del container
docker run -p 3000:3000 node-api-server

# Oppure con docker-compose (opzionale)
docker-compose up
```

**Requisiti Dockerfile:**

- Usa un'immagine Node.js LTS (es. `node:20-alpine` per ottimizzare dimensioni)
- Installa dipendenze in fase di build
- Compila TypeScript in produzione
- Esponi la porta 3000
- Usa un utente non-root per sicurezza (opzionale ma consigliato)

---

## 🔧 NOTE TECNICHE

- **TypeScript:** Configurato con ES Modules (`type: "module"`)
- **Target:** ES2022 con moduleResolution bundler
- **Executor:** `tsx` per sviluppo veloce
- **Testing:** Jest con ts-jest per supporto ESM

---

**Buon lavoro! 🚀**
