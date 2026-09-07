---
title: Stile fondamentale di testo e font
short-title: Fondamenti di testo e font
slug: Learn_web_development/Core/Text_styling/Fundamentals
l10n:
  sourceCommit: 87adaa5384b1015690f3435ce0ba64ac097764eb
---

{{NextMenu("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling")}}

In questo articolo si inizierà il percorso verso la padronanza dello stile del testo con {{Glossary("CSS", "CSS")}}. Verranno esaminati in dettaglio tutti i fondamenti di base dello stile di testo/font, inclusa l'impostazione dello spessore, della famiglia e dello stile del font, la scorciatoia per i font, l'allineamento del testo e altri effetti, nonché la spaziatura tra righe e lettere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere i concetti di famiglie di font, stack di font e font sicuri per il web.</li>
          <li>Impostare il colore, lo spessore, la dimensione e lo stile del font.</li>
          <li>Impostare l'allineamento, la trasformazione e la decorazione del testo.</li>
          <li>Impostare l'altezza della riga.</li>
          <li>Sapere che esistono diverse altre proprietà per lo stile di font e testo ed essere incoraggiati a esplorarle.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa comporta lo stile del testo in CSS?

Il testo all'interno di un elemento viene disposto all'interno del [content box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#parts_of_a_box) dell'elemento. Inizia in alto a sinistra dell'area del contenuto (oppure in alto a destra, nel caso di contenuti in linguaggi RTL) e scorre verso la fine della riga. Una volta raggiunta la fine, prosegue alla riga successiva e scorre nuovamente fino alla fine. Questo schema si ripete finché tutto il contenuto non è stato inserito nel box. Il contenuto testuale si comporta di fatto come una serie di elementi inline: viene disposto su righe adiacenti tra loro e non crea interruzioni di riga finché non viene raggiunta la fine della riga, a meno che non venga forzata manualmente un'interruzione di riga usando l'elemento {{htmlelement("br")}}.

> [!NOTE]
> Se il paragrafo precedente risulta poco chiaro, non importa: tornare indietro e ripassare il nostro articolo sul [Box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) per rinfrescare la teoria del box model prima di continuare.

Le proprietà CSS usate per assegnare uno stile al testo rientrano generalmente in due categorie, che verranno esaminate separatamente in questo articolo:

- **Stili del font**: proprietà che influenzano il font di un testo, ad esempio quale font viene applicato, la sua dimensione e se è in grassetto, corsivo e così via.
- **Stili di layout del testo**: proprietà che influenzano la spaziatura e altre caratteristiche di layout del testo, consentendo di manipolare, ad esempio, lo spazio tra righe e lettere e il modo in cui il testo viene allineato nel content box.

> [!NOTE]
> Tenere presente che tutto il testo all'interno di un elemento viene influenzato come un'unica entità. Non è possibile selezionare e applicare stili a sottosezioni del testo, a meno di racchiuderle in un elemento appropriato, ad esempio {{htmlelement("span")}} o {{htmlelement("strong")}}, oppure di usare uno pseudo-elemento specifico per il testo come {{cssxref("::first-letter")}} (seleziona la prima lettera del testo di un elemento), {{cssxref("::first-line")}} (seleziona la prima riga del testo di un elemento) o {{cssxref("::selection")}} (seleziona il testo attualmente evidenziato dal cursore).

## Font

Passiamo direttamente alle proprietà per lo stile dei font. In questo esempio, verranno applicate alcune proprietà CSS al seguente esempio HTML:

```html live-sample___0unstyled live-sample___1color live-sample___2fonts live-sample___3font-style live-sample___4shadows live-sample___5text-align live-sample___6line-height live-sample___7letter-word-spacing
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

### Colore

La proprietà {{cssxref("color")}} imposta il colore del contenuto in primo piano degli elementi selezionati, che solitamente è il testo, ma può includere anche altri elementi, ad esempio una sottolineatura o una linea sopra il testo applicata usando la proprietà {{cssxref("text-decoration")}}.

`color` può accettare qualsiasi [unità di colore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color), ad esempio:

```css live-sample___1color live-sample___2fonts live-sample___3font-style live-sample___4shadows live-sample___5text-align live-sample___6line-height live-sample___7letter-word-spacing
p {
  color: red;
}
```

Questo farà diventare rossi i paragrafi, anziché usare il nero predefinito standard del browser, come segue:

{{ EmbedLiveSample('1color', '100%', 230) }}

### Famiglie di font

Per impostare un font diverso per il testo, si usa la proprietà {{cssxref("font-family")}}: consente di specificare un font, o un elenco di font, che il browser deve applicare agli elementi selezionati. Il browser applicherà un font solo se è disponibile nel computer da cui si accede al sito web; altrimenti, userà semplicemente un [font predefinito](#font_predefiniti) del browser. Un esempio semplice è il seguente:

```css
p {
  font-family: "Arial";
}
```

Questo farebbe adottare a tutti i paragrafi di una pagina il font Arial, disponibile su qualsiasi computer.

> [!NOTE]
> Lo scrim [Web-safe fonts](https://scrimba.com/learn-html-and-css-c0p/~01r?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una guida interattiva sul motivo per cui i font sono importanti, sui font sicuri per il web e su come specificare i font in CSS, insieme a una sfida per verificare le proprie conoscenze.

#### Font sicuri per il web

Parlando di disponibilità dei font, esiste solo un certo numero di font generalmente disponibili su tutti i sistemi e che possono quindi essere usati senza troppe preoccupazioni. Questi sono i cosiddetti **font sicuri per il web**.

Nella maggior parte dei casi, come sviluppatori web si desidera avere un controllo più specifico sui font usati per visualizzare il contenuto testuale. Il problema è trovare un modo per sapere quale font sia disponibile sul computer usato per visualizzare le pagine web. Non è possibile saperlo in tutti i casi, ma i font sicuri per il web sono noti per essere disponibili in quasi tutte le installazioni dei sistemi operativi più usati (Windows, macOS, le distribuzioni Linux più comuni, Android e iOS).

L'elenco effettivo dei font sicuri per il web cambierà con l'evoluzione dei sistemi operativi, ma è ragionevole considerare sicuri per il web i seguenti font, almeno per il momento (molti di essi sono stati resi popolari grazie all'iniziativa Microsoft _[Core fonts for the Web](https://en.wikipedia.org/wiki/Core_fonts_for_the_Web)_ alla fine degli anni Novanta e nei primi anni Duemila):

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
        Spesso è considerata una buona pratica aggiungere anche <em>Helvetica</em> come
        alternativa preferita a <em>Arial</em> poiché, sebbene i loro caratteri siano
        quasi identici, si ritiene che <em>Helvetica</em> abbia una forma più gradevole,
        anche se <em>Arial</em> è disponibile più diffusamente.
      </td>
    </tr>
    <tr>
      <td>Courier New</td>
      <td>monospace</td>
      <td>
        Alcuni sistemi operativi hanno una versione alternativa, probabilmente più vecchia,
        del font <em>Courier New</em>, chiamata <em>Courier</em>. È considerata una buona
        pratica usare entrambi, con <em>Courier New</em> come alternativa preferita.
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
        Alcuni sistemi operativi hanno una versione alternativa, probabilmente più vecchia,
        del font <em>Times New Roman</em>, chiamata <em>Times</em>. È considerata una buona
        pratica usare entrambi, con <em>Times New Roman</em> come alternativa preferita.
      </td>
    </tr>
    <tr>
      <td>Trebuchet MS</td>
      <td>sans-serif</td>
      <td>
        Prestare attenzione nell'usare questo font: non è molto disponibile sui sistemi
        operativi mobili.
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
> Tra le varie risorse, il sito web [cssfontstack.com](https://www.cssfontstack.com/) mantiene un elenco di font sicuri per il web disponibili sui sistemi operativi Windows e macOS, che può aiutare a decidere quali font siano sicuri per il proprio utilizzo.

> [!NOTE]
> Esiste un modo per scaricare un font personalizzato insieme a una pagina web, così da poter personalizzare l'uso dei font come desiderato: i **web font**. È un argomento un po' più complesso, che verrà affrontato più avanti nel modulo in un [articolo separato](/it/docs/Learn_web_development/Core/Text_styling/Web_fonts).

#### Font predefiniti

CSS definisce cinque nomi generici per i font: `serif`, `sans-serif`, `monospace`, `cursive` e `fantasy`. Sono nomi molto generici e il carattere esatto usato da questi nomi generici può variare tra browser e sistemi operativi su cui vengono visualizzati. Rappresentano uno _scenario peggiore_ in cui il browser cercherà di fornire il font dall'aspetto più appropriato possibile. `serif`, `sans-serif` e `monospace` sono abbastanza prevedibili e dovrebbero fornire un risultato ragionevole. `cursive` e `fantasy`, invece, sono meno prevedibili e se ne raccomanda un uso molto attento, con test durante il lavoro.

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
        Font con grazie (gli abbellimenti e altri piccoli dettagli visibili alle
        estremità dei tratti in alcuni caratteri tipografici).
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
      <td>Font senza grazie.</td>
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
        Font in cui ogni carattere ha la stessa larghezza, tipicamente usati negli
        elenchi di codice.
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
        Font pensati per emulare la scrittura a mano, con tratti fluidi e collegati.
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
      <td>Font pensati per essere decorativi.</td>
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

#### Stack di font

Poiché non è possibile garantire la disponibilità dei font che si desidera usare nelle pagine web, anche un web font _potrebbe_ non funzionare per qualche motivo, è possibile fornire uno **stack di font** affinché il browser disponga di più font tra cui scegliere. Ciò implica un valore `font-family` composto da più nomi di font separati da virgole, ad esempio:

```css
p {
  font-family: "Trebuchet MS", "Verdana", sans-serif;
}
```

In questo caso, il browser parte dall'inizio dell'elenco e verifica se il font è disponibile nel computer. Se lo è, applica quel font agli elementi selezionati. In caso contrario, passa al font successivo e così via.

È una buona idea fornire un nome di font generico adatto alla fine dello stack, in modo che, se nessuno dei font elencati è disponibile, il browser possa almeno fornire qualcosa di approssimativamente adatto. Per sottolineare questo punto, ai paragrafi viene assegnato il font serif predefinito del browser se nessun'altra opzione è disponibile, che solitamente è Times New Roman: non è adatto a un font sans-serif.

> [!NOTE]
> Sebbene sia possibile usare nomi di famiglie di font che contengono uno spazio, come `Trebuchet MS`, senza racchiudere il nome tra virgolette, per evitare errori nell'escape è consigliabile racchiudere tra virgolette i nomi delle famiglie di font che contengono spazi vuoti, cifre o caratteri di punteggiatura diversi dai trattini.

> [!WARNING]
> Qualsiasi nome di famiglia di font che potrebbe essere interpretato erroneamente come nome di famiglia generico o keyword CSS globale deve essere racchiuso tra virgolette. Sebbene i nomi di `font-family` possano essere inclusi come {{cssxref("custom-ident")}} o {{cssxref("string")}}, i nomi delle famiglie di font che coincidono con un valore di proprietà CSS globale, come `initial` o `inherit`, oppure che hanno lo stesso nome di una delle famiglie di font generiche, come `sans-serif` o `fantasy`, devono essere inclusi come stringhe tra virgolette. In caso contrario, il nome della famiglia di font verrà interpretato come keyword CSS equivalente o nome di famiglia generico. Se usati come keyword, i nomi delle famiglie di font generiche — `serif`, `sans-serif`, `monospace`, `cursive` e `fantasy` — e le keyword CSS globali NON DEVONO essere racchiusi tra virgolette, poiché le stringhe non vengono interpretate come keyword CSS.

#### Un esempio di font-family

Aggiungiamo al precedente esempio un font sans-serif per i paragrafi:

```css live-sample___2fonts live-sample___3font-style live-sample___4shadows live-sample___5text-align live-sample___6line-height live-sample___7letter-word-spacing
p {
  color: red;
  font-family: "Helvetica", "Arial", sans-serif;
}
```

Questo fornisce il seguente risultato:

{{ EmbedLiveSample('2fonts', '100%', 220) }}

### Dimensione del font

Nell'articolo del modulo precedente su [valori e unità CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units), sono state esaminate le unità di lunghezza e dimensione. La dimensione del font, impostata con la proprietà {{cssxref("font-size")}}, può accettare valori misurati nella maggior parte di queste unità, e in altre, come le [percentuali](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#percentages); tuttavia, le unità più comuni da usare per dimensionare il testo sono:

- `px` (pixel): il numero di pixel di altezza desiderato per il testo. È un'unità assoluta: produce lo stesso valore calcolato finale per il font nella pagina praticamente in ogni situazione.
- `em`: 1 `em` equivale alla dimensione del font impostata sull'elemento padre dell'elemento corrente a cui viene applicato lo stile; più precisamente, alla larghezza della lettera maiuscola M contenuta nell'elemento padre. Questo può diventare difficile da calcolare se sono presenti molti elementi annidati con dimensioni del font diverse, ma è possibile, come si vedrà più avanti. Perché usarle? Una volta acquisita familiarità, è piuttosto naturale e `em` può essere usato per dimensionare tutto, non solo il testo. È possibile dimensionare un intero sito web usando `em`, rendendo semplice la manutenzione.
- `rem`: funzionano come `em`, tranne per il fatto che 1 `rem` equivale alla dimensione del font impostata sull'elemento radice del documento, ovvero {{htmlelement("html")}}, e non sull'elemento padre. Questo rende molto più semplice il calcolo delle dimensioni del font.

Il `font-size` di un elemento viene ereditato dall'elemento padre. Tutto inizia con l'elemento radice dell'intero documento, {{htmlelement("html")}}, il cui `font-size` standard è impostato a `16px` nei browser. Qualsiasi paragrafo, o altro elemento per cui il browser non imposta una dimensione diversa, all'interno dell'elemento radice avrà una dimensione finale di `16px`. Altri elementi possono avere dimensioni predefinite diverse. Ad esempio, un elemento {{htmlelement("Heading_Elements", "h1")}} ha una dimensione predefinita di `2em`, quindi avrà una dimensione finale di `32px`.

Le cose diventano più complesse quando si inizia ad alterare la dimensione del font di elementi annidati. Ad esempio, se nella pagina fosse presente un elemento {{htmlelement("article")}} e il suo `font-size` fosse impostato a 1.5 `em`, valore che verrebbe calcolato come dimensione finale di 24 `px`, e si volesse che i paragrafi all'interno degli elementi `<article>` avessero una dimensione del font calcolata di 20 `px`, quale valore `em` si dovrebbe usare?

```html
<!-- document base font-size is 16px -->
<article>
  <!-- If my font-size is 1.5em -->
  <p>My paragraph</p>
  <!-- How do I compute to 20px font-size? -->
</article>
```

Sarebbe necessario impostare il valore `em` a 20/24, ovvero 0.83333333 `em`. Il calcolo può essere complicato, quindi occorre prestare attenzione a come vengono applicati gli stili. È preferibile usare `rem` quando possibile per mantenere le cose semplici ed evitare, se possibile, di impostare il `font-size` degli elementi contenitore.

### Stile del font, spessore del font, trasformazione del testo e decorazione del testo

CSS fornisce quattro proprietà comuni per modificare il peso visivo/l'enfasi del testo:

- {{cssxref("font-style")}}: usata per attivare o disattivare il testo in corsivo. I valori possibili sono i seguenti; raramente verrà usata, a meno che non si voglia disattivare uno stile corsivo per qualche motivo:
  - `normal`: imposta il testo sul font normale e disattiva il corsivo esistente.
  - `italic`: imposta il testo affinché usi la versione corsiva del font, se disponibile; altrimenti simulerà il corsivo usando `oblique`.
  - `oblique`: imposta il testo affinché usi una versione simulata di un font corsivo, creata inclinando la versione normale.

- {{cssxref("font-weight")}}: imposta quanto il testo è in grassetto. Sono disponibili molti valori nel caso siano disponibili molte varianti del font, come _-light_, _-normal_, _-bold_, _-extrabold_, _-black_ e così via, ma realisticamente verranno usati raramente valori diversi da `normal` e `bold`:
  - `normal`, `bold`: spessore normale e grassetto del font.
  - `lighter`, `bolder`: impostano il grassetto dell'elemento corrente a un livello più leggero o più pesante rispetto al grassetto dell'elemento padre.
  - `100` – `900`: valori numerici di grassetto che, se necessario, forniscono un controllo più preciso rispetto alle keyword precedenti.

- {{cssxref("text-transform")}}: consente di trasformare il font. I valori includono:
  - `none`: impedisce qualsiasi trasformazione.
  - `uppercase`: trasforma tutto il testo in maiuscolo.
  - `lowercase`: trasforma tutto il testo in minuscolo.
  - `capitalize`: trasforma tutte le parole mettendo in maiuscolo la prima lettera.
  - `full-width`: trasforma tutti i glifi affinché vengano scritti all'interno di un quadrato a larghezza fissa, simile a un font monospace, consentendo di allineare, ad esempio, caratteri latini insieme a glifi di lingue asiatiche, come cinese, giapponese e coreano.

- {{cssxref("text-decoration")}}: imposta o annulla le decorazioni del testo sui font; viene usata principalmente per annullare la sottolineatura predefinita dei link durante l'applicazione dello stile. I valori disponibili sono:
  - `none`: annulla qualsiasi decorazione del testo già presente.
  - `underline`: sottolinea il testo.
  - `overline`: aggiunge una linea sopra il testo.
  - `line-through`: aggiunge una barratura sul testo.

  Occorre notare che {{cssxref("text-decoration")}} può accettare più valori contemporaneamente se si desidera aggiungere più decorazioni simultaneamente, ad esempio `text-decoration: underline overline`. Inoltre, {{cssxref("text-decoration")}} è una proprietà shorthand per {{cssxref("text-decoration-line")}}, {{cssxref("text-decoration-style")}} e {{cssxref("text-decoration-color")}}. È possibile usare combinazioni dei valori di queste proprietà per creare effetti interessanti, ad esempio: `text-decoration: line-through red wavy`.

Vediamo come aggiungere alcune di queste proprietà al nostro esempio:

```css live-sample___3font-style live-sample___4shadows live-sample___5text-align live-sample___6line-height live-sample___7letter-word-spacing
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
  font-family: "Helvetica", "Arial", sans-serif;
}
```

Il nuovo risultato è il seguente:

{{ EmbedLiveSample('3font-style', '100%', 260) }}

### Ombre esterne del testo

È possibile applicare ombre esterne al testo usando la proprietà {{cssxref("text-shadow")}}. Questa accetta fino a quattro valori, come mostrato nell'esempio seguente:

```css
text-shadow: 4px 4px 5px red;
```

Le quattro proprietà sono le seguenti:

1. L'offset orizzontale dell'ombra rispetto al testo originale: può usare la maggior parte delle [unità di lunghezza e dimensione CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths) disponibili, ma verranno usati più comunemente i `px`; i valori positivi spostano l'ombra a destra, quelli negativi a sinistra. Questo valore deve essere incluso.
2. L'offset verticale dell'ombra rispetto al testo originale. Si comporta in modo simile all'offset orizzontale, tranne per il fatto che sposta l'ombra verso l'alto o verso il basso, anziché a sinistra o a destra. Questo valore deve essere incluso.
3. Il raggio di sfocatura: un valore più alto significa che l'ombra viene diffusa più ampiamente. Se questo valore non viene incluso, il valore predefinito è 0, ovvero nessuna sfocatura. Può usare la maggior parte delle [unità di lunghezza e dimensione CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths) disponibili.
4. Il colore di base dell'ombra, che può usare qualsiasi [unità di colore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color). Se non viene incluso, il valore predefinito è [`currentColor`](/it/docs/Web/CSS/Reference/Values/color_value#currentcolor_keyword), cioè il colore dell'ombra viene ricavato dalla proprietà {{cssxref("color")}} dell'elemento.

#### Ombre multiple

È possibile applicare più ombre allo stesso testo includendo più valori di ombra separati da virgole, ad esempio:

```css live-sample___4shadows live-sample___5text-align live-sample___6line-height live-sample___7letter-word-spacing
h1 {
  text-shadow:
    1px 1px 1px red,
    2px 2px 1px red;
}
```

Se questo venisse applicato all'elemento {{htmlelement("Heading_Elements", "&lt;h1>")}} nell'esempio Tommy The Cat, il risultato sarebbe il seguente:

{{ EmbedLiveSample('4shadows', '100%', 260) }}

> [!NOTE]
> È possibile vedere esempi più interessanti dell'uso di `text-shadow` nell'articolo di Sitepoint [Moonlighting with CSS text-shadow](https://www.sitepoint.com/moonlighting-css-text-shadow/).

## Layout del testo

Dopo aver esaminato le proprietà di base dei font, vediamo le proprietà che possono essere usate per influenzare il layout del testo.

### Allineamento del testo

La proprietà {{cssxref("text-align")}} viene usata per controllare il modo in cui il testo viene allineato all'interno del content box contenitore. I valori disponibili sono elencati di seguito e funzionano più o meno come in una normale applicazione di elaborazione testi:

- `left`: giustifica il testo a sinistra.
- `right`: giustifica il testo a destra.
- `center`: centra il testo.
- `justify`: distribuisce il testo, variando gli spazi tra le parole affinché tutte le righe di testo abbiano la stessa larghezza. Va usato con attenzione: può avere un aspetto terribile, soprattutto se applicato a un paragrafo che contiene molte parole lunghe. Se viene usato, è opportuno considerare anche l'uso di qualcos'altro insieme a esso, come {{cssxref("hyphens")}}, per spezzare alcune delle parole più lunghe tra le righe.

Se si applicasse `text-align: center;` all'elemento {{htmlelement("Heading_Elements", "&lt;h1>")}} nell'esempio, si otterrebbe quanto segue:

```css hidden live-sample___5text-align live-sample___6line-height live-sample___7letter-word-spacing
h1 {
  text-align: center;
}
```

{{ EmbedLiveSample('5text-align', '100%', 260) }}

### Altezza della riga

La proprietà {{cssxref("line-height")}} imposta l'altezza di ogni riga di testo. Questa proprietà può accettare non solo la maggior parte delle [unità di lunghezza e dimensione](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths), ma anche un valore senza unità, che agisce come moltiplicatore ed è generalmente considerato l'opzione migliore. Con un valore senza unità, {{cssxref("font-size")}} viene moltiplicato e produce il valore di `line-height`. Il testo del corpo ha generalmente un aspetto migliore ed è più facile da leggere quando le righe sono distanziate. L'altezza della riga consigliata è circa 1.5 – 2, ovvero interlinea doppia. Per impostare le righe di testo a 1.6 volte l'altezza del font, si userebbe:

```css live-sample___6line-height live-sample___7letter-word-spacing
p {
  line-height: 1.6;
}
```

Applicando questo agli elementi {{htmlelement("p")}} dell'esempio si otterrebbe il seguente risultato:

{{ EmbedLiveSample('6line-height', '100%', 300) }}

### Spaziatura tra lettere e parole

Le proprietà {{cssxref("letter-spacing")}} e {{cssxref("word-spacing")}} consentono di impostare la spaziatura tra lettere e parole nel testo. Non verranno usate molto spesso, ma possono essere utili per ottenere un aspetto specifico o migliorare la leggibilità di un font particolarmente fitto. Possono accettare la maggior parte delle [unità di lunghezza](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#lengths).

Per illustrare questo concetto, è possibile applicare un po' di spaziatura tra parole e lettere alla prima riga di ogni elemento {{htmlelement("p")}} nell'esempio HTML con:

```css live-sample___7letter-word-spacing
p::first-line {
  letter-spacing: 4px;
  word-spacing: 4px;
}
```

Questo rende l'HTML come segue:

{{ EmbedLiveSample('7letter-word-spacing', '100%', 330) }}

### Altre proprietà che vale la pena esaminare

Le proprietà precedenti forniscono un'idea di come iniziare ad applicare stili al testo in una pagina web, ma esistono molte altre proprietà utilizzabili. Qui sono state trattate solo quelle più importanti. Dopo aver acquisito familiarità con quelle precedenti, è opportuno esplorare anche le seguenti:

Stili del font:

- {{cssxref("font-variant")}}: passa tra maiuscoletto e alternative di font normali.
- {{cssxref("font-kerning")}}: attiva e disattiva le opzioni di kerning del font.
- {{cssxref("font-feature-settings")}}: attiva e disattiva varie funzionalità dei font [OpenType](https://en.wikipedia.org/wiki/OpenType).
- {{cssxref("font-variant-alternates")}}: controlla l'uso di glifi alternativi per un determinato font-face.
- {{cssxref("font-variant-caps")}}: controlla l'uso di glifi maiuscoli alternativi.
- {{cssxref("font-variant-east-asian")}}: controlla l'uso di glifi alternativi per le scritture dell'Asia orientale, come giapponese e cinese.
- {{cssxref("font-variant-ligatures")}}: controlla quali legature e forme contestuali vengono usate nel testo.
- {{cssxref("font-variant-numeric")}}: controlla l'uso di glifi alternativi per numeri, frazioni e indicatori ordinali.
- {{cssxref("font-variant-position")}}: controlla l'uso di glifi alternativi di dimensioni minori posizionati come apice o pedice.
- {{cssxref("font-size-adjust")}}: regola la dimensione visiva del font indipendentemente dalla sua dimensione effettiva.
- {{cssxref("font-stretch")}}: passa tra le possibili versioni alternative estese di un determinato font.
- {{cssxref("text-underline-position")}}: specifica la posizione delle sottolineature impostate usando il valore `underline` della proprietà `text-decoration-line`.
- {{cssxref("text-rendering")}}: cerca di eseguire alcune ottimizzazioni del rendering del testo.

Stili di layout del testo:

- {{cssxref("text-indent")}}: specifica quanto spazio orizzontale deve essere lasciato prima dell'inizio della prima riga del contenuto testuale.
- {{cssxref("text-overflow")}}: definisce come viene segnalato agli utenti il contenuto in overflow che non viene visualizzato.
- {{cssxref("white-space")}}: definisce come vengono gestiti gli spazi vuoti e le interruzioni di riga associate all'interno dell'elemento.
- {{cssxref("word-break")}}: specifica se interrompere le righe all'interno delle parole.
- {{cssxref("direction")}}: definisce la direzione del testo. Dipende dalla lingua e di solito è meglio lasciare che HTML gestisca questa parte, poiché è legata al contenuto testuale.
- {{cssxref("hyphens")}}: attiva e disattiva la sillabazione per le lingue supportate.
- {{cssxref("line-break")}}: riduce o aumenta le restrizioni sulle interruzioni di riga per le lingue asiatiche.
- {{cssxref("text-align-last")}}: definisce come viene allineata l'ultima riga di un blocco o una riga immediatamente prima di un'interruzione di riga forzata.
- {{cssxref("text-orientation")}}: definisce l'orientamento del testo in una riga.
- {{cssxref("overflow-wrap")}}: specifica se il browser può interrompere o meno le righe all'interno delle parole per evitare l'overflow.
- {{cssxref("writing-mode")}}: definisce se le righe di testo vengono disposte orizzontalmente o verticalmente e la direzione in cui scorrono le righe successive.

## Scorciatoia per i font

Molte proprietà dei font possono essere impostate anche tramite la proprietà shorthand {{cssxref("font")}}. Sono scritte nel seguente ordine: {{cssxref("font-style")}}, {{cssxref("font-variant")}}, {{cssxref("font-weight")}}, {{cssxref("font-stretch")}}, {{cssxref("font-size")}}, {{cssxref("line-height")}} e {{cssxref("font-family")}}.

Tra tutte queste proprietà, solo `font-size` e `font-family` sono obbligatorie quando si usa la proprietà shorthand `font`.

Tra le proprietà {{cssxref("font-size")}} e {{cssxref("line-height")}} deve essere inserita una barra.

Un esempio completo sarebbe il seguente:

```css
font:
  italic normal bold normal 3em/1.5 "Helvetica",
  "Arial",
  sans-serif;
```

## Sperimentare con lo stile del testo

Ora è il momento di provare. Per questa attività non sono previsti esercizi specifici. L'obiettivo è semplicemente sperimentare con alcune proprietà di layout di font/testo e vedere quali risultati è possibile ottenere.

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel Playground MDN.
2. Aggiungere alcune dichiarazioni alla regola vuota `p { }` fornita per modificare lo stile del testo. Usare tutta la creatività desiderata.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel Playground MDN. Fare riferimento alle sezioni precedenti dell'articolo per ulteriori informazioni sugli stili di font e testo che possono essere impostati.

```html live-sample___fonts_text
<p>Some sample text for your delight</p>
```

```css-nolint live-sample___fonts_text
p {

}
```

{{ EmbedLiveSample('fonts_text', "100%", 60) }}

## Riepilogo

Ci auguriamo che sperimentare con il testo in questo articolo sia stato piacevole. Il prossimo articolo fornirà tutto ciò che occorre sapere sullo stile degli elenchi HTML.

## Vedere anche

- [Tutto sulla proprietà CSS font-family](https://explainers.dev/font-family/), explainers.dev
- [Font sicuri per il web](https://scrimba.com/the-frontend-developer-career-path-c0j/~02b?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>

{{NextMenu("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling")}}
