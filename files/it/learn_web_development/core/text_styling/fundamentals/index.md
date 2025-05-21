---
title: Stile di base del testo e dei caratteri
short-title: Fondamenti di testo e caratteri
slug: Learn_web_development/Core/Text_styling/Fundamentals
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{NextMenu("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling")}}

In questo articolo inizieremo il tuo viaggio verso la padronanza dello stile del testo con {{Glossary("CSS", "CSS")}}. Esamineremo tutti i fondamenti di base dello stile del testo/font in dettaglio, inclusa la regolazione del peso, della famiglia e dello stile del carattere, lo shorthand per i font, l'allineamento del testo e altri effetti, e la spaziatura tra linee e lettere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS: nozioni di base sullo stile</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere i concetti di famiglie di font, stack di font e font sicuri per il web.</li>
          <li>Impostare il colore, il peso, la dimensione e lo stile del font.</li>
          <li>Impostare l'allineamento, la trasformazione e la decorazione del testo.</li>
          <li>Impostare l'altezza della linea.</li>
          <li>Conoscere che esistono diverse altre proprietà di stile del testo e del font ed essere incoraggiati a esplorarle.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa comporta la stilizzazione del testo in CSS?

Il testo all'interno di un elemento viene disposto all'interno del [content box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#parts_of_a_box) dell'elemento. Inizia nell'angolo superiore sinistro dell'area di contenuto (o nell'angolo superiore destro, nel caso di contenuto in lingua RTL) e fluisce verso la fine della riga. Una volta raggiunta la fine, scende alla riga successiva e fluisce nuovamente fino alla fine. Questo schema si ripete fino a quando tutto il contenuto è stato inserito nel box. Il contenuto testuale si comporta effettivamente come una serie di elementi inline, disposti su righe adiacenti tra loro, senza creare interruzioni di linea fino al raggiungimento della fine della riga, o a meno che non si forzi manualmente un'interruzione di linea usando l'elemento {{htmlelement("br")}}.

> [!NOTE]
> Se il paragrafo sopra ti lascia confuso, non importa — torna e rivedi il nostro articolo [Modello a scatola](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) per potenziare la tua comprensione teorica del modello a scatola prima di continuare.

Le proprietà CSS utilizzate per stilizzare il testo generalmente rientrano in due categorie, che esamineremo separatamente in questo articolo:

- **Stili di font**: Proprietà che influenzano il font di un testo, ad esempio quale font viene applicato, la sua dimensione e se è grassetto, corsivo, ecc.
- **Stili di layout del testo**: Proprietà che influenzano la spaziatura e altre caratteristiche di layout del testo, permettendo di manipolare, per esempio, lo spazio tra le linee e le lettere, e come il testo è allineato all'interno del content box.

> [!NOTE]
> Ricorda che il testo all'interno di un elemento è tutto influenzato come un'unica entità. Non è possibile selezionare e stilizzare sottosezioni di testo a meno che non siano avvolte in un elemento appropriato (come {{htmlelement("span")}} o {{htmlelement("strong")}}), o utilizzando pseudo-elementi specifici per il testo come [`::first-letter`](/it/docs/Web/CSS/::first-letter) (seleziona la prima lettera del testo di un elemento), [`::first-line`](/it/docs/Web/CSS/::first-line) (seleziona la prima linea del testo di un elemento), o [`::selection`](/it/docs/Web/CSS/::selection) (seleziona il testo attualmente evidenziato dal cursore).

## Font

Procediamo direttamente a esaminare le proprietà per lo stile dei font. In questo esempio, applicheremo alcune proprietà CSS al seguente esempio HTML:

```html
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

Puoi trovare l'[esempio completo su GitHub](https://mdn.github.io/learning-area/css/styling-text/fundamentals/) (vedi anche [il codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-text/fundamentals/index.html)).

### Colore

La proprietà {{cssxref("color")}} imposta il colore del contenuto in primo piano degli elementi selezionati, che di solito è il testo, ma può anche includere un paio di altre cose, come una sottolineatura o sovrallineatura posta sul testo utilizzando la proprietà {{cssxref("text-decoration")}}.

`color` può accettare qualsiasi [unità di colore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color), per esempio:

```css
p {
  color: red;
}
```

Questo farà sì che i paragrafi diventino rossi, anziché il nero predefinito del browser, come segue:

```html hidden
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

{{ EmbedLiveSample('Color', '100%', 230) }}

### Famiglie di font

Per impostare un font diverso per il tuo testo, utilizza la proprietà {{cssxref("font-family")}} — questa ti permette di specificare un font (o una lista di font) per il browser da applicare agli elementi selezionati. Il browser applicherà un font solo se è disponibile sulla macchina da cui si accede al sito; in caso contrario, utilizzerà semplicemente un [font predefinito del browser](#font_predefiniti). Un semplice esempio appare così:

```css
p {
  font-family: Arial;
}
```

Questo farebbe sì che tutti i paragrafi su una pagina utilizzino il font arial, che si trova su qualsiasi computer.

> [!NOTE]
> Il modulo [Web-safe fonts](https://scrimba.com/learn-html-and-css-c0p/~01r?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una guida interattiva su perché i font sono importanti, i font sicuri per il web e come specificare i font in CSS, insieme a una sfida per testare le tue conoscenze.

#### Font sicuri per il web

Parlando di disponibilità dei font, c'è solo un numero limitato di font generalmente disponibili su tutti i sistemi e che quindi possono essere utilizzati senza molte preoccupazioni. Questi sono i cosiddetti **font sicuri per il web**.

La maggior parte delle volte, come sviluppatori web, vogliamo avere un controllo più specifico sui font utilizzati per visualizzare il nostro contenuto testuale. Il problema è trovare un modo per sapere quale font è disponibile sul computer utilizzato per visualizzare le nostre pagine web. Non esiste un modo per saperlo in ogni caso, ma i font sicuri per il web sono noti per essere disponibili su quasi tutte le istanze dei sistemi operativi più usati (Windows, macOS, le distribuzioni Linux più comuni, Android e iOS).

L'elenco effettivo dei font sicuri per il web cambierà con l'evolversi dei sistemi operativi, ma è ragionevole considerare i seguenti font sicuri per il web, almeno per ora (molti di essi sono stati resi popolari grazie all'iniziativa Microsoft _[Core fonts for the Web](https://en.wikipedia.org/wiki/Core_fonts_for_the_Web)_ alla fine degli anni '90 e all'inizio degli anni 2000):

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col">Nome</th>
      <th scope="col">Tipo generico</th>
      <th scope="col">Note</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Arial</td>
      <td>sans-serif</td>
      <td>
        È considerata una buona pratica aggiungere anche <em>Helvetica</em> come
        alternativa preferita a <em>Arial</em> poiché, sebbene le loro facce di font
        siano quasi identiche, <em>Helvetica</em> è considerato avere una forma più
        elegante, anche se <em>Arial</em> è più ampiamente disponibile.
      </td>
    </tr>
    <tr>
      <td>Courier New</td>
      <td>monospace</td>
      <td>
        Alcuni sistemi operativi hanno una versione alternativa (probabilmente più
        vecchia) del font <em>Courier New</em> chiamata <em>Courier</em>. È
        considerata una buona pratica usare entrambi con <em>Courier New</em> come
        alternativa preferita.
      </td>
    </tr>
    <tr>
      <td>Georgia</td>
      <td>serif</td>
      <td></td>
    </tr>
    <tr>
      <td>Times New Roman</td>
      <td>serif</td>
      <td>
        Alcuni sistemi operativi hanno una versione alternativa (probabilmente più
        vecchia) del font <em>Times New Roman</em> chiamata <em>Times</em>. È
        considerata una buona pratica usare entrambi con <em>Times New Roman</em>
        come alternativa preferita.
      </td>
    </tr>
    <tr>
      <td>Trebuchet MS</td>
      <td>sans-serif</td>
      <td>
        Dovresti essere cauto nell'usare questo font, poiché non è ampiamente
        disponibile sui sistemi operativi mobili.
      </td>
    </tr>
    <tr>
      <td>Verdana</td>
      <td>sans-serif</td>
      <td></td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Tra le varie risorse, il sito web [cssfontstack.com](https://www.cssfontstack.com/) mantiene un elenco di font sicuri per il web disponibili sui sistemi operativi Windows e macOS, che possono aiutarti a decidere cosa considerare sicuro per il tuo utilizzo.

> [!NOTE]
> Esiste un modo per scaricare un font personalizzato insieme a una pagina web, per consentirti di personalizzare l'uso del font in qualsiasi modo desideri: **web fonts**. Questo è un po' più complesso e ne discuteremo in un [articolo separato](/it/docs/Learn_web_development/Core/Text_styling/Web_fonts) più avanti nel modulo.

#### Font predefiniti

CSS definisce cinque nomi generici per i font: `serif`, `sans-serif`, `monospace`, `cursive` e `fantasy`. Questi sono molto generici e la faccia del font esatta che viene utilizzata da questi nomi generici può variare tra i vari browser e sistemi operativi su cui sono visualizzati. Rappresenta uno _scenario peggiore_ in cui il browser cercherà di fornire un font che sembri appropriato. `serif`, `sans-serif` e `monospace` sono piuttosto prevedibili e dovrebbero fornire qualcosa di ragionevole. D'altra parte, `cursive` e `fantasy` sono meno prevedibili e raccomandiamo di usarli con molta cura, testando man mano che procedi.

I cinque nomi sono definiti come segue:

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col">Termine</th>
      <th scope="col">Definizione</th>
      <th scope="col">Esempio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>serif</code></td>
      <td>
        Font che hanno le grazie (le rifiniture e altri piccoli dettagli che si
        vedono alle estremità dei tratti in alcuni tipi di carattere).
      </td>
      <td id="serif-example">
        <pre class="brush: html hidden">My big red elephant</pre>
        <pre class="brush: css hidden">
body {
  font-family: serif;
}</pre
        >
        {{EmbedLiveSample("serif-example", 100, 60)}}
      </td>
    </tr>
    <tr>
      <td><code>sans-serif</code></td>
      <td>Font che non hanno le grazie.</td>
      <td id="sans-serif-example">
        <pre class="brush: html hidden">My big red elephant</pre>
        <pre class="brush: css hidden">
body {
  font-family: sans-serif;
}</pre
        >
        {{EmbedLiveSample("sans-serif-example", 100, 60)}}
      </td>
    </tr>
    <tr>
      <td><code>monospace</code></td>
      <td>
        Font in cui ogni carattere ha la stessa larghezza, tipicamente usati
        nelle elencazioni di codice.
      </td>
      <td id="monospace-example">
        <pre class="brush: html hidden">My big red elephant</pre>
        <pre class="brush: css hidden">
body {
  font-family: monospace;
}</pre
        >
        {{EmbedLiveSample("monospace-example", 100, 60)}}
      </td>
    </tr>
    <tr>
      <td><code>cursive</code></td>
      <td>
        Font che sono progettati per emulare la scrittura a mano, con tratti
        fluidi e collegati.
      </td>
      <td id="cursive-example">
        <pre class="brush: html hidden">My big red elephant</pre>
        <pre class="brush: css hidden">
body {
  font-family: cursive;
}</pre
        >
        {{EmbedLiveSample("cursive-example", 100, 60)}}
      </td>
    </tr>
    <tr>
      <td><code>fantasy</code></td>
      <td>Font che sono progettati per essere decorativi.</td>
      <td id="fantasy-example">
        <pre class="brush: html hidden">My big red elephant</pre>
        <pre class="brush: css hidden">
body {
  font-family: fantasy;
}</pre
        >
        {{EmbedLiveSample("fantasy-example", 100, 60)}}
      </td>
    </tr>
  </tbody>
</table>

#### Font stacks

Poiché non puoi garantire la disponibilità dei font che vuoi utilizzare sulle tue pagine web (anche un web font _potrebbe_ fallire per qualche motivo), puoi fornire una **stack di font** in modo che il browser abbia diversi font tra cui scegliere. Questo coinvolge un valore `font-family` costituito da nomi di font multipli separati da virgole, ad esempio,

```css
p {
  font-family: "Trebuchet MS", Verdana, sans-serif;
}
```

In un caso del genere, il browser inizia dall'inizio dell'elenco e controlla se quel font è disponibile sulla macchina. Se lo è, applica quel font agli elementi selezionati. In caso contrario, passa al font successivo, e così via.

È una buona idea fornire un nome di font generico appropriato alla fine della stack in modo che, se nessuno dei font elencati è disponibile, il browser possa almeno fornire qualcosa di approssimativamente adatto. Per sottolineare questo punto, i paragrafi sono dati il font serif predefinito del browser se nessuna altra opzione è disponibile — che di solito è Times New Roman — questo non va bene per un font sans-serif!

> [!NOTE]
> Sebbene tu possa utilizzare nomi di famiglia di font che contengono uno spazio, come `Trebuchet MS`, senza citare il nome, per evitare errori di escaping, si consiglia di citare i nomi delle famiglie di font che contengono spazi bianchi, cifre o caratteri di punteggiatura diversi dai trattini.

> [!WARNING]
> Qualsiasi nome di famiglia di font che potrebbe essere interpretato erroneamente come un nome di famiglia generico o una parola chiave CSS-wide deve essere citato. Sebbene i nomi delle famiglie di font possano essere inclusi come un {{cssxref("custom-ident")}} o una {{cssxref("string")}}, i nomi delle famiglie di font che risultano essere gli stessi di un valore di proprietà CSS predefinito, come `initial`, o `inherit`, o che condividono il nome con uno dei nomi delle famiglie di font generici, come `sans-serif` o `fantasy`, devono essere inclusi come una stringa citata. Altrimenti, il nome della famiglia di font verrà interpretato come equivalente alla parola chiave CSS o al nome della famiglia generica. Quando usate come parole chiave, i nomi delle famiglie di font generiche —`serif`, `sans-serif`, `monospace`, `cursive`, e `fantasy` — e le parole chiave globali CSS NON DEVONO essere citate, poiché le stringhe non sono interpretate come parole chiave CSS.

#### Un esempio di font-family

Aggiungiamo al nostro esempio precedente, dando ai paragrafi un font sans-serif:

```css
p {
  color: red;
  font-family: Helvetica, Arial, sans-serif;
}
```

Questo ci dà il seguente risultato:

```html hidden
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

{{ EmbedLiveSample('A_font-family_example', '100%', 220) }}

### Dimensione del font

Nell'articolo precedente sui [valori e unità CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units), abbiamo esaminato le unità di lunghezza e dimensione. La dimensione del font (impostata con la proprietà {{cssxref("font-size")}}) può prendere valori misurati in molte di queste unità (e altre, come le [percentuali](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#percentages)); tuttavia, le unità più comuni che userai per dimensionare il testo sono:

- `px` (pixel): Il numero di pixel di altezza che desideri per il testo. Questa è un'unità assoluta — risulta in un valore finale calcolato uguale per il font sulla pagina in praticamente qualsiasi situazione.
- `em`s: 1 `em` è pari alla dimensione del font impostata sull'elemento padre dell'elemento corrente che stiamo stilizzando (più precisamente, la larghezza di una lettera maiuscola M contenuta nell'elemento padre). Questo può diventare complicato da calcolare se hai molti elementi annidati con dimensioni di font differenti impostate, ma è fattibile, come vedrai in seguito. Perché preoccuparsi? È abbastanza naturale una volta che ci si abitua, e puoi usare `em` per dimensionare tutto, non solo il testo. Puoi avere un intero sito web dimensionato usando `em`, il che rende la manutenzione facile.
- `rem`s: Questi funzionano proprio come `em`, tranne per il fatto che 1 `rem` è pari alla dimensione del font impostata sull'elemento root del documento (cioè, {{htmlelement("html")}}), non l'elemento padre. Questo rende i calcoli per determinare le dimensioni dei font molto più semplici.

La `font-size` di un elemento è ereditata dall'elemento padre di quell'elemento. Tutto inizia con l'elemento root dell'intero documento — {{htmlelement("html")}} — la cui `font-size` standard è impostata a `16px` sui browser. Qualsiasi paragrafo (o altro elemento che non ha una dimensione diversa impostata dal browser) all'interno dell'elemento root avrà una dimensione finale di `16px`. Altri elementi possono avere dimensioni predefinite diverse. Ad esempio, un elemento {{htmlelement("Heading_Elements", "h1")}} ha una dimensione predefinita di `2em` impostata, quindi avrà una dimensione finale di `32px`.

Le cose diventano più complicate quando inizi a modificare la dimensione del font di elementi annidati. Ad esempio, se avessi un elemento {{htmlelement("article")}} nella tua pagina e imposti la sua `font-size` a 1,5 `em` (che si tradurrà in una dimensione finale di 24 `px`), e quindi desideri che i paragrafi all'interno degli elementi `<article>` abbiano una dimensione finale di 20 `px`, quale valore `em` dovresti usare?

```html
<!-- document base font-size is 16px -->
<article>
  <!-- If my font-size is 1.5em -->
  <p>My paragraph</p>
  <!-- How do I compute to 20px font-size? -->
</article>
```

Dovresti impostare il suo valore `em` a 20/24, o 0,83333333 `em`. I calcoli possono essere complicati, quindi è necessario essere prudenti su come si stilizzano gli elementi. È meglio usare `rem` dove puoi per mantenere le cose semplici, ed evitare di impostare la `font-size` degli elementi contenitore dove possibile.

### Stile del font, peso del font, trasformazione del testo e decorazione del testo

CSS offre quattro proprietà comuni per alterare il peso/forma visiva del testo:

- {{cssxref("font-style")}}: Usato per attivare o disattivare il testo in corsivo. I valori possibili sono i seguenti (raramente userai questa funzione, a meno che tu non voglia disattivare uno stile già in corsivo per qualche motivo):

  - `normal`: Imposta il testo sul font normale (disattiva eventuali corsivi esistenti).
  - `italic`: Imposta il testo per usare la versione in corsivo del font, se disponibile; in caso contrario, simulerà il corsivo con oblique.
  - `oblique`: Imposta il testo per usare una versione simulata di un font in corsivo, creata inclinando la versione normale.

- {{cssxref("font-weight")}}: Imposta quanto il testo è in grassetto. Questo ha molti valori disponibili nel caso in cui tu abbia molte varianti di font disponibili (come _-light_, _-normal_, _-bold_, _-extrabold_, _-black_, ecc.), ma realisticamente userai raramente altri valori eccetto `normal` e `bold`:

  - `normal`, `bold`: Peso del font normale e grassetto.
  - `lighter`, `bolder`: Imposta il grassetto dell'elemento corrente un passo più leggero o più pesante rispetto al grassetto dell'elemento padre.
  - `100` – `900`: Valori numerici del grassetto che offrono un controllo più fine rispetto alle parole chiave sopra menzionate, se necessario.

- {{cssxref("text-transform")}}: Ti permette di impostare che il tuo font venga trasformato. I valori includono:

  - `none`: Impedisce qualsiasi trasformazione.
  - `uppercase`: Trasforma tutto il testo in maiuscolo.
  - `lowercase`: Trasforma tutto il testo in minuscolo.
  - `capitalize`: Trasforma tutte le parole per avere la prima lettera maiuscola.
  - `full-width`: Trasforma tutti i glifi in modo che siano scritti all'interno di un quadrato di larghezza fissa, in modo simile a un font monospace, consentendo l'allineamento di, ad esempio, caratteri latini insieme a glifi di lingue asiatiche (come cinese, giapponese, coreano).

- {{cssxref("text-decoration")}}: Imposta/disimposta le decorazioni del testo sui font (userai principalmente questo per disimpostare la sottolineatura predefinita sui link quando li stili). I valori disponibili sono:

  - `none`: Disimposta eventuali decorazioni del testo già presenti.
  - `underline`: Sottolinea il testo.
  - `overline`: Aggiunge una linea sopra il testo.
  - `line-through`: Pone una linea attraverso il testo.

  Dovresti notare che {{cssxref("text-decoration")}} può accettare valori multipli contemporaneamente se desideri aggiungere decorazioni multiple simultaneamente, per esempio, `text-decoration: underline overline`. Nota anche che {{cssxref("text-decoration")}} è una proprietà abbreviata per {{cssxref("text-decoration-line")}}, {{cssxref("text-decoration-style")}}, e {{cssxref("text-decoration-color")}}. È possibile utilizzare combinazioni di questi valori di proprietà per creare effetti interessanti, per esempio: `text-decoration: line-through red wavy`.

Diamo un'occhiata ad aggiungere alcune di queste proprietà al nostro esempio:

Il nostro nuovo risultato è il seguente:

```html hidden
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

```css
html {
  font-size: 10px;
}

h1 {
  font-size: 5rem;
  text-transform: capitalize;
}

h1 + p {
  font-weight: bold;
}

p {
  font-size: 1.5rem;
  color: red;
  font-family: Helvetica, Arial, sans-serif;
}
```

{{ EmbedLiveSample('Font_style_font_weight_text_transform_and_text_decoration', '100%', 260) }}

### Effetti ombra al testo

Puoi applicare effetti ombra al testo usando la proprietà {{cssxref("text-shadow")}}. Questa accetta fino a quattro valori, come mostrato nell'esempio seguente:

```css
text-shadow: 4px 4px 5px red;
```

Le quattro proprietà sono le seguenti:

1. Lo spostamento orizzontale dell'ombra dal testo originale — questo può prendere la maggior parte delle unità di [lunghezza e dimensione CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths), ma userai più comunemente `px`; valori positivi spostano l'ombra a destra, mentre i valori negativi a sinistra. Questo valore deve essere incluso.
2. Lo spostamento verticale dell'ombra dal testo originale. Questo si comporta in modo simile allo spostamento orizzontale, eccetto che sposta l'ombra su/giù, non a sinistra/destra. Questo valore deve essere incluso.
3. Il raggio di sfocatura: un valore più alto significa che l'ombra viene dispersa più ampiamente. Se questo valore non è incluso, predefinisce a 0, il che significa nessuna sfocatura. Questo può prendere la maggior parte delle unità di [lunghezza e dimensione CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths).
4. Il colore base dell'ombra, che può prendere qualsiasi [unità di colore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color). Se non incluso, predefinisce a [`currentcolor`](/it/docs/Web/CSS/color_value#currentcolor_keyword), vale a dire che il colore dell'ombra viene preso dalla proprietà [`color`](/it/docs/Web/CSS/color) dell'elemento.

#### Ombre multiple

Puoi applicare ombre multiple allo stesso testo includendo più valori di ombra separati da virgole, per esempio:

```css
h1 {
  text-shadow:
    1px 1px 1px red,
    2px 2px 1px red;
}
```

Se applicassimo questo all'elemento {{htmlelement("Heading_Elements", "h1")}} nel nostro esempio Tommy The Cat, otterremmo questo:

```html hidden
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

```css hidden
html {
  font-size: 10px;
}

h1 {
  font-size: 5rem;
  text-transform: capitalize;
}

h1 + p {
  font-weight: bold;
}

p {
  font-size: 1.5rem;
  color: red;
  font-family: Helvetica, Arial, sans-serif;
}
```

{{ EmbedLiveSample('Multiple_shadows', '100%', 260) }}

> [!NOTE]
> Puoi vedere esempi più interessanti sull'uso di `text-shadow` nell'articolo di Sitepoint [Moonlighting with CSS text-shadow](https://www.sitepoint.com/moonlighting-css-text-shadow/).

## Layout del testo

Con le proprietà di base per i font fuori strada, diamo un'occhiata alle proprietà che possiamo usare per influenzare il layout del testo.

### Allineamento del testo

La proprietà {{cssxref("text-align")}} è usata per controllare come il testo è allineato all'interno del suo contenitore. I valori disponibili sono elencati di seguito. Funzionano in modo molto simile a come fanno in un normale applicativo di elaborazione testi:

- `left`: Giustifica il testo a sinistra.
- `right`: Giustifica il testo a destra.
- `center`: Centra il testo.
- `justify`: Fa sì che il testo si espanda, variando gli spazi tra le parole in modo che tutte le linee di testo abbiano la stessa larghezza. Devi usare questo con attenzione — può sembrare terribile, specialmente quando applicato a un paragrafo con molte parole lunghe. Se hai intenzione di usarlo, dovresti anche pensare di usare qualcos'altro insieme ad esso, come {{cssxref("hyphens")}}, per spezzare alcune delle parole più lunghe su più righe.

Se applichiamo `text-align: center;` all'elemento {{htmlelement("Heading_Elements", "h1")}} nel nostro esempio, otterremmo questo:

```html hidden
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

```css
html {
  font-size: 10px;
}

h1 {
  font-size: 5rem;
  text-transform: capitalize;
  text-shadow:
    1px 1px 1px red,
    2px 2px 1px red;
  text-align: center;
}

h1 + p {
  font-weight: bold;
}

p {
  font-size: 1.5rem;
  color: red;
  font-family: Helvetica, Arial, sans-serif;
}
```

{{ EmbedLiveSample('Text_alignment', '100%', 260) }}

### Altezza della linea

La proprietà {{cssxref("line-height")}} imposta l'altezza di ciascuna linea di testo. Questa proprietà può non solo prendere la maggior parte delle [unità di lunghezza e dimensione](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths), ma può anche prendere un valore senza unità, che funge da moltiplicatore ed è generalmente considerata l'opzione migliore. Con un valore senza unità, la {{cssxref("font-size")}} viene moltiplicata e risulta nell'`line-height`. Il testo del corpo generalmente appare meglio ed è più facile da leggere quando le righe sono distanziate. L'altezza della linea raccomandata è di 1,5 – 2 (doppio spazio). Per impostare le nostre linee di testo a 1,6 volte l'altezza del font, useremmo:

```css
p {
  line-height: 1.6;
}
```

Applicando questo agli elementi {{htmlelement("p")}} nel nostro esempio, otterremmo questo risultato:

```html hidden
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

```css
html {
  font-size: 10px;
}

h1 {
  font-size: 5rem;
  text-transform: capitalize;
  text-shadow:
    1px 1px 1px red,
    2px 2px 1px red;
  text-align: center;
}

h1 + p {
  font-weight: bold;
}

p {
  font-size: 1.5rem;
  color: red;
  font-family: Helvetica, Arial, sans-serif;
  line-height: 1.6;
}
```

{{ EmbedLiveSample('Line_height', '100%', 300) }}

### Spaziatura tra lettere e parole

Le proprietà {{cssxref("letter-spacing")}} e {{cssxref("word-spacing")}} ti permettono di impostare la spaziatura tra le lettere e le parole nel tuo testo. Non le userai molto spesso, ma potrebbero essere utili per ottenere un aspetto specifico o per migliorare la leggibilità di un font particolarmente denso. Possono prendere la maggior parte delle [unità di lunghezza](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths).

Per illustrare, potremmo applicare un po' di spaziatura tra parole e lettere alla prima riga di ciascun elemento {{htmlelement("p")}} nel nostro esempio HTML con:

```css
p::first-line {
  letter-spacing: 4px;
  word-spacing: 4px;
}
```

Questo rende il nostro HTML come:

```html hidden
<h1>Tommy the cat</h1>

<p>Well I remember it as though it were a meal ago…</p>

<p>
  Said Tommy the Cat as he reeled back to clear whatever foreign matter may have
  nestled its way into his mighty throat. Many a fat alley rat had met its
  demise while staring point blank down the cavernous barrel of this awesome
  prowling machine. Truly a wonder of nature this urban predator — Tommy the cat
  had many a story to tell. But it was a rare occasion such as this that he did.
</p>
```

```css
html {
  font-size: 10px;
}

h1 {
  font-size: 5rem;
  text-transform: capitalize;
  text-shadow:
    1px 1px 1px red,
    2px 2px 1px red;
  text-align: center;
  letter-spacing: 2px;
}

h1 + p {
  font-weight: bold;
}

p {
  font-size: 1.5rem;
  color: red;
  font-family: Helvetica, Arial, sans-serif;
  line-height: 1.6;
  letter-spacing: 1px;
}
```

{{ EmbedLiveSample('Letter_and_word_spacing', '100%', 330) }}

### Altre proprietà da esaminare

Le proprietà menzionate sopra ti danno un'idea di come iniziare a stilizzare il testo su una pagina web, ma ci sono molte altre proprietà che potresti usare. Volevamo solo coprire le più importanti qui. Una volta che ti sei abituato a usare le sopra menzionate, dovresti anche esplorare le seguenti:

Stili di font:

- {{cssxref("font-variant")}}: Passare tra piccole maiuscole e normali alternative di carattere.
- {{cssxref("font-kerning")}}: Accendere e spegnere le opzioni di crenatura del carattere.
- {{cssxref("font-feature-settings")}}: Accendere e spegnere vari aspetti del font [OpenType](https://en.wikipedia.org/wiki/OpenType).
- {{cssxref("font-variant-alternates")}}: Controllare l'uso di glifi alternativi per un dato font.
- {{cssxref("font-variant-caps")}}: Controllare l'uso di glifi alternativi per le maiuscole.
- {{cssxref("font-variant-east-asian")}}: Controllare l'uso di glifi alternativi per scritture dell'Asia orientale, come giapponese e cinese.
- {{cssxref("font-variant-ligatures")}}: Controllare quali legature e forme contestuali sono utilizzate nel testo.
- {{cssxref("font-variant-numeric")}}: Controllare l'uso di glifi alternativi per numeri, frazioni e segni ordinali.
- {{cssxref("font-variant-position")}}: Controllare l'uso di glifi alternativi di dimensioni più piccole posizionati come apice o pedice.
- {{cssxref("font-size-adjust")}}: Regolare la dimensione visiva del font indipendentemente dalla sua dimensione reale.
- {{cssxref("font-stretch")}}: Commutare tra possibili alternative versioni allungate di un dato font.
- {{cssxref("text-underline-position")}}: Specificare la posizione delle sottolineature impostate utilizzando il valore `underline` di `text-decoration-line`.
- {{cssxref("text-rendering")}}: Cercare di ottimizzare il rendering del testo.

Stili di layout del testo:

- {{cssxref("text-indent")}}: Specificare quanto spazio orizzontale dovrebbe essere lasciato prima dell'inizio della prima linea del contenuto di testo.
- {{cssxref("text-overflow")}}: Definire come il contenuto traboccante che non è visualizzato è segnalato agli utenti.
- {{cssxref("white-space")}}: Definire come sono gestiti gli spazi bianchi e i relativi interruzioni di linea all'interno dell'elemento.
- {{cssxref("word-break")}}: Specificare se interrompere le linee all'interno delle parole.
- {{cssxref("direction")}}: Definire la direzione del testo. (Questo dipende dalla lingua e di solito è meglio lasciare gestire quella parte al HTML poiché è legata al contenuto testuale.)
- {{cssxref("hyphens")}}: Accendere e spegnere la sillabazione per le lingue supportate.
- {{cssxref("line-break")}}: Rilassare o rafforzare l'interruzione di riga per le lingue asiatiche.
- {{cssxref("text-align-last")}}: Definire come è allineata l'ultima linea di un blocco o una linea, subito prima di un'interruzione di riga forzata.
- {{cssxref("text-orientation")}}: Definire l'orientamento del testo in una linea.
- {{cssxref("overflow-wrap")}}: Specificare se o meno il browser può interrompere le linee all'interno delle parole per prevenire l'overflow.
- {{cssxref("writing-mode")}}: Definire se le linee di testo sono disposte orizzontalmente o verticalmente e la direzione in cui le righe successive fluiscono.

## Abbreviazione del font

Molte proprietà del font possono anche essere impostate tramite la proprietà abbreviata {{cssxref("font")}}. Queste sono scritte nel seguente ordine: {{cssxref("font-style")}}, {{cssxref("font-variant")}}, {{cssxref("font-weight")}}, {{cssxref("font-stretch")}}, {{cssxref("font-size")}}, {{cssxref("line-height")}}, e {{cssxref("font-family")}}.

Tra tutte queste proprietà, solo `font-size` e `font-family` sono richieste quando si utilizza la proprietà abbreviata `font`.

Un barra deve essere inserita tra le proprietà {{cssxref("font-size")}} e {{cssxref("line-height")}}.

Un esempio completo potrebbe apparire in questo modo:

```css
font:
  italic normal bold normal 3em/1.5 Helvetica,
  Arial,
  sans-serif;
```

## Apprendimento attivo: giocare con lo stile del testo

In questa sessione di apprendimento attivo non abbiamo esercizi specifici per te. Desideriamo semplicemente che tu ti diverta a sperimentare alcune proprietà del layout del font/testo. Vedi cosa puoi creare! Puoi farlo utilizzando file HTML/CSS offline, o inserire il tuo codice nell'esempio modificabile dal vivo qui sotto.

Se fai un errore, puoi sempre resettarlo usando il pulsante _Reset_.

```html hidden
<div
  class="body-wrapper"
  style="font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;">
  <h2>HTML Input</h2>
  <textarea
    id="code"
    class="html-input"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;">
<p>Some sample text for your delight</p>
  </textarea>

  <h2>CSS Input</h2>
  <textarea
    id="code"
    class="css-input"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;">
p {

}
  </textarea>

  <h2>Output</h2>
  <div
    class="output"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;"></div>
  <div class="controls">
    <input
      id="reset"
      type="button"
      value="Reset"
      style="margin: 10px 10px 0 0;" />
  </div>
</div>
```

```js hidden
const htmlInput = document.querySelector(".html-input");
const cssInput = document.querySelector(".css-input");
const reset = document.getElementById("reset");
let htmlCode = htmlInput.value;
let cssCode = cssInput.value;
const output = document.querySelector(".output");

const styleElem = document.createElement("style");
const headElem = document.querySelector("head");
headElem.appendChild(styleElem);

function drawOutput() {
  output.innerHTML = htmlInput.value;
  styleElem.textContent = cssInput.value;
}

reset.addEventListener("click", () => {
  htmlInput.value = htmlCode;
  cssInput.value = cssCode;
  drawOutput();
});

htmlInput.addEventListener("input", drawOutput);
cssInput.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

{{ EmbedLiveSample('Active_learning_Playing_with_styling_text', 700, 800) }}

## Sommario

Speriamo che ti sia divertito a giocare con il testo in questo articolo! Il prossimo articolo ti fornirà tutto ciò che devi sapere sullo stile delle liste HTML.

## Vedi anche

- [Tutto sulla proprietà CSS font-family](https://explainers.dev/font-family/), explainers.dev
- [Font sicuri per il web](https://scrimba.com/the-frontend-developer-career-path-c0j/~02b?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>

{{NextMenu("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling")}}
