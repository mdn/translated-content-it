---
title: "Sfida: creare un'interfaccia utente per dati sulle case"
short-title: "Sfida: interfaccia utente per dati sulle case"
slug: Learn_web_development/Core/Scripting/House_data_UI
l10n:
  sourceCommit: 7f138099644a02640a903b2abc39e685ca8ca7cd
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/JSON", "Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}

In questa sfida verrà richiesto di scrivere del JavaScript per una pagina di ricerca/filtro di case su un sito web immobiliare. Ciò includerà il recupero di dati JSON, il filtraggio di tali dati in base ai valori inseriti nei controlli del modulo forniti e il rendering di tali dati nell'interfaccia utente. Durante il percorso, verranno testate anche le conoscenze su condizionali, cicli, array e metodi degli array, oltre ad altro.

## Punto di partenza

Per iniziare, fare clic sul pulsante **Play** in uno dei pannelli di codice qui sotto per aprire l'esempio fornito in MDN Playground. Seguire quindi le istruzioni nella sezione [Descrizione del progetto](#descrizione_del_progetto) per completare la funzionalità JavaScript.

```html live-sample___house-ui-start live-sample___house-ui-finish
<h1>House search</h1>
<p>
  Search for houses for sale. You can filter your search by street, number of
  bedrooms, and number of bathrooms, or just submit the search with no filters
  to display all available properties.
</p>
<form>
  <div>
    <label for="choose-street">Street:</label>
    <select id="choose-street" name="choose-street">
      <option value="">No street selected</option>
    </select>
  </div>
  <div>
    <label for="choose-bedrooms">Number of bedrooms:</label>
    <select id="choose-bedrooms" name="choose-bedrooms">
      <option value="">Any number of bedrooms</option>
    </select>
  </div>
  <div>
    <label for="choose-bathrooms">Number of bathrooms:</label>
    <select id="choose-bathrooms" name="choose-bathrooms">
      <option value="">Any number of bathrooms</option>
    </select>
  </div>
  <div>
    <button>Search for houses</button>
  </div>
</form>
<p id="result-count">Results returned: 0</p>
<section id="output"></section>
```

```css hidden live-sample___house-ui-start live-sample___house-ui-finish
body {
  font: 1.1em / 1.5 system-ui;
  width: clamp(480px, 90%, 1200px);
  margin: 0 auto;
}

h1 {
  font-size: 1.5em;
}

h2 {
  font-size: 1.3em;
}

form div {
  display: flex;
  width: 100%;
  max-width: 500px;
  align-items: center;
  margin-bottom: 20px;
}

label[for],
select {
  flex: 1;
}

#output {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  gap: 50px;
}

#output article {
  padding: 0 20px;
  background-color: #efefef;
  border: 2px solid #cccccc;
  border-radius: 10px;
}

#output ul {
  list-style-type: none;
  padding-left: 0;
}
```

```js-nolint live-sample___house-ui-start
const streetSelect = document.getElementById("choose-street");
const bedroomSelect = document.getElementById("choose-bedrooms");
const bathroomSelect = document.getElementById("choose-bathrooms");
const form = document.querySelector("form");

const resultCount = document.getElementById("result-count");
const output = document.getElementById("output");

let houses;

function initializeForm() {

}

function renderHouses(e) {
  // Stop the form submitting
  e.preventDefault();

  // Add rest of code here
}

// Add a submit listener to the <form> element
form.addEventListener("submit", renderHouses);

// Call fetchHouseData() to initialize the app
fetchHouseData();
```

{{EmbedLiveSample("house-ui-start", "100%", 400)}}

## Descrizione del progetto

Viene fornita una pagina indice HTML contenente un modulo che permette all'utente di cercare case per via, numero di camere da letto e numero di bagni, oltre a un paio di elementi che conterranno i risultati della ricerca. Viene inoltre fornito un file JavaScript contenente alcune definizioni di costanti e variabili, oltre a un paio di definizioni di funzioni scheletro. Il compito consiste nel completare il JavaScript mancante per far funzionare l'interfaccia di ricerca delle case.

Le definizioni di costanti e variabili fornite contengono i riferimenti seguenti:

- `streetSelect`: l'elemento `<select>` "choose-street".
- `bedroomSelect`: l'elemento `<select>` "choose-bedrooms".
- `bathroomSelect`: l'elemento `<select>` "choose-bathrooms".
- `form`: l'elemento `<form>` complessivo che contiene gli elementi `<select>`.
- `resultCount`: l'elemento `<p>` "result-count", che si aggiorna per visualizzare il numero di risultati restituiti dopo ogni ricerca.
- `output`: l'elemento `<section>` "output", che visualizza i risultati della ricerca.
- `houses`: inizialmente vuoto, ma conterrà l'oggetto dei dati sulle case creato analizzando i dati JSON recuperati.

Le funzioni scheletro sono:

- `initializeForm()`: interrogherà i dati e popolerà gli elementi `<select>` con i possibili valori ricercabili.
- `renderHouses()`: filtrerà i dati in base ai valori degli elementi `<select>` ed eseguirà il rendering dei risultati.

### Recuperare i dati

La prima cosa da fare è creare una nuova funzione per recuperare i dati sulle case e memorizzarli nella variabile `houses`.

Per farlo:

1. Creare una nuova funzione subito sotto le definizioni delle variabili e delle costanti chiamata `fetchHouseData()`.
2. All'interno del corpo della funzione, utilizzare il metodo `fetch()` per recuperare il JSON disponibile all'indirizzo [https://mdn.github.io/shared-assets/misc/houses.json](https://mdn.github.io/shared-assets/misc/houses.json). Studiare la struttura di questi dati in preparazione ad alcuni dei passaggi successivi.
3. Quando la promise risultante si risolve, controllare la proprietà `ok` della risposta. Se è `false`, generare un errore personalizzato che riporti il `status` della risposta.
4. Se la risposta è corretta, restituirla come JSON utilizzando il metodo `json()`.
5. Quando la promise risultante si risolve, impostare la variabile `houses` uguale al risultato del metodo `json()` (dovrebbe essere un array di oggetti contenenti dati sulle case) e chiamare la funzione `initializeForm()`.

### Completare la funzione `initializeForm()`

Ora è necessario scrivere il contenuto della funzione `initializeForm()`. Questa interrogherà i dati memorizzati in `houses` e li utilizzerà per popolare gli elementi `<select>` con elementi `<option>` che rappresentano tutti i diversi valori filtrabili. Al momento, gli elementi `<select>` contengono soltanto un singolo elemento `<option>` con valore `""` (una stringa vuota), che rappresenta tutti i valori. L'utente può scegliere questa opzione se non desidera che i risultati siano filtrati in base a quel campo.

All'interno del corpo della funzione, scrivere codice che esegua quanto segue:

1. Creare elementi `<option>` per tutti i diversi nomi delle vie nell'elemento `<select>` "choose-street". Esistono diversi modi per farlo, ma è consigliabile creare un array temporaneo e poi iterare su tutti gli oggetti all'interno di `houses`. All'interno del ciclo, controllare se l'array temporaneo include la proprietà `street` della casa corrente. In caso contrario, aggiungerla all'array temporaneo e aggiungere un `<option>` all'elemento `<select>` "choose-street" che includa la proprietà `street` come valore.
2. Creare opzioni per tutti i possibili valori del numero di camere da letto nell'elemento `<select>` "choose-bedrooms". Per farlo, è possibile iterare sull'array `houses` e determinare qual è il valore `bedrooms` più grande, quindi scrivere un secondo ciclo che aggiunga un `<option>` all'elemento `<select>` "choose-bedrooms" per ogni numero da `1` fino a quel valore massimo.
3. Creare opzioni per tutti i possibili valori del numero di bagni nell'elemento `<select>` "choose-bathrooms". Questo può essere risolto usando la stessa tecnica del passaggio precedente.

> [!NOTE]
> Sarebbe possibile inserire direttamente gli elementi `<option>` nell'HTML, ma ciò funzionerebbe solo per questo specifico set di dati. L'obiettivo è scrivere JavaScript che popoli correttamente il modulo indipendentemente dai valori dei dati forniti (ogni oggetto casa dovrebbe avere la stessa struttura).

> [!NOTE]
> È possibile utilizzare la proprietà `innerHTML` per aggiungere contenuto figlio all'interno di elementi HTML, ma è consigliabile non farlo. Non è sempre possibile fidarsi dei dati aggiunti alla pagina: se non sono correttamente sanificati sul server, soggetti malevoli potrebbero usare `innerHTML` come mezzo per eseguire attacchi di [Cross-site scripting (XSS)](/it/docs/Web/Security/Attacks/XSS) sulla pagina. Un approccio più sicuro consiste nell'utilizzare funzionalità di scripting DOM quali `createElement()`, `appendChild()` e `textContent`. L'uso di `innerHTML` per rimuovere contenuto figlio non rappresenta un problema altrettanto rilevante.

### Completare la funzione `renderHouses()`

Successivamente, è necessario completare il corpo della funzione `renderHouses()`. Questa filtrerà i dati in base ai valori degli elementi `<select>` ed eseguirà il rendering dei risultati nell'interfaccia utente.

1. Per prima cosa, è necessario filtrare i dati. Probabilmente il modo migliore è utilizzare il metodo dell'array `filter()`, che restituisce un nuovo array contenente soltanto gli elementi dell'array che soddisfano i criteri di filtro.
   1. Questa è una funzione `filter()` piuttosto complessa da scrivere. È necessario verificare se la proprietà `street` della casa è uguale al valore selezionato dell'elemento `<select>` "choose-street", se la proprietà `bedrooms` della casa è uguale al valore selezionato dell'elemento `<select>` "choose-bedrooms" e se la proprietà `bathrooms` della casa è uguale al valore selezionato dell'elemento `<select>` "choose-bathrooms".
   2. Ogni componente del test deve restituire sempre `true` se il valore dell'elemento `<select>` associato è `""` (la stringa vuota, che rappresenta tutti i valori). Questo può essere ottenuto usando il "short-circuiting" per ciascun controllo.
   3. È inoltre necessario assicurarsi che i tipi di dati corrispondano in ciascun controllo. Il valore di un elemento di un modulo è sempre una stringa. Ciò non è necessariamente vero per i valori delle proprietà degli oggetti. Come è possibile fare corrispondere i tipi di dati ai fini del test?
2. Visualizzare il numero di risultati della ricerca filtrati nell'elemento `<p>` "result-count", usando la struttura della stringa "Risultati restituiti: numero".
3. Svuotare l'elemento `<section>` "output", in modo che non contenga elementi HTML figli. Se non viene fatto, ogni volta che viene eseguita una ricerca i risultati saranno aggiunti alla fine dei risultati precedenti anziché sostituirli.
4. Creare una nuova funzione all'interno di `renderHouses()` chiamata `renderHouse()`. Questa funzione deve accettare un oggetto casa come argomento ed eseguire due operazioni:
   1. Calcolare l'area totale delle stanze contenute nell'oggetto `room_sizes` della casa. Non è semplice come iterare su un array di numeri e sommarli, ma non è troppo difficile.
   2. Aggiungere un elemento `<article>` all'interno dell'elemento `<section>` "output" contenente il numero civico, il nome della via, il numero di camere da letto e bagni, l'area totale delle stanze e il prezzo della casa. La struttura può variare, ma dovrebbe essere simile a questo frammento HTML:

   ```html
   <article>
     <h2>number street name</h2>
     <ul>
       <li>🛏️ Bedrooms: number</li>
       <li>🛀 Bathrooms: number</li>
       <li>Room area: number m²</li>
       <li>Price: £price</li>
     </ul>
   </article>
   ```

5. Iterare su tutte le case nell'array filtrato e passare ciascuna a una chiamata a `renderHouse()`.

## Suggerimenti

- Non è necessario modificare in alcun modo l'HTML o il CSS.
- Per operazioni come trovare il valore più grande in un array di valori, la funzione di array `reduce()` è molto utile. Non è stata trattata in questo corso perché è piuttosto complessa, ma è molto potente una volta acquisita familiarità. Come obiettivo aggiuntivo, provare a documentarsi su di essa e a utilizzarla nella propria risposta.

## Esempio

L'app completata dovrebbe funzionare come nel seguente esempio live:

{{EmbedLiveSample("house-ui-finish", "100%", 700)}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile al seguente:

```js live-sample___house-ui-finish
const streetSelect = document.getElementById("choose-street");
const bedroomSelect = document.getElementById("choose-bedrooms");
const bathroomSelect = document.getElementById("choose-bathrooms");
const form = document.querySelector("form");
const resultCount = document.getElementById("result-count");
const output = document.getElementById("output");

let houses;

// Solution: Fetching the data

function fetchHouseData() {
  fetch("https://mdn.github.io/shared-assets/misc/houses.json")
    .then((response) => {
      if (!response.ok) {
        throw new Error(`HTTP error: ${response.status}`);
      }

      return response.json();
    })
    .then((json) => {
      houses = json;
      initializeForm();
    });
}

// Solution: Completing the initializeForm() function

function initializeForm() {
  // Create options for all the different street names
  const streetArray = [];
  for (let house of houses) {
    if (!streetArray.includes(house.street)) {
      streetArray.push(house.street);
      streetSelect.appendChild(document.createElement("option")).textContent =
        house.street;
    }
  }

  // Create options for all the possible bedroom values
  const largestBedrooms = houses.reduce(
    (largest, house) => (house.bedrooms > largest ? house.bedrooms : largest),
    houses[0].bedrooms,
  );
  let i = 1;
  while (i <= largestBedrooms) {
    bedroomSelect.appendChild(document.createElement("option")).textContent = i;
    i++;
  }

  // Create options for all the possible bathroom values
  const largestBathrooms = houses.reduce(
    (largest, house) => (house.bathrooms > largest ? house.bathrooms : largest),
    houses[0].bathrooms,
  );
  let j = 1;
  while (j <= largestBathrooms) {
    bathroomSelect.appendChild(document.createElement("option")).textContent =
      j;
    j++;
  }
}

// Solution: Completing the renderHouses() function

function renderHouses(e) {
  // Stop the form submitting
  e.preventDefault();

  // Filter the data
  const filteredHouses = houses.filter((house) => {
    // prettier-ignore
    const test = (streetSelect.value === "" ||
                  house.street === streetSelect.value) &&
                 (bedroomSelect.value === "" ||
                  String(house.bedrooms) === bedroomSelect.value) &&
                 (bathroomSelect.value === "" ||
                  String(house.bathrooms) === bathroomSelect.value);
    return test;
  });

  // Output the result count to the "result-count" paragraph
  resultCount.textContent = `Results returned: ${filteredHouses.length}`;

  // Empty the output element
  output.innerHTML = "";

  // Create renderHouse() function
  function renderHouse(house) {
    // Calculate total room size
    let totalArea = 0;
    const keys = Object.keys(house.room_sizes);
    for (let key of keys) {
      totalArea += house.room_sizes[key];
    }

    // Output house to UI
    const articleElem = document.createElement("article");
    articleElem.appendChild(document.createElement("h2")).textContent =
      `${house.house_number} ${house.street}`;
    const listElem = document.createElement("ul");
    listElem.appendChild(document.createElement("li")).textContent =
      `🛏️ Bedrooms: ${house.bedrooms}`;
    listElem.appendChild(document.createElement("li")).textContent =
      `🛀 Bathrooms: ${house.bathrooms}`;
    listElem.appendChild(document.createElement("li")).textContent =
      `Room area: ${totalArea}m²`;
    listElem.appendChild(document.createElement("li")).textContent =
      `Price: £${house.price}`;
    articleElem.appendChild(listElem);
    output.appendChild(articleElem);
  }

  // Pass each house in the filtered array into renderHouse()
  for (let house of filteredHouses) {
    renderHouse(house);
  }
}

// Add a submit listener to the <form> element
form.addEventListener("submit", renderHouses);

// Call fetchHouseData() to initialize the app
fetchHouseData();
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/JSON", "Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}
