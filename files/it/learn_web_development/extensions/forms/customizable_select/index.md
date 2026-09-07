---
title: Elementi select personalizzabili
short-title: Select personalizzabili
slug: Learn_web_development/Extensions/Forms/Customizable_select
l10n:
  sourceCommit: a25c283c3fe8986e9d31f0bb64b345ad1bb7b64d
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Advanced_form_styling", "Learn_web_development/Extensions/Forms/Customizable_select_listboxes", "Learn_web_development/Extensions/Forms")}}

Questo articolo spiega come creare elementi {{htmlelement("select")}} completamente personalizzati usando funzionalità sperimentali del browser. Ciò include il controllo completo dello stile del pulsante select, del selettore a discesa, dell'icona a freccia, del segno di spunta della selezione corrente e di ogni singolo elemento {{htmlelement("option")}}.

> [!WARNING]
> Le funzionalità CSS e HTML illustrate in questo articolo hanno attualmente un supporto limitato nei browser; per maggiori dettagli, controllare le tabelle di compatibilità del browser nelle pagine di riferimento delle singole funzionalità. Alcuni framework JavaScript bloccano queste funzionalità; in altri, causano errori di hydration quando è abilitato il Server-Side Rendering (SSR).

## Contesto

Tradizionalmente è stato difficile personalizzare l'aspetto e il comportamento degli elementi `<select>` perché contengono componenti interni il cui stile è definito a livello di sistema operativo e che non possono essere selezionati usando CSS. Ciò include il selettore a discesa, l'icona a freccia e così via.

In precedenza, la migliore opzione disponibile — oltre a usare una libreria JavaScript personalizzata — era impostare un valore {{cssxref("appearance")}} di `none` sull'elemento `<select>` per rimuovere parte dello stile a livello di sistema operativo, quindi usare CSS per personalizzare le parti che possono essere stilizzate. Questa tecnica è spiegata in [Stile avanzato dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling).

Gli elementi `<select>` personalizzabili forniscono una soluzione a questi problemi. Consentono di creare esempi come il seguente, usando solo HTML e CSS, completamente personalizzati nei [browser supportati](#compatibilità_del_browser). Ciò include il layout di `<select>` e del selettore a discesa, la combinazione di colori, le icone, il carattere, le transizioni, il posizionamento, gli indicatori dell'icona selezionata e altro ancora.

{{EmbedLiveSample("full-render", "100%", "410px")}}

Inoltre, forniscono un miglioramento progressivo rispetto alla funzionalità esistente, ricorrendo ai select "classici" nei browser che non li supportano.

Nelle sezioni seguenti verrà illustrato come creare questo esempio.

> [!NOTE]
> Questo articolo illustra il contesto degli elementi select personalizzabili e mostra come creare select "a menu a discesa singolo" che sfruttano queste funzionalità, ovvero menu a discesa che mostrano una singola opzione alla volta e consentono di selezionare una sola opzione.
>
> Per informazioni sulla creazione di select "listbox" — menu che mostrano più opzioni contemporaneamente e consentono di selezionare una o più opzioni — vedere [Listbox select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select_listboxes).

## Quali funzionalità costituiscono un select personalizzabile?

È possibile creare elementi `<select>` personalizzabili usando le seguenti funzionalità HTML e CSS:

- I classici elementi {{htmlelement("select")}}, {{htmlelement("option")}} e {{htmlelement("optgroup")}}. Funzionano esattamente come nei select "classici", tranne per il fatto che dispongono di ulteriori tipi di contenuto consentiti.
- Un elemento {{htmlelement("button")}} incluso come primo figlio all'interno dell'elemento `<select>`, che in precedenza non era consentito nei select "classici". Quando incluso, sostituisce il rendering predefinito del "pulsante" dell'elemento `<select>` chiuso. Questo è comunemente noto come **pulsante select** (poiché è il pulsante da premere per aprire il selettore a discesa).
  > [!NOTE]
  > Il pulsante select è [inert](/it/docs/Web/HTML/Reference/Global_attributes/inert) per impostazione predefinita, pertanto, se al suo interno sono inclusi figli interattivi (ad esempio, link o pulsanti), verrà comunque trattato come un singolo pulsante ai fini dell'interazione: per esempio, gli elementi figli non potranno ricevere il focus né essere selezionati con un clic.
- L'elemento {{htmlelement("selectedcontent")}} può essere facoltativamente incluso nel primo elemento figlio `<button>` dell'elemento `<select>` per visualizzare il valore attualmente selezionato all'interno dell'elemento `<select>` _chiuso_.
  Questo contiene un clone del contenuto dell'elemento `<option>` attualmente selezionato (creato internamente usando [`cloneNode()`](/it/docs/Web/API/Node/cloneNode)).
- Lo pseudo-elemento {{cssxref("::picker()", "::picker(select)")}}, che seleziona l'intero contenuto del selettore. Ciò include tutti gli elementi all'interno dell'elemento `<select>`, eccetto il primo elemento figlio `<button>`.
- Il valore `base-select` della proprietà {{cssxref("appearance")}}, che abilita sull'elemento `<select>` e sullo pseudo-elemento `::picker(select)` gli stili e il comportamento predefiniti del browser per i select personalizzabili.
- La pseudo-classe {{cssxref(":open")}}, che seleziona il pulsante select quando il selettore (`::picker(select)`) è aperto.
- Lo pseudo-elemento {{cssxref("::picker-icon")}}, che seleziona l'icona all'interno del pulsante select, ovvero la freccia rivolta verso il basso quando il select è chiuso.
- La pseudo-classe {{cssxref(":checked")}}, che seleziona l'elemento `<option>` attualmente selezionato.
- Lo pseudo-elemento {{cssxref("::checkmark")}}, che seleziona il segno di spunta inserito nell'elemento `<option>` attualmente selezionato per fornire un'indicazione visiva di quale sia selezionato.

Inoltre, l'elemento `<select>` e il suo selettore a discesa dispongono di un riferimento ad ancora implicito, il che significa che il selettore viene associato automaticamente all'elemento `<select>` tramite il [posizionamento con ancore CSS](/it/docs/Web/CSS/Guides/Anchor_positioning). Gli stili predefiniti del browser posizionano il selettore rispetto al pulsante (l'ancora) ed è possibile personalizzare questa posizione come spiegato in [Posizionare elementi rispetto alla propria ancora](/it/docs/Web/CSS/Guides/Anchor_positioning/Using#positioning_elements_relative_to_their_anchor). Gli stili predefiniti del browser definiscono inoltre alcuni fallback position-try che riposizionano il selettore se rischia di fuoriuscire dalla viewport. I fallback position-try sono illustrati in [Gestire l'overflow: fallback try e nascondimento condizionale](/it/docs/Web/CSS/Guides/Anchor_positioning/Try_options_hiding).

> [!NOTE]
> È possibile verificare il supporto del browser per `<select>` personalizzabili consultando le tabelle di compatibilità del browser nelle pagine di riferimento delle funzionalità correlate, come {{htmlelement("selectedcontent")}}, {{cssxref("::picker()", "::picker(select)")}} e {{cssxref("::checkmark")}}.

Vediamo tutte le funzionalità descritte sopra in azione, analizzando l'esempio mostrato all'inizio della pagina.

## Markup di un select personalizzabile

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
        <span class="icon" aria-hidden="true">🐱</span>
        <span class="option-label">Cat</span>
      </option>
      <option value="dog">
        <span class="icon" aria-hidden="true">🐶</span>
        <span class="option-label">Dog</span>
      </option>
      <option value="hamster">
        <span class="icon" aria-hidden="true">🐹</span>
        <span class="option-label">Hamster</span>
      </option>
      <option value="chicken">
        <span class="icon" aria-hidden="true">🐔</span>
        <span class="option-label">Chicken</span>
      </option>
      <option value="fish">
        <span class="icon" aria-hidden="true">🐟</span>
        <span class="option-label">Fish</span>
      </option>
      <option value="snake">
        <span class="icon" aria-hidden="true">🐍</span>
        <span class="option-label">Snake</span>
      </option>
    </select>
  </p>
</form>
```

> [!NOTE]
> L'attributo [`aria-hidden="true"`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden) è incluso nelle icone affinché vengano nascoste alle tecnologie assistive, evitando che i valori delle opzioni vengano annunciati due volte (ad esempio, "gatto gatto").

Il markup dell'esempio è quasi uguale a quello di un `<select>` "classico", con le seguenti differenze:

- La struttura `<button><selectedcontent></selectedcontent></button>` rappresenta il {{htmlelement("button")}} select.
  L'aggiunta dell'elemento {{htmlelement("selectedcontent")}} fa sì che il browser cloni l'elemento {{htmlelement("option")}} attualmente selezionato all'interno del pulsante, per il quale è quindi possibile [fornire stili personalizzati](#regolare_lo_stile_del_contenuto_dell'opzione_selezionata_all'interno_del_pulsante_select). Se questa struttura non è inclusa nel markup, il browser ricorre al rendering del testo dell'opzione selezionata all'interno del pulsante predefinito, che non sarà possibile stilizzare altrettanto facilmente.
  > [!NOTE]
  > È possibile includere contenuto arbitrario all'interno di `<button>` per visualizzare qualsiasi cosa all'interno del `<select>` chiuso, ma occorre prestare attenzione. Il contenuto incluso può alterare il valore accessibile esposto alle tecnologie assistive per l'elemento `<select>`.
- Il resto del contenuto di `<select>` rappresenta il selettore a discesa, che in genere è limitato agli elementi `<option>` che rappresentano le diverse scelte nel selettore. È possibile includere altro contenuto nel selettore, ma non è consigliato.
- Tradizionalmente, gli elementi `<option>` potevano contenere solo testo, ma in un select personalizzabile è possibile includere altre strutture di markup come immagini, altri elementi semantici non interattivi a livello di testo e altro ancora. È persino possibile usare gli pseudo-elementi {{cssxref("::before")}} e {{cssxref("::after")}} per includere altro contenuto, tenendo però presente che questo non verrebbe incluso nel valore inviabile. Nel nostro esempio, ogni `<option>` contiene due elementi {{htmlelement("span")}} contenenti rispettivamente un'icona e un'etichetta di testo, consentendo di stilizzare e posizionare ciascuno di essi in modo indipendente.

  > [!NOTE]
  > Poiché il contenuto di `<option>` può contenere sottoalberi DOM a più livelli, e non solo nodi di testo, esistono regole relative al modo in cui il browser deve estrarre tramite JavaScript il [valore `<select>` corrente](/it/docs/Web/API/HTMLSelectElement/value). Viene recuperato il valore della proprietà [`textContent`](/it/docs/Web/API/Node/textContent) dell'elemento `<option>` selezionato, su di esso viene eseguito {{jsxref("String.prototype.trim", "trim()")}} e il risultato viene impostato come valore di `<select>`.

Questo design consente ai browser non supportati di ricorrere a un'esperienza `<select>` classica. La struttura `<button><selectedcontent></selectedcontent></button>` verrà ignorata completamente e il contenuto non testuale di `<option>` verrà rimosso, lasciando soltanto il contenuto dei nodi di testo, ma il risultato continuerà a funzionare.

## Abilitare il rendering del select personalizzato

Per abilitare la funzionalità select personalizzata e gli stili base minimi del browser (e rimuovere lo stile fornito dal sistema operativo), l'elemento `<select>` e il suo selettore a discesa (rappresentato dallo pseudo-elemento `::picker(select)`) devono entrambi avere impostato un valore {{cssxref("appearance")}} di `base-select`:

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
  font-family: "Helvetica", "Arial", sans-serif;
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

È possibile abilitare solo l'elemento `<select>` alla nuova funzionalità, lasciando il selettore con lo stile predefinito del sistema operativo, ma nella maggior parte dei casi sarà opportuno abilitare entrambi. Non è possibile abilitare il selettore senza abilitare l'elemento `<select>`.

Una volta fatto, il risultato è un rendering molto semplice di un elemento `<select>`:

{{EmbedLiveSample("plain-render", "100%", "240px")}}

Ora è possibile stilizzarlo in qualsiasi modo. Per iniziare, l'elemento `<select>` ha valori personalizzati di {{cssxref("border")}}, {{cssxref("background")}} (che cambia con {{cssxref(":hover")}} o {{cssxref(":focus")}}) e {{cssxref("padding")}}, oltre a una {{cssxref("transition")}} affinché il cambio di sfondo venga animato fluidamente:

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select {
  border: 2px solid #dddddd;
  background: #eeeeee;
  padding: 10px;
  transition: 0.4s;
}

select:hover,
select:focus {
  background: #dddddd;
}
```

## Stilizzare l'icona del selettore

Per stilizzare l'icona all'interno del pulsante select, ovvero la freccia rivolta verso il basso quando il select è chiuso, è possibile selezionarla con lo pseudo-elemento {{cssxref("::picker-icon")}}. Il codice seguente assegna all'icona un valore {{cssxref("color")}} personalizzato e una `transition` affinché le modifiche alla sua proprietà {{cssxref("rotate")}} siano animate fluidamente:

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select::picker-icon {
  color: #999999;
  transition: 0.4s rotate;
}
```

Successivamente, `::picker-icon` viene combinato con la pseudo-classe {{cssxref(":open")}}, che seleziona il pulsante select solo quando il selettore a discesa è aperto, per assegnare all'icona un valore `rotate` di `180deg` quando `<select>` viene aperto.

```css live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render
select:open::picker-icon {
  rotate: 180deg;
}
```

Vediamo il lavoro svolto finora: notare come la freccia del selettore ruoti fluidamente di 180 gradi quando `<select>` si apre e si chiude:

{{EmbedLiveSample("second-render", "100%", "250px")}}

## Stilizzare il selettore a discesa

Il selettore a discesa può essere selezionato usando lo pseudo-elemento {{cssxref("::picker()", "::picker(select)")}}. Come menzionato in precedenza, il selettore contiene tutto ciò che è incluso nell'elemento `<select>` tranne il pulsante e `<selectedcontent>`. Nel nostro esempio, ciò significa tutti gli elementi `<option>` e il loro contenuto.

Il selettore è un [popover](/it/docs/Web/API/Popover_API). Pertanto, quando viene aperto, il suo contenuto (contenuto nello pseudo-elemento `::picker(select)`) viene promosso nel {{Glossary("top_layer", "livello superiore")}}. Ciò garantisce che il selettore venga visualizzato sopra gli altri elementi e interagisca correttamente con gli altri popover nella pagina, ad esempio chiudendo popover non correlati già aperti.

> [!NOTE]
> Il selettore a discesa del select è inoltre soggetto al comportamento del [confine di corrispondenza degli antenati del livello superiore](/it/docs/Web/CSS/Reference/Selectors/Pseudo-classes#top-layer_ancestor_matching_boundary). Ciò assicura che gli stili {{cssxref(":hover")}}, {{cssxref(":active")}} o {{cssxref(":focus-within")}} applicati a `<select>` corrispondano ai discendenti del selettore solo mentre si interagisce con quest'ultimo, non all'elemento `<select>` stesso.

Nel nostro esempio, iniziamo rimuovendo il {{cssxref("border")}} nero predefinito del selettore:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
::picker(select) {
  border: none;
}
```

> [!NOTE]
> L'argomento passato allo pseudo-elemento `::picker()` rappresenta il tipo di elemento di cui si desidera selezionare il selettore, in questo caso gli elementi `<select>`. Se si desidera selezionare il selettore di uno specifico elemento `<select>` anziché di tutti, è possibile combinare lo pseudo-elemento `::picker()` con un altro selettore. Ad esempio, l'elemento `<select>` del nostro esempio ha un ID `pet-select`, quindi il suo selettore può essere selezionato esclusivamente con `#pet-select::picker(select) { ... }`.

Ora vengono stilizzati gli elementi `<option>`. Sono disposti con [flexbox](/it/docs/Web/CSS/Guides/Flexible_box_layout), allineandoli tutti all'inizio del contenitore flex e includendo un {{cssxref("gap")}} di `20px` tra ciascuno. A ogni `<option>` vengono inoltre assegnati gli stessi valori di {{cssxref("border")}}, {{cssxref("background")}}, {{cssxref("padding")}} e {{cssxref("transition")}} di `<select>`, per fornire un aspetto coerente:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option {
  display: flex;
  justify-content: flex-start;
  gap: 20px;

  border: 2px solid #dddddd;
  background: #eeeeee;
  padding: 10px;
  transition: 0.4s;
}
```

> [!NOTE]
> Agli elementi `<option>` degli elementi `<select>` personalizzabili è impostato `display: flex` per impostazione predefinita, ma è comunque incluso nel nostro foglio di stile per chiarire cosa accade.

Successivamente, una combinazione delle pseudo-classi {{cssxref(":first-of-type")}}, {{cssxref(":last-of-type")}} e {{cssxref(":not()")}} viene usata per impostare un {{cssxref("border-radius")}} appropriato sugli elementi `<option>` superiore e inferiore e rimuovere {{cssxref("border-bottom")}} da tutti gli elementi `<option>`, tranne l'ultimo, affinché i bordi non appaiano disordinati e sovrapposti. Impostiamo inoltre lo stesso `border-radius` sul contenitore esterno `::picker(select)`, così da non ritrovarci con un antiestetico riquadro bianco quadrato attorno alle opzioni se decidiamo di impostare un background-color diverso nella pagina.

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option:first-of-type {
  border-radius: 8px 8px 0 0;
}

option:last-of-type {
  border-radius: 0 0 8px 8px;
}

::picker(select) {
  border-radius: 8px;
}

option:not(option:last-of-type) {
  border-bottom: none;
}
```

Successivamente, viene impostato un colore `background` diverso sugli elementi `<option>` dispari usando {{cssxref(":nth-of-type()", ":nth-of-type(odd)")}} per implementare le righe zebrate, e un colore `background` diverso sugli elementi `<option>` con focus e hover per fornire un'evidenziazione visiva utile durante la selezione:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option:nth-of-type(odd) {
  background: white;
}

option:hover,
option:focus {
  background: plum;
}
```

Infine, per questa sezione, viene impostato un {{cssxref("font-size")}} più grande sulle icone `<option>` (contenute negli elementi `<span>` con classe `icon`) per renderle più grandi, e la proprietà {{cssxref("text-box")}} viene usata per rimuovere parte della fastidiosa spaziatura ai bordi block-start e block-end delle emoji delle icone, facendole allineare meglio alle etichette di testo:

```css live-sample___third-render live-sample___fourth-render live-sample___full-render
option .icon {
  font-size: 1.6rem;
  text-box: trim-both cap alphabetic;
}
```

Il nostro esempio ora viene renderizzato così:

{{EmbedLiveSample("third-render", "100%", "370px")}}

## Regolare lo stile del contenuto dell'opzione selezionata all'interno del pulsante select

Se viene selezionata una qualsiasi opzione animale dagli ultimi esempi live, si noterà un problema: le icone degli animali fanno aumentare l'altezza del pulsante select, modificando anche la posizione dell'icona del selettore, e non è presente alcuno spazio tra l'icona dell'opzione e l'etichetta.

Questo problema può essere risolto nascondendo l'icona quando è contenuta in `<selectedcontent>`, che rappresenta il contenuto dell'elemento `<option>` selezionato così come appare all'interno del pulsante select. Nel nostro esempio, viene nascosta usando {{cssxref("display", "display: none")}}:

```css live-sample___fourth-render live-sample___full-render
selectedcontent .icon {
  display: none;
}
```

Ciò non influisce sullo stile del contenuto di `<option>` così come appare all'interno del selettore a discesa.

## Stilizzare l'opzione attualmente selezionata

Per stilizzare l'elemento `<option>` attualmente selezionato così come appare all'interno del selettore a discesa, è possibile selezionarlo usando la pseudo-classe {{cssxref(":checked")}}. Viene usata per impostare il {{cssxref("font-weight")}} dell'elemento `<option>` selezionato su `bold`:

```css live-sample___fourth-render live-sample___full-render
option:checked {
  font-weight: bold;
}
```

## Stilizzare il segno di spunta della selezione corrente

Probabilmente è stato notato che, quando si apre il selettore per effettuare una selezione, l'elemento `<option>` attualmente selezionato presenta un segno di spunta al suo bordo inline-start. Questo segno di spunta può essere selezionato usando lo pseudo-elemento {{cssxref("::checkmark")}}. Ad esempio, potrebbe essere opportuno nascondere questo segno di spunta, ad esempio tramite `display: none`.

È anche possibile scegliere di fare qualcosa di più interessante: in precedenza gli elementi `<option>` sono stati disposti orizzontalmente usando flexbox, con gli elementi flex allineati all'inizio della riga. Nella regola seguente, il segno di spunta viene spostato dall'inizio della riga alla fine impostando su di esso un valore {{cssxref("order")}} maggiore di `0` e allineandolo alla fine della riga usando un valore {{cssxref("margin-left")}} `auto` (vedere [Allineamento e margini automatici](/it/docs/Web/CSS/Guides/Box_alignment/In_flexbox#alignment_and_auto_margins)).

Infine, il valore della proprietà {{cssxref("content")}} viene impostato su un'emoji diversa, per impostare un'icona differente da visualizzare.

```css live-sample___fourth-render live-sample___full-render
option::checkmark {
  order: 1;
  margin-left: auto;
  content: "☑️";
}
```

> [!NOTE]
> Gli pseudo-elementi `::checkmark` e `::picker-icon` non sono inclusi nell'albero di accessibilità, pertanto qualsiasi {{cssxref("content")}} generato impostato su di essi non verrà annunciato dalle tecnologie assistive. Occorre comunque assicurarsi che ogni nuova icona impostata abbia visivamente senso per lo scopo previsto.

Verifichiamo nuovamente come viene renderizzato l'esempio. Lo stato aggiornato dopo le ultime tre sezioni è il seguente:

{{EmbedLiveSample("fourth-render", "100%", "410px")}}

## Animare il selettore usando gli stati popover

Il `button` select e il selettore a discesa di un elemento `<select>` personalizzabile ricevono automaticamente una relazione invoker/popover, come descritto in [Usare la Popover API](/it/docs/Web/API/Popover_API/Using). Ci sono molti vantaggi che ciò apporta agli elementi `<select>`; il nostro esempio sfrutta la possibilità di animare tra gli stati nascosto e visualizzato del popover usando le transizioni. La pseudo-classe {{cssxref(":open")}} rappresenta gli elementi select in stato aperto.

La tecnica viene illustrata rapidamente in questa sezione: leggere [Animare i popover](/it/docs/Web/API/Popover_API/Using#animating_popovers) per una descrizione più dettagliata.

Prima di tutto, il selettore viene selezionato usando `::picker(select)` e gli viene assegnato un valore {{cssxref("opacity")}} di `0` e un valore `transition` di `all 0.4s allow-discrete`. Questo fa sì che tutte le proprietà che cambiano valore quando lo stato del popover passa da nascosto a visualizzato vengano animate.

```css live-sample___full-render
::picker(select) {
  opacity: 0;
  transition: all 0.4s allow-discrete;
}
```

L'elenco delle proprietà sottoposte a transizione include `opacity`, ma include anche due proprietà discrete i cui valori sono impostati dagli stili predefiniti del browser:

- {{cssxref("display")}}
  - : I valori di `display` cambiano da `none` a `block` quando il popover passa dallo stato nascosto a quello visualizzato. Questa proprietà deve essere animata per garantire che le altre transizioni siano visibili.
- {{cssxref("overlay")}}
  - : Il valore di `overlay` cambia da `none` a `auto` quando il popover passa dallo stato nascosto a quello visualizzato, per promuoverlo al livello superiore, quindi torna indietro quando viene nascosto per rimuoverlo. Questa proprietà deve essere animata per garantire che la rimozione del popover dal livello superiore venga posticipata fino al completamento della transizione, assicurando che la transizione sia visibile.

> [!NOTE]
> Il valore [`allow-discrete`](/it/docs/Web/CSS/Reference/Properties/transition-behavior#allow-discrete) è necessario per abilitare le animazioni delle proprietà discrete.

Successivamente, il selettore viene selezionato nello stato visualizzato usando `:open::picker(select)` e gli viene assegnato un valore `opacity` di `1`: questo è lo stato finale della transizione:

```css live-sample___full-render
:open::picker(select) {
  opacity: 1;
}
```

Infine, poiché il selettore viene sottoposto a transizione mentre passa da `display: none` a un valore `display` che lo rende visibile, lo stato iniziale della transizione deve essere specificato all'interno di un blocco {{cssxref("@starting-style")}}:

```css live-sample___full-render
@starting-style {
  :open::picker(select) {
    opacity: 0;
  }
}
```

Queste regole lavorano insieme affinché il selettore appaia e scompaia gradualmente quando `<select>` viene aperto e chiuso.

## Posizionare il selettore usando il posizionamento con ancore

Il pulsante select e il selettore a discesa di un elemento `<select>` personalizzabile dispongono di un riferimento ad ancora implicito, e il selettore viene associato automaticamente al pulsante select tramite il [posizionamento con ancore CSS](/it/docs/Web/CSS/Guides/Anchor_positioning). Ciò significa che non è necessario effettuare un'associazione esplicita usando le proprietà {{cssxref("anchor-name")}} e {{cssxref("position-anchor")}}.

Inoltre, gli [stili predefiniti del browser forniscono una posizione predefinita](/it/docs/Web/CSS/Reference/Selectors/::picker#picker_anchor_positioning), che è possibile personalizzare come spiegato in [Posizionare elementi rispetto alla propria ancora](/it/docs/Web/CSS/Guides/Anchor_positioning/Using#positioning_elements_relative_to_their_anchor).

Nella nostra demo, la posizione del selettore viene impostata rispetto alla sua ancora usando la funzione {{cssxref("anchor()")}} all'interno dei valori delle proprietà {{cssxref("top")}} e {{cssxref("left")}}:

```css live-sample___full-render
::picker(select) {
  top: calc(anchor(bottom) + 1px);
  left: anchor(10%);
}
```

Il risultato è che il bordo superiore del selettore viene sempre posizionato 1 pixel sotto il bordo inferiore del pulsante select, mentre il bordo sinistro del selettore viene sempre posizionato a una distanza pari al `10%` della larghezza del pulsante select dal suo bordo sinistro.

> [!NOTE]
> Se si desidera rimuovere il riferimento ad ancora implicito per impedire che il selettore sia ancorato all'elemento `<select>`, è possibile farlo impostando la proprietà `position-anchor` del selettore su un nome di ancora che non esiste nel documento corrente, come `--not-an-anchor-name`. Vedere anche [rimuovere un'associazione ad ancora](/it/docs/Web/CSS/Guides/Anchor_positioning/Using#removing_an_anchor_association).

## Risultato finale dell'esempio principale

Dopo le ultime due sezioni, lo stato finale aggiornato del nostro `<select>` viene renderizzato così:

{{EmbedLiveSample("full-render", "100%", "410px")}}

## Stilizzare gli elementi optgroup

Lo stile predefinito degli elementi {{htmlelement("optgroup")}} nei select personalizzabili è lo stesso degli elementi `<select>` classici: in grassetto e con un rientro inferiore rispetto alle opzioni contenute. Nei select personalizzabili, tuttavia, i gruppi di opzioni si comportano come qualsiasi altro contenitore a livello di blocco e possono essere stilizzati di conseguenza. Inoltre, l'elemento {{htmlelement("legend")}} è consentito come figlio di `<optgroup>`, per fornire un'etichetta facile da selezionare e stilizzare. Questo sostituisce qualsiasi testo impostato nell'attributo `label` dell'elemento `<optgroup>` e ha la stessa semantica.

Vediamo un esempio di base. Il nostro HTML è simile a questo:

```html live-sample___optgroup-example
<label for="animal-select">Select animal:</label><br />
<select id="animal-select">
  <optgroup>
    <legend>Domestic</legend>
    <option value="cat">Cat</option>
    <option value="dog">Dog</option>
    <option value="guinea">Guinea pig</option>
  </optgroup>
  <optgroup>
    <legend>Farm</legend>
    <option value="chicken">Chicken</option>
    <option value="cow">Cow</option>
    <option value="pig">Pig</option>
  </optgroup>
</select>
```

Iniziamo il CSS stilizzando gli elementi `<optgroup>` stessi. Sono per lo più stili rudimentali per far apparire gli elementi optgroup come contenitori per i loro elementi `<option>` discendenti. Sono stati assegnati alcuni {{cssxref("margin-top")}} per inserire spazio tra ogni optgroup e tra l'optgroup superiore e il pulsante select.

```css hidden live-sample___optgroup-example
* {
  box-sizing: border-box;
}

html {
  font-family: "Arial", sans-serif;
}

select,
::picker(select) {
  appearance: base-select;
  width: 200px;
}

select {
  border: 2px solid #dddddd;
  background: #eeeeee;
  padding: 10px;
}

::picker(select) {
  border: none;
}
```

```css live-sample___optgroup-example
optgroup {
  border: 2px solid #dddddd;
  border-radius: 8px;
  background: #eeeeee;
  padding: 10px 0 0 0;
  margin-top: 5px;
}
```

Successivamente, stilizziamo gli elementi `<legend>`, allineando il testo al centro e includendo alcuni margini per separarli dalle opzioni.

```css live-sample___optgroup-example
optgroup legend {
  text-align: center;
  margin-bottom: 10px;
}
```

Infine, stilizziamo gli elementi `<option>`, fornendo un colore {{cssxref("background")}}, {{cssxref("padding")}} e stilizzando il {{cssxref("border-radius")}} inferiore dell'ultimo `<option>` in ciascun caso affinché si adatti agli angoli arrotondati dell'elemento `<optgroup>` padre. Implementiamo inoltre le righe zebrate assegnando un colore di sfondo diverso agli elementi `<option>` dispari e forniamo uno stato hover e focus distinto per le opzioni.

```css live-sample___optgroup-example
option {
  background: #eeeeee;
  padding: 10px;
}

option:last-of-type {
  border-radius: 0 0 8px 8px;
}

option:nth-of-type(odd) {
  background: white;
}

option:hover,
option:focus {
  background: plum;
}
```

Il resto degli stili è stato nascosto per brevità.

L'esempio viene renderizzato così:

{{EmbedLiveSample("optgroup-example", "100%", "410px")}}

```css hidden live-sample___plain-render live-sample___second-render live-sample___third-render live-sample___fourth-render live-sample___full-render live-sample___optgroup-example
@supports not (appearance: base-select) {
  body::before {
    content: "Your browser does not support `appearance: base-select`.";
    color: black;
    background-color: wheat;
    position: fixed;
    left: 0;
    right: 0;
    top: 40%;
    text-align: center;
    padding: 1rem 0;
    z-index: 1;
  }
}
```

## Compatibilità del browser

{{Compat}}

## Passaggio successivo

Nel prossimo articolo di questo modulo verrà mostrato come stilizzare le [Listbox select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select_listboxes).

## Vedere anche

- {{htmlelement("select")}}, {{htmlelement("option")}}, {{htmlelement("optgroup")}}, {{htmlelement("label")}}, {{htmlelement("button")}}, {{htmlelement("selectedcontent")}}
- {{cssxref("appearance")}}
- {{cssxref("::picker()", "::picker(select)")}}, {{cssxref("::picker-icon")}}, {{cssxref("::checkmark")}}
- {{cssxref(":open")}}, {{cssxref(":checked")}}

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Advanced_form_styling", "Learn_web_development/Extensions/Forms/Customizable_select_listboxes", "Learn_web_development/Extensions/Forms")}}
