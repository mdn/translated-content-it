---
title: "JavaScript: Aggiungere interattività"
short-title: Aggiungere interattività
slug: Learn_web_development/Getting_started/Your_first_website/Adding_interactivity
l10n:
  sourceCommit: fdb47ddce77e5737d7f127e9809ab498c46162b3
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website/Publishing_your_website", "Learn_web_development/Getting_started/Your_first_website")}}

JavaScript è un linguaggio di programmazione che aggiunge interattività ai siti web. È possibile utilizzarlo per controllare praticamente qualsiasi cosa — convalida dei dati dei form, funzionalità dei pulsanti, logica dei giochi, styling dinamico, aggiornamenti delle animazioni, e molto altro. Questo articolo ti inizierà a far utilizzare JavaScript, guidandoti nell'aggiungere alcune funzionalità divertenti al tuo primo sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del tuo computer, il software basilare che utilizzerai per costruire un sito web e i file system.
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

## Cos'è JavaScript?

{{Glossary("JavaScript", "JavaScript")}} è un linguaggio di programmazione completo - contiene tutte le classiche funzionalità di programmazione che potresti aver visto in altri linguaggi di programmazione (o di cui almeno hai sentito parlare), come **variabili**, **loop** e **funzioni**.

JavaScript, quando usato sulle pagine web (sebbene possa essere usato anche in altri contesti), generalmente funziona:

- Ottenendo riferimenti a uno o più valori come numeri o elementi sulla pagina.
- Facendo qualcosa con quei valori, come sommare i numeri insieme.
- Restituendo un risultato che può essere usato per fare qualcos'altro successivamente. Ad esempio, potresti voler visualizzare la somma di quei numeri sulla pagina.

Vediamo un esempio. Utilizzeremo la stessa lista di base che abbiamo visto negli ultimi articoli:

```html live-sample___basic-js
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Definiremo anche una classe CSS chiamata `.done` che stilerà qualsiasi elemento a cui è applicata, facendolo sembrare un'attività completata con colore del testo verde e una barra sopra. La applicheremo ai nostri elementi `<li>` usando JavaScript nel passaggio successivo.

```css live-sample___basic-js
.done {
  color: darkseagreen;
  text-decoration: line-through solid black 2px;
}
```

Passiamo ora al JavaScript. Qui, prima memorizziamo riferimenti agli elementi `<li>` all'interno di una variabile chiamata `listItems`. Quindi definiamo una funzione chiamata `toggleDone()` che aggiunge la classe `done` a un elemento della lista se non lo ha già, e la rimuove se invece ce l'ha. Infine, attraversiamo gli elementi della lista (usando `forEach()`) e aggiungiamo un gestore di eventi (usando `addEventListener()`) a ciascun elemento della lista in modo che quando viene cliccato, la classe `done` viene attivata o disattivata, applicando il CSS che abbiamo definito prima.

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

Non preoccuparti se non capisci ora il JavaScript sopra. Diventare familiari con JavaScript è più impegnativo rispetto a HTML e CSS, ma le cose saranno più chiare più avanti nel corso.

Questo esempio verrà visualizzato come segue in un browser web:

{{EmbedLiveSample("basic-js", "100%", "140px")}}

Prova a cliccare gli elementi della lista alcune volte e nota come gli stili "done" vengono attivati e disattivati. Non male per 11 righe di JavaScript.

## Un percorso "Hello world!"

Per iniziare a scrivere del JavaScript, ti guideremo nell'aggiungere un esempio _Hello world!_ al tuo sito web di esempio. ([_Hello world!_](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) è il classico esempio introduttivo nella programmazione.)

> [!WARNING]
> Se non hai seguito il resto del nostro corso, [scarica questo codice di esempio](https://codeload.github.com/mdn/beginner-html-site-styled/zip/refs/heads/gh-pages) e usalo come punto di partenza.

1. All'interno della tua cartella `first-website` o della cartella di esempio che hai appena scaricato, crea una nuova cartella chiamata `scripts`.
2. All'interno della cartella `scripts`, crea un nuovo documento di testo chiamato `main.js`, e salvalo.
3. Vai al tuo file `index.html` ed inserisci questo codice su una nuova riga, appena prima del tag di chiusura `</head>`:

   ```html
   <script async src="scripts/main.js"></script>
   ```

   Questo svolge lo stesso lavoro dell'elemento {{htmlelement("link")}} per il CSS – applica il JavaScript alla pagina in modo che possa influenzare l'HTML (insieme al CSS e a qualsiasi altra cosa sulla pagina).

4. Aggiungi questo codice al tuo file `scripts/main.js`:

   ```js
   // Store a reference to the <h1> in a variable
   const myHeading = document.querySelector("h1");
   // Update the text content of the <h1>
   myHeading.textContent = "Hello world!";
   ```

5. Assicurati che i file HTML e JavaScript siano salvati, quindi carica `index.html` nel tuo browser. Dovresti vedere qualcosa di simile a questo:

![Titolo "hello world" sopra un logo firefox](hello-world.png)

Facciamo un'analisi di come funziona questo esempio.

Abbiamo usato JavaScript per cambiare il testo del titolo in `Hello world!`. Abbiamo acquisito un riferimento al titolo e lo abbiamo memorizzato in una variabile chiamata `myHeading` (un contenitore che memorizza un valore). Questo è simile a come si applica il CSS agli elementi – prima si selezionano gli elementi che si vogliono influenzare utilizzando un selettore CSS, e poi si definiscono gli stili per quegli elementi. In entrambi i casi, quando si vuole fare qualcosa a un elemento, bisogna selezionarlo prima.

Successivamente, abbiamo impostato il valore della proprietà `textContent` della variabile `myHeading` (che rappresenta il contenuto del testo dell'elemento `<h1>`) su _Hello world!_.

Le righe che iniziano con `//` sono commenti JavaScript. Come i commenti HTML e CSS, il browser li ignora, fornendo un modo per aggiungere note sul tuo codice per aiutarti a spiegare come funziona.

Passiamo ad aggiungere alcune nuove funzionalità al nostro sito di esempio.

> [!WARNING]
> Prima di procedere ulteriormente, elimina il codice "Hello world!" dal tuo file `main.js`. Se non lo fai, il codice esistente entrerà in conflitto con il nuovo codice che stai per aggiungere.

## Aggiungere un cambiamento di immagine

In questa sezione, utilizzerai funzionalità di JavaScript e [DOM API](/it/docs/Web/API/HTML_DOM_API) per alternare la visualizzazione tra due immagini. Questo cambiamento avverrà quando un utente clicca sull'immagine visualizzata.

1. Scegli un'altra immagine da inserire nel tuo sito di esempio. Idealmente, l'immagine dovrebbe avere la stessa dimensione di quella che hai aggiunto precedentemente, o il più vicino possibile.
2. Salva questa immagine nella tua cartella `images`.
3. Aggiungi il seguente codice JavaScript al tuo file `main.js`, assicurandoti di sostituire `firefox2.png` e entrambe le istanze di `firefox-icon.png` con i nomi della seconda e della prima immagine, rispettivamente.

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

4. Salva tutti i file e carica `index.html` nel browser. Ora quando clicchi sull'immagine, dovrebbe cambiare nell'altra.

In questo codice, hai memorizzato un riferimento al tuo elemento {{htmlelement("img")}} nella variabile `myImage`. Poi gli hai assegnato una funzione gestore di eventi `click`. Ogni volta che l'`<img>` viene cliccato, la funzione fa quanto segue:

- Recupera il valore dell'attributo `src` dell'immagine.
- Usa un condizionale (una struttura `if ... else`) per controllare se il valore di `src` è uguale al percorso dell'immagine originale:

  - Se lo è, il codice cambia il valore `src` al percorso della seconda immagine, costringendo l'altra immagine a essere caricata all'interno dell'elemento `<img>`.
  - Se non lo è (significa che l'immagine è già stata cambiata), il valore `src` torna al percorso dell'immagine originale.

> [!NOTE]
> Questa sezione introduce diversi termini importanti. I concetti chiave includono:
>
> - {{Glossary("API", "API")}}: Un insieme di funzionalità che permette a uno sviluppatore di interagire con un ambiente di programmazione. Le API web (come quelle del DOM API che abbiamo utilizzato sopra) sono costruite sopra il linguaggio JavaScript e ti permettono di manipolare varie parti del browser e delle pagine web che visualizza.
> - [Eventi](/it/docs/Learn_web_development/Core/Scripting/Events): Cose che accadono nel browser. Sono fondamentali per rendere i siti web interattivi. Puoi eseguire codice in risposta agli eventi usando **funzioni gestore di eventi** – queste sono blocchi di codice che vengono eseguiti quando si verifica un evento. L'esempio più comune è l'[evento click](/it/docs/Web/API/Element/click_event), che viene emesso dal browser quando un utente clicca su qualcosa.
> - [Funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions): Un modo di impacchettare il codice che desideri riutilizzare. Puoi definire il tuo codice all'interno di una funzione una volta e poi eseguirlo tutte le volte che vuoi, il che ti aiuta a evitare di scrivere lo stesso codice più volte. Nel nostro esempio qui, abbiamo definito una funzione gestore di eventi per il `click`, che viene eseguita ogni volta che un utente clicca sull'immagine.
> - [Condizionali](/it/docs/Learn_web_development/Core/Scripting/Conditionals): Strutture di codice utilizzate per testare se un'espressione restituisce `true` o `false` ed eseguire codice diverso in risposta a ciascun risultato. Una forma molto comune di condizionali è l'istruzione `if...else`.

## Aggiungere un messaggio di benvenuto personalizzato

A questo punto, cambiamo il titolo della pagina per mostrare un messaggio di benvenuto personalizzato quando l'utente visita per la prima volta il sito. Questo messaggio di benvenuto verrà salvato nel browser utilizzando il [Web Storage API](/it/docs/Web/API/Web_Storage_API), quindi se l'utente lascia il sito e torna più tardi, i suoi dati personali saranno ancora lì. Includeremo anche un modo per consentire all'utente di cambiare il messaggio.

1. In `index.html`, aggiungi la seguente riga appena prima del tag di chiusura `</body>`:

   ```html
   <button>Change user</button>
   ```

2. In `main.js`, inserisci il seguente codice alla fine del file, esattamente come è scritto. Questo crea riferimenti al nuovo pulsante e al titolo, memorizzando ciascuno all'interno di variabili:

   ```js
   let myButton = document.querySelector("button");
   let myHeading = document.querySelector("h1");
   ```

3. Aggiungi la seguente funzione per impostare il saluto personalizzato. Questo non farà nulla ancora; chiameremo la funzione più avanti.

   ```js
   function setUserName() {
     const myName = prompt("Please enter your name.");
     localStorage.setItem("name", myName);
     myHeading.textContent = `Mozilla is cool, ${myName}`;
   }
   ```

   La funzione `setUserName()` contiene una funzione [`prompt()`](/it/docs/Web/API/Window/prompt), che chiede all'utente di inserire dati e li memorizza in una variabile dopo che l'utente ha cliccato _OK_. In questo esempio, stiamo chiedendo all'utente di inserire un nome e lo stiamo memorizzando in `myName`.<br /><br />

   Successivamente, il codice utilizza il [Web Storage API](/it/docs/Web/API/Web_Storage_API), che ci permette di memorizzare dati nel browser e recuperarli successivamente. Utilizziamo la funzione [`localStorage.setItem()`](/it/docs/Web/API/Storage/setItem) per creare e memorizzare un elemento di dati chiamato `"name"`, impostando il suo valore sulla variabile `myName`, che contiene l'input dell'utente.<br /><br />

   Infine, impostiamo il `textContent` del titolo su una stringa che include il nome memorizzato dall'utente.

4. Aggiungi il seguente blocco condizionale dopo la dichiarazione della funzione. Questo è il nostro _codice di inizializzazione_ — viene eseguito quando la pagina viene caricata per la prima volta per avviare il programma:

   ```js
   if (!localStorage.getItem("name")) {
     setUserName();
   } else {
     const storedName = localStorage.getItem("name");
     myHeading.textContent = `Mozilla is cool, ${storedName}`;
   }
   ```

   La prima riga di questo blocco utilizza l'operatore di negazione (NOT logico, rappresentato dal carattere `!`) per controllare se l'elemento di dati `name` non è già memorizzato in `localStorage`. Se non lo è, la funzione `setUserName()` viene eseguita per crearlo. Se esiste (cioè, l'utente ha impostato un nome utente durante una visita precedente), recuperiamo il nome memorizzato utilizzando [`localStorage.getItem()`](/it/docs/Web/API/Storage/getItem) e impostiamo il `textContent` del titolo su una stringa, più il nome dell'utente – proprio come abbiamo fatto all'interno di `setUserName()`.

5. Aggiungi una funzione gestore di eventi `click` al pulsante. Quando viene cliccato, `setUserName()` viene eseguita. Questo permette all'utente di memorizzare un nome diverso se desidera.

   ```js
   myButton.addEventListener("click", () => {
     setUserName();
   });
   ```

6. Salva tutti i file e carica `index.html` nel browser. Dovresti essere immediatamente chiesto di inserire il tuo nome. Dopo averlo fatto, esso apparirà all'interno dell'`<h1>` come parte del saluto personalizzato. Nota come la personalizzazione persiste anche dopo aver ricaricato la pagina. Puoi cliccare sul pulsante "Change user" per inserire un nuovo nome.

> [!NOTE]
> Il termine [operatore](/it/docs/Learn_web_development/Core/Scripting/Math) si riferisce a un carattere del linguaggio JavaScript che esegue un'operazione su uno o più valori. Esempi includono `+` (somma i valori), `-` (sottrae un valore da un altro) e `!` (nega un valore — come hai visto in precedenza).

## Un nome utente di null?

Quando esegui l'esempio e ottieni la finestra di dialogo che ti chiede di inserire il tuo nome, prova a premere il pulsante _Cancel_. Dovresti finire con un titolo che dice _Mozilla is cool, null_. Questo accade perché il valore è impostato su [`null`](/it/docs/Web/JavaScript/Reference/Operators/null) quando annulli il prompt. In JavaScript, _null_ è un valore speciale che rappresenta l'assenza di un valore.

Inoltre, prova a cliccare _OK_ senza inserire un nome. Dovresti finire con un titolo che dice _Mozilla is cool,_ perché hai impostato `myName` su una stringa vuota.

Per evitare questi problemi, puoi aggiungere un ulteriore test condizionale per assicurarti che l'utente non abbia inserito un nome vuoto. Aggiorna la tua funzione `setUserName()` al seguente:

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

In linguaggio umano, questo significa: Se `myName` non ha valore, esegui di nuovo `setUserName()` dall'inizio. Se ha valore (se l'affermazione precedente non è vera), allora memorizza il valore in `localStorage` e impostalo come testo del titolo.

## Conclusione

Se hai seguito tutte le istruzioni di questo articolo, dovresti finire con una pagina che assomiglia a qualcosa come l'immagine qui sotto. Puoi anche [visualizzare la nostra versione](https://mdn.github.io/beginner-html-site-scripted/).

![Aspetto finale della pagina HTML dopo aver creato elementi: un'intestazione, un grande logo centrato, contenuto e un pulsante](website-screen-scripted.png)

Se ti blocchi, puoi confrontare il tuo lavoro con il nostro [codice di esempio finale su GitHub](https://github.com/mdn/beginner-html-site-scripted/blob/main/scripts/main.js).

Abbiamo solo davvero grattato la superficie di JavaScript in questo articolo. Imparerai molto di più nel nostro modulo Core [Script dinamici con JavaScript](/it/docs/Learn_web_development/Core/Scripting) più avanti nel corso.

## Vedi anche

- [Scrimba: Learn JavaScript](https://scrimba.com/learn-javascript-c0v?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn JavaScript_ di [Scrimba](https://scrimba.com?via=mdn) ti insegna JavaScript risolvendo oltre 140 sfide di codifica interattive, costruendo progetti tra cui un gioco, un'estensione del browser e persino un'app mobile. Scrimba offre lezioni interattive divertenti insegnate da insegnanti competenti.
- [Learn JavaScript](https://learnjavascript.online/)
  - : Questa è una risorsa eccellente per gli aspiranti sviluppatori web! Impara JavaScript in un ambiente interattivo, con lezioni brevi e test interattivi, guidati da una valutazione automatizzata. Le prime 40 lezioni sono gratuite. Il corso completo è disponibile per un piccolo pagamento una tantum.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website/Publishing_your_website", "Learn_web_development/Getting_started/Your_first_website")}}
