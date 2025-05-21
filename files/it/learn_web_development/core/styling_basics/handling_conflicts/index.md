---
title: Gestire i conflitti
slug: Learn_web_development/Core/Styling_basics/Handling_conflicts
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics")}}

L'obiettivo di questa lezione è sviluppare la tua comprensione di alcuni dei concetti più fondamentali di CSS — la cascata, la specificità e l'ereditarietà — che controllano come il CSS viene applicato a HTML e come vengono risolti i conflitti tra dichiarazioni di stile.

Potrebbe sembrare meno immediatamente rilevante e un po' più accademico rispetto ad altre parti del corso, ma comprendere questi concetti ti risparmierà molto dolore in seguito! Ti incoraggiamo a lavorare con attenzione su questa sezione e a verificare di aver compreso i concetti prima di procedere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studiare
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
          <li>I principali concetti che governano l'esito dei conflitti — specificità, ordine delle fonti e importanza.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Regole in conflitto

CSS sta per **Cascading Style Sheets**, e quella prima parola _cascading_ è incredibilmente importante da comprendere — il modo in cui si comporta la cascata è fondamentale per comprendere CSS.

A un certo punto, lavorando su un progetto, potresti scoprire che il CSS che pensavi dovesse essere applicato a un elemento non funziona. Spesso il problema è creare due regole che applicano valori diversi della stessa proprietà allo stesso elemento. La [**cascata**](/it/docs/Web/CSS/CSS_cascade/Cascade) e il concetto strettamente correlato di [**specificità**](/it/docs/Web/CSS/CSS_cascade/Specificity) sono meccanismi che controllano quale regola si applica quando vi è un conflitto. La regola che sta stilizzando il tuo elemento potrebbe non essere quella che ti aspettavi, quindi è necessario capire come funzionano questi meccanismi.

Significativo è anche il concetto di [**ereditarietà**](/it/docs/Web/CSS/CSS_cascade/Inheritance), il che significa che alcune proprietà CSS di default ereditano i valori impostati sull'elemento padre attuale e altre no. Questo può anche causare un comportamento che potresti non aspettarti.

Cominciamo dando un rapido sguardo alle cose principali con cui ci stiamo occupando, poi esamineremo ciascuna e vedremo come interagiscono tra loro e con il tuo CSS. Possono sembrare un insieme di concetti difficili da comprendere. Man mano che fai pratica scrivendo CSS, il modo in cui funziona diventerà più ovvio per te.

### Cascata

I fogli di stile [**cascano**](/it/docs/Web/CSS/CSS_cascade/Cascade) — a un livello molto semplice, ciò significa che l'origine e l'ordine delle regole CSS sono importanti. Quando due regole hanno entrambe la stessa specificità, quella definita per ultima nel foglio di stile è quella che verrà utilizzata. Ci sono altri concetti che hanno un effetto, come i [livelli della cascata](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers), ma questi sono più avanzati e non verranno trattati in dettaglio qui.

Nell'esempio qui sotto, abbiamo due regole che potrebbero applicarsi all'elemento `<h1>`. Il contenuto dell'`<h1>` finisce per essere colorato di blu. Questo avviene perché entrambe le regole provengono dalla stessa fonte, hanno un selettore di elementi identico e quindi hanno la stessa specificità, ma l'ultima nell'ordine delle fonti vince.

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

La [specificità](/it/docs/Web/CSS/CSS_cascade/Specificity) è l'algoritmo che il browser utilizza per decidere quale valore di proprietà viene applicato a un elemento. Se più blocchi di stile hanno selettori diversi che configurano la stessa proprietà con valori diversi e puntano allo stesso elemento, la specificità decide il valore della proprietà che viene applicato all'elemento. La specificità è fondamentalmente una misura di quanto sarà specifica la selezione di un selettore:

- Un selettore di elementi è meno specifico; selezionerà tutti gli elementi di quel tipo che appaiono su una pagina, quindi ha meno peso. I selettori pseudo-elementi hanno la stessa specificità dei normali selettori di elementi.
- Un selettore di classi è più specifico; selezionerà solo gli elementi su una pagina che hanno un valore specifico dell'attributo `class`, quindi ha più peso. I selettori di attributo e le pseudo-classi hanno lo stesso peso di una classe.

Sotto, abbiamo di nuovo due regole che potrebbero applicarsi all'elemento `<h1>`. Il contenuto dell'`<h1>` finisce per essere colorato di rosso perché il selettore di classe `main-heading` conferisce alla sua regola una specificità più alta. Quindi, anche se la regola con il selettore di elementi `<h1>` appare più in basso nell'ordine delle fonti, quella con la specificità più alta, definita utilizzando il selettore di classe, verrà applicata.

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

Spiegheremo l'algoritmo della specificità più avanti.

### Ereditarietà

Anche l'ereditarietà deve essere compresa in questo contesto — alcuni valori delle proprietà CSS impostati sugli elementi padre vengono ereditati dai loro elementi figlio, e alcuni no.

Ad esempio, se imposti un `color` e `font-family` su un elemento, ogni elemento all'interno sarà stilizzato con quel colore e font, a meno che tu non abbia applicato valori di colore e font diversi direttamente a loro.

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

Alcune proprietà non ereditano — ad esempio, se imposti una {{cssxref("width")}} del 50% su un elemento, tutti i suoi discendenti non acquisiscono una larghezza del 50% della larghezza del loro genitore. Se fosse così, CSS sarebbe molto frustrante da usare!

> [!NOTE]
> Nelle pagine di riferimento delle proprietà CSS su MDN, puoi trovare una casella di informazioni tecniche chiamata "Definizione formale", che elenca una serie di punti dati su quella proprietà, incluso se è ereditata o meno. Vedi la [sezione Definizione formale della proprietà color](/it/docs/Web/CSS/color#formal_definition) come esempio.

### Comprendere come i concetti funzionano insieme

Questi tre concetti (cascata, specificità ed ereditarietà) insieme controllano quale CSS si applica a quale elemento. Nelle sezioni seguenti, vedremo come funzionano insieme. A volte può sembrare un po' complicato, ma comincerai a ricordarteli man mano che acquisisci più esperienza con CSS, e puoi sempre cercare i dettagli se te li dimentichi! Anche gli sviluppatori esperti non ricordano tutti i dettagli.

## Comprendere l'ereditarietà

Inizieremo con l'ereditarietà. Nell'esempio qui sotto, abbiamo un elemento {{HTMLElement("ul")}} con due livelli di elenchi non ordinati annidati al suo interno. Abbiamo dato al `<ul>` esterno un bordo, riempimento e colore del font.

La proprietà `color` è una proprietà ereditata. Quindi, il valore della proprietà `color` è applicato ai figli diretti e anche ai figli indiretti — gli immediati figli `<li>` e quelli all'interno del primo elenco annidato. Abbiamo poi aggiunto la classe `special` al secondo elenco annidato e gli abbiamo applicato un colore diverso. Questo poi si eredita attraverso i suoi figli.

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

Proprietà come `width` (come menzionato in precedenza), `margin`, `padding` e `border` non sono proprietà ereditate. Se un bordo fosse ereditato dai figli in questo esempio di elenco, ogni singolo elenco e elemento di elenco acquisirebbe un bordo — probabilmente non un effetto che vorremmo mai!

Sebbene ogni pagina della proprietà CSS elenchi se la proprietà è ereditata o meno, puoi spesso intuire lo stesso se sai quale aspetto la proprietà valuterà.

### Controllo dell'ereditarietà

CSS fornisce cinque valori speciali universali delle proprietà per controllare l'ereditarietà. Ogni proprietà CSS accetta questi valori.

- {{cssxref("inherit")}}
  - : Imposta il valore della proprietà applicato a un elemento selezionato per essere lo stesso di quello del suo elemento padre. Effettivamente, questo "attiva l'ereditarietà".
- {{cssxref("initial")}}
  - : Imposta il valore della proprietà applicato a un elemento selezionato al [valore iniziale](/it/docs/Web/CSS/CSS_cascade/Value_processing#initial_value) di quella proprietà.
- {{cssxref("revert")}}
  - : Reimposta il valore della proprietà applicata a un elemento selezionato allo stile predefinito del browser anziché ai predefiniti applicati a quella proprietà. Questo valore si comporta come {{cssxref("unset")}} in molti casi.
- {{cssxref("revert-layer")}}
  - : Reimposta il valore della proprietà applicata a un selezionato per il valore stabilito in un precedente [livello di cascata](/it/docs/Web/CSS/@layer).
- {{cssxref("unset")}}
  - : Reimposta la proprietà al suo valore naturale, il che significa che se la proprietà è naturalmente ereditata si comporta come `inherit`, altrimenti si comporta come `initial`.

> [!NOTE]
> Vedi [Tipi di origine](/it/docs/Web/CSS/CSS_cascade/Cascade#origin_types) per ulteriori informazioni su ciascuno di questi e su come funzionano.

Possiamo esaminare un elenco di link ed esplorare come funzionano i valori universali. Il seguente esempio dal vivo ti consente di giocare con il CSS e vedere cosa succede quando apporti modifiche. Giocare con il codice è davvero il modo migliore per comprendere meglio HTML e CSS.

Ad esempio:

1. Al secondo elemento dell'elenco è applicata la classe `my-class-1`. Questo imposta il colore dell'elemento `<a>` annidato all'interno su `inherit`. Se rimuovi la regola, come cambia il colore del link?
2. Capisci perché il terzo e quarto link sono del colore che sono? Il terzo link è impostato su `initial`, il che significa che utilizza il valore iniziale della proprietà (in questo caso nero) e non il valore predefinito del browser per i link, che è blu. Il quarto è impostato su `unset` il che significa che il testo del link utilizza il colore dell'elemento padre, verde.
3. Quali dei link cambieranno colore se definisci un nuovo colore per l'elemento `<a>` — ad esempio `a { color: red; }`?
4. Dopo aver letto la prossima sezione sulla reimpostazione di tutte le proprietà, torna e cambia la proprietà `color` in `all`. Nota come il secondo link sia su una nuova riga e abbia un punto. Quali proprietà pensi siano state ereditate?

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

### Reimpostazione di tutti i valori delle proprietà

La proprietà abbreviata CSS [`all`](/it/docs/Web/CSS/all) può essere utilizzata per applicare uno di questi valori di ereditarietà a (quasi) tutte le proprietà in una volta. Il suo valore può essere uno dei valori di ereditarietà (`inherit`, `initial`, `revert`, `revert-layer` o `unset`). È un modo conveniente per annullare le modifiche apportate agli stili in modo da poter tornare a un punto di partenza noto prima di iniziare nuove modifiche.

Nell'esempio sottostante, abbiamo due blockquote. Il primo ha uno stile applicato all'elemento blockquote stesso. Il secondo ha una classe applicata al blockquote, che imposta il valore di `all` su `unset`.

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

Prova a impostare il valore di `all` su alcuni degli altri valori disponibili e osserva quale sia la differenza.

## Comprendere la cascata

Ora comprendiamo che l'ereditarietà è il motivo per cui un paragrafo annidato nella struttura del tuo HTML ha lo stesso colore del CSS applicato al corpo. Dalle lezioni introduttive, abbiamo una comprensione di come cambiare il CSS applicato a qualcosa in qualsiasi punto del documento — sia assegnando CSS a un elemento o creando una classe. Ora esamineremo come la cascata definisce quali regole CSS si applicano quando più di un blocco di stile applica la stessa proprietà, ma con valori diversi, allo stesso elemento.

Ci sono tre fattori da considerare, elencati qui in ordine crescente di importanza. Le ultime regolano sulle prime:

1. **Ordine delle fonti**
2. **Specificità**
3. **Importanza**

Esamineremo questi fattori per vedere come i browser determinano esattamente quale CSS dovrebbe essere applicato.

### Ordine delle fonti

Abbiamo già visto come l'ordine delle fonti abbia importanza per la cascata. Se hai più di una regola, tutte con lo stesso peso, allora quella che viene per ultima nel CSS vincerà. Puoi pensare a questo come: la regola che è più vicina all'elemento stesso sovrascrive le precedenti fino a quando l'ultima vince e stila l'elemento.

L'ordine delle fonti ha importanza solo quando il peso in specificità delle regole è lo stesso, quindi diamo un'occhiata alla specificità:

### Specificità

Spesso ti troverai in una situazione in cui sai che una regola viene successivamente nel foglio di stile, ma viene applicata una regola più in alto in conflitto. Questo accade perché la regola in alto ha una **specificità più alta** — è più specifica e quindi viene scelta dal browser come quella che dovrebbe stilizzare l'elemento.

Come abbiamo visto in precedenza in questa lezione, un selettore di classi ha più peso di un selettore di elementi, quindi le proprietà definite nel blocco di stile di classe sovrascriveranno quelle definite nel blocco di stile di elemento.

Da notare qui è che sebbene stiamo pensando a selettori e regole che vengono applicati al testo o al componente che selezionano, non è l'intera regola che viene sovrascritta, solo le proprietà che sono dichiarate in più punti.

Questo comportamento aiuta a evitare ripetizioni nel tuo CSS. Una pratica comune è definire stili generici per gli elementi di base, e poi creare classi per quelli che sono differenti. Ad esempio, nel foglio di stile sottostante, abbiamo definito stili generici per intestazioni di livello 2 e poi creato alcune classi che cambiano solo alcune delle proprietà e valori. I valori definiti inizialmente sono applicati a tutte le intestazioni, quindi i valori più specifici sono applicati alle intestazioni con le classi.

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

Ora diamo un'occhiata a come il browser calcolerà la specificità. Sappiamo già che un selettore di elementi ha una specificità bassa e può essere sovrascritto da una classe. Essenzialmente, un valore in punti viene assegnato a diversi tipi di selettori, e sommando questi valori si ottiene il peso di quel particolare selettore, che può quindi essere valutato rispetto ad altri potenziali corrispondenze.

La quantità di specificità che un selettore ha è misurata utilizzando tre valori diversi (o componenti), che possono essere pensati come colonne ID, CLASS, ed ELEMENT nei centinaia, decine, e unità:

- **Identificatori**: Assegna uno in questa colonna per ogni selettore ID contenuto all'interno del selettore globale.
- **Classi**: Assegna uno in questa colonna per ogni selettore di classe, selettore di attributo, o pseudo-classe contenuta all'interno del selettore globale.
- **Elementi**: Assegna uno in questa colonna per ogni selettore di elementi o pseudo-elemento contenuto all'interno del selettore globale.

> [!NOTE]
> Il selettore universale ([`*`](/it/docs/Web/CSS/Universal_selectors)), i [combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators) (`+`, `>`, `~`, ' '), e il selettore di regolazione della specificità ([`:where()`](/it/docs/Web/CSS/:where)) insieme ai suoi parametri, non hanno effetto sulla specificità.

Le pseudo-classi di negazione ([`:not()`](/it/docs/Web/CSS/:not)), selettore relazionale ([`:has()`](/it/docs/Web/CSS/:has)), e il [CSS nesting](/it/docs/Web/CSS/CSS_nesting/Nesting_and_specificity) stesso, non aggiungono specificità, ma i loro parametri o regole annidate sì. Il peso di specificità che ognuno contribuisce all'algoritmo di specificità è il peso di specificità del selettore nel parametro o nella regola annidata con il peso maggiore.

La seguente tabella mostra alcuni esempi isolati per entrare nel clima. Prova a esaminare questi esempi e assicurati di capire perché hanno la specificità che abbiamo dato loro. Non abbiamo ancora trattato in dettaglio i selettori, ma puoi trovare dettagli di ciascun selettore sulla piattaforma [di riferimento ai selettori di MDN](/it/docs/Web/CSS/CSS_selectors/Selectors_and_combinators).

| Selettore                                | Identificatori | Classi | Elementi | Specificità totale |
| -----------------------------------------| -------------  | ------ | -------- | ------------------- |
| `h1`                                      | 0              | 0      | 1        | 0-0-1               |
| `h1 + p::first-letter`                    | 0              | 0      | 3        | 0-0-3               |
| `li > a[href*="en-US"] > .inline-warning` | 0              | 2      | 2        | 0-2-2               |
| `#identifier`                             | 1              | 0      | 0        | 1-0-0               |
| `button:not(#mainBtn, .cta`)              | 1              | 0      | 1        | 1-0-1               |

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

Quindi, cosa sta succedendo qui? Prima di tutto, siamo interessati solo alle prime sette regole di questo esempio, e come noterai, abbiamo incluso i loro valori di specificità in un commento prima di ciascuna.

- I primi due selettori competono sullo stile del colore di sfondo del link. Il secondo vince e rende lo sfondo blu perché ha un selettore ID aggiuntivo nella catena: la sua specificità è 2-0-1 rispetto a 1-0-1.
- I selettori 3 e 4 competono sullo stile del colore del testo del link. Il secondo vince e rende il testo bianco perché, anche se ha un selettore di elementi in meno, il selettore mancante è sostituito da un selettore di classe, che ha più peso di infiniti selettori di elementi. La specificità vincente è 1-1-3 rispetto a 1-0-4.
- I selettori 5–7 competono sullo stile del bordo del link al passaggio del mouse. Il selettore 6 perde chiaramente contro il selettore 5 con una specificità di 0-2-3 rispetto a 0-2-4; ha un selettore di elementi in meno nella catena. Tuttavia, il selettore 7 batte entrambi i selettori 5 e 6 perché ha lo stesso numero di sotto-selettori nella catena del selettore 5, ma un selettore di elementi è stato sostituito da un selettore di classe. Quindi, la specificità vincente è 0-3-3 rispetto a 0-2-3 e 0-2-4.

> [!NOTE]
> Ogni tipo di selettore ha il proprio livello di specificità che non può essere sovrascritto da selettori con un livello di specificità inferiore. Ad esempio, un _milione_ di selettori **classe** combinati non sarebbe in grado di sovrascrivere la specificità di _uno_ selettore **id**.
>
> Il modo migliore per valutare la specificità è di valutare i livelli di specificità individualmente iniziando dal più alto e andando verso il più basso quando necessario. Solo quando c'è un pareggio tra i punteggi dei selettori all'interno di una colonna di specificità ti serve valutare la colonna successiva verso il basso; altrimenti, puoi ignorare i selettori a specificità inferiore poiché non possono mai sovrascrivere i selettori a specificità più alta.

#### ID contro classi

I selettori ID hanno specificità alta. Questo significa che gli stili applicati basandosi sull'appartenenza a un selettore ID sovrascriveranno gli stili applicati basandosi su altri selettori, incluse classi e selettori di tipo. Poiché un ID può verificarsi solo una volta su una pagina e a causa dell'alta specificità dei selettori ID, è preferibile aggiungere una classe a un elemento invece di un ID.

Se utilizzare l'ID è l'unico modo per puntare all'elemento — forse perché non hai accesso all'HTML e non puoi modificarlo — considera l'utilizzo dell'ID all'interno di un [selettore di attributi](/it/docs/Web/CSS/Attribute_selectors), come `p[id="header"]`.

### Stili inline

Gli stili inline, vale a dire, la dichiarazione all'interno di un attributo [`style`](/it/docs/Web/HTML/Reference/Global_attributes/style), hanno la precedenza su tutti i normali stili, indipendentemente dalla specificità. Tali dichiarazioni non hanno selettori, ma la loro specificità può essere considerata come 1-0-0-0; sempre più di qualsiasi altro peso di specificità, indipendentemente dal numero di ID nei selettori.

### !important

Esiste un pezzo speciale di CSS che puoi usare per sovrascrivere tutti i calcoli precedenti, anche gli stili inline - il flag `!important`. Tuttavia, dovresti essere molto attento durante il suo utilizzo. Questo flag viene utilizzato per rendere una coppia di proprietà e valori individuale il regola più specifica, sovrascrivendo così le normali regole della cascata, incluse le normali stili inline.

> [!NOTE]
> È utile sapere che il flag `!important` esiste in modo che tu sappia cosa è quando lo incontri nel codice di altre persone. **Tuttavia, raccomandiamo fortemente di non usarlo mai a meno che non sia assolutamente necessario.** Il flag `!important` cambia il modo in cui la cascata funziona normalmente, quindi può rendere molto difficile la risoluzione di problemi CSS, soprattutto in un grande foglio di stile.

Dai un'occhiata a questo esempio in cui abbiamo due paragrafi, uno dei quali ha un ID.

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

Esaminiamo questo per vedere cosa sta succedendo — prova a rimuovere alcune delle proprietà per vedere cosa succede se trovi difficile comprendere:

1. Noterai che i valori di {{cssxref("color")}} e {{cssxref("padding")}} della terza regola sono stati applicati, ma non il {{cssxref("background-color")}}. Perché? In realtà, tutte e tre dovrebbero sicuramente applicarsi perché le regole più in basso nell'ordine delle fonti generalmente sovrascrivono le regole più alte.
2. Tuttavia, le regole sopra vincono perché i selettori di classe hanno una specificità più alta rispetto ai selettori di elementi.
3. Entrambi gli elementi hanno una [`class`](/it/docs/Web/HTML/Reference/Global_attributes/class) di `better`, ma il 2° ha un [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) di `winning` anche. Poiché gli ID hanno una specificità _ancora più alta_ rispetto alle classi (puoi avere solo un elemento con ciascun ID unico su una pagina, ma molti elementi con la stessa classe — i selettori ID sono _molto specifici_ in ciò che prendono di mira), il colore di sfondo rosso e il bordo nero da 1px dovrebbero entrambi essere applicati al 2° elemento, con il primo elemento che riceve il colore di sfondo grigio, e nessun bordo, come specificato dalla classe.
4. Il 2° elemento _ottiene_ il colore di sfondo rosso, ma nessun bordo. Perché? A causa del flag `!important` nella seconda regola. L'aggiunta del flag `!important` dopo `border: none` significa che questa dichiarazione vincerà sul valore del `border` nella regola precedente, anche se il selettore ID ha una specificità più alta.

> [!NOTE]
> L'unico modo per sovrascrivere una dichiarazione importante è includere un'altra dichiarazione importante con la _stessa specificità_ più avanti nell'ordine delle fonti, o una con specificità più alta.

Un caso in cui potresti dover utilizzare il flag `!important` è quando stai lavorando su un CMS in cui non puoi modificare i moduli CSS di base, e vuoi davvero sovrascrivere uno stile inline o una dichiarazione importante che non può essere sovrascritta in nessun altro modo. Ma davvero, non usarlo se puoi evitarlo.

## L'effetto della collocazione del CSS

Infine, è importante notare che la precedenza di una dichiarazione CSS dipende dal foglio di stile in cui è specificata.

È possibile per gli utenti impostare fogli di stile personalizzati per sovrascrivere gli stili dello sviluppatore. Ad esempio, un utente con problemi di vista potrebbe voler impostare la dimensione del font su tutte le pagine web che visita al doppio della dimensione normale per consentire una lettura più facile.

### Ordine delle dichiarazioni che si sovrascrivono

Le dichiarazioni in conflitto saranno applicate nel seguente ordine, con le successive che sovrascrivono le precedenti:

1. Dichiarazioni nei fogli di stile dell'user agent (es. stili predefiniti del browser, usati quando nessun altro stile è stato impostato).
2. Dichiarazioni normali nei fogli di stile dell'utente (stili personalizzati impostati da un utente).
3. Dichiarazioni normali nei fogli di stile dell'autore (questi sono gli stili impostati da noi, gli sviluppatori web).
4. Dichiarazioni importanti nei fogli di stile dell'autore.
5. Dichiarazioni importanti nei fogli di stile dell'utente.
6. Dichiarazioni importanti nei fogli di stile dell'user agent.

> [!NOTE]
> L'ordine della precedenza è invertito per gli stili contrassegnati con `!important`. Ha senso che i fogli di stile degli sviluppatori web sovrascrivano i fogli di stile degli utenti, in modo che il design possa essere mantenuto come previsto; tuttavia, a volte gli utenti hanno buone ragioni per sovrascrivere gli stili degli sviluppatori web, come menzionato sopra, e questo può essere ottenuto usando `!important` nelle loro regole.

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma ricordi le informazioni più importanti? Puoi trovare ulteriori test per verificare se hai conservato queste informazioni prima di procedere — vedi [Test your skills: The Cascade](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade).

## Riepilogo

Se hai capito la maggior parte di questo articolo, allora ben fatto — hai iniziato a familiarizzare con i meccanismi fondamentali di CSS.

Se non hai compreso appieno la cascata, la specificità e l'ereditarietà, non preoccuparti! Questo è decisamente l'argomento più complicato che abbiamo trattato finora nel corso ed è qualcosa che anche gli sviluppatori web professionisti a volte trovano difficile. Ti consigliamo di tornare a questo articolo alcune volte mentre continui il corso e di continuare a rifletterci sopra.

Torna qui se inizi a incontrare problemi strani con gli stili che non si applicano come previsto. Potrebbe essere un problema di specificità.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics")}}
