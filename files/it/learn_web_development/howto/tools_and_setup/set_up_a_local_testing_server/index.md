---
title: Come impostare un server di test locale?
slug: Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo spiega come impostare un semplice server di test locale sulla sua macchina e le nozioni di base su come utilizzarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario prima sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >, e
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server"
          >cos'è un server Web</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparerà come impostare un server di test locale.</td>
    </tr>
  </tbody>
</table>

## File locali vs. file remoti

Nella maggior parte dell'area di apprendimento, le diciamo di aprire semplicemente i suoi esempi direttamente in un browser — questo può essere fatto facendo doppio clic sul file HTML, trascinandolo e rilasciandolo nella finestra del browser, o scegliendo _File_ > _Apri…_ e navigando fino al file HTML. Ci sono molti modi per ottenere questo.

Se il percorso dell'indirizzo web inizia con `file://` seguito dal percorso del file sul suo disco rigido locale, si sta utilizzando un file locale. Al contrario, se visualizza uno dei nostri esempi ospitati su GitHub (o un esempio su qualche altro server remoto), l'indirizzo web inizierà con `http://` o `https://`, per mostrare che il file è stato ricevuto tramite HTTP.

## Il problema con il test dei file locali

Alcuni esempi non funzioneranno se li apre come file locali. Questo può dipendere da una varietà di motivi, il più probabile è:

- **Presentano richieste asincrone**. Alcuni browser (incluso Chrome) non eseguiranno richieste async (vedere [Learn: Making network requests with JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests)) se si esegue l'esempio da un file locale. Questo è dovuto a restrizioni di sicurezza (per ulteriori informazioni sulla sicurezza web, legga [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)).
- **Includono un linguaggio lato server**. I linguaggi lato server (come PHP o Python) richiedono un server speciale per interpretare il codice e fornire i risultati.
- **Includono altri file**. I browser trattano comunemente le richieste di caricamento delle risorse utilizzando lo schema `file://` come richieste di origine incrociata. Quindi, se carica un file locale che include altri file locali, questo potrebbe innescare un errore {{Glossary("CORS", "CORS")}}.

## Esecuzione di un semplice server HTTP locale

Per aggirare il problema delle richieste asincrone, è necessario testare tali esempi eseguendoli tramite un server web locale.

### Utilizzando un'estensione nel suo editor di codice

Se ha solo bisogno di HTML, CSS e JavaScript, e nessun linguaggio lato server, il modo più semplice potrebbe essere quello di cercare estensioni nel suo editor di codice. Oltre ad automatizzare l'installazione e l'impostazione per il suo server HTTP locale, si integrano bene anche con i suoi editor di codice. Testare file locali in un server HTTP potrebbe essere a un clic di distanza.

Per VS Code, provi le seguenti estensioni gratuite:

- [Live Preview](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server)
- [Preview on Web Server](https://marketplace.visualstudio.com/items?itemName=yuichinukiyama.vscode-preview-server)

### Utilizzando Node.js

Il modulo Node.js [`http-server`](https://www.npmjs.com/package/http-server) è il modo più semplice per ospitare file HTML in qualsiasi directory.

Per utilizzare il modulo:

1. Esegua i seguenti comandi per verificare se Node.js è già installato:

   ```bash
   node -v
   npm -v
   npx -v
   ```

2. Se Node.js non è installato, è necessario installarlo. Segua le [istruzioni di download](https://nodejs.org/en/download) nei documenti Node.js, quindi esegua nuovamente i comandi sopra per verificare se l'installazione ha avuto successo.

3. Supponiamo che la directory sia `/path/to/project`. Esegua il seguente comando per avviare il server:

   ```bash
   npx http-server /path/to/project -o -p 9999
   ```

   Questo ospita tutti i file nella directory `/path/to/project` su `localhost:9999`. L'opzione `-o` aprirà la pagina `index.html` in un browser web. Se `index.html` non esiste, la directory viene visualizzata invece.

### Utilizzando Python

Un altro modo per ottenere questo è utilizzare il modulo `http.server` di Python.

> [!NOTE]
> Le versioni più vecchie di Python (fino alla versione 2.7) fornivano un modulo simile chiamato `SimpleHTTPServer`. Python 2 è già a fine vita, quindi raccomandiamo di utilizzare Python 3.

Per fare questo:

1. Esegua il seguente comando per verificare se Python è già installato:

   ```bash
   python -V
   # If the above fails, try:
   python3 -V
   # Or, if the "py" command is available, try:
   py -3 -V
   ```

2. Se Python non è installato, è necessario installarlo. Segua le [istruzioni di download](https://www.python.org/downloads/) nei documenti Python (abbiamo anche spiegazioni più dettagliate nel nostro [tutorial Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#installing_python_3)), quindi esegua nuovamente i comandi sopra per verificare se l'installazione ha avuto successo.

3. Se Python è impostato, navighi nella directory che contiene il codice del sito web che vuole testare, utilizzando il comando `cd`.

   ```bash
   # include the directory name to enter it, for example
   cd Desktop
   # use two dots to jump up one directory level if you need to
   cd ..
   ```

4. Inserisca il comando per avviare il server in quella directory:

   ```bash
   # On Windows, try "python -m http.server" or "py -3 -m http.server"
   python3 -m http.server
   ```

5. Per impostazione predefinita, questo eseguirà i contenuti della directory su un server web locale, sulla porta 8000. Può accedere a questo server andando all'URL `localhost:8000` nel suo browser web. Qui vedrà elencati i contenuti della directory — clicchi sul file HTML che vuole eseguire.

> [!NOTE]
> Se ha già qualcosa in esecuzione sulla porta 8000, può scegliere un'altra porta eseguendo il comando del server seguito da un numero di porta alternativo, ad esempio, `python3 -m http.server 7800`. Poi può accedere ai suoi contenuti su `localhost:7800`.

## Esecuzione di linguaggi lato server localmente

Il miglior approccio per lavorare con linguaggi lato server, come Python, PHP o JavaScript, dipende dal linguaggio lato server che sta utilizzando e se sta lavorando con un framework web o con codice "stand-alone".

Se sta lavorando con un framework web, di solito il framework fornirà il proprio server di sviluppo.
Ad esempio, i seguenti linguaggi/framework includono un server di sviluppo:

- Framework web Python, come [Django](/it/docs/Learn_web_development/Extensions/Server-side/Django), [Flask](https://flask.palletsprojects.com/), e [Pyramid](https://trypyramid.com/).
- Framework Node/JavaScript come [Express Web Framework (Node.js/JavaScript)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)
- PHP ha il proprio [server di sviluppo integrato](https://www.php.net/manual/en/features.commandline.webserver.php):

  ```bash
  cd path/to/your/php/code
  php -S localhost:8000
  ```

Se non sta lavorando direttamente con un framework lato server o un linguaggio di programmazione che fornisce un server di sviluppo, il modulo `http.server` di Python può anche essere utilizzato per testare codice lato server scritto in linguaggi come Python, PHP, JavaScript e così via, invocando script CGI (Common Gateway Interface) lato server. Per esempi su come utilizzare questa funzionalità, veda [Eseguire uno Script Remotamente Attraverso il Common Gateway Interface (CGI)](https://realpython.com/python-http-server/#execute-a-script-remotely-through-the-common-gateway-interface-cgi) in _How to Launch an HTTP Server in One Line of Python Code_ su realpython.com.
