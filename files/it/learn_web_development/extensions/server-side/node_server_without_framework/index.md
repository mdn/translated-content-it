---
title: Server Node.js senza un framework
slug: Learn_web_development/Extensions/Server-side/Node_server_without_framework
l10n:
  sourceCommit: e4e57ab3ccb5f93319f8fe13848d4895d3e1e771
---

Questo articolo mostra un server di file statici costruito in [Node.js](https://nodejs.org/en/) senza l'uso di alcun framework. Lo stato attuale di Node.js è tale che quasi tutto ciò di cui abbiamo bisogno per il server di file statici è fornito dalle API interne e da poche righe di codice.

## Esempio

Un server di file statici costruito con Node.js:

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
  const paths = [STATIC_PATH, url];
  if (url.endsWith("/")) paths.push("index.html");
  const filePath = path.join(...paths);
  const pathTraversal = !filePath.startsWith(STATIC_PATH);
  const exists = await fs.promises.access(filePath).then(...toBool);
  const found = !pathTraversal && exists;
  const streamPath = found ? filePath : STATIC_PATH + "/404.html";
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

### Spiegazione

Le seguenti righe importano moduli interni di Node.js.

```js
import * as fs from "node:fs";
import * as http from "node:http";
import * as path from "node:path";
```

Successivamente abbiamo una funzione per creare il server. `https.createServer` restituisce un oggetto `Server`, che possiamo avviare ascoltando su `PORT`.

```js
http
  .createServer((req, res) => {
    /* handle http requests */
  })
  .listen(PORT);

console.log(`Server running at http://127.0.0.1:${PORT}/`);
```

La funzione asincrona `prepareFile` restituisce la struttura: `{ found: boolean, ext: string, stream: ReadableStream }`. Se il file può essere servito (il processo del server ha accesso e non è stata trovata alcuna vulnerabilità di path-traversal), restituiremo lo stato HTTP di `200` come `statusCode` che indica successo (altrimenti restituiremo `HTTP 404`). Si noti che altri codici di stato possono essere trovati in `http.STATUS_CODES`. Con lo stato `404` restituiremo il contenuto del file `'/404.html'`.

L'estensione del file richiesto verrà analizzata e convertita in minuscolo. Dopo di che cercheremo nella collezione `MIME_TYPES` per i corretti [tipi MIME](/it/docs/Web/HTTP/Guides/MIME_types). Se non vengono trovate corrispondenze, utilizziamo `application/octet-stream` come tipo predefinito.

Infine, se non ci sono errori, inviamo il file richiesto. Il `file.stream` conterrà un flusso `Readable` che verrà incanalato in `res` (un'istanza del flusso `Writable`).

```js
res.writeHead(statusCode, { "Content-Type": mimeType });
file.stream.pipe(res);
```
