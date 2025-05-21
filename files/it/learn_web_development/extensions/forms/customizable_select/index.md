---
title: Elementi select personalizzabili
short-title: Select personalizzabili
slug: Learn_web_development/Extensions/Forms/Customizable_select
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Advanced_form_styling", "Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms")}}

Questo articolo spiega come utilizzare insieme funzionalità moderne e dedicate di HTML e CSS per creare elementi {{htmlelement("select")}} completamente personalizzati. Questo include avere il pieno controllo sullo stile del pulsante select, del menu a discesa, dell'icona della freccia, del segno di spunta della selezione corrente e di ciascun elemento {{htmlelement("option")}} individualmente.

## Background

Tradizionalmente è stato difficile personalizzare l'aspetto degli elementi `<select>` perché contengono elementi interni stile a livello di sistema operativo, che non possono essere mirati utilizzando CSS. Questo include il menu a discesa, l'icona della freccia e così via.

In passato, la migliore opzione disponibile — a parte l'uso di una libreria JavaScript personalizzata — era impostare un valore {{cssxref("appearance")}} di `none` sull'elemento `<select>` per rimuovere parte della stilizzazione a livello di sistema operativo, e quindi utilizzare CSS per personalizzare le parti che possono essere stilizzate. Questa tecnica è spiegata in [Advanced form styling](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling).

Gli elementi `<select>` personalizzabili offrono una soluzione a questi problemi. Consentono di creare esempi come il seguente, utilizzando solo HTML e CSS, che sono completamente personalizzati nei browser che li supportano. Questo include il layout del `<select>` e del menu a discesa, lo schema dei colori, le icone, il font, le transizioni, il posizionamento, i marcatori per indicare l'icona selezionata e altro ancora.

{{EmbedLiveSample("full-render", "100%", "410px")}}

Inoltre, forniscono un miglioramento progressivo rispetto alla funzionalità esistente, ripiegando su select "classici" nei browser che non li supportano.

Scoprirai come costruire questo esempio nelle sezioni successive.

## Quali caratteristiche comprende un select personalizzabile?

Puoi costruire elementi `<select>` personalizzabili utilizzando le seguenti funzionalità HTML e CSS:

- Gli elementi classici come {{htmlelement("select")}}, {{htmlelement("option")}}, e {{htmlelement("optgroup")}}. Questi funzionano come nei select "classici", tranne che hanno tipi di contenuto aggiuntivi permessi.
- Un elemento {{htmlelement("button")}} incluso come primo figlio all'interno dell'elemento `<select>`, che non era precedentemente consentito nei select "classici". Quando questo è incluso, sostituisce il rendering "button" predefinito dell'elemento `<select>` chiuso. Questo è comunemente noto come **pulsante select** (poiché è il pulsante che è necessario premere per aprire il menu a discesa).
  > [!NOTE]
  > Il pulsante select è [inerte](/it/docs/Web/HTML/Reference/Global_attributes/inert) per impostazione predefinita, in modo che se i figli interattivi (ad esempio, link o pulsanti) sono inclusi all'interno esso, sarà comunque trattato come un singolo pulsante a scopo di interazione — ad esempio, gli elementi figlio non saranno focalizzabili o cliccabili.
- L'elemento {{htmlelement("selectedcontent")}} può essere opzionalmente incluso all'interno del primo figlio `<button>` dell'elemento `<select>` per visualizzare il valore attualmente selezionato all'interno dell'elemento `<select>` _chiuso_.
  Questo contiene un clone del contenuto dell'elemento `<option>` attualmente selezionato (creato usando [`cloneNode()`](/it/docs/Web/API/Node/cloneNode) in background).
- Il pseudo-elemento {{cssxref("::picker()", "::picker(select)")}}, che mira all'intero contenuto del picker. Questo include tutti gli elementi all'interno dell'elemento `<select>`, eccetto il primo figlio `<button>`.
- Il valore della proprietà {{cssxref("appearance")}} `base-select`, che inserisce l'elemento `<select>` e il pseudo-elemento `::picker(select)` negli stili e comportamenti predefiniti definiti dal browser per select personalizzabili.
- La pseudo-classe {{cssxref(":open")}}, che mira al pulsante select quando il picker (`::picker(select)`) è aperto.
- Il pseudo-elemento {{cssxref("::picker-icon")}}, che mira all'icona all'interno del pulsante select — la freccia che punta verso il basso quando il select è chiuso.
- La pseudo-classe {{cssxref(":checked")}}, che mira all'elemento `<option>` attualmente selezionato.
- Il pseudo-elemento {{cssxref("::checkmark")}}, che mira al segno di spunta posizionato all'interno dell'elemento `<option>` attualmente selezionato per fornire un'indicazione visiva di quale sia selezionato.

Inoltre, l'elemento `<select>` e il suo menu a discesa hanno automaticamente il seguente comportamento assegnato a loro:

- Hanno una relazione invocatore/popover, come specificato dal [Popover API](/it/docs/Web/API/Popover_API), che fornisce la possibilità di selezionare il picker quando è aperto tramite la pseudo-classe {{cssxref(":popover-open")}}. Vedi [Using the Popover API](/it/docs/Web/API/Popover_API/Using) per maggiori dettagli sul comportamento del popover.
- Hanno un riferimento di ancoraggio implicito, il che significa che il picker è automaticamente associato all'elemento `<select>` tramite [CSS anchor positioning](/it/docs/Web/CSS/CSS_anchor_positioning). Gli stili predefiniti del browser posizionano il picker rispetto al pulsante (l'ancoraggio) e puoi personalizzare questa posizione come spiegato in [Positioning elements relative to their anchor](/it/docs/Web/CSS/CSS_anchor_positioning/Using#positioning_elements_relative_to_their_anchor). Gli stili predefiniti del browser definiscono anche alcuni fallback di posizione-try che riposizionano il picker se è in pericolo di traboccare il viewport. I fallback di posizione try sono spiegati in [Handling overflow: try fallbacks and conditional hiding](/it/docs/Web/CSS/CSS_anchor_positioning/Try_options_hiding).

> [!NOTE]
> Puoi controllare il supporto del browser per gli `<select>` personalizzabili visualizzando le tabelle di compatibilità del browser sulle pagine di riferimento per funzionalità correlate come {{htmlelement("selectedcontent")}}, {{cssxref("::picker()", "::picker(select)")}}, e {{cssxref("::checkmark")}}.

Esaminiamo tutte le funzionalità sopra descritte in azione, passando attraverso l'esempio mostrato all'inizio della pagina.

## Marcatura del select personalizzabile

Il nostro esempio è un tipico menu {{htmlelement("select")}} che ti permette di scegliere un animale domestico. La marcatura è la seguente:

```html live-sample___plain-render live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
<form>
  <p>
    <label for="pet-select">Select pet:</label>
    <select id="pet-select">
      <button>
        <selectedcontent></selectedcontent>
      </button>

      <option value="">Please select a pet</option>
      <option value="cat">
        <span class="icon" aria-hidden="true">🐱</span
        ><span class="option-label">Cat</span>
      </option>
      <option value="dog">
        <span class="icon" aria-hidden="true">🐶</span
        ><span class="option-label">Dog</span>
      </option>
      <option value="hamster">
        <span class="icon" aria-hidden="true">🐹</span
        ><span class="option-label">Hamster</span>
      </option>
      <option value="chicken">
        <span class="icon" aria-hidden="true">🐔</span
        ><span class="option-label">Chicken</span>
      </option>
      <option value="fish">
        <span class="icon" aria-hidden="true">🐟</span
        ><span class="option-label">Fish</span>
      </option>
      <option value="snake">
        <span class="icon" aria-hidden="true">🐍</span
        ><span class="option-label">Snake</span>
      </option>
    </select>
  </p>
</form>
```

> [!NOTE]
> L'attributo [`aria-hidden="true"`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden) è incluso sulle icone in modo che siano nascoste alle tecnologie assistive, evitando che i valori delle opzioni vengano annunciati due volte (ad esempio, "cat cat").

Il markup dell'esempio è quasi lo stesso del markup `<select>` classico, con le seguenti differenze:

- La struttura `<button><selectedcontent></selectedcontent></button>` rappresenta il pulsante {{htmlelement("button")}} del select.
  Aggiungendo l'elemento {{htmlelement("selectedcontent")}} al browser il contenuto dell'{{htmlelement("option")}} attualmente selezionato viene clonato all'interno del pulsante, che puoi poi [fornire stili personalizzati](#regolazione_dello_stile_del_contenuto_dell'opzione_selezionata_all'interno_del_pulsante_select). Se questa struttura non è inclusa nel tuo markup, il browser tornerà a visualizzare il testo dell'opzione selezionata all'interno del pulsante predefinito, e non sarai in grado di stilizzarlo facilmente.
  > [!NOTE]
  > È _possibile_ includere contenuti arbitrari all'interno del `<button>` per visualizzare ciò che desideri all'interno del `<select>` chiuso, ma fai attenzione. Ciò che includi può alterare il valore accessibile esposto alla tecnologia assistiva per l'elemento `<select>`.
- Il resto dei contenuti `<select>` rappresenta il menu a discesa, che è di solito limitato agli elementi `<option>` che rappresentano le diverse scelte nel menu. Puoi includere altri contenuti nel menu, ma non è raccomandato.
- Tradizionalmente, gli elementi `<option>` potevano contenere solo testo, ma in un select personalizzabile puoi includere altre strutture di markup come immagini, altri elementi semantici di testo non interattivi, e altro. Puoi persino usare i pseudo-elementi {{cssxref("::before")}} e {{cssxref("::after")}} per includere altri contenuti, anche se tieni presente che questi non saranno inclusi nel valore inviabile. Nel nostro esempio, ciascun `<option>` contiene due elementi {{htmlelement("span")}} con un'icona e un'etichetta di testo rispettivamente, consentendo a ciascuno di essere stilizzato e posizionato indipendentemente.

  > [!NOTE]
  > Poiché il contenuto dell'elemento `<option>` può contenere sottostrutture DOM a più livelli, non solo nodi di testo, ci sono regole su come il browser dovrebbe estrarre il [valore corrente del `<select>`](/it/docs/Web/API/HTMLSelectElement/value) tramite JavaScript. Il valore della proprietà [`textContent`](/it/docs/Web/API/Node/textContent) dell'elemento `<option>` selezionato viene recuperato, viene eseguito su di esso {{jsxref("String.prototype.trim", "trim()")}}, e il risultato viene impostato come valore del `<select>`.

Questo design consente ai browser non supportati di tornare a un'esperienza `<select>` classica. La struttura `<button><selectedcontent></selectedcontent></button>` verrà completamente ignorata, e il contenuto non testuale degli `<option>` verrà spogliato per lasciare solo i contenuti dei nodi di testo, ma il risultato funzionerà comunque.

## Optare per il rendering del select personalizzabile

Per optare per la funzionalità di select personalizzato e gli stili di base minimi del browser (e rimuovere lo stile fornito dal sistema operativo), il tuo elemento `<select>` e il suo menu a discesa (rappresentato dal pseudo-elemento `::picker(select)`) devono avere entrambi un valore {{cssxref("appearance")}} di `base-select` impostato su di essi:

```css live-sample___plain-render live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select,
::picker(select) {
  appearance: base-select;
}
```

```css hidden live-sample___plain-render live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
* {
  box-sizing: border-box;
}

html {
  font-family: Arial, Helvetica, sans-serif;
}

body {
  width: 100%;
  padding: 0 10px;
  max-width: 480px;
  margin: 0 auto;
}

h2 {
  font-size: 1.2rem;
}

p {
  display: flex;
  gap: 10px;
}

label {
  width: fit-content;
  align-self: center;
}

select {
  flex: 1;
}
```

Puoi scegliere di fare opt-in solo sull'elemento `<select>` alla nuova funzionalità, lasciando il menu con lo stile predefinito del sistema operativo, ma nella maggior parte dei casi, vorrai fare opt-in su entrambi. Non puoi fare opt-in sul menu senza farlo sull'elemento `<select>`.

Una volta fatto ciò, il risultato è una rappresentazione molto semplice di un elemento `<select>`:

{{EmbedLiveSample("plain-render", "100%", "240px")}}

Ora sei libero di stilizzarlo come preferisci. Per iniziare, l'elemento `<select>` ha valori personalizzati di {{cssxref("border")}}, {{cssxref("background")}} (che cambia su {{cssxref(":hover")}} o {{cssxref(":focus")}}), e {{cssxref("padding")}}, oltre a una {{cssxref("transition")}} in modo che il cambio di sfondo si animi in modo fluido:

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select {
  border: 2px solid #ddd;
  background: #eee;
  padding: 10px;
  transition: 0.4s;
}

select:hover,
select:focus {
  background: #ddd;
}
```

## Stilizzazione dell'icona del picker

Per stilizzare l'icona all'interno del pulsante select — la freccia che punta verso il basso quando il select è chiuso — può essere mirata con il pseudo-elemento {{cssxref("::picker-icon")}}. Il seguente codice conferisce all'icona un {{cssxref("color")}} personalizzato e una `transition` in modo che i cambiamenti alla sua proprietà {{cssxref("rotate")}} siano animati in modo fluido:

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select::picker-icon {
  color: #999;
  transition: 0.4s rotate;
}
```

Successivamente, `::picker-icon` è combinato con la pseudo-classe {{cssxref(":open")}} — che mira al pulsante select solo quando il menu a discesa è aperto — per dare all'icona un valore di `rotate` di `180deg` quando il `<select>` è aperto.

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select:open::picker-icon {
  rotate: 180deg;
}
```

Diamo un'occhiata al lavoro fin qui — nota come la freccia del picker ruota dolcemente di 180 gradi quando il `<select>` si apre e si chiude:

{{EmbedLiveSample("second-render", "100%", "250px")}}

## Stilizzazione del menu a discesa

Il menu a discesa può essere mirata usando il pseudo-elemento {{cssxref("::picker()", "::picker(select)")}}. Come detto in precedenza, il picker contiene tutto all'interno dell'elemento `<select>` che non è il pulsante e il `<selectedcontent>`. Nel nostro esempio, ciò significa tutti gli elementi `<option>` e i loro contenuti.

Innanzitutto, il {{cssxref("border")}} predefinito nero del picker viene rimosso:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
::picker(select) {
  border: none;
}
```

Ora gli elementi `<option>` sono stilizzati. Sono disposti con [flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout), allineandoli tutti all'inizio del contenitore flessibile e includendo uno {{cssxref("gap")}} di `20px` tra ciascuno. A ciascun `<option>` vengono anche dati lo stesso {{cssxref("border")}}, {{cssxref("background")}}, {{cssxref("padding")}}, e {{cssxref("transition")}} del `<select>`, per fornire un aspetto e un aspetto coerente:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option {
  display: flex;
  justify-content: flex-start;
  gap: 20px;

  border: 2px solid #ddd;
  background: #eee;
  padding: 10px;
  transition: 0.4s;
}
```

> [!NOTE]
> I `<option>` degli elementi `<select>` personalizzabili hanno `display: flex` impostato di default, ma è incluso comunque nel nostro foglio di stile per chiarire cosa sta succedendo.

Successivamente, una combinazione delle pseudo-classi {{cssxref(":first-of-type")}}, {{cssxref(":last-of-type")}}, e {{cssxref(":not()")}} è utilizzata per impostare un {{cssxref("border-radius")}} appropriato sugli angoli superiori e inferiori del picker, e rimuovere il {{cssxref("border-bottom")}} da tutti gli elementi `<option>` tranne l'ultimo per evitare che i bordi appaiano disordinati e raddoppiati:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option:first-of-type {
  border-radius: 8px 8px 0 0;
}

option:last-of-type {
  border-radius: 0 0 8px 8px;
}

option:not(option:last-of-type) {
  border-bottom: none;
}
```

Successivamente, un colore di `background` diverso viene impostato sugli elementi `<option>` dispari usando {{cssxref(":nth-of-type()", ":nth-of-type(odd)")}} per implementare l'effetto zebra-striping, e un colore di `background` diverso è impostato sugli elementi `<option>` al focus e al hover, per fornire un utile evidenziamento visivo durante la selezione:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option:nth-of-type(odd) {
  background: #fff;
}

option:hover,
option:focus {
  background: plum;
}
```

Infine per questa sezione, una dimensione del {{cssxref("font-size")}} più grande è impostata sulle icone degli `<option>` (contenute all'interno degli elementi `<span>` con una classe `icon`) per renderle più grandi, e la proprietà {{cssxref("text-box")}} è utilizzata per rimuovere parte dello spazio fastidioso ai bordi di inizio e fine blocco delle icone emoji, allineandole meglio con le etichette di testo:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option .icon {
  font-size: 1.6rem;
  text-box: trim-both cap alphabetic;
}
```

Il nostro esempio ora si rinnova così:

{{EmbedLiveSample("third-render", "100%", "370px")}}

## Regolazione dello stile del contenuto dell'opzione selezionata all'interno del pulsante select

Se selezioni qualsiasi opzione di animale domestico dagli ultimi esempi live, noterai un problema: le icone degli animali causano l'aumento dell'altezza del pulsante select, che cambia anche la posizione dell'icona del picker, e non c'è spazio tra l'icona dell'opzione e l'etichetta.

Questo può essere risolto nascondendo l'icona quando si trova all'interno di `<selectedcontent>`, che rappresenta il contenuto dell'{{htmlelement("option")}} selezionato a quanto appaiono nel pulsante select. Nel nostro esempio, è nascosto utilizzando {{cssxref("display", "display: none")}}:

```css live-sample___fourth-render live-sample___full-render
selectedcontent .icon {
  display: none;
}
```

Questo non influisce sulla stilizzazione del contenuto dell'`<option>` come appare all'interno del menu a discesa.

## Stilizzazione dell'opzione attualmente selezionata

Per stilizzare l'opzione attualmente selezionata `<option>` come appare all'interno del menu a discesa, puoi mirarla utilizzando la pseudo-classe {{cssxref(":checked")}}. Questo è utilizzato per impostare il valore {{cssxref("font-weight")}} dell'elemento `<option>` selezionato a `bold`:

```css live-sample___fourth-render live-sample___full-render
option:checked {
  font-weight: bold;
}
```

## Stilizzazione del segno di spunta della selezione corrente

Probabilmente hai notato che quando apri il picker per fare una selezione, l'elemento `<option>` attualmente selezionato ha un segno di spunta alla sua estremità di inizio in linea. Questo segno di spunta può essere mirato utilizzando il pseudo-elemento {{cssxref("::checkmark")}}. Ad esempio, potresti voler nascondere questo segno di spunta (ad esempio, tramite `display: none`).

Potresti anche scegliere di fare qualcosa di un po' più interessante con esso — in precedenza gli elementi `<option>` erano disposti orizzontalmente usando il flexbox, con gli elementi flessibili allineati all'inizio della riga. Nella regola sotto riportata, il segno di spunta è spostato dall'inizio della riga alla fine impostando un valore di {{cssxref("order")}} su di esso maggiore di `0`, e allineandolo alla fine della riga usando un valore di {{cssxref("margin-left")}} `auto` (vedi [Alignment and auto margins](/it/docs/Web/CSS/CSS_box_alignment/Box_alignment_in_flexbox#alignment_and_auto_margins)).

Infine, il valore della proprietà {{cssxref("content")}} è impostato su una emoji diversa, per impostare un'icona diversa da visualizzare.

```css live-sample___fourth-render live-sample___full-render
option::checkmark {
  order: 1;
  margin-left: auto;
  content: "☑️";
}
```

> [!NOTE]
> I pseudo-elementi `::checkmark` e `::picker-icon` non sono inclusi nell'albero dell'accessibilità, quindi qualsiasi contenuto generato {{cssxref("content")}} impostato su di essi non sarà annunciato dalle tecnologie assistive. Dovresti comunque assicurarti che qualsiasi nuova icona impostata abbia senso visivamente per il suo scopo previsto.

Verifichiamo di nuovo come l'esempio è renderizzato. Lo stato aggiornato dopo le ultime tre sezioni è il seguente:

{{EmbedLiveSample("fourth-render", "100%", "410px")}}

## Animazione del picker usando gli stati popover

Il pulsante select e il menu a discesa dell'elemento `<select>` personalizzabile sono automaticamente forniti di una relazione invocatore/popover, come descritto in [Using the Popover API](/it/docs/Web/API/Popover_API/Using). Ci sono molti vantaggi che questo porta agli elementi `<select>`; il nostro esempio sfrutta la capacità di animare tra gli stati del popover nascosto e visualizzato utilizzando transizioni. La pseudo-classe {{cssxref(":popover-open")}} rappresenta popover nello stato visualizzato.

La tecnica è descritta brevemente in questa sezione — leggi [Animating popovers](/it/docs/Web/API/Popover_API/Using#animating_popovers) per una descrizione più dettagliata.

Innanzitutto, il picker è selezionato usando `::picker(select)`, e gli viene dato un valore di {{cssxref("opacity")}} di `0` e un valore di `transition` di `all 0.4s allow-discrete`. Questo causa l'animazione di tutte le proprietà che cambiano valore quando lo stato del popover cambia da nascosto a visualizzato.

```css live-sample___full-render
::picker(select) {
  opacity: 0;
  transition: all 0.4s allow-discrete;
}
```

L'elenco delle proprietà in transizione include `opacity`, ma include anche due proprietà discrete i cui valori sono impostati dagli stili predefiniti del browser:

- {{cssxref("display")}}
  - : I valori `display` cambiano da `none` a `block` quando il popover cambia stato da nascosto a mostrato. Questo deve essere animato per assicurare che siano visibili altre transizioni.
- {{cssxref("overlay")}}
  - : Il valore `overlay` cambia da `none` a `auto` quando il popover cambia stato da nascosto a mostrato, per promuoverlo al {{Glossary("top_layer", "top layer")}}, poi di nuovo quando è nascosto per rimuoverlo. Questo deve essere animato per garantire che la rimozione del popover dal top layer sia rinviata fino al completamento della transizione, assicurando che la transizione sia visibile.

> [!NOTE]
> Il valore [`allow-discrete`](/it/docs/Web/CSS/transition-behavior#allow-discrete) è necessario per abilitare le animazioni di proprietà discrete.

Successivamente, il picker è selezionato nello stato visualizzato usando `::picker(select):popover-open` e gli viene dato un valore di `opacity` pari a `1` — questo è lo stato finale della transizione:

```css live-sample___full-render
::picker(select):popover-open {
  opacity: 1;
}
```

Infine, poiché il picker è in transizione mentre sta passando da `display: none` a un valore `display` che lo rende visibile, lo stato iniziale della transizione deve essere specificato all'interno di un blocco {{cssxref("@starting-style")}}:

```css live-sample___full-render
@starting-style {
  ::picker(select):popover-open {
    opacity: 0;
  }
}
```

Queste regole lavorano insieme per fare in modo che il picker svanisca in modo fluido e scompaia quando il `<select>` viene aperto e chiuso.

## Posizionare il picker utilizzando il posizionamento dell'ancora

Un pulsante select e un menu a discesa di un elemento `<select>` personalizzabile hanno un riferimento implicito all'ancora, e il picker è automaticamente associato al pulsante select tramite [CSS anchor positioning](/it/docs/Web/CSS/CSS_anchor_positioning). Questo significa che un'associazione esplicita non deve essere effettuata usando le proprietà {{cssxref("anchor-name")}} e {{cssxref("position-anchor")}}.

Inoltre, gli [stili predefiniti del browser forniscono una posizione predefinita](/it/docs/Web/CSS/::picker#picker_anchor_positioning), che puoi personalizzare come spiegato in [Positioning elements relative to their anchor](/it/docs/Web/CSS/CSS_anchor_positioning/Using#positioning_elements_relative_to_their_anchor).

Nella nostra demo, la posizione del picker è impostata rispetto alla sua ancora utilizzando la funzione {{cssxref("anchor()")}} all'interno dei valori delle proprietà {{cssxref("top")}} e {{cssxref("left")}}:

```css live-sample___full-render
::picker(select) {
  top: calc(anchor(bottom) + 1px);
  left: anchor(10%);
}
```

Questo fa sì che il bordo superiore del picker sia sempre posizionato 1 pixel sotto il bordo inferiore del pulsante select, e il bordo sinistro del picker sia sempre posizionato `10%` della larghezza del pulsante select attraverso il suo bordo sinistro.

## Risultato finale

Dopo le ultime due sezioni, lo stato aggiornato finale del nostro `<select>` viene renderizzato in questo modo:

{{EmbedLiveSample("full-render", "100%", "410px")}}

## Personalizzazione di altre caratteristiche del select classico

Le sezioni precedenti hanno coperto tutte le nuove funzionalità disponibili nei select personalizzabili, e mostrato come interagiscano con selezioni classiche a singola linea, e funzionalità moderne correlate come popover e posizionamento dell'ancora. Ci sono alcune altre caratteristiche degli elementi `<select>` non menzionate sopra; questa sezione spiega come attualmente lavorano insieme ai select personalizzabili:

- [`<select multiple>`](/it/docs/Web/HTML/Reference/Attributes/multiple)
  - : Attualmente non c'è alcun supporto specificato per l'attributo `multiple` sugli elementi `<select>` personalizzabili, ma in futuro si lavorerà su questo.
- {{htmlelement("optgroup")}}
  - : Lo stile predefinito degli elementi `<optgroup>` è lo stesso degli elementi `<select>` classici — in grassetto e rientrato meno delle opzioni contenute. Devi assicurarti di stilizzare gli elementi `<optgroup>` in modo che si adattino al design generale, e tieni presente che si comporteranno proprio come ci si aspetta che si comportino i contenitori nel HTML convenzionale. Negli elementi `<select>` personalizzabili, l'elemento {{htmlelement("legend")}} è permesso come figlio di `<optgroup>`, per fornire un'etichetta che è facile da mirare e stilizzare. Questo sostituisce qualsiasi testo impostato nell'attributo `label` dell'elemento `<optgroup>`, e ha la stessa semantica.

## Prossimamente

Nel prossimo articolo di questo modulo, esploreremo le diverse [pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes) disponibili in moderni browser per stilizzare i moduli in diversi stati.

## Vedi anche

- {{htmlelement("select")}}, {{htmlelement("option")}}, {{htmlelement("optgroup")}}, {{htmlelement("label")}}, {{htmlelement("button")}}, {{htmlelement("selectedcontent")}}
- {{cssxref("appearance")}}
- {{cssxref("::picker()", "::picker(select)")}}, {{cssxref("::picker-icon")}}, {{cssxref("::checkmark")}}
- {{cssxref(":open")}}, {{cssxref(":checked")}}

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Advanced_form_styling", "Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms")}}
