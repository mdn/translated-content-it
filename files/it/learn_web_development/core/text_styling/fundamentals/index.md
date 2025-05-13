---
title: Fondamenti di stile del testo e dei font
short-title: Fondamenti di testo e font
slug: Learn_web_development/Core/Text_styling/Fundamentals
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{NextMenu("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling")}}

In questo articolo inizieremo il viaggio verso la padronanza dello stile del testo con {{Glossary("CSS", "CSS")}}. Qui tratteremo in dettaglio tutti i fondamenti basilari dello stile del testo/font, inclusa l'impostazione del peso del font, della famiglia e dello stile, il ridimensionamento del font, l'allineamento del testo e altri effetti, nonché la spaziatura tra righe e lettere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS - Nozioni base sullo stile</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere i concetti di famiglie di font, stack di font e font sicuri per il web.</li>
          <li>Impostare il colore, il peso, la dimensione e lo stile del font.</li>
          <li>Impostare l'allineamento, la trasformazione e la decorazione del testo.</li>
          <li>Impostare l'altezza della linea.</li>
          <li>Conoscere che esistono diverse altre proprietà di stile di font e testo, ed essere incoraggiati a esplorarle.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa comporta lo styling del testo in CSS?

Il testo all'interno di un elemento viene disposto all'interno della [content box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#parts_of_a_box) dell'elemento. Inizia dall'angolo in alto a sinistra dell'area di contenuto (o dall'angolo in alto a destra, nel caso di contenuto in una lingua RTL) e scorre fino alla fine della linea. Una volta raggiunta la fine, scende alla riga successiva e scorre fino alla fine ancora. Questo schema si ripete finché tutto il contenuto non è stato posizionato nella casella. Il contenuto del testo si comporta effettivamente come una serie di elementi inline, venendo disposto su linee adiacenti e non creando interruzioni di linea fino a quando non viene raggiunta la fine della linea, o a meno che non forziate un'interruzione di linea manualmente utilizzando l'elemento {{htmlelement("br")}}.

> [!NOTE]
> Se il paragrafo sopra vi lascia confusi, non importa: tornate a rivedere il nostro articolo sul [modello di box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) per rinfrescare la teoria del modello di box prima di continuare.

Le proprietà CSS utilizzate per lo stile del testo generalmente ricadono in due categorie, che esamineremo separatamente in questo articolo:

- **Stile dei font**: Proprietà che influenzano il font di un testo, ad esempio quale font viene applicato, la sua dimensione, e se è in grassetto, corsivo, ecc.
- **Stile del layout del testo**: Proprietà che influenzano la spaziatura e altre caratteristiche del layout del testo, consentendo la manipolazione, ad esempio, dello spazio tra righe e lettere, e di come il testo è allineato all'interno della content box.

> [!NOTE]
> Tenere presente che il testo all'interno di un elemento è tutto influenzato come una singola entità. Non si possono selezionare e stilizzare sottosezioni di testo a meno che non le si avvolgano in un elemento appropriato (come un {{htmlelement("span")}} o un {{htmlelement("strong")}}), o si utilizzi un pseudo-elemento specifico del testo come [`::first-letter`](/it/docs/Web/CSS/::first-letter) (seleziona la prima lettera del testo di un elemento), [`::first-line`](/it/docs/Web/CSS/::first-line) (seleziona la prima riga del testo di un elemento), o [`::selection`](/it/docs/Web/CSS/::selection) (seleziona il testo attualmente evidenziato dal cursore).

## Font

Passiamo subito a esaminare le proprietà per lo stile dei font. In questo esempio, applicheremo alcune proprietà CSS all'esempio HTML seguente:

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

Può trovare l'[esempio completo su GitHub](https://mdn.github.io/learning-area/css/styling-text/fundamentals/) (vedere anche [il codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-text/fundamentals/index.html)).

### Colore

La proprietà {{cssxref("color")}} imposta il colore del contenuto in primo piano degli elementi selezionati, che di solito è il testo, ma può includere anche altre cose, come una sottolineatura o una sovralineatura posta sul testo utilizzando la proprietà {{cssxref("text-decoration")}}.

`color` può accettare qualsiasi [unità di colore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color), ad esempio:

```css
p {
  color: red;
}
```

Questo farà sì che i paragrafi diventino rossi, piuttosto che il nero standard di default del browser, come mostrato:

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

Per impostare un font diverso per il suo testo, si usa la proprietà {{cssxref("font-family")}} — questa permette di specificare un font (o un elenco di font) che il browser deve applicare agli elementi selezionati. Il browser applicherà un font solo se è disponibile sulla macchina in cui si sta accedendo al sito web; in caso contrario, utilizzerà un [font predefinito del browser](#font_predefiniti). Un esempio semplice ha il seguente aspetto:

```css
p {
  font-family: Arial;
}
```

Questo farebbe sì che tutti i paragrafi di una pagina adottino il font Arial, che si trova su qualsiasi computer.

> [!NOTE]
> La scrim [Caratteri sicuri per il web](https://scrimba.com/learn-html-and-css-c0p/~01r?via=mdn) <sup>[_Partner educativo MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba offre una guida interattiva sul perché i font sono importanti, i font sicuri per il web e come specificare i font in CSS — insieme a una sfida per testare le sue conoscenze.

#### Font sicuri per il web

Parlando di disponibilità dei font, ci sono solo un certo numero di font che sono generalmente disponibili su tutti i sistemi e possono quindi essere utilizzati senza molte preoccupazioni. Questi sono i cosiddetti **font sicuri per il web**.

La maggior parte delle volte, come sviluppatori web, vogliamo avere un controllo più specifico sui font usati per visualizzare il nostro contenuto testuale. Il problema è trovare un modo per sapere quale font è disponibile sul computer utilizzato per vedere le nostre pagine web. Non c'è modo di saperlo in ogni caso, ma i font sicuri per il web sono noti per essere disponibili nella maggior parte delle istanze dei sistemi operativi più usati (Windows, macOS, le distribuzioni Linux più comuni, Android e iOS).

L'elenco dei font realmente sicuri per il web cambierà man mano che i sistemi operativi evolvono, ma è ragionevole considerare i seguenti font sicuri per il web, almeno per ora (molti di essi sono stati resi popolari grazie all'iniziativa Microsoft _[Core fonts for the Web](https://en.wikipedia.org/wiki/Core_fonts_for_the_Web)_ tra la fine degli anni '90 e l'inizio degli anni 2000):

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
        È spesso considerata una buona pratica aggiungere anche <em>Helvetica</em> come
        alternativa preferita ad <em>Arial</em> poiché, sebbene i loro stili di carattere
        siano quasi identici, <em>Helvetica</em> è considerata avere una forma più elegante,
        anche se <em>Arial</em> è più ampiamente disponibile.
      </td>
    </tr>
    <tr>
      <td>Courier New</td>
      <td>monospace</td>
      <td>
        Alcuni OS hanno una versione alternativa (forse più vecchia) del 
        font <em>Courier New</em> chiamata <em>Courier</em>. È
        considerata una buona pratica usare entrambi con
        <em>Courier New</em> come alternativa preferita.
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
        Alcuni OS hanno una versione alternativa (forse più vecchia) del
        font <em>Times New Roman</em> chiamata <em>Times</em>. È considerata
        una buona pratica usare entrambi con <em>Times New Roman</em> come alternativa
        preferita.
      </td>
    </tr>
    <tr>
      <td>Trebuchet MS</td>
      <td>sans-serif</td>
      <td>
        Dovrebbe fare attenzione nell'usare questo font — non è ampiamente disponibile
        sui sistemi operativi mobili.
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
> Tra le varie risorse, il sito [cssfontstack.com](https://www.cssfontstack.com/) mantiene un elenco di font sicuri per il web disponibili sui sistemi operativi Windows e macOS, che può aiutarti a prendere decisioni su ciò che consideri sicuro per il tuo uso.

> [!NOTE]
> C'è un modo per scaricare un font personalizzato insieme a una pagina web, per permettere di personalizzare l'uso del font in qualsiasi modo desiderato: **web font**. Questo è un po' più complesso e ne parleremo in un [articolo separato](/it/docs/Learn_web_development/Core/Text_styling/Web_fonts) più avanti nel modulo.

#### Font predefiniti

CSS definisce cinque nomi generici per i font: `serif`, `sans-serif`, `monospace`, `cursive` e `fantasy`. Questi sono molto generici e il tipo di carattere esatto usato da questi nomi generici può variare tra ogni browser e ogni sistema operativo su cui viene visualizzato. Rappresenta uno scenario _peggiore_ in cui il browser farà del suo meglio per fornire un font che sembri appropriato. `serif`, `sans-serif` e `monospace` sono abbastanza prevedibili e dovrebbero fornire qualcosa di ragionevole. D'altra parte, `cursive` e `fantasy` sono meno prevedibili e si consiglia di usarli con molta attenzione, testandoli mentre si prosegue.

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
        Font che hanno grazie (le decorazioni e altri piccoli dettagli che si vedono
        alle estremità dei tratti in alcuni tipi di carattere).
      </td>
      <td id="serif-example">
        <pre class="brush: html hidden">Il mio grande elefante rosso</pre>
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
      <td>Font che non hanno grazie.</td>
      <td id="sans-serif-example">
        <pre class="brush: html hidden">Il mio grande elefante rosso</pre>
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
        Font in cui ogni carattere ha la stessa larghezza, tipicamente usati negli elenchi di codice.
      </td>
      <td id="monospace-example">
        <pre class="brush: html hidden">Il mio grande elefante rosso</pre>
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
        Font che sono intesi per emulare la scrittura a mano, con tratti fluidi e connessi.
      </td>
      <td id="cursive-example">
        <pre class="brush: html hidden">Il mio grande elefante rosso</pre>
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
      <td>Font che sono intesi per essere decorativi.</td>
      <td id="fantasy-example">
        <pre class="brush: html hidden">Il mio grande elefante rosso</pre>
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

#### Stack di font

Poiché non si può garantire la disponibilità dei font che si desidera utilizzare sulle proprie pagine web (anche un font web _potrebbe_ fallire per qualche motivo), è possibile fornire uno **stack di font** in modo che il browser abbia più font da scegliere. Questo comporta un valore `font-family` composto da nomi di font multipli separati da virgole, ad esempio,

```css
p {
  font-family: "Trebuchet MS", Verdana, sans-serif;
}
```

In tal caso, il browser inizia dall'inizio dell'elenco e guarda se quel font è disponibile sulla macchina. Se lo è, lo applica agli elementi selezionati. In caso contrario, passa al font successivo, e così via.

È una buona idea fornire un nome generico di font adeguato alla fine dello stack in modo che, se nessuno dei font elencati è disponibile, il browser possa almeno fornire qualcosa di approssimativamente adatto. Per sottolineare questo punto, i paragrafi ricevono il font serif predefinito del browser se nessuna altra opzione è disponibile — che di solito è Times New Roman — questo non va bene per un font sans-serif!

> [!NOTE]
> Mentre è possibile utilizzare nomi di famiglie di font che contengono uno spazio, come `Trebuchet MS`, senza quotare il nome, per evitare errori di escape, si consiglia di quotare i nomi delle famiglie di font che contengono spazi bianchi, cifre o caratteri di punteggiatura diversi dai trattini.

> [!WARNING]
> Qualsiasi nome di famiglia di font che potrebbe essere interpretato erroneamente come un nome di famiglia generico o una parola chiave CSS-ampio deve essere quotato. Mentre i nomi delle famiglie di font possono essere inclusi come un {{cssxref("custom-ident")}} o una {{cssxref("string")}}, i nomi delle famiglie di font che accadono di essere gli stessi di un valore di proprietà CSS-ampio, come `initial`, o `inherit`, o CSS hanno lo stesso nome di uno dei nomi delle famiglie di font generiche, come `sans-serif` o `fantasy`, devono essere inclusi come stringa quotata. Altrimenti, il nome della famiglia di font sarà interpretato come se fosse la parola chiave CSS o il nome della famiglia generica equivalenti. Quando usati come parole chiave, i nomi delle famiglie di font generiche — `serif`, `sans-serif`, `monospace`, `cursive` e `fantasy` — e le parole chiave CSS globali NON devono essere quotati, poiché le stringhe non sono interpretate come parole chiave CSS.

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

Nell'articolo del nostro modulo precedente sui [valori e unità CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units), abbiamo esaminato le unità di lunghezza e dimensione. La dimensione del font (impostata con la proprietà {{cssxref("font-size")}}) può prendere valori misurati nella maggior parte di queste unità (e altre, come le [percentuali](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#percentages)); tuttavia, le unità più comuni che utilizzerai per dimensionare il testo sono:

- `px` (pixel): il numero di pixel di altezza che si desidera avere per il testo. Questa è un'unità assoluta — risulta nello stesso valore finale calcolato per il font sulla pagina in quasi tutte le situazioni.
- `em`: 1 `em` è uguale alla dimensione del font impostata sull'elemento genitore dell'elemento attuale che stiamo stilizzando (più precisamente, la larghezza di una lettera M maiuscola contenuta all'interno dell'elemento genitore). Questo può diventare complicato da calcolare se hai molti elementi nidificati con diverse dimensioni di font impostate, ma è fattibile, come vedrai di seguito. Perché preoccuparsi? È abbastanza naturale una volta che ci si abitua, e si può usare `em` per dimensionare tutto, non solo il testo. Si può avere un intero sito web dimensionato usando `em`, il che facilita la manutenzione.
- `rem`: funzionano proprio come `em`, tranne che 1 `rem` è uguale alla dimensione del font impostata sull'elemento radice del documento (cioè, {{htmlelement("html")}}), non sull'elemento genitore. Questo rende molto più facile fare i calcoli per determinare le dimensioni dei tuoi font.

Il `font-size` di un elemento viene ereditato dall'elemento genitore di quell'elemento. Tutto ciò inizia con l'elemento radice dell'intero documento — {{htmlelement("html")}} — la dimensione standard del quale è impostata a `16px` nei browser. Qualsiasi paragrafo (o altro elemento che non ha una dimensione diversa impostata dal browser) all'interno dell'elemento radice avrà una dimensione finale di `16px`. Altri elementi possono avere dimensioni predefinite diverse. Ad esempio, un elemento {{htmlelement("Heading_Elements", "h1")}} ha una dimensione di `2em` impostata di default, quindi avrà una dimensione finale di `32px`.

Le cose diventano più complicate quando si inizia a modificare la dimensione del font di elementi nidificati. Ad esempio, se ha un elemento {{htmlelement("article")}} nella sua pagina e imposta la sua `font-size` a 1.5 `em` (che si tradurrebbe in una dimensione finale di 24 `px`), e poi desidera che i paragrafi all'interno degli elementi `<article>` abbiano una dimensione di font calcolata di 20 `px`, quale valore di `em` dovrebbe utilizzare?

```html
<!-- document base font-size is 16px -->
<article>
  <!-- If my font-size is 1.5em -->
  <p>My paragraph</p>
  <!-- How do I compute to 20px font-size? -->
</article>
```

Dovrebbe impostare il suo valore `em` a 20/24, o 0.83333333 `em`. I calcoli possono risultare complicati, quindi deve prestare attenzione a come stila le cose. È meglio utilizzare `rem` dove possibile per mantenere le cose semplici e evitare di impostare la `font-size` degli elementi contenitore dove possibile.

### Font style, font weight, text transform, e text decoration

CSS fornisce quattro proprietà comuni per alterare il peso/emphasis visivo del testo:

- {{cssxref("font-style")}}: Usato per attivare o disattivare il testo in corsivo. I valori possibili sono i seguenti (raramente userà questo, a meno che non intenda disattivare qualche formattazione in corsivo per qualche motivo):

  - `normal`: Imposta il testo sul font normale (disattiva il corsivo esistente).
  - `italic`: Imposta il testo per utilizzare la versione corsiva del font, se disponibile; in caso contrario, simulerà il corsivo con l'obliquo.
  - `oblique`: Imposta il testo per utilizzare una versione simulata di un font corsivo, creata inclinando la versione normale.

- {{cssxref("font-weight")}}: Imposta quanto è grassetto il testo. Questo ha molti valori disponibili nel caso abbia molte varianti di font disponibili (come _-light_, _-normal_, _-bold_, _-extrabold_, _-black_, ecc.), ma realisticamente userà raramente qualsiasi di essi tranne `normal` e `bold`:

  - `normal`, `bold`: Peso del font normale e grassetto.
  - `lighter`, `bolder`: Imposta la pesantezza dell'elemento corrente per essere un passo più chiaro o più pesante rispetto alla pesantezza dell'elemento genitore.
  - `100` – `900`: Valori numerici di pesantezza che forniscono un controllo più raffinato rispetto alle parole chiave sopra, se necessario.

- {{cssxref("text-transform")}}: Permette di impostare la trasformazione del font. I valori includono:

  - `none`: Impedisce qualsiasi trasformazione.
  - `uppercase`: Trasforma tutto il testo in maiuscolo.
  - `lowercase`: Trasforma tutto il testo in minuscolo.
  - `capitalize`: Trasforma tutte le parole in modo che abbiano la prima lettera in maiuscolo.
  - `full-width`: Trasforma tutti i glifi per essere scritti all'interno di un quadrato a larghezza fissa, simile a un font monospace, permettendo di allineare, ad esempio, i caratteri latini insieme ai glifi delle lingue asiatiche (come cinese, giapponese, coreano).

- {{cssxref("text-decoration")}}: Imposta/rimuove decorazioni del testo sui font (utilizzerà principalmente questo per rimuovere la sottolineatura predefinita sui link quando li stila). I valori disponibili sono:

  - `none`: Rimuove qualsiasi decorazione del testo già presente.
  - `underline`: Sottolinea il testo.
  - `overline`: Aggiunge una linea sopra il testo.
  - `line-through`: Traccia una linea sopra il testo.

  Notare che {{cssxref("text-decoration")}} può accettare valori multipli contemporaneamente se desidera aggiungere più decorazioni simultaneamente, ad esempio, `text-decoration: underline overline`. Notare inoltre che {{cssxref("text-decoration")}} è una proprietà abbreviata per {{cssxref("text-decoration-line")}}, {{cssxref("text-decoration-style")}}, e {{cssxref("text-decoration-color")}}. Può usare combinazioni di questi valori di proprietà per creare effetti interessanti, ad esempio: `text-decoration: line-through red wavy`.

Vediamo di aggiungere un paio di queste proprietà al nostro esempio:

Il nostro nuovo risultato è come segue:

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

### Ombre cadenti per il testo

Può applicare ombre cadenti al suo testo usando la proprietà {{cssxref("text-shadow")}}. Questa prende fino a quattro valori, come mostrato nell'esempio seguente:

```css
text-shadow: 4px 4px 5px red;
```

Le quattro proprietà sono le seguenti:

1. L'offeset orizzontale dell'ombra dal testo originale — questo può prendere la maggior parte delle disponibili [unità di lunghezza e dimensione CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths), ma userete più comunemente `px`; i valori positivi spostano l'ombra a destra, e i valori negativi a sinistra. Questo valore deve essere incluso.
2. L'offset verticale dell'ombra dal testo originale. Questo si comporta in modo simile all'offset orizzontale, tranne che sposta l'ombra su/giù, non a sinistra/destra. Questo valore deve essere incluso.
3. Il raggio di sfocatura: un valore più alto significa che l'ombra è dispersa più ampiamente. Se questo valore non è incluso, predefinisce a 0, il che significa nessuna sfocatura. Questo può prendere la maggior parte delle disponibili [unità di lunghezza e dimensione CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths).
4. Il colore di base dell'ombra, che può prendere qualsiasi [unità di colore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color). Se non incluso, predefinisce a [`currentcolor`](/it/docs/Web/CSS/color_value#currentcolor_keyword), cioè il colore dell'ombra è preso dalla proprietà [`color`](/it/docs/Web/CSS/color) dell'elemento.

#### Ombre multiple

Può applicare multiple ombre allo stesso testo includendo multiple valori di ombre separati da virgole, ad esempio:

```css
h1 {
  text-shadow:
    1px 1px 1px red,
    2px 2px 1px red;
}
```

Se applicassimo questo all'elemento {{htmlelement("Heading_Elements", "h1")}} nel nostro esempio Tommy The Cat, finiremmo con questo:

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
> Può vedere esempi più interessanti di utilizzo di `text-shadow` nell'articolo di Sitepoint [Moonlighting with CSS text-shadow](https://www.sitepoint.com/moonlighting-css-text-shadow/).

## Layout del testo

Con le proprietà base del font fuori dai piedi, diamo un'occhiata alle proprietà che possiamo utilizzare per influenzare il layout del testo.

### Allineamento del testo

La proprietà {{cssxref("text-align")}} viene utilizzata per controllare come il testo è allineato all'interno della sua content box contenente. I valori disponibili sono elencati di seguito. Essi funzionano praticamente nello stesso modo in cui fanno in un'applicazione di elaboratore di testi regolare:

- `left`: Giustifica il testo a sinistra.
- `right`: Giustifica il testo a destra.
- `center`: Centra il testo.
- `justify`: Fa sì che il testo si espanda, variando gli spazi tra le parole in modo che tutte le righe di testo abbiano la stessa larghezza. Deve utilizzare questo con attenzione — può sembrare terribile, specialmente quando applicato a un paragrafo con molte parole lunghe. Se intende usare questo, dovrebbe anche pensare a usare qualcosa d'altro insieme ad esso, come {{cssxref("hyphens")}}, per spezzare alcune delle parole più lunghe tra le righe.

Se applicassimo `text-align: center;` all'elemento {{htmlelement("Heading_Elements", "h1")}} nel nostro esempio, avremmo questo:

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

La proprietà {{cssxref("line-height")}} imposta l'altezza di ciascuna riga di testo. Questa proprietà non solo può prendere la maggior parte delle [unità di lunghezza e dimensione](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths), ma può anche prendere un valore senza unità, che agisce come moltiplicatore ed è generalmente considerato l'opzione migliore. Con un valore senza unità, la {{cssxref("font-size")}} viene moltiplicata e risulta nell'`line-height`. Il testo del corpo generalmente sembra più bello ed è più facile da leggere quando le righe sono distanziate. L'altezza raccomandata delle linee è intorno a 1.5 – 2 (doppio spazio). Per impostare le nostre righe di testo a 1.6 volte l'altezza del font, useremmo:

```css
p {
  line-height: 1.6;
}
```

Applicando questo agli elementi {{htmlelement("p")}} nel nostro esempio ci darebbe questo risultato:

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

Le proprietà {{cssxref("letter-spacing")}} e {{cssxref("word-spacing")}} permettono di impostare la spaziatura tra lettere e parole nel testo. Non usiamo queste molto spesso, ma potremmo trovare un utilizzo per ottenere un aspetto specifico o per migliorare la leggibilità di un font particolarmente denso. Possono prendere la maggior parte delle [unità di lunghezza](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths).

Per illustrare, potremmo applicare una certa spaziatura tra parole e lettere alla prima riga di ciascun elemento {{htmlelement("p")}} nel nostro esempio HTML con:

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

### Altre proprietà degne di attenzione

Le proprietà sopra forniscono un'idea su come iniziare a stilizzare il testo su una pagina web, ma ci sono molte altre proprietà che potrebbe usare. Volevamo solo coprire le più importanti qui. Una volta che ha preso dimestichezza nell'usare quelle sopra, dovrebbe anche esplorare le seguenti:

Stili dei font:

- {{cssxref("font-variant")}}: Cambia tra alternative di font in maiuscolo e font normale.
- {{cssxref("font-kerning")}}: Attiva o disattiva le opzioni di kerning del font.
- {{cssxref("font-feature-settings")}}: Attiva o disattiva varie funzionalità del font [OpenType](https://en.wikipedia.org/wiki/OpenType).
- {{cssxref("font-variant-alternates")}}: Controlla l'uso di glifi alternativi per una data faccia del font.
- {{cssxref("font-variant-caps")}}: Controlla l'uso di glifi alternativi per maiuscole.
- {{cssxref("font-variant-east-asian")}}: Controlla l'uso di glifi alternativi per script dell'Asia orientale, come giapponese e cinese.
- {{cssxref("font-variant-ligatures")}}: Controlla quali legature e forme contestuali vengono usate nel testo.
- {{cssxref("font-variant-numeric")}}: Controlla l'uso di glifi alternativi per numeri, frazioni e marcatori ordinali.
- {{cssxref("font-variant-position")}}: Controlla l'uso di glifi alternativi di dimensioni minori posizionati come apici o pedici.
- {{cssxref("font-size-adjust")}}: Regola la dimensione visiva del font indipendentemente dalla sua effettiva dimensione del font.
- {{cssxref("font-stretch")}}: Cambia tra possibili versioni allungate alternative di un dato font.
- {{cssxref("text-underline-position")}}: Specifica la posizione delle sottolineature impostate usando il valore `underline` della proprietà `text-decoration-line`.
- {{cssxref("text-rendering")}}: Tenta di eseguire alcuni miglioramenti al rendering del testo.

Stili del layout del testo:

- {{cssxref("text-indent")}}: Specifica quanto spazio orizzontale deve essere lasciato prima dell'inizio della prima riga del contenuto testuale.
- {{cssxref("text-overflow")}}: Definisce come viene segnalato agli utenti il contenuto ricoperto non visualizzato.
- {{cssxref("white-space")}}: Definisce come vengono gestiti gli spazi bianchi e le interruzioni di linea associate all'interno dell'elemento.
- {{cssxref("word-break")}}: Specifica se spezzare le linee all'interno delle parole.
- {{cssxref("direction")}}: Definisce la direzione del testo. (Questo dipende dalla lingua e di solito è meglio lasciare che HTML gestisca quella parte in quanto è legato al contenuto testuale.)
- {{cssxref("hyphens")}}: Attiva o disattiva l'ortografia per le lingue supportate.
- {{cssxref("line-break")}}: Rilassa o rafforza l'interruzione di linee per le lingue asiatiche.
- {{cssxref("text-align-last")}}: Definisce come è allineata l'ultima linea di un blocco o una linea, appena prima di un'interruzione di linea forzata.
- {{cssxref("text-orientation")}}: Definisce l'orientamento del testo in una linea.
- {{cssxref("overflow-wrap")}}: Specifica se il browser può o meno spezzare le linee all'interno delle parole per prevenire l'overflow.
- {{cssxref("writing-mode")}}: Definisce se le linee di testo sono disposte orizzontalmente o verticalmente e la direzione in cui fluiscono le linee successive.

## Abbreviazione dei font

Molte proprietà dei font possono anche essere impostate tramite la proprietà abbreviata {{cssxref("font")}}. Queste sono scritte nell'ordine seguente: {{cssxref("font-style")}}, {{cssxref("font-variant")}}, {{cssxref("font-weight")}}, {{cssxref("font-stretch")}}, {{cssxref("font-size")}}, {{cssxref("line-height")}}, e {{cssxref("font-family")}}.

Tra tutte queste proprietà, solo `font-size` e `font-family` sono richieste quando si utilizza la proprietà abbreviata `font`.

Una barra obliqua deve essere inserita tra le proprietà {{cssxref("font-size")}} e {{cssxref("line-height")}}.

Un esempio completo avrebbe questo aspetto:

```css
font:
  italic normal bold normal 3em/1.5 Helvetica,
  Arial,
  sans-serif;
```

## Apprendimento attivo: Giocare con lo stile del testo

In questa sessione di apprendimento attivo non abbiamo esercizi specifici per lei. Vorremmo solo che si divertisse a giocare con alcune proprietà di stile del testo/layout. Veda lei stesso cosa può inventare! Può fare questo utilizzando file HTML/CSS offline, o inserire il suo codice nell'esempio modificabile dal vivo qui sotto.

Se commette un errore, può sempre reimpostarlo usando il pulsante _Reset_.

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

## Riepilogo

Speriamo si sia divertito a giocare con il testo in questo articolo! Il prossimo articolo fornirà tutto ciò che le serve sapere sullo stile delle liste HTML.

## Vedi anche

- [Tutto sulla proprietà CSS font-family](https://explainers.dev/font-family/), explainers.dev
- [Font sicuri per il web](https://scrimba.com/the-frontend-developer-career-path-c0j/~02b?via=mdn), Scrimba <sup>[_Partner educativo MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>

{{NextMenu("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling")}}
