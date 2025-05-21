---
title: "JavaScript: Aggiungere interattività"
short-title: Aggiungere interattività
slug: Learn_web_development/Getting_started/Your_first_website/Adding_interactivity
l10n:
  sourceCommit: fdb47ddce77e5737d7f127e9809ab498c46162b3
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website/Publishing_your_website", "Learn_web_development/Getting_started/Your_first_website")}}

JavaScript è un linguaggio di programmazione che aggiunge interattività ai siti web. Puoi usarlo per controllare praticamente qualsiasi cosa — validazione dei dati dei moduli, funzionalità dei pulsanti, logica dei giochi, stile dinamico, aggiornamenti delle animazioni e molto altro. Questo articolo ti introduce a JavaScript e ti guida nell'aggiungere alcune funzionalità divertenti al tuo primo sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del tuo computer, il software di base che utilizzerai per costruire un sito web, e i sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione di JavaScript.</li>
          <li>Una comprensione di base dei fondamenti del linguaggio JavaScript come variabili, operatori, condizionali, funzioni ed eventi.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è JavaScript?

{{Glossary("JavaScript", "JavaScript")}} è un linguaggio di programmazione completo — contiene tutte le caratteristiche classiche della programmazione che potresti aver visto in altri linguaggi di programmazione (o almeno di cui hai sentito parlare), come **variabili**, **cicli**, e **funzioni**.

JavaScript, quando utilizzato nelle pagine web (anche se può essere usato in altri contesti), generalmente funziona:

- Ottenendo riferimenti a uno o più valori come numeri o elementi sulla pagina.
- Facendo qualcosa con quei valori, come sommare i numeri.
- Restituendo un risultato che può essere utilizzato per fare qualcos'altro successivamente. Ad esempio, potresti voler visualizzare la somma di quei numeri sulla pagina.

Guardiamo un esempio. Useremo la stessa lista base che abbiamo visto negli ultimi articoli:

```html live-sample___basic-js
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Definiremo anche una classe CSS chiamata `.done` che stilerà qualsiasi elemento a cui viene applicata, facendolo sembrare un compito completato con il testo in colore verde e una barratura. La applicheremo ai nostri elementi `<li>` usando JavaScript nel prossimo passo.

```css live-sample___basic-js
.done {
  color: darkseagreen;
  text-decoration: line-through solid black 2px;
}
```

Passiamo ora a JavaScript. Qui, prima conserviamo i riferimenti agli elementi `<li>` all'interno di una variabile chiamata `listItems`. Quindi definiamo una funzione chiamata `toggleDone()` che aggiunge la classe `done` a un elemento della lista se non la ha già, e la rimuove se ce l'ha. Infine, cicliamo attraverso gli elementi della lista (usando `forEach()`) e aggiungiamo un gestore di eventi (usando `addEventListener()`) a ciascun elemento in modo che quando viene cliccato, la classe `done` venga attivata, applicando il CSS che abbiamo definito in precedenza.

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

Non preoccuparti se non capisci subito lo JavaScript sopra. Familiarizzare con JavaScript è più difficile che con HTML e CSS, ma le cose diventeranno più chiare più avanti nel corso.

Questo esempio verrà visualizzato come segue in un browser web:

{{EmbedLiveSample("basic-js", "100%", "140px")}}

Prova a cliccare sugli elementi della lista alcune volte e nota come gli stili "done" vengono attivati e disattivati come risultato. Niente male per 11 righe di JavaScript.

## Una guida a "Hello world!"

Per iniziare a scrivere un po' di JavaScript, ti guideremo nell'aggiunta di un esempio di _Hello world!_ al tuo sito web di esempio. ([_Hello world!_](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) è l'esempio introduttivo standard nella programmazione.)

> [!WARNING]
> Se non hai seguito il resto del nostro corso, [scarica questo esempio di codice](https://codeload.github.com/mdn/beginner-html-site-styled/zip/refs/heads/gh-pages) e usalo come punto di partenza.

1. All'interno della cartella `first-website` o della cartella di esempio che hai appena scaricato, crea una nuova cartella chiamata `scripts`.
2. All'interno della cartella `scripts`, crea un nuovo documento di testo chiamato `main.js` e salvalo.
3. Vai al tuo file `index.html` e inserisci questo codice su una nuova riga, appena prima del tag di chiusura `</head>`:

   ```html
   <script async src="scripts/main.js"></script>
   ```

   Questo svolge lo stesso lavoro dell'elemento {{htmlelement("link")}} per il CSS – applica il JavaScript alla pagina permettendo di influenzare l'HTML (insieme al CSS e qualsiasi altra cosa sulla pagina).

4. Aggiungi questo codice al tuo file `scripts/main.js`:

   ```js
   // Store a reference to the <h1> in a variable
   const myHeading = document.querySelector("h1");
   // Update the text content of the <h1>
   myHeading.textContent = "Hello world!";
   ```

5. Assicurati che i file HTML e JavaScript siano salvati, quindi carica `index.html` nel tuo browser. Dovresti vedere qualcosa del genere:

![Intestazione "hello world" sopra un logo di Firefox](hello-world.png)

Spieghiamo come funziona questo esempio.

Abbiamo usato JavaScript per cambiare il testo dell'intestazione in `Hello world!`. Abbiamo ottenuto un riferimento dell'intestazione e lo abbiamo salvato in una variabile chiamata `myHeading` (un contenitore che memorizza un valore). Questo è simile a come si applica il CSS agli elementi – prima selezioni gli elementi che vuoi influenzare usando un selettore CSS, e poi definisci gli stili che vuoi per quegli elementi. In entrambi i casi, quando vuoi fare qualcosa a un elemento, devi prima selezionarlo.

Successivamente, abbiamo impostato il valore della proprietà `textContent` della variabile `myHeading` (che rappresenta il contenuto testuale dell'elemento `<h1>`) su _Hello world!_.

Le righe che iniziano con `//` sono commenti JavaScript. Allo stesso modo dei commenti HTML e CSS, il browser li ignora, fornendo un modo per aggiungere note riguardo al tuo codice per aiutarti a spiegare come funziona.

Andiamo avanti e aggiungiamo alcune nuove funzionalità al nostro sito di esempio.

> [!WARNING]
> Prima di andare oltre, elimina il codice "Hello world!" dal tuo file `main.js`. Se non lo fai, il codice esistente entrerà in conflitto con il nuovo codice che stai per aggiungere.

## Aggiungere un cambio di immagine

In questa sezione, utilizzerai JavaScript e le funzionalità della [DOM API](/it/docs/Web/API/HTML_DOM_API) per alternare la visualizzazione tra due immagini. Questo cambiamento avverrà quando un utente clicca sull'immagine visualizzata.

1. Scegli un'altra immagine da presentare sul tuo sito di esempio. Idealmente, l'immagine dovrebbe essere della stessa dimensione di quella che hai aggiunto in precedenza, o il più vicino possibile.
2. Salva questa immagine nella tua cartella `images`.
3. Aggiungi il seguente codice JavaScript al tuo file `main.js`, assicurandoti di sostituire `firefox2.png` e entrambe le istanze di `firefox-icon.png` con i nomi della tua seconda e prima immagine, rispettivamente.

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

4. Salva tutti i file e carica `index.html` nel browser. Ora, quando clicchi sull'immagine, dovrebbe cambiare con l'altra.

In questo codice, hai memorizzato un riferimento al tuo elemento {{htmlelement("img")}} nella variabile `myImage`. Quindi gli hai assegnato una funzione gestore dell'evento `click`. Ogni volta che l'`<img>` viene cliccato, la funzione esegue quanto segue:

- Recupera il valore dell'attributo `src` dell' immagine.
- Usa un condizionale (struttura `if ... else`) per controllare se il valore di `src` è uguale al percorso dell'immagine originale:

  - Se lo è, il codice modifica il valore di `src` al percorso della seconda immagine, forzando il caricamento dell'altra immagine all'interno dell'elemento `<img>`.
  - Se non lo è (significa che l'immagine è già stata cambiata), il valore di `src` torna al percorso dell'immagine originale.

> [!NOTE]
> Questa sezione introduce diversi termini importanti. Concetti chiave includono:
>
> - {{Glossary("API", "API")}}: Un insieme di funzionalità che consente a uno sviluppatore di interagire con un ambiente di programmazione. Le Web API (come le funzionalità DOM API che abbiamo usato sopra) sono costruite sopra il linguaggio JavaScript e consentono di manipolare varie parti del browser e delle pagine web che visualizza.
> - [Eventi](/it/docs/Learn_web_development/Core/Scripting/Events): Cose che succedono nel browser. Sono fondamentali per rendere interattivi i siti web. Puoi eseguire il codice in risposta agli eventi usando **funzioni gestori degli eventi** – questi sono blocchi di codice che vengono eseguiti quando si verifica un evento. Il più comune esempio è l'[evento click](/it/docs/Web/API/Element/click_event), che viene attivato dal browser quando un utente clicca su qualcosa.
> - [Funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions): Un modo di impacchettare il codice che desideri riutilizzare. Puoi definire il tuo codice all'interno di una funzione una volta e poi eseguirlo tutte le volte che vuoi, il che ti aiuta a evitare di scrivere lo stesso codice più volte. Nel nostro esempio qui, abbiamo definito una funzione gestore dell'evento `click`, che viene eseguita ogni volta che un utente clicca sull'immagine.
> - [Condizionali](/it/docs/Learn_web_development/Core/Scripting/Conditionals): Strutture di codice usate per verificare se un'espressione restituisce `true` o `false` e eseguire codice diverso in risposta a ciascun risultato. Una forma molto comune di condizionali è l'istruzione `if...else`.

## Aggiungere un messaggio di benvenuto personalizzato

Successivamente, cambiamo l'intestazione della pagina per mostrare un messaggio di benvenuto personalizzato quando l'utente visita per la prima volta il sito. Questo messaggio di benvenuto verrà salvato nel browser usando la [Web Storage API](/it/docs/Web/API/Web_Storage_API), in modo che se l'utente lascia il sito e ritorna più tardi, i suoi dati personalizzati saranno ancora lì. Includeremo anche un modo per l'utente di cambiare il messaggio.

1. In `index.html`, aggiungi la seguente riga appena prima del tag di chiusura `</body>`:

   ```html
   <button>Change user</button>
   ```

2. In `main.js`, posiziona il seguente codice in fondo al file, esattamente come è scritto. Questo crea riferimenti al nuovo pulsante e all'intestazione, memorizzandoli ciascuno all'interno di variabili:

   ```js
   let myButton = document.querySelector("button");
   let myHeading = document.querySelector("h1");
   ```

3. Aggiungi la seguente funzione per impostare il saluto personalizzato. Questo non farà ancora nulla; chiameremo la funzione più tardi.

   ```js
   function setUserName() {
     const myName = prompt("Please enter your name.");
     localStorage.setItem("name", myName);
     myHeading.textContent = `Mozilla is cool, ${myName}`;
   }
   ```

   La funzione `setUserName()` contiene una funzione [`prompt()`](/it/docs/Web/API/Window/prompt), che chiede all'utente di inserire dati e li memorizza in una variabile dopo che preme _OK_. In questo esempio, stiamo chiedendo all'utente di inserire un nome e lo memorizziamo in `myName`.<br /><br />

   Successivamente, il codice usa la [Web Storage API](/it/docs/Web/API/Web_Storage_API), che ci permette di memorizzare dati nel browser e recuperarli più tardi. Usiamo la funzione [`localStorage.setItem()`](/it/docs/Web/API/Storage/setItem) per creare e memorizzare un elemento di dati chiamato `"name"`, impostando il suo valore alla variabile `myName`, che contiene l'input dell'utente.<br /><br />

   Infine, impostiamo il `textContent` dell'intestazione su una stringa che include il nome memorizzato dell'utente.

4. Aggiungi il seguente blocco condizionale dopo la dichiarazione della funzione. Questo è il nostro _codice di inizializzazione_ — viene eseguito quando la pagina si carica per la prima volta per avviare il programma:

   ```js
   if (!localStorage.getItem("name")) {
     setUserName();
   } else {
     const storedName = localStorage.getItem("name");
     myHeading.textContent = `Mozilla is cool, ${storedName}`;
   }
   ```

   La prima linea di questo blocco usa l'operatore di negazione (NOT logico, rappresentato dal carattere `!`) per controllare se l'elemento di dati `name` _non_ è già memorizzato in `localStorage`. Se non lo è, la funzione `setUserName()` viene eseguita per crearlo. Se esiste (cioè, l'utente ha impostato un nome utente durante una visita precedente), recuperiamo il nome memorizzato usando [`localStorage.getItem()`](/it/docs/Web/API/Storage/getItem) e impostiamo il `textContent` dell'intestazione su una stringa più il nome dell'utente – esattamente come abbiamo fatto all'interno di `setUserName()`.

5. Aggiungi una funzione gestore dell'evento `click` al pulsante. Quando viene cliccato, `setUserName()` viene eseguito. Questo permette all'utente di memorizzare un nome diverso, se lo desidera.

   ```js
   myButton.addEventListener("click", () => {
     setUserName();
   });
   ```

6. Salva tutti i file e carica `index.html` nel browser. Dovresti essere immediatamente chiesto di inserire il tuo nome. Dopo averlo fatto, esso apparirà all'interno dell'`<h1>` come parte del saluto personalizzato. Nota come la personalizzazione persiste anche dopo aver ricaricato la pagina. Puoi cliccare sul pulsante "Cambia utente" per inserire un nuovo nome.

> [!NOTE]
> Il termine [operatore](/it/docs/Learn_web_development/Core/Scripting/Math) si riferisce a un carattere del linguaggio JavaScript che esegue un'operazione su uno o più valori. Esempi includono `+` (somma dei valori), `-` (sottrazione di un valore da un altro), e `!` (nega un valore — come hai visto prima).

## Un nome utente di null?

Quando esegui l'esempio e ottieni la finestra di dialogo che ti chiede di inserire il tuo nome, prova a premere il pulsante _Cancel_. Dovresti ritrovarti con un titolo che dice _Mozilla è fantastico, null_. Questo accade perché il valore viene impostato su [`null`](/it/docs/Web/JavaScript/Reference/Operators/null) quando annulli il prompt. In JavaScript, _null_ è un valore speciale che rappresenta l'assenza di un valore.

Prova anche a cliccare _OK_ senza inserire un nome. Dovresti ritrovarti con un titolo che dice _Mozilla è fantastico,_ perché hai impostato `myName` su una stringa vuota.

Per evitare questi problemi, puoi aggiungere un'altra condizionale per verificare che l'utente non abbia inserito un nome vuoto. Aggiorna la tua funzione `setUserName()` come segue:

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

In termini umani, questo significa: Se `myName` non ha valore, esegui di nuovo `setUserName()` dall'inizio. Se ha valore (se l'affermazione sopra non è vera), memorizza il valore in `localStorage` e impostalo come testo dell'intestazione.

## Conclusione

Se hai seguito tutte le istruzioni in questo articolo, dovresti ritrovarti con una pagina che assomiglia all'immagine qui sotto. Puoi anche [visualizzare la nostra versione](https://mdn.github.io/beginner-html-site-scripted/).

![Aspetto finale della pagina HTML dopo aver creato elementi: un'intestazione, un grande logo centrato, contenuto, e un pulsante](website-screen-scripted.png)

Se rimani bloccato, puoi confrontare il tuo lavoro con il nostro [codice di esempio concluso su GitHub](https://github.com/mdn/beginner-html-site-scripted/blob/main/scripts/main.js).

In questo articolo abbiamo appena graffiato la superficie di JavaScript. Imparerai molto di più nel nostro modulo core [Scripting dinamico con JavaScript](/it/docs/Learn_web_development/Core/Scripting) più avanti nel corso.

## Vedi anche

- [Scrimba: Impara JavaScript](https://scrimba.com/learn-javascript-c0v?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn JavaScript_ di [Scrimba](https://scrimba.com?via=mdn) ti insegna JavaScript risolvendo oltre 140 sfide di coding interattive, costruendo progetti inclusi un gioco, un'estensione per il browser e persino un'app mobile. Scrimba offre lezioni interattive divertenti tenute da insegnanti competenti.
- [Impara JavaScript](https://learnjavascript.online/)
  - : Questa è una risorsa eccellente per aspiranti sviluppatori web! Impara JavaScript in un ambiente interattivo, con lezioni brevi e test interattivi, guidato da una valutazione automatizzata. Le prime 40 lezioni sono gratuite. Il corso completo è disponibile per un piccolo pagamento una tantum.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website/Publishing_your_website", "Learn_web_development/Getting_started/Your_first_website")}}
