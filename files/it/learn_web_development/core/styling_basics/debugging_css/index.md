---
title: Debugging CSS
slug: Learn_web_development/Core/Styling_basics/Debugging_CSS
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics")}}

A volte, quando si scrive CSS, ci si imbatte in un problema in cui il CSS non sembra fare ciò che si prevede. Forse si crede che un certo selettore dovrebbe corrispondere a un elemento, ma non accade nulla, oppure un riquadro ha una dimensione diversa da quella prevista. Questo articolo offrirà una guida su come affrontare il debugging di un problema CSS e ti mostrerà come gli strumenti di sviluppo inclusi in tutti i browser moderni possono aiutare a capire cosa sta succedendo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >, basi di stile CSS (coperte nelle lezioni precedenti di questo modulo!)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Usare il <a href="https://validator.w3.org/">validatore HTML</a> per vedere se ci sono marcature non valide sulla pagina che causano problemi di CSS.</li>
          <li>Usare il <a href="https://jigsaw.w3.org/css-validator/">validatore CSS</a> per controllare l'eventuale presenza di codice CSS malformato.</li>
          <li>Usare gli strumenti di sviluppo del browser per ispezionare il CSS applicato agli elementi HTML su una pagina.</li>
          <li>Modificare il CSS applicato per determinare quali cambiamenti sono necessari per ottenere ciò che si desidera. Questo include abilitare e disabilitare dichiarazioni, modificare valori e aggiungere nuove dichiarazioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Come accedere agli strumenti di sviluppo del browser

L'articolo [Cosa sono gli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) spiega come accedere agli strumenti su vari browser e piattaforme. Mentre potresti scegliere di sviluppare principalmente in un determinato browser, diventando quindi più familiare con gli strumenti inclusi in quel browser, è utile sapere come accedirli in altri browser. Questo aiuterà se si osservano differenze di rendering tra più browser.

In questa lezione esamineremo alcune caratteristiche utili dei DevTools di Firefox per lavorare con il CSS. Utilizzerò [un file di esempio](https://mdn.github.io/css-examples/learn/inspecting/inspecting.html). Carica questo esempio in una nuova scheda se vuoi seguirlo, e apri i tuoi DevTools come descritto nell'articolo collegato sopra.

## Il DOM contro la visualizzazione del codice sorgente

Qualcosa che può confondere i nuovi arrivati ai DevTools è la differenza tra ciò che si vede quando si [visualizza il codice sorgente](https://firefox-source-docs.mozilla.org/devtools-user/view_source/index.html) di una pagina web, o si guarda il file HTML che si è caricato sul server, e ciò che si può vedere nel [pannello HTML](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#html-pane) dei DevTools. Anche se sembra simile a ciò che si può vedere tramite Visualizza codice sorgente, ci sono alcune differenze.

Nel DOM renderizzato, il browser potrebbe aver normalizzato l'HTML, ad esempio correggendo qualche HTML scritto male per te. Se hai chiuso un elemento in modo errato, ad esempio aprendo un `<h2>` ma chiudendo con un `</h3>`, il browser capirà cosa intendevi fare e nel DOM l'HTML chiuderà correttamente il `<h2>` aperto con un `</h2>`. Il DOM mostrerà anche eventuali cambiamenti apportati da JavaScript.

Visualizza codice sorgente, in confronto, è il codice sorgente HTML come memorizzato sul server. L'[albero HTML](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_html/index.html#html-tree) nei tuoi DevTools mostra esattamente ciò che il browser sta rendendo in un dato momento, quindi ti dà un'idea di cosa sta realmente accadendo.

## Ispezionare il CSS applicato

Seleziona un elemento sulla tua pagina, facendo clic destro/ctrl su di esso e selezionando _Ispeziona_, oppure selezionandolo dall'albero HTML a sinistra della visualizzazione dei DevTools. Prova a selezionare l'elemento con la classe `box1`; questo è il primo elemento sulla pagina con un riquadro bordato disegnato attorno ad esso.

![La pagina di esempio per questo tutorial con DevTools aperti.](inspecting1.png)

Se guardi alla [vista Regole](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#rules-view) a destra del tuo HTML, dovresti essere in grado di vedere le proprietà CSS e i valori applicati a quell'elemento. Vedrai le regole direttamente applicate alla classe `box1` e anche il CSS che viene ereditato dal riquadro dai suoi antenati, in questo caso dal `<body>`. Questo è utile se vedi applicare del CSS che non ti aspettavi. Forse viene ereditato da un elemento padre e devi aggiungere una regola per sovrascriverlo nel contesto di questo elemento.

Anche utile è la possibilità di espandere le proprietà abbreviate. Nel nostro esempio viene usata la scorciatoia `margin`.

**Fai clic sulla piccola freccia per espandere la visualizzazione, mostrando le diverse proprietà dettagliate e i loro valori.**

**Puoi attivare e disattivare i valori nella visualizzazione Regole quando quel pannello è attivo — se tieni il mouse su di esso compariranno delle caselle di controllo. Deseleziona la casella di controllo di una regola, ad esempio `border-radius`, e il CSS smetterà di essere applicato.**

Puoi usarlo per fare un confronto A/B, decidere se qualcosa appare meglio con una regola applicata o no, e anche per aiutare a fare debug — ad esempio, se un layout sta andando storto e stai cercando di capire quale proprietà sta causando il problema.

## Modifica dei valori

Oltre a attivare e disattivare le proprietà, puoi modificarne i valori. Forse desideri vedere se un altro colore appare meglio o desideri regolare la dimensione di qualcosa? DevTools può farti risparmiare molto tempo modificando un foglio di stile e ricaricando la pagina.

**Con `box1` selezionato, fai clic sul campione (il piccolo cerchio colorato) che mostra il colore applicato al bordo. Si aprirà un selettore di colori e potrai provare colori diversi; questi verranno aggiornati in tempo reale sulla pagina. In modo simile, potresti cambiare la larghezza o lo stile del bordo.**

![Pannello Stili DevTools con un selettore di colori aperto.](inspecting2-color-picker.png)

## Aggiungere una nuova proprietà

Puoi aggiungere proprietà utilizzando i DevTools. Forse hai realizzato che non vuoi che il tuo riquadro erediti la dimensione del font dell'elemento `<body>`, e vuoi impostare una dimensione specifica? Puoi provarlo nei DevTools prima di aggiungerlo al tuo file CSS.

**Puoi fare clic sulla parentesi graffa di chiusura nella regola per iniziare a inserire una nuova dichiarazione in essa, a questo punto puoi iniziare a digitare la nuova proprietà e DevTools ti mostrerà un elenco di completamento automatico delle proprietà corrispondenti. Dopo aver selezionato `font-size`, inserisci il valore che desideri provare. Puoi anche fare clic sul pulsante + per aggiungere una regola aggiuntiva con lo stesso selettore e aggiungere le tue nuove regole lì.**

![Il pannello DevTools, aggiungendo una nuova proprietà alle regole, con l'autocompletamento per font- aperto](inspecting3-font-size.png)

> [!NOTE]
> Ci sono anche altre funzionalità utili nella vista Regole, ad esempio le dichiarazioni con valori non validi sono barrate. Puoi saperne di più su [Esaminare e modificare il CSS](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html).

## Comprendere il modello di riquadro

Nelle lezioni precedenti abbiamo discusso [del modello di riquadro](/it/docs/Learn_web_development/Core/Styling_basics/Box_model), e del fatto che abbiamo un modello di riquadro alternativo che cambia come vengono calcolate le dimensioni degli elementi basandosi sulla dimensione che gli assegni, oltre al padding e ai bordi. DevTools può veramente aiutarti a capire come viene calcolata la dimensione di un elemento.

La [vista Layout](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/ui_tour/index.html#layout-view) ti mostra un diagramma del modello di riquadro sull'elemento selezionato, insieme a una descrizione delle proprietà e dei valori che cambiano come l'elemento è disposto. Questo include una descrizione delle proprietà che potresti non aver usato esplicitamente sull'elemento, ma che hanno comunque valori iniziali impostati.

In questo pannello, una delle proprietà dettagliate è la proprietà `box-sizing`, che controlla quale modello di riquadro l'elemento utilizza.

**Confronta le due caselle con le classi `box1` e `box2`. Entrambi hanno la stessa larghezza applicata (400px), tuttavia `box1` è visivamente più largo. Puoi vedere nel pannello layout che sta usando `content-box`. Questo è il valore che prende la dimensione che assegni all'elemento e poi aggiunge il padding e la larghezza del bordo.**

L'elemento con una classe di `box2` sta usando `border-box`, quindi qui il padding e il bordo vengono sottratti dalla dimensione che hai assegnato all'elemento. Questo significa che lo spazio occupato sulla pagina dal riquadro è esattamente la dimensione che hai specificato — nel nostro caso `width: 400px`.

![La sezione Layout dei DevTools](inspecting4-box-model.png)

> [!NOTE]
> Per saperne di più su [Esaminare e ispezionare il modello di riquadro](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_the_box_model/index.html).

## Risolvere problemi di specificità

A volte durante lo sviluppo, ma in particolare quando si deve modificare il CSS su un sito esistente, ci si trova ad avere difficoltà a far applicare un certo CSS. Non importa cosa si faccia, l'elemento sembra semplicemente non prendere il CSS. Ciò che generalmente accade qui è che un selettore più specifico sta sovrascrivendo le tue modifiche, e qui DevTools ti aiuterà veramente.

Nel nostro file di esempio ci sono due parole che sono state racchiuse in un elemento `<em>`. Una è mostrata in arancione e l'altra in hotpink. Nel CSS abbiamo applicato:

```css
em {
  color: hotpink;
  font-weight: bold;
}
```

Sopra di esso nel foglio di stile c'è però una regola con un selettore `.special`:

```css
.special {
  color: orange;
}
```

Come ricorderai dalla lezione su [gestire i conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts) dove abbiamo discusso di specificità, i selettori di classe sono più specifici dei selettori di elemento, e quindi questo è il valore che si applica. DevTools può aiutarti a trovare tali problemi, specialmente se le informazioni sono sepolte da qualche parte in un grande foglio di stile.

**Ispeziona l'elemento `<em>` con la classe `.special` e DevTools ti mostrerà che l'arancione è il colore che si applica, e anche che la proprietà `color` applicata all'elemento `<em>` è barrata. Ora puoi vedere che il selettore di classe sta sovrascrivendo il selettore di elemento.**

![Selezionare un em e guardare DevTools per vedere cosa sta sovrascrivendo il colore.](inspecting5-specificity.png)

## Debugging dei problemi nel CSS

DevTools può essere di grande aiuto nel risolvere problemi CSS, quindi quando ti trovi in una situazione in cui il CSS non si comporta come ti aspetti, come dovresti procedere per risolverlo? I seguenti passaggi dovrebbero aiutarti.

### Prenditi una pausa dal problema

Qualsiasi problema di codifica può essere frustrante, specialmente i problemi CSS, perché spesso non si ottiene un messaggio di errore da cercare online per aiutare a trovare una soluzione. Se ti stai innervosendo, prenditi una pausa dalla questione per un po' — vai a fare una passeggiata, prendi una bevanda, parla con un collega, o lavora su qualcos'altro per un po'. A volte la soluzione appare magicamente quando smetti di pensare al problema, e anche se non accade, lavorarci quando si è rinfrescati sarà molto più facile.

### Hai HTML e CSS validi?

I browser si aspettano che il tuo CSS e HTML siano scritti correttamente, tuttavia i browser sono anche molto indulgenti e cercheranno di visualizzare il tuo sito anche se ci sono errori nel markup o nel foglio di stile. Se hai errori nel tuo codice, il browser deve fare una supposizione su cosa intendevi, e potrebbe fare una scelta diversa da quella che avevi in mente. Inoltre, due browser diversi potrebbero gestire il problema in modi diversi. Un buon primo passo, quindi, è eseguire il tuo HTML e CSS attraverso un validatore, per rilevare e correggere eventuali errori.

- [Validatore CSS](https://jigsaw.w3.org/css-validator/)
- [Validatore HTML](https://validator.w3.org/)

### La proprietà e il valore sono supportati dal browser che stai testando?

I browser ignorano i CSS che non comprendono. Se la proprietà o il valore che stai usando non è supportato dal browser che stai testando, allora nulla si romperà, ma quel CSS non verrà applicato. DevTools generalmente evidenzierà le proprietà e i valori non supportati in qualche modo. Nello screenshot qui sotto, il browser non supporta il valore subgrid di {{cssxref("grid-template-columns")}}.

![Immagine dei DevTools del browser con grid-template-columns: subgrid barrato poiché il valore subgrid non è supportato.](no-support.png)

Puoi anche dare un'occhiata alle tabelle di compatibilità del browser in fondo a ogni pagina delle proprietà su MDN. Queste mostrano il supporto del browser per quella proprietà, spesso suddiviso se c'è supporto per alcuni usi della proprietà e non per altri. [Vedi la tabella di compatibilità per la proprietà `grid-template-columns`](/it/docs/Web/CSS/grid-template-columns#browser_compatibility).

### Qualcosa sta sovrascrivendo il tuo CSS?

Qui entra in gioco l'informazione che hai appreso sulla specificità. Se hai qualcosa di più specifico che sovrascrive ciò che stai cercando di fare, puoi entrare in un gioco molto frustrante nel cercare di capire cosa. Tuttavia, come descritto sopra, DevTools ti mostrerà quale CSS si sta applicando e puoi capire come rendere il nuovo selettore abbastanza specifico per sovrascriverlo.

### Crea un caso di test ridotto del problema

Se il problema non è risolto dai passaggi sopra, allora dovrai fare ulteriori indagini. La cosa migliore da fare a questo punto è creare qualcosa noto come caso di test ridotto. Essere in grado di "ridurre un problema" è una competenza davvero utile. Ti aiuterà a trovare problemi nel tuo codice e in quello dei tuoi colleghi, e ti permetterà anche di segnalare bug e chiedere aiuto in modo più efficace.

Un caso di test ridotto è un esempio di codice che dimostra il problema nel modo più semplice possibile, con contenuti e stili circostanti non correlati rimossi. Questo spesso significa prendere il codice problematico fuori dal tuo layout per creare un piccolo esempio che mostra solo quel codice o caratteristica.

Per creare un caso di test ridotto:

1. Se il tuo markup è generato dinamicamente — ad esempio tramite un CMS — fai una versione statica dell'output che mostra il problema. Un sito di condivisione di codice come [CodePen](https://codepen.io/) è utile per ospitare casi di test ridotti, poiché sono accessibili online e puoi facilmente condividerli con i colleghi. Puoi iniziare facendo Visualizza codice sorgente sulla pagina e copiando l'HTML in CodePen, poi prendi il CSS e il JavaScript rilevanti e includilo anche. Dopodiché, puoi verificare se il problema è ancora evidente.
2. Se rimuovere JavaScript non fa scomparire il problema, non includere JavaScript. Se rimuovere JavaScript _fa_ scomparire il problema, allora rimuovi il più possibile di JavaScript, lasciando il necessario che causa il problema.
3. Rimuovere qualsiasi HTML che non contribuisce al problema. Rimuovi componenti o anche elementi principali del layout. Ancora, cerca di arrivare alla quantità minima di codice che mostra ancora il problema.
4. Rimuovere qualsiasi CSS che non influisce sul problema.

Nel processo di fare questo, potresti scoprire cosa sta causando il problema, o almeno essere in grado di accenderlo e spegnerlo rimuovendo qualcosa di specifico. Vale la pena aggiungere alcuni commenti al tuo codice mentre scopri le cose. Se hai bisogno di chiedere aiuto, mostreranno alla persona che ti aiuta cosa hai già provato. Questo potrebbe benissimo darti abbastanza informazioni per essere in grado di cercare probabili problemi e soluzioni alternative.

Se stai ancora avendo difficoltà a risolvere il problema, avere un caso di test ridotto ti dà qualcosa per chiedere aiuto, pubblicando su un forum, o mostrato a un collega. È molto più probabile che riceverai aiuto se puoi dimostrare che hai fatto il lavoro di riduzione del problema e identificato esattamente dove accade, prima di chiedere aiuto. Un sviluppatore più esperto potrebbe essere in grado di individuare rapidamente il problema e indirizzarti nella direzione giusta, e anche se non fosse così, il tuo caso di test ridotto gli permetterà di dare un'occhiata veloce e sperabilmente essere in grado di offrirti almeno un po' di aiuto.

Nel caso in cui il tuo problema sia effettivamente un bug in un browser, allora un caso di test ridotto può anche essere utilizzato per presentare un rapporto di bug al venditore del browser pertinente (ad esempio, sul sito di [bugzilla di Mozilla](https://bugzilla.mozilla.org/)).

Man mano che diventi più esperto con il CSS, scoprirai che diventi più veloce a capire i problemi. Tuttavia, anche i più esperti di noi a volte si trovano a chiedersi cosa diavolo stia succedendo. Prendere un approccio metodico, fare un caso di test ridotto, e spiegare il problema a qualcun altro di solito porterà a una soluzione trovata.

## Sommario

Ecco dunque: un'introduzione al debugging CSS, che dovrebbe darti delle competenze utili su cui contare quando inizi a fare debugging del CSS e di altri tipi di codice più avanti nella tua carriera.

Ecco tutto per tutte le lezioni di questo modulo. Per concluderlo, metteremo alla prova la tua conoscenza degli argomenti trattati con una serie di sfide.

## Vedi anche

- [Firefox > Esaminare e modificare il CSS](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html), Documenti sorgente di Firefox
- [Chrome > Visualizzare e modificare il CSS](https://developer.chrome.com/docs/devtools/css/), developer.chrome.com

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics")}}
