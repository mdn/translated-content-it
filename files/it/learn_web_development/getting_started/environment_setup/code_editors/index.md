---
title: Editor di codice
slug: Learn_web_development/Getting_started/Environment_setup/Code_editors
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Environment_setup")}}

Precedentemente, ti abbiamo detto di installare un editor di codice, poiché ne avrai bisogno per completare questo percorso. In questo articolo esaminiamo gli editor di codice in modo più dettagliato, dandoti un'idea di cosa possono fare per te.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il tuo sistema operativo del computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Quali editor di codice sono disponibili e quali sono adatti ai tuoi scopi.</li>
          <li>Cosa può fare un editor di codice di base.</li>
          <li>Cosa possono fare le estensioni degli editor di codice e come installarne una.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali editor di codice sono disponibili?

Prima di iniziare a programmare, potresti aver avuto un po' di esperienza nel lavorare su documenti di testo in un programma come Microsoft Word. Potresti anche chiederti se puoi lavorare con il codice in questi stessi programmi. Purtroppo, la risposta è "non proprio":

- Programmi come Microsoft Word sono editor di file **binarie**; i loro file contengono un formato non testuale che può essere compreso solo da quei programmi. Il codice sorgente del sito web, invece, è memorizzato come testo semplice.
- Word _può_ aprire e modificare file di testo semplice, ma non li gestisce molto bene. Non ha un insieme di funzionalità progettate per lavorare con il codice — è per scrivere documenti come lettere e rapporti. Hai bisogno di un programma progettato per gestire e produrre testo semplice e lavorare con il codice.

Probabilmente hai già un editor di testo semplice sul tuo computer. Di default, Windows include [Notepad](https://en.wikipedia.org/wiki/Microsoft_Notepad) e macOS viene fornito con [TextEdit](https://en.wikipedia.org/wiki/TextEdit). Le distribuzioni Linux variano; il rilascio di Ubuntu 22.04 LTS viene fornito con [GNOME Text Editor](https://en.wikipedia.org/wiki/GNOME_Text_Editor) di default. Gli editor di testo semplice di default del sistema operativo possono andare bene, ma hanno anche un insieme di funzionalità limitato.

È meglio optare per un editor di codice completo come [Visual Studio Code](https://code.visualstudio.com/) (multipiattaforma, gratuito), [Sublime Text](https://www.sublimetext.com/) (multipiattaforma, non gratuito), o [Notepad++](https://notepad-plus-plus.org/) (Windows, gratuito).

Raccomandiamo Visual Studio Code (VS Code), poiché è l'editor che utilizziamo principalmente. Se non hai già installato VS Code (o un altro editor di codice), dovresti [installarlo prima di procedere](https://code.visualstudio.com/).

> [!NOTE]
> Gli Ambienti di Sviluppo Integrato (IDE) come [NetBeans](https://netbeans.apache.org/front/main/index.html) (multipiattaforma, gratuito), e [WebStorm](https://www.jetbrains.com/webstorm/) (multipiattaforma, non gratuito) tendono ad avere più funzionalità rispetto a semplici editor di codice ma tendono ad essere più complessi di quanto tu abbia bisogno in questo stadio del tuo percorso di apprendimento.

## Funzionalità di base degli editor di codice

In questa sezione, esamineremo alcune delle funzionalità più significative che puoi trovare negli editor di codice, descrivendo come possono aiutarti nel tuo lavoro di programmazione.

> [!NOTE]
> Le sezioni seguenti forniscono solo una panoramica delle potenzialità di un editor di codice. Per un elenco di funzionalità più completo, consulta la [documentazione di Visual Studio Code](https://code.visualstudio.com/docs) (o cerca sul web la documentazione del tuo editor di codice scelto se stai utilizzando qualcosa di diverso).

> [!NOTE]
> Se utilizzi solo la tastiera, sappi che VS Code ha un potente set di scorciatoie da tastiera. Vedi il [Riferimento scorciatoie tastiera predefinite di VS Code](https://code.visualstudio.com/docs/reference/default-keybindings).

### Apertura e modifica di file

Questo potrebbe sembrare un punto ovvio, ma installare un editor di codice è utile perché ti darà un'unica app che aprirà tutti i file di codice che potresti voler utilizzare durante il tuo lavoro di sviluppo. Non c'è niente di più fastidioso che fare doppio click su un file sul tuo computer e farlo aprire in un'app a caso, non correlata, o che il tuo sistema operativo ti dica che non riconosce quel file.

Questo dovrebbe accadere automaticamente quando installi VS Code, ma se hai ancora problemi con certi tipi di file, puoi impostarli manualmente per aprirli tramite quell'app. Questo può variare a seconda del tuo sistema operativo, quindi per scoprirlo, vai al tuo motore di ricerca preferito e cerca "scegli quale applicazione apre un tipo di file &lt;nome-e-versione-OS>" — per esempio, "scegli quale applicazione apre un tipo di file windows 11" se sei su Windows 11.

Puoi trovare molte più informazioni sull'apertura e la modifica di file e cartelle nel nostro prossimo articolo.

### Colorazione sintattica

Gli editor di codice come VS Code forniscono la colorazione sintattica — ovvero, le caratteristiche riconosciute del codice vengono mostrate in colori diversi. Questo rende il codice molto più facile da leggere rispetto a colorarlo tutto in un unico colore. Usciamo il seguente esempio di funzione JavaScript:

```js
function createGreeting(name) {
  const greeting = `Hello, ${name}!`;
  return greeting;
}
```

Non è necessario capire cosa stia facendo questo codice per ora, ma puoi già vedere come appare la colorazione sintattica sopra. Sì, anche noi forniamo la colorazione sintattica su MDN!

Proviamo un esercizio in VS Code:

1. Copia l'esempio di codice sopra negli appunti (i blocchi di codice MDN hanno un'icona di copia nell'angolo in alto a destra su cui puoi premere per farlo).
2. Apri VS Code e crea un nuovo file scegliendo _File_ > _New File..._
3. All'interno del nuovo file, clicca sul testo _Select a language_, quindi scegli _JavaScript_ dal menu a discesa che si apre.
4. Incolla il codice nel nuovo file per vedere come appare la colorazione sintattica di JavaScript di VS Code.

VS Code fornisce anche altre funzionalità sintattiche. Ad esempio:

- Vedrai una sottile linea verticale percorrere dalla parola chiave `function` fino alla parentesi graffa chiusa (`}`) — queste linee sono usate per segnare livelli diversi di [indentazione](https://en.wikipedia.org/wiki/Indentation_style) nel codice, rendendo più facile identificare dove iniziano e finiscono i blocchi.
- Prova anche a spostare il cursore testuale lampeggiante sopra la parentesi graffa aperta o chiusa (`{` o `}`) — vedrai entrambe evidenziate. Questo aiuta anche a identificare l'inizio e la fine dei blocchi ed è utile quando stai cercando di trovare dove manca un carattere in una struttura più complicata con molti blocchi nidificati. Questo evidenziamento funziona anche con altri delimitatori come le parentesi tonde (`(` e `)`) e le parentesi quadre (`[` e `]`).

### Completamento/suggerimento del codice

Quando digiti il codice in un editor di codice, spesso sarà in grado di suggerire cosa dovresti digitare successivamente e compilare un po' di codice boilerplate per te (ossia il codice standard che sarà sempre lo stesso).

Prova questo ora in VS Code:

1. Torna al file JavaScript che hai creato nella sezione precedente.
2. Vai in fondo al file e premi <kbd>Enter</kbd>/<kbd>Return</kbd> un paio di volte per assicurarti di essere su una nuova riga.
3. Inizia a digitare "function" — una lista di opzioni dovrebbe apparire in un elenco a destra del tuo testo.
4. Seleziona l'opzione _function_ con _Function Statement_ scritto alla sua destra. Sarà compilato il seguente codice per te:

   ```js-nolint
   function name(params) {

   }
   ```

5. Clicca all'interno della funzione sulla riga vuota tra le due parentesi graffe. Inizia a digitare "document" e riceverai di nuovo un elenco di opzioni. Seleziona la prima. Questo è un riferimento all'oggetto [`Document`](/it/docs/Web/API/Document) (ancora una volta, non preoccuparti di cosa significhi per ora).
6. Subito dopo `document`, digita un punto (`.`) — riceverai di nuovo un elenco di opzioni, questa volta contenente tutte le proprietà e i metodi disponibili sull'oggetto `document`!

Per ora basta così. Andiamo avanti.

### Aiuto per il debug

Gli editor di codice non possono correggere automaticamente tutti i tuoi problemi di codice, ma possono certamente aiutarti a trovare errori di battitura e altri errori semplici. Vediamo un paio di esempi.

1. Torna al tuo file JavaScript e cancella tutto il codice che hai attualmente lì. Sostituiscilo con il seguente:

   ```js-nolint example-bad
   function createGreeting(name) {
     const greeting = `Hello, ${Name}!`;
     return greeting;
   }

   const helloChris = createGreeting("Chris);

   console.log(helloChris;
   ```

2. L'icona della piccola croce a destra dell'elenco del codice sopra è il modo MDN per indicare un esempio di codice errato, e giustamente — ci sono tre errori nel codice sopra! Dai un'occhiata all'evidenziazione di VS Code per vedere se puoi individuare come ha evidenziato gli errori, poi passeremo insieme attraverso la loro correzione.
3. Il primo errore è che abbiamo usato `name` sulla prima riga, ma `Name` sulla seconda riga per riferirsi alla stessa variabile. Questo è un problema perché JavaScript fa distinzione tra maiuscole e minuscole e quindi considera questi come due nomi diversi. VS Code ha evidenziato questo in due modi diversi — colorando `name` di grigio scuro per indicare che il valore è dichiarato ma mai usato (spesso una buona indicazione che hai commesso un errore di battitura da qualche parte) e mettendo tre puntini sotto `Name` per indicare che ha un suggerimento per te su come migliorare il codice (in questo caso chiedendo se intendevi scrivere `name`). Per risolvere questo errore, cambia `Name` in `name`.
   > [!NOTE]
   > Puoi passare sopra ciascuno degli evidenziamenti indicati con il puntatore del mouse per ottenere maggiori informazioni.
4. Il secondo errore è sulla sesta riga, dove scriviamo `"Chris`. In JavaScript, un pezzo di testo (noto come **stringa**) deve essere racchiuso in due virgolette, ma la seconda manca. VS Code ha evidenziato questo sottolineando il testo dove l'errore è notato per primo (potrebbe non essere l'esatto luogo in cui effettivamente si trova l'errore) con una linea rossa ondulata, simile a quella utilizzata in Microsoft Word per evidenziare errori di ortografia. Per correggerlo, aggiorna `"Chris` a `"Chris"`.
5. Sull'ultima riga, rimane una piccola sottolineatura rossa ondulata vicino alla fine, anche dopo aver corretto l'errore precedente. Questo è a causa del terzo errore — in JavaScript, una parentesi aperta ha sempre bisogno di una parentesi chiusa corrispondente. Risolvi questo aggiornando `(helloChris` a `(helloChris)`.

### Ricerca e sostituzione

Ogni buon editor di codice ha una solida funzione di ricerca e sostituzione. Questo è utile, ad esempio, se scopri che un errore si verifica in una funzione specifica e vuoi trovarlo nel tuo codice, o se decidi di cambiare il nome di una variabile e devi assicurarti che venga cambiato in tutti i luoghi che lo riferiscono.

Il concetto di ricerca e sostituzione dovrebbe esserti familiare se hai già utilizzato un computer, ma esploriamolo rapidamente per completezza:

1. Torna al tuo file JavaScript in VS Code e apri il pannello di ricerca e sostituzione in modalità ricerca scegliendo _Modifica_ > _Trova_ dal menu.
2. Digita `createGreeting` nella casella _Trova_ — vedrai che entrambe le istanze sono evidenziate e puoi muoverti tra esse con le frecce su e giù nel pannello. L'istanza attivamente evidenziata ha l'evidenziazione più luminosa.
3. Ora apri il pannello di ricerca e sostituzione in modalità sostituzione scegliendo _Modifica_ > _Sostituisci_ dal menu, o cliccando sulla freccia a sinistra della casella _Trova_.
4. Digita `sayHello` nella casella _Sostituisci_ che dovrebbe ora essere visibile.
5. Ora puoi sostituire tutte le istanze di `createGreeting` nel codice con `sayHello` utilizzando i due pulsanti a destra della casella _Sostituisci_. Il pulsante sinistro passa all'istanza successiva della stringa di ricerca con un click e la sostituisce con un secondo click. Il pulsante destro sostituisce tutte le istanze con un solo click.

VS Code ha molte potenti funzionalità di ricerca e sostituzione — vedi [Trova e sostituisci](https://code.visualstudio.com/docs/editing/codebasics#_find-and-replace).

## Potenziare il tuo editor di codice con estensioni

La maggior parte degli editor di codice ha un sistema di estensione o plugin per permetterti di aggiungere funzionalità al programma che non sono disponibili di default. Queste possono svolgere una varietà di compiti, come:

- Abilitare funzionalità di completamento del codice, linting o debugging per lingue non supportate di default, o fornire funzionalità aggiuntive per quelle che lo sono.
- Permetterti di utilizzare la funzionalità di altri strumenti direttamente all'interno dell'editor di codice, come strumenti di controllo versione o server di test locali.
- Fornire ulteriori temi/schemi di colore per l'interfaccia utente o per l'evidenziazione del codice.
- Suggerire snippet di codice per soddisfare le esigenze. Questi possono essere generati da modelli statici, o tramite strumenti di AI. Usare l'AI per generare snippet di codice ha molti degli stessi vantaggi e avvertenze che ha usarla per generare risultati di ricerca (vedi [Cercare informazioni > Usare l'AI](/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web#using_ai) per ulteriori informazioni).

Le estensioni di VS Code sono gestite tramite il pannello del Marketplace delle Estensioni in VS Code, accessibile tramite il menu _Visualizza_ > _Estensioni_. Esploriamolo ora.

1. Apri il pannello del Marketplace delle Estensioni.
2. Nella casella _Search..._ in alto, digita "JavaScript" per vedere quali estensioni relative a JavaScript sono disponibili. Prova a cliccare su alcuni dei risultati di ricerca che appaiono per vedere il tipo di cose che fanno. Per ora non installare nessuna di esse.
3. Invece, installiamo un'estensione che è facile da capire e sarà utile per praticamente qualsiasi file di codice con cui lavori in questo set di moduli. Digita "Prettier" nella casella _Search..._ e clicca sul risultato _Prettier - code formatter_. Quando l'estensione [Prettier](https://prettier.io/) è installata, può essere utilizzata per formattare il tuo codice ogni volta che salvi un file, rendendo molto più facile da leggere.
4. Clicca sul pulsante _Install_ nella scheda _Extension_. Chiudi la scheda quando l'installazione è completata.
5. Per far funzionare Prettier, è necessario aggiornare un paio di impostazioni. Apri la scheda delle Impostazioni di VS Code (_Codice_ > _Impostazioni..._ > _Impostazioni_ su macOS, _File_ > _Preferenze_ > _Impostazioni_ su Windows).
6. Nella casella _Search settings_ in alto, digita "formatter" per filtrare l'elenco delle impostazioni e mostrare solo quelle che contengono "formatter".
7. Trova l'opzione _Editor: Default Formatter_, e seleziona l'opzione _Prettier - Code formatter_ dal menu a discesa associato.
8. Trova l'opzione _Editor: Format On Save_ e abilitala cliccando la sua casella di controllo.
9. Chiudi la scheda _Impostazioni_.

Abbiamo finito con l'installazione; vediamo Prettier in azione.

1. Torna alla scheda del tuo file JavaScript e salvalo (_File_ > _Salva_). Il file deve essere salvato affinché Prettier funzioni. Chiamalo `test.js`. La posizione in cui lo salvi non ha molta importanza.
2. Sostituisci il contenuto attuale con il seguente codice:

   ```js-nolint example-bad
   function sayHello(name){const greeting = `Hello, ${name}!`;
   return greeting;}
   ```

3. Salva di nuovo il file; a questo punto, Prettier dovrebbe riformattare il codice in modo ordinato, così:

   ```js
   function sayHello(name) {
     const greeting = `Hello, ${name}!`;
     return greeting;
   }
   ```

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Environment_setup")}}
