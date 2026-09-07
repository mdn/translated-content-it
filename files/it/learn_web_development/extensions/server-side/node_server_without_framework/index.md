---
title: Server Node.js senza framework
short-title: Server Node.js semplice
slug: Learn_web_development/Extensions/Server-side/Node_server_without_framework
l10n:
  sourceCommit: c5d8af227105b2a6d2ab50ff74295ead221fce64
---

Questo articolo mostra un server di file statici realizzato in [Node.js](https://nodejs.org/en/) senza usare alcun framework.
Lo stato attuale di Node.js è tale che quasi tutto il necessario per il server di file statici è fornito dalle API integrate e da poche righe di codice.

## Esempio

Un server di file statici realizzato con Node.js:

```js
import * as fs from "node:fs";
import * as http from "node:http";
import * as path from "node:path";

const PORT = 8000;

const MIME_TYPES = {
  default: "application/octet-stream",
  html: "text/html; charset=UTF-8",
  js: "text/javascript",
  css: "text/css",
  png: "image/png",
  jpg: "image/jpeg",
  gif: "image/gif",
  ico: "image/x-icon",
  svg: "image/svg+xml",
};

const STATIC_PATH = path.join(process.cwd(), "./static");

const toBool = [() => true, () => false];

const prepareFile = async (url) => {
  const urlAsPath = decodeURI(url);
  const paths = [STATIC_PATH, urlAsPath];
  if (url.endsWith("/")) paths.push("index.html");
  const filePath = path.join(...paths);
  const pathTraversal = !filePath.startsWith(STATIC_PATH);
  const exists = await fs.promises.access(filePath).then(...toBool);
  const found = !pathTraversal && exists;
  const streamPath = found ? filePath : `${STATIC_PATH}/404.html`;
  const ext = path.extname(streamPath).substring(1).toLowerCase();
  const stream = fs.createReadStream(streamPath);
  return { found, ext, stream };
};

http
  .createServer(async (req, res) => {
    const file = await prepareFile(req.url);
    const statusCode = file.found ? 200 : 404;
    const mimeType = MIME_TYPES[file.ext] || MIME_TYPES.default;
    res.writeHead(statusCode, { "Content-Type": mimeType });
    file.stream.pipe(res);
    console.log(`${req.method} ${req.url} ${statusCode}`);
  })
  .listen(PORT);

console.log(`Server running at http://127.0.0.1:${PORT}/`);
```

### Analisi

Le righe seguenti importano moduli interni di Node.js.

```js
import * as fs from "node:fs";
import * as http from "node:http";
import * as path from "node:path";
```

Successivamente è presente una funzione per creare il server. `https.createServer` restituisce un oggetto `Server`, che può essere avviato mettendolo in ascolto su `PORT`.

```js
http
  .createServer((req, res) => {
    /* handle http requests */
  })
  .listen(PORT);

console.log(`Server running at http://127.0.0.1:${PORT}/`);
```

La funzione asincrona `prepareFile` restituisce la struttura: `{ found: boolean, ext: string, stream: ReadableStream }`.
Se il file può essere servito (il processo del server ha accesso e non viene rilevata alcuna vulnerabilità di path traversal), verrà restituito lo stato HTTP `200` come `statusCode` che indica il successo (altrimenti viene restituito `HTTP 404`).
È possibile trovare altri codici di stato in `http.STATUS_CODES`.
Con lo stato `404` verrà restituito il contenuto del file `'/404.html'`.

L'estensione del file richiesto verrà analizzata e convertita in minuscolo. Successivamente verrà cercato nella raccolta `MIME_TYPES` il corretto [tipo MIME](/it/docs/Web/HTTP/Guides/MIME_types). Se non viene trovata alcuna corrispondenza, viene usato `application/octet-stream` come tipo predefinito.

Infine, se non sono presenti errori, viene inviato il file richiesto. `file.stream` conterrà uno stream `Readable`, che verrà inoltrato tramite pipe in `res` (un'istanza dello stream `Writable`).

```js
res.writeHead(statusCode, { "Content-Type": mimeType });
file.stream.pipe(res);
```
