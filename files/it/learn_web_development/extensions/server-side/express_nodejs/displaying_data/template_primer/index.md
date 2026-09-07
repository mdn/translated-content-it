---
title: Introduzione ai template
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

Un template è un file di testo che definisce la _struttura_ o il layout di un file di output, con segnaposto utilizzati per rappresentare i punti in cui verranno inseriti i dati quando il template viene sottoposto a rendering (in _Express_, i template sono chiamati _views_).

## Scelte di template per Express

Express può essere utilizzato con molti diversi [motori di rendering dei template](https://expressjs.com/en/guide/using-template-engines/). In questo tutorial viene utilizzato [Pug](https://pugjs.org/api/getting-started.html) (precedentemente noto come _Jade_) per i template. È il linguaggio di template per Node più popolare e si descrive come una "sintassi pulita, sensibile agli spazi bianchi per scrivere HTML, fortemente influenzata da [Haml](https://haml.info/)".

Diversi linguaggi di template utilizzano approcci diversi per definire il layout e contrassegnare i segnaposto per i dati: alcuni utilizzano HTML per definire il layout, mentre altri utilizzano formati di markup diversi che possono essere traspilati in HTML. Pug appartiene al secondo tipo; utilizza una _rappresentazione_ di HTML in cui la prima parola di ogni riga rappresenta solitamente un elemento HTML e il rientro nelle righe successive viene utilizzato per rappresentare l'annidamento. Il risultato è una definizione di pagina che si traduce direttamente in HTML, ma è più concisa e, probabilmente, più facile da leggere.

> [!NOTE]
> Uno svantaggio dell'uso di _Pug_ è che è sensibile al rientro e agli spazi bianchi (se viene aggiunto uno spazio in più nel punto sbagliato, potrebbe essere generato un codice di errore poco utile). Una volta predisposti i template, tuttavia, sono molto facili da leggere e mantenere.

## Configurazione dei template

_LocalLibrary_ è stata configurata per utilizzare [Pug](https://pugjs.org/api/getting-started.html) quando è stato [creato il sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website). Il modulo pug dovrebbe essere incluso come dipendenza nel file **package.json** del sito web e nel file **app.js** dovrebbero essere presenti le seguenti impostazioni di configurazione. Le impostazioni indicano che pug viene utilizzato come motore di view e che _Express_ deve cercare i template nella sottodirectory **/views**.

```js
// View engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Osservando la directory views, saranno visibili i file .pug per le view predefinite del progetto.
Questi includono la view per la pagina iniziale (**index.pug**) e il template di base (**layout.pug**), che dovranno essere sostituiti con contenuto personalizzato.

```plain
/express-locallibrary-tutorial  # the project root
  /views
    error.pug
    index.pug
    layout.pug
```

## Sintassi dei template

Il file di template di esempio seguente mostra molte delle funzionalità più utili di Pug.

La prima cosa da notare è che il file mappa la struttura di un tipico file HTML: la prima parola in (quasi) ogni riga è un elemento HTML e il rientro viene utilizzato per indicare gli elementi annidati. Ad esempio, l'elemento `body` si trova all'interno di un elemento `html` e gli elementi paragrafo (`p`) si trovano all'interno dell'elemento `body`, e così via. Gli elementi non annidati, ad esempio i singoli paragrafi, si trovano su righe separate.

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

Gli attributi dell'elemento sono definiti tra parentesi dopo l'elemento associato. All'interno delle parentesi, gli attributi sono definiti in elenchi di coppie di nomi e valori degli attributi, separati da virgole o spazi bianchi, ad esempio:

- `script(type='text/javascript')`, `link(rel='stylesheet', href='/stylesheets/style.css')`
- `meta(name='viewport' content='width=device-width')`

I valori di tutti gli attributi vengono sottoposti a _escaping_ (ad esempio, caratteri come `>` vengono convertiti nei rispettivi equivalenti in codice HTML, come `&gt;`) per impedire attacchi di injection JavaScript o cross-site scripting.

Se un tag è seguito dal segno di uguale, il testo seguente viene trattato come un'_espressione_ JavaScript. Ad esempio, nella prima riga seguente, il contenuto del tag `h1` sarà la _variabile_ `title` (definita nel file o passata al template da Express). Nella seconda riga, il contenuto del paragrafo è una stringa di testo concatenata con la variabile `title`. In entrambi i casi, il comportamento predefinito consiste nell'effettuare l'_escaping_ della riga.

```pug
h1= title
p= 'Evaluated and <em>escaped expression</em>:' + title
```

> [!NOTE]
> Nei template Pug, una variabile utilizzata ma non passata dal codice Express, né definita localmente, è "undefined".
> Se questo template venisse utilizzato senza passare una variabile `title`, i tag verrebbero creati ma conterrebbero una stringa vuota.
> Se vengono utilizzate variabili undefined nelle istruzioni condizionali, queste vengono valutate come `false`.
> Altri linguaggi di template potrebbero richiedere che le variabili utilizzate nel template siano definite.

Se dopo il tag non è presente un simbolo di uguale, il contenuto viene trattato come testo semplice. Nel testo semplice è possibile inserire dati sottoposti a escaping e non sottoposti a escaping utilizzando rispettivamente la sintassi `#{}` e `!{}`, come mostrato di seguito. È inoltre possibile aggiungere HTML non elaborato nel testo semplice.

```pug
p This is a line with #[em some emphasis] and #[strong strong text] markup.
p This line has an un-escaped string: !{'<em> is emphasized</em>'}, an escaped string: #{'<em> is not emphasized</em>'}, and escaped variables: #{title}.
```

> [!NOTE]
> Quasi sempre sarà opportuno effettuare l'escaping dei dati degli utenti, tramite la sintassi **`#{}`**. I dati affidabili, ad esempio conteggi di record generati, possono essere visualizzati senza effettuare l'escaping dei valori.

È possibile utilizzare il carattere pipe ('**|**') all'inizio di una riga per indicare il "[testo semplice](https://pugjs.org/language/plain-text.html)". Ad esempio, il testo aggiuntivo mostrato di seguito verrà visualizzato sulla stessa riga dell'anchor precedente, ma non sarà un collegamento.

```pug
a(href='http://someurl/') Link text
| Plain text
```

Pug consente di eseguire operazioni condizionali utilizzando `if`, `else`, `else if` e `unless`, ad esempio:

```pug
if title
  p A variable named "title" exists
else
  p A variable named "title" does not exist
```

È inoltre possibile eseguire operazioni di ciclo/iterazione utilizzando la sintassi `each-in` o `while`. Nel frammento di codice seguente, viene eseguito un ciclo attraverso un array per visualizzare un elenco di variabili. Si noti l'uso di `li=` per valutare `val` come variabile. Il valore su cui viene eseguita l'iterazione può anche essere passato al template come variabile.

```pug
ul
  each val in [1, 2, 3, 4, 5]
    li= val
```

La sintassi supporta inoltre commenti, che possono essere sottoposti a rendering nell'output oppure no, a scelta; mixin per creare blocchi di codice riutilizzabili; istruzioni case e molte altre funzionalità. Per informazioni più dettagliate, consultare [la documentazione di Pug](https://pugjs.org/api/getting-started.html).

## Estendere i template

In un sito, è normale che tutte le pagine abbiano una struttura comune, inclusi markup HTML standard per head, footer, navigazione e così via. Anziché costringere gli sviluppatori a duplicare questo "codice boilerplate" in ogni pagina, _Pug_ consente di dichiarare un template di base e quindi estenderlo, sostituendo solo le parti differenti per ogni pagina specifica.

Ad esempio, il template di base **layout.pug** creato nel [progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website) ha questo aspetto:

```pug
doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    block content
```

Il tag `block` viene utilizzato per contrassegnare sezioni di contenuto che possono essere sostituite in un template derivato. Se il blocco non viene ridefinito, viene utilizzata la relativa implementazione nella classe di base.

Il file **index.pug** predefinito, creato per il progetto scheletro, mostra come sovrascrivere il template di base. Il tag `extends` identifica il template di base da utilizzare, quindi `block section_name` viene utilizzato per indicare il nuovo contenuto della sezione che verrà sovrascritta.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Passaggi successivi

- Tornare a [Tutorial su Express, parte 5: Visualizzare i dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proseguire al sottoarticolo successivo della parte 5: [Il template di base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template).
