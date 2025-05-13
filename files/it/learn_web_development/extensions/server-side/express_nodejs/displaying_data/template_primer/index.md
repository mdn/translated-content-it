---
title: Guida ai modelli
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

Un modello è un file di testo che definisce la _struttura_ o il layout di un file di output, con segnaposto utilizzati per rappresentare dove verranno inseriti i dati quando il modello viene renderizzato (in _Express_, i modelli sono indicati come _view_).

## Scelte di modelli in Express

Express può essere utilizzato con molti diversi [motori di rendering dei modelli](https://expressjs.com/en/guide/using-template-engines.html). In questo tutorial utilizziamo [Pug](https://pugjs.org/api/getting-started.html) (precedentemente noto come _Jade_) per i nostri modelli. Questa è la lingua dei modelli più popolare per Node e si descrive come una "sintassi pulita e sensibile agli spazi bianchi per scrivere HTML, fortemente influenzata da [Haml](https://haml.info/)".

I vari linguaggi di modelli utilizzano approcci diversi per definire il layout e marcare i segnaposto per i dati: alcuni usano l'HTML per definire il layout, mentre altri usano diversi formati di markup che possono essere compilati in HTML. Pug è del secondo tipo; utilizza una _rappresentazione_ di HTML dove la prima parola di una qualsiasi riga rappresenta di solito un elemento HTML, e l'indentazione delle righe successive è utilizzata per rappresentare la nidificazione. Il risultato è una definizione della pagina che traduce direttamente in HTML, ma che è più concisa e probabilmente più facile da leggere.

> [!NOTE]
> Un aspetto negativo dell'uso di _Pug_ è che è sensibile all'indentazione e agli spazi bianchi (se si aggiunge uno spazio extra nel posto sbagliato si potrebbe ottenere un codice di errore non utile). Tuttavia, una volta che si sono impostati i modelli, sono molto facili da leggere e mantenere.

## Configurazione del modello

Il sito _LocalLibrary_ è stato configurato per utilizzare [Pug](https://pugjs.org/api/getting-started.html) quando abbiamo [creato lo scheletro del sito web](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website). Dovresti vedere il modulo pug incluso come dipendenza nel file **package.json** del sito web, e le seguenti impostazioni di configurazione nel file **app.js**. Le impostazioni ci dicono che stiamo usando pug come motore di viste, e che _Express_ dovrebbe cercare i modelli nella sottodirectory **/views**.

```js
// View engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Se guardi nella directory views vedrai i file .pug per le viste predefinite del progetto.
Questi includono la vista per la pagina iniziale (**index.pug**) e il modello base (**layout.pug**) che dovremo sostituire con il nostro contenuto.

```plain
/express-locallibrary-tutorial  # the project root
  /views
    error.pug
    index.pug
    layout.pug
```

## Sintassi del modello

Il file di esempio del modello sotto mostra molte delle funzionalità più utili di Pug.

La prima cosa da notare è che il file mappa la struttura di un tipico file HTML, con la prima parola in (quasi) ogni riga come un elemento HTML, e l'indentazione utilizzata per indicare gli elementi nidificati. Quindi ad esempio, l'elemento `body` è all'interno di un elemento `html`, e gli elementi paragrafo (`p`) sono all'interno dell'elemento `body`, ecc. Gli elementi non nidificati (ad es., paragrafi individuali) sono su linee separate.

```pug
doctype html
html(lang="en")
  head
    title= title
    script(type='text/javascript').
  body
    h1= title

    p This is a line with #[em some emphasis] and #[strong strong text] markup.
    p This line has un-escaped data: !{'<em> is emphasized</em>'} and escaped data: #{'<em> is not emphasized</em>'}.
      | This line follows on.
    p= 'Evaluated and <em>escaped expression</em>:' + title

    <!-- You can add HTML comments directly -->
    // You can add single line JavaScript comments and they are generated to HTML comments
    //- Introducing a single line JavaScript comment with "//-" ensures the comment isn't rendered to HTML

    p A line with a link
      a(href='/catalog/authors') Some link text
      |  and some extra text.

    #container.col
      if title
        p A variable named "title" exists.
      else
        p A variable named "title" does not exist.
      p.
        Pug is a terse and simple template language with a
        strong focus on performance and powerful features.

    h2 Generate a list

    ul
      each val in [1, 2, 3, 4, 5]
        li= val
```

Gli attributi degli elementi sono definiti tra parentesi dopo il loro elemento associato. All'interno delle parentesi, gli attributi sono definiti in elenchi separati da virgole o spazi bianchi delle coppie di nomi e valori degli attributi, ad esempio:

- `script(type='text/javascript')`, `link(rel='stylesheet', href='/stylesheets/style.css')`
- `meta(name='viewport' content='width=device-width initial-scale=1')`

I valori di tutti gli attributi sono _escapati_ (ad es., caratteri come `>` sono convertiti nei loro equivalenti di codice HTML come `&gt;`) per prevenire attacchi di iniezione JavaScript o cross-site scripting.

Se un tag è seguito dal segno uguale, il testo seguente è trattato come un'espressione JavaScript. Quindi ad esempio, nella prima riga sotto, il contenuto del tag `h1` sarà la _variabile_ `title` (definita nel file o passata nel modello da Express). Nella seconda riga il contenuto del paragrafo è una stringa di testo concatenata con la variabile `title`. In entrambi i casi il comportamento predefinito è di _escapare_ la riga.

```pug
h1= title
p= 'Evaluated and <em>escaped expression</em>:' + title
```

> [!NOTE]
> Nei modelli Pug, una variabile che è usata ma non passata dal codice di Express (o definita localmente) è "undefined".
> Se si utilizza questo modello senza passare una variabile `title`, i tag verrebbero creati ma conterrebbero una stringa vuota.
> Se usi variabili non definite in istruzioni condizionali, allora valutano a `false`.
> Altri linguaggi di modello potrebbero richiedere che le variabili utilizzate nel modello debbano essere definite.

Se non c'è il simbolo di uguale dopo il tag, il contenuto è trattato come testo normale. All'interno del testo normale è possibile inserire dati escapati e non escapati utilizzando rispettivamente la sintassi `#{}` e `!{}` come mostrato sotto. Puoi anche aggiungere HTML grezzo all'interno del testo normale.

```pug
p This is a line with #[em some emphasis] and #[strong strong text] markup.
p This line has an un-escaped string: !{'<em> is emphasized</em>'}, an escaped string: #{'<em> is not emphasized</em>'}, and escaped variables: #{title}.
```

> [!NOTE]
> Vorrai quasi sempre escapare i dati dagli utenti (tramite la sintassi **`#{}`**). I dati che possono essere considerati fidati (ad esempio, conteggi generati di record, ecc.) possono essere visualizzati senza escapare i valori.

Puoi usare il carattere pipe ('**|**') all'inizio di una linea per indicare "[testo normale](https://pugjs.org/language/plain-text.html)". Ad esempio, il testo aggiuntivo mostrato sotto sarà visualizzato sulla stessa riga del collegamento precedente, ma non sarà collegato.

```pug
a(href='http://someurl/') Link text
| Plain text
```

Pug ti consente di eseguire operazioni condizionali utilizzando `if`, `else`, `else if` e `unless` — ad esempio:

```pug
if title
  p A variable named "title" exists
else
  p A variable named "title" does not exist
```

Puoi anche eseguire operazioni di loop/iterazione usando la sintassi `each-in` o `while`. Nel frammento di codice sotto abbiamo iterato attraverso un array per visualizzare un elenco di variabili (nota l'uso di 'li=' per valutare il "val" come variabile sotto. Il valore su cui iterare può anche essere passato nel modello come variabile!

```pug
ul
  each val in [1, 2, 3, 4, 5]
    li= val
```

La sintassi supporta anche commenti (che possono essere resi nell'output — o meno — a seconda delle scelte), mixin per creare blocchi di codice riutilizzabili, istruzioni case e molte altre funzionalità. Per informazioni più dettagliate, vedere [La documentazione di Pug](https://pugjs.org/api/getting-started.html).

## Estensione dei modelli

Su un sito, è comune che tutte le pagine abbiano una struttura comune, incluso il markup HTML standard per l'intestazione, il piè di pagina, la navigazione, ecc. Piuttosto che costringere gli sviluppatori a duplicare questo "boilerplate" in ogni pagina, _Pug_ consente di dichiarare un modello base e quindi estenderlo, sostituendo solo le parti che sono diverse per ciascuna pagina specifica.

Ad esempio, il modello base **layout.pug** creato nel nostro [progetto skeleton](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website) è simile a questo:

```pug
doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    block content
```

Il tag `block` è utilizzato per contrassegnare sezioni di contenuto che possono essere sostituite in un modello derivato (se il block non è ridefinito, allora la sua implementazione nella classe base viene utilizzata).

Il **index.pug** predefinito (creato per il nostro progetto skeleton) mostra come sovrascriviamo il modello base. Il tag `extends` identifica il modello base da usare, e poi usiamo `block section_name` per indicare il nuovo contenuto della sezione che sovrascriveremo.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Prossimi passi

- Torna al [Tutorial di Express Parte 5: Visualizzare i dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi al sottoprogetto successivo della parte 5: [Il modello base della LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template).
