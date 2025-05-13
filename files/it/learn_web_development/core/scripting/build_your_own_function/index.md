---
title: Costruisca la sua funzione
slug: Learn_web_development/Core/Scripting/Build_your_own_function
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Functions","Learn_web_development/Core/Scripting/Return_values", "Learn_web_development/Core/Scripting")}}

Con la maggior parte della teoria essenziale trattata nell'articolo precedente, questo articolo fornisce un'esperienza pratica. Qui avrà l'opportunità di fare pratica costruendo la sua funzione personalizzata. Nel frattempo, spiegheremo anche alcuni dettagli utili per gestire le funzioni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi delle funzioni JavaScript come trattato nella lezione precedente.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Esperienza nella costruzione di funzioni personalizzate.</li>
          <li>Aggiunta di parametri alle sue funzioni.</li>
          <li>Chiamata della sua funzione.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Apprendimento attivo: Costruiamo una funzione

La funzione personalizzata che andremo a costruire si chiamerà `displayMessage()`. Visualizzerà una finestra di messaggio personalizzata su una pagina web e agirà come una sostituzione personalizzata per la funzione [`alert()`](/it/docs/Web/API/Window/alert) integrata nel browser. L'abbiamo già vista in precedenza, ma rispolveriamo la memoria. Digiti quanto segue nella console JavaScript del suo browser, su qualsiasi pagina desideri:

```js
alert("This is a message");
```

La funzione `alert()` accetta un unico argomento: la stringa visualizzata nella finestra di avviso. Provi a variare la stringa per cambiare il messaggio.

La funzione `alert()` è limitata: può modificare il messaggio, ma non può facilmente variare nient'altro, come il colore, l'icona o altro ancora. Ne costruiremo una che risulterà più divertente.

> [!NOTE]
> Questo esempio dovrebbe funzionare correttamente in tutti i browser moderni, ma lo stile potrebbe sembrare un po' strano nei browser leggermente più vecchi. Si consiglia di svolgere questo esercizio su un browser moderno come Firefox, Opera o Chrome.

## La funzione di base

Per iniziare, componiamo una funzione di base.

> [!NOTE]
> Per le convenzioni di denominazione delle funzioni, dovrebbe seguire le stesse regole delle [convenzioni di denominazione delle variabili](/it/docs/Learn_web_development/Core/Scripting/Variables#an_aside_on_variable_naming_rules). Va bene, poiché può distinguerle: i nomi delle funzioni appaiono con delle parentesi dopo di essi, mentre le variabili no.

1. Inizi accedendo al file [function-start.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-start.html) e facendone una copia locale. Vedrà che l'HTML è semplice: il corpo contiene solo un singolo pulsante. Abbiamo anche fornito del CSS di base per stilizzare la finestra di messaggio personalizzata e un elemento {{htmlelement("script")}} vuoto per inserire il nostro JavaScript.
2. Successivamente, aggiunga quanto segue all'interno dell'elemento `<script>`:

   ```js
   function displayMessage() {
     // …
   }
   ```

   Iniziamo con la parola chiave `function`, che significa che stiamo definendo una funzione. Segue il nome che vogliamo attribuire alla nostra funzione, un insieme di parentesi tonde e un insieme di parentesi graffe. Qualsiasi parametro desideriamo dare alla nostra funzione va dentro le parentesi tonde e il codice che viene eseguito quando chiamiamo la funzione va dentro le parentesi graffe.

3. Infine, aggiunga il seguente codice all'interno delle parentesi graffe:

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

   closeBtn.addEventListener("click", () =>
     panel.parentNode.removeChild(panel),
   );
   ```

Questo è un bel po' di codice da attraversare, quindi lo illustreremo passo dopo passo.

La prima riga seleziona l'elemento {{htmlelement("body")}} utilizzando l'API DOM per ottenere la proprietà [`body`](/it/docs/Web/API/Document/body) dell'oggetto globale [`document`](/it/docs/Web/API/Document/body) e assegnandolo a una costante chiamata `body`, in modo da poterci fare qualcosa in seguito:

```js
const body = document.body;
```

La sezione successiva utilizza una funzione dell'API DOM chiamata [`document.createElement()`](/it/docs/Web/API/Document/createElement) per creare un elemento {{htmlelement("div")}} e memorizzare un riferimento ad esso in una costante chiamata `panel`. Questo elemento sarà il contenitore esterno della nostra finestra di messaggio.

Utilizziamo quindi un'altra funzione dell'API DOM chiamata [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute) per impostare un attributo `class` sul nostro pannello con un valore di `msgBox`. Questo è per facilitare lo stile dell'elemento: se guarda il CSS sulla pagina, vedrà che stiamo usando un selettore di classe `.msgBox` per stilizzare la finestra di messaggio e i suoi contenuti.

Infine, chiamiamo una funzione DOM chiamata [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild) sulla costante `body` che abbiamo memorizzato in precedenza, che nidifica un elemento all'interno di un altro come suo figlio. Specifichiamo il pannello `<div>` come figlio che vogliamo aggiungere all'interno dell'elemento `<body>`. Dobbiamo farlo poiché l'elemento creato non apparirà da solo sulla pagina: dobbiamo specificare dove posizionarlo.

```js
const panel = document.createElement("div");
panel.setAttribute("class", "msgBox");
body.appendChild(panel);
```

Le due sezioni successive utilizzano le stesse funzioni `createElement()` e `appendChild()` che abbiamo già visto per creare due nuovi elementi — un {{htmlelement("p")}} e un {{htmlelement("button")}} — e inserirli nella pagina come figli del pannello `<div>`. Utilizziamo la loro proprietà [`Node.textContent`](/it/docs/Web/API/Node/textContent) — che rappresenta il contenuto di testo di un elemento — per inserire un messaggio nel paragrafo e una "x" nel pulsante. Questo pulsante sarà ciò che dovrà essere cliccato/attivato quando l'utente vorrà chiudere la finestra di messaggio.

```js
const msg = document.createElement("p");
msg.textContent = "This is a message box";
panel.appendChild(msg);

const closeBtn = document.createElement("button");
closeBtn.textContent = "x";
panel.appendChild(closeBtn);
```

Infine, chiamiamo [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) per aggiungere una funzione che verrà chiamata quando l'utente clicca sul pulsante "chiudi". Il codice eliminerà l'intero pannello dalla pagina per chiudere la finestra di messaggio.

Brevemente, il metodo `addEventListener()` è fornito dal pulsante (o in realtà, da qualsiasi elemento sulla pagina) e può ricevere una funzione e il nome di un evento. In questo caso, il nome dell'evento è 'click', il che significa che quando l'utente clicca sul pulsante, la funzione verrà eseguita. Imparerà molto di più sugli eventi nel nostro [articolo sugli eventi](/it/docs/Learn_web_development/Core/Scripting/Events). La riga all'interno della funzione utilizza la funzione dell'API DOM [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild) per specificare che vogliamo rimuovere un elemento figlio specifico dell'elemento HTML — in questo caso, il pannello `<div>`.

```js
closeBtn.addEventListener("click", () => panel.parentNode.removeChild(panel));
```

Fondamentalmente, questo intero blocco di codice sta generando un blocco di HTML che appare così e lo inserisce nella pagina:

```html
<div class="msgBox">
  <p>This is a message box</p>
  <button>x</button>
</div>
```

È stato necessario lavorare su molto codice — non si preoccupi troppo se non ricorda esattamente come funziona ogni parte di esso per ora! La parte principale su cui vogliamo concentrarci qui è la struttura e l'uso della funzione, ma volevamo mostrare qualcosa di interessante per questo esempio.

## Chiamare la funzione

Ora ha la definizione della sua funzione scritta nel suo elemento `<script>` correttamente, ma non farà nulla così com'è.

1. Provi a includere la seguente riga sotto la sua funzione per chiamarla:

   ```js
   displayMessage();
   ```

   Questa riga invoca la funzione, facendola eseguire immediatamente. Quando salva il suo codice e lo ricarica nel browser, vedrà immediatamente apparire la piccola finestra di messaggio, solo una volta. Dopo tutto, la stiamo chiamando solo una volta.

2. Ora apra gli strumenti di sviluppo del suo browser sulla pagina di esempio, vada alla console JavaScript e digiti nuovamente la riga, la vedrà comparire di nuovo! Quindi questo è divertente — ora abbiamo una funzione riutilizzabile che possiamo chiamare ogni volta che lo desideriamo.

   Ma probabilmente vogliamo che appaia in risposta a azioni dell'utente e del sistema. In una vera applicazione, una tale finestra di messaggio probabilmente verrebbe chiamata in risposta a nuovi dati disponibili, o a un errore, o all'utente che tenta di eliminare il proprio profilo ("è sicuro di questo?"), o all'utente che aggiunge un nuovo contatto e l'operazione è stata completata con successo, ecc.

   In questa dimostrazione, faremo apparire la finestra di messaggio quando l'utente clicca sul pulsante.

3. Cancelli la riga precedente che ha aggiunto.
4. Successivamente, selezioniamo il pulsante e memorizziamo un riferimento ad esso in una costante. Aggiunga la seguente riga al suo codice, sopra la definizione della funzione:

   ```js
   const btn = document.querySelector("button");
   ```

5. Infine, aggiunga la seguente riga sotto quella precedente:

   ```js
   btn.addEventListener("click", displayMessage);
   ```

   In modo simile al nostro gestore degli eventi click di closeBtn, qui stiamo chiamando del codice in risposta a un pulsante cliccato. Ma in questo caso, invece di chiamare una funzione anonima contenente del codice, stiamo chiamando la nostra funzione `displayMessage()` per nome.

6. Provi a salvare e ricaricare la pagina — ora dovrebbe vedere la finestra di messaggio apparire quando clicca sul pulsante.

Potreste chiedervi perché non abbiamo incluso le parentesi dopo il nome della funzione. Questo perché non vogliamo chiamare immediatamente la funzione — solo dopo che il pulsante è stato cliccato. Se provate a cambiare la riga in

```js
btn.addEventListener("click", displayMessage());
```

e a salvare e ricaricare, vedrete che la finestra di messaggio appare senza che il pulsante venga cliccato! Le parentesi in questo contesto sono talvolta chiamate l'"operatore di invocazione della funzione". Le si utilizzano solo quando si desidera eseguire la funzione immediatamente nell'ambito corrente. Allo stesso modo, il codice all'interno della funzione anonima non viene eseguito immediatamente, poiché è all'interno dell'ambito della funzione.

Se avete fatto l'ultimo esperimento, assicuratevi di annullare l'ultimo cambiamento prima di procedere.

## Migliorare la funzione con parametri

Così com'è, la funzione è ancora poco utile — non vogliamo solo mostrare lo stesso messaggio predefinito ogni volta. Miglioriamo la nostra funzione aggiungendo alcuni parametri, permettendoci di chiamarla con opzioni diverse.

1. Prima di tutto, aggiorni la prima riga della funzione:

   ```js
   function displayMessage() {
   ```

   a questo:

   ```js
   function displayMessage(msgText, msgType) {
   ```

   Ora, quando chiamiamo la funzione, possiamo fornire due valori di variabile all'interno delle parentesi per specificare il messaggio da visualizzare nella finestra di messaggio e il tipo di messaggio.

2. Per utilizzare il primo parametro, aggiorni la seguente riga all'interno della sua funzione:

   ```js
   msg.textContent = "This is a message box";
   ```

   a

   ```js
   msg.textContent = msgText;
   ```

3. Ultimo ma non meno importante, ora deve aggiornare la chiamata della sua funzione per includere del testo di messaggio aggiornato. Cambi la seguente riga:

   ```js
   btn.addEventListener("click", displayMessage);
   ```

   in questo blocco:

   ```js
   btn.addEventListener("click", () =>
     displayMessage("Woo, this is a different message!"),
   );
   ```

   Se vogliamo specificare parametri all'interno delle parentesi per la funzione che stiamo chiamando, non possiamo chiamarla direttamente — dobbiamo inserirla all'interno di una funzione anonima in modo che non sia nell'ambito immediato e quindi non venga chiamata immediatamente. Ora non verrà chiamata finché il pulsante non verrà cliccato.

4. Ricarichi e provi nuovamente il codice e vedrà che funziona ancora perfettamente, tranne che ora può anche variare il messaggio all'interno del parametro per visualizzare messaggi diversi nella finestra!

### Un parametro più complesso

Passiamo al parametro successivo. Questo coinvolgerà un po' più di lavoro: lo imposteremo in modo che a seconda di quale sia il parametro `msgType`, la funzione visualizzi un'icona diversa e un colore di sfondo diverso.

1. Prima di tutto, scarichi le icone necessarie per questo esercizio ([warning](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/icons/warning.png) e [chat](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/icons/chat.png)) da GitHub. Le salvi in una nuova cartella chiamata `icons` nella stessa posizione del suo file HTML.

   > [!NOTE]
   > Le icone di avviso e chat sono state originariamente trovate su [iconfinder.com](https://www.iconfinder.com/), e progettate da [Nazarrudin Ansyari](https://www.iconfinder.com/nazarr) — Grazie! (Le pagine reali delle icone sono state spostate o rimosse.)

2. Successivamente, trovi il CSS all'interno del suo file HTML. Faremo alcune modifiche per far spazio alle icone. Per prima cosa, aggiorni la larghezza di `.msgBox` da:

   ```css
   width: 200px;
   ```

   a

   ```css
   width: 242px;
   ```

3. Successivamente, aggiunga le seguenti righe all'interno della regola `.msgBox p { }`:

   ```css
   padding-left: 82px;
   background-position: 25px center;
   background-repeat: no-repeat;
   ```

4. Ora dobbiamo aggiungere del codice alla nostra funzione `displayMessage()` per gestire la visualizzazione delle icone. Aggiunga il seguente blocco appena sopra la parentesi graffa chiusa (`}`) della sua funzione:

   ```js
   if (msgType === "warning") {
     msg.style.backgroundImage = "url(icons/warning.png)";
     panel.style.backgroundColor = "red";
   } else if (msgType === "chat") {
     msg.style.backgroundImage = "url(icons/chat.png)";
     panel.style.backgroundColor = "aqua";
   } else {
     msg.style.paddingLeft = "20px";
   }
   ```

   Qui, se il parametro `msgType` è impostato come `'warning'`, viene visualizzata l'icona di avviso e il colore di sfondo del pannello è impostato su rosso. Se è impostato su `'chat'`, viene visualizzata l'icona di chat e il colore di sfondo del pannello è impostato su blu acquamarina. Se il parametro `msgType` non è impostato affatto (o su qualcosa di diverso), allora entra in gioco la parte `else { }` del codice, e il paragrafo riceve un padding predefinito e nessuna icona, senza alcun colore di sfondo del pannello impostato. Questo fornisce uno stato predefinito se non viene fornito alcun parametro `msgType`, il che significa che è un parametro opzionale!

5. Proviamo la nostra funzione aggiornata, provi a aggiornare la chiamata di `displayMessage()` da questo:

   ```js
   displayMessage("Woo, this is a different message!");
   ```

   a uno di questi:

   ```js
   displayMessage("Your inbox is almost full — delete some mails", "warning");
   displayMessage("Brian: Hi there, how are you today?", "chat");
   ```

   Può vedere quanto sta diventando utile la nostra funzione (ora non più così) piccola.

> [!NOTE]
> Se ha difficoltà a far funzionare l'esempio, si senta libero di confrontare il suo codice con la [versione completa su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-stage-4.html) (veda anche [l'esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/functions/function-stage-4.html)), o ci chieda aiuto.

## Testi le sue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver assimilato queste informazioni prima di proseguire — veda [Testi le sue abilità: Funzioni](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Functions). Questi test richiedono competenze che sono trattate nell'articolo successivo, quindi potrebbe voler leggere prima quello prima di provare il test.

## Sommario

Complimenti per aver raggiunto la fine! Questo articolo l'ha guidata attraverso l'intero processo di costruzione di una funzione personalizzata pratica, che con un po' più di lavoro potrebbe essere trapiantata in un progetto reale. Nell'articolo successivo, concluderemo le funzioni spiegando un altro concetto essenziale correlato — i valori di ritorno.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Functions","Learn_web_development/Core/Scripting/Return_values", "Learn_web_development/Core/Scripting")}}
