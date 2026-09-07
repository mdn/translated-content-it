---
title: Gestione dei conflitti
slug: Learn_web_development/Core/Styling_basics/Handling_conflicts
l10n:
  sourceCommit: f99d00a1c3697e26a679925954e26564e7e79b98
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model", "Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade", "Learn_web_development/Core/Styling_basics")}}

L'obiettivo di questa lezione è sviluppare la comprensione di alcuni dei concetti più fondamentali di CSS — la cascata, la specificità e l'ereditarietà — che controllano il modo in cui CSS viene applicato a HTML e come vengono risolti i conflitti tra le dichiarazioni di stile.

Sebbene affrontare questa lezione possa sembrare nell'immediato meno rilevante e un po' più accademico rispetto ad altre parti del corso, comprendere questi concetti eviterà molti problemi in seguito. Si consiglia di affrontare attentamente questa sezione e di verificare di aver compreso i concetti prima di proseguire.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Fondamenti di HTML (studiare la
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">selettori CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere come le regole possano entrare in conflitto in CSS.</li>
          <li>Ereditarietà.</li>
          <li>La cascata.</li>
          <li>I concetti principali che regolano l'esito dei conflitti — specificità, ordine sorgente e importanza.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Regole in conflitto

CSS significa **Cascading Style Sheets** e la prima parola, _cascading_, è incredibilmente importante da comprendere: il modo in cui si comporta la cascata è fondamentale per comprendere CSS.

A un certo punto, durante il lavoro su un progetto, può capitare di scoprire che del CSS che si ritiene debba essere applicato a un elemento non funziona. Spesso, questo problema si verifica quando vengono create due regole che applicano valori diversi della stessa proprietà allo stesso elemento.

La [**cascata**](/it/docs/Web/CSS/Guides/Cascade/Introduction), e il concetto strettamente correlato di [**specificità**](/it/docs/Web/CSS/Guides/Cascade/Specificity), sono meccanismi che controllano quale regola viene applicata quando si verifica un conflitto di questo tipo. La dichiarazione che assegna lo stile all'elemento potrebbe non essere quella prevista, quindi è necessario comprendere il funzionamento di questi meccanismi.

È importante anche il concetto di [**ereditarietà**](/it/docs/Web/CSS/Guides/Cascade/Inheritance), che significa che alcune proprietà CSS ereditano per impostazione predefinita i valori impostati sull'elemento genitore dell'elemento corrente, mentre altre non lo fanno. Anche questo può causare comportamenti imprevisti.

Iniziamo osservando brevemente i concetti principali coinvolti, poi esamineremo ciascuno di essi e vedremo come interagiscono tra loro e con il CSS. Questi concetti possono sembrare difficili da comprendere, ma diventeranno più chiari con una maggiore pratica nella scrittura di CSS.

### Cascata

I fogli di stile seguono la [**cascata**](/it/docs/Web/CSS/Guides/Cascade/Introduction). A un livello molto semplice, ciò significa che l'origine e l'ordine delle regole CSS sono importanti. Quando due regole hanno la stessa specificità, viene usata quella definita per ultima nel foglio di stile. Esistono altri concetti che hanno effetto, come i [livelli di cascata](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers), ma sono più avanzati e non verranno trattati qui in dettaglio.

Nell'esempio seguente, sono presenti due regole che potrebbero applicarsi all'elemento `<h1>`. Il contenuto di `<h1>` finisce per essere colorato di blu. Questo perché entrambe le regole provengono dalla stessa origine, hanno un selettore di elemento identico e quindi la stessa specificità, ma vince l'ultima nell'ordine sorgente.

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

La [specificità](/it/docs/Web/CSS/Guides/Cascade/Specificity) è un algoritmo che il browser usa per decidere quale valore di proprietà viene applicato a un elemento. Se più regole hanno selettori diversi che impostano valori diversi per la stessa proprietà e hanno come destinazione lo stesso elemento, la specificità decide il valore della proprietà da applicare all'elemento. La specificità è fondamentalmente una misura di quanto sia specifica la selezione di un selettore:

- Un selettore di tipo (elemento) è meno specifico; selezionerà tutti gli elementi di quel tipo presenti in una pagina e quindi ha meno peso. I selettori di pseudo-elemento hanno la stessa specificità dei normali selettori di elemento.
- Un selettore di classe è più specifico; selezionerà solo gli elementi di una pagina che hanno uno specifico valore dell'attributo `class` e quindi ha più peso. I selettori di attributo e le pseudo-classi hanno lo stesso peso di una classe.
- Un selettore ID è ancora più specifico: seleziona soltanto un singolo elemento con uno specifico valore `id`. Di conseguenza, ha ancora più peso.

Di seguito sono presenti ancora due regole che potrebbero applicarsi all'elemento `<h1>`. Il contenuto di `<h1>` finisce per essere colorato di `red`, anche se la dichiarazione `color: blue` appare più tardi nell'ordine sorgente, perché il selettore di classe `main-heading` assegna alla propria regola una specificità superiore rispetto al selettore di tipo `h1`. Viene applicata la dichiarazione con la specificità maggiore, definita mediante il selettore di classe.

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

L'algoritmo di specificità verrà spiegato più avanti.

### Ereditarietà

Anche l'ereditarietà deve essere compresa in questo contesto: alcuni valori delle proprietà CSS impostati sugli elementi genitori vengono ereditati dai relativi elementi figli, mentre altri no.

Ad esempio, se si impostano `color` e `font-family` su un elemento, ogni elemento al suo interno verrà anch'esso stilizzato con quel colore e quel carattere, a meno che non siano stati applicati direttamente valori di colore e carattere diversi.

```html live-sample___inheritance-simple
<p>
  As the body has been set to have a color of blue this is inherited through the
  descendants.
</p>
<p>
  We can change the color by specifically targeting an element with a different
  style, such as this
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

Alcune proprietà non vengono ereditate, ad esempio {{cssxref("width")}}. Se si imposta un valore `width` pari a `50%` su un elemento, tutti i suoi discendenti non ricevono una larghezza pari al `50%` della `width` del genitore. Se fosse così, CSS sarebbe molto frustrante da usare.

> [!NOTE]
> Nelle pagine di riferimento delle proprietà CSS su MDN, è possibile trovare un riquadro di informazioni tecniche chiamato "Formal definition", che elenca diversi dati relativi a quella proprietà, incluso se viene ereditata o meno. Per un esempio, vedere la sezione [Formal definition della proprietà color](/it/docs/Web/CSS/Reference/Properties/color#formal_definition).

### Comprendere come i concetti lavorano insieme

Questi tre concetti (cascata, specificità ed ereditarietà) controllano insieme quale CSS viene applicato a quale elemento. Nelle sezioni seguenti vedremo come funzionano insieme. A volte può sembrare un po' complicato, ma inizieranno a essere ricordati con una maggiore esperienza con CSS ed è sempre possibile consultare i dettagli se vengono dimenticati. Nemmeno gli sviluppatori esperti ricordano tutti i dettagli.

## Comprendere l'ereditarietà

Iniziamo dall'ereditarietà. Nell'esempio seguente, è presente un elemento {{HTMLElement("ul")}} con due livelli di liste non ordinate annidate al suo interno. Alla `<ul>` esterna sono stati assegnati un bordo, una spaziatura interna e un colore del carattere.

La proprietà `color` è una proprietà ereditata. Quindi, il valore della proprietà `color` viene applicato ai figli diretti e anche ai figli indiretti: gli `<li>` figli immediati e quelli all'interno della prima lista annidata. È stata poi aggiunta la classe `special` alla seconda lista annidata e le è stato applicato un colore diverso. Questo viene quindi ereditato dai suoi figli.

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
  border: 2px solid #cccccc;
  padding: 1em;
}

.special {
  color: black;
  font-weight: bold;
}
```

{{EmbedLiveSample("inheritance", "", "280px")}}

Proprietà come `width` (come menzionato in precedenza), `margin`, `padding` e `border` non sono proprietà ereditate. Se un bordo venisse ereditato dai figli in questo esempio di lista, ogni singola lista ed elemento della lista riceverebbe un bordo: probabilmente non sarebbe mai un effetto desiderato.

Sebbene ogni pagina CSS relativa a una proprietà indichi se questa venga ereditata o meno, spesso è possibile intuirlo se si conosce quale aspetto verrà stilizzato dal valore della proprietà.

### Controllare l'ereditarietà

CSS fornisce cinque valori speciali universali per controllare l'ereditarietà. Ogni proprietà CSS accetta questi valori.

- {{cssxref("inherit")}}
  - : Imposta il valore della proprietà applicato a un elemento selezionato uguale a quello del suo elemento genitore. In pratica, "attiva l'ereditarietà".
- {{cssxref("initial")}}
  - : Imposta il valore della proprietà applicato a un elemento selezionato al [valore iniziale](/it/docs/Web/CSS/Guides/Cascade/Property_value_processing#initial_value) di quella proprietà.
- {{cssxref("revert")}}
  - : Reimposta il valore della proprietà applicato a un elemento selezionato allo stile predefinito del browser anziché ai valori predefiniti applicati a quella proprietà. Questo valore si comporta come {{cssxref("unset")}} in molti casi.
- {{cssxref("revert-layer")}}
  - : Reimposta il valore della proprietà applicato a un elemento selezionato al valore stabilito in un precedente [livello di cascata](/it/docs/Web/CSS/Reference/At-rules/@layer).
- {{cssxref("unset")}}
  - : Reimposta la proprietà al suo valore naturale, il che significa che se la proprietà è naturalmente ereditata si comporta come `inherit`, altrimenti si comporta come `initial`.

> [!NOTE]
> Consultare [Tipi di origine](/it/docs/Web/CSS/Guides/Cascade/Introduction#origin_types) per maggiori informazioni su ciascuno di questi valori e sul loro funzionamento.

### Provare le proprietà di controllo dell'ereditarietà

È possibile osservare un elenco di collegamenti ed esplorare il funzionamento dei valori universali. L'esempio live seguente permette di modificare il CSS e osservare cosa accade quando vengono apportate modifiche. Provare il codice è davvero il modo migliore per comprendere meglio HTML e CSS.

Ad esempio:

1. Al secondo elemento della lista è applicata la classe `my-class-1`. Questa imposta il colore dell'elemento `<a>` annidato al suo interno su `inherit`. Rimuovendo la regola, come cambia il colore del collegamento?
2. È chiaro perché il terzo e il quarto collegamento hanno quel colore? Il terzo collegamento è impostato su `initial`, quindi usa il valore iniziale della proprietà (in questo caso nero) e non il valore predefinito del browser per i collegamenti, che è blu. Il quarto è impostato su `unset`, il che significa che il testo del collegamento usa il colore dell'elemento genitore, verde.
3. Quali collegamenti cambieranno colore se viene definito un nuovo colore per l'elemento `<a>` — ad esempio `a { color: red; }`?
4. Dopo aver letto la sezione successiva sulla reimpostazione di tutte le proprietà, tornare qui e modificare la proprietà `color` in `all`. Si noti come il secondo collegamento si trovi su una nuova riga e abbia un punto elenco. Quali proprietà sono state ereditate?

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

La proprietà abbreviata CSS {{cssxref("all")}} può essere usata per applicare uno di questi valori di ereditarietà a (quasi) tutte le proprietà contemporaneamente. Il suo valore può essere uno qualsiasi dei valori di ereditarietà (`inherit`, `initial`, `revert`, `revert-layer` o `unset`). È un modo pratico per annullare le modifiche apportate agli stili, così da poter tornare a un punto di partenza noto prima di iniziare nuove modifiche.

Nell'esempio seguente sono presenti due citazioni a blocchi. Alla prima viene applicato uno stile all'elemento blockquote stesso. Alla seconda viene applicata una classe al blockquote, che imposta il valore di `all` su `unset`.

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

Provare a impostare il valore di `all` su alcuni degli altri valori disponibili e osservare la differenza.

## Comprendere la cascata

Ora è chiaro che l'ereditarietà è il motivo per cui un paragrafo annidato in profondità nella struttura HTML ha lo stesso colore del CSS applicato al body. Dalle lezioni introduttive, è noto come modificare il CSS applicato a qualcosa in qualsiasi punto del documento, sia assegnando CSS a un elemento sia creando una classe. Ora verrà esaminato il modo in cui la cascata definisce quali regole CSS vengono applicate quando più blocchi di stile applicano la stessa proprietà, ma con valori diversi, allo stesso elemento.

Ci sono tre fattori da considerare, elencati qui in ordine crescente di importanza. Quelli successivi prevalgono su quelli precedenti:

1. **Ordine sorgente**
2. **Specificità**
3. **Importanza**

Vedremo questi fattori per capire in che modo i browser determinano esattamente quale CSS applicare.

### Ordine sorgente

È già stato visto come l'ordine sorgente sia importante per la cascata. Se è presente più di una regola, tutte con esattamente lo stesso peso, vincerà quella che si trova per ultima nel CSS. È possibile considerarlo in questo modo: la regola più vicina all'elemento stesso sovrascrive quelle precedenti, finché l'ultima vince e ottiene di stilizzare l'elemento.

L'ordine sorgente è importante solo quando il peso di specificità delle regole è lo stesso, quindi esaminiamo ora la specificità.

### Specificità

Capiterà spesso di trovarsi in una situazione in cui si sa che una regola si trova più avanti nel foglio di stile, ma viene applicata una regola precedente in conflitto. Ciò accade perché la regola precedente ha una **specificità più alta**: è più specifica e viene quindi scelta dal browser per stilizzare l'elemento.

Come visto in precedenza in questa lezione, un selettore di classe ha più peso di un selettore di elemento, quindi le proprietà definite nel blocco di stile della classe sovrascriveranno quelle definite nel blocco di stile dell'elemento.

È importante notare che, sebbene si stia pensando ai selettori e alle regole applicate al testo o al componente che selezionano, non viene sovrascritta l'intera regola, ma soltanto le proprietà dichiarate in più punti.

Questo comportamento aiuta a evitare ripetizioni nel CSS. Una pratica comune consiste nel definire stili generici per gli elementi di base e quindi creare classi per quelli differenti. Ad esempio, nel foglio di stile seguente sono stati definiti stili generici per le intestazioni di livello 2 e poi create alcune classi che modificano soltanto alcune proprietà e valori. I valori definiti inizialmente vengono applicati a tutte le intestazioni, quindi i valori più specifici vengono applicati alle intestazioni con le classi.

```html live-sample___mixing-rules
<h2>Heading with no class</h2>
<h2 class="small">Heading with class of small</h2>
<h2 class="bright">Heading with class of bright</h2>
```

```css live-sample___mixing-rules
h2 {
  font-size: 2em;
  color: black;
  font-family: "Georgia", serif;
}

.small {
  font-size: 1em;
}

.bright {
  color: rebeccapurple;
}
```

{{EmbedLiveSample("mixing-rules", "", "240px")}}

Vediamo ora come il browser calcola la specificità. È già noto che un selettore di elemento ha bassa specificità e può essere sovrascritto da una classe. In sostanza, viene assegnato un valore in punti a diversi tipi di selettori e la loro somma determina il peso di quel particolare selettore, che può quindi essere confrontato con altre potenziali corrispondenze.

La quantità di specificità di un selettore viene misurata usando tre valori distinti (o componenti), che possono essere considerati colonne ID, CLASSE ed ELEMENTO, del valore rispettivamente di centinaia, decine e unità:

- **ID**: assegnano un punto in questa colonna (100 punti) per ogni selettore ID contenuto nel selettore complessivo.
- **Classi**: assegnano un punto in questa colonna (10 punti) per ogni selettore di classe, selettore di attributo o pseudo-classe contenuto nel selettore complessivo.
- **Elementi**: assegnano un punto in questa colonna (1 punto) per ogni selettore di elemento o pseudo-elemento contenuto nel selettore complessivo.

> [!NOTE]
> Il selettore universale ([`*`](/it/docs/Web/CSS/Reference/Selectors/Universal_selectors)), i [combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators) (`+`, `>`, `~`, ' ') e il selettore di regolazione della specificità ({{cssxref(":where()")}}), insieme ai relativi parametri, non hanno effetto sulla specificità.

La tabella seguente mostra alcuni esempi isolati per iniziare. Provare a esaminarli e assicurarsi di comprendere perché hanno la specificità assegnata. I dettagli di ogni selettore sono disponibili nel [riferimento ai selettori](/it/docs/Web/CSS/Guides/Selectors/Selectors_and_combinators) di MDN.

| Selettore                                 | Identificatori | Classi | Elementi | Specificità totale |
| ----------------------------------------- | -------------- | ------ | -------- | ------------------ |
| `h1`                                      | 0              | 0      | 1        | 0-0-1              |
| `h1 + p::first-letter`                    | 0              | 0      | 3        | 0-0-3              |
| `li > a[href*="en-US"] > .inline-warning` | 0              | 2      | 2        | 0-2-2              |
| `#identifier`                             | 1              | 0      | 0        | 1-0-0              |

#### Esempio approfondito di specificità

Prima di proseguire, vediamo un esempio in azione. Potrebbe essere utile aprirlo in MDN Playground in una scheda separata, così da poterlo consultare facilmente durante la lettura della spiegazione.

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

{{EmbedLiveSample("specificity-boxes", "100%", "170")}}

Cosa sta succedendo? Prima di tutto, interessano soltanto le prime sette regole di questo esempio e, come si può notare, i loro valori di specificità sono stati inclusi in un commento prima di ciascuna regola.

- I primi due selettori competono per lo stile di `background-color` del collegamento. Vince il secondo e rende il colore di sfondo `blue`, perché contiene un selettore ID aggiuntivo nella catena: la sua specificità è 2-0-1 contro 1-0-1.
- I selettori 3 e 4 competono per lo stile del `color` del testo del collegamento. Vince il secondo e rende il testo `white` perché, pur avendo un selettore di elemento in meno, il selettore mancante viene sostituito da un selettore di classe, che ha più peso di un selettore di elemento. La specificità vincente è 1-1-3 contro 1-0-4.
- I selettori 5–7 competono per lo stile del `border` del collegamento al passaggio del puntatore. Il selettore 6 perde chiaramente contro il selettore 5, con una specificità di 0-2-3 contro 0-2-4; ha un selettore di elemento in meno nella catena. Il selettore 7, tuttavia, batte entrambi i selettori 5 e 6 perché ha lo stesso numero di sotto-selettori nella catena del selettore 5, ma un elemento è stato sostituito da un selettore di classe. Quindi la specificità vincente è 0-3-3 contro 0-2-3 e 0-2-4.

> [!NOTE]
> Ogni tipo di selettore ha il proprio livello di specificità, che non può essere sovrascritto da selettori con un livello di specificità inferiore. Ad esempio, un _milione_ di selettori di **classe** combinati non potrebbe sovrascrivere la specificità di _un_ selettore **id**.
>
> Il modo migliore per valutare la specificità consiste nel calcolare i livelli di specificità individualmente, partendo da quello più alto e procedendo verso quello più basso quando necessario. È necessario valutare la colonna successiva soltanto quando c'è un pareggio tra i punteggi dei selettori all'interno di una colonna di specificità; altrimenti, i selettori con specificità inferiore possono essere ignorati, poiché non potranno mai sovrascrivere quelli con specificità superiore.

#### ID rispetto alle classi

I selettori ID hanno un'alta specificità. Ciò significa che gli stili applicati in base alla corrispondenza con un selettore ID prevalgono sugli stili applicati in base ad altri selettori, inclusi i selettori di classe e di tipo. Poiché un ID può comparire una sola volta in una pagina e data l'elevata specificità dei selettori ID, è preferibile aggiungere una classe a un elemento anziché un ID.

Se l'uso dell'ID è l'unico modo per selezionare l'elemento, magari perché non si ha accesso al markup e non è possibile modificarlo, si può considerare di usare l'ID all'interno di un [selettore di attributo](/it/docs/Web/CSS/Reference/Selectors/Attribute_selectors), come `p[id="header"]`.

### Stili inline

Gli stili inline, ovvero la dichiarazione di stile all'interno di un attributo [`style`](/it/docs/Web/HTML/Reference/Global_attributes/style), hanno la precedenza su tutti gli stili normali, indipendentemente dalla specificità. Tali dichiarazioni non hanno selettori, ma la loro specificità può essere considerata pari a 1-0-0-0, sempre maggiore di qualsiasi altro peso di specificità, indipendentemente dal numero di ID presenti nei selettori.

### !important

Esiste una speciale istruzione CSS che può essere usata per prevalere su tutti i calcoli precedenti, inclusi gli stili inline: il flag `!important`. Tuttavia, è necessario prestare molta attenzione quando lo si usa. Questo flag viene utilizzato per rendere una singola coppia proprietà-valore la regola più specifica, sovrascrivendo così le normali regole della cascata, inclusi gli stili inline normali.

> [!NOTE]
> È utile sapere che il flag `!important` esiste, per riconoscerlo quando lo si incontra nel codice di altre persone. **Tuttavia, si consiglia vivamente di non usarlo mai, a meno che non sia assolutamente necessario.** Il flag `!important` modifica il normale funzionamento della cascata, quindi può rendere molto difficile il debug dei problemi CSS, in particolare in un foglio di stile di grandi dimensioni.

Osservare questo esempio, in cui sono presenti due paragrafi, uno dei quali ha un ID.

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

Analizziamo cosa sta accadendo: provare a rimuovere alcune proprietà per osservare cosa succede, se risulta difficile da comprendere:

1. Si noterà che sono stati applicati i valori {{cssxref("color")}} e {{cssxref("padding")}} della terza regola, ma non {{cssxref("background-color")}}. Perché? In realtà, tutti e tre dovrebbero essere applicati, poiché le regole successive nell'ordine sorgente generalmente sovrascrivono quelle precedenti.
2. Tuttavia, le regole precedenti vincono perché i selettori di classe hanno una specificità più alta rispetto ai selettori di elemento.
3. Entrambi gli elementi hanno una [`class`](/it/docs/Web/HTML/Reference/Global_attributes/class) pari a `better`, ma il secondo ha anche un [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) pari a `winning`. Poiché gli ID hanno una specificità _ancora più alta_ delle classi, `background-color` `red` e `border` `1px black` dovrebbero essere entrambi applicati al secondo elemento; il primo elemento dovrebbe invece ricevere il colore di sfondo grigio e nessun bordo, come specificato dalla classe.
4. Il secondo elemento riceve _effettivamente_ `background-color` `red`, ma nessun `border`. Perché? A causa del flag `!important` nella seconda regola. Aggiungere il flag `!important` dopo `border: none` significa che questa dichiarazione vincerà sul valore `border` nella regola precedente, anche se il selettore ID ha una specificità più alta.

> [!NOTE]
> L'unico modo per sovrascrivere una dichiarazione importante è includere un'altra dichiarazione importante con la _stessa specificità_ più tardi nell'ordine sorgente, oppure una con specificità superiore.

Una situazione in cui potrebbe essere necessario usare il flag `!important` è quando si lavora su un CMS in cui non è possibile modificare i moduli CSS principali e si desidera davvero sovrascrivere uno stile inline o una dichiarazione importante che non può essere sovrascritta in nessun altro modo. Tuttavia, non usarlo se è possibile evitarlo.

## L'effetto della posizione del CSS

Infine, è importante notare che la precedenza di una dichiarazione CSS dipende dal foglio di stile in cui è specificata.

È possibile che gli utenti impostino fogli di stile personalizzati per sovrascrivere gli stili dello sviluppatore. Ad esempio, un utente con disabilità visiva potrebbe voler impostare la dimensione del carattere su tutte le pagine web visitate al doppio della dimensione normale, per facilitare la lettura.

### Ordine delle dichiarazioni che prevalgono

Le dichiarazioni in conflitto vengono applicate nel seguente ordine, con quelle successive che sovrascrivono le precedenti:

1. Dichiarazioni nei fogli di stile dell'user agent, ad esempio gli stili predefiniti del browser, usati quando non è impostato nessun altro stile.
2. Dichiarazioni normali nei fogli di stile dell'utente (stili personalizzati impostati da un utente).
3. Dichiarazioni normali nei fogli di stile dell'autore (gli stili impostati dagli sviluppatori web).
4. Dichiarazioni importanti nei fogli di stile dell'autore.
5. Dichiarazioni importanti nei fogli di stile dell'utente.
6. Dichiarazioni importanti nei fogli di stile dell'user agent.

> [!NOTE]
> L'ordine di precedenza è invertito per gli stili contrassegnati con `!important`. È logico che i fogli di stile degli sviluppatori web sovrascrivano quelli degli utenti, affinché il design possa essere mantenuto come previsto; tuttavia, a volte gli utenti hanno buone ragioni per sovrascrivere gli stili degli sviluppatori web, come menzionato in precedenza, e ciò può essere ottenuto usando `!important` nelle proprie regole.

## Riepilogo

Se la maggior parte di questo articolo è stata compresa, ottimo lavoro: si sta iniziando a prendere familiarità con i meccanismi fondamentali di CSS.

Se cascata, specificità ed ereditarietà non sono state comprese completamente, non è un problema. Questo è sicuramente l'argomento più complesso affrontato finora nel corso ed è qualcosa che persino gli sviluppatori web professionisti a volte trovano difficile. Si consiglia di tornare a questo articolo più volte man mano che si prosegue nel corso e di continuare a rifletterci.

Fare riferimento a questa pagina se iniziano a verificarsi strani problemi con gli stili che non vengono applicati come previsto. Potrebbe trattarsi di un problema di specificità. Successivamente verranno proposti alcuni test che possono essere usati per verificare quanto bene siano state comprese e ricordate le informazioni fornite sulla cascata.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model", "Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade", "Learn_web_development/Core/Styling_basics")}}
