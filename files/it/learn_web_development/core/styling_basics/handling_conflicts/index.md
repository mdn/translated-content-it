---
title: Gestione dei conflitti
slug: Learn_web_development/Core/Styling_basics/Handling_conflicts
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics")}}

L'obiettivo di questa lezione è sviluppare la sua comprensione di alcuni dei concetti più fondamentali di CSS — la cascata, la specificità e l'ereditarietà — che controllano come CSS viene applicato a HTML e come vengono risolti i conflitti tra le dichiarazioni di stile.

Sebbene lavorare attraverso questa lezione possa sembrare inizialmente meno rilevante e un po' più accademico rispetto ad altre parti del corso, una comprensione di questi concetti le risparmierà molti problemi in seguito! Le consigliamo di affrontare attentamente questa sezione e verificare di aver compreso i concetti prima di procedere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere come le regole possano entrare in conflitto in CSS.</li>
          <li>Ereditarietà.</li>
          <li>La cascata.</li>
          <li>I principali concetti che governano l'esito dei conflitti — specificità, ordine di origine e importanza.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Regole in conflitto

CSS sta per **Cascading Style Sheets**, e quella prima parola _cascading_ è estremamente importante da capire — il modo in cui la cascata si comporta è la chiave per comprendere CSS.

A un certo punto, lavorerà su un progetto e scoprirà che il CSS che pensava dovesse essere applicato a un elemento non funziona. Spesso il problema è che crea due regole che applicano valori diversi della stessa proprietà allo stesso elemento. La [**cascata**](/it/docs/Web/CSS/CSS_cascade/Cascade) e il concetto strettamente correlato di [**specificità**](/it/docs/Web/CSS/CSS_cascade/Specificity) sono meccanismi che controllano quale regola si applica quando c'è un tale conflitto. La regola che sta stilizzando il suo elemento potrebbe non essere quella che si aspetta, quindi deve capire come funzionano questi meccanismi.

Anche il concetto di [**ereditarietà**](/it/docs/Web/CSS/CSS_cascade/Inheritance) è significativo qui, il che significa che alcune proprietà CSS ereditano di default i valori impostati sull'elemento genitore corrente e altre no. Anche questo può causare comportamenti che potrebbe non aspettarsi.

Iniziamo dando un rapido sguardo alle cose chiave con cui stiamo lavorando, quindi le esamineremo ciascuna a turno e vedremo come interagiscono tra loro e con il suo CSS. Questi possono sembrare un insieme di concetti difficili da capire. Mentre acquisisce più esperienza con la scrittura di CSS, il modo in cui funziona diventerà più ovvio.

### Cascata

I fogli di stile [**cascading**](/it/docs/Web/CSS/CSS_cascade/Cascade) — a un livello molto semplice, questo significa che l'origine e l'ordine delle regole CSS sono importanti. Quando due regole hanno entrambe stessa specificità, quella definita per ultima nel foglio di stile è quella che verrà utilizzata. Ci sono altri concetti che hanno un effetto, come i [livelli di cascata](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers), ma questi sono più avanzati e non li tratteremo in dettaglio qui.

Nell'esempio seguente, abbiamo due regole che potrebbero applicarsi all'elemento `<h1>`. Il contenuto `<h1>` finisce per essere colorato di blu. Questo perché entrambe le regole provengono dalla stessa sorgente, hanno un selettore di elementi identico e quindi hanno la stessa specificità, ma quella che si trova per ultima nell'ordine di origine vince.

```html live-sample___cascade-simple
<h1>This is my heading.</h1>
```

```css live-sample___cascade-simple
h1 {
  color: red;
}
h1 {
  color: blue;
}
```

{{EmbedLiveSample("cascade-simple")}}

### Specificità

[Specificità](/it/docs/Web/CSS/CSS_cascade/Specificity) è l'algoritmo che il browser utilizza per decidere quale valore di proprietà applicare a un elemento. Se più blocchi di stile hanno selettori diversi che configurano la stessa proprietà con valori diversi e mirano allo stesso elemento, la specificità decide quale valore della proprietà viene applicato all'elemento. La specificità è fondamentalmente una misura di quanto sarà specifica la selezione di un selettore:

- Un selettore di elementi è meno specifico; selezionerà tutti gli elementi di quel tipo che appaiono su una pagina, quindi ha meno peso. I selettori di pseudo-elementi hanno la stessa specificità dei selettori di elementi normali.
- Un selettore di classe è più specifico; selezionerà solo gli elementi su una pagina che hanno un valore specifico dell'attributo `class`, quindi ha più peso. I selettori di attributi e le pseudo-classi hanno lo stesso peso di una classe.

Di seguito, abbiamo di nuovo due regole che potrebbero applicarsi all'elemento `<h1>`. Il contenuto `<h1>` finisce per essere colorato di rosso perché il selettore di classe `main-heading` conferisce alla sua regola una specificità maggiore. Quindi, anche se la regola con il selettore di elemento `<h1>` appare più avanti nell'ordine di origine, quella con la maggiore specificità, definita utilizzando il selettore di classe, verrà applicata.

```html live-sample___specificity-simple
<h1 class="main-heading">This is my heading.</h1>
```

```css live-sample___specificity-simple
.main-heading {
  color: red;
}

h1 {
  color: blue;
}
```

{{EmbedLiveSample("specificity-simple")}}

Spiegheremo l'algoritmo di specificità più avanti.

### Ereditarietà

Anche l'ereditarietà deve essere compresa in questo contesto — alcuni valori delle proprietà CSS impostati sugli elementi genitori vengono ereditati dai loro elementi figli e alcuni no.

Ad esempio, se imposta un `color` e `font-family` su un elemento, ogni elemento all'interno verrà anche stilizzato con quel colore e quel font, a meno che non abbia applicato valori di colore e font diversi direttamente a loro.

```html live-sample___inheritance-simple
<p>
  As the body has been set to have a color of blue this is inherited through the
  descendants.
</p>
<p>
  We can change the color by targeting the element with a selector, such as this
  <span>span</span>.
</p>
```

```css live-sample___inheritance-simple
body {
  color: blue;
}

span {
  color: black;
}
```

{{EmbedLiveSample("inheritance-simple")}}

Alcune proprietà non sono ereditate — ad esempio, se imposta una {{cssxref("width")}} del 50% su un elemento, tutti i suoi discendenti non ottengono una larghezza del 50% della larghezza del loro genitore. Se fosse così, utilizzare CSS sarebbe molto frustrante!

> [!NOTE]
> Sulle pagine di riferimento delle proprietà CSS su MDN, è possibile trovare una casella di informazioni tecniche chiamata "Definizione formale", che elenca una serie di dati su quella proprietà, incluso se è ereditata o meno. Vedere la sezione [Definizione formale della proprietà color](/it/docs/Web/CSS/color#formal_definition) come esempio.

### Comprendere come funzionano insieme i concetti

Questi tre concetti (cascata, specificità ed ereditarietà) insieme controllano quale CSS si applica a quale elemento. Nelle sezioni sottostanti, vedremo come funzionano insieme. A volte può sembrare un po' complicato, ma inizierà a ricordarli man mano che diventa più esperta di CSS, e può sempre cercare i dettagli se li dimentica! Anche gli sviluppatori esperti non ricordano tutti i dettagli.

## Comprendere l'ereditarietà

Inizieremo con l'ereditarietà. Nell'esempio sottostante, abbiamo un elemento {{HTMLElement("ul")}} con due livelli di elenchi non ordinati annidati all'interno. Abbiamo dato al `<ul>` esterno un bordo, un padding e un colore di font.

La proprietà `color` è una proprietà ereditata. Quindi, il valore della proprietà `color` viene applicato ai figli diretti e anche ai figli indiretti - i figli immediati `<li>` e quelli all'interno del primo elenco annidato. Poi abbiamo aggiunto la classe `special` al secondo elenco annidato e applicato un colore diverso ad esso. Questo colore viene quindi ereditato dai suoi figli.

```html live-sample___inheritance
<ul class="main">
  <li>Item One</li>
  <li>
    Item Two
    <ul>
      <li>2.1</li>
      <li>2.2</li>
    </ul>
  </li>
  <li>
    Item Three
    <ul class="special">
      <li>
        3.1
        <ul>
          <li>3.1.1</li>
          <li>3.1.2</li>
        </ul>
      </li>
      <li>3.2</li>
    </ul>
  </li>
</ul>
```

```css live-sample___inheritance
.main {
  color: rebeccapurple;
  border: 2px solid #ccc;
  padding: 1em;
}

.special {
  color: black;
  font-weight: bold;
}
```

{{EmbedLiveSample("inheritance", "", "280px")}}

Proprietà come `width` (come detto in precedenza), `margin`, `padding` e `border` non sono proprietà ereditate. Se un bordo dovesse essere ereditato dai bambini in questo esempio di elenco, ogni singolo elenco e elemento dell'elenco guadagnerebbe un bordo — probabilmente non è un effetto che desidereremmo mai!

Anche se ogni pagina di proprietà CSS elenca se la proprietà è ereditata o meno, può spesso indovinarlo intuitivamente se sa quale aspetto lo stile della proprietà.

### Controllo dell'ereditarietà

CSS fornisce cinque valori di proprietà speciali universali per controllare l'ereditarietà. Ogni proprietà CSS accetta questi valori.

- {{cssxref("inherit")}}
  - : Imposta il valore della proprietà applicato a un elemento selezionato per essere lo stesso di quello del suo elemento genitore. In pratica, questo "accende l'ereditarietà".
- {{cssxref("initial")}}
  - : Imposta il valore della proprietà applicato a un elemento selezionato per essere il [valore iniziale](/it/docs/Web/CSS/CSS_cascade/Value_processing#initial_value) di quella proprietà.
- {{cssxref("revert")}}
  - : Ripristina il valore della proprietà applicato a un elemento selezionato ai default del browser piuttosto che ai default applicati a quella proprietà. Questo valore si comporta come {{cssxref("unset")}} in molti casi.
- {{cssxref("revert-layer")}}
  - : Ripristina il valore della proprietà applicato a un elemento selezionato al valore stabilito in un precedente [livello della cascata](/it/docs/Web/CSS/@layer).
- {{cssxref("unset")}}
  - : Reimposta la proprietà al suo valore naturale, il che significa che se la proprietà è naturalmente ereditata si comporta come `inherit`, altrimenti si comporta come `initial`.

> [!NOTE]
> Vedere [Tipi di origine](/it/docs/Web/CSS/CSS_cascade/Cascade#origin_types) per ulteriori informazioni su ciascuno di questi e su come funzionano.

Possiamo guardare un elenco di collegamenti ed esplorare come funzionano i valori universali. L'esempio interattivo qui sotto le permette di giocare con il CSS e vedere cosa succede quando apporta modifiche. Giocare con il codice è davvero il modo migliore per comprendere meglio HTML e CSS.

Ad esempio:

1. Il secondo elemento dell'elenco ha applicata la classe `my-class-1`. Questo imposta il colore dell'elemento `<a>` annidato all'interno su `inherit`. Se rimuove la regola, come cambia il colore del collegamento?
2. Capisce perché il terzo e quarto collegamento hanno il colore che hanno? Il terzo collegamento è impostato su `initial`, il che significa che usa il valore iniziale della proprietà (in questo caso nero) e non il default del browser per i collegamenti, che è blu. Il quarto è impostato su `unset`, il che significa che il testo del collegamento usa il colore dell'elemento genitore, verde.
3. Quali dei collegamenti cambieranno colore se definisce un nuovo colore per l'elemento `<a>` — ad esempio `a { color: red; }`?
4. Dopo aver letto la sezione successiva sul ripristino di tutte le proprietà, torni e cambi la proprietà `color` in `all`. Noti come il secondo collegamento sia su una nuova riga e abbia un punto elenco. Quali proprietà pensa siano state ereditate?

```html live-sample___keywords
<ul>
  <li>Default <a href="#">link</a> color</li>
  <li class="my-class-1">Inherit the <a href="#">link</a> color</li>
  <li class="my-class-2">Reset the <a href="#">link</a> color</li>
  <li class="my-class-3">Unset the <a href="#">link</a> color</li>
</ul>
```

```css live-sample___keywords
body {
  color: green;
}

.my-class-1 a {
  color: inherit;
}

.my-class-2 a {
  color: initial;
}

.my-class-3 a {
  color: unset;
}
```

{{EmbedLiveSample("keywords")}}

### Reimpostare tutti i valori delle proprietà

La proprietà shorthand CSS [`all`](/it/docs/Web/CSS/all) può essere utilizzata per applicare uno di questi valori di ereditarietà a (quasi) tutte le proprietà contemporaneamente. Il suo valore può essere uno di questi valori di ereditarietà (`inherit`, `initial`, `revert`, `revert-layer`, o `unset`). È un modo conveniente per annullare le modifiche ai stili in modo da poter tornare a un punto di partenza noto prima di iniziare nuove modifiche.

Nell'esempio seguente, abbiamo due blockquote. Il primo ha uno stile applicato all'elemento blockquote stesso. Il secondo ha una classe applicata al blockquote, che imposta il valore di `all` su `unset`.

```html live-sample___all
<blockquote>
  <p>This blockquote is styled</p>
</blockquote>

<blockquote class="fix-this">
  <p>This blockquote is not styled</p>
</blockquote>
```

```css live-sample___all
blockquote {
  background-color: orange;
  border: 2px solid blue;
}

.fix-this {
  all: unset;
}
```

{{EmbedLiveSample("all")}}

Provi a impostare il valore di `all` su alcuni degli altri valori disponibili e osservi la differenza.

## Comprendere la cascata

Ora capiamo che l'ereditarietà è il motivo per cui un paragrafo annidato in profondità nella struttura del suo HTML ha lo stesso colore del CSS applicato al corpo. Dalle lezioni introduttive, abbiamo una comprensione di come cambiare il CSS applicato a qualcosa in qualsiasi punto del documento — sia assegnando CSS a un elemento sia creando una classe. Ora vedremo come la cascata definisce quali regole CSS si applicano quando più di un blocco di stile applica la stessa proprietà, ma con valori diversi, allo stesso elemento.

Ci sono tre fattori da considerare, elencati qui in ordine crescente di importanza. I più recenti prevalgono sui precedenti:

1. **Ordine di origine**
2. **Specificità**
3. **Importanza**

Li esamineremo per vedere come i browser calcolano esattamente quale CSS dovrebbe essere applicato.

### Ordine di origine

Abbiamo già visto quanto l'ordine di origine sia importante per la cascata. Se ha più di una regola, tutte con lo stesso peso, allora vince quella che viene per ultima nel CSS. Può pensare a questo come un: la regola che è più vicina all'elemento stesso sovrascrive quelle precedenti fino a quando l'ultima vince e riesce a stilizzare l'elemento.

L'ordine di origine conta solo quando il peso di specificità delle regole è lo stesso, quindi diamo un'occhiata alla specificità:

### Specificità

Spesso ci si ritrova in una situazione in cui sa che una regola viene dopo nel foglio di stile, ma una regola di conflitto anteriore è applicata. Questo accade poiché la regola anteriore ha una **maggiore specificità** — è più specifica e quindi viene scelta dal browser come quella che dovrebbe stilizzare l'elemento.

Come abbiamo visto all'inizio di questa lezione, un selettore di classe ha più peso di un selettore di elemento, quindi le proprietà definite nel blocco di stile della classe sovrascriveranno quelle definite nel blocco di stile dell'elemento.

Una cosa da notare qui è che, anche se stiamo pensando ai selettori e le regole che vengono applicate al testo o al componente che selezionano, non è l'intera regola ad essere sovrascritta, ma solo le proprietà che sono dichiarate in più posti.

Questo comportamento aiuta a evitare la ripetizione nel suo CSS. Una pratica comune è definire stili generici per gli elementi di base e poi creare classi per quelli che sono diversi. Ad esempio, nel foglio di stile sottostante, abbiamo definito stili generici per i titoli di livello 2 e abbiamo poi creato alcune classi che cambiano solo alcune delle proprietà e valori. I valori definiti inizialmente vengono applicati a tutti i titoli, quindi i valori più specifici vengono applicati ai titoli con le classi.

```html live-sample___mixing-rules
<h2>Heading with no class</h2>
<h2 class="small">Heading with class of small</h2>
<h2 class="bright">Heading with class of bright</h2>
```

```css live-sample___mixing-rules
h2 {
  font-size: 2em;
  color: #000;
  font-family: Georgia, "Times New Roman", Times, serif;
}

.small {
  font-size: 1em;
}

.bright {
  color: rebeccapurple;
}
```

{{EmbedLiveSample("mixing-rules", "", "240px")}}

Diamo ora un'occhiata a come il browser calcolerà la specificità. Sappiamo già che un selettore di elemento ha bassa specificità e può essere sovrascritto da una classe. Essenzialmente, un valore in punti viene assegnato a diversi tipi di selettori, e sommando questi punti ottiene il peso di quel particolare selettore, che può poi essere valutato rispetto ad altri potenziali abbinamenti.

La quantità di specificità che un selettore ha è misurata usando tre valori diversi (o componenti), che possono essere pensati come le colonne ID, CLASSE ed ELEMENTO nelle tre cifre del peso:

- **Identificatori**: Punteggio uno in questa colonna per ogni ID selettore contenuto all'interno del selettore complessivo.
- **Classi**: Punteggio uno in questa colonna per ogni classe selettore, selettore di attributi o pseudo-classe contenuti all'interno del selettore complessivo.
- **Elementi**: Punteggio uno in questa colonna per ogni selettore di elemento o pseudo-elemento contenuto all'interno del selettore complessivo.

> [!NOTE]
> Il selettore universale ([`*`](/it/docs/Web/CSS/Universal_selectors)), [combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators) (`+`, `>`, `~`, ' '), e il selettore di aggiustamento della specificità ([`:where()`](/it/docs/Web/CSS/:where)) insieme ai suoi parametri, non hanno effetto sulla specificità.

Le pseudo-classi di negazione ([`:not()`](/it/docs/Web/CSS/:not)), selettore relazionale ([`:has()`](/it/docs/Web/CSS/:has)), il selettore di abbinamento ([`:is()`](/it/docs/Web/CSS/:is)) e [CSS nesting](/it/docs/Web/CSS/CSS_nesting/Nesting_and_specificity) non aggiungono da sole specificità, ma i loro parametri o regole annidate lo fanno. Il peso di specificità che ciascuna contribuisce all'algoritmo di specificità è il peso di specificità del selettore nel parametro o nella regola annidata con il peso maggiore.

La seguente tabella mostra alcuni esempi isolati per farla entrare nel tema. Provi a passare attraverso questi esempi e si assicuri di capire perché hanno la specificità che abbiamo dato loro. Non abbiamo ancora trattato i selettori in dettaglio, ma può trovare i dettagli di ogni selettore nel riferimento dei [selettori su MDN](/it/docs/Web/CSS/CSS_selectors/Selectors_and_combinators).

| Selettore                                | Identificatori | Classi | Elementi | Specificità totale |
| ---------------------------------------- | -------------- | ------ | -------- | ------------------ |
| `h1`                                     | 0              | 0      | 1        | 0-0-1              |
| `h1 + p::first-letter`                   | 0              | 0      | 3        | 0-0-3              |
| `li > a[href*="en-US"] > .inline-warning`| 0              | 2      | 2        | 0-2-2              |
| `#identifier`                            | 1              | 0      | 0        | 1-0-0              |
| `button:not(#mainBtn, .cta)`             | 1              | 0      | 1        | 1-0-1              |

Prima di procedere, diamo un'occhiata a un esempio in azione.

```html live-sample___specificity-boxes
<div class="container" id="outer">
  <div class="container" id="inner">
    <ul>
      <li class="nav"><a href="#">One</a></li>
      <li class="nav"><a href="#">Two</a></li>
    </ul>
  </div>
</div>
```

```css live-sample___specificity-boxes
/* 1. specificity: 1-0-1 */
#outer a {
  background-color: red;
}

/* 2. specificity: 2-0-1 */
#outer #inner a {
  background-color: blue;
}

/* 3. specificity: 1-0-4 */
#outer div ul li a {
  color: yellow;
}

/* 4. specificity: 1-1-3 */
#outer div ul .nav a {
  color: white;
}

/* 5. specificity: 0-2-4 */
div div li:nth-child(2) a:hover {
  border: 10px solid black;
}

/* 6. specificity: 0-2-3 */
div li:nth-child(2) a:hover {
  border: 10px dashed black;
}

/* 7. specificity: 0-3-3 */
div div .nav:nth-child(2) a:hover {
  border: 10px double black;
}

a {
  display: inline-block;
  line-height: 40px;
  font-size: 20px;
  text-decoration: none;
  text-align: center;
  width: 200px;
  margin-bottom: 10px;
}

ul {
  padding: 0;
}

li {
  list-style-type: none;
}
```

{{EmbedLiveSample("specificity-boxes")}}

Quindi cosa sta succedendo qui? Innanzitutto, siamo interessati solo alle prime sette regole di questo esempio, e come noterà, abbiamo incluso i loro valori di specificità in un commento prima di ciascuna.

- I primi due selettori stanno competendo per lo styling del colore di sfondo del collegamento. Il secondo vince e rende il colore di sfondo blu perché ha un extra selettore ID nella catena: la sua specificità è 2-0-1 contro 1-0-1.
- I selettori 3 e 4 stanno competendo per lo styling del colore del testo del collegamento. Il secondo vince e rende il testo bianco perché, anche se ha un selettore elemento in meno, il selettore mancante è sostituito con un selettore di classe, che ha più peso di infiniti selettori di elemento. La specificità vincente è 1-1-3 contro 1-0-4.
- I selettori 5–7 stanno competendo per lo styling del bordo del collegamento quando è sospeso. Il selettore 6 chiaramente perde contro il selettore 5 con una specificità di 0-2-3 contro 0-2-4; ha un selettore di elemento in meno nella catena. Il selettore 7, tuttavia, batte sia i selettori 5 che 6 perché ha lo stesso numero di sotto-selettori nella catena del selettore 5, ma un elemento è stato sostituito con un selettore di classe. Quindi la specificità vincente è 0-3-3 contro 0-2-3 e 0-2-4.

> [!NOTE]
> Ogni tipo di selettore ha il proprio livello di specificità che non può essere sovrascritto da selettori con un livello di specificità inferiore. Ad esempio, _un milione_ di selettori di **classe** combinati non sarebbe in grado di sovrascrivere la specificità di _un_ selettore di **ID**.
>
> Il modo migliore per valutare la specificità è quello di segnare i livelli di specificità singolarmente partendo dal più alto e passando al più basso se necessario. Solo quando c'è un pareggio tra i punteggi dei selettori all'interno di una colonna di specificità deve valutare la colonna successiva; altrimenti, può ignorare i selettori con specificità inferiore poiché non possono mai sovrascrivere i selettori con specificità superiore.

#### ID contro classi

I selettori di ID hanno una specificità elevata. Questo significa che gli stili applicati in base alla corrispondenza di un selettore di ID sovrascriveranno gli stili applicati in base ad altri selettori, inclusi selettori di classe e di tipo. Poiché un ID può apparire solo una volta in una pagina e a causa della specificità elevata dei selettori di ID, è preferibile aggiungere una classe a un elemento invece di un ID.

Se l'utilizzo dell'ID è l'unico modo per individuare l'elemento — forse perché non ha accesso al markup e non può modificarlo — consideri di utilizzare l'ID all'interno di un [selettore di attributi](/it/docs/Web/CSS/Attribute_selectors), come `p[id="header"]`.

### Stili in linea

Gli stili in linea, cioè la dichiarazione di stile all'interno di un attributo [`style`](/it/docs/Web/HTML/Reference/Global_attributes/style), hanno la precedenza su tutti gli stili normali, indipendentemente dalla specificità. Tali dichiarazioni non hanno selettori, ma la loro specificità può essere considerata come 1-0-0-0; sempre più di qualsiasi altro peso di specificità, indipendentemente da quanti ID siano nei selettori.

### !important

C'è uno speciale pezzo di CSS che può utilizzare per sovrascrivere tutti i calcoli sopra menzionati, anche gli stili in linea - il flag `!important`. Tuttavia, dovrebbe essere molto attento quando lo usa. Questo flag viene utilizzato per rendere una coppia di proprietà e valori individuali la regola più specifica, superando così le regole normali della cascata, inclusi gli stili in linea normali.

> [!NOTE]
> È utile sapere che esiste il flag `!important` così potrà riconoscerlo quando lo trova nel codice di altri. **Tuttavia, le consigliamo vivamente di non usarlo a meno che non sia assolutamente necessario.** Il flag `!important` cambia il modo in cui la cascata normalmente funziona, quindi può rendere molto difficile capire i problemi di debug del CSS, specialmente in un foglio di stile di grandi dimensioni.

Dia un'occhiata a questo esempio dove abbiamo due paragrafi, uno dei quali ha un ID.

```html live-sample___important
<p class="better">This is a paragraph.</p>
<p class="better" id="winning">One selector to rule them all!</p>
```

```css live-sample___important
#winning {
  background-color: red;
  border: 1px solid black;
}

.better {
  background-color: gray;
  border: none !important;
}

p {
  background-color: blue;
  color: white;
  padding: 5px;
}
```

{{EmbedLiveSample("important")}}

Camminiamo attraverso questo esempio per vedere cosa sta succedendo — provi a rimuovere alcune delle proprietà per vedere cosa succede se sta trovando difficile comprendere:

1. Noterà che i valori di {{cssxref("color")}} e {{cssxref("padding")}} della terza regola sono stati applicati, ma {{cssxref("background-color")}} non è stato applicato. Perché? Davvero, tutti e tre dovrebbero essere applicati poiché le regole successive nell'ordine di origine generalmente sovrascrivono le regole precedenti.
2. Tuttavia, le regole sopra vincono perché i selettori di classe hanno una specificità maggiore rispetto ai selettori di elemento.
3. Entrambi gli elementi hanno una [`class`](/it/docs/Web/HTML/Reference/Global_attributes/class) di `better`, ma il 2º ha un [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) di `winning` anche. Poiché gli ID hanno una specificità _ancora più alta_ rispetto alle classi (può avere uno e un solo elemento con ciascun ID univoco in una pagina, ma molti elementi con la stessa classe — i selettori di ID sono _molto specifici_ in ciò che targetizzano), il colore di sfondo rosso e il bordo nero di 1px dovrebbero essere applicati al 2º elemento, con il primo elemento che ottiene il colore di sfondo grigio, e nessun bordo, come specificato dalla classe.
4. Il 2º element _ottiene_ il colore di sfondo rosso, ma non il bordo. Perché? A causa del flag `!important` nella seconda regola. Aggiungere il flag `!important` dopo `border: none` significa che questa dichiarazione vincerà sul valore del `border` nella regola precedente, anche se il selettore di ID ha una specificità più alta.

> [!NOTE]
> L'unico modo per sovrascrivere una dichiarazione importante è includere un'altra dichiarazione importante con la _stessa specificità_ più avanti nell'ordine di origine, o una con specificità più elevata.

Una situazione in cui potrebbe dover utilizzare il flag `!important` è quando lavora su un CMS dove non può modificare i moduli CSS core, e vuole davvero sovrascrivere uno stile in linea o una dichiarazione importante che non può essere sovrascritta in nessun altro modo. Ma davvero, non lo usi se può evitarlo.

## L'effetto della posizione CSS

Infine, è importante notare che la precedenza di una dichiarazione CSS dipende dal foglio di stile in cui è specificata.

È possibile per gli utenti impostare fogli di stile personalizzati per sovrascrivere gli stili dello sviluppatore. Ad esempio, un utente ipovedente potrebbe voler impostare la dimensione del carattere su tutte le pagine web che visita al doppio della dimensione normale per consentire una lettura più agevole.

### Ordine delle dichiarazioni che sovrascrivono

Le dichiarazioni in conflitto verranno applicate nel seguente ordine, con le dichiarazioni più recenti che sovrascrivono quelle precedenti:

1. Dichiarazioni nei fogli di stile dell'agente utente (ad esempio, gli stili predefiniti del browser, utilizzati quando non è impostato nessun altro stile).
2. Dichiarazioni normali nei fogli di stile dell'utente (stili personalizzati impostati da un utente).
3. Dichiarazioni normali nei fogli di stile dell'autore (questi sono gli stili impostati da noi, gli sviluppatori web).
4. Dichiarazioni importanti nei fogli di stile dell'autore.
5. Dichiarazioni importanti nei fogli di stile dell'utente.
6. Dichiarazioni importanti nei fogli di stile dell'agente utente.

> [!NOTE]
> L'ordine di precedenza è invertito per gli stili contrassegnati con `!important`. Ha senso che i fogli di stile degli sviluppatori web sovrascrivano i fogli di stile dell'utente, in modo che il design possa essere mantenuto come previsto; tuttavia, a volte gli utenti hanno buoni motivi per sovrascrivere gli stili dello sviluppatore web, come menzionato sopra, e ciò può essere ottenuto utilizzando `!important` nelle loro regole.

## Testa le sue abilità!

È arrivato alla fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare che abbia trattenuto queste informazioni prima di procedere — vedere [Testa le tue abilità: La Cascata](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade).

## Sommario

Se ha compreso la maggior parte di questo articolo, allora ben fatto — ha iniziato a familiarizzare con la meccanica fondamentale di CSS.

Se non ha compreso appieno la cascata, la specificità e l'ereditarietà, allora non si preoccupi! Questo è sicuramente il tema più complicato che abbiamo coperto fino ad ora nel corso ed è qualcosa che anche i professionisti del web development a volte trovano difficile. Le consigliamo di tornare su questo articolo un paio di volte mentre continua il corso, e di continuare a rifletterci su.

Riferisca qui se inizia a incontrare strani problemi con stili che non si applicano come previsto. Potrebbe essere un problema di specificità.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics")}}
