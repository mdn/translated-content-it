---
title: Costruisci la tua funzione
slug: Learn_web_development/Core/Scripting/Build_your_own_function
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Functions","Learn_web_development/Core/Scripting/Return_values", "Learn_web_development/Core/Scripting")}}

Con la maggior parte della teoria essenziale affrontata nell'articolo precedente, questo articolo fornisce esperienza pratica. Qui farai pratica costruendo la tua funzione personalizzata. Lungo il percorso, spiegheremo anche alcuni dettagli utili per lavorare con le funzioni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi delle funzioni JavaScript come trattato nella lezione precedente.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Esperienza nella costruzione delle proprie funzioni personalizzate.</li>
          <li>Aggiungere parametri alle proprie funzioni.</li>
          <li>Chiamare la propria funzione.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Apprendimento attivo: Costruiamo una funzione

La funzione personalizzata che andremo a costruire si chiamerà `displayMessage()`. Visualizzerà una casella di messaggio personalizzata in una pagina web e fungerà da sostituto personalizzato per la funzione incorporata del browser [`alert()`](/it/docs/Web/API/Window/alert). Lo abbiamo già visto, ma rinfreschiamo la memoria. Scrivi quanto segue nella console JavaScript del tuo browser, su qualsiasi pagina tu voglia:

```js
alert("This is a message");
```

La funzione `alert()` prende un singolo argomento: la stringa che viene mostrata nella casella di allerta. Prova a variare la stringa per cambiare il messaggio.

La funzione `alert()` è limitata: puoi modificare il messaggio, ma non puoi facilmente variare nient'altro, come il colore, l'icona o qualsiasi altra cosa. Ne costruiremo una che sarà più divertente.

> [!NOTE]
> Questo esempio dovrebbe funzionare bene in tutti i browser moderni, ma la formattazione potrebbe sembrare un po' strana nei browser leggermente più vecchi. Ti consigliamo di fare questo esercizio in un browser moderno come Firefox, Opera o Chrome.

## La funzione di base

Per iniziare, mettiamo insieme una funzione di base.

> [!NOTE]
> Per le convenzioni di denominazione delle funzioni, dovresti seguire le stesse regole delle [convenzioni di denominazione delle variabili](/it/docs/Learn_web_development/Core/Scripting/Variables#an_aside_on_variable_naming_rules). Questo va bene, poiché puoi distinguerle: i nomi delle funzioni appaiono con le parentesi dopo, e le variabili no.

1. Inizia accedendo al file [function-start.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-start.html) e facendone una copia locale. Vedrai che l'HTML è semplice: il corpo contiene solo un singolo pulsante. Abbiamo anche fornito un po' di CSS di base per formattare la casella di messaggio personalizzata e un elemento vuoto {{htmlelement("script")}} in cui inserire il nostro JavaScript.
2. Successivamente, aggiungi quanto segue all'interno dell'elemento `<script>`:

   ```js
   function displayMessage() {
     // …
   }
   ```

   Iniziamo con la parola chiave `function`, che significa che stiamo definendo una funzione. Questo è seguito dal nome che vogliamo dare alla nostra funzione, un set di parentesi e un set di parentesi graffe. Eventuali parametri che vogliamo dare alla nostra funzione vanno all'interno delle parentesi, e il codice che viene eseguito quando chiamiamo la funzione va all'interno delle parentesi graffe.

3. Infine, aggiungi il seguente codice all'interno delle parentesi graffe:

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

Questo è parecchio codice da esaminare, quindi lo esamineremo passo dopo passo.

La prima riga seleziona l'elemento {{htmlelement("body")}} utilizzando l'API [DOM](/it/docs/Web/API/Document_Object_Model) per ottenere la proprietà [`body`](/it/docs/Web/API/Document/body) dell'oggetto globale [`document`](/it/docs/Web/API/Document/body), e assegnandola a una costante chiamata `body`, così possiamo farci delle cose più tardi:

```js
const body = document.body;
```

La sezione successiva utilizza una funzione dell'API DOM chiamata [`document.createElement()`](/it/docs/Web/API/Document/createElement) per creare un elemento {{htmlelement("div")}} e memorizzarne un riferimento in una costante chiamata `panel`. Questo elemento sarà il contenitore esterno della nostra casella di messaggio.

Poi usiamo un'altra funzione dell'API DOM chiamata [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute) per impostare un attributo `class` sul nostro pannello con un valore di `msgBox`. Questo per rendere più facile formattare l'elemento: se guardi il CSS sulla pagina, vedrai che stiamo usando un selettore di classe `.msgBox` per formattare la casella di messaggio e il suo contenuto.

Infine, chiamiamo una funzione DOM chiamata [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild) sulla costante `body` che abbiamo memorizzato prima, che annida un elemento all'interno di un altro come suo figlio. Specifichiamo il `<div>` del pannello come il figlio che vogliamo aggiungere all'interno dell'elemento `<body>`. Dobbiamo farlo poiché l'elemento creato non apparirà semplicemente sulla pagina da solo: dobbiamo specificare dove metterlo.

```js
const panel = document.createElement("div");
panel.setAttribute("class", "msgBox");
body.appendChild(panel);
```

Le successive due sezioni utilizzano le stesse funzioni `createElement()` e `appendChild()` che abbiamo già visto per creare due nuovi elementi — un {{htmlelement("p")}} e un {{htmlelement("button")}} — e inserirli nella pagina come figli del `<div>` del pannello. Usiamo la loro proprietà [`Node.textContent`](/it/docs/Web/API/Node/textContent) — che rappresenta il contenuto testuale di un elemento — per inserire un messaggio all'interno del paragrafo e una "x" all'interno del pulsante. Questo pulsante sarà ciò che deve essere cliccato/attivato quando l'utente vuole chiudere la casella di messaggio.

```js
const msg = document.createElement("p");
msg.textContent = "This is a message box";
panel.appendChild(msg);

const closeBtn = document.createElement("button");
closeBtn.textContent = "x";
panel.appendChild(closeBtn);
```

Infine, chiamiamo [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) per aggiungere una funzione che verrà chiamata quando l'utente clicca sul pulsante "close". Il codice eliminerà l'intero pannello dalla pagina — per chiudere la casella di messaggio.

Brevemente, il metodo `addEventListener()` è fornito dal pulsante (o in realtà, qualsiasi elemento sulla pagina) che può ricevere una funzione e il nome di un evento. In questo caso, il nome dell'evento è 'click', il che significa che quando l'utente clicca il pulsante, la funzione verrà eseguita. Imparerai molto di più sugli eventi nel nostro [articolo sugli eventi](/it/docs/Learn_web_development/Core/Scripting/Events). La riga all'interno della funzione usa la funzione DOM [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild) per specificare che vogliamo rimuovere un elemento figlio specifico dell'elemento HTML — in questo caso, il `<div>` del pannello.

```js
closeBtn.addEventListener("click", () => panel.parentNode.removeChild(panel));
```

Fondamentalmente, tutto questo blocco di codice genera un blocco di HTML che appare così, e lo inserisce nella pagina:

```html
<div class="msgBox">
  <p>This is a message box</p>
  <button>x</button>
</div>
```

È stato un bel po' di codice da analizzare — non preoccuparti troppo se non ricordi esattamente come funziona ogni sua parte adesso! La parte principale su cui vogliamo concentrarci qui è la struttura e l'uso della funzione, ma volevamo mostrare qualcosa di interessante per questo esempio.

## Chiamare la funzione

Hai ora la definizione della tua funzione scritta nel tuo elemento `<script>` correttamente, ma non farà nulla così com'è.

1. Prova a includere la seguente riga sotto la tua funzione per chiamarla:

   ```js
   displayMessage();
   ```

   Questa linea invoca la funzione, facendo sì che venga eseguita immediatamente. Quando salvi il tuo codice e lo ricarichi nel browser, vedrai la piccola casella di messaggio apparire immediatamente, solo una volta. Dopotutto stiamo chiamandola solo una volta.

2. Ora apri gli strumenti per sviluppatori del tuo browser sulla pagina di esempio, vai alla console JavaScript e scrivi di nuovo la linea lì, la vedrai apparire di nuovo! Quindi questo è divertente — ora abbiamo una funzione riutilizzabile che possiamo chiamare ogni volta che vogliamo.

   Ma probabilmente vogliamo che appaia in risposta ad azioni dell'utente e del sistema. In un'applicazione reale, una tale casella di messaggio probabilmente verrebbe chiamata in risposta a nuovi dati disponibili, o a un errore verificatosi, o all'utente che cerca di eliminare il proprio profilo ("sei sicuro di questo?"), o all'utente che aggiunge un nuovo contatto e l'operazione viene completata con successo, ecc.

   In questa demo, faremo apparire la casella di messaggio quando l'utente clicca il pulsante.

3. Elimina la riga precedente che hai aggiunto.
4. Successivamente, selezioneremo il pulsante e memorizzeremo un riferimento a esso in una costante. Aggiungi la seguente riga al tuo codice, sopra la definizione della funzione:

   ```js
   const btn = document.querySelector("button");
   ```

5. Infine, aggiungi la seguente riga sotto la precedente:

   ```js
   btn.addEventListener("click", displayMessage);
   ```

   In modo simile al gestore degli eventi di click del nostro closeBtn, qui stiamo chiamando del codice in risposta a un pulsante che viene cliccato. Ma in questo caso, invece di chiamare una funzione anonima che contiene del codice, stiamo chiamando la nostra funzione `displayMessage()` per nome.

6. Prova a salvare e aggiornare la pagina — ora dovresti vedere la casella di messaggio apparire quando clicchi il pulsante.

Potresti chiederti perché non abbiamo incluso le parentesi dopo il nome della funzione. Questo perché non vogliamo chiamare la funzione immediatamente — solo dopo che il pulsante è stato cliccato. Se provi a cambiare la linea in

```js
btn.addEventListener("click", displayMessage());
```

e a salvare e ricaricare, vedrai che la casella di messaggio appare senza che il pulsante venga cliccato! Le parentesi in questo contesto sono talvolta chiamate "operatore di invocazione della funzione". Le usi solo quando vuoi eseguire la funzione immediatamente nell'ambito corrente. Nello stesso modo, il codice all'interno della funzione anonima non viene eseguito immediatamente, poiché è all'interno dell'ambito della funzione.

Se hai provato l'ultimo esperimento, assicurati di annullare l'ultima modifica prima di continuare.

## Migliorare la funzione con parametri

Così com'è, la funzione non è ancora molto utile — non vogliamo mostrare semplicemente lo stesso messaggio predefinito ogni volta. Miglioriamo la nostra funzione aggiungendo alcuni parametri, permettendoci di chiamarla con opzioni diverse.

1. Prima di tutto, aggiorna la prima riga della funzione:

   ```js
   function displayMessage() {
   ```

   a questo:

   ```js
   function displayMessage(msgText, msgType) {
   ```

   Ora, quando chiamiamo la funzione, possiamo fornire due valori variabili all'interno delle parentesi per specificare il messaggio da visualizzare nella casella di messaggio e il tipo di messaggio che è.

2. Per utilizzare il primo parametro, aggiorna la seguente riga all'interno della tua funzione:

   ```js
   msg.textContent = "This is a message box";
   ```

   a

   ```js
   msg.textContent = msgText;
   ```

3. Infine, devi ora aggiornare la chiamata alla tua funzione per includere del testo di messaggio aggiornato. Cambia la seguente riga:

   ```js
   btn.addEventListener("click", displayMessage);
   ```

   in questo blocco:

   ```js
   btn.addEventListener("click", () =>
     displayMessage("Woo, this is a different message!"),
   );
   ```

   Se vogliamo specificare dei parametri all'interno delle parentesi per la funzione che stiamo chiamando, allora non possiamo chiamarla direttamente — dobbiamo metterla all'interno di una funzione anonima in modo che non sia nell'ambito immediato e quindi non venga chiamata immediatamente. Ora non verrà chiamata finché il pulsante non verrà cliccato.

4. Ricarica e prova di nuovo il codice e vedrai che funziona ancora tutto correttamente, tranne che ora puoi anche variare il messaggio all'interno del parametro per ottenere messaggi diversi visualizzati nella casella!

### Un parametro più complesso

Passiamo al prossimo parametro. Questo richiederà un po' più di lavoro — imposteremo in modo che a seconda di cosa è impostato il parametro `msgType`, la funzione visualizzerà un'icona diversa e un diverso colore di sfondo.

1. Prima di tutto, scarica le icone necessarie per questo esercizio ([warning](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/icons/warning.png) e [chat](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/icons/chat.png)) da GitHub. Salvale in una nuova cartella chiamata `icons` nello stesso luogo del tuo file HTML.

   > [!NOTE]
   > Le icone di avviso e chat sono state originariamente trovate su [iconfinder.com](https://www.iconfinder.com/), e progettate da [Nazarrudin Ansyari](https://www.iconfinder.com/nazarr) — Grazie! (Le pagine delle icone effettive sono state successivamente spostate o rimosse.)

2. Successivamente, trova il CSS all'interno del tuo file HTML. Faremo alcune modifiche per far spazio alle icone. Per prima cosa, aggiorna la larghezza di `.msgBox` da:

   ```css
   width: 200px;
   ```

   a

   ```css
   width: 242px;
   ```

3. Successivamente, aggiungi le seguenti righe all'interno della regola `.msgBox p { }`:

   ```css
   padding-left: 82px;
   background-position: 25px center;
   background-repeat: no-repeat;
   ```

4. Ora dobbiamo aggiungere del codice alla nostra funzione `displayMessage()` per gestire la visualizzazione delle icone. Aggiungi il seguente blocco appena sopra la parentesi graffa chiusa (`}`) della tua funzione:

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

   Qui, se il parametro `msgType` è impostato su `'warning'`, viene visualizzata l'icona di avviso e il colore di sfondo del pannello viene impostato su rosso. Se è impostato su `'chat'`, viene visualizzata l'icona di chat e il colore di sfondo del pannello viene impostato su blu acqua. Se il parametro `msgType` non è impostato affatto (o su qualcosa di diverso), allora entra in gioco la parte di codice `else { }`, e al paragrafo viene assegnato un padding predefinito e nessuna icona, senza che venga impostato nemmeno il colore del pannello di sfondo. Questo fornisce uno stato predefinito se non viene fornito alcun parametro `msgType`, il che significa che è un parametro opzionale!

5. Testiamo la nostra funzione aggiornata, prova a aggiornare la chiamata a `displayMessage()` da questo:

   ```js
   displayMessage("Woo, this is a different message!");
   ```

   a uno di questi:

   ```js
   displayMessage("Your inbox is almost full — delete some mails", "warning");
   displayMessage("Brian: Hi there, how are you today?", "chat");
   ```

   Puoi vedere quanto utile sta diventando la nostra (ora non così) piccola funzione.

> [!NOTE]
> Se hai difficoltà a far funzionare l'esempio, sentiti libero di controllare il tuo codice rispetto alla [versione finale su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-stage-4.html) (vedi anche [eseguirla dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/functions/function-stage-4.html)), o chiedici aiuto.

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare se hai assimilato queste informazioni prima di passare oltre — vedi [Metti alla prova le tue abilità: Funzioni](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Functions). Questi test richiedono abilità che sono coperte nell'articolo successivo, quindi potresti volerlo leggere prima di provare il test.

## Riassunto

Congratulazioni per essere arrivato alla fine! Questo articolo ti ha guidato attraverso l'intero processo di costruzione di una funzione personalizzata pratica, che con un po' più di lavoro potrebbe essere trapiantata in un progetto reale. Nell'articolo successivo, concluderemo sulle funzioni spiegando un altro concetto correlato essenziale — i valori di ritorno.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Functions","Learn_web_development/Core/Scripting/Return_values", "Learn_web_development/Core/Scripting")}}
