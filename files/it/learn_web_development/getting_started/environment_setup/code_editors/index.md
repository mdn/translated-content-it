---
title: Editor di codice
slug: Learn_web_development/Getting_started/Environment_setup/Code_editors
l10n:
  sourceCommit: 62ab95d20f246369cfab654c5a7a8727deb21ea6
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Environment_setup")}}

In precedenza è stato consigliato di installare un editor di codice, poiché ne servirà uno per seguire questo percorso. In questo articolo gli editor di codice vengono esaminati più nel dettaglio, per dare un'idea di ciò che possono offrire.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base del sistema operativo del proprio computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Quali editor di codice sono disponibili e quali sono adatti ai propri scopi.</li>
          <li>Che cosa può fare un editor di codice di base.</li>
          <li>Che cosa possono fare le estensioni di un editor di codice e come installarne una.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali editor di codice sono disponibili?

Prima di iniziare a scrivere codice, potrebbe esserci già stata un po' di esperienza nel lavorare con documenti di testo in un programma come Microsoft Word. Potrebbe inoltre sorgere il dubbio se sia possibile lavorare con il codice negli stessi programmi. Sfortunatamente, la risposta è "non proprio":

- Programmi come Microsoft Word sono editor di **file binari**; i loro file contengono un formato non testuale che può essere compreso solo da tali programmi. Il codice sorgente di un sito web, invece, viene memorizzato come testo semplice.
- Word _può_ aprire e modificare file di testo semplice, ma non li gestisce molto bene. Non dispone di un insieme di funzionalità progettato per lavorare con il codice: serve per scrivere documenti come lettere e relazioni. È necessario un programma progettato per gestire e produrre testo semplice in modo appropriato e per lavorare con il codice.

Probabilmente sul computer è già presente un editor di testo semplice. Per impostazione predefinita, Windows include [Notepad](https://en.wikipedia.org/wiki/Microsoft_Notepad) e macOS include [TextEdit](https://en.wikipedia.org/wiki/TextEdit). Le distribuzioni Linux variano; la versione Ubuntu 22.04 LTS include per impostazione predefinita [GNOME Text Editor](https://en.wikipedia.org/wiki/GNOME_Text_Editor). Gli editor di testo semplice predefiniti del sistema operativo possono andare bene, ma dispongono anche di un insieme limitato di funzionalità.

È preferibile utilizzare un editor di codice completo come [Visual Studio Code](https://code.visualstudio.com/) (multipiattaforma, gratuito), [Sublime Text](https://www.sublimetext.com/) (multipiattaforma, non gratuito) o [Notepad++](https://notepad-plus-plus.org/) (Windows, gratuito).

Si consiglia Visual Studio Code (VS Code), poiché è l'editor utilizzato più spesso. Se VS Code (o un altro editor di codice) non è ancora installato, occorre [installarlo prima di procedere](https://code.visualstudio.com/).

> [!NOTE]
> Gli ambienti di sviluppo integrati (IDE) come [NetBeans](https://netbeans.apache.org/front/main/index.html) (multipiattaforma, gratuito) e [WebStorm](https://www.jetbrains.com/webstorm/) (multipiattaforma, non gratuito) dispongono di più funzionalità rispetto ai semplici editor di codice, ma tendono a essere più complessi di quanto necessario in questa fase del percorso di apprendimento.

## Funzionalità di base di un editor di codice

In questa sezione verranno esaminate alcune delle funzionalità più importanti presenti negli editor di codice, descrivendo come possono aiutare nel lavoro di programmazione.

> [!NOTE]
> Le sezioni seguenti illustrano solo una piccola parte di ciò che può fare un editor di codice. Per un elenco più completo delle funzionalità, consultare la [documentazione di Visual Studio Code](https://code.visualstudio.com/docs) (oppure cercare sul web la documentazione dell'editor di codice scelto, se ne viene usato uno diverso).

> [!NOTE]
> Per chi utilizza soltanto la tastiera, VS Code dispone di un potente insieme di scorciatoie da tastiera. Consultare il riferimento alle [scorciatoie da tastiera predefinite](https://code.visualstudio.com/docs/reference/default-keybindings) di VS Code.

### Apertura e modifica dei file

Questo può sembrare un punto ovvio, ma installare un editor di codice è utile perché fornisce una singola applicazione in grado di aprire tutti i file di codice che potrebbero essere usati durante il lavoro di sviluppo. Non c'è nulla di più fastidioso che fare doppio clic su un file nel computer e vederlo aprirsi in un'applicazione casuale e non correlata, oppure ricevere dal sistema operativo il messaggio che quel file non viene riconosciuto.

Durante l'installazione di VS Code tutto questo dovrebbe avvenire automaticamente, ma se persistono problemi con determinati tipi di file, è possibile impostarli manualmente affinché vengano aperti con tale applicazione. La procedura può variare a seconda del sistema operativo; per scoprirla, usare il motore di ricerca preferito e cercare "choose what application opens a file type &lt;OS-name-and-number>" — per esempio, "choose what application opens a file type windows 11" se si usa Windows 11.

Nel prossimo articolo sono disponibili molte più informazioni sull'apertura e la modifica di file e cartelle.

### Evidenziazione della sintassi

Gli editor di codice come VS Code forniscono l'evidenziazione della sintassi: le funzionalità di codice riconosciute mostrano parti diverse in colori diversi. Ciò rende il codice molto più facile da leggere rispetto a visualizzarlo tutto in un unico colore. Usiamo come esempio la seguente funzione JavaScript:

```js
function createGreeting(name) {
  const greeting = `Hello, ${name}!`;
  return greeting;
}
```

Per ora non è necessario capire cosa faccia questo codice, ma è già possibile vedere sopra l'aspetto dell'evidenziazione della sintassi. Sì, anche MDN fornisce l'evidenziazione della sintassi!

Proviamo un esercizio in VS Code:

1. Copiare negli appunti l'esempio di codice precedente (i blocchi di codice di MDN hanno un'icona di copia nell'angolo superiore destro su cui è possibile fare clic).
2. Aprire VS Code e creare un nuovo file scegliendo _File_ > _New File..._
3. Nel nuovo file, fare clic sul testo _Select a language_, quindi scegliere _JavaScript_ dal menu a discesa che si apre.
4. Incollare il codice nel nuovo file per vedere l'aspetto dell'evidenziazione della sintassi JavaScript di VS Code.

VS Code offre anche altre funzionalità relative alla sintassi. Per esempio:

- Verrà visualizzata una sottile linea verticale che scende dalla parola chiave `function` alla parentesi graffa di chiusura (`}`): queste linee vengono usate per contrassegnare i diversi livelli di [rientro](https://en.wikipedia.org/wiki/Indentation_style) nel codice, rendendo più facile identificare dove iniziano e terminano i blocchi.
- Provare anche a spostare il cursore di testo lampeggiante sulla parentesi graffa di apertura o di chiusura (`{` o `}`): verranno evidenziate entrambe. Questo aiuta anche a identificare l'inizio e la fine dei blocchi ed è utile quando si cerca di capire dove manca un carattere in una struttura più complicata con molti blocchi annidati. Questa evidenziazione funziona anche con altri delimitatori, quali parentesi tonde (`(` e `)`) e parentesi quadre (`[` e `]`).

### Completamento/suggerimento del codice

Quando si digita codice in un editor di codice, spesso questo è in grado di suggerire cosa digitare successivamente e di compilare parte del codice standard (ovvero codice che sarà sempre uguale).

Provarlo ora in VS Code:

1. Tornare al file JavaScript creato nella sezione precedente.
2. Andare alla fine del file e premere <kbd>Enter</kbd>/<kbd>Return</kbd> un paio di volte per assicurarsi di trovarsi su una nuova riga.
3. Iniziare a digitare "function": a destra del testo dovrebbe comparire un elenco di opzioni.
4. Selezionare l'opzione _function_ che riporta _Function Statement_ sulla destra. Verrà compilato il codice seguente:

   ```js-nolint
   function name(params) {

   }
   ```

5. Fare clic all'interno della funzione, sulla riga vuota tra le due parentesi graffe. Iniziare a digitare "document" e verrà nuovamente mostrato un elenco di opzioni. Selezionare la prima. Si tratta di un riferimento all'oggetto [`Document`](/it/docs/Web/API/Document) (anche in questo caso, per ora non è necessario preoccuparsi del suo significato).
6. Subito dopo `document`, digitare un punto (`.`): verrà visualizzato di nuovo un elenco di opzioni, questa volta contenente tutte le proprietà e i metodi disponibili sull'oggetto `document`!

Per ora è sufficiente. Proseguiamo.

### Aiuto per il debug

Gli editor di codice non possono correggere automaticamente tutti i problemi del codice, ma possono certamente aiutare a individuare errori di battitura e altri semplici errori. Vediamo un paio di esempi.

1. Tornare al file JavaScript ed eliminare tutto il codice attualmente presente. Sostituirlo con quanto segue:

   ```js-nolint example-bad
   function createGreeting(name) {
     const greeting = `Hello, ${Name}!`;
     return greeting;
   }

   const helloChris = createGreeting("Chris);

   console.log(helloChris;
   ```

2. La piccola icona a forma di croce a destra dell'elenco di codice precedente è il modo in cui MDN indica un cattivo esempio di codice, e a ragione: il codice precedente contiene tre errori! Osservare l'evidenziazione di VS Code per verificare se è possibile individuare come ha evidenziato gli errori, quindi verranno esaminati e corretti insieme.
3. Il primo errore consiste nell'uso di `name` nella prima riga, ma di `Name` nella seconda riga per riferirsi alla stessa variabile. Questo è un problema perché JavaScript distingue tra maiuscole e minuscole e quindi considera questi due nomi differenti. VS Code ha evidenziato questo aspetto in due modi diversi: colorando `name` in grigio scuro per indicare che il valore è dichiarato ma non viene mai usato (spesso un buon segnale che è stato commesso un errore di battitura da qualche parte) e mettendo tre puntini sotto `Name` per indicare che dispone di un suggerimento su come migliorare il codice (in questo caso, chiedendo se si intendeva scrivere `name`). Per correggere questo errore, modificare `Name` in `name`.
   > [!NOTE]
   > È possibile passare il puntatore del mouse su ciascuna delle evidenziazioni indicate per ottenere maggiori informazioni.
4. Il secondo errore si trova nella sesta riga, dove viene scritto `"Chris`. In JavaScript, una porzione di testo (nota come **stringa**) deve essere racchiusa tra due virgolette, ma manca la seconda. VS Code ha evidenziato questo problema sottolineando con una linea rossa ondulata il testo in cui viene rilevato per la prima volta l'errore (potrebbe non essere il punto esatto in cui si trova effettivamente l'errore), in modo simile a quella utilizzata in Microsoft Word per evidenziare gli errori ortografici. Per correggerlo, aggiornare `"Chris` in `"Chris"`.
5. Sull'ultima riga rimane una piccola parte di sottolineatura rossa ondulata verso la fine, anche dopo aver corretto l'errore precedente. Questo è dovuto al terzo errore: in JavaScript, una parentesi di apertura necessita sempre di una parentesi di chiusura corrispondente. Correggere il problema aggiornando `(helloChris` in `(helloChris)`.

### Cerca e sostituisci

Ogni editor di codice degno di questo nome dispone di una solida funzione di ricerca e sostituzione. È utile, ad esempio, se si scopre che un errore si verifica in una funzione specifica e la si vuole trovare nel codice, oppure se si decide di modificare il nome di una variabile e occorre assicurarsi che venga modificato in tutti i punti che vi fanno riferimento.

Il concetto di ricerca e sostituzione dovrebbe essere abbastanza familiare a chi ha già usato un computer, ma esaminiamolo rapidamente per completezza:

1. Tornare al file JavaScript in VS Code e aprire il pannello di ricerca e sostituzione in modalità di ricerca scegliendo _Edit_ > _Find_ dal menu.
2. Digitare `createGreeting` nella casella _Find_: entrambe le occorrenze verranno evidenziate e sarà possibile spostarsi tra di esse con le frecce su e giù nel pannello. L'occorrenza attualmente evidenziata in modo attivo ha un'evidenziazione più luminosa.
3. Ora aprire il pannello di ricerca e sostituzione in modalità di sostituzione scegliendo _Edit_ > _Replace_ dal menu, oppure facendo clic sulla freccia a sinistra della casella _Find_.
4. Digitare `sayHello` nella casella _Replace_ che ora dovrebbe essere visibile.
5. Ora è possibile sostituire tutte le occorrenze di `createGreeting` nel codice con `sayHello` usando i due pulsanti a destra della casella _Replace_. Il pulsante sinistro passa all'occorrenza successiva della stringa cercata con un clic e la sostituisce con un secondo clic. Il pulsante destro sostituisce tutte le occorrenze con un solo clic.

VS Code dispone di molte potenti funzionalità di ricerca e sostituzione: vedere [Find and replace](https://code.visualstudio.com/docs/editing/codebasics#_find-and-replace).

## Potenziare l'editor di codice con le estensioni

La maggior parte degli editor di codice dispone di un sistema di estensioni o plugin che consente di aggiungere al programma funzionalità non disponibili per impostazione predefinita. Queste possono svolgere varie attività, come:

- Abilitare funzionalità di completamento del codice, linting o debug per linguaggi non supportati per impostazione predefinita, oppure fornire funzionalità aggiuntive per quelli supportati.
- Consentire l'uso delle funzionalità di altri strumenti direttamente dall'editor di codice, come strumenti di controllo delle versioni o server di test locali.
- Fornire temi/schemi di colori aggiuntivi per l'interfaccia utente o l'evidenziazione del codice.
- Suggerire frammenti di codice per soddisfare requisiti. Questi possono essere generati da modelli statici o tramite strumenti di IA. L'uso dell'IA per generare frammenti di codice presenta molti degli stessi vantaggi e limiti del suo uso per generare risultati di ricerca (per maggiori informazioni, vedere [Ricerca di informazioni > Uso dell'IA](/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web#using_ai)).

### Esplorazione delle estensioni di VS Code

Le estensioni di VS Code vengono gestite tramite il pannello Extensions Marketplace in VS Code, accessibile dal menu _View_ > _Extensions_. Esploriamolo ora.

1. Aprire il pannello Extensions Marketplace.
2. Nella casella _Search..._ nella parte superiore del pannello, digitare "JavaScript" per vedere quali estensioni relative a JavaScript sono disponibili. Provare a fare clic su alcuni dei risultati di ricerca visualizzati per osservare che tipo di funzionalità offrono. Per ora non installarne nessuna.
3. Installiamo invece un'estensione facile da comprendere e utile per praticamente qualsiasi file di codice usato in questo insieme di moduli. Digitare "Prettier" nella casella _Search..._ e fare clic sul risultato _Prettier - code formatter_. Quando l'estensione [Prettier](https://prettier.io/) è installata, può essere utilizzata per formattare il codice ogni volta che viene salvato un file, rendendolo di conseguenza molto più facile da leggere.
4. Fare clic sul pulsante _Install_ nella scheda _Extension_. Chiudere la scheda al termine dell'installazione.
5. Per far funzionare Prettier, occorre aggiornare un paio di impostazioni. Aprire la scheda delle impostazioni di VS Code (_Code_ > _Settings..._ > _Settings_ su macOS, _File_ > _Preferences_ > _Settings_ su Windows).
6. Nella casella _Search settings_ in alto, digitare "formatter" per filtrare l'elenco delle impostazioni e mostrare solo quelle che contengono "formatter".
7. Trovare l'opzione _Editor: Default Formatter_ e selezionare l'opzione _Prettier - Code formatter_ dal menu a discesa associato.
8. Trovare l'opzione _Editor: Format On Save_ e abilitarla facendo clic sulla relativa casella di controllo.
9. Chiudere la scheda _Settings_.

La configurazione è completata; vediamo Prettier in azione.

1. Tornare alla scheda del file JavaScript e salvarlo (_File_ > _Save_). Perché Prettier funzioni, il file deve essere salvato. Chiamarlo `test.js`. La posizione in cui viene salvato non ha particolare importanza.
2. Sostituire il contenuto attuale con il codice seguente:

   ```js-nolint example-bad
   function sayHello(name){const greeting = `Hello, ${name}!`;
   return greeting;}
   ```

3. Salvare nuovamente il file; a questo punto, Prettier dovrebbe riformattare il codice in modo ordinato, così:

   ```js
   function sayHello(name) {
     const greeting = `Hello, ${name}!`;
     return greeting;
   }
   ```

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Environment_setup")}}
