---
title: Introduzione allo scripting DOM
short-title: Scripting DOM
slug: Learn_web_development/Core/Scripting/DOM_scripting
l10n:
  sourceCommit: 273e96b5d57d1fe5210756edb145688e0bb04d3b
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Object_basics","Learn_web_development/Core/Scripting/Image_gallery", "Learn_web_development/Core/Scripting")}}

Durante la scrittura di pagine web e app, una delle operazioni più comuni consiste nel modificare in qualche modo la struttura del documento. Questo viene solitamente fatto manipolando il Document Object Model (DOM) tramite un insieme di API del browser integrate per controllare le informazioni HTML e di stile. In questo articolo verrà introdotto lo **scripting DOM**.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è il DOM: la rappresentazione interna del browser della struttura HTML del documento come gerarchia di oggetti.</li>
          <li>Le parti importanti di un browser web così come sono rappresentate in JavaScript: <code>Navigator</code>, <code>Window</code> e <code>Document</code>.</li>
          <li>Come i nodi DOM esistono in relazione tra loro nell'albero DOM: radice, genitore, figlio, fratello e discendente.</li>
          <li>Ottenere riferimenti ai nodi DOM, creare nuovi nodi, aggiungere e rimuovere nodi e attributi.</li>
          <li>Manipolare gli stili CSS con JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Le parti importanti di un browser web

I browser web sono software molto complessi con molte parti in movimento, molte delle quali non possono essere controllate o manipolate da uno sviluppatore web tramite JavaScript. Si potrebbe pensare che tali limitazioni siano negative, ma i browser sono protetti per buone ragioni, soprattutto legate alla sicurezza. Immaginare se un sito web potesse accedere alle password memorizzate o ad altre informazioni sensibili e accedere ai siti web fingendo di essere l'utente.

Nonostante le limitazioni, le API Web offrono comunque accesso a molte funzionalità che consentono di fare moltissime cose con le pagine web. Vi sono alcune parti molto evidenti a cui fare riferimento regolarmente nel codice: si consideri il diagramma seguente, che rappresenta le parti principali di un browser direttamente coinvolte nella visualizzazione delle pagine web:

![Parti importanti di un browser web; il document è la pagina web. Il window include l'intero document e anche la scheda. Il navigator è il browser, che include il window (che include il document) e tutte le altre finestre.](document-window-navigator.png)

- Il **window** rappresenta la scheda del browser in cui è caricata una pagina web; in JavaScript è rappresentato dall'oggetto [`Window`](/it/docs/Web/API/Window). Utilizzando i metodi disponibili su questo oggetto è possibile, ad esempio, restituire le dimensioni della finestra (vedere [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) e [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight)), manipolare il documento caricato in quella finestra, memorizzare sul lato client dati specifici di quel documento (ad esempio utilizzando un database locale o un altro meccanismo di archiviazione), collegare un [gestore di eventi](/it/docs/Learn_web_development/Core/Scripting/Events) alla finestra corrente e altro ancora.
- Il **navigator** rappresenta lo stato e l'identità del browser così come esiste sul web. In JavaScript è rappresentato dall'oggetto [`Navigator`](/it/docs/Web/API/Navigator). Questo oggetto può essere utilizzato per recuperare elementi quali la lingua preferita dell'utente, un flusso multimediale dalla webcam dell'utente e così via.
- Il **document** (rappresentato dal DOM nei browser) è la pagina effettivamente caricata nella finestra ed è rappresentato in JavaScript dall'oggetto [`Document`](/it/docs/Web/API/Document). Questo oggetto può essere utilizzato per recuperare e manipolare informazioni sull'HTML e sul CSS che compongono il documento, ad esempio ottenere un riferimento a un elemento nel DOM, modificarne il contenuto testuale, applicarvi nuovi stili, creare nuovi elementi e aggiungerli all'elemento corrente come figli oppure persino eliminarlo completamente.

In questo articolo verrà trattata principalmente la manipolazione del documento, ma verranno mostrati anche alcuni altri aspetti utili.

## Il modello a oggetti del documento

Facciamo un breve ripasso del Document Object Model (DOM), già trattato in precedenza nel corso. Il documento attualmente caricato in ciascuna scheda del browser è rappresentato da un DOM. Si tratta di una rappresentazione a "struttura ad albero" creata dal browser che consente di accedere facilmente alla struttura HTML tramite linguaggi di programmazione. Ad esempio, il browser stesso la utilizza per applicare stili e altre informazioni agli elementi corretti durante il rendering di una pagina, mentre gli sviluppatori possono manipolare il DOM con JavaScript dopo il rendering della pagina.

> [!NOTE]
> [The Document Object Model](https://scrimba.com/learn-javascript-c0v/~0g?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una pratica panoramica del termine "DOM" e del suo significato.

È stata creata una pagina di esempio in [dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) ([visualizzarla anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example.html)). Provare ad aprirla nel browser: è una pagina molto semplice contenente un elemento {{htmlelement("section")}}, al cui interno si trovano un'immagine e un paragrafo con un link. Il codice sorgente HTML è simile a questo:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>Simple DOM example</title>
  </head>
  <body>
    <section>
      <img
        src="dinosaur.png"
        alt="A red Tyrannosaurus Rex: A two legged dinosaur
        standing upright like a human, with small arms, and a
        large head with lots of sharp teeth." />
      <p>
        Here we will add a link to the
        <a href="https://www.mozilla.org/">Mozilla homepage</a>
      </p>
    </section>
  </body>
</html>
```

Il DOM, invece, ha questo aspetto:

![Rappresentazione a struttura ad albero del Document Object Model: il nodo superiore è il doctype e l'elemento HTML. I nodi figli di HTML includono head e body. Ogni elemento figlio è un ramo. Viene mostrato anche tutto il testo, inclusi gli spazi bianchi.](dom-screenshot.png)

> [!NOTE]
> Questo diagramma dell'albero DOM è stato creato utilizzando [Live DOM viewer](https://software.hixie.ch/utilities/js/live-dom-viewer/) di Ian Hickson.

Ogni voce nell'albero è chiamata **nodo**. Nel diagramma precedente si può vedere che alcuni nodi rappresentano elementi (identificati come `HTML`, `HEAD`, `META` e così via), mentre altri rappresentano testo (identificato come `#text`). Esistono anche [altri tipi di nodi](/it/docs/Web/API/Node/nodeType), ma questi sono quelli principali che si incontreranno.

I nodi vengono inoltre indicati in base alla loro posizione nell'albero rispetto ad altri nodi:

- **Nodo radice**: il nodo superiore nell'albero, che nel caso di HTML è sempre il nodo `HTML` (altri vocabolari di markup come SVG e XML personalizzato avranno elementi radice diversi).
- **Nodo figlio**: un nodo _direttamente_ all'interno di un altro nodo. Ad esempio, `IMG` è un figlio di `SECTION` nell'esempio precedente.
- **Nodo discendente**: un nodo _in qualsiasi posizione_ all'interno di un altro nodo. Ad esempio, `IMG` è un figlio di `SECTION` nell'esempio precedente ed è anche un discendente. `IMG` non è un figlio di `BODY`, poiché si trova due livelli più in basso nell'albero, ma è un discendente di `BODY`.
- **Nodo genitore**: un nodo che contiene un altro nodo. Ad esempio, `BODY` è il nodo genitore di `SECTION` nell'esempio precedente.
- **Nodi fratelli**: nodi che si trovano allo stesso livello sotto lo stesso nodo genitore nell'albero DOM. Ad esempio, `IMG` e `P` sono fratelli nell'esempio precedente.

È utile acquisire familiarità con questa terminologia prima di lavorare con il DOM, poiché diversi termini del codice che si incontreranno la utilizzano. Verranno inoltre incontrati in CSS (ad esempio, selettore discendente, selettore figlio).

## Eseguire alcune manipolazioni DOM di base

Per iniziare a conoscere la manipolazione del DOM, cominciamo con un esempio pratico.

1. Creare una copia locale della [pagina dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) e dell'[immagine](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dinosaur.png) associata.
2. Aggiungere un elemento `<script></script>` appena sopra il tag di chiusura `</body>`.
3. Per manipolare un elemento all'interno del DOM, è prima necessario selezionarlo e memorizzarne un riferimento in una variabile. All'interno dell'elemento script, aggiungere la riga seguente:

   ```js
   const link = document.querySelector("a");
   ```

4. Ora che il riferimento all'elemento è memorizzato in una variabile, è possibile iniziare a manipolarlo utilizzando le proprietà e i metodi disponibili (questi sono definiti su interfacce come [`HTMLAnchorElement`](/it/docs/Web/API/HTMLAnchorElement) nel caso dell'elemento {{htmlelement("a")}}, sulla sua interfaccia genitore più generale [`HTMLElement`](/it/docs/Web/API/HTMLElement) e su [`Node`](/it/docs/Web/API/Node), che rappresenta tutti i nodi in un DOM). Innanzitutto, modificare il testo all'interno del link aggiornando il valore della proprietà [`Node.textContent`](/it/docs/Web/API/Node/textContent). Aggiungere la riga seguente sotto quella precedente:

   ```js
   link.textContent = "Mozilla Developer Network";
   ```

5. Occorre inoltre modificare l'URL a cui punta il link, affinché non conduca al posto sbagliato quando viene selezionato. Aggiungere la riga seguente, di nuovo in fondo:

   ```js
   link.href = "https://developer.mozilla.org";
   ```

Si noti che, come per molte altre cose in JavaScript, esistono molti modi per selezionare un elemento e memorizzarne un riferimento in una variabile. [`Document.querySelector()`](/it/docs/Web/API/Document/querySelector) è l'approccio moderno consigliato. È pratico perché consente di selezionare elementi mediante selettori CSS. La chiamata `querySelector()` precedente corrisponderà al primo elemento {{htmlelement("a")}} visualizzato nel documento. Se si volessero trovare ed eseguire operazioni su più elementi, si potrebbe usare [`Document.querySelectorAll()`](/it/docs/Web/API/Document/querySelectorAll), che trova ogni elemento del documento corrispondente al selettore e ne memorizza i riferimenti in un oggetto simile a un [array](/it/docs/Learn_web_development/Core/Scripting/Arrays) chiamato [`NodeList`](/it/docs/Web/API/NodeList).

Sono disponibili metodi meno recenti per ottenere riferimenti agli elementi, quali:

- [`Document.getElementById()`](/it/docs/Web/API/Document/getElementById), che seleziona un elemento con un determinato valore dell'attributo `id`, ad esempio `<p id="myId">My paragraph</p>`. L'ID viene passato alla funzione come parametro, ovvero `const elementRef = document.getElementById('myId')`.
- [`Document.getElementsByTagName()`](/it/docs/Web/API/Document/getElementsByTagName), che restituisce un oggetto simile a un array contenente tutti gli elementi della pagina di un determinato tipo, ad esempio `<p>`, `<a>` e così via. Il tipo di elemento viene passato alla funzione come parametro, ovvero `const elementRefArray = document.getElementsByTagName('p')`.

Questi due metodi funzionano meglio nei browser meno recenti rispetto ai metodi moderni come `querySelector()`, ma non sono altrettanto pratici. Esaminarli e cercare quali altri metodi sono disponibili.

### Creare e inserire nuovi nodi

Quanto visto finora offre un piccolo assaggio di ciò che è possibile fare, ma andiamo oltre e vediamo come creare nuovi elementi.

1. Tornando all'esempio corrente, iniziare ottenendo un riferimento all'elemento {{htmlelement("section")}}. Aggiungere il codice seguente in fondo allo script esistente (fare lo stesso anche con le altre righe):

   ```js
   const sect = document.querySelector("section");
   ```

2. Ora creare un nuovo paragrafo utilizzando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e assegnargli del contenuto testuale nello stesso modo di prima:

   ```js
   const para = document.createElement("p");
   para.textContent = "We hope you enjoyed the ride.";
   ```

3. Ora è possibile aggiungere il nuovo paragrafo alla fine della sezione utilizzando [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild):

   ```js
   sect.appendChild(para);
   ```

4. Infine, per questa parte, aggiungere un nodo di testo al paragrafo in cui si trova il link, per completare bene la frase. Per prima cosa verrà creato il nodo di testo utilizzando [`Document.createTextNode()`](/it/docs/Web/API/Document/createTextNode):

   ```js
   const text = document.createTextNode(
     " — the premier source for web development knowledge.",
   );
   ```

5. Ora ottenere un riferimento al paragrafo in cui si trova il link e aggiungervi il nodo di testo:

   ```js
   const linkPara = document.querySelector("p");
   linkPara.appendChild(text);
   ```

Questo è quasi tutto ciò che serve per aggiungere nodi al DOM: questi metodi verranno utilizzati spesso durante la creazione di interfacce dinamiche (alcuni esempi verranno esaminati in seguito).

### Spostare e rimuovere elementi

In alcuni casi potrebbe essere necessario spostare nodi oppure eliminarli completamente dal DOM. Questo è perfettamente possibile.

Se si volesse spostare il paragrafo contenente il link in fondo alla sezione, si potrebbe fare così:

```js
sect.appendChild(linkPara);
```

Questo sposta il paragrafo in fondo alla sezione. Si potrebbe pensare che venga creata una seconda copia, ma non è così: `linkPara` è un riferimento all'unica copia di quel paragrafo. Se si volesse creare una copia e aggiungere anche quella, sarebbe necessario usare [`Node.cloneNode()`](/it/docs/Web/API/Node/cloneNode).

Anche rimuovere un nodo è abbastanza semplice, almeno quando si dispone di un riferimento al nodo da rimuovere e al suo genitore. Nel caso corrente, basta usare [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild), in questo modo:

```js
sect.removeChild(linkPara);
```

Quando si desidera rimuovere un nodo basandosi solo su un riferimento a sé stesso, situazione piuttosto comune, è possibile usare [`Element.remove()`](/it/docs/Web/API/Element/remove):

```js
linkPara.remove();
```

Questo metodo non è supportato nei browser meno recenti. Questi non dispongono di un metodo per dire a un nodo di rimuoversi da solo, quindi sarebbe necessario fare quanto segue:

```js
linkPara.parentNode.removeChild(linkPara);
```

Provare ad aggiungere le righe precedenti al codice.

### Manipolare gli stili

È possibile manipolare gli stili CSS tramite JavaScript in vari modi.

Per cominciare, è possibile ottenere un elenco di tutti i fogli di stile collegati a un documento utilizzando [`Document.styleSheets`](/it/docs/Web/API/Document/styleSheets), che restituisce un oggetto simile a un array contenente oggetti [`CSSStyleSheet`](/it/docs/Web/API/CSSStyleSheet). È quindi possibile aggiungere o rimuovere stili a piacere. Tuttavia, queste funzionalità non verranno approfondite perché rappresentano un modo piuttosto arcaico e difficile di manipolare gli stili. Esistono modi molto più semplici.

Il primo modo consiste nell'aggiungere stili inline direttamente agli elementi che si desidera stilizzare dinamicamente. Ciò avviene tramite la proprietà [`HTMLElement.style`](/it/docs/Web/API/HTMLElement/style), che contiene le informazioni di stile inline per ciascun elemento del documento. È possibile impostare le proprietà di questo oggetto per aggiornare direttamente gli stili dell'elemento.

1. Come esempio, provare ad aggiungere queste righe all'esempio in corso:

   ```js
   para.style.color = "white";
   para.style.backgroundColor = "black";
   para.style.padding = "10px";
   para.style.width = "250px";
   para.style.textAlign = "center";
   ```

2. Ricaricare la pagina e si vedrà che gli stili sono stati applicati al paragrafo. Osservando quel paragrafo nell'[Ispettore pagina/ispettore DOM](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/index.html) del browser, si potrà vedere che queste righe aggiungono effettivamente stili inline al documento:

   ```html
   <p
     style="color: white; background-color: black; padding: 10px; width: 250px; text-align: center;">
     We hope you enjoyed the ride.
   </p>
   ```

> [!NOTE]
> Si noti come le versioni delle proprietà JavaScript degli stili CSS siano scritte in {{Glossary("camel_case", "camel case minuscolo")}}, mentre le versioni CSS usano trattini ({{Glossary("kebab_case", "kebab-case")}}) (ad esempio, `backgroundColor` rispetto a `background-color`). Assicurarsi di non confonderle, altrimenti non funzionerà.

Esiste un altro modo comune per manipolare dinamicamente gli stili del documento: scrivere gli stili in un foglio di stile separato e fare riferimento a tali stili aggiungendo o rimuovendo un nome di classe.

1. Eliminare le cinque righe precedentemente aggiunte a JavaScript.
2. Aggiungere il seguente codice all'interno dell'elemento HTML {{htmlelement("head")}}:

   ```html
   <style>
     .highlight {
       color: white;
       background-color: black;
       padding: 10px;
       width: 250px;
       text-align: center;
     }
   </style>
   ```

3. Per aggiungere questo nome di classe all'elemento, usare il metodo `add()` di [`classList`](/it/docs/Web/API/Element/classList) dell'elemento:

   ```js
   para.classList.add("highlight");
   ```

4. Aggiornare la pagina e non si noterà alcun cambiamento: il CSS viene ancora applicato al paragrafo, ma questa volta assegnandogli una classe selezionata dalla regola CSS, non come stili CSS inline.

La scelta del metodo dipende dalle necessità; entrambi presentano vantaggi e svantaggi. Il primo metodo richiede meno configurazione ed è adatto a utilizzi semplici, mentre il secondo è più ortodosso (nessuna mescolanza di CSS e JavaScript, nessuno stile inline, considerato una cattiva pratica). Man mano che si inizieranno a creare app più grandi e complesse, probabilmente si userà maggiormente il secondo metodo, ma la scelta dipende effettivamente dalle preferenze.

A questo punto, non è stato fatto nulla di realmente utile. Non ha senso usare JavaScript per creare contenuto statico: tanto vale scriverlo direttamente nell'HTML e non usare JavaScript. È più complesso dell'HTML e creare contenuto con JavaScript comporta anche altri problemi, come il fatto di non essere leggibile dai motori di ricerca.

Nella prossima sezione verrà esaminato un utilizzo più pratico delle API DOM.

> [!NOTE]
> La [versione completata della demo dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example-manipulated.html) è disponibile su GitHub ([visualizzarla anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example-manipulated.html)).

## Creare una lista della spesa dinamica

In questo esercizio, occorre creare una lista della spesa dinamica che permetta di aggiungere elementi tramite un campo di input di un modulo e un pulsante. Dopo aver digitato un elemento nel campo di input e aver selezionato il pulsante o premuto il tasto <kbd>Enter</kbd>, dovrebbe accadere quanto segue:

- L'elemento dovrebbe comparire nell'elenco.
- Ogni elemento dovrebbe avere accanto un pulsante che lo rimuove dall'elenco quando viene selezionato.
- I campi di input dovrebbero essere svuotati e ricevere il focus, pronti per l'immissione dell'elemento successivo.

La demo completata avrà un aspetto simile al seguente: provarla prima di crearla.

```html hidden live-sample___dynamic-shopping-list
<h1>My shopping list</h1>

<form>
  <label for="item">Enter a new item:</label>
  <input type="text" name="item" id="item" />
  <button>Add item</button>
</form>

<ul></ul>
```

```css hidden live-sample___dynamic-shopping-list
li {
  margin-bottom: 10px;
}

li button {
  font-size: 12px;
  margin-left: 20px;
}
```

```js hidden live-sample___dynamic-shopping-list
const list = document.querySelector("ul");
const input = document.querySelector("input");
const button = document.querySelector("button");

button.addEventListener("click", (event) => {
  event.preventDefault();

  const myItem = input.value;
  input.value = "";

  const listItem = document.createElement("li");
  const listText = document.createElement("span");
  const listBtn = document.createElement("button");

  listItem.appendChild(listText);
  listText.textContent = myItem;
  listItem.appendChild(listBtn);
  listBtn.textContent = "Delete";
  list.appendChild(listItem);

  listBtn.addEventListener("click", () => {
    list.removeChild(listItem);
  });

  input.focus();
});
```

{{EmbedLiveSample("dynamic-shopping-list", "100%", 300)}}

Per completare l'esercizio, seguire i passaggi riportati di seguito e assicurarsi che l'elenco si comporti come descritto sopra.

1. Per iniziare, scaricare una copia del file iniziale [shopping-list.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/shopping-list.html) e copiarlo in una posizione a scelta. Si vedrà che contiene del CSS minimo, un modulo con etichetta, input e pulsante, un elenco vuoto e un elemento {{htmlelement("script")}}. Tutte le aggiunte andranno effettuate all'interno dello script.
2. Creare tre variabili che contengano riferimenti agli elementi elenco ({{htmlelement("ul")}}), {{htmlelement("input")}} e {{htmlelement("button")}}.
3. Creare una [funzione](/it/docs/Learn_web_development/Core/Scripting/Functions) che verrà eseguita in risposta alla selezione del pulsante.
4. All'interno del corpo della funzione, iniziare chiamando [`preventDefault()`](/it/docs/Web/API/Event/preventDefault). Poiché l'input è racchiuso in un elemento modulo, premendo il tasto <kbd>Enter</kbd> verrà attivato l'invio del modulo. La chiamata a `preventDefault()` impedirà al modulo di aggiornare la pagina, in modo che sia invece possibile aggiungere un nuovo elemento all'elenco.
5. Continuare memorizzando il [valore](/it/docs/Web/API/HTMLInputElement/value) corrente dell'input in una variabile.
6. Quindi, svuotare l'elemento input impostandone il valore su una stringa vuota (`""`).
7. Creare tre nuovi elementi, un elemento della lista ({{htmlelement('li')}}), un elemento {{htmlelement('span')}} e un elemento {{htmlelement('button')}}, e memorizzarli in variabili.
8. Aggiungere lo span e il pulsante come figli dell'elemento della lista.
9. Impostare il contenuto testuale dello span sul valore dell'input salvato in precedenza e il contenuto testuale del pulsante su `Delete`.
10. Aggiungere l'elemento della lista all'elenco.
11. Collegare un gestore di eventi al pulsante **Delete** affinché, quando viene selezionato, rimuova l'intero elemento della lista (`<li>...</li>`).
12. Infine, usare il metodo [`focus()`](/it/docs/Web/API/HTMLElement/focus) per portare il focus sull'elemento input, così che sia pronto per l'inserimento del successivo elemento della lista della spesa.

## Riepilogo

Si è giunti alla fine dello studio sulla manipolazione di documenti e DOM. A questo punto dovrebbe essere chiaro quali sono le parti importanti di un browser web per quanto riguarda il controllo dei documenti e altri aspetti dell'esperienza web dell'utente. Soprattutto, dovrebbe essere chiaro che cos'è il Document Object Model e come manipolarlo per creare funzionalità utili.

## Vedere anche

- Esistono molte altre funzionalità utilizzabili per manipolare i documenti. Consultare alcuni dei riferimenti seguenti e scoprire cosa è possibile fare:
  - [`Document`](/it/docs/Web/API/Document)
  - [`Window`](/it/docs/Web/API/Window)
  - [`Node`](/it/docs/Web/API/Node)
  - [`HTMLElement`](/it/docs/Web/API/HTMLElement), [`HTMLInputElement`](/it/docs/Web/API/HTMLInputElement), [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement), ecc.
- [DOM Scripting](https://explainers.dev/dom-scripting/), explainers.dev

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Object_basics","Learn_web_development/Core/Scripting/Image_gallery", "Learn_web_development/Core/Scripting")}}
