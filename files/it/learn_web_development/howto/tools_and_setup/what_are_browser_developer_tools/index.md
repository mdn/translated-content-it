---
title: Che cosa sono gli strumenti per sviluppatori del browser?
slug: Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Ogni moderno browser web include una potente suite di strumenti per sviluppatori. Questi strumenti eseguono una serie di operazioni, dall'ispezione di HTML, CSS e JavaScript attualmente caricati alla visualizzazione delle risorse richieste dalla pagina e del tempo impiegato per caricarle. Questo articolo spiega come utilizzare le funzioni di base degli strumenti per sviluppatori del suo browser.

> [!NOTE]
> Prima di esaminare gli esempi sottostanti, apra il [sito di esempio per principianti](https://mdn.github.io/beginner-html-site-scripted/) che abbiamo costruito durante la serie di articoli [Primi passi con il Web](/it/docs/Learn_web_development/Getting_started/Your_first_website). Dovrebbe tenerlo aperto mentre segue i passaggi qui sotto.

## Come aprire gli strumenti per sviluppatori nel suo browser

Gli strumenti per sviluppatori si trovano all'interno del suo browser in una sottofinestra che appare all'incirca così, a seconda del browser che sta utilizzando:

![Screenshot di un browser con gli strumenti per sviluppatori aperti. La pagina web è visualizzata nella metà superiore del browser, mentre gli strumenti per sviluppatori occupano la metà inferiore. Ci sono tre pannelli aperti negli strumenti per sviluppatori: HTML, con l'elemento body selezionato, un pannello CSS che mostra i blocchi di stile che mirano al body evidenziato, e un pannello di stili calcolati che mostra tutti gli stili dell'autore; la casella di controllo degli stili del browser non è selezionata.](devtools_63_inspector.png)

Come si attivano? In tre modi:

- **_Tastiera:_**

  - **Windows:** <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>I</kbd> o <kbd>F12</kbd>
  - **macOS:** <kbd>⌘</kbd> + <kbd>⌥</kbd> + <kbd>I</kbd>

- **_Barra del menu:_**

  - **Firefox:** _Menu (☰) ➤ Altri strumenti ➤ Strumenti per sviluppatori Web_
  - **Chrome:** _Altri strumenti ➤ Strumenti per sviluppatori_
  - **Opera:** _Sviluppatore ➤ Strumenti per sviluppatori_
  - **Safari:** _Sviluppo ➤ Mostra Web Inspector._

    > [!NOTE]
    > Gli strumenti per sviluppatori di Safari non sono abilitati di default.
    > Per abilitarli, vada su _Safari ➤ Preferenze ➤ Avanzate_, e selezioni la casella _Mostra menu Sviluppo nella barra dei menu_ o _Abilita funzioni per sviluppatori web_.

- **_Menu contestuale:_** Premere e tenere premuto/fare clic con il tasto destro su un elemento in una pagina web (Ctrl-clic su Mac) e scegliere _Ispeziona elemento_ dal menu contestuale che appare. (_Un vantaggio aggiunto:_ questo metodo evidenzia immediatamente il codice dell'elemento su cui ha fatto clic con il tasto destro.)

![Il logo di Firefox come un elemento DOM in un sito web di esempio con un menu contestuale mostrato. Un menu contestuale appare quando un qualsiasi elemento sulla pagina web viene cliccato con il tasto destro. L'ultimo elemento del menu è 'Inspect element'.](inspector_context.png)

## L'Inspector: esploratore DOM e editor CSS

Gli strumenti per sviluppatori di solito si aprono di default sull'inspector, che appare simile al seguente screenshot. Questo strumento mostra come appare l'HTML sulla sua pagina a runtime, così come quali CSS vengono applicati a ciascun elemento sulla pagina. Consente anche di modificare immediatamente l'HTML e il CSS e vedere i risultati dei suoi cambiamenti riflessi in tempo reale nella finestra del browser.

![Un sito di test è aperto in una scheda del browser. La sottofinestra degli strumenti per sviluppatori del browser è aperta. Gli strumenti per sviluppatori hanno diverse schede. Inspector è una di queste schede. La scheda Inspector visualizza il codice HTML del sito web. Un tag immagine è selezionato dal codice HTML. Questo risulta nel mettere in evidenza l'immagine corrispondente al tag selezionato nel sito web.](inspector_highlighted.png)

Se _non_ vede l'inspector,

- **Firefox:** Selezioni la scheda **Inspector**.
- **Altri browser:** Selezioni la scheda **Elements**.

### Esplorare l'inspector del DOM

Per iniziare, faccia clic con il tasto destro (Ctrl-clic) su un elemento HTML nell'inspector del DOM e guardi il menu contestuale. Le opzioni del menu disponibili variano tra i browser, ma quelle importanti sono per lo più le stesse:

![La sottofinestra degli strumenti per sviluppatori del browser è aperta. La scheda inspector è selezionata. Un elemento link è cliccato con il tasto destro dal codice HTML disponibile nella scheda inspector. Un menu contestuale appare. Le opzioni del menu disponibili variano tra i browser, ma quelle importanti sono per lo più le stesse.](dom_inspector.png)

- **Delete Node** (a volte _Delete Element_). Elimina l'elemento corrente.
- **Edit as HTML** (a volte _Add attribute_/_Edit text_). Le permette di cambiare l'HTML e vedere i risultati sul momento. Molto utile per il debug e i test.
- **:hover/:active/:focus**. Forza l'attivazione degli stati degli elementi, così può vedere come sarebbe il loro stile.
- **Copy/Copy as HTML**. Copia l'HTML attualmente selezionato.
- Alcuni browser offrono anche _Copy CSS Path_ e _Copy XPath_, per permetterle di copiare il selettore CSS o l'espressione XPath che selezionerebbe l'elemento HTML corrente.

Provi a modificare un po' il suo DOM ora. Faccia doppio clic su un elemento, o faccia clic con il tasto destro e scelga _Edit as HTML_ dal menu contestuale. Può apportare qualsiasi modifica desideri, ma non può salvare le modifiche.

### Esplorare l'editor CSS

Di default, l'editor CSS visualizza le regole CSS applicate all'elemento attualmente selezionato:

![Ritaglio del pannello CSS e del pannello layout che possono essere visti adiacenti all'editor HTML negli strumenti per sviluppatori del browser. Di default, l'editor CSS visualizza le regole CSS applicate all'elemento attualmente selezionato nell'editor HTML. Il pannello layout mostra le proprietà del modello a scatola dell'elemento selezionato.](css_inspector.png)

Queste caratteristiche sono particolarmente utili:

- Le regole applicate all'elemento corrente sono mostrate in ordine dal più al meno specifico.
- Clicchi sulle caselle di controllo accanto a ciascuna dichiarazione per vedere cosa succederebbe se rimuovesse la dichiarazione.
- Clicchi sulla piccola freccia accanto a ciascuna proprietà abbreviata per mostrare gli equivalenti delle proprietà lunghe.
- Clicchi su un nome o un valore di proprietà per aprire una casella di testo, dove può inserire un nuovo valore per ottenere un'anteprima dal vivo di una modifica dello stile.
- Accanto a ciascuna regola è presente il nome del file e il numero di riga in cui la regola è definita. Cliccando su quella regola, gli strumenti per sviluppatori passano a visualizzarla in una propria vista, dove generalmente può essere modificata e salvata.
- Può anche cliccare la parentesi graffa di chiusura di qualsiasi regola per aprire una casella di testo su una nuova riga, dove può scrivere una dichiarazione completamente nuova per la sua pagina.

Noterà un numero di schede cliccabili nella parte superiore del Visualizzatore CSS:

- _Computed_: Questa mostra gli stili calcolati per l'elemento attualmente selezionato (i valori finali e normalizzati che il browser applica).
- _Layout_: Questa mostra i dettagli per le modalità di layout CSS [grid](/it/docs/Web/CSS/CSS_grid_layout) e [flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout) se l'elemento che sta ispezionando le utilizza.
- _Fonts_: In Firefox e Safari, la scheda _Fonts_ mostra i font applicati all'elemento corrente.

La vista _box model_ rappresenta visivamente il modello a scatola dell'elemento corrente, così può vedere a colpo d'occhio quale padding, bordo e margine è applicato, e quanto è grande il suo contenuto. In Firefox, si trova nella scheda _Layout_, e in altri browser è nella scheda _Computed_.

In alcuni browser, i dettagli JavaScript dell'elemento selezionato possono essere visualizzati anche in questo pannello. In Safari, questi sono unificati nella scheda _Node_, ma sono in schede separate in Chrome, Opera e Edge.

- _Properties_: Le {{Glossary("Property/JavaScript", "proprietà")}} dell'oggetto elemento.
- _Event Listeners_: Gli [eventi](/it/docs/Web/API/Event) associati all'elemento.

### Scoprire di più

Scopra di più sull'Inspector in diversi browser:

- [Inspector di pagina di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/index.html)
- [Inspector DOM di Chrome](https://developer.chrome.com/docs/devtools/dom/) (L'inspector di Opera e Edge è lo stesso)
- [Scheda Elemente di Safari](https://webkit.org/web-inspector/elements-tab/)

## Il debugger JavaScript

Il debugger JavaScript le consente di osservare il valore delle variabili e impostare punti di interruzione, cioè i luoghi nel suo codice dove desidera sospendere l'esecuzione e identificare i problemi che impediscono al suo codice di funzionare correttamente.

![Un sito di test che è servito localmente su porta 8080. La sottofinestra degli strumenti per sviluppatori è aperta. La scheda del debugger JavaScript è selezionata. Consente di osservare il valore delle variabili e impostare punti di interruzione. Un file con nome 'example.js' è selezionato dal riquadro delle risorse. Un punto di interruzione è impostato al numero di riga 18 del file.](firefox_debugger.png)

Per accedere al debugger:

**Firefox**: Apra gli strumenti per sviluppatori e selezioni la scheda **Debugger**.
**Altri browser**: Apra gli strumenti per sviluppatori e selezioni la scheda **Sources**.

### Esplorare il debugger

Il debugger JavaScript di ciascun browser è diviso in tre pannelli. La disposizione di questi è leggermente diversa a seconda del browser che sta utilizzando; questa guida utilizza Firefox come riferimento.

#### Elenco dei file

Il primo pannello a sinistra contiene l'elenco dei file associati alla pagina che sta eseguendo il debug. Selezioni il file con cui vuole lavorare da questo elenco. Faccia clic su un file per selezionarlo e visualizzare il suo contenuto nel pannello centrale del Debugger.

![Ritaglio del riquadro delle risorse della scheda debugger negli strumenti per sviluppatori del browser. I file relativi alla pagina corrente che sta eseguendo il debug sono visibili sotto la cartella il cui nome è lo stesso dell'URL del sito aperto nella scheda corrente del browser.](file_list.png)

#### Codice sorgente

Imposti punti di interruzione dove vuole sospendere l'esecuzione. Nell'immagine seguente, l'evidenziazione sul numero 18 mostra che la linea ha un punto di interruzione impostato.

![Ritaglio del pannello del debugger degli strumenti per sviluppatori con il punto di interruzione alla riga 18 evidenziato.](source_code.png)

#### Espressioni monitorate e punti di interruzione

Il pannello a destra mostra un elenco delle espressioni monitorate che ha aggiunto e dei punti di interruzione che ha impostato.

Nell'immagine, la prima sezione, **Watch expressions**, mostra che la variabile listItems è stata aggiunta. Può espandere l'elenco per visualizzare i valori nell'array.

La sezione successiva, **Breakpoints**, elenca i punti di interruzione impostati sulla pagina. In example.js, è stato impostato un punto di interruzione sull'istruzione `listItems.push(inputNewItem.value);`

Le ultime due sezioni appaiono solo quando il codice è in esecuzione.

La sezione **Call stack** le mostra quale codice è stato eseguito per arrivare alla linea corrente. Può vedere che il codice si trova nella funzione che gestisce un click del mouse e che il codice è attualmente in pausa sul punto di interruzione.

L'ultima sezione, **Scopes**, mostra quali valori sono visibili da vari punti nel suo codice. Ad esempio, nell'immagine sottostante, può vedere gli oggetti disponibili per il codice nella funzione addItemClick.

![Ritaglio del riquadro delle risorse della scheda debugger degli strumenti per sviluppatori del browser. Nella call stack mostra la funzione che è chiamata alla Linea 18, evidenziando che un punto di interruzione è impostato a questa linea e mostrando lo scope.](watch_items.png)

### Scoprire di più

Scopra di più sul debugger JavaScript in diversi browser:

- [Debugger JavaScript di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html))
- [Debugger di Chrome](https://developer.chrome.com/docs/devtools/javascript/) (Il debugger di Opera e Edge è lo stesso)
- [Scheda Sources di Safari](https://webkit.org/web-inspector/sources-tab/)

## La console JavaScript

La console JavaScript è uno strumento incredibilmente utile per fare debugging sul JavaScript che non funziona come previsto. Le consente di eseguire righe di JavaScript sulla pagina attualmente caricata nel browser e segnala gli errori incontrati mentre il browser tenta di eseguire il suo codice.

Per accedere alla console in qualsiasi browser, apra gli strumenti per sviluppatori e selezioni la scheda **Console**. Questo le darà una finestra simile alla seguente:

![La scheda Console degli strumenti per sviluppatori del browser. Due funzioni JavaScript sono state eseguite nella console. L'utente ha inserito le funzioni, e la console ha visualizzato i valori di ritorno.](console_only.png)

Per vedere cosa succede, provi a inserire i seguenti frammenti di codice nella console uno per uno (e quindi premere Invio):

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

Ora provi a inserire le versioni errate del codice seguente e veda cosa ottiene.

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

Inizierà a vedere il tipo di errori che il browser restituisce. Spesso questi errori sono piuttosto criptici, ma dovrebbe essere abbastanza semplice capire questi problemi!

### Scoprire di più

Scopra di più sulla console JavaScript in diversi browser:

- [Console Web di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/web_console/index.html)
- [Console JavaScript di Chrome](https://developer.chrome.com/docs/devtools/console/) (La console di Opera e Edge è la stessa)
- [API per l'oggetto Console di Safari](https://webkit.org/web-inspector/console-object-api/) e [API della linea di comando della console](https://webkit.org/web-inspector/console-command-line-api/)

## Vedere anche

- [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML)
- [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS)
