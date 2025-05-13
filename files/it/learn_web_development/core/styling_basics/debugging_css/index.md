---
title: Debugging CSS
slug: Learn_web_development/Core/Styling_basics/Debugging_CSS
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics")}}

A volte, quando si scrive CSS, si può incorrere in un problema in cui il CSS non sembra funzionare come ci si aspetta. Forse si crede che un determinato selettore debba corrispondere a un elemento, ma non accade nulla, oppure un riquadro ha dimensioni diverse da quelle previste. Questo articolo fornirà alcune linee guida su come procedere per il debug di un problema con il CSS e mostrerà come gli strumenti per sviluppatori (DevTools) inclusi in tutti i browser moderni possano aiutare a capire cosa sta succedendo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >, Fondamenti di styling CSS (trattati nelle lezioni precedenti in questo modulo!)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Utilizzare il <a href="https://validator.w3.org/">validatore HTML</a> per verificare se ci sono markup non validi nella sua pagina che causano problemi con il CSS.</li>
          <li>Utilizzare il <a href="https://jigsaw.w3.org/css-validator/">validatore CSS</a> per controllare il codice CSS non ben formattato.</li>
          <li>Usare gli strumenti per sviluppatori del browser per ispezionare il CSS applicato agli elementi HTML in una pagina.</li>
          <li>Modificare il CSS applicato per capire quali cambiamenti sono necessari per ottenere ciò che desidera. Questo include l'attivazione e la disattivazione delle dichiarazioni, la modifica dei valori e l'aggiunta di nuove dichiarazioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Come accedere ai DevTools del browser

L'articolo [Che cosa sono gli strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) spiega come accedere agli strumenti in vari browser e piattaforme. Mentre può scegliere di sviluppare principalmente in un determinato browser, e quindi familiarizzare maggiormente con gli strumenti inclusi in quel browser, è utile sapere come accedervi in altri browser. Questo sarà utile se nota differenze di rendering tra più browser.

In questa lezione esamineremo alcune funzionalità utili dei DevTools di Firefox per lavorare con il CSS. Per farlo userò [un file di esempio](https://mdn.github.io/css-examples/learn/inspecting/inspecting.html). Carichi questo file in una nuova scheda se vuole seguire il tutorial, e apra il suo DevTools come descritto nell'articolo collegato sopra.

## Il DOM contro la visualizzazione del sorgente

Qualcosa che può confondere i nuovi utenti dei DevTools è la differenza tra ciò che si vede quando si [visualizza il sorgente](https://firefox-source-docs.mozilla.org/devtools-user/view_source/index.html) di una pagina web, o si guarda il file HTML che è stato caricato sul server, e ciò che può vedere nel [Riquadro HTML](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#html-pane) dei DevTools. Anche se sembra simile a ciò che può vedere tramite la visualizzazione del sorgente, ci sono delle differenze.

Nel DOM reso, il browser potrebbe aver normalizzato l'HTML, ad esempio correggendo alcuni HTML scritti male per lei. Se ha chiuso un elemento in modo errato, ad esempio aprendo un `<h2>` ma chiudendo con un `</h3>`, il browser capirà cosa intendeva fare e l'HTML nel DOM chiuderà correttamente l`<h2>` aperto con un `</h2>`. Il DOM mostrerà anche eventuali modifiche apportate da JavaScript.

Visualizza Sorgente, in confronto, è il codice sorgente HTML come memorizzato sul server. L'[albero HTML](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_html/index.html#html-tree) nei suoi DevTools mostra esattamente cosa il browser sta renderizzando in qualsiasi momento, quindi le dà un'idea di ciò che sta realmente accadendo.

## Ispezionare il CSS applicato

Selezioni un elemento sulla sua pagina, sia facendo clic destro/ctrl su di esso e selezionando _Ispeziona_, sia selezionandolo dall'albero HTML a sinistra della finestra dei DevTools. Provi a selezionare l'elemento con la classe `box1`; è il primo elemento sulla pagina con un riquadro delimitato disegnato intorno.

![La pagina di esempio per questo tutorial con i DevTools aperti.](inspecting1.png)

Se guarda alla [vista Regole](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#rules-view) a destra del suo HTML, dovrebbe essere in grado di vedere le proprietà CSS e i valori applicati a quel elemento. Vedrà le regole applicate direttamente alla classe `box1` e anche il CSS che viene ereditato dalla casella dai suoi antenati, in questo caso da `<body>`. Questo è utile se vede che alcuni CSS sono applicati inaspettatamente. Forse viene ereditato da un elemento padre e deve aggiungere una regola per sovrascriverlo nel contesto di questo elemento.

È anche utile la possibilità di espandere le proprietà shorthand. Nel nostro esempio viene utilizzato lo shorthand `margin`.

**Clicchi sulla piccola freccia per espandere la vista, mostrando le diverse proprietà dettagliate e i loro valori.**

**Può attivare e disattivare i valori nella vista Regole quando quel pannello è attivo — se tiene il mouse sopra, appariranno delle caselle di controllo. Deselezioni la casella di controllo di una regola, ad esempio `border-radius`, e il CSS smetterà di applicarsi.**

Può usare questo per fare un confronto A/B, decidendo se qualcosa sembra meglio con una regola applicata o no, e anche per aiutare a fare il debug — ad esempio, se un layout va male e prova a capire quale proprietà sta causando il problema.

## Modificare i valori

Oltre a attivare e disattivare le proprietà, può modificarne i valori. Forse vuole vedere se un altro colore sembra migliore o desidera modificare la dimensione di qualcosa? I DevTools possono farle risparmiare molto tempo modificando un foglio di stile e ricaricando la pagina.

**Con `box1` selezionato, clicchi sulla campionatura (il piccolo cerchio colorato) che mostra il colore applicato al bordo. Si aprirà un selettore di colori e può provare alcuni colori diversi; questi si aggiorneranno in tempo reale sulla pagina. In modo simile, potrebbe cambiare la larghezza o lo stile del bordo.**

![Pannello Styles dei DevTools con un selettore di colori aperto.](inspecting2-color-picker.png)

## Aggiungere una nuova proprietà

Può aggiungere proprietà utilizzando i DevTools. Forse ha realizzato che non vuole che la sua casella erediti la dimensione del font dell'elemento `<body>`, e vuole impostare la sua dimensione specifica? Può provare questo nei DevTools prima di aggiungerlo al suo file CSS.

**Può cliccare la parentese graffa di chiusura nella regola per iniziare a inserire una nuova dichiarazione e, a questo punto, può iniziare a digitare la nuova proprietà e i DevTools le mostreranno un elenco di completamento automatico delle proprietà corrispondenti. Dopo aver selezionato `font-size`, inserisca il valore che desidera provare. Può anche cliccare il pulsante + per aggiungere un'ulteriore regola con lo stesso selettore e aggiungere lì le sue nuove regole.**

![Il pannello DevTools, aggiungendo una nuova proprietà alle regole, con il completamento automatico per font- aperto.](inspecting3-font-size.png)

> [!NOTE]
> Ci sono altre funzionalità utili anche nella vista Regole, ad esempio le dichiarazioni con valori non validi sono barrate. Può saperne di più su [Esaminare e modificare il CSS](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html).

## Comprendere il modello della scatola

Nelle lezioni precedenti abbiamo discusso [il modello della scatola](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) e il fatto che abbiamo un modello di scatola alternativo che cambia il modo in cui vengono calcolate le dimensioni degli elementi basati sulla dimensione che viene data loro, oltre al padding e ai bordi. I DevTools possono davvero aiutare a capire come viene calcolata la dimensione di un elemento.

La [vista Layout](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#layout-view) mostra un diagramma del modello della scatola sull'elemento selezionato, insieme a una descrizione delle proprietà e dei valori che cambiano il layout dell'elemento. Ciò include una descrizione delle proprietà che potrebbe non aver esplicitamente usato sull'elemento, ma che comunque hanno valori iniziali impostati.

In questo pannello, una delle proprietà dettagliate è la proprietà `box-sizing`, che controlla quale modello di scatola l'elemento utilizza.

**Confronti le due scatole con classi `box1` e `box2`. Entrambe hanno la stessa larghezza applicata (400px), tuttavia `box1` è visualmente più ampia. Può vedere nel pannello layout che sta utilizzando `content-box`. Questo è il valore che prende la dimensione che dà all'elemento e poi aggiunge il padding e la larghezza del bordo.**

L'elemento con una classe di `box2` utilizza invece `border-box`, quindi qui il padding e il bordo vengono sottratti alla dimensione che ha dato all'elemento. Questo significa che lo spazio occupato dalla scatola sulla pagina è esattamente la dimensione che ha specificato — nel nostro caso `width: 400px`.

![La sezione Layout dei DevTools](inspecting4-box-model.png)

> [!NOTE]
> Scopra di più in [Esaminare e Ispezionare il Modello della Scatola](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_the_box_model/index.html).

## Risolvere problemi di specificità

A volte durante lo sviluppo, in particolare quando deve modificare il CSS su un sito esistente, si troverà a avere difficoltà nel far applicare alcuni CSS. Non importa cosa faccia, l'elemento sembra semplicemente non prendere il CSS. Generalmente, ciò che sta accadendo è che un selettore più specifico sta sovrascrivendo le sue modifiche, e qui i DevTools le saranno davvero di aiuto.

Nel nostro file di esempio ci sono due parole che sono state racchiuse in un elemento `<em>`. Una si visualizza in arancione e l'altra in rosa acceso. Nel CSS abbiamo applicato:

```css
em {
  color: hotpink;
  font-weight: bold;
}
```

Sopra di essa nel foglio di stile, tuttavia, c'è una regola con un selettore `.special`:

```css
.special {
  color: orange;
}
```

Come ricorderà dalla lezione su [gestione dei conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts) dove abbiamo discusso della specificità, i selettori di classi sono più specifici dei selettori di elementi, e quindi questo è il valore che si applica. I DevTools possono aiutarla a trovare tali problemi, soprattutto se l'informazione è nascosta da qualche parte in un enorme foglio di stile.

**Ispezioni l`<em>` con la classe `.special` e i DevTools le mostreranno che arancio è il colore che si applica, e che anche la proprietà `color` applicata all`<em>` è barrata. Ora può vedere che il selettore di classe sta sovrascrivendo il selettore di elemento.**

![Selezionare un `em` e guardare i DevTools per vedere cosa sta sovraeseguendo il colore.](inspecting5-specificity.png)

## Debugging dei problemi nel CSS

I DevTools possono essere di grande aiuto quando si risolvono problemi con il CSS, quindi quando si trova in una situazione in cui il CSS non si comporta come ci si aspetta, come dovrebbe procedere per risolvere il problema? I seguenti passaggi dovrebbero aiutare.

### Fare un passo indietro rispetto al problema

Qualsiasi problema di codifica può essere frustrante, in particolare i problemi CSS perché spesso non si ottiene un messaggio di errore da cercare online per trovare una soluzione. Se si sente frustrato, faccia un passo indietro dal problema per un po' — faccia una passeggiata, prenda una bevanda, parli con un collega o lavori su un'altra cosa per un po'. A volte la soluzione appare magicamente quando si smette di pensare al problema, e anche se non lo fa, lavorarci sopra quando ci si sente freschi sarà molto più facile.

### Ha HTML e CSS validi?

I browser si aspettano che il suo CSS e HTML siano scritti correttamente, tuttavia i browser sono anche molto permissivi e faranno del loro meglio per visualizzare le sue pagine web anche se ci sono errori nel markup o nel foglio di stile. Se ci sono errori nel codice, il browser ha bisogno di fare un'ipotesi su cosa intendeva, e potrebbe prendere una decisione diversa da ciò che aveva in mente. Inoltre, due diversi browser potrebbero affrontare il problema in due modi diversi. Un buon primo passo, quindi, è eseguire il suo HTML e CSS attraverso un validatore, per identificare e correggere eventuali errori.

- [Validatore CSS](https://jigsaw.w3.org/css-validator/)
- [Validatore HTML](https://validator.w3.org/)

### La proprietà e il valore sono supportati dal browser che sta testando?

I browser ignorano il CSS che non capiscono. Se il nome della proprietà o il valore che sta utilizzando non sono supportati dal browser che sta testando, niente si romperà, ma quel CSS non verrà applicato. I DevTools generalmente evidenzieranno proprietà e valori non supportati in qualche modo. Nello screenshot qui sotto, il browser non supporta il valore subgrid di {{cssxref("grid-template-columns")}}.

![Immagine dei DevTools del browser con `grid-template-columns: subgrid` barrato poiché il valore subgrid non è supportato.](no-support.png)

Può anche dare uno sguardo alle Tabelle della compatibilità del browser che si trovano in fondo a ciascuna pagina delle proprietà su MDN. Queste mostrano il supporto del browser per quella proprietà, spesso suddiviso se c'è supporto per alcuni usi della proprietà e non per altri. [Veda la tabella di compatibilità per la proprietà `grid-template-columns`](/it/docs/Web/CSS/grid-template-columns#browser_compatibility).

### Qualcosa sta sovrascrivendo il suo CSS?

Qui le informazioni che ha appreso sulla specificità saranno molto utili. Se c'è qualcosa di più specifico che sta sovrascrivendo ciò che sta cercando di fare, può entrare in un gioco molto frustrante cercando di capire cosa. Tuttavia, come descritto sopra, i DevTools le mostreranno quale CSS si sta applicando e può lavorare su come rendere il nuovo selettore abbastanza specifico da sovrascriverlo.

### Creare un caso di test ridotto del problema

Se il problema non viene risolto dai passaggi precedenti, allora avrà bisogno di fare un po' più di ricerca. La cosa migliore da fare a questo punto è creare qualcosa noto come un caso di test ridotto. Essere in grado di "ridurre un problema" è una competenza davvero utile. Aiuterà a trovare problemi nel proprio codice e in quello dei colleghi, oltre a consentire di segnalare bug e chiedere aiuto in modo più efficace.

Un caso di test ridotto è un esempio di codice che dimostra il problema nel modo più semplice possibile, rimuovendo contenuto e formattazione non correlati. Questo spesso significherà estrarre il codice problematico dal layout per creare un piccolo esempio che mostri solo quel codice o funzione.

Per creare un caso di test ridotto:

1. Se il suo markup è generato dinamicamente — ad esempio tramite un CMS — crei una versione statica dell'output che mostra il problema. Un sito di condivisione del codice come [CodePen](https://codepen.io/) è utile per ospitare casi di test ridotti, dato che sono accessibili online e può condividerli facilmente con i colleghi. Potrebbe iniziare visualizzando il sorgente della pagina e copiando l'HTML in CodePen, quindi prendere il CSS e il JavaScript rilevanti ed includerli. Dopo di che, può verificare se il problema è ancora presente.
2. Se rimuovere il JavaScript non fa scomparire il problema, non includa il JavaScript. Se rimuovere il JavaScript _fa_ scomparire il problema, rimuova quanta più parte di JavaScript possibile, lasciando solo ciò che causa il problema.
3. Rimuova qualsiasi HTML che non contribuisce al problema. Rimuova componenti o anche elementi principali del layout. Ancora, cerchi di arrivare alla quantità minima di codice che continua a mostrare il problema.
4. Rimuova qualsiasi CSS che non impatta sul problema.

Nel processo di riduzione, potrebbe scoprire cosa sta causando il problema o almeno essere in grado di accenderlo e spegnerlo rimuovendo qualcosa di specifico. Vale la pena di aggiungere alcuni commenti al suo codice man mano che scopre le cose. Se deve chiedere aiuto, mostreranno alla persona che la aiuta cosa ha già provato. Questo potrebbe fornirle abbastanza informazioni per poter cercare problemi e soluzioni probabili.

Se sta ancora lottando per risolvere il problema, avere un caso di test ridotto le fornisce qualcosa su cui chiedere aiuto, pubblicando su un forum o mostrandolo a un collega. È molto più probabile che riceva aiuto se può dimostrare di aver lavorato per ridurre il problema e identificare esattamente dove si verifica, prima di chiedere aiuto. Uno sviluppatore più esperto potrebbe essere in grado di individuare rapidamente il problema e indicarli nella direzione giusta, e anche se non lo fosse, il suo caso di test ridotto permetterà loro di dare un'occhiata rapido e si spera di offrire almeno un po' di aiuto.

Nell'eventualità che il suo problema sia effettivamente un bug in un browser, un caso di test ridotto può anche essere utilizzato per segnalare un bug al fornitore del browser rilevante (ad esempio, sul sito [bugzilla di Mozilla](https://bugzilla.mozilla.org/)).

Man mano che acquisirà più esperienza con il CSS, scoprirà di diventare più veloce a capire i problemi. Tuttavia, anche i più esperti di noi si trovano talvolta a chiedersi cosa stia succedendo. Adottare un approccio metodico, creare un caso di test ridotto e spiegare il problema a qualcun altro solitamente porterà a una soluzione.

## Sommario

Ecco quindi: un'introduzione al debug del CSS, che dovrebbe fornirle alcune competenze utili su cui fare affidamento quando inizierà a fare il debug del CSS e di altri tipi di codice più avanti nella sua carriera.

È tutto per le lezioni in questo modulo. Per concludere, metteremo alla prova la sua conoscenza degli argomenti trattati con una serie di sfide.

## Vedere anche

- [Firefox > Esaminare e modificare il CSS](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html), Documenti di origine di Firefox
- [Chrome > Visualizzare e modificare il CSS](https://developer.chrome.com/docs/devtools/css/), developer.chrome.com

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics")}}
