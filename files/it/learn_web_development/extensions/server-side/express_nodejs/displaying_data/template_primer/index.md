---
title: Template primer
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

Un modello è un file di testo che definisce la _struttura_ o il layout di un file di output, con segnaposti utilizzati per rappresentare dove verranno inseriti i dati quando il modello viene reso. In _Express_, i modelli sono indicati come _views_ (viste).

## Scelte di modelli Express

Express può essere utilizzato con molti diversi [motori di rendering dei modelli](https://expressjs.com/en/guide/using-template-engines.html). In questo tutorial utilizziamo [Pug](https://pugjs.org/api/getting-started.html) (precedentemente noto come _Jade_) per i nostri modelli. Questo è il linguaggio di modelli più popolare di Node, e si descrive come una "sintassi pulita e sensibile agli spazi bianchi per scrivere HTML, fortemente influenzata da [Haml](https://haml.info/)".

I diversi linguaggi di modelli utilizzano diversi approcci per definire il layout e contrassegnare i segnaposti per i dati—alcuni usano HTML per definire il layout mentre altri usano diversi formati di markup che possono essere traspilati in HTML. Pug appartiene al secondo tipo; utilizza una _rappresentazione_ di HTML dove la prima parola in ogni riga rappresenta solitamente un elemento HTML, e l'indentazione sulle righe successive è usata per rappresentare il nesting. Il risultato è una definizione di pagina che viene tradotta direttamente in HTML, ma è più concisa e, per certi versi, più facile da leggere.

> [!NOTE]
> Uno svantaggio dell'uso di _Pug_ è che è sensibile all'indentazione e agli spazi bianchi (se si aggiunge uno spazio in più nel punto sbagliato si potrebbe ottenere un codice di errore poco utile). Tuttavia, una volta che i modelli sono in posizione, sono molto facili da leggere e mantenere.

## Configurazione del modello

Il _LocalLibrary_ è stato configurato per utilizzare [Pug](https://pugjs.org/api/getting-started.html) quando abbiamo [creato il sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website). Si dovrebbe vedere il modulo pug incluso come dipendenza nel file **package.json** del sito web e le seguenti impostazioni di configurazione nel file **app.js**. Le impostazioni ci dicono che stiamo utilizzando pug come motore di visualizzazione e che _Express_ dovrebbe cercare i modelli nella sottodirectory **/views**.

```js
// View engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Se si guarda nella directory delle viste si vedranno i file .pug per le viste predefinite del progetto. Questi includono la vista per la home page (**index.pug**) e il modello di base (**layout.pug**) che dovremo sostituire con i nostri contenuti.

```plain
/express-locallibrary-tutorial  # the project root
  /views
    error.pug
    index.pug
    layout.pug
```

## Sintassi del modello

Il file di esempio del modello qui sotto mostra molte delle caratteristiche più utili di Pug.

La prima cosa da notare è che il file mappa la struttura di un tipico file HTML, con la prima parola in (quasi) ogni riga che è un elemento HTML, e l'indentazione usata per indicare gli elementi nidificati. Quindi, ad esempio, l'elemento `body` è all'interno di un elemento `html`, e gli elementi di paragrafo (`p`) sono all'interno dell'elemento `body`, ecc. Gli elementi non nidificati (ad esempio, i singoli paragrafi) sono su linee separate.

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

Gli attributi degli elementi sono definiti tra parentesi dopo il loro elemento associato. All'interno delle parentesi, gli attributi sono definiti in elenchi separati da virgole o spazi dei nomi degli attributi e dei valori degli attributi, ad esempio:

- `script(type='text/javascript')`, `link(rel='stylesheet', href='/stylesheets/style.css')`
- `meta(name='viewport' content='width=device-width initial-scale=1')`

I valori di tutti gli attributi sono _escapate_ (ad esempio, caratteri come `>` vengono convertiti nei loro equivalenti in codice HTML come `&gt;`) per prevenire l'iniezione di JavaScript o attacchi di scripting tra siti.

Se un tag è seguito dal segno di uguale, il testo seguente viene trattato come un'espressione JavaScript. Quindi, ad esempio, nella prima riga qui sotto, il contenuto del tag `h1` sarà la _variabile_ `title` (definita nel file o passata nel modello da Express). Nella seconda riga il contenuto del paragrafo è una stringa di testo concatenata con la variabile `title`. In entrambi i casi il comportamento predefinito è di _eseguire un'esapazione_ della riga.

```pug
h1= title
p= 'Evaluated and <em>escaped expression</em>:' + title
```

> [!NOTE]
> Nei modelli Pug, una variabile utilizzata ma non passata dal codice di Express (o definita localmente) è "undefined".
> Se si utilizza questo modello senza passare la variabile `title`, i tag verrebbero creati ma conterrebbero una stringa vuota.
> Se si usano variabili non definite in istruzioni condizionali, queste valutano a `false`.
> Altri linguaggi di modelli possono richiedere che le variabili usate nel modello siano definite.

Se non c'è il simbolo dell'uguale dopo il tag, il contenuto viene trattato come testo normale. All'interno del testo normale si possono inserire dati con escape e senza escape utilizzando rispettivamente la sintassi `#{}` e `!{}`, come mostrato di seguito. Si può anche aggiungere HTML grezzo all'interno del testo normale.

```pug
p This is a line with #[em some emphasis] and #[strong strong text] markup.
p This line has an un-escaped string: !{'<em> is emphasized</em>'}, an escaped string: #{'<em> is not emphasized</em>'}, and escaped variables: #{title}.
```

> [!NOTE]
> Si vorrà quasi sempre eseguire l'escape dei dati dagli utenti (tramite la sintassi **`#{}`**). I dati che possono essere considerati affidabili (ad esempio, conteggi generati di record, ecc.) possono essere visualizzati senza eseguire l'escape dei valori.

È possibile utilizzare il carattere pipe ('**|**') all'inizio di una riga per indicare "[testo normale](https://pugjs.org/language/plain-text.html)". Ad esempio, il testo aggiuntivo mostrato di seguito verrà visualizzato sulla stessa riga del collegamento precedente, ma non sarà collegato.

```pug
a(href='http://someurl/') Link text
| Plain text
```

Pug consente di eseguire operazioni condizionali utilizzando `if`, `else`, `else if` e `unless` — ad esempio:

```pug
if title
  p A variable named "title" exists
else
  p A variable named "title" does not exist
```

È anche possibile eseguire operazioni di ciclo/iterazione utilizzando la sintassi `each-in` o `while`. Nel frammento di codice sottostante abbiamo effettuato un ciclo attraverso un array per visualizzare una lista di variabili (nota l'uso di 'li=' per valutare "val" come variabile qui sotto. Il valore su cui iterare può anche essere passato nel modello come variabile!

```pug
ul
  each val in [1, 2, 3, 4, 5]
    li= val
```

La sintassi supporta anche commenti (che possono essere resi nell'output - o meno - a seconda della scelta), mixin per creare blocchi di codice riutilizzabili, dichiarazioni di caso e molte altre funzionalità. Per informazioni più dettagliate si veda [La documentazione di Pug](https://pugjs.org/api/getting-started.html).

## Estendere i modelli

In un sito, è usuale che tutte le pagine abbiano una struttura comune, inclusi markup HTML standard per l'intestazione, il piè di pagina, la navigazione, ecc. Piuttosto che costringere gli sviluppatori a duplicare questo "boilerplate" in ogni pagina, _Pug_ consente di dichiarare un modello di base e poi estenderlo, sostituendo solo le parti che sono diverse per ogni pagina specifica.

Ad esempio, il modello di base **layout.pug** creato nel nostro [progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website) ha il seguente aspetto:

```pug
doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    block content
```

Il tag `block` è utilizzato per segnare sezioni di contenuto che possono essere sostituite in un modello derivato (se il blocco non viene ridefinito allora viene utilizzata la sua implementazione nella classe base).

Il **index.pug** predefinito (creato per il nostro progetto scheletro) mostra come sovrascriviamo il modello di base. Il tag `extends` identifica il modello di base da utilizzare, e poi usiamo `block section_name` per indicare il nuovo contenuto della sezione che sovrascriveremo.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Prossimi passi

- Torna a [Express Tutorial Part 5: Displaying library data](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi al prossimo sottoarticolo della parte 5: [Il modello base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template).
