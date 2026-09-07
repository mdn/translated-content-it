---
title: "JavaScript: aggiungere interattività"
short-title: Aggiungere interattività
slug: Learn_web_development/Getting_started/Your_first_website/Adding_interactivity
l10n:
  sourceCommit: b5a6d8bc5fd751032f70b88e7ec1ec61339937de
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website/Publishing_your_website", "Learn_web_development/Getting_started/Your_first_website")}}

JavaScript è un linguaggio di programmazione che aggiunge interattività ai siti web. Può essere usato per controllare quasi qualsiasi cosa: convalida dei dati dei moduli, funzionalità dei pulsanti, logica dei giochi, stili dinamici, aggiornamenti delle animazioni e molto altro. Questo articolo introduce JavaScript e guida nell'aggiunta di alcune funzionalità divertenti al primo sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, con il software di base usato per creare un sito web e con i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione di JavaScript.</li>
          <li>Una comprensione di base dei fondamenti del linguaggio JavaScript, come variabili, operatori, condizionali, funzioni ed eventi.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è JavaScript?

{{Glossary("JavaScript", "JavaScript")}} è un linguaggio di programmazione completo: contiene tutte le classiche funzionalità di programmazione che potrebbero essere state viste in altri linguaggi di programmazione (o di cui almeno si è sentito parlare), come **variabili**, **cicli** e **funzioni**.

JavaScript, quando viene usato nelle pagine web (anche se può essere usato in altri contesti), generalmente funziona così:

- Ottiene riferimenti a uno o più valori, come numeri, oppure a elementi della pagina.
- Esegue un'operazione con tali valori, come sommare i numeri.
- Restituisce un risultato che può essere usato successivamente per fare qualcos'altro. Ad esempio, potrebbe essere necessario visualizzare sulla pagina la somma di tali numeri.

Vediamo un esempio. Verrà usato lo stesso elenco di base visto negli ultimi articoli:

```html live-sample___basic-js
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Verrà inoltre definita una classe CSS chiamata `.done`, che applicherà uno stile a qualsiasi elemento a cui viene assegnata, facendolo apparire come un'attività completata con testo verde e barrato. Nel passaggio successivo verrà applicata agli elementi `<li>` mediante JavaScript.

```css live-sample___basic-js
.done {
  color: darkseagreen;
  text-decoration: line-through solid black 2px;
}
```

Passiamo ora a JavaScript. Qui, vengono prima memorizzati in una variabile chiamata `listItems` i riferimenti agli elementi `<li>`. Viene quindi definita una funzione chiamata `toggleDone()` che aggiunge la classe `done` a un elemento dell'elenco se non la possiede già e rimuove la classe se la possiede. Infine, si scorre l'elenco degli elementi (usando `forEach()`) e si aggiunge un event listener (usando `addEventListener()`) a ciascun elemento, così che, quando viene fatto clic su di esso, la classe `done` venga attivata o disattivata, applicando il CSS definito in precedenza.

```js live-sample___basic-js
const listItems = document.querySelectorAll("li");

function toggleDone(e) {
  if (!e.target.className) {
    e.target.className = "done";
  } else {
    e.target.className = "";
  }
}

listItems.forEach((item) => {
  item.addEventListener("click", toggleDone);
});
```

Non è un problema se il JavaScript mostrato sopra non è ancora chiaro. Acquisire dimestichezza con JavaScript è più impegnativo che acquisirla con HTML e CSS, ma i concetti diventeranno più chiari nel seguito del corso.

Questo esempio verrà visualizzato nel browser web come segue:

{{EmbedLiveSample("basic-js", "100%", "140px")}}

Provare a fare clic sugli elementi dell'elenco alcune volte e osservare come gli stili "done" vengano attivati e disattivati di conseguenza. Non male per 11 righe di JavaScript.

## Guida a "Hello world!"

Per iniziare a scrivere JavaScript, verrà illustrato come aggiungere un esempio _Hello world!_ al sito web di esempio. ([_Hello world!_](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) è il classico esempio introduttivo alla programmazione.)

> [!WARNING]
> Se non è stato seguito il resto del corso, [scaricare questo codice di esempio](https://codeload.github.com/mdn/beginner-html-site-styled/zip/refs/heads/main) e usarlo come punto di partenza.

1. All'interno della cartella `first-website` o della cartella dell'esempio appena scaricato, creare una nuova cartella chiamata `scripts`.
2. All'interno della cartella `scripts`, creare un nuovo documento di testo chiamato `main.js` e salvarlo.
3. Andare al file `index.html` e inserire questo codice su una nuova riga, immediatamente prima del tag di chiusura `</head>`:

   ```html
   <script async src="scripts/main.js"></script>
   ```

   Questo svolge lo stesso compito dell'elemento {{htmlelement("link")}} per CSS: applica JavaScript alla pagina affinché possa influire su HTML, CSS e qualsiasi altro elemento della pagina.

4. Aggiungere questo codice al file `scripts/main.js`:

   ```js
   // Store a reference to the <h1> in a variable
   const myHeading = document.querySelector("h1");
   // Update the text content of the <h1>
   myHeading.textContent = "Hello world!";
   ```

5. Assicurarsi che i file HTML e JavaScript siano salvati, quindi caricare `index.html` nel browser. Dovrebbe apparire qualcosa di simile:

![Titolo "hello world" sopra un logo di firefox](hello-world.png)

Vediamo nel dettaglio come funziona questo esempio.

JavaScript è stato usato per modificare il testo dell'intestazione in `Hello world!`. È stato ottenuto un riferimento all'intestazione e memorizzato in una variabile chiamata `myHeading` (un contenitore che memorizza un valore). Questo è simile all'applicazione di CSS agli elementi: prima si selezionano gli elementi su cui intervenire usando un selettore CSS, quindi si definiscono gli stili desiderati per tali elementi. In entrambi i casi, quando è necessario fare qualcosa a un elemento, occorre prima selezionarlo.

Successivamente, il valore della proprietà `textContent` della variabile `myHeading` (che rappresenta il contenuto testuale dell'elemento `<h1>`) viene impostato su _Hello world!_.

Le righe che iniziano con `//` sono commenti JavaScript. Come per i commenti HTML e CSS, il browser li ignora e consentono di aggiungere note al codice per spiegare come funziona.

Proseguiamo aggiungendo alcune nuove funzionalità al sito di esempio.

> [!WARNING]
> Prima di proseguire, eliminare il codice "Hello world!" dal file `main.js`. In caso contrario, il codice esistente entrerà in conflitto con il nuovo codice che sta per essere aggiunto.

## Aggiungere un selettore di immagini

In questa sezione verranno usate funzionalità di JavaScript e dell'[API DOM](/it/docs/Web/API/HTML_DOM_API) per alternare la visualizzazione tra due immagini. Questa modifica avverrà quando un utente fa clic sull'immagine visualizzata.

1. Scegliere un'altra immagine da inserire nel sito di esempio. Idealmente, l'immagine dovrebbe avere le stesse dimensioni di quella aggiunta in precedenza, o dimensioni il più possibile simili.
2. Salvare questa immagine nella cartella `images`.
3. Aggiungere il seguente codice JavaScript al file `main.js`, assicurandosi di sostituire `firefox2.png` e entrambe le occorrenze di `firefox-icon.png` rispettivamente con i nomi della seconda e della prima immagine.

   ```js
   const myImage = document.querySelector("img");

   myImage.addEventListener("click", () => {
     const mySrc = myImage.getAttribute("src");
     if (mySrc === "images/firefox-icon.png") {
       myImage.setAttribute("src", "images/firefox2.png");
     } else {
       myImage.setAttribute("src", "images/firefox-icon.png");
     }
   });
   ```

4. Salvare tutti i file e caricare `index.html` nel browser. Ora, quando viene fatto clic sull'immagine, dovrebbe cambiare nell'altra.

In questo codice, è stato memorizzato nella variabile `myImage` un riferimento all'elemento {{htmlelement("img")}}. Gli è stata quindi assegnata una funzione event handler per l'evento `click`. Ogni volta che si fa clic su `<img>`, la funzione esegue quanto segue:

- Recupera il valore dell'attributo `src` dell'immagine.
- Usa un condizionale (struttura `if...else`) per verificare se il valore di `src` è uguale al percorso dell'immagine originale:
  - Se lo è, il codice modifica il valore di `src` nel percorso della seconda immagine, forzando il caricamento dell'altra immagine nell'elemento `<img>`.
  - Se non lo è (ovvero se l'immagine è già stata modificata), il valore di `src` torna al percorso dell'immagine originale.

> [!NOTE]
> Questa sezione introduce diversi termini importanti. I concetti chiave includono:
>
> - {{Glossary("API", "API")}}: un insieme di funzionalità che consente a uno sviluppatore di interagire con un ambiente di programmazione. Le Web API (come le funzionalità dell'API DOM usate sopra) sono costruite sopra il linguaggio JavaScript e consentono di manipolare varie parti del browser e le pagine web che visualizza.
> - [Eventi](/it/docs/Learn_web_development/Core/Scripting/Events): elementi che accadono nel browser. Sono fondamentali per rendere interattivi i siti web. È possibile eseguire codice in risposta agli eventi usando le **funzioni event handler**: sono blocchi di codice eseguiti quando si verifica un evento. L'esempio più comune è l'[evento `click`](/it/docs/Web/API/Element/click_event), generato dal browser quando un utente fa clic su qualcosa.
> - [Funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions): un modo per raggruppare codice che si desidera riutilizzare. Il codice può essere definito una sola volta all'interno di una funzione ed eseguito tutte le volte necessarie, evitando di scrivere ripetutamente lo stesso codice. Nell'esempio qui presente, è stata definita una funzione event handler per l'evento `click`, che viene eseguita ogni volta che un utente fa clic sull'immagine.
> - [Condizionali](/it/docs/Learn_web_development/Core/Scripting/Conditionals): strutture di codice usate per verificare se un'espressione restituisce `true` o `false` ed eseguire codice diverso in risposta a ciascun risultato. Una forma molto comune di condizionali è l'istruzione `if...else`.

## Aggiungere un messaggio di benvenuto personalizzato

Ora modifichiamo l'intestazione della pagina affinché mostri un messaggio di benvenuto personalizzato quando l'utente visita il sito per la prima volta. Questo messaggio di benvenuto verrà salvato nel browser usando la [Web Storage API](/it/docs/Web/API/Web_Storage_API), quindi, se l'utente lascia il sito e torna in seguito, i dati personalizzati saranno ancora disponibili. Verrà inoltre incluso un modo per consentire all'utente di modificare il messaggio.

1. In `index.html`, aggiungere la seguente riga immediatamente prima del tag di chiusura `</body>`:

   ```html
   <button>Change user</button>
   ```

2. In `main.js`, inserire il seguente codice alla fine del file, esattamente come è scritto. Questo crea riferimenti al nuovo pulsante e all'intestazione, memorizzandoli ciascuno in una variabile:

   ```js
   let myButton = document.querySelector("button");
   let myHeading = document.querySelector("h1");
   ```

3. Aggiungere la seguente funzione per impostare il saluto personalizzato. Per il momento non farà nulla; la funzione verrà chiamata in seguito.

   ```js
   function setUserName() {
     const myName = prompt("Please enter your name.");
     localStorage.setItem("name", myName);
     myHeading.textContent = `Mozilla is cool, ${myName}`;
   }
   ```

   La funzione `setUserName()` contiene una funzione [`prompt()`](/it/docs/Web/API/Window/prompt), che chiede all'utente di inserire dati e li memorizza in una variabile dopo che viene fatto clic su _OK_. In questo esempio, viene chiesto all'utente di inserire un nome, che viene memorizzato in `myName`.<br /><br />

   Successivamente, il codice usa la [Web Storage API](/it/docs/Web/API/Web_Storage_API), che consente di memorizzare dati nel browser e recuperarli in seguito. Viene usata la funzione [`localStorage.setItem()`](/it/docs/Web/API/Storage/setItem) per creare e memorizzare un elemento di dati chiamato `"name"`, impostando il suo valore sulla variabile `myName`, che contiene l'input dell'utente.<br /><br />

   Infine, viene impostato il `textContent` dell'intestazione su una stringa che include il nome memorizzato dell'utente.

4. Aggiungere il seguente blocco condizionale dopo la dichiarazione della funzione. Questo è il _codice di inizializzazione_: viene eseguito al primo caricamento della pagina per avviare il programma.

   ```js
   if (!localStorage.getItem("name")) {
     setUserName();
   } else {
     const storedName = localStorage.getItem("name");
     myHeading.textContent = `Mozilla is cool, ${storedName}`;
   }
   ```

   La prima riga di questo blocco usa l'operatore di negazione (NOT logico, rappresentato dal carattere `!`) per verificare che l'elemento di dati `name` _non_ sia già memorizzato in `localStorage`. In caso contrario, viene eseguita la funzione `setUserName()` per crearlo. Se esiste (ovvero, se l'utente ha impostato un nome utente durante una visita precedente), viene recuperato il nome memorizzato usando [`localStorage.getItem()`](/it/docs/Web/API/Storage/getItem) e il `textContent` dell'intestazione viene impostato su una stringa più il nome dell'utente, proprio come all'interno di `setUserName()`.

5. Aggiungere una funzione event handler per l'evento `click` al pulsante. Quando viene fatto clic, viene eseguita `setUserName()`. Ciò consente all'utente di memorizzare un nome diverso, se lo desidera.

   ```js
   myButton.addEventListener("click", () => {
     setUserName();
   });
   ```

6. Salvare tutti i file e caricare `index.html` nel browser. Dovrebbe essere chiesto immediatamente di inserire il proprio nome. Dopo averlo fatto, apparirà all'interno di `<h1>` come parte del saluto personalizzato. Notare come la personalizzazione persista anche dopo aver ricaricato la pagina. È possibile fare clic sul pulsante "Change user" per inserire un nuovo nome.

> [!NOTE]
> Il termine [operatore](/it/docs/Learn_web_development/Core/Scripting/Math) si riferisce a un carattere del linguaggio JavaScript che esegue un'operazione su uno o più valori. Alcuni esempi includono `+` (somma valori), `-` (sottrae un valore da un altro) e `!` (nega un valore, come visto in precedenza).

## Un nome utente `null`?

Quando viene eseguito l'esempio e appare la finestra di dialogo che richiede di inserire il nome, provare a premere il pulsante _Cancel_. Il titolo dovrebbe diventare _Mozilla is cool, null_. Questo accade perché il valore viene impostato su [`null`](/it/docs/Web/JavaScript/Reference/Operators/null) quando viene annullata la richiesta. In JavaScript, _null_ è un valore speciale che rappresenta l'assenza di un valore.

Provare anche a fare clic su _OK_ senza inserire un nome. Il titolo dovrebbe diventare _Mozilla is cool,_ perché `myName` è stato impostato su una stringa vuota.

Per evitare questi problemi, è possibile aggiungere un altro condizionale per verificare che l'utente non abbia inserito un nome vuoto. Aggiornare la funzione `setUserName()` come segue:

```js
function setUserName() {
  const myName = prompt("Please enter your name.");
  if (!myName) {
    setUserName();
  } else {
    localStorage.setItem("name", myName);
    myHeading.textContent = `Mozilla is cool, ${myName}`;
  }
}
```

In linguaggio naturale, questo significa: se `myName` non ha alcun valore, eseguire nuovamente `setUserName()` dall'inizio. Se invece ha un valore (se l'istruzione precedente non è vera), memorizzare il valore in `localStorage` e impostarlo come testo dell'intestazione.

## Conclusione

Se tutte le istruzioni di questo articolo sono state seguite, si dovrebbe ottenere una pagina simile all'immagine seguente. È anche possibile [visualizzare la nostra versione](https://mdn.github.io/beginner-html-site-scripted/).

![Aspetto finale della pagina HTML dopo la creazione degli elementi: un'intestazione, un grande logo centrato, contenuto e un pulsante](website-screen-scripted.png)

In caso di difficoltà, è possibile confrontare il proprio lavoro con il [codice di esempio completo su GitHub](https://github.com/mdn/beginner-html-site-scripted/blob/main/scripts/main.js).

In questo articolo è stata solo sfiorata la superficie di JavaScript. Molto di più verrà appreso nel modulo Core [Dynamic scripting with JavaScript](/it/docs/Learn_web_development/Core/Scripting) più avanti nel corso.

## Vedi anche

- [Scrimba: Imparare JavaScript](https://scrimba.com/learn-javascript-c0v?via=mdn) <sup>[_partner didattico MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn JavaScript_ di [Scrimba](https://scrimba.com?via=mdn) insegna JavaScript attraverso la risoluzione di oltre 140 sfide interattive di programmazione, creando progetti che includono un gioco, un'estensione del browser e persino un'app mobile. Scrimba offre lezioni interattive e coinvolgenti tenute da insegnanti competenti.
- [Imparare JavaScript](https://learnjavascript.online/)
  - : Questa è un'eccellente risorsa per aspiranti sviluppatori web. JavaScript viene insegnato in un ambiente interattivo, con brevi lezioni e test interattivi, guidati da una valutazione automatizzata. Le prime 40 lezioni sono gratuite. Il corso completo è disponibile con un piccolo pagamento una tantum.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website/Publishing_your_website", "Learn_web_development/Getting_started/Your_first_website")}}
