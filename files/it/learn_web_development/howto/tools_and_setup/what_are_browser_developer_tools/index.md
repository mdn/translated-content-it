---
title: Cosa sono gli strumenti per sviluppatori del browser?
slug: Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

Ogni browser Web moderno include una potente suite di strumenti per sviluppatori. Questi strumenti svolgono varie funzioni, dall'ispezione di HTML, CSS e JavaScript attualmente caricati alla visualizzazione delle risorse richieste dalla pagina e del tempo impiegato per caricarle. Questo articolo spiega come usare le funzioni di base dei devtools del browser.

> [!NOTE]
> Prima di eseguire gli esempi seguenti, aprire il [sito di esempio per principianti](https://mdn.github.io/beginner-html-site-scripted/) creato durante la serie di articoli [Introduzione al Web](/it/docs/Learn_web_development/Getting_started/Your_first_website). Tenerlo aperto mentre si seguono i passaggi riportati di seguito.

## Come aprire i devtools nel browser

I devtools si trovano all'interno del browser in una sottofinestra che, a seconda del browser usato, assomiglia approssimativamente a questa:

![Schermata di un browser con gli strumenti per sviluppatori aperti. La pagina Web è visualizzata nella metà superiore del browser, mentre gli strumenti per sviluppatori occupano la metà inferiore. Negli strumenti per sviluppatori sono aperti tre pannelli: HTML, con l'elemento body selezionato; un pannello CSS che mostra i blocchi di stili destinati al body evidenziato; e un pannello degli stili calcolati che mostra tutti gli stili dell'autore; la casella di controllo degli stili del browser non è selezionata.](devtools_63_inspector.png)

Come aprirli? Esistono tre modi:

- **_Tastiera:_**
  - **Windows:** <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>I</kbd> oppure <kbd>F12</kbd>
  - **macOS:** <kbd>⌘</kbd> + <kbd>⌥</kbd> + <kbd>I</kbd>

- **_Barra dei menu:_**
  - **Firefox:** _Menu (☰) ➤ Altri strumenti ➤ Strumenti per sviluppatori Web_
  - **Chrome:** _Altri strumenti ➤ Strumenti per sviluppatori_
  - **Opera**: _Sviluppatore ➤ Strumenti per sviluppatori_
  - **Safari:** _Sviluppo ➤ Mostra Web Inspector._

    > [!NOTE]
    > Gli strumenti per sviluppatori di Safari non sono abilitati per impostazione predefinita.
    > Per abilitarli, andare in _Safari ➤ Preferenze ➤ Avanzate_ e selezionare la casella _Mostra menu Sviluppo nella barra dei menu_ oppure _Abilita funzionalità per sviluppatori Web_.

- **_Menu contestuale:_** Tenere premuto/fare clic con il pulsante destro del mouse su un elemento di una pagina Web (Ctrl-clic su Mac), quindi scegliere _Ispeziona elemento_ dal menu contestuale visualizzato. (_Un ulteriore vantaggio:_ questo metodo evidenzia immediatamente il codice dell'elemento su cui è stato fatto clic con il pulsante destro.)

![Il logo di Firefox come elemento DOM in un sito Web di esempio con un menu contestuale visualizzato. Un menu contestuale appare quando viene fatto clic con il pulsante destro su qualsiasi elemento della pagina Web. L'ultima voce di menu è 'Inspect element'.](inspector_context.png)

## L'Inspector: esploratore DOM ed editor CSS

Gli strumenti per sviluppatori si aprono solitamente per impostazione predefinita nell'Inspector, simile alla schermata seguente. Questo strumento mostra l'aspetto dell'HTML della pagina in fase di esecuzione e quale CSS viene applicato a ogni elemento della pagina. Consente inoltre di modificare istantaneamente HTML e CSS e di vedere i risultati delle modifiche aggiornati in tempo reale nella viewport del browser.

![Un sito Web di test è aperto in una scheda del browser. La sottofinestra degli strumenti per sviluppatori del browser è aperta. Gli strumenti per sviluppatori dispongono di diverse schede. Inspector è una di queste schede. La scheda Inspector visualizza il codice HTML del sito Web. Un tag immagine è selezionato dal codice HTML. Di conseguenza, nel sito Web viene evidenziata l'immagine corrispondente al tag selezionato.](inspector_highlighted.png)

Se l'Inspector _non_ è visibile:

- **Firefox:** selezionare la scheda **Inspector**.
- **Altri browser:** selezionare la scheda **Elements**.

### Esplorare l'Inspector DOM

Per iniziare, fare clic con il pulsante destro del mouse (Ctrl-clic) su un elemento HTML nell'Inspector DOM e osservare il menu contestuale. Le opzioni di menu disponibili variano tra i browser, ma quelle importanti sono per lo più le stesse:

![La sottofinestra degli strumenti per sviluppatori del browser è aperta. La scheda Inspector è selezionata. Un elemento link del codice HTML disponibile nella scheda Inspector viene selezionato con il pulsante destro del mouse. Viene visualizzato un menu contestuale. Le opzioni di menu disponibili variano tra i browser, ma quelle importanti sono per lo più le stesse.](dom_inspector.png)

- **Delete Node** (talvolta _Delete Element_). Elimina l'elemento corrente.
- **Edit as HTML** (talvolta _Add attribute_/_Edit text_). Consente di modificare l'HTML e vedere i risultati immediatamente. È molto utile per il debug e i test.
- **:hover/:active/:focus**. Forza l'attivazione degli stati dell'elemento, in modo da poter vedere quale sarebbe il loro stile.
- **Copy/Copy as HTML**. Copia l'HTML attualmente selezionato.
- Alcuni browser dispongono anche delle opzioni _Copy CSS Path_ e _Copy XPath_, che consentono di copiare il selettore CSS o l'espressione XPath che selezionerebbe l'elemento HTML corrente.

Provare ora a modificare parte del DOM. Fare doppio clic su un elemento oppure fare clic con il pulsante destro e scegliere _Edit as HTML_ dal menu contestuale. È possibile apportare qualsiasi modifica, ma non è possibile salvarla.

### Esplorare l'editor CSS

Per impostazione predefinita, l'editor CSS visualizza le regole CSS applicate all'elemento attualmente selezionato:

![Frammento del pannello CSS e del pannello di layout visibili accanto all'editor HTML negli strumenti per sviluppatori del browser. Per impostazione predefinita, l'editor CSS visualizza le regole CSS applicate all'elemento attualmente selezionato nell'editor HTML. Il pannello di layout mostra le proprietà del box model dell'elemento selezionato.](css_inspector.png)

Queste funzionalità sono particolarmente utili:

- Le regole applicate all'elemento corrente vengono mostrate in ordine dalla più alla meno specifica.
- Fare clic sulle caselle di controllo accanto a ogni dichiarazione per vedere cosa accadrebbe rimuovendola.
- Fare clic sulla piccola freccia accanto a ogni proprietà shorthand per mostrare gli equivalenti longhand della proprietà.
- Fare clic sul nome o sul valore di una proprietà per visualizzare una casella di testo, nella quale è possibile inserire un nuovo valore per ottenere un'anteprima in tempo reale della modifica di stile.
- Accanto a ogni regola sono riportati il nome del file e il numero di riga in cui la regola è definita. Facendo clic su quella regola, i devtools la mostrano nella relativa vista, dove generalmente può essere modificata e salvata.
- È inoltre possibile fare clic sulla parentesi graffa di chiusura di qualsiasi regola per visualizzare una casella di testo su una nuova riga, nella quale scrivere una dichiarazione completamente nuova per la pagina.

Nella parte superiore del visualizzatore CSS sono presenti diverse schede selezionabili:

- _Computed_: mostra gli stili calcolati per l'elemento attualmente selezionato, ovvero i valori finali e normalizzati applicati dal browser.
- _Layout_: mostra i dettagli delle modalità di layout CSS [grid](/it/docs/Web/CSS/Guides/Grid_layout) e [flexbox](/it/docs/Web/CSS/Guides/Flexible_box_layout) se l'elemento ispezionato le utilizza.
- _Fonts_: in Firefox e Safari, la scheda _Fonts_ mostra i font applicati all'elemento corrente.

La vista del _box model_ rappresenta visivamente il box model dell'elemento corrente, permettendo di vedere immediatamente quali padding, border e margin sono applicati e quanto è grande il relativo contenuto. In Firefox si trova nella scheda _Layout_; negli altri browser si trova nella scheda _Computed_.

In alcuni browser, in questo pannello è possibile visualizzare anche i dettagli JavaScript dell'elemento selezionato. In Safari, sono riuniti nella scheda _Node_, mentre in Chrome, Opera ed Edge sono disponibili in schede separate.

- _Properties_: le {{Glossary("Property/JavaScript", "proprietà")}} dell'oggetto elemento.
- _Event Listeners_: gli [eventi](/it/docs/Web/API/Event) associati all'elemento.

### Per saperne di più

Per ulteriori informazioni sull'Inspector nei vari browser:

- [Firefox Page inspector](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/index.html)
- [Chrome DOM inspector](https://developer.chrome.com/docs/devtools/dom/) (l'Inspector di Opera ed Edge è lo stesso)
- [Scheda Elements di Safari](https://webkit.org/web-inspector/elements-tab/)

## Il debugger JavaScript

Il debugger JavaScript consente di osservare il valore delle variabili e impostare breakpoint, ovvero punti nel codice in cui sospendere l'esecuzione e identificare i problemi che impediscono al codice di essere eseguito correttamente.

![Un sito Web di test servito localmente sulla porta 8080. La sottofinestra degli strumenti per sviluppatori è aperta. La scheda del debugger JavaScript è selezionata. Consente di osservare il valore delle variabili e impostare breakpoint. Dal riquadro delle sorgenti è selezionato un file denominato 'example.js'. Un breakpoint è impostato alla riga 18 del file.](firefox_debugger.png)

Per accedere al debugger:

- **Firefox**: aprire gli strumenti per sviluppatori e selezionare la scheda **Debugger**.
- **Altri browser**: aprire gli strumenti per sviluppatori e selezionare la scheda **Sources**.

### Esplorare il debugger

Il debugger JavaScript di ciascun browser è suddiviso in tre riquadri. La disposizione varia tra i browser; questa guida usa Firefox come riferimento.

#### Elenco dei file

Il primo riquadro a sinistra contiene l'elenco dei file associati alla pagina di cui si sta effettuando il debug. Selezionare dall'elenco il file su cui lavorare. Fare clic su un file per selezionarlo e visualizzarne il contenuto nel riquadro centrale del Debugger.

![Frammento del riquadro delle sorgenti della scheda debugger negli strumenti per sviluppatori del browser. I file relativi alla pagina corrente di cui si sta effettuando il debug sono visibili nella cartella il cui nome corrisponde all'URL del sito aperto nella scheda corrente del browser.](file_list.png)

#### Codice sorgente

Impostare breakpoint nei punti in cui si desidera sospendere l'esecuzione. Nell'immagine seguente, l'evidenziazione del numero 18 indica che per quella riga è impostato un breakpoint.

![Frammento del pannello debugger degli strumenti per sviluppatori con il breakpoint alla riga 18 evidenziato.](source_code.png)

#### Espressioni di controllo e breakpoint

Il riquadro a destra mostra un elenco delle espressioni di controllo aggiunte e dei breakpoint impostati.

Nell'immagine, la prima sezione, **Watch expressions**, mostra che è stata aggiunta la variabile `listItems`. È possibile espandere l'elenco per visualizzare i valori nell'array.

La sezione successiva, **Breakpoints**, elenca i breakpoint impostati nella pagina. In example.js, è stato impostato un breakpoint sull'istruzione `listItems.push(inputNewItem.value);`

Le ultime due sezioni vengono visualizzate solo quando il codice è in esecuzione.

La sezione **Call stack** mostra quale codice è stato eseguito per raggiungere la riga corrente. È possibile vedere che il codice si trova nella funzione che gestisce un clic del mouse e che è attualmente in pausa sul breakpoint.

La sezione finale, **Scopes**, mostra quali valori sono visibili da vari punti del codice. Ad esempio, nell'immagine seguente è possibile vedere gli oggetti disponibili al codice nella funzione addItemClick.

![Frammento del riquadro delle sorgenti della scheda debugger degli strumenti per sviluppatori del browser. Nel call stack viene mostrata la funzione chiamata alla riga 18, evidenziando che su questa riga è impostato un breakpoint e mostrando lo scope.](watch_items.png)

### Per saperne di più

Per ulteriori informazioni sul debugger JavaScript nei vari browser:

- [Firefox JavaScript Debugger](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html))
- [Chrome Debugger](https://developer.chrome.com/docs/devtools/javascript/) (il debugger di Opera ed Edge è lo stesso)
- [Scheda Sources di Safari](https://webkit.org/web-inspector/sources-tab/)

## La console JavaScript

La console JavaScript è uno strumento estremamente utile per eseguire il debug di JavaScript che non funziona come previsto. Consente di eseguire righe di JavaScript sulla pagina attualmente caricata nel browser e segnala gli errori riscontrati mentre il browser tenta di eseguire il codice.

Per accedere alla console in qualsiasi browser, aprire gli strumenti per sviluppatori e selezionare la scheda **Console**. Verrà visualizzata una finestra simile alla seguente:

![La scheda Console degli strumenti per sviluppatori del browser. Nella console sono state eseguite due funzioni JavaScript. L'utente ha inserito le funzioni e la console ha visualizzato i valori restituiti.](console_only.png)

Per vedere cosa accade, provare a inserire nella console i seguenti frammenti di codice uno alla volta, quindi premere Invio:

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
  "https://mdn.github.io/shared-assets/images/examples/login-button.png",
);
document.querySelector("h1").appendChild(loginImage);
```

Ora provare a inserire le seguenti versioni errate del codice e osservare il risultato.

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
  "https://mdn.github.io/shared-assets/images/examples/login-button.png",
);
document.querySelector("h1").appendChild(loginImage);
```

Si inizieranno a vedere i tipi di errori restituiti dal browser. Spesso questi errori sono piuttosto criptici, ma dovrebbe essere abbastanza semplice individuare questi problemi.

### Per saperne di più

Per ulteriori informazioni sulla console JavaScript nei vari browser:

- [Firefox Web Console](https://firefox-source-docs.mozilla.org/devtools-user/web_console/index.html)
- [Chrome JavaScript Console](https://developer.chrome.com/docs/devtools/console/) (la console di Opera ed Edge è la stessa)
- [Safari Console Object API](https://webkit.org/web-inspector/console-object-api/) e [Console Command Line API](https://webkit.org/web-inspector/console-command-line-api/)

## Vedi anche

- [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML)
- [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS)
