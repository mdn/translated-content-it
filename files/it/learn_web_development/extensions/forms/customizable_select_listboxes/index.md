---
title: Caselle di riepilogo select personalizzabili
short-title: Caselle di riepilogo personalizzabili
slug: Learn_web_development/Extensions/Forms/Customizable_select_listboxes
l10n:
  sourceCommit: 09d8ff096be97b28ea415fc4c68fb1cff0ff8af9
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms")}}

Questo articolo prosegue il precedente e illustra come applicare stili agli elementi {{htmlelement("select")}} di tipo listbox personalizzabili.

Uno dei principali vantaggi delle listbox `<select>` personalizzabili rispetto alle listbox select "classiche" è che è possibile applicare completamente gli stili a tutte le parti del controllo e includere al loro interno una varietà molto più ampia di elementi figli, offrendo quindi maggiore flessibilità in termini di design e funzionalità.

## Listbox select e select a discesa

Nell'articolo precedente abbiamo parlato degli elementi `<select>` "a discesa", ovvero controlli dotati di un pulsante che, quando viene premuto, mostra un selettore a discesa dal quale è possibile scegliere un'opzione. Questi vengono specificati tramite HTML di base, ad esempio `<select>`.

Gli elementi `<select>` di tipo "listbox", al contrario, sono controlli caratterizzati da una casella che mostra più opzioni contemporaneamente, dalle quali è possibile selezionare una o più opzioni. È possibile attivare il rendering di una select "listbox" specificando l'attributo `multiple` (per consentire selezioni multiple) e/o un valore `size` maggiore di `1`. Ad esempio, `<select multiple>` o `<select size="3">`.

Il seguente esempio dal vivo illustra la differenza:

```html hidden live-sample___select-comparison
<form>
  <p>
    <label for="pet-select">Select pet dropdown:</label><br />
    <select id="pet-select">
      <option value="cat">Cat</option>
      <option value="dog">Dog</option>
      <option value="chicken">Chicken</option>
      <option value="fish">Fish</option>
      <option value="Hamster">Hamster</option>
    </select>
  </p>
  <p>
    <label for="pet-select2">Select pets listbox:</label><br />
    <select id="pet-select2" multiple>
      <option value="cat">Cat</option>
      <option value="dog">Dog</option>
      <option value="chicken">Chicken</option>
      <option value="fish">Fish</option>
      <option value="hamster">Hamster</option>
    </select>
  </p>
</form>
```

```css hidden live-sample___select-comparison
select,
::picker(select) {
  appearance: base-select;
}

form {
  display: flex;
  gap: 100px;
  justify-content: center;
}
```

{{EmbedLiveSample("select-comparison", "100%", "200px")}}

> [!NOTE]
> L'attributo `multiple`, così come qualsiasi valore `size` maggiore di `1`, attiva la modalità listbox per l'elemento `<select>`.

### Come si confrontano le listbox personalizzabili con i menu a discesa personalizzabili?

Una `<select>` listbox personalizzabile è più semplice da stilizzare rispetto alla variante a discesa:

- Non esiste un selettore a discesa, quindi non è necessario preoccuparsi di applicargli stili tramite il pseudo-elemento {{cssxref("::picker()", "::picker(select)")}} o i relativi stati {{cssxref(":open")}} e chiuso.
- Non è necessario preoccuparsi di applicare stili all'icona del pulsante select tramite {{cssxref("::picker-icon")}}, né di manipolare il modo in cui l'`<option>` attualmente selezionato viene mostrato all'interno del pulsante usando l'elemento {{htmlelement("selectedcontent")}}.
- È coinvolto un solo contenitore; non è necessario preoccuparsi della posizione del selettore rispetto al pulsante.

## Una listbox personalizzata di base

Esaminiamo un esempio di base per mostrare come viene implementata una listbox personalizzata. Il markup di questo esempio è il seguente:

```html live-sample___basic-listbox live-sample___expanding-listbox
<p>
  <label for="pet-select">Select pets:</label><br />
  <select id="pet-select" multiple>
    <option value="cat">Cat</option>
    <option value="dog">Dog</option>
    <option value="chicken">Chicken</option>
    <option value="fish">Fish</option>
    <option value="hamster">Hamster</option>
  </select>
</p>
```

Non vi è nulla di particolare. Si noti che la listbox viene resa usando `<select multiple>` anziché `<select size="3">`. L'unica differenza è che è possibile selezionare più opzioni anziché una singola opzione. Lo stile funziona esattamente nello stesso modo.

Si inizia lo stile attivando lo stile personalizzato per `<select>` con un valore {{cssxref("appearance")}} di `base-select`:

```css hidden live-sample___basic-listbox live-sample___expanding-listbox live-sample___horizontal-listbox
* {
  box-sizing: border-box;
}

html {
  font-family: "Arial", sans-serif;
}
```

```css live-sample___basic-listbox live-sample___expanding-listbox live-sample___horizontal-listbox
select {
  appearance: base-select;
}
```

Fatto ciò, è ora possibile stilizzare gli elementi {{htmlelement("select")}} e {{htmlelement("option")}} come si desidera.

Gli stili di base sono i seguenti:

```css live-sample___basic-listbox live-sample___expanding-listbox live-sample___horizontal-listbox
select {
  border: 2px solid #dddddd;
  border-radius: 8px;
  background: #eeeeee;
  width: 200px;
  height: 130px;
}

option {
  background: #eeeeee;
  padding: 10px;
  height: 40px;
  outline: none;
}

option:nth-of-type(odd) {
  background: white;
}
```

Successivamente, viene impostato un valore {{cssxref("order")}} di `1` sul pseudo-elemento {{cssxref("::checkmark")}} per far apparire il segno di spunta delle opzioni selezionate sulla destra anziché sulla sinistra, e viene impostata un'icona personalizzata per il segno di spunta tramite la proprietà {{cssxref("content")}}.

```css live-sample___basic-listbox live-sample___expanding-listbox
option::checkmark {
  order: 1;
  margin-left: auto;
  content: "☑️";
}
```

Infine, viene impostato un {{cssxref("font-weight")}} `bold` sulle opzioni {{cssxref(":checked")}}, e un colore {{cssxref("background")}} personalizzato per gli stati {{cssxref(":hover")}} e {{cssxref(":focus")}} delle opzioni, così da sapere sempre quale opzione è stata passata con il mouse o ha ricevuto il focus.

```css live-sample___basic-listbox live-sample___expanding-listbox
option:checked {
  font-weight: bold;
}

option:hover,
option:focus {
  background: plum;
}
```

Questo esempio viene reso nel modo seguente:

{{EmbedLiveSample("basic-listbox", "100%", "200px")}}

## Varianti di stile delle listbox

Poiché le listbox personalizzate sono semplicemente elementi HTML standard, è possibile stilizzarle come si desidera. In questa sezione vengono mostrate alcune varianti dell'esempio precedente. Entrambe usano lo stesso markup o un markup simile; è stato aggiunto del CSS aggiuntivo per modificare significativamente l'aspetto e il comportamento.

### Listbox espandibile

In questo esempio, la listbox viene presentata per impostazione predefinita all'{{cssxref("height")}} di una singola opzione, nascondendo l'{{cssxref("overflow")}} così creato e aggiungendo una {{cssxref("transition")}} per animare fluidamente l'altezza di `<select>` quando il suo stato cambia. Viene inoltre impostato un valore {{cssxref("interpolate-size")}} di `allow-keywords` per attivare nel browser l'animazione tra lunghezze e parole chiave.

```css live-sample___expanding-listbox
select {
  height: 44px;
  overflow: hidden;
  transition: 0.6s height;
  interpolate-size: allow-keywords;
}
```

L'`height` viene modificata in `fit-content` quando `<select>` è in hover o ha il focus, in modo che si espanda fino alla sua altezza completa. Si noti che quando si raggiunge con Tab una select personalizzata, il focus viene assegnato alla prima `<option>` anziché a `<select>` stesso. Di conseguenza, è stato necessario usare `select:has(option:focus)` per selezionare `<select>` quando una `<option>` ha il focus, anziché semplicemente `select:focus`.

```css live-sample___expanding-listbox
select:hover,
select:has(option:focus) {
  height: fit-content;
}
```

L'esempio viene ora reso in questo modo:

{{EmbedLiveSample("expanding-listbox", "100%", "260px")}}

### Listbox orizzontale

In questo esempio, le opzioni della listbox vengono presentate orizzontalmente anziché verticalmente.

L'HTML è lo stesso degli esempi precedenti, eccetto per l'inclusione di un ulteriore `<div>` wrapper che consente di impostare una `width` su `<select>` e quindi una `width` diversa sul wrapper, in modo che tutti gli elementi `<option>` possano essere mantenuti su una riga e scorsi quando `<select>` diventa troppo stretto per contenerli tutti.

```html live-sample___horizontal-listbox
<p>
  <label for="pet-select">Select pets:</label><br />
  <select id="pet-select" multiple>
    <div class="wrapper">
      <option value="cat">Cat</option>
      <option value="dog">Dog</option>
      <option value="chicken">Chicken</option>
      <option value="fish">Fish</option>
      <option value="hamster">Hamster</option>
      <option value="gerbil">Gerbil</option>
      <option value="guinea">Guinea pig</option>
    </div>
  </select>
</p>
```

Nel CSS, si inizia impostando la {{cssxref("width")}} e il {{cssxref("margin")}} dell'elemento {{htmlelement("p")}} contenitore, in modo che la demo sia centrata orizzontalmente nel viewport e occupi gran parte della larghezza. Quindi, `<select>` viene dimensionato per occupare l'intera larghezza del proprio elemento padre e avere solo l'altezza degli elementi `<option>`. Al `<div>` `.wrapper` viene assegnato un valore {{cssxref("display")}} di `flex`, facendo sì che gli elementi `<option>` siano disposti orizzontalmente in una riga; la sua `width` viene quindi impostata affinché sia sempre larga quanto gli elementi `<option>`.

```css live-sample___horizontal-listbox
p {
  width: 90%;
  margin: 0 auto;
}

select {
  width: 100%;
  height: fit-content;
}

.wrapper {
  display: flex;
  width: fit-content;
}
```

Successivamente, agli elementi `<option>` viene aggiunto del padding per distanziarli orizzontalmente e un valore {{cssxref("position")}} di relative, così da poter posizionare i loro discendenti relativamente a essi.

```css live-sample___horizontal-listbox
option {
  padding: 10px 30px;
  position: relative;
}
```

Infine, i segni di spunta delle opzioni vengono posizionati in modo assoluto e ricevono un aspetto personalizzato.

```css live-sample___horizontal-listbox
option::checkmark {
  position: absolute;
  top: -2px;
  left: 2px;
  font-size: 1.5rem;
  color: red;
  text-shadow: 1px 1px 1px black;
}
```

```css hidden live-sample___horizontal-listbox
option:hover,
option:focus {
  background: plum;
}
```

La seconda variante viene resa nel modo seguente:

{{EmbedLiveSample("horizontal-listbox", "100%", "100px")}}

## Una listbox più complessa

In questa sezione verrà esaminato un esempio più complesso, che fornisce una listbox per la selezione dei contatti con un campo filtro integrato e un collegamento per accedere a una modalità di modifica dei contatti (fittizia).

### HTML

Nel markup, viene incluso un {{htmlelement("form")}} che contiene un'intestazione e un {{htmlelement("div")}} wrapper. All'interno del wrapper, sono inclusi altri tre elementi `<div>` che contengono rispettivamente un {{htmlelement("input")}} di testo che rappresenta il campo filtro, una listbox {{htmlelement("select")}} e un collegamento. `<select>` verrà popolato tramite JavaScript con elementi {{htmlelement("option")}} che rappresentano le scelte di contatto.

```html live-sample___complex-listbox
<form>
  <h2>Contact select</h2>
  <div class="wrapper">
    <div class="filter">
      <input
        type="text"
        aria-label="Filter contacts"
        placeholder="Filter by name, e.g. amara" />
    </div>
    <div class="options">
      <select
        multiple
        name="contact-select"
        aria-label="Select contacts"></select>
    </div>
    <div class="edit">
      <a href="#">Edit contacts</a>
    </div>
  </div>
</form>
```

### CSS

Il CSS inizia attivando lo stile personalizzato per l'elemento `<select>`, come prima:

```css hidden live-sample___complex-listbox
* {
  box-sizing: border-box;
}

html {
  font-family: "Arial", sans-serif;
}
```

```css live-sample___complex-listbox
select {
  appearance: base-select;
}
```

La maggior parte dello stile è piuttosto elementare, ma verrà esaminato evidenziando gli aspetti significativi. Innanzitutto, viene stilizzato il `<div>` `.wrapper`, assegnandogli una {{cssxref("width")}} fissa che controlla il dimensionamento orizzontale dell'intero controllo.

```css live-sample___complex-listbox
.wrapper {
  border: 2px solid #dddddd;
  border-radius: 8px;
  background: #dddddd;
  width: 250px;
}
```

Successivamente, vengono stilizzati l'`<input>` filtro, il `<div>` `.options` e il `<select>` contenuto, nonché il `<div>` `.edit` contenente il collegamento. In particolare, a `<select>` vengono assegnati una {{cssxref("height")}} fissa e un valore {{cssxref("overflow-y")}} di `scroll`, affinché gli elementi `<option>` contenuti possano scorrere al suo interno.

```css live-sample___complex-listbox
.filter input {
  display: block;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #bbbbbb;
  width: 95%;
  margin: 8px auto;
}

.options {
  padding: 0 5px;
  background: #dddddd;
}

select {
  height: 200px;
  overflow-y: scroll;
  width: 100%;
  border: 1px solid #bbbbbb;
}

.edit {
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

Gli elementi `<option>` vengono stilizzati in modo simile agli esempi precedenti, assegnando loro un effetto zebra e stili `:hover` e `:focus` evidenti:

```css live-sample___complex-listbox
option {
  background: #eeeeee;
  padding: 10px;
}

option:nth-of-type(odd) {
  background: white;
}

option:checked {
  font-weight: bold;
}

option:hover,
option:focus {
  background: plum;
}
```

Il passo successivo consiste nell'eliminare il contorno del focus predefinito per gli elementi `<input>`, `<option>` e `<a>`. Lo stile alternativo per gli elementi `<option>` è già stato fornito nel blocco di codice precedente; qui vengono fornite alternative più discrete per gli elementi `<input>` e `<a>`.

```css live-sample___complex-listbox
input,
option,
a {
  outline: none;
}

input:hover,
input:focus {
  border: 1px solid #999999;
  background: #eeeeff;
}

.edit a {
  color: #333333;
}

a:hover,
a:focus {
  outline: 2px dotted #666666;
}
```

Infine, viene fornito uno stile personalizzato per i segni di spunta delle opzioni selezionate tramite il pseudo-elemento `::checkmark`:

```css live-sample___complex-listbox
option::checkmark {
  order: 1;
  margin-left: auto;
  content: "☑️";
}
```

### JavaScript

L'ultima aggiunta necessaria all'esempio è del JavaScript per gestire il popolamento delle opzioni e la funzionalità di filtro.

In un sito reale, probabilmente l'elenco dei contatti aggiornato verrebbe recuperato da un server, ma in questo caso i dati sono stati forniti in un oggetto `contacts` statico (la maggior parte dei contatti è stata nascosta per brevità). Per ciascun contatto vengono memorizzati un nome e un valore booleano che indica se è stato selezionato nell'elemento `<select>`.

```js
const contacts = [
  { name: "Aisha Khan", selected: false },
  // …
];
```

```js hidden live-sample___complex-listbox
const contacts = [
  { name: "Aisha Khan", selected: false },
  { name: "Aisyah Rahman", selected: false },
  { name: "Amara Okafor", selected: false },
  { name: "Ananya Sharma", selected: false },
  { name: "Andrei Popescu", selected: false },
  { name: "Anh Nguyen", selected: false },
  { name: "Arjun Patel", selected: false },
  { name: "Arun Prasetyo", selected: false },
  { name: "Aya Nakamura", selected: false },
  { name: "Benjamin Brown", selected: false },
  { name: "Carlos Mendez", selected: false },
  { name: "Chloe Dubois", selected: false },
  { name: "Clara Fischer", selected: false },
  { name: "Daniel Kim", selected: false },
  { name: "Daniel Muller", selected: false },
  { name: "Diego Alvarez", selected: false },
  { name: "Ethan Williams", selected: false },
  { name: "Fatima Al-Farsi", selected: false },
  { name: "Freya Andersen", selected: false },
  { name: "Gabriel Costa", selected: false },
  { name: "Hannah Cohen", selected: false },
  { name: "Hiroshi Tanaka", selected: false },
  { name: "Isabella Martinez", selected: false },
  { name: "Jakub Novak", selected: false },
  { name: "Jonas Schmidt", selected: false },
  { name: "Kanya Chaiyaporn", selected: false },
  { name: "Kwame Mensah", selected: false },
  { name: "Leila Haddad", selected: false },
  { name: "Lena Gruber", selected: false },
  { name: "Liam O'Connor", selected: false },
  { name: "Liam Silva", selected: false },
  { name: "Lucas Silva", selected: false },
  { name: "Maria Santos", selected: false },
  { name: "Mariam Said", selected: false },
  { name: "Mateo Garcia", selected: false },
  { name: "Maya Chen", selected: false },
  { name: "Maya Nguyen", selected: false },
  { name: "Mohamed Salah", selected: false },
  { name: "Nadia Rahman", selected: false },
  { name: "Nathan Lee", selected: false },
  { name: "Nguyen Minh", selected: false },
  { name: "Noah Kim", selected: false },
  { name: "Oliver Smith", selected: false },
  { name: "Omar Hassan", selected: false },
  { name: "Ravi Reddy", selected: false },
  { name: "Samuel Johnson", selected: false },
  { name: "Sofia Rossi", selected: false },
  { name: "Thomas Anderson", selected: false },
  { name: "Valentina Ivanova", selected: false },
  { name: "Yusuf Demir", selected: false },
];
```

Si inizia ottenendo riferimenti agli elementi `<input>` `.filter` e `<select>`:

```js live-sample___complex-listbox
const filterInput = document.querySelector(".filter input");
const select = document.querySelector("select");
```

Successivamente, viene definita una funzione chiamata `populateOptions()`, che accetta come parametro un array di oggetti. All'interno della funzione viene prima svuotato il contenuto dell'elemento `<select>`. Quindi viene eseguito un ciclo sull'array di input e viene creato un elemento `<option>` per ciascun oggetto dell'array, impostando le relative proprietà `textContent` e `selected` in modo che corrispondano alle proprietà `name` e `selected` dell'oggetto. Ciascun elemento `<option>` viene aggiunto al DOM come figlio di `<select>`.

```js live-sample___complex-listbox
function populateOptions(array) {
  select.innerHTML = "";

  array.forEach((obj) => {
    const option = document.createElement("option");
    option.textContent = obj.name;
    option.selected = obj.selected;
    select.appendChild(option);
  });
}
```

Viene ora definita un'altra funzione, `filterOptions()`, che accetta come parametri una stringa di filtro e un array di oggetti. Viene verificato se la stringa è uguale alla stringa vuota o a uno o più spazi confrontando il valore restituito dal suo metodo {{jsxref("String.trim", "trim()")}} con `""`. Se restituisce `true`, viene eseguita la funzione `populateOptions()`, passandole l'intero array affinché `<select>` venga popolato con tutti gli elementi `<option>`. Se restituisce `false`, l'array di input viene filtrato usando il metodo {{jsxref("Array.filter", "filter()")}} per includere solo gli oggetti la cui proprietà `name` {{jsxref("String.startsWith", "startsWith()")}} la stringa `filter`; quindi l'array filtrato viene passato alla funzione `populateOptions()` affinché `<select>` venga popolato con un insieme filtrato di elementi `<option>`.

```js live-sample___complex-listbox
function filterOptions(filter, array) {
  if (filter.trim() === "") {
    populateOptions(array);
  } else {
    const filteredArray = array.filter((obj) =>
      obj.name.toLowerCase().startsWith(filter.toLowerCase()),
    );
    populateOptions(filteredArray);
  }
}
```

> [!NOTE]
> Sia il `name` dell'oggetto sia la stringa `filter` vengono convertiti in minuscolo tramite {{jsxref("String.toLowerCase", "toLowerCase()")}}, affinché la corrispondenza del filtro non distingua tra maiuscole e minuscole.

Successivamente, viene aggiunto un event listener [`input`](/it/docs/Web/API/Element/input_event) all'elemento `<input>` `.filter`, affinché quando il suo valore viene modificato esegua la funzione `filterOptions()` per filtrare gli elementi `<option>` visualizzati. Vengono passati il valore corrente dell'`<input>` come stringa di filtro e l'array `contacts` come array di input.

```js live-sample___complex-listbox
filterInput.addEventListener("input", () => {
  filterOptions(filterInput.value, contacts);
});
```

La parte di codice successiva aggiunge un event listener [`change`](/it/docs/Web/API/HTMLElement/change_event) all'elemento `<select>`, affinché ogni volta che un'<option> viene selezionata o deselezionata, lo stato `selected` degli oggetti nell'array `contacts` venga sincronizzato con lo stato selezionato degli oggetti `<option>` attualmente visualizzati. Ciò è necessario perché ogni volta che viene applicato un nuovo filtro all'elemento `<select>`, gli elementi `<option>` visualizzati vengono generati nuovamente dall'array `contacts`, che include il loro stato selezionato. Senza questa operazione, le opzioni selezionate andrebbero perse ogni volta che il filtro viene modificato.

Non esiste un modo per rilevare esattamente quale `<option>` è stata modificata ogni volta che viene attivata o disattivata, quindi il problema è stato risolto nel modo seguente:

1. Ottenere un array di tutti i valori `<option>` attualmente visualizzati creando un array dalla raccolta [`select.options`](/it/docs/Web/API/HTMLSelectElement/options) tramite {{jsxref("Array.from")}}, quindi applicando il mapping tramite il relativo metodo {{jsxref("Array.map", "map()")}} per sostituire ogni `<option>` nell'array con il suo valore.
2. Ottenere un array di tutti i valori `<option>` attualmente selezionati usando la stessa metodologia, eccetto che questa volta l'array di input viene creato dalla raccolta [`select.selectedOptions`](/it/docs/Web/API/HTMLSelectElement/selectedOptions).
3. Per ogni oggetto contatto nell'array `contacts`, verificare se il valore della proprietà `name` del contatto è incluso nell'array `allCurrentValues` usando il metodo {{jsxref("Array.includes", "includes()")}}. In caso contrario, ignorarlo, per evitare di modificare lo stato selezionato dei contatti che non sono nemmeno visualizzati. In caso affermativo, impostare la proprietà `selected` del contatto al risultato della verifica se l'array `currentSelectedValues` {{jsxref("Array.includes", "includes()")}} il `name` del contatto: se questo è il caso, impostare la proprietà dell'oggetto su `true`, altrimenti su `false`.

```js live-sample___complex-listbox
select.addEventListener("change", () => {
  const allCurrentValues = Array.from(select.options).map(
    (option) => option.value,
  );
  const currentSelectedValues = Array.from(select.selectedOptions).map(
    (option) => option.value,
  );

  contacts.forEach((contact) => {
    if (allCurrentValues.includes(contact.name)) {
      contact.selected = currentSelectedValues.includes(contact.name);
    }
  });
});
```

Infine, viene eseguita la funzione `populateOptions()`, passandole l'array `contacts`, affinché al caricamento della pagina venga visualizzato l'elenco completo dei contatti.

```js live-sample___complex-listbox
populateOptions(contacts);
```

### Risultato

L'esempio viene reso nel modo seguente:

{{EmbedLiveSample("complex-listbox", "100%", "380px")}}

```css hidden live-sample___basic-listbox live-sample___expanding-listbox live-sample___horizontal-listbox live-sample___complex-listbox
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

## Passo successivo

Nel prossimo articolo di questo modulo verranno esplorate le diverse [pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes) disponibili nei browser moderni per stilizzare i moduli in stati diversi.

## Vedere anche

- {{htmlelement("select")}}, {{htmlelement("option")}}, {{htmlelement("optgroup")}}, {{htmlelement("label")}}
- {{cssxref("appearance")}}
- {{cssxref("::checkmark")}}
- {{cssxref(":checked")}}
- [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select)

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms")}}
