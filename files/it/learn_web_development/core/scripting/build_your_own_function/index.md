---
title: Creare una funzione personale
slug: Learn_web_development/Core/Scripting/Build_your_own_function
l10n:
  sourceCommit: 30cb9ca54d74a63bd95e0e0f5281e9ade578c044
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Functions","Learn_web_development/Core/Scripting/Return_values", "Learn_web_development/Core/Scripting")}}

Dopo aver trattato gran parte della teoria essenziale nell'articolo precedente, questo articolo offre esperienza pratica. Qui sarà possibile esercitarsi a creare una funzione personalizzata. Nel frattempo, verranno anche spiegati alcuni dettagli utili sulla gestione delle funzioni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi delle funzioni JavaScript trattate nella lezione precedente.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Esperienza nella creazione di funzioni personalizzate.</li>
          <li>Aggiunta di parametri alle funzioni.</li>
          <li>Chiamata di una funzione.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Creiamo una funzione

La funzione personalizzata che verrà creata si chiamerà `displayMessage()`. Visualizzerà una finestra di messaggio personalizzata in una pagina web e fungerà da sostituto personalizzato della funzione [`alert()`](/it/docs/Web/API/Window/alert) integrata nel browser. È già stata vista in precedenza, ma rinfreschiamo brevemente la memoria. Digitare quanto segue nella console JavaScript del browser, in qualsiasi pagina:

```js
alert("This is a message");
```

La funzione `alert()` accetta un singolo argomento: la stringa visualizzata nella finestra di avviso. Provare a modificare la stringa per cambiare il messaggio.

La funzione `alert()` è limitata: è possibile modificare il messaggio, ma non è semplice variare altro, come il colore, l'icona o altri aspetti. Ne verrà creata una più divertente.

> [!NOTE]
> Questo esempio dovrebbe funzionare correttamente in tutti i browser moderni, ma lo stile potrebbe apparire un po' strano nei browser leggermente meno recenti. Si consiglia di svolgere questo esercizio in un browser moderno come Firefox, Opera o Chrome.

## La funzione di base

Per iniziare, creiamo una funzione di base.

> [!NOTE]
> Per le convenzioni di denominazione delle funzioni, seguire le stesse regole delle [convenzioni di denominazione delle variabili](/it/docs/Learn_web_development/Core/Scripting/Variables#an_aside_on_variable_naming_rules). Questo va bene, poiché è possibile distinguerle: i nomi delle funzioni sono seguiti da parentesi, mentre le variabili no.

1. Iniziare accedendo al file [function-start.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-start.html) e crearne una copia locale. L'HTML è semplice: il body contiene un solo pulsante. Sono stati forniti anche alcuni CSS di base per definire lo stile della finestra di messaggio personalizzata e un elemento {{htmlelement("script")}} vuoto in cui inserire JavaScript.
2. Successivamente, aggiungere quanto segue all'interno dell'elemento `<script>`:

   ```js
   function displayMessage() {
     // …
   }
   ```

   Si inizia con la parola chiave `function`, che indica che viene definita una funzione. Seguono il nome da assegnare alla funzione, una coppia di parentesi e una coppia di parentesi graffe. Gli eventuali parametri da assegnare alla funzione vanno inseriti tra le parentesi, mentre il codice da eseguire quando viene chiamata la funzione va inserito tra le parentesi graffe.

3. Infine, aggiungere il codice seguente all'interno delle parentesi graffe:

   ```js
   const body = document.body;

   const panel = document.createElement("div");
   panel.setAttribute("class", "msgBox");
   body.appendChild(panel);

   const msg = document.createElement("p");
   msg.textContent = "This is a message box";
   panel.appendChild(msg);

   const closeBtn = document.createElement("button");
   closeBtn.textContent = "x";
   panel.appendChild(closeBtn);

   closeBtn.addEventListener("click", () => body.removeChild(panel));
   ```

Si tratta di una quantità considerevole di codice da analizzare, quindi verrà spiegato passo dopo passo.

La prima riga seleziona l'elemento {{htmlelement("body")}} usando la [DOM API](/it/docs/Web/API/Document_Object_Model) per ottenere la proprietà [`body`](/it/docs/Web/API/Document/body) dell'oggetto globale [`document`](/it/docs/Web/API/Document/body), e la assegna a una costante chiamata `body`, in modo da potervi operare in seguito:

```js
const body = document.body;
```

La sezione successiva usa una funzione DOM API chiamata [`document.createElement()`](/it/docs/Web/API/Document/createElement) per creare un elemento {{htmlelement("div")}} e memorizzarne un riferimento in una costante chiamata `panel`. Questo elemento sarà il contenitore esterno della finestra di messaggio.

Viene quindi usata un'altra funzione DOM API chiamata [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute) per impostare sull'elemento panel un attributo `class` con valore `msgBox`. Questo semplifica la definizione dello stile dell'elemento: osservando il CSS della pagina, si noterà che viene utilizzato un selettore di classe `.msgBox` per applicare lo stile alla finestra di messaggio e al suo contenuto.

Infine, viene chiamata una funzione DOM chiamata [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild) sulla costante `body` memorizzata in precedenza, che annida un elemento all'interno dell'altro come suo figlio. Il `<div>` panel viene specificato come figlio da aggiungere all'interno dell'elemento `<body>`. Questo è necessario perché l'elemento creato non apparirà nella pagina autonomamente: occorre specificare dove collocarlo.

```js
const panel = document.createElement("div");
panel.setAttribute("class", "msgBox");
body.appendChild(panel);
```

Le due sezioni successive usano le stesse funzioni `createElement()` e `appendChild()` già viste per creare due nuovi elementi, un {{htmlelement("p")}} e un {{htmlelement("button")}}, e inserirli nella pagina come figli del `<div>` panel. Viene utilizzata la loro proprietà [`Node.textContent`](/it/docs/Web/API/Node/textContent), che rappresenta il contenuto testuale di un elemento, per inserire un messaggio nel paragrafo e una "x" nel pulsante. Questo pulsante sarà quello su cui fare clic/da attivare quando l'utente desidera chiudere la finestra di messaggio.

```js
const msg = document.createElement("p");
msg.textContent = "This is a message box";
panel.appendChild(msg);

const closeBtn = document.createElement("button");
closeBtn.textContent = "x";
panel.appendChild(closeBtn);
```

Infine, viene chiamato [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) per aggiungere una funzione che verrà chiamata quando l'utente fa clic sul pulsante "chiudi". Il codice eliminerà l'intero panel dalla pagina, chiudendo la finestra di messaggio.

In breve, il metodo `addEventListener()` può essere chiamato su qualsiasi elemento della pagina e riceve solitamente due argomenti: il nome di un evento e una funzione da eseguire quando si verifica l'evento. In questo caso, il nome dell'evento è `click`, il che significa che la funzione verrà eseguita quando l'utente fa clic sul pulsante. Gli eventi verranno approfonditi nell'[articolo sugli eventi](/it/docs/Learn_web_development/Core/Scripting/Events). La riga all'interno della funzione utilizza il metodo [`removeChild()`](/it/docs/Web/API/Node/removeChild) per specificare che si desidera rimuovere un elemento figlio specifico dell'elemento `<body>`: in questo caso, il `<div>` panel.

```js
closeBtn.addEventListener("click", () => body.removeChild(panel));
```

In sostanza, l'intero blocco di codice genera un blocco HTML simile al seguente e lo inserisce nella pagina:

```html
<div class="msgBox">
  <p>This is a message box</p>
  <button>x</button>
</div>
```

C'era molto codice da analizzare: non preoccuparsi troppo se al momento non si ricorda esattamente come funziona ogni sua parte. La parte principale su cui concentrarsi qui è la struttura e l'utilizzo della funzione, ma per questo esempio si è voluto mostrare qualcosa di interessante.

## Chiamare la funzione

La definizione della funzione è ora scritta correttamente nell'elemento `<script>`, ma nello stato attuale non farà nulla.

1. Provare a includere la riga seguente sotto la funzione per chiamarla:

   ```js
   displayMessage();
   ```

   Questa riga invoca la funzione, facendola eseguire immediatamente. Salvando il codice e ricaricandolo nel browser, verrà visualizzata immediatamente la piccola finestra di messaggio, una sola volta. Dopotutto, viene chiamata una sola volta.

2. Ora aprire gli strumenti per sviluppatori del browser nella pagina di esempio, accedere alla console JavaScript e digitare nuovamente la riga: la finestra apparirà di nuovo. È quindi disponibile una funzione riutilizzabile che può essere chiamata in qualsiasi momento.

Tuttavia, probabilmente si desidera che la finestra di messaggio appaia in risposta ad azioni dell'utente e del sistema. In una vera applicazione, una finestra di questo tipo verrebbe probabilmente chiamata in risposta alla disponibilità di nuovi dati, al verificarsi di un errore, al tentativo dell'utente di eliminare il proprio profilo ("si è sicuri?"), oppure all'aggiunta di un nuovo contatto e al completamento dell'operazione, e così via.

In questa demo, la finestra di messaggio apparirà quando l'utente fa clic sul pulsante.
Ecco i passaggi da seguire per farlo funzionare:

1. Eliminare la riga aggiunta in precedenza (`displayMessage();`).
2. Selezionare l'elemento `<button>` e memorizzarne un riferimento in una costante. Aggiungere la riga seguente al codice, sopra la definizione della funzione:

   ```js
   const btn = document.querySelector("button");
   ```

3. Creare un event listener per i clic sul pulsante che chiami la funzione. Aggiungere la riga seguente dopo quella `const btn =`:

   ```js
   btn.addEventListener("click", displayMessage);
   ```

   In modo simile al gestore dell'evento `click` di closeBtn, qui viene chiamato del codice in risposta al clic su un pulsante. Tuttavia, in questo caso, invece di chiamare una funzione anonima contenente del codice, viene chiamata per nome la funzione `displayMessage()`.

4. Infine, provare a salvare e aggiornare la pagina: ora la finestra di messaggio dovrebbe apparire quando viene fatto clic sul pulsante.

Ci si potrebbe chiedere perché non siano state incluse le parentesi dopo il nome della funzione. Questo perché non si desidera chiamare la funzione immediatamente, ma solo dopo aver fatto clic sul pulsante. Provando a modificare la riga in

```js example-bad
btn.addEventListener("click", displayMessage());
```

e salvando e ricaricando, si noterà che la finestra di messaggio appare senza che sia stato fatto clic sul pulsante. In questo contesto, le parentesi vengono talvolta chiamate "operatore di invocazione della funzione". Vanno usate solo quando si desidera eseguire immediatamente la funzione nell'ambito corrente. Analogamente, il codice all'interno della funzione anonima non viene eseguito immediatamente, poiché si trova nell'ambito della funzione.

Se è stato provato l'ultimo esperimento, assicurarsi di annullare l'ultima modifica prima di proseguire.

> [!NOTE]
> Per fare ulteriore pratica con le funzioni, consultare la sfida Scrimba<sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> [Write your first function](https://scrimba.com/fullstack-path-c0fullstack/~04h?via=mdn).

## Migliorare la funzione con i parametri

Nello stato attuale, la funzione non è ancora molto utile: non si desidera mostrare sempre lo stesso messaggio predefinito. Miglioriamo la funzione aggiungendo alcuni parametri, che consentiranno di chiamarla con opzioni differenti.

1. Prima di tutto, aggiornare la prima riga della funzione:

   ```js
   function displayMessage() {
   ```

   in:

   ```js
   function displayMessage(msgText, msgType) {
   ```

   Ora, quando viene chiamata la funzione, è possibile fornire due valori variabili tra le parentesi per specificare il messaggio da visualizzare nella finestra di messaggio e il relativo tipo.

2. Per utilizzare il primo parametro, aggiornare la riga seguente all'interno della funzione:

   ```js
   msg.textContent = "This is a message box";
   ```

   in

   ```js
   msg.textContent = msgText;
   ```

3. Infine, è necessario aggiornare la chiamata della funzione per includere un testo del messaggio aggiornato. Modificare la riga seguente:

   ```js
   btn.addEventListener("click", displayMessage);
   ```

   in questo blocco:

   ```js
   btn.addEventListener("click", () =>
     displayMessage("Woo, this is a different message!"),
   );
   ```

   Se si desidera specificare parametri tra parentesi per la funzione che viene chiamata, non è possibile chiamarla direttamente: occorre inserirla all'interno di una funzione anonima, in modo che non si trovi nell'ambito immediato e non venga quindi chiamata subito. Ora verrà chiamata solo quando viene fatto clic sul pulsante.

4. Ricaricare e provare nuovamente il codice: continuerà a funzionare correttamente, ma ora sarà anche possibile variare il messaggio nel parametro per visualizzare messaggi diversi nella finestra.

### Un parametro più complesso

Passiamo al parametro successivo. Questo richiederà un po' più di lavoro: verrà impostato in modo che, in base al valore del parametro `msgType`, la funzione visualizzi un'icona e un colore di sfondo diversi.

1. Per prima cosa, scaricare da GitHub le icone necessarie per questo esercizio ([warning](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/icons/warning.png) e [chat](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/icons/chat.png)). Salvarle in una nuova cartella denominata `icons`, nella stessa posizione del file HTML.

   > [!NOTE]
   > Le icone warning e chat sono state originariamente trovate su iconfinder.com e progettate da Nazarrudin Ansyari — grazie! (Le pagine effettive delle icone sono state successivamente spostate o rimosse.)

2. Successivamente, trovare il CSS all'interno del file HTML. Verranno apportate alcune modifiche per fare spazio alle icone. Per prima cosa, aggiornare la larghezza di `.msgBox` da:

   ```css
   width: 200px;
   ```

   a:

   ```css
   width: 242px;
   ```

3. Successivamente, aggiungere le righe seguenti all'interno della regola `.msgBox p { }`:

   ```css
   padding-left: 82px;
   background-position: 25px center;
   background-repeat: no-repeat;
   ```

4. Ora occorre aggiungere codice alla funzione `displayMessage()` per gestire la visualizzazione delle icone. Aggiungere il blocco seguente appena sopra la parentesi graffa di chiusura (`}`) della funzione:

   ```js
   if (msgType === "warning") {
     msg.style.backgroundImage = 'url("icons/warning.png")';
     panel.style.backgroundColor = "red";
   } else if (msgType === "chat") {
     msg.style.backgroundImage = 'url("icons/chat.png")';
     panel.style.backgroundColor = "aqua";
   } else {
     msg.style.paddingLeft = "20px";
   }
   ```

   Qui, se il parametro `msgType` è impostato su `"warning"`, viene visualizzata l'icona di avviso e il colore di sfondo del panel viene impostato sul rosso. Se è impostato su `"chat"`, viene visualizzata l'icona chat e il colore di sfondo del panel viene impostato su blu acqua. Se il parametro `msgType` non è impostato affatto, oppure è impostato su un valore diverso, entra in gioco la parte `else { }` del codice: al paragrafo vengono assegnati padding predefiniti e nessuna icona, inoltre non viene impostato alcun colore di sfondo per il panel. Questo fornisce uno stato predefinito quando non viene fornito alcun parametro `msgType`, il che significa che si tratta di un parametro facoltativo.

5. Testiamo la funzione aggiornata: provare ad aggiornare la chiamata a `displayMessage()` da:

   ```js
   displayMessage("Woo, this is a different message!");
   ```

   a una di queste:

   ```js
   displayMessage("Your inbox is almost full — delete some mails", "warning");
   displayMessage("Brian: Hi there, how are you today?", "chat");
   ```

   È possibile vedere quanto stia diventando utile la piccola funzione, ormai non più tanto piccola.

> [!NOTE]
> In caso di problemi nel far funzionare l'esempio, confrontare il codice con la [versione completata su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-stage-4.html) ([visualizzarla anche in esecuzione](https://mdn.github.io/learning-area/javascript/building-blocks/functions/function-stage-4.html)), oppure chiedere aiuto.

## Riepilogo

Congratulazioni per essere arrivati alla fine! Questo articolo ha illustrato l'intero processo di creazione di una funzione personalizzata pratica, che con un po' più di lavoro potrebbe essere trasferita in un progetto reale. Nel prossimo articolo verrà concluso l'argomento delle funzioni spiegando un altro concetto essenziale correlato: i valori di ritorno.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Functions","Learn_web_development/Core/Scripting/Return_values", "Learn_web_development/Core/Scripting")}}
