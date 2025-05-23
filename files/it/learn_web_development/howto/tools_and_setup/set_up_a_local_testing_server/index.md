---
title: Come si configura un server di collaudo locale?
slug: Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo spiega come configurare un semplice server di collaudo locale sulla tua macchina e le basi su come utilizzarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Devi prima sapere
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
      <td>Imparerai come configurare un server di collaudo locale.</td>
    </tr>
  </tbody>
</table>

## File locali vs. file remoti

Nel corso della maggior parte dell'area di apprendimento, ti diciamo di aprire direttamente i tuoi esempi in un browser — questo può essere fatto facendo doppio clic sul file HTML, trascinandolo e rilasciandolo nella finestra del browser, o scegliendo _File_ > _Apri..._ e navigando fino al file HTML. Ci sono molti modi per ottenere questo risultato.

Se il percorso dell'indirizzo web inizia con `file://` seguito dal percorso del file sul tuo disco rigido locale, si sta utilizzando un file locale. Al contrario, se visualizzi uno dei nostri esempi ospitati su GitHub (o un esempio su un altro server remoto), l'indirizzo web inizierà con `http://` o `https://`, per indicare che il file è stato ricevuto tramite HTTP.

## Il problema con il collaudo di file locali

Alcuni esempi non funzioneranno se li apri come file locali. Questo può accadere per vari motivi, il più probabile dei quali è:

- **Contengono richieste asincrone**. Alcuni browser (incluso Chrome) non eseguiranno richieste asincrone (vedi [Learn: Making network requests with JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests)) se esegui semplicemente l'esempio da un file locale. Questo a causa di restrizioni di sicurezza (per maggiori informazioni sulla sicurezza web, leggi [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)).
- **Contengono un linguaggio lato server**. I linguaggi lato server (come PHP o Python) richiedono un server speciale per interpretare il codice e fornire i risultati.
- **Includono altri file**. I browser di solito trattano le richieste di caricamento delle risorse usando lo schema `file://` come richieste cross-origin.
  Quindi, se carichi un file locale che include altri file locali, questo potrebbe attivare un errore di {{Glossary("CORS", "CORS")}}.

## Esecuzione di un semplice server HTTP locale

Per aggirare il problema delle richieste asincrone, dobbiamo testare tali esempi eseguendoli attraverso un server web locale.

### Utilizzo di un'estensione nel tuo editor di codice

Se hai bisogno solo di HTML, CSS e JavaScript, e non di linguaggi lato server, il modo più semplice potrebbe essere quello di cercare estensioni nel tuo editor di codice. Oltre ad automatizzare l'installazione e la configurazione del tuo server HTTP locale, si integrano bene con i tuoi editor di codice. Testare file locali in un server HTTP può essere a un solo clic di distanza.

Per VS Code, prova le seguenti estensioni gratuite:

- [Live Preview](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server)
- [Preview on Web Server](https://marketplace.visualstudio.com/items?itemName=yuichinukiyama.vscode-preview-server)

### Utilizzo di Node.js

Il modulo Node.js [`http-server`](https://www.npmjs.com/package/http-server) è il modo più semplice per ospitare file HTML in qualsiasi directory.

Per usare il modulo:

1. Esegui i seguenti comandi per verificare se Node.js è già installato:

   ```bash
   node -v
   npm -v
   npx -v
   ```

2. Se Node.js non è installato, devi installarlo. Segui le [istruzioni di download](https://nodejs.org/en/download) nella documentazione di Node.js, quindi esegui nuovamente i comandi sopra per verificare se l'installazione è riuscita.

3. Supponiamo che la directory sia `/path/to/project`. Esegui il seguente comando per avviare il server:

   ```bash
   npx http-server /path/to/project -o -p 9999
   ```

   Questo ospiterà tutti i file nella directory `/path/to/project` su `localhost:9999`. L'opzione `-o` aprirà la pagina `index.html` in un browser web. Se `index.html` non esiste, verrà visualizzata la directory invece.

### Utilizzo di Python

Un altro modo per ottenere questo risultato è utilizzare il modulo `http.server` di Python.

> [!NOTE]
> Le versioni più vecchie di Python (fino alla versione 2.7) fornivano un modulo simile chiamato `SimpleHTTPServer`. Python 2 è già end-of-life, quindi raccomandiamo di usare Python 3.

Per fare ciò:

1. Esegui il seguente comando per verificare se Python è già installato:

   ```bash
   python -V
   # If the above fails, try:
   python3 -V
   # Or, if the "py" command is available, try:
   py -3 -V
   ```

2. Se Python non è installato, devi installarlo. Segui le [istruzioni di download](https://www.python.org/downloads/) nella documentazione di Python (abbiamo anche spiegazioni più dettagliate nel nostro [tutorial su Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#installing_python_3)), quindi esegui nuovamente i comandi sopra per verificare se l'installazione è riuscita.

3. Se Python è configurato, naviga nella directory che contiene il codice del sito web che desideri testare, usando il comando `cd`.

   ```bash
   # include the directory name to enter it, for example
   cd Desktop
   # use two dots to jump up one directory level if you need to
   cd ..
   ```

4. Inserisci il comando per avviare il server in quella directory:

   ```bash
   # On Windows, try "python -m http.server" or "py -3 -m http.server"
   python3 -m http.server
   ```

5. Per impostazione predefinita, questo eseguirà il contenuto della directory su un server web locale, sulla porta 8000. Puoi accedere a questo server andando all'URL `localhost:8000` nel tuo browser web. Qui vedrai elencato il contenuto della directory — clicca sul file HTML che vuoi eseguire.

> [!NOTE]
> Se hai già qualcosa in esecuzione sulla porta 8000, puoi scegliere un'altra porta eseguendo il comando del server seguito da un numero di porta alternativo, ad esempio `python3 -m http.server 7800`. Puoi quindi accedere al tuo contenuto a `localhost:7800`.

## Esecuzione di linguaggi lato server localmente

Il miglior approccio per lavorare con i linguaggi lato server, come Python, PHP, o JavaScript, dipende dal linguaggio lato server che stai utilizzando, e se stai lavorando con un framework web o con codice "autonomo".

Se stai lavorando con un framework web, di solito il framework fornirà il proprio server di sviluppo.
Ad esempio, i seguenti linguaggi/framework includono un server di sviluppo:

- Framework web Python, come [Django](/it/docs/Learn_web_development/Extensions/Server-side/Django), [Flask](https://flask.palletsprojects.com/) e [Pyramid](https://trypyramid.com/).
- Framework Node/JavaScript come [Express Web Framework (Node.js/JavaScript)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)
- PHP ha il suo [server di sviluppo integrato](https://www.php.net/manual/en/features.commandline.webserver.php):

  ```bash
  cd path/to/your/php/code
  php -S localhost:8000
  ```

Se non stai lavorando direttamente con un framework lato server o un linguaggio di programmazione che fornisce un server di sviluppo, il modulo `http.server` di Python può essere utilizzato anche per testare il codice lato server scritto in linguaggi come Python, PHP, JavaScript, e così via, invocando script Common Gateway Interface (CGI) lato server.
Per esempi su come utilizzare questa funzionalità, vedi [Eseguire uno Script da Remoto tramite il Common Gateway Interface (CGI)](https://realpython.com/python-http-server/#execute-a-script-remotely-through-the-common-gateway-interface-cgi) in _How to Launch an HTTP Server in One Line of Python Code_ su realpython.com.
