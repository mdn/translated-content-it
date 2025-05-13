---
title: Editor di codice
slug: Learn_web_development/Getting_started/Environment_setup/Code_editors
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Environment_setup")}}

In precedenza, le abbiamo detto di installare un editor di codice, poiché ne avrà bisogno per seguire questo percorso. In questo articolo analizziamo gli editor di codice in modo più dettagliato, dandole un'idea di cosa possono fare per lei.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del proprio computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Quali editor di codice sono disponibili e quali sono adatti ai suoi scopi.</li>
          <li>Cosa può fare un editor di codice di base.</li>
          <li>Cosa possono fare le estensioni degli editor di codice e come installarne una.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali editor di codice sono disponibili?

Prima di iniziare a programmare, potrebbe avere avuto un'esperienza di lavoro su documenti di testo in un programma come Microsoft Word. Potrebbe anche chiedersi se può lavorare con il codice negli stessi programmi. Sfortunatamente, la risposta è "non proprio":

- Programmi come Microsoft Word sono editor di file **binarie**; i loro file contengono un formato non-testuale che può essere compreso solo da quei programmi. Il codice sorgente del sito web, d'altra parte, è memorizzato come testo semplice.
- Word _può_ aprire e modificare file di testo semplice, ma non li gestisce molto bene. Non dispone di un set di funzionalità progettato per lavorare con il codice — è per scrivere documenti come lettere e relazioni. Ha bisogno di un programma progettato per gestire e produrre testo semplice in modo pulito e lavorare con il codice.

Probabilmente ha già un editor di testo semplice sul suo computer. Per impostazione predefinita, Windows include [Notepad](https://en.wikipedia.org/wiki/Microsoft_Notepad) e macOS dispone di [TextEdit](https://en.wikipedia.org/wiki/TextEdit). Le distribuzioni Linux variano; la versione Ubuntu 22.04 LTS viene fornita con [GNOME Text Editor](https://en.wikipedia.org/wiki/GNOME_Text_Editor) per impostazione predefinita. Gli editor di testo semplice di sistema operativo predefiniti possono andare bene, ma hanno anche un set di funzionalità limitato.

È meglio utilizzare un editor di codice completo come [Visual Studio Code](https://code.visualstudio.com/) (multipiattaforma, gratuito), [Sublime Text](https://www.sublimetext.com/) (multipiattaforma, non gratuito) o [Notepad++](https://notepad-plus-plus.org/) (Windows, gratuito).

Consigliamo Visual Studio Code (VS Code), poiché è l'editor che utilizziamo principalmente. Se non ha già installato VS Code (o un altro editor di codice), dovrebbe [installarlo prima di procedere](https://code.visualstudio.com/).

> [!NOTE]
> Gli Ambienti di Sviluppo Integrati (IDEs) come [NetBeans](https://netbeans.apache.org/front/main/index.html) (multipiattaforma, gratuito), e [WebStorm](https://www.jetbrains.com/webstorm/) (multipiattaforma, non gratuito) tendono ad avere più funzionalità rispetto agli editor di codice semplici ma sono solitamente più complessi di quanto necessario a questo punto del suo percorso di apprendimento.

## Funzionalità di base degli editor di codice

In questa sezione, esamineremo alcune delle funzionalità più significative che si trovano negli editor di codice, descrivendo come possono aiutarla nel suo lavoro di programmazione.

> [!NOTE]
> Le sezioni seguenti rappresentano solo una panoramica delle capacità di un editor di codice. Per un elenco più completo delle funzionalità, consultare la [documentazione di Visual Studio Code](https://code.visualstudio.com/docs) (oppure cerchi sul web la documentazione dell'editor di codice scelto se ne usa uno diverso).

> [!NOTE]
> Se utilizza solo la tastiera, sappia che VS Code ha un potente set di scorciatoie da tastiera. Veda il [Riferimento scorciatoie da tastiera predefinite di VS Code](https://code.visualstudio.com/docs/reference/default-keybindings).

### Apertura e modifica dei file

Potrebbe sembrare un punto ovvio, ma installare un editor di codice è utile perché le fornirà un'unica app che aprirà tutti i file di codice che potrebbe voler utilizzare nel suo lavoro di sviluppo. Non c'è niente di più fastidioso che fare doppio clic su un file sul proprio computer e vederlo aprire in un'applicazione casuale non correlata, o che il sistema operativo le dica che non riconosce quel file.

Questo dovrebbe avvenire automaticamente quando si installa VS Code, ma se ha ancora problemi con determinati tipi di file, può impostarli manualmente per aprirli tramite quell'app. Questo può variare a seconda del sistema operativo, quindi per scoprirlo, vada al suo motore di ricerca preferito e cerchi "scegli quale applicazione apre un tipo di file &lt;OS-nome-e-numero>" — ad esempio, "scegli quale applicazione apre un tipo di file windows 11" se utilizza Windows 11.

Può trovare molte più informazioni sull'apertura e la modifica di file e cartelle nel nostro prossimo articolo.

### Evidenziazione della sintassi

Gli editor di codice come VS Code forniscono l'evidenziazione della sintassi — cioè, le caratteristiche del codice riconosciuto hanno parti visibili in colori diversi. Questo rende il codice molto più facile da leggere rispetto all'averlo tutto in un unico colore. Usiamo il seguente esempio di funzione JavaScript:

```js
function createGreeting(name) {
  const greeting = `Hello, ${name}!`;
  return greeting;
}
```

Non è necessario che capisca cosa fa questo codice per ora, ma può già vedere come appare l'evidenziazione della sintassi sopra. Sì, forniamo anche l'evidenziazione della sintassi su MDN!

Proviamo un esercizio su VS Code:

1. Copi l'esempio di codice sopra negli appunti (i blocchi di codice MDN hanno un'icona di copia in alto a destra su cui può premere per farlo).
2. Apre VS Code e crei un nuovo file selezionando _File_ > _Nuovo File..._
3. All'interno del nuovo file, clicchi sul testo _Seleziona una lingua_, quindi scelga _JavaScript_ dal menu a discesa che si apre.
4. Incolli il codice nel nuovo file per vedere come appare l'evidenziazione della sintassi JavaScript di VS Code.

VS Code offre anche altre caratteristiche di sintassi. Per esempio:

- Vedrà una sottile linea verticale che corre dal `function` alla parentesi graffa di chiusura (`}`) — queste linee vengono utilizzate per contrassegnare diversi livelli di [indentazione](https://en.wikipedia.org/wiki/Indentation_style) nel codice, rendendo più facile individuare dove iniziano e finiscono i blocchi.
- Provi anche a spostare il cursore del testo lampeggiante sopra la parentesi graffa di apertura o chiusura (`{` o `}`) — vedrà entrambe evidenziate. Questo aiuta a identificare l'inizio e la fine dei blocchi ed è utile quando cerca di trovare dove manca un carattere in caso di una struttura più complicata con molti blocchi annidati. Questo evidenziamento funziona anche con altri delimitatori come parentesi tonde (`(` e `)`) e parentesi quadre (`[` e `]`).

### Completamento/suggerimento del codice

Quando digita codice in un editor di codice, esso sarà spesso in grado di suggerire cosa dovrebbe digitare successivamente e compilare automaticamente del codice boilerplate (che significa codice standard che sarà sempre uguale).

Provi ora in VS Code:

1. Torni al file JavaScript creato nella sezione precedente.
2. Vada in fondo al file e prema <kbd>Enter</kbd>/<kbd>Return</kbd> un paio di volte per assicurarsi di essere su una nuova riga.
3. Inizi a digitare "function" — dovrebbero apparire delle opzioni in un elenco alla destra del suo testo.
4. Selezioni l'opzione _function_ con _Function Statement_ scritto a destra. Compilerà automaticamente il seguente codice per lei:

   ```js-nolint
   function name(params) {

   }
   ```

5. Clicchi all'interno della funzione, sulla riga vuota tra le due parentesi graffe. Inizi a digitare "document" e le verrà nuovamente fornito un elenco di opzioni. Selezioni la prima. Questo è un riferimento all'oggetto [`Document`](/it/docs/Web/API/Document) (non si preoccupi di capire cosa significhi per ora).
6. Subito dopo `document`, digiti un punto (`.`) — avrà di nuovo un elenco di opzioni, questa volta contenente tutte le proprietà e i metodi disponibili sull'oggetto `document`!

Questo è sufficiente per ora. Passiamo oltre.

### Aiuto al debugging

Gli editor di codice non possono risolvere automaticamente tutti i suoi problemi di codice, ma possono certamente aiutarla a trovare errori di battitura e altri errori semplici. Esaminiamo un paio di esempi.

1. Torni al suo file JavaScript e cancelli tutto il codice attualmente presente. Sostituisca tutto con il seguente:

   ```js-nolint example-bad
   function createGreeting(name) {
     const greeting = `Hello, ${Name}!`;
     return greeting;
   }

   const helloChris = createGreeting("Chris);

   console.log(helloChris;
   ```

2. La piccola icona a croce alla destra dell'elenco di codice sopra è il sistema di indicazione di MDN per indicare un cattivo esempio di codice, e giustamente — ci sono tre errori nel codice sopra! Osservi l'evidenziazione di VS Code per vedere se riesce a capire come evidenzia gli errori, quindi li analizzeremo insieme.
3. Il primo errore è che abbiamo usato `name` nella prima riga, ma `Name` nella seconda riga per riferirci alla stessa variabile. Questo è un problema perché JavaScript fa distinzione tra maiuscole e minuscole e quindi le considera due nomi diversi. VS Code ha evidenziato questo in due modi diversi — colorando `name` in grigio scuro per indicare che il valore è dichiarato ma mai usato (spesso un buon segno che ha commesso un errore di battitura da qualche parte) e mettendo tre punti sotto `Name` per indicare che ha un suggerimento su come migliorare il codice (in questo caso chiedendo se intendeva scrivere `name`). Per risolvere questo errore, cambi `Name` in `name`.
   > [!NOTE]
   > Può passare sopra ciascuno degli evidenziamenti indicati con il puntatore del mouse per ottenere ulteriori informazioni.
4. Il secondo errore è sulla sesta riga, dove scriviamo `"Chris`. In JavaScript, un pezzo di testo (noto come **stringa**) deve essere racchiuso tra due virgolette, ma manca la seconda. VS Code ha evidenziato questo sottolineando il testo in cui l'errore è stato prima notato (potrebbe non essere il punto esatto dove l'errore è effettivamente) con una linea ondulata rossa, simile a quella usata in Microsoft Word per evidenziare errori di ortografia. Per risolverlo, aggiorni `"Chris` in `"Chris"`.
5. Nell'ultima riga, rimane una piccola parte di sottolineatura rossa ondulata vicino alla fine, anche dopo aver risolto l'errore precedente. Questo a causa del terzo errore — in JavaScript un'apertura di parentesi ha sempre bisogno di una parentesi di chiusura. Risolva questo aggiornando `(helloChris` in `(helloChris)`.

### Cerca e sostituisci

Ogni valido editor di codice ha una funzione robusta di ricerca e sostituzione. Questo è utile ad esempio se scopre che un errore si verifica in una funzione specifica e vuole trovarlo nel suo codice, o se decide di cambiare il nome di una variabile e ha bisogno che venga modificato in tutti i luoghi che lo citano.

Il concetto di cerca e sostituisci dovrebbe esserle abbastanza familiare se ha utilizzato un computer in precedenza, ma esploriamolo rapidamente per completezza:

1. Torni al suo file JavaScript in VS Code e apra il pannello trova e sostituisci in modalità trova selezionando _Modifica_ > _Trova_ dal menu.
2. Digiti `createGreeting` nella casella _Trova_ — vedrà che entrambe le istanze sono evidenziate, e può spostarsi tra di esse con le frecce su e giù nel pannello. L'istanza attualmente attiva è evidenziata in modo più brillante.
3. Ora apra il pannello trova e sostituisci in modalità sostituisci selezionando _Modifica_ > _Sostituisci_ dal menu, o cliccando la freccia a sinistra della casella _Trova_.
4. Digiti `sayHello` nella casella _Sostituisci_ che ora dovrebbe essere visibile.
5. Ora può sostituire tutte le istanze di `createGreeting` nel codice con `sayHello` usando i due pulsanti a destra della casella _Sostituisci_. Il pulsante sinistro passa alla successiva istanza della stringa di ricerca con un clic e la sostituisce con un secondo clic. Il pulsante destro sostituisce tutte le istanze con un solo clic.

VS Code ha molte potenti funzionalità di trova e sostituisci — veda [Trova e sostituisci](https://code.visualstudio.com/docs/editing/codebasics#_find-and-replace).

## Migliorare il suo editor di codice con estensioni

La maggior parte degli editor di codice ha un sistema di estensioni o plugin per consentire di aggiungere alla funzionalità del programma qualcosa che non è disponibile di default. Queste possono fare una varietà di attività, come:

- Abilitare il completamento del codice, linting, o funzionalità di debugging per linguaggi non supportati di default, o fornire funzionalità aggiuntive per quelli che lo sono.
- Consentirle di utilizzare la funzionalità di altri strumenti direttamente dall'interno dell'editor di codice, come strumenti di controllo di versione o server di test locali.
- Fornire interfacce utente aggiuntive o temi/schemi di colore di evidenziazione del codice.
- Suggerire snippet di codice per soddisfare i requisiti. Questi possono essere generati da modelli statici, o tramite strumenti di intelligenza artificiale. L'uso dell'AI per generare snippet di codice ha molti degli stessi vantaggi e avvertenze dell'usarli per generare risultati di ricerca (vedi [Ricerca di informazioni > Uso dell'AI](/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web#using_ai) per ulteriori informazioni).

Le estensioni di VS Code sono gestite tramite il pannello Extensions Marketplace in VS Code, accessibile attraverso il menu _Visualizza_ > _Estensioni_. Esploriamolo ora.

1. Apre il pannello Extensions Marketplace.
2. Nella casella _Cerca..._ in alto al pannello, digiti "JavaScript" per vedere quali estensioni relative a JavaScript sono disponibili. Provi a cliccare su alcune delle ricerche che appaiono per vedere cosa fanno. Non ne installi nessuna per ora.
3. Invece, installiamo un'estensione che è facile da comprendere e sarà utile per praticamente qualsiasi file di codice su cui lavora in questo set di moduli. Digiti "Prettier" nella casella _Cerca..._ e clicchi sul risultato _Prettier - code formatter_. Quando l'estensione [Prettier](https://prettier.io/) è installata, può essere utilizzata per formattare il suo codice ogni volta che salva un file, rendendo il suo codice molto più facile da leggere.
4. Clicchi il pulsante _Installa_ nella scheda _Estensione_. Chiuda la scheda quando l'installazione è finita.
5. Per far funzionare Prettier, è necessario aggiornare alcune impostazioni. Apre la scheda Impostazioni di VS Code (_Code_ > _Impostazioni..._ > _Impostazioni_ su macOS, _File_ > _Preferenze_ > _Impostazioni_ su Windows).
6. Nella casella _Cerca impostazioni_ in alto, digiti "formatter" per filtrare l'elenco delle impostazioni e mostrare solo quelle che contengono "formatter".
7. Trovi l'opzione _Editor: Formatter Predefinito_, e selezioni l'opzione _Prettier - Code formatter_ dal menu a discesa associato.
8. Trovi l'opzione _Editor: Format On Save_ e la abiliti cliccando sulla sua casella di controllo.
9. Chiuda la scheda _Impostazioni_.

Ogni configurazione è pronta; vediamo Prettier in azione.

1. Torni alla scheda del suo file JavaScript e lo salvi (_File_ > _Salva_). Il file deve essere salvato affinché Prettier funzioni. Lo chiami `test.js`. La posizione in cui lo salva non è veramente importante.
2. Sostituisca il contenuto corrente con il seguente codice:

   ```js-nolint example-bad
   function sayHello(name){const greeting = `Hello, ${name}!`;
   return greeting;}
   ```

3. Salvi di nuovo il file; a questo punto, Prettier dovrebbe riformattare il codice in modo ordinato, come segue:

   ```js
   function sayHello(name) {
     const greeting = `Hello, ${name}!`;
     return greeting;
   }
   ```

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Environment_setup")}}
