---
title: Elementi select personalizzabili
short-title: Select personalizzabili
slug: Learn_web_development/Extensions/Forms/Customizable_select
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Advanced_form_styling", "Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms")}}

Questo articolo spiega come utilizzare insieme funzionalità moderne di HTML e CSS dedicate per creare elementi {{htmlelement("select")}} completamente personalizzati. Questo include avere pieno controllo sullo stile del pulsante select, del selettore a discesa, dell'icona freccia, del segno di spunta della selezione attuale e di ciascun elemento {{htmlelement("option")}} individuale.

## Background

Tradizionalmente è stato difficile personalizzare l'aspetto degli elementi `<select>` perché contengono parti interne che sono stilizzate a livello di sistema operativo, che non possono essere targetizzate usando CSS. Questo include il selettore a discesa, l'icona freccia, e così via.

In precedenza, l'opzione migliore disponibile — a parte l'uso di una libreria JavaScript personalizzata — era impostare un valore {{cssxref("appearance")}} di `none` sull'elemento `<select>` per rimuovere parte della stilizzazione a livello di sistema operativo e quindi usare CSS per personalizzare le parti che possono essere stilizzate. Questa tecnica è spiegata in [Stile avanzato del modulo](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling).

Gli elementi `<select>` personalizzabili forniscono una soluzione a questi problemi. Permettono di costruire esempi come il seguente, usando solo HTML e CSS, che sono completamente personalizzati nei browser supportati. Questo include layout di `<select>` e del selettore a discesa, schema di colori, icone, font, transizioni, posizionamento, marcatori per indicare l'icona selezionata, e altro.

{{EmbedLiveSample("full-render", "100%", "410px")}}

Inoltre, forniscono un miglioramento progressivo rispetto alla funzionalità esistente, ritornando a selects "classici" nei browser non supportati.

Scoprirà come costruire questo esempio nelle sezioni seguenti.

## Quali funzionalità comprendono un select personalizzabile?

Può costruire elementi `<select>` personalizzabili usando le seguenti caratteristiche di HTML e CSS:

- I semplici elementi {{htmlelement("select")}}, {{htmlelement("option")}}, e {{htmlelement("optgroup")}}. Questi funzionano nello stesso modo dei selects "classici", tranne per il fatto che hanno tipi di contenuti aggiuntivi permessi.
- Un elemento {{htmlelement("button")}} incluso come primo figlio all'interno dell'elemento `<select>`, che in precedenza non era consentito nei selects "classici". Quando questo è incluso, sostituisce il rendering predefinito del "pulsante" dell'elemento `<select>` chiuso. Questo è comunemente noto come **select button** (poiché è il pulsante che deve premere per aprire il selettore a discesa).
  > [!NOTE]
  > Il pulsante select è [inerte](/it/docs/Web/HTML/Reference/Global_attributes/inert) per impostazione predefinita così che, se vengono inclusi bambini interattivi (ad esempio, link o pulsanti) al suo interno, verrà comunque trattato come un singolo pulsante per scopi di interazione — ad esempio, gli elementi figli non saranno focalizzabili o cliccabili.
- L'elemento {{htmlelement("selectedcontent")}} può opzionalmente essere incluso all'interno dell'elemento `<button>` primo figlio dell'elemento `<select>` per visualizzare il valore attualmente selezionato all'interno del `<select>` chiuso.
  Questo contiene un clone del contenuto dell'elemento `<option>` attualmente selezionato (creato usando [`cloneNode()`](/it/docs/Web/API/Node/cloneNode) sotto il cofano).
- Il pseudo-elemento {{cssxref("::picker()", "::picker(select)")}}, che targetizza l'intero contenuto del selettore. Questo include tutti gli elementi all'interno dell'elemento `<select>`, tranne il primo figlio `<button>`.
- Il valore della proprietà {{cssxref("appearance")}} `base-select`, che inserisce l'elemento `<select>` e il pseudo-elemento `::picker(select)` nei stili e comportamenti predefiniti definiti dal browser per il select personalizzabile.
- La pseudo-classe {{cssxref(":open")}}, che targetizza il pulsante select quando il selettore (`::picker(select)`) è aperto.
- Il pseudo-elemento {{cssxref("::picker-icon")}}, che targetizza l'icona all'interno del pulsante select — la freccia che punta verso il basso quando il select è chiuso.
- La pseudo-classe {{cssxref(":checked")}}, che targetizza l'elemento `<option>` attualmente selezionato.
- Il pseudo-elemento {{cssxref("::checkmark")}}, che targetizza il segno di spunta posizionato all'interno dell'elemento `<option>` attualmente selezionato per fornire una indicazione visiva di quale è stato selezionato.

In aggiunta, l'elemento `<select>` e il suo selettore a discesa hanno il comportamento seguente assegnato automaticamente:

- Hanno un rapporto invocatore/popover, come specificato dall'[API Popover](/it/docs/Web/API/Popover_API), che fornisce la possibilità di selezionare il selettore quando aperto tramite la pseudo-classe {{cssxref(":popover-open")}}. Vedere [Uso dell'API Popover](/it/docs/Web/API/Popover_API/Using) per ulteriori dettagli sul comportamento dei popover.
- Hanno un riferimento implicito all'ancora, il che significa che il selettore è automaticamente associato all'elemento `<select>` tramite [posizionamento dell'ancora CSS](/it/docs/Web/CSS/CSS_anchor_positioning). Gli stili predefiniti del browser posizionano il selettore rispetto al pulsante (l'ancora) e può personalizzare questa posizione come spiegato in [Posizionamento degli elementi rispetto alla loro ancora](/it/docs/Web/CSS/CSS_anchor_positioning/Using#positioning_elements_relative_to_their_anchor). Gli stili predefiniti del browser definiscono anche alcuni fallback di posizione-try che riposizionano il selettore se è in pericolo di debordare dalla viewport. I fallback di posizione-try sono spiegati in [Gestione dell'overflow: try fallbacks e nascondere condizionale](/it/docs/Web/CSS/CSS_anchor_positioning/Try_options_hiding).

> [!NOTE]
> Può controllare il supporto del browser per il `<select>` personalizzabile visualizzando le tabelle di compatibilità del browser sulle pagine di riferimento per le funzionalità correlate come {{htmlelement("selectedcontent")}}, {{cssxref("::picker()", "::picker(select)")}}, e {{cssxref("::checkmark")}}.

Diamo un'occhiata a tutte le funzionalità elencate in azione, percorrendo l'esempio mostrato in cima alla pagina.

## Marcup del select personalizzabile

Il nostro esempio è un tipico menu {{htmlelement("select")}} che consente di scegliere un animale domestico. Il markup è il seguente:

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
> L'attributo [`aria-hidden="true"`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden) è incluso sulle icone così che saranno nascoste dalle tecnologie assistive, evitando che i valori delle opzioni siano annunciati due volte (ad esempio, "cat cat").

Il markup dell'esempio è quasi lo stesso del markup del `<select>` "classico", con le seguenti differenze:

- La struttura `<button><selectedcontent></selectedcontent></button>` rappresenta il pulsante {{htmlelement("select")}}.
  L'aggiunta dell'elemento {{htmlelement("selectedcontent")}} fa sì che il browser cloni l'{{htmlelement("option")}} attualmente selezionata all'interno del pulsante, per la quale può quindi [fornire stili personalizzati](#regolazione_dei_contenuti_dell'opzione_selezionata_all'interno_del_pulsante_select). Se questa struttura non è inclusa nel suo markup, il browser tornerà a rendere il testo dell'opzione selezionata all'interno del pulsante predefinito, e non potrà stilizzarlo facilmente.
  > [!NOTE]
  > _Può_ includere contenuti arbitrari all'interno del `<button>` per rendere qualsiasi cosa desidera all'interno del `<select>` chiuso, ma stia attento quando lo fa. Quello che include può modificare il valore accessibile esposto alla tecnologia assistiva per l'elemento `<select>`.
- Il resto dei contenuti del `<select>` rappresenta il selettore a discesa, che di solito è limitato agli elementi `<option>` che rappresentano le diverse scelte nel selettore. Può includere altri contenuti nel selettore, ma non è raccomandato.
- Tradizionalmente, gli elementi `<option>` potevano contenere solo testo, ma in un select personalizzabile può includere altre strutture di markup come immagini, altri elementi semantici a livello di testo non interattivi, e altro ancora. Può persino utilizzare gli pseudo-elementi {{cssxref("::before")}} e {{cssxref("::after")}} per includere altri contenuti, sebbene tenga presente che questi non sarebbero inclusi nel valore inviabile. Nel nostro esempio, ciascun `<option>` contiene due elementi {{htmlelement("span")}} contenenti rispettivamente un'icona e un'etichetta di testo, consentendo a ciascuno di stilizzarli e posizionarli indipendentemente.

  > [!NOTE]
  > Poiché il contenuto di `<option>` può contenere sotto-alberi DOM a più livelli, non solo nodi di testo, ci sono regole su come il browser dovrebbe estrarre il [valore corrente del `<select>`](/it/docs/Web/API/HTMLSelectElement/value) tramite JavaScript. Viene recuperato il valore della proprietà [`textContent`](/it/docs/Web/API/Node/textContent) dell'elemento `<option>` selezionato, viene eseguito {{jsxref("String.prototype.trim", "trim()")}} su di esso, e il risultato viene impostato come valore del `<select>`.

Questo design consente ai browser non supportati di ripiegare su un'esperienza di `<select>` classico. La struttura `<button><selectedcontent></selectedcontent></button>` verrà completamente ignorata, e i contenuti non di testo di `<option>` verranno eliminati per lasciare solo i contenuti del nodo di testo, ma il risultato funzionerà comunque.

## Opting in alla resa del select personalizzabile

Per optare alla funzionalità del select personalizzabile e agli stili base minimi del browser (e rimuovere la stilizzazione fornita dal sistema operativo), il suo elemento `<select>` e il suo selettore a discesa (rappresentato dal pseudo-elemento `::picker(select)`) devono avere entrambi un valore {{cssxref("appearance")}} di `base-select` impostato su di essi:

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

Può scegliere di optare solo l'elemento `<select>` alla nuova funzionalità, lasciando il selettore con lo stile predefinito del sistema operativo, ma nella maggior parte dei casi vorrà optare entrambi. Non può optare il selettore senza fare lo stesso per l'elemento `<select>`.

Una volta fatto questo, il risultato è una resa molto semplice di un elemento `<select>`:

{{EmbedLiveSample("plain-render", "100%", "240px")}}

Ora è libero di stilizzarlo in qualsiasi modo desidera. Inizialmente, l'elemento `<select>` ha valori personalizzati di {{cssxref("border")}}, {{cssxref("background")}} (che cambia su {{cssxref(":hover")}} o {{cssxref(":focus")}}), e {{cssxref("padding")}}, oltre a una {{cssxref("transition")}} in modo che il cambio di sfondo venga animato senza intoppi:

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

## Stilizzazione dell'icona del selettore

Per stilizzare l'icona all'interno del pulsante select — la freccia che punta verso il basso quando il select è chiuso — può targetizzarla con lo pseudo-elemento {{cssxref("::picker-icon")}}. Il codice seguente dà all'icona una {{cssxref("color")}} personalizzata e una `transition` in modo che le modifiche alla sua proprietà {{cssxref("rotate")}} siano animate senza intoppi:

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select::picker-icon {
  color: #999;
  transition: 0.4s rotate;
}
```

Successivamente, `::picker-icon` è combinato con la pseudo-classe {{cssxref(":open")}} — che targetizza il pulsante select solo quando il selettore a discesa è aperto — per dare all'icona un valore di `rotate` di `180deg` quando il `<select>` è aperto.

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select:open::picker-icon {
  rotate: 180deg;
}
```

Diamo un'occhiata al lavoro finora — noti come la freccia del selettore ruota senza intoppi di 180 gradi quando il `<select>` si apre e si chiude:

{{EmbedLiveSample("second-render", "100%", "250px")}}

## Stilizzazione del selettore a discesa

Il selettore a discesa può essere targetizzato utilizzando il pseudo-elemento {{cssxref("::picker()", "::picker(select)")}}. Come menzionato in precedenza, il selettore include tutto ciò che si trova all'interno dell'elemento `<select>` tranne il pulsante e il `<selectedcontent>`. Nel nostro esempio, questo significa tutti gli elementi `<option>` e i loro contenuti.

Prima di tutto, viene rimossa la {{cssxref("border")}} nera predefinita del selettore:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
::picker(select) {
  border: none;
}
```

Ora vengono stilizzati gli elementi `<option>`. Sono disposti con [flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout), allineandoli tutti all'inizio del contenitore flessibile e includendo un {{cssxref("gap")}} di `20px` tra ciascuno. Ogni `<option>` riceve inoltre la stessa {{cssxref("border")}}, {{cssxref("background")}}, {{cssxref("padding")}}, e {{cssxref("transition")}} dell'elemento `<select>`, per fornire un aspetto e sensazione coerenti:

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
> Gli elementi `<option>` del `<select>` personalizzabile hanno per impostazione predefinita `display: flex`, ma è incluso nel nostro foglio di stile per chiarire cosa sta succedendo.

Successivamente, una combinazione di pseudo-classi {{cssxref(":first-of-type")}}, {{cssxref(":last-of-type")}}, e {{cssxref(":not()")}} è utilizzata per impostare un {{cssxref("border-radius")}} appropriato sugli angoli superiori e inferiori del selettore, e rimuovere la {{cssxref("border-bottom")}} da tutti gli elementi `<option>` tranne l'ultimo così che i bordi non sembrino disordinati e raddoppiati:

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

Successivamente, un diverso colore di `background` è impostato sugli elementi `<option>` dispari utilizzando {{cssxref(":nth-of-type()", ":nth-of-type(odd)")}} per implementare il striping a zebra, e un diverso colore di `background` è impostato sui `<option>` quando sono al focus e hover, per fornire un'evidenziazione visiva utile durante la selezione:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option:nth-of-type(odd) {
  background: #fff;
}

option:hover,
option:focus {
  background: plum;
}
```

Infine per questa sezione, una dimensione del {{cssxref("font-size")}} maggiore è impostata sulle icone delle `<option>` (contenute all'interno di elementi `<span>` con una classe di `icon`) per renderle più grandi, e la proprietà {{cssxref("text-box")}} è utilizzata per rimuovere parte dell'ingombrante spaziatura ai bordi di inizio e fine blocco delle emoji icone, facendole allineare meglio con le etichette di testo:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option .icon {
  font-size: 1.6rem;
  text-box: trim-both cap alphabetic;
}
```

Il nostro esempio ora viene reso in questo modo:

{{EmbedLiveSample("third-render", "100%", "370px")}}

## Regolazione dei contenuti dell'opzione selezionata all'interno del pulsante select

Se si seleziona una qualsiasi opzione animale dagli ultimi esempi in tempo reale, si noterà un problema: le icone degli animali causano un aumento in altezza del pulsante select, il che cambia anche la posizione dell'icona del selettore, e non c'è spaziatura tra l'icona dell'opzione e l'etichetta.

Questo può essere risolto nascondendo l'icona quando è contenuta all'interno di `<selectedcontent>`, che rappresenta i contenuti dell'opzione `<option>` selezionata come appaiono all'interno del pulsante select. Nel nostro esempio, è nascosto usando {{cssxref("display", "display: none")}}:

```css live-sample___fourth-render live-sample___full-render
selectedcontent .icon {
  display: none;
}
```

Questo non influisce sulla stilizzazione dei contenuti `<option>` come appaiono all'interno del selettore a discesa.

## Stilizzazione dell'opzione attualmente selezionata

Per stilizzare l'opzione `<option>` attualmente selezionata così come appare all'interno del selettore a discesa, può targetizzarla usando la pseudo-classe {{cssxref(":checked")}}. Questo viene utilizzato per impostare il valore della {{cssxref("font-weight")}} dell'elemento `<option>` selezionato su `bold`:

```css live-sample___fourth-render live-sample___full-render
option:checked {
  font-weight: bold;
}
```

## Stilizzazione del segno di spunta della selezione corrente

Probabilmente avrà notato che quando apre il selettore per fare una selezione, l'opzione `<option>` attualmente selezionata ha un segno di spunta alla sua estremità di partenza in linea. Questo segno di spunta può essere targetizzato utilizzando il pseudo-elemento {{cssxref("::checkmark")}}. Ad esempio, potreste voler nascondere questo segno di spunta (ad esempio, tramite `display: none`).

Potrebbe anche scegliere di fare qualcosa di più interessante — in precedenza gli elementi `<option>` sono stati disposti orizzontalmente utilizzando flexbox, con gli elementi flessibili allineati all'inizio della riga. Nella regola sottostante, il segno di spunta è spostato dall'inizio della riga alla fine impostando un valore {{cssxref("order")}} superiore a `0`, e allineandolo alla fine della riga usando un valore {{cssxref("margin-left")}} `auto` (vedere [Allineamento e margini automatici](/it/docs/Web/CSS/CSS_box_alignment/Box_alignment_in_flexbox#alignment_and_auto_margins)).

Infine, viene impostato il valore della proprietà {{cssxref("content")}} su un'emoji diversa, per impostare un'icona diversa da visualizzare.

```css live-sample___fourth-render live-sample___full-render
option::checkmark {
  order: 1;
  margin-left: auto;
  content: "☑️";
}
```

> [!NOTE]
> Gli pseudo-elementi `::checkmark` e `::picker-icon` non sono inclusi nell'albero dell'accessibilità, quindi qualsiasi contenuto generato {{cssxref("content")}} impostato su di essi non sarà annunciato dalle tecnologie assistive. Dovrebbe comunque assicurarsi che qualsiasi nuova icona impostata abbia un senso visivo per il suo scopo previsto.

Controlliamo di nuovo come sta rendendo l'esempio. Lo stato aggiornato dopo le ultime tre sezioni è il seguente:

{{EmbedLiveSample("fourth-render", "100%", "410px")}}

## Animazione del selettore usando stati popover

Il pulsante select e il selettore a discesa dell'elemento `<select>` personalizzabile sono automaticamente assegnati a un rapporto invocatore/popover, come descritto in [Usare l'API Popover](/it/docs/Web/API/Popover_API/Using). Ci sono molti vantaggi che questo porta agli elementi `<select>`; il nostro esempio sfrutta la capacità di animare tra gli stati nascosti e visibili del popover usando transizioni. La pseudo-classe {{cssxref(":popover-open")}} rappresenta i popover nello stato visibile.

La tecnica è trattata rapidamente in questa sezione — legga [Animare popover](/it/docs/Web/API/Popover_API/Using#animating_popovers) per una descrizione più dettagliata.

Per prima cosa, il selettore viene selezionato usando `::picker(select)`, e viene dato un valore di {{cssxref("opacity")}} di `0` e un valore di `transition` di `all 0.4s allow-discrete`. Questo fa sì che tutte le proprietà che cambiano valore quando lo stato del popover cambia da nascosto a visibile vengano animate.

```css live-sample___full-render
::picker(select) {
  opacity: 0;
  transition: all 0.4s allow-discrete;
}
```

L'elenco delle proprietà in transizione include `opacity`, tuttavia include anche due proprietà discrete i cui valori sono impostati dagli stili predefiniti del browser:

- {{cssxref("display")}}
  - : I valori `display` cambiano da `none` a `block` quando il popover cambia stato da nascosto a mostrato. Questo deve essere animato per garantire che altre transizioni siano visibili.
- {{cssxref("overlay")}}
  - : Il valore `overlay` cambia da `none` ad `auto` quando il popover cambia stato da nascosto a mostrato, per promuoverlo al {{Glossary("top_layer", "top layer")}}, e poi di nuovo quando è nascosto per rimuoverlo. Questo deve essere animato per garantire che la rimozione del popover dal top layer sia differita finché la transizione non è completata, garantendo la transizione sia visibile.

> [!NOTE]
> Il valore [`allow-discrete`](/it/docs/Web/CSS/transition-behavior#allow-discrete) è necessario per abilitare l'animazione delle proprietà discrete.

Successivamente, il selettore viene selezionato nello stato visibile usando `::picker(select):popover-open` e viene dato un valore `opacity` di `1` — questo è lo stato finale della transizione:

```css live-sample___full-render
::picker(select):popover-open {
  opacity: 1;
}
```

Infine, poiché il selettore viene transizionato mentre si sposta da `display: none` a un valore `display` che lo rende visibile, lo stato iniziale della transizione deve essere specificato all'interno di un blocco {{cssxref("@starting-style")}}:

```css live-sample___full-render
@starting-style {
  ::picker(select):popover-open {
    opacity: 0;
  }
}
```

Queste regole lavorano insieme per far sì che il selettore sfumi dolcemente dentro e fuori quando il `<select>` si apre e si chiude.

## Posizionamento del selettore usando il posizionamento dell'ancora

Il pulsante select e il selettore a discesa di un elemento `<select>` personalizzabile hanno un riferimento implicito all'ancora, e il selettore è automaticamente associato con il pulsante select tramite [posizionamento dell'ancora CSS](/it/docs/Web/CSS/CSS_anchor_positioning). Questo significa che un'associazione esplicita non deve essere fatta usando le proprietà {{cssxref("anchor-name")}} e {{cssxref("position-anchor")}}.

Inoltre, [gli stili predefiniti del browser forniscono una posizione predefinita](/it/docs/Web/CSS/::picker#picker_anchor_positioning), che può personalizzare come spiegato in [Posizionamento degli elementi rispetto alla loro ancora](/it/docs/Web/CSS/CSS_anchor_positioning/Using#positioning_elements_relative_to_their_anchor).

Nel nostro demo, la posizione del selettore è impostata rispetto alla sua ancora utilizzando la funzione {{cssxref("anchor()")}} all'interno dei valori delle proprietà {{cssxref("top")}} e {{cssxref("left")}}:

```css live-sample___full-render
::picker(select) {
  top: calc(anchor(bottom) + 1px);
  left: anchor(10%);
}
```

Questo si traduce nel bordo superiore del selettore sempre posizionato 1 pixel sotto il bordo inferiore del pulsante select, e nel bordo sinistro del selettore sempre posizionato `10%` della larghezza del pulsante select rispetto al suo bordo sinistro.

## Risultato finale

Dopo le ultime due sezioni, lo stato aggiornato finale del nostro `<select>` viene reso in questo modo:

{{EmbedLiveSample("full-render", "100%", "410px")}}

## Personalizzazione di altre caratteristiche dei select classici

Le sezioni precedenti hanno coperto tutte le nuove funzionalità disponibili nei select personalizzabili, e mostrato come interagiscono sia con i select classici a linea singola, sia con funzionalità moderne correlate come i popover e il posizionamento dell'ancora. Ci sono alcune altre funzionalità dell'elemento `<select>` non menzionate sopra; questa sezione parla di come funzionano attualmente insieme ai select personalizzabili:

- [`<select multiple>`](/it/docs/Web/HTML/Reference/Attributes/multiple)
  - : Attualmente non c'è alcun supporto specificato per l'attributo `multiple` sugli elementi `<select>` personalizzabili, ma su questo si lavorerà in futuro.
- {{htmlelement("optgroup")}}
  - : Lo stile predefinito degli elementi `<optgroup>` è lo stesso degli elementi `<select>` classici — in grassetto e rientrato meno delle opzioni contenute. Deve assicurarsi di stilizzare gli elementi `<optgroup>` in modo che si adattino al design complessivo, e tenere presente che si comporteranno proprio come ci si aspetterebbe che i contenitori si comportino in HTML convenzionale. Negli elementi `<select>` personalizzabili, è consentito l'elemento {{htmlelement("legend")}} come figlio di `<optgroup>`, per fornire un'etichetta che è facile da targetizzare e stilizzare. Questo sostituisce qualsiasi testo impostato nell'attributo `label` dell'elemento `<optgroup>`, e ha la stessa semantica.

## Prossimo passo

Nel prossimo articolo di questo modulo, esploreremo le diverse [pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes) disponibili nei browser moderni per stilizzare moduli in diversi stati.

## Vedi anche

- {{htmlelement("select")}}, {{htmlelement("option")}}, {{htmlelement("optgroup")}}, {{htmlelement("label")}}, {{htmlelement("button")}}, {{htmlelement("selectedcontent")}}
- {{cssxref("appearance")}}
- {{cssxref("::picker()", "::picker(select)")}}, {{cssxref("::picker-icon")}}, {{cssxref("::checkmark")}}
- {{cssxref(":open")}}, {{cssxref(":checked")}}

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Advanced_form_styling", "Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms")}}
