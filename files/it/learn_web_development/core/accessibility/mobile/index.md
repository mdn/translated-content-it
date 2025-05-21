---
title: Accessibilità mobile
slug: Learn_web_development/Core/Accessibility/Mobile
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Multimedia","Learn_web_development/Core/Accessibility/Accessibility_troubleshooting", "Learn_web_development/Core/Accessibility")}}

Con l'accesso al web su dispositivi mobili così diffuso e piattaforme rinomate come iOS e Android dotate di strumenti di accessibilità completi, è importante considerare l'accessibilità dei contenuti web su queste piattaforme. Questo articolo esamina le considerazioni sull'accessibilità specifiche per i dispositivi mobili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le migliori pratiche di accessibilità insegnate nelle lezioni precedenti di questo modulo.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Familiarità con i lettori di schermo su iOS e Android.</li>
          <li>Familiarità con i problemi di accessibilità legati a certi tipi di eventi.</li>
          <li>Tecniche specifiche per meccanismi di input utente più usabili su mobile.</li>
          <li>Sapere che i browser mobili forniscono vantaggi specifici di usabilità per tipi di <code>&lt;input&gt;</code> specifici come <code>number</code> o <code>tel</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Accessibilità sui dispositivi mobili

Lo stato dell'accessibilità — e il supporto agli standard web in generale — è buono nei dispositivi mobili moderni. Sono passati i giorni in cui i dispositivi mobili utilizzavano tecnologie web completamente diverse dai browser desktop, costringendo gli sviluppatori a rilevare il tipo di browser e servire loro siti completamente separati (anche se molte aziende continuano a rilevare l'uso di dispositivi mobili e a servire loro un dominio mobile separato).

Oggi, i dispositivi mobili possono gestire normalmente siti web completi, e le principali piattaforme hanno persino lettori di schermo integrati per abilitare l'uso da parte degli utenti ipovedenti. Anche i browser mobili moderni tendono ad avere un buon supporto per [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

Per rendere un sito web accessibile e usabile su mobile, è sufficiente seguire le migliori pratiche generali di web design e accessibilità.

Esistono alcune eccezioni che necessitano di considerazioni speciali per i mobili; le principali sono:

- Meccanismi di controllo — Assicurarsi che i controlli dell'interfaccia, come i pulsanti, siano accessibili su mobili (cioè, principalmente touchscreen), oltre che su desktop/laptop (principalmente mouse/tastiera).
- Input utente — Rendere i requisiti di input utente il più semplici possibile su mobile (ad esempio, nei moduli, ridurre al minimo la digitazione).
- Design reattivo — Assicurarsi che i layout funzionino su mobili, conservare le dimensioni di download delle immagini e pensare alla fornitura di immagini per schermi ad alta risoluzione.

## Riepilogo dei test del lettore di schermo su Android e iOS

Le piattaforme mobili più comuni hanno lettori di schermo pienamente funzionali. Questi funzionano in modo simile ai lettori di schermo desktop, eccetto che sono operati principalmente tramite gesti touch anziché combinazioni di tasti.

Esaminiamo i due principali: TalkBack su Android e VoiceOver su iOS.

### Android TalkBack

Il lettore di schermo TalkBack è integrato nel sistema operativo Android.

Per attivarlo, cerca quale modello di telefono e versione di Android hai, quindi cerca dove si trova il menu TalkBack. Tende a variare ampiamente tra le versioni di Android e anche tra diversi modelli di telefoni. Alcuni produttori di telefoni (es. Samsung) non includono nemmeno TalkBack nei telefoni più recenti e hanno invece optato per il loro lettore di schermo.

Quando hai trovato il menu TalkBack, premi l'interruttore a scorrimento per attivare TalkBack. Segui eventuali ulteriori istruzioni a schermo che ti vengono presentate.

Quando TalkBack è attivo, i controlli di base del tuo dispositivo Android saranno un po' diversi. Ad esempio:

1. Toccare una volta un'app la selezionerà e il dispositivo leggerà cosa è l'app.
2. Scorrendo a sinistra e a destra si sposterà tra le app, o tra pulsanti/controlli se sei in una barra di controllo. Il dispositivo leggerà ogni opzione.
3. Toccare due volte ovunque aprirà l'app/selezionerà l'opzione.
4. Puoi anche "esplorare al tatto" — tieni premuto il dito sullo schermo e trascinalo, e il tuo dispositivo leggerà le diverse app/voci su cui ti sposti.

Se vuoi disattivare TalkBack:

1. Torna alla schermata del menu TalkBack (usando i diversi gesti attualmente abilitati).
2. Naviga fino all'interruttore a scorrimento e attivalo per spegnerlo.

> [!NOTE]
> Puoi accedere alla schermata iniziale in qualsiasi momento scorrendo in alto e a sinistra con un movimento fluido. Se hai più di una schermata iniziale, puoi muoverti tra di esse scorrendo due dita a sinistra e a destra.

Per un elenco più completo dei gesti di TalkBack, vedi [Use TalkBack gestures](https://support.google.com/accessibility/android/answer/6151827).

#### Sbloccare il telefono

Quando TalkBack è attivo, sbloccare il telefono è un po' diverso.

Puoi eseguire uno swipe con due dita verso l'alto dalla parte inferiore della schermata di blocco. Se hai impostato un codice di sblocco o un pattern, verrai portato alla schermata di immissione pertinente per inserirlo.

Puoi anche esplorare al tatto per trovare il pulsante _Sblocca_ nella parte inferiore centrale dello schermo, e poi toccare due volte.

#### Menu globali e locali

TalkBack ti consente di accedere a menu di contesto globali e locali, ovunque ti trovi nel dispositivo. Il primo fornisce opzioni globali relative al dispositivo nel suo insieme, mentre il secondo fornisce opzioni relative solo all'app/schermata attuale in cui ti trovi.

Per accedere a questi menu:

1. Accedi al menu globale scorrendo velocemente verso il basso, e poi a destra.
2. Accedi al menu locale scorrendo velocemente verso l'alto, e poi a destra.
3. Scorri a sinistra e a destra per passare tra le diverse opzioni.
4. Una volta selezionata l'opzione desiderata, tocca due volte per scegliere quell'opzione.

Per dettagli su tutte le opzioni disponibili nei menu di contesto globali e locali, vedere [Use global and local context menus](https://support.google.com/accessibility/android/answer/6007066).

#### Navigazione nelle pagine Web

Puoi utilizzare il menu di contesto locale mentre sei in un browser web per trovare opzioni per navigare tra le pagine web utilizzando solo i titoli, i controlli dei moduli o i collegamenti, oppure navigare linea per linea, ecc.

Ad esempio, con TalkBack attivato:

1. Apri il tuo browser web.
2. Attiva la barra degli URL.
3. Inserisci una pagina web che abbia un insieme di titoli, come la pagina principale di bbc.co.uk. Per inserire il testo dell'URL:

   - Seleziona la barra degli URL scorrendo a sinistra/destra finché ci arrivi, e poi toccando due volte.
   - Tieni premuto il dito sulla tastiera virtuale fino a ottenere il carattere desiderato, quindi rilascia il dito per digitarlo. Ripeti per ogni carattere.
   - Una volta finito, trova il tasto Invio e premi.

4. Scorri a sinistra e a destra per spostarti tra le diverse voci sulla pagina.
5. Scorri in alto e a destra con un movimento fluido per entrare nel menu del contenuto locale.
6. Scorri a destra finché non trovi l'opzione "Titoli e Punti di riferimento".
7. Tocca due volte per selezionarla. Ora sarai in grado di scorrere a sinistra e a destra per muoverti tra titoli e punti di riferimento ARIA.
8. Per tornare alla modalità predefinita, entra nuovamente nel menu di contesto locale scorrendo in alto e a destra, seleziona "Predefinito", e poi tocca due volte per attivare.

> [!NOTE]
> Vedi [Get started on Android with TalkBack](https://support.google.com/accessibility/android/answer/6283677?hl=en&ref_topic=3529932) per una documentazione più completa.

### iOS VoiceOver

Una versione mobile di VoiceOver è integrata nel sistema operativo iOS.

Per attivarlo, vai all'app _Impostazioni_ e seleziona _Accessibilità > VoiceOver_. Premi il cursore _VoiceOver_ per abilitarlo (vedrai anche diverse altre opzioni relative a VoiceOver in questa pagina).

> [!NOTE]
> Alcuni vecchi dispositivi iOS hanno il menu VoiceOver in _Impostazioni_ > _Generali_ > _Accessibilità_ > _VoiceOver_.

Una volta abilitato VoiceOver, i gesti di controllo di base di iOS saranno un po' diversi:

1. Un tocco singolo farà sì che l'elemento su cui tocchi venga selezionato; il tuo dispositivo parlerà l'elemento su cui hai toccato.
2. Puoi anche navigare tra gli oggetti sullo schermo scorrendo a sinistra e a destra per muoverti tra di essi, oppure facendo scorrere il dito sullo schermo per spostarti tra diversi elementi (quando trovi l'elemento desiderato, puoi sollevare il dito per selezionarlo).
3. Per attivare l'elemento selezionato (ad es., aprire un'app selezionata), tocca due volte ovunque sullo schermo.
4. Scorri con tre dita per scorrere attraverso una pagina.
5. Tocca con due dita per eseguire un'azione contestualizzata — ad esempio, scattare una foto mentre sei nell'app fotocamera.

Per disattivarlo nuovamente, torna a _Impostazioni > Generali > Accessibilità > VoiceOver_ utilizzando i gesti sopra menzionati, e attiva il cursore _VoiceOver_ per disattivarlo.

#### Sblocca telefono

Per sbloccare il telefono, devi premere il pulsante home (o scorrere) come di consueto. Se hai impostato un codice di accesso, puoi selezionare ciascun numero scorrendo/facendo scorrere (come spiegato sopra) e poi toccare due volte per inserire ciascun numero quando hai trovato quello giusto.

#### Utilizzo del Rotor

Quando VoiceOver è attivo, hai a tua disposizione una funzionalità di navigazione chiamata Rotor, che ti consente di scegliere rapidamente tra una serie di opzioni comuni utili. Per utilizzarlo:

1. Ruota due dita sullo schermo come se stessi girando una manopola. Ogni opzione verrà letta ad alta voce man mano che ruoti ulteriormente. Puoi andare avanti e indietro per scorrere tra le opzioni.
2. Una volta trovata l'opzione desiderata:

   - Rilascia le dita per selezionarla.
   - Se è un'opzione di cui puoi iterare il valore (come Volume o Velocità di lettura), puoi fare uno swipe in su o in giù per aumentare o diminuire il valore dell'elemento selezionato.

Le opzioni disponibili sotto il Rotor sono sensibili al contesto — differiranno a seconda dell'app o della vista in cui ti trovi (vedi sotto per un esempio).

#### Navigazione nelle pagine web

Facciamo una prova con la navigazione web con VoiceOver:

1. Apri il tuo browser web.
2. Attiva la barra degli URL.
3. Inserisci una pagina web che abbia un insieme di titoli, come la pagina principale di bbc.co.uk. Per inserire il testo dell'URL:

   - Seleziona la barra degli URL scorrendo a sinistra/destra fino a raggiungerla, e quindi toccando due volte.
   - Per ogni carattere, tieni premuto il dito sulla tastiera virtuale finché non ottieni il carattere desiderato, quindi rilascia il dito per selezionarlo. Tocca due volte per digitare.
   - Una volta finito, trova il tasto Invio e premi.

4. Scorri a sinistra e a destra per spostarti tra gli elementi sulla pagina. Puoi toccare due volte un elemento per selezionarlo (ad es., seguire un collegamento).
5. Di default, l'opzione Rotor selezionata sarà Velocità di lettura; puoi attualmente scorrere su e giù per aumentare o diminuire la velocità di lettura.
6. Ora ruota due dita attorno allo schermo come un quadrante per mostrare il rotor e muoverti tra le sue opzioni. Ecco alcuni esempi delle opzioni disponibili:

   - _Velocità di lettura_: Cambia la velocità di lettura.
   - _Contenitori_: Spostarsi tra diversi contenitori semantici sulla pagina.
   - _Titoli_: Spostarsi tra i titoli sulla pagina.
   - _Link_: Spostarsi tra i link sulla pagina.
   - _Controlli moduli_: Spostarsi tra i controlli dei moduli sulla pagina.
   - _Lingua_: Spostarsi tra diverse traduzioni, se disponibili.

7. Selezionare _Titoli_. Ora potrai scorrere su e giù per muoverti tra i titoli sulla pagina.

> [!NOTE]
> Per un riferimento più completo che copre i gesti di VoiceOver disponibili e altri suggerimenti su come eseguire test di accessibilità su iOS, vedi la [documentazione di VoiceOver di Apple](https://developer.apple.com/documentation/accessibility/voiceover/).

## Meccanismi di controllo

Nel nostro articolo su accessibilità CSS e JavaScript, abbiamo esaminato l'idea degli eventi specifici di un certo tipo di meccanismo di controllo (vedi [Eventi specifici del mouse](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#mouse-specific_events)). Per riepilogare, questi causano problemi di accessibilità perché altri meccanismi di controllo non possono attivare la funzionalità associata.

Ad esempio, l'evento [click](/it/docs/Web/API/Element/click_event) è buono in termini di accessibilità — un event handler associato può essere invocato cliccando sull'elemento sul quale è impostato l'handler, selezionandolo con il tasto Tab e premendo Invio/Ritorna, o toccandolo su un dispositivo touchscreen. Prova il nostro esempio [simple-button-example.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/simple-button-example.html) ([vedilo in esecuzione live](https://mdn.github.io/learning-area/accessibility/mobile/simple-button-example.html)) per vedere cosa intendiamo.

In alternativa, eventi specifici del mouse come [mousedown](/it/docs/Web/API/Element/mousedown_event) e [mouseup](/it/docs/Web/API/Element/mouseup_event) creano problemi — i loro handler di eventi non possono essere invocati usando controlli non basati sul mouse.

Se provi a controllare il nostro esempio [simple-box-drag.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/simple-box-drag.html) ([vedi esempio dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/simple-box-drag.html)) con una tastiera o un touch, vedrai il problema. Questo accade perché stiamo usando codice come il seguente:

```js
div.onmousedown = () => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  movePanel();
};

document.onmouseup = stopMove;
```

Per abilitare altre forme di controllo, devi utilizzare eventi diversi ma equivalenti — ad esempio, gli eventi touch funzionano sui dispositivi touchscreen:

```js
div.ontouchstart = (e) => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  positionHandler(e);
  movePanel();
};

panel.ontouchend = stopMove;
```

Abbiamo fornito un esempio semplice che mostra come utilizzare insieme i mouse e gli eventi touch — vedi [multi-control-box-drag.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/multi-control-box-drag.html) (vedi anche l'[esempio dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/multi-control-box-drag.html)).

> [!NOTE]
> Puoi anche vedere esempi completamente funzionali che mostrano come implementare diversi meccanismi di controllo in [Implementing game control mechanisms](/it/docs/Games/Techniques/Control_mechanisms).

## Design reattivo

[Responsive design](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) è la pratica di rendere i layout e altre funzionalità delle app dinamicamente cambiabili a seconda di fattori come le dimensioni e la risoluzione dello schermo, così da essere utilizzabili e accessibili agli utenti di diversi tipi di dispositivo.

In particolare, i problemi più comuni che devono essere affrontati per il mobile sono:

- Adeguatezza dei layout per i dispositivi mobili. Un layout a più colonne non funzionerà altrettanto bene su uno schermo stretto, per esempio, e le dimensioni del testo potrebbero dover essere aumentate affinché siano leggibili. Tali problemi possono essere risolti creando un layout reattivo utilizzando tecnologie come le [media queries](/it/docs/Web/CSS/CSS_media_queries), il [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element) e il [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox).
- Conservare le dimensioni delle immagini scaricate. In generale, i dispositivi con schermo piccolo non avranno bisogno di immagini che sono grandi quanto le loro controparti desktop e sono più probabilmente su connessioni di rete lente. Pertanto, è saggio fornire immagini più piccole ai dispositivi con schermo stretto, quando opportuno. Puoi gestire questo usando [tecniche di immagini reattive](/it/docs/Web/HTML/Guides/Responsive_images).
- Pensare alle alte risoluzioni. Molti dispositivi mobili hanno schermi ad alta risoluzione e quindi necessitano di immagini ad alta risoluzione affinché il display possa continuare a sembrare nitido e chiaro. Ancora una volta, puoi fornire immagini adeguate usando tecniche di immagini reattive. Inoltre, molte esigenze di immagini possono essere soddisfatte usando il formato di immagini vettoriali SVG, che è ben supportato nei browser odierni. SVG ha una dimensione di file ridotta e rimarrà nitido indipendentemente dalle dimensioni in cui viene visualizzato (vedi [Includere grafica vettoriale in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML) per maggiori dettagli).

> [!NOTE]
> Non forniremo una discussione completa sulle tecniche di design reattivo qui, poiché sono coperte in altre sezioni su MDN (consulta i link sopra).

### Considerazioni specifiche per il mobile

Ci sono altri problemi importanti da considerare per rendere i siti più accessibili sui dispositivi mobili. Ne abbiamo elencati un paio qui, ma ne aggiungeremo altri quando ne penseremo.

#### Non disabilitare lo zoom

Usando [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element), è possibile disabilitare lo zoom. Assicurati sempre che il ridimensionamento sia abilitato e imposta la larghezza alla larghezza del dispositivo nel {{htmlelement("head")}}:

```html
<meta name="viewport" content="width=device-width; user-scalable=yes" />
```

Non dovresti mai impostare `user-scalable=no` se possibile — molte persone si affidano allo zoom per poter vedere i contenuti del tuo sito web, quindi togliere questa funzionalità è una pessima idea. Ci sono situazioni in cui lo zoom potrebbe interrompere l'interfaccia utente; in tali casi, se ritieni di dover disabilitare lo zoom, dovresti fornire qualche altro tipo di equivalente, come un controllo per aumentare la dimensione del testo in un modo che non interrompa la tua interfaccia utente.

#### Mantenere accessibili i menu

Poiché lo schermo è molto più stretto sui dispositivi mobili, è molto comune utilizzare media query e altre tecnologie per far sì che il menu di navigazione si riduca a una piccola icona in cima al display — che può essere premuta per rivelare il menu solo se necessario — quando il sito viene visualizzato su mobile. Questo è comunemente rappresentato da un'icona "tre linee orizzontali" e il modello di design è conseguentemente noto come "menu hamburger".

Quando si implementa tale menu, è necessario assicurarsi che il controllo per rivelarlo sia accessibile tramite meccanismi di controllo appropriati (normalmente touch per mobile), come discusso in [Meccanismi di controllo](#meccanismi_di_controllo) sopra, e che il resto della pagina venga spostato o nascosto in qualche modo mentre il menu viene accesso, per evitare confusione durante la navigazione.

Clicca qui per un [buon esempio di menu hamburger](https://fritz-weisshart.de/meg_men/).

## Input utente

Su dispositivi mobili, immettere dati tende ad essere più fastidioso per gli utenti rispetto all'esperienza equivalente su computer desktop. È più conveniente digitare testo nei campi modulo utilizzando una tastiera desktop o laptop piuttosto che una tastiera virtuale touchscreen o una piccola tastiera fisica mobile.

Per questo motivo, vale la pena provare a ridurre al minimo la quantità di digitazione necessaria. Come esempio, invece di far compilare agli utenti il loro titolo lavorativo ogni volta utilizzando un input di testo regolare, potresti invece offrire un menu {{htmlelement("select")}} contenente le opzioni più comuni (che aiuta anche con la coerenza nell'immissione dei dati) e offrire un'opzione "Altro" che visualizzi un campo di testo per eventuali valori non standard. Puoi vedere un semplice esempio di questa idea in azione in [common-job-types.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/common-job-types.html) (vedi l'[esempio di titoli di lavoro comuni dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/common-job-types.html)).

Vale anche la pena considerare l'uso dei tipi di input del modulo HTML come la data su piattaforme mobili poiché le gestiscono bene — sia Android che iOS, ad esempio, mostrano widget utilizzabili che si adattano bene all'esperienza del dispositivo. Vedi [html5-form-examples.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/html5-form-examples.html) per alcuni esempi (vedi gli [esempi di moduli HTML5 dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/html5-form-examples.html)) — prova a caricarli e manipolarli su dispositivi mobili. Ad esempio:

- I tipi `number`, `tel`, e `email` visualizzano tastiere virtuali appropriate per immettere numeri/numeri di telefono.
- I tipi `time` e `date` visualizzano selettori adatti per selezionare orari e date.

Se vuoi fornire una soluzione diversa per i desktop, potresti sempre fornire un markup diverso ai tuoi dispositivi mobili usando il rilevamento delle funzionalità. Controlla il nostro articolo sul [rilevamento delle funzionalità](/it/docs/Learn_web_development/Extensions/Testing/Feature_detection) per ulteriori informazioni.

## Riassunto

In questo articolo, ti abbiamo fornito alcuni dettagli sui problemi comuni specifici dell'accessibilità mobile e su come superarli. Ti abbiamo anche guidato attraverso l'uso dei lettori di schermo più comuni per aiutarti nei test di accessibilità.

## Vedi anche

- [Guidelines For Mobile Web Development](https://www.smashingmagazine.com/2012/07/guidelines-for-mobile-web-development/) — Un elenco di articoli su _Smashing Magazine_ che coprono diverse tecniche per il design web mobile.
- [Make your site work on touch devices](https://www.creativebloq.com/javascript/make-your-site-work-touch-devices-51411644) — Articolo utile sull'uso degli eventi touch per far funzionare le interazioni sui dispositivi mobili.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Multimedia","Learn_web_development/Core/Accessibility/Accessibility_troubleshooting", "Learn_web_development/Core/Accessibility")}}
