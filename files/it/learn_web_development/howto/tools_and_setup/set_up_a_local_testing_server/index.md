---
title: Come configurare un server di test locale?
slug: Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server
l10n:
  sourceCommit: b275a4cda2d6583bc065a680599cc6de5dbaa8f4
---

Questo articolo spiega come configurare un semplice server di test locale sul proprio computer e le basi del suo utilizzo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Occorre prima sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        > e
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server"
          >che cos'è un server Web</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come configurare un server di test locale.</td>
    </tr>
  </tbody>
</table>

## File locali e file remoti

Nella maggior parte dell'area di apprendimento, viene indicato di aprire gli esempi direttamente in un browser: è possibile farlo facendo doppio clic sul file HTML, trascinandolo nella finestra del browser oppure scegliendo _File_ > _Apri…_ e individuando il file HTML. Esistono molti modi per farlo.

Se il percorso dell'indirizzo web inizia con `file://`, seguito dal percorso del file sul disco rigido locale, viene utilizzato un file locale. Al contrario, se si visualizza uno dei nostri esempi ospitati su GitHub, o un esempio su un altro server remoto, l'indirizzo web inizierà con `http://` o `https://`, a indicare che il file è stato ricevuto tramite HTTP.

## Il problema nel testare file locali

Alcuni esempi non vengono eseguiti se aperti come file locali. Ciò può dipendere da vari motivi; i più probabili sono:

- **Contengono richieste asincrone**. Alcuni browser, incluso Chrome, non eseguono richieste async (vedere [Learn: effettuare richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests)) se l'esempio viene eseguito direttamente da un file locale. Questo è dovuto a restrizioni di sicurezza; per maggiori informazioni sulla sicurezza web, consultare [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security).
- **Contengono un linguaggio lato server**. I linguaggi lato server, come PHP o Python, richiedono un server speciale per interpretare il codice e fornire i risultati.
- **Includono altri file**. I browser in genere trattano le richieste di caricamento delle risorse che usano lo schema `file://` come richieste cross-origin.
  Pertanto, il caricamento di un file locale che include altri file locali può attivare un errore {{Glossary("CORS", "CORS")}}.

## Eseguire un semplice server HTTP locale

Per aggirare il problema delle richieste async, è necessario testare tali esempi eseguendoli tramite un server web locale.

### Usare un'estensione nell'editor di codice

Se servono solo HTML, CSS e JavaScript, senza alcun linguaggio lato server, il modo più semplice potrebbe essere cercare estensioni nell'editor di codice. Oltre ad automatizzare l'installazione e la configurazione del server HTTP locale, queste si integrano bene anche con gli editor di codice. Il test dei file locali in un server HTTP potrebbe essere a portata di clic.

Per VS Code, provare le seguenti estensioni gratuite:

- [Live Preview](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server)
- [Anteprima su Web Server](https://marketplace.visualstudio.com/items?itemName=yuichinukiyama.vscode-preview-server)

### Usare Node.js

Il modulo [`http-server`](https://www.npmjs.com/package/http-server) di Node.js è un modo semplice per ospitare file HTML in qualsiasi directory.

Per utilizzare il modulo:

1. Eseguire i seguenti comandi per verificare se Node.js è già installato:

   ```bash
   node -v
   npm -v
   npx -v
   ```

2. Se Node.js non è installato, occorre installarlo. Seguire le [istruzioni per il download](https://nodejs.org/en/download) nella documentazione di Node.js, quindi eseguire nuovamente i comandi precedenti per verificare che l'installazione sia riuscita.

3. Si supponga che la directory sia `/path/to/project`. Eseguire il seguente comando per avviare il server:

   ```bash
   npx http-server /path/to/project -o -p 9999
   ```

   Questo ospita tutti i file nella directory `/path/to/project` su `localhost:9999`. L'opzione `-o` aprirà la pagina `index.html` in un browser web. Se `index.html` non esiste, verrà invece visualizzata la directory.

### Usare Python

Un altro modo per farlo è utilizzare il modulo `http.server` di Python.

> [!NOTE]
> Le versioni precedenti di Python, fino alla versione 2.7, fornivano un modulo simile denominato `SimpleHTTPServer`. Python 2 ha già raggiunto la fine del ciclo di vita, pertanto è consigliato usare Python 3.

Per farlo:

1. Eseguire il seguente comando per verificare se Python è già installato:

   ```bash
   python -V
   # If the above fails, try:
   python3 -V
   # Or, if the "py" command is available, try:
   py -3 -V
   ```

2. Se Python non è installato, occorre installarlo. Seguire le [istruzioni per il download](https://www.python.org/downloads/) nella documentazione di Python; sono disponibili anche spiegazioni più dettagliate nel nostro [tutorial su Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#installing_python_3). Quindi eseguire nuovamente i comandi precedenti per verificare che l'installazione sia riuscita.

3. Se Python è configurato, passare alla directory che contiene il codice del sito web da testare, utilizzando il comando `cd`.

   ```bash
   # include the directory name to enter it, for example
   cd Desktop
   # use two dots to jump up one directory level if you need to
   cd ..
   ```

4. Inserire il comando per avviare il server in quella directory:

   ```bash
   # On Windows, try "python -m http.server" or "py -3 -m http.server"
   python3 -m http.server
   ```

5. Per impostazione predefinita, questo eseguirà il contenuto della directory su un server web locale, alla porta 8000. È possibile raggiungere questo server visitando l'URL `localhost:8000` nel browser web. Verrà visualizzato un elenco dei contenuti della directory: fare clic sul file HTML da eseguire.

> [!NOTE]
> Se è già presente un processo in esecuzione sulla porta 8000, è possibile scegliere un'altra porta eseguendo il comando del server seguito da un numero di porta alternativo, ad esempio `python3 -m http.server 7800`. Sarà quindi possibile accedere al contenuto all'indirizzo `localhost:7800`.

## Eseguire linguaggi lato server localmente

L'approccio migliore per lavorare con linguaggi lato server, come Python, PHP o JavaScript, dipende dal linguaggio lato server utilizzato e dal fatto che si stia lavorando con un framework web o con codice "autonomo".

Quando si lavora con un framework web, in genere il framework fornisce il proprio server di sviluppo.
Ad esempio, i seguenti linguaggi/framework includono un server di sviluppo:

- Framework web Python, come [Django](/it/docs/Learn_web_development/Extensions/Server-side/Django), [Flask](https://flask.palletsprojects.com/) e [Pyramid](https://trypyramid.com/).
- Framework Node/JavaScript come [Express Web Framework (Node.js/JavaScript)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)
- PHP dispone del proprio [server di sviluppo integrato](https://www.php.net/manual/en/features.commandline.webserver.php):

  ```bash
  cd path/to/your/php/code
  php -S localhost:8000
  ```

Se non si lavora direttamente con un framework lato server o con un linguaggio di programmazione che fornisce un server di sviluppo, il modulo `http.server` di Python può essere usato anche per testare codice lato server scritto in linguaggi come Python, PHP, JavaScript e così via, invocando script Common Gateway Interface (CGI) lato server.
Per esempi su come usare questa funzionalità, vedere [Eseguire uno script da remoto tramite Common Gateway Interface (CGI)](https://realpython.com/python-http-server/#execute-a-script-remotely-through-the-common-gateway-interface-cgi) in _How to Launch an HTTP Server in One Line of Python Code_ su realpython.com.
