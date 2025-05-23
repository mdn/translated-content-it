---
title: Cosa sono gli strumenti di sviluppo del browser?
slug: Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Ogni browser web moderno include una potente suite di strumenti di sviluppo. Questi strumenti svolgono una serie di funzioni, dall'ispezione di HTML, CSS e JavaScript attualmente caricati alla visualizzazione delle risorse richieste dalla pagina e di quanto tempo hanno impiegato per caricarsi. Questo articolo spiega come utilizzare le funzioni di base degli strumenti di sviluppo del browser.

> [!NOTE]
> Prima di seguire gli esempi riportati di seguito, apri il [sito di esempio per principianti](https://mdn.github.io/beginner-html-site-scripted/) che abbiamo costruito durante la serie di articoli [Primi passi con il Web](/it/docs/Learn_web_development/Getting_started/Your_first_website). Dovresti avere questo aperto mentre segui i passaggi riportati di seguito.

## Come aprire gli strumenti di sviluppo nel tuo browser

Gli strumenti di sviluppo sono ospitati all'interno del tuo browser in una sottofinestra che appare più o meno così, a seconda del browser che stai utilizzando:

![Screenshot di un browser con strumenti di sviluppo aperti. La pagina web è mostrata nella metà superiore del browser, mentre gli strumenti di sviluppo occupano la metà inferiore. Ci sono tre pannelli aperti negli strumenti di sviluppo: HTML, con l'elemento body selezionato, un pannello CSS che mostra i blocchi di stili che si applicano al body evidenziato, e un pannello di stili calcolati che mostra tutti gli stili dell'autore; la casella degli stili del browser non è selezionata.](devtools_63_inspector.png)

Come si aprono? Tre modi:

- **_Tastiera:_**

  - **Windows:** <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>I</kbd> o <kbd>F12</kbd>
  - **macOS:** <kbd>⌘</kbd> + <kbd>⌥</kbd> + <kbd>I</kbd>

- **_Barra dei menu:_**

  - **Firefox:** _Menu (☰) ➤ Altri strumenti ➤ Strumenti di sviluppo web_
  - **Chrome:** _Altri strumenti ➤ Strumenti per sviluppatori_
  - **Opera**: _Sviluppatore ➤ Strumenti per sviluppatori_
  - **Safari:** _Sviluppo ➤ Mostra Web Inspector._

    > [!NOTE]
    > Gli strumenti di sviluppo di Safari non sono abilitati per impostazione predefinita.
    > Per abilitarli, vai su _Safari ➤ Preferenze ➤ Avanzate_, e seleziona la casella _Mostra menu Sviluppo nella barra dei menu_ o _Abilita funzionalità per sviluppatori web_.

- **_Menu contestuale:_** Premi e tieni premuto/opzione di tasto destro su un elemento di una pagina web (Ctrl-click su Mac), e scegli _Ispeziona Elemento_ dal menu contestuale che appare. (_Un vantaggio aggiunto:_ questo metodo evidenzia direttamente il codice dell'elemento che hai cliccato con il tasto destro.)

![Il logo di Firefox come elemento DOM in un sito web di esempio con un menu contestuale mostrato. Un menu contestuale appare quando qualsiasi elemento sulla pagina web viene cliccato con il tasto destro. L'ultimo elemento del menu è 'Ispeziona elemento'.](inspector_context.png)

## L’Inspector: Esploratore DOM e editor CSS

Gli strumenti di sviluppo solitamente si aprono di default sull'inspector, che appare come lo screenshot seguente. Questo strumento mostra come appare l'HTML sulla tua pagina a runtime, così come quali CSS vengono applicati a ciascun elemento sulla pagina. Ti consente inoltre di modificare immediatamente l'HTML e il CSS e vedere i risultati delle tue modifiche riflessi in tempo reale nella finestra del browser.

![Un sito web di test è aperto in una scheda nel browser. La sottofinestra degli strumenti di sviluppo del browser è aperta. Gli strumenti di sviluppo hanno diverse schede. Inspector è una di quelle schede. La scheda Inspector visualizza il codice HTML del sito web. Un tag immagine è selezionato dal codice HTML. Questo risulta nell'evidenziazione dell'immagine corrispondente al tag selezionato nel sito web.](inspector_highlighted.png)

Se _non_ vedi l’inspector,

- **Firefox:** Seleziona la scheda **Inspector**.
- **Altri browser:** Seleziona la scheda **Elements**.

### Esplorare l’inspector DOM

Per iniziare, fai clic con il tasto destro (Ctrl-click) su un elemento HTML nell’inspector DOM e guarda il menu contestuale. Le opzioni di menu disponibili variano tra i browser, ma le più importanti sono generalmente le stesse:

![La sottofinestra degli strumenti di sviluppo del browser è aperta. La scheda inspector è selezionata. Un elemento link è cliccato con il tasto destro dal codice HTML disponibile nella scheda inspector. Appare un menu contestuale. Le opzioni di menu disponibili variano tra i browser, ma le più importanti sono generalmente le stesse.](dom_inspector.png)

- **Delete Node** (a volte _Delete Element_). Cancella l'elemento attuale.
- **Edit as HTML** (a volte _Add attribute_/_Edit text_). Ti permette di cambiare l'HTML e vedere i risultati al volo. Molto utile per il debugging e il collaudo.
- **:hover/:active/:focus**. Forza l'attivazione degli stati degli elementi, così puoi vedere come apparirebbe lo styling.
- **Copy/Copy as HTML**. Copia l'HTML attualmente selezionato.
- Alcuni browser offrono anche _Copy CSS Path_ e _Copy XPath_ disponibili, per permetterti di copiare il selettore CSS o l'espressione XPath che selezionerebbe l'elemento HTML corrente.

Prova a modificare parte del tuo DOM ora. Fai doppio clic su un elemento o cliccaci con il tasto destro e scegli _Edit as HTML_ dal menu contestuale. Puoi fare tutte le modifiche che vuoi, ma non puoi salvare le modifiche.

### Esplorare l'editor CSS

Per impostazione predefinita, l'editor CSS mostra le regole CSS applicate all'elemento attualmente selezionato:

![Estratto del pannello CSS e del pannello layout che si possono vedere accanto all'editor HTML negli strumenti di sviluppo del browser. Per impostazione predefinita, l'editor CSS mostra le regole CSS applicate all'elemento attualmente selezionato nell'editor HTML. Il pannello layout mostra le proprietà del modello di box dell'elemento selezionato.](css_inspector.png)

Queste caratteristiche sono particolarmente utili:

- Le regole applicate all'elemento corrente sono mostrate in ordine da quella più specifica a quella meno specifica.
- Fai clic sulle caselle di controllo accanto a ciascuna dichiarazione per vedere cosa succederebbe se rimuovessi la dichiarazione.
- Fai clic sulla piccola freccia accanto a ciascuna proprietà abbreviata per mostrare le sue equivalenti dettagliate.
- Fai clic sul nome di una proprietà o sul valore per aprire una casella di testo, dove puoi immettere un nuovo valore per ottenere un'anteprima in tempo reale di una modifica allo stile.
- Accanto a ciascuna regola compare il nome del file e il numero di riga in cui la regola è definita. Cliccando su quella regola, gli strumenti di sviluppo salteranno a mostrarla nella sua vista dedicata, dove generalmente può essere modificata e salvata.
- Puoi anche fare clic sulla parentesi graffa di chiusura di qualsiasi regola per aprire una casella di testo su una nuova riga, dove puoi scrivere una dichiarazione completamente nuova per la tua pagina.

Vedrai un numero di schede cliccabili nella parte superiore del Visualizzatore CSS:

- _Computed_: Questo mostra gli stili calcolati per l'elemento attualmente selezionato (i valori finali e normalizzati che il browser applica).
- _Layout_: Questo mostra i dettagli per le modalità di layout [grid](/it/docs/Web/CSS/CSS_grid_layout) e [flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout) se l'elemento che stai ispezionando le utilizza.
- _Fonts_: In Firefox e Safari, la scheda _Fonts_ mostra i caratteri applicati all'elemento corrente.

La vista _box model_ rappresenta visivamente il modello di box dell'elemento corrente, così puoi vedere a colpo d'occhio quali padding, border e margin sono applicati ad esso e quanto grande è il suo contenuto. In Firefox, questo si trova nella scheda _Layout_, e in altri browser è nella scheda _Computed_.

In alcuni browser, i dettagli JavaScript dell'elemento selezionato possono essere visualizzati anche in questo pannello. In Safari, questi sono unificati nella scheda _Node_, ma si trovano in schede separate in Chrome, Opera ed Edge.

- _Properties_: Le {{Glossary("Property/JavaScript", "proprietà")}} dell'oggetto elemento.
- _Event Listeners_: Gli [eventi](/it/docs/Web/API/Event) associati all'elemento.

### Scopri di più

Scopri di più sull'Inspector nei diversi browser:

- [Inspector di pagina di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/index.html)
- [Inspector DOM di Chrome](https://developer.chrome.com/docs/devtools/dom/) (L'inspector di Opera ed Edge è lo stesso)
- [Scheda Elements di Safari](https://webkit.org/web-inspector/elements-tab/)

## Il debugger JavaScript

Il debugger JavaScript ti permette di osservare il valore delle variabili e impostare breakpoint, luoghi nel tuo codice in cui vuoi interrompere l'esecuzione per identificare i problemi che impediscono al codice di eseguire correttamente.

![Un sito web di test che è servito localmente alla porta 8080. La sottofinestra degli strumenti di sviluppo è aperta. La scheda del debugger JavaScript è selezionata. Ti permette di osservare il valore delle variabili e impostare breakpoint. Un file con nome 'example.js' è selezionato dal pannello delle fonti. Un breakpoint è impostato al numero di linea 18 del file.](firefox_debugger.png)

Per accedere al debugger:

**Firefox**: Apri gli strumenti di sviluppo e seleziona la scheda **Debugger**.
**Altri browser**: Apri gli strumenti di sviluppo e seleziona la scheda **Sources**.

### Esplorare il debugger

Il debugger JavaScript di ciascun browser è suddiviso in tre pannelli. Il layout di questi varia leggermente a seconda del browser che stai utilizzando; questa guida usa Firefox come riferimento.

#### Elenco dei file

Il primo pannello a sinistra contiene l'elenco di file associati alla pagina che stai debugando. Seleziona il file con cui vuoi lavorare da questo elenco. Clicca su un file per selezionarlo e visualizzarne il contenuto nel pannello centrale del Debugger.

![Estratto del pannello delle fonti della scheda del debugger negli strumenti di sviluppo del browser. I file relativi alla pagina attuale che stai debugando sono visibili sotto la cartella il cui nome è lo stesso dell'URL del sito aperto nella scheda attuale del browser.](file_list.png)

#### Codice sorgente

Imposta breakpoint dove vuoi interrompere l'esecuzione. Nell'immagine seguente, l'evidenziazione al numero 18 mostra che la linea ha un breakpoint impostato.

![Estratto del pannello del debugger strumenti di sviluppo con il breakpoint alla linea 18 evidenziato.](source_code.png)

#### Espressioni di controllo e breakpoint

Il pannello a destra mostra un elenco delle espressioni di controllo che hai aggiunto e dei breakpoint che hai impostato.

Nell'immagine, la prima sezione, **Watch expressions**, mostra che la variabile listItems è stata aggiunta. Puoi espandere l'elenco per vedere i valori nell'array.

La sezione successiva, **Breakpoints**, elenca i breakpoint impostati sulla pagina. In example.js, è stato impostato un breakpoint sull'istruzione `listItems.push(inputNewItem.value);`

Le ultime due sezioni appaiono solo quando il codice è in esecuzione.

La sezione **Call stack** mostra quale codice è stato eseguito per arrivare alla riga corrente. Puoi vedere che il codice è nella funzione che gestisce un clic del mouse e che il codice è attualmente interrotto al breakpoint.

L'ultima sezione, **Scopes**, mostra quali valori sono visibili da vari punti all'interno del tuo codice. Ad esempio, nell'immagine sottostante, puoi vedere gli oggetti disponibili al codice nella funzione addItemClick.

![Estratto del pannello delle fonti della scheda del debugger degli strumenti di sviluppo del browser. Nel call stack mostra la funzione che viene chiamata alla linea 18, evidenziando che un breakpoint è impostato a questa linea e mostrando l'ambito.](watch_items.png)

### Scopri di più

Scopri di più sul debugger JavaScript nei diversi browser:

- [Debugger JavaScript di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html))
- [Debugger di Chrome](https://developer.chrome.com/docs/devtools/javascript/) (Il debugger di Opera ed Edge è lo stesso)
- [Scheda Sources di Safari](https://webkit.org/web-inspector/sources-tab/)

## La console JavaScript

La console JavaScript è uno strumento incredibilmente utile per il debugging di JavaScript che non funziona come previsto. Ti consente di eseguire delle righe di JavaScript contro la pagina attualmente caricata nel browser e riporta gli errori incontrati mentre il browser cerca di eseguire il tuo codice.

Per accedere alla console in qualsiasi browser, apri gli strumenti di sviluppo e seleziona la scheda **Console**. Questo ti darà una finestra simile alla seguente:

![La scheda Console degli strumenti di sviluppo del browser. Due funzioni JavaScript sono state eseguite nella console. L'utente ha inserito le funzioni e la console ha visualizzato i valori di ritorno.](console_only.png)

Per vedere cosa succede, prova a inserire i seguenti frammenti di codice nella console uno per uno (e poi premi Invio):

```js
alert("hello!");
```

```js
document.querySelector("html").style.backgroundColor = "purple";
```

```js
const loginImage = document.createElement("img");
loginImage.setAttribute(
  "src",
  "https://raw.githubusercontent.com/mdn/learning-area/master/html/forms/image-type-example/login.png",
);
document.querySelector("h1").appendChild(loginImage);
```

Ora prova a inserire le seguenti versioni errate del codice e vedi cosa ottieni.

```js-nolint example-bad
alert("hello!);
```

```js example-bad
document.cheeseSelector("html").style.backgroundColor = "purple";
```

```js example-bad
const loginImage = document.createElement("img");
banana.setAttribute(
  "src",
  "https://raw.githubusercontent.com/mdn/learning-area/master/html/forms/image-type-example/login.png",
);
document.querySelector("h1").appendChild(loginImage);
```

Inizierai a vedere il tipo di errori che il browser restituisce. Spesso questi errori sono piuttosto criptici, ma dovrebbe essere abbastanza semplice capire questi problemi!

### Scopri di più

Scopri di più sulla console JavaScript nei diversi browser:

- [Console Web di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/web_console/index.html)
- [Console JavaScript di Chrome](https://developer.chrome.com/docs/devtools/console/) (La console di Opera ed Edge è la stessa)
- [API della Console di Safari](https://webkit.org/web-inspector/console-object-api/) e [API della riga di comando della Console](https://webkit.org/web-inspector/console-command-line-api/)

## Vedi anche

- [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML)
- [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS)
