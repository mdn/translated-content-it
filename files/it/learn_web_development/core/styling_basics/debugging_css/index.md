---
title: Debugging del CSS
slug: Learn_web_development/Core/Styling_basics/Debugging_CSS
l10n:
  sourceCommit: 418fefaa02f8e1ea53d53cb6fc510a4dc4100dc5
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Home_color_scheme_search", "Learn_web_development/Core/Text_styling", "Learn_web_development/Core/Styling_basics")}}

A volte, durante la scrittura di CSS, si può incontrare un problema per cui il CSS non sembra fare ciò che ci si aspetta. Forse si ritiene che un determinato selettore debba corrispondere a un elemento, ma non accade nulla, oppure una casella ha dimensioni diverse da quelle previste. Questo articolo fornisce indicazioni su come eseguire il debugging di un problema CSS e mostra come i DevTools inclusi in tutti i browser moderni possano aiutare a capire cosa sta succedendo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >, nozioni di base sullo stile CSS (trattate nelle lezioni precedenti di questo modulo!)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Usare il <a href="https://validator.w3.org/">validatore HTML</a> per verificare se nella pagina è presente markup non valido che causa problemi CSS.</li>
          <li>Usare il <a href="https://jigsaw.w3.org/css-validator/">validatore CSS</a> per verificare la presenza di codice CSS malformato.</li>
          <li>Usare gli strumenti di sviluppo del browser per ispezionare il CSS applicato agli elementi HTML in una pagina.</li>
          <li>Modificare il CSS applicato per capire quali cambiamenti sono necessari per ottenere il risultato desiderato. Ciò include l'attivazione e la disattivazione di dichiarazioni, la modifica dei valori e l'aggiunta di nuove dichiarazioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Come accedere ai DevTools del browser

L'articolo [Cosa sono gli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) spiega come accedere agli strumenti in vari browser e piattaforme. Sebbene si possa scegliere di sviluppare principalmente in un browser specifico e, di conseguenza, acquisire maggiore familiarità con gli strumenti inclusi in quel browser, è utile sapere come accedervi anche negli altri browser. Questo è utile quando si osservano differenze di rendering tra più browser.

In questa lezione verranno esaminate alcune funzionalità utili dei Firefox DevTools per lavorare con CSS. A questo scopo verrà usato [un file di esempio](https://mdn.github.io/css-examples/learn/inspecting/inspecting.html). Aprirlo in una nuova scheda per seguire gli esempi e aprire i DevTools come descritto nell'articolo collegato sopra.

## Il DOM rispetto alla visualizzazione del sorgente

Un aspetto che può confondere chi è alle prime armi con i DevTools è la differenza tra ciò che si vede quando si [visualizza il sorgente](https://firefox-source-docs.mozilla.org/devtools-user/view_source/index.html) di una pagina web, o si osserva il file HTML caricato sul server, e ciò che è visibile nel [pannello HTML](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#html-pane) dei DevTools. Sebbene appaia in modo approssimativamente simile a quanto visibile tramite View Source, esistono alcune differenze.

Nel DOM renderizzato, il browser potrebbe aver normalizzato l'HTML, ad esempio correggendo markup HTML scritto in modo errato. Se un elemento viene chiuso in modo scorretto, per esempio aprendo un `<h2>` ma chiudendolo con `</h3>`, il browser capirà l'intenzione e l'HTML nel DOM chiuderà correttamente l'elemento `<h2>` aperto con `</h2>`. Il DOM mostrerà anche tutte le modifiche eseguite da JavaScript.

View Source, al contrario, è il codice sorgente HTML così come è memorizzato sul server. L'[albero HTML](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_html/index.html#html-tree) nei DevTools mostra esattamente ciò che il browser sta renderizzando in un determinato momento, quindi permette di capire cosa sta realmente succedendo.

## Ispezionare il CSS applicato

Selezionare un elemento nella pagina facendo clic con il pulsante destro del mouse o Ctrl+clic su di esso e scegliendo _Inspect_, oppure selezionandolo dall'albero HTML a sinistra nella visualizzazione dei DevTools. Provare a selezionare l'elemento con classe `box1`; è il primo elemento della pagina con una casella bordata disegnata intorno.

![La pagina di esempio per questo tutorial con i DevTools aperti.](inspecting1.png)

Osservando la [vista Rules](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#rules-view) a destra dell'HTML, dovrebbero essere visibili le proprietà CSS e i valori applicati a quell'elemento. Saranno visibili le regole applicate direttamente alla classe `box1` e anche il CSS ereditato dalla casella dai suoi antenati, in questo caso da `<body>`. Ciò è utile se viene applicato del CSS non previsto. Forse è ereditato da un elemento genitore e occorre aggiungere una regola per sovrascriverlo nel contesto di questo elemento.

È inoltre utile poter espandere le proprietà shorthand. Nel nostro esempio viene usata la shorthand `margin`.

**Fare clic sulla piccola freccia per espandere la vista e visualizzare le diverse proprietà longhand e i relativi valori.**

**È possibile attivare e disattivare i valori nella vista Rules quando il pannello è attivo: passando il mouse sopra di esso verranno visualizzate delle caselle di controllo. Deselezionare la casella di controllo di una regola, ad esempio `border-radius`, e il CSS smetterà di essere applicato.**

Questo permette di effettuare un confronto A/B, decidendo se un elemento appare meglio con una regola applicata o meno, e aiuta anche nel debugging: ad esempio, se un layout non funziona correttamente e si sta cercando di capire quale proprietà causa il problema.

## Modificare i valori

Oltre ad attivare e disattivare le proprietà, è possibile modificarne i valori. Forse si desidera verificare se un altro colore sia migliore oppure regolare la dimensione di qualcosa. I DevTools possono far risparmiare molto tempo rispetto alla modifica di un foglio di stile e al ricaricamento della pagina.

**Con `box1` selezionato, fare clic sul campione (il piccolo cerchio colorato) che mostra il colore applicato al bordo. Si aprirà un selettore di colori, con cui sarà possibile provare colori diversi; questi aggiorneranno la pagina in tempo reale. In modo analogo, si potrebbe modificare la larghezza o lo stile del bordo.**

![Pannello Styles dei DevTools con un selettore di colori aperto.](inspecting2-color-picker.png)

## Aggiungere una nuova proprietà

È possibile aggiungere proprietà usando i DevTools. Forse ci si è resi conto di non volere che la casella erediti la dimensione del font dell'elemento `<body>` e si desidera impostare una dimensione specifica. È possibile provarlo nei DevTools prima di aggiungerlo al file CSS.

**È possibile fare clic sulla parentesi graffa di chiusura nella regola per iniziare a inserire una nuova dichiarazione; a quel punto, iniziando a digitare la nuova proprietà, i DevTools mostreranno un elenco di proprietà corrispondenti con completamento automatico. Dopo aver selezionato `font-size`, inserire il valore da provare. È anche possibile fare clic sul pulsante + per aggiungere un'ulteriore regola con lo stesso selettore e aggiungere lì le nuove regole.**

![Il pannello DevTools, con l'aggiunta di una nuova proprietà alle regole e il completamento automatico per font- aperto](inspecting3-font-size.png)

> [!NOTE]
> La vista Rules dispone anche di altre funzionalità utili; ad esempio, le dichiarazioni con valori non validi vengono barrate. Per ulteriori informazioni, vedere [Esaminare e modificare CSS](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html).

## Comprendere il box model

Nelle lezioni precedenti è stato trattato [il box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) e il fatto che esiste un box model alternativo che modifica il modo in cui vengono calcolate le dimensioni degli elementi in base alla dimensione assegnata, oltre a padding e bordi. I DevTools possono essere molto utili per comprendere come viene calcolata la dimensione di un elemento.

La [vista Layout](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#layout-view) mostra un diagramma del box model dell'elemento selezionato, insieme a una descrizione delle proprietà e dei valori che modificano il layout dell'elemento. Include una descrizione delle proprietà che potrebbero non essere state usate esplicitamente sull'elemento, ma che hanno comunque valori iniziali impostati.

In questo pannello, una delle proprietà dettagliate è `box-sizing`, che controlla quale box model usa l'elemento.

**Confrontare le due caselle con classi `box1` e `box2`. A entrambe viene applicata la stessa larghezza (400px), tuttavia `box1` è visivamente più larga. Nel pannello Layout è possibile vedere che usa `content-box`. Questo valore prende la dimensione assegnata all'elemento e vi aggiunge il padding e la larghezza del bordo.**

L'elemento con classe `box2` usa `border-box`, quindi in questo caso padding e bordo vengono sottratti dalla dimensione assegnata all'elemento. Ciò significa che lo spazio occupato nella pagina dalla casella corrisponde esattamente alla dimensione specificata, nel nostro caso `width: 400px`.

![La sezione Layout dei DevTools](inspecting4-box-model.png)

> [!NOTE]
> Per ulteriori informazioni, vedere [Esaminare e ispezionare il Box Model](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_the_box_model/index.html).

## Risolvere problemi di specificità

A volte durante lo sviluppo, soprattutto quando è necessario modificare il CSS di un sito esistente, può risultare difficile applicare del CSS. Qualunque cosa si faccia, l'elemento non sembra accettare il CSS. In genere ciò accade perché un selettore più specifico sta sovrascrivendo le modifiche, e qui i DevTools possono essere davvero d'aiuto.

Nel nostro file di esempio ci sono due parole racchiuse in un elemento `<em>`. Una viene visualizzata in arancione e l'altra in hotpink. Nel CSS è stato applicato:

```css
em {
  color: hotpink;
  font-weight: bold;
}
```

Tuttavia, più sopra nel foglio di stile è presente una regola con un selettore `.special`:

```css
.special {
  color: orange;
}
```

Come ricordato dalla lezione sulla [gestione dei conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts), nella quale è stata trattata la specificità, i selettori di classe sono più specifici dei selettori di elemento, quindi viene applicato questo valore. I DevTools possono aiutare a individuare problemi di questo tipo, soprattutto se le informazioni sono nascoste da qualche parte in un foglio di stile molto grande.

**Ispezionare l'elemento `<em>` con classe `.special`: i DevTools mostreranno che l'arancione è il colore applicato e che la proprietà `color` applicata a `<em>` è barrata. A questo punto è possibile vedere che il selettore di classe sta sovrascrivendo il selettore di elemento.**

![Selezione di un elemento em e osservazione dei DevTools per vedere cosa sta sovrascrivendo il colore.](inspecting5-specificity.png)

## Eseguire il debugging dei problemi nel CSS

I DevTools possono essere di grande aiuto nella risoluzione dei problemi CSS; quindi, quando il CSS non si comporta come previsto, come procedere? I passaggi seguenti dovrebbero essere utili.

### Fare un passo indietro rispetto al problema

Qualsiasi problema di programmazione può essere frustrante, in particolare i problemi CSS, perché spesso non viene visualizzato un messaggio di errore da cercare online per trovare una soluzione. Se la frustrazione aumenta, allontanarsi dal problema per un po': fare una passeggiata, prendere qualcosa da bere, parlare con un collega o lavorare temporaneamente a qualcos'altro. Talvolta la soluzione appare magicamente quando si smette di pensare al problema; anche in caso contrario, sarà molto più semplice lavorarci sentendosi riposati.

### HTML e CSS sono validi?

I browser si aspettano CSS e HTML scritti correttamente, ma sono anche molto tolleranti e cercheranno di visualizzare le pagine web anche in presenza di errori nel markup o nel foglio di stile. Se il codice contiene errori, il browser deve ipotizzare quale fosse l'intenzione e potrebbe prendere una decisione diversa da quella prevista. Inoltre, due browser diversi potrebbero gestire il problema in due modi diversi. Un buon primo passo consiste quindi nell'eseguire HTML e CSS attraverso un validatore, per rilevare e correggere eventuali errori.

- [Validatore CSS](https://jigsaw.w3.org/css-validator/)
- [Validatore HTML](https://validator.w3.org/)

### La proprietà e il valore sono supportati dal browser in cui si sta effettuando il test?

I browser ignorano il CSS che non comprendono. Se la proprietà o il valore usato non è supportato dal browser in cui si sta effettuando il test, non si interromperà nulla, ma quel CSS non verrà applicato. In genere i DevTools evidenziano in qualche modo le proprietà e i valori non supportati. Nello screenshot seguente, il browser non supporta il valore subgrid di {{cssxref("grid-template-columns")}}.

![Immagine dei DevTools del browser con grid-template-columns: subgrid barrato perché il valore subgrid non è supportato.](no-support.png)

È anche possibile consultare le tabelle di Compatibilità del browser in fondo a ogni pagina delle proprietà su MDN. Queste mostrano il supporto dei browser per quella proprietà, spesso suddiviso quando è supportato un determinato utilizzo della proprietà ma non altri. [Vedere la tabella di compatibilità della proprietà `grid-template-columns`](/it/docs/Web/CSS/Reference/Properties/grid-template-columns#browser_compatibility).

### Qualcos'altro sta sovrascrivendo il CSS?

Qui le informazioni apprese sulla specificità saranno molto utili. Se qualcosa di più specifico sta sovrascrivendo ciò che si sta cercando di fare, si può entrare in un gioco molto frustrante nel tentativo di capire cosa sia. Tuttavia, come descritto sopra, i DevTools mostrano quale CSS viene applicato e permettono di capire come rendere il nuovo selettore sufficientemente specifico da sovrascriverlo.

### Creare un caso di test ridotto del problema

Se il problema non viene risolto dai passaggi precedenti, sarà necessario svolgere ulteriori indagini. La cosa migliore da fare a questo punto è creare qualcosa noto come caso di test ridotto. La capacità di "ridurre un problema" è un'abilità davvero utile. Aiuta a trovare problemi nel proprio codice e in quello dei colleghi, e consente inoltre di segnalare bug e chiedere aiuto in modo più efficace.

Un caso di test ridotto è un esempio di codice che dimostra il problema nel modo più semplice possibile, rimuovendo contenuti e stili circostanti non correlati. Spesso ciò significa estrarre il codice problematico dal layout per creare un piccolo esempio che mostri solo quel codice o quella funzionalità.

Per creare un caso di test ridotto:

1. Se il markup viene generato dinamicamente, ad esempio tramite un CMS, creare una versione statica dell'output che mostri il problema. Un sito di condivisione del codice come [CodePen](https://codepen.io/) è utile per ospitare casi di test ridotti, poiché saranno accessibili online e facilmente condivisibili con i colleghi. Si può iniziare eseguendo View Source della pagina e copiando l'HTML in CodePen, quindi recuperando il CSS e JavaScript pertinenti e includendo anche questi. Successivamente, è possibile verificare se il problema è ancora evidente.
2. Se rimuovendo JavaScript il problema non scompare, non includere JavaScript. Se rimuovendo JavaScript il problema _scompare_, allora rimuovere quanto più JavaScript possibile, mantenendo ciò che causa il problema.
3. Rimuovere qualsiasi HTML che non contribuisca al problema. Rimuovere componenti o persino elementi principali del layout. Anche in questo caso, cercare di arrivare alla quantità minima di codice che continua a mostrare il problema.
4. Rimuovere qualsiasi CSS che non influisca sul problema.

Durante questo processo, si potrebbe scoprire cosa sta causando il problema o almeno riuscire ad attivarlo e disattivarlo rimuovendo qualcosa di specifico. È utile aggiungere alcuni commenti al codice man mano che si scoprono elementi rilevanti. Se è necessario chiedere aiuto, questi mostreranno alla persona che aiuta cosa è già stato provato. Ciò potrebbe fornire informazioni sufficienti per cercare problemi probabili e soluzioni alternative.

Se risulta ancora difficile correggere il problema, avere un caso di test ridotto fornisce qualcosa con cui chiedere aiuto, pubblicandolo in un forum o mostrandolo a un collega. È molto più probabile ricevere aiuto se si può dimostrare di aver svolto il lavoro di riduzione del problema e di aver identificato esattamente dove si verifica, prima di chiedere assistenza. Uno sviluppatore più esperto potrebbe individuare rapidamente il problema e indicare la giusta direzione; anche in caso contrario, il caso di test ridotto gli permetterà di esaminarlo rapidamente e, si spera, di offrire almeno qualche suggerimento.

Nel caso in cui il problema sia effettivamente un bug di un browser, un caso di test ridotto può essere usato anche per inviare una segnalazione di bug al fornitore del browser pertinente, ad esempio sul [sito bugzilla](https://bugzilla.mozilla.org/) di Mozilla.

Con l'aumentare dell'esperienza con CSS, si diventerà più rapidi nell'individuare i problemi. Tuttavia, anche gli sviluppatori più esperti talvolta si chiedono cosa stia succedendo. Adottare un approccio metodico, creare un caso di test ridotto e spiegare il problema a qualcun altro porterà generalmente a trovare una correzione.

## Riepilogo

Ecco quindi un'introduzione al debugging del CSS, che dovrebbe fornire alcune abilità utili su cui fare affidamento quando sarà necessario eseguire il debugging di CSS e di altri tipi di codice nel corso della carriera.

Questo modulo termina qui. Quando si è pronti, è possibile passare al modulo sullo [stile del testo CSS](/it/docs/Learn_web_development/Core/Text_styling).

## Vedere anche

- [Firefox > Esaminare e modificare CSS](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html), Firefox Source Docs
- [Chrome > Visualizzare e modificare CSS](https://developer.chrome.com/docs/devtools/css/), developer.chrome.com

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Home_color_scheme_search", "Learn_web_development/Core/Text_styling", "Learn_web_development/Core/Styling_basics")}}
