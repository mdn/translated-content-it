---
title: Accessibilità nei dispositivi mobili
slug: Learn_web_development/Core/Accessibility/Mobile
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Multimedia","Learn_web_development/Core/Accessibility/Accessibility_troubleshooting", "Learn_web_development/Core/Accessibility")}}

Con l'accesso al web su dispositivi mobili che è così popolare e con piattaforme rinomate come iOS e Android che dispongono di strumenti di accessibilità completi, è importante considerare l'accessibilità del suo contenuto web su queste piattaforme. Questo articolo esamina le considerazioni specifiche per l'accessibilità mobile.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le migliori pratiche di accessibilità come insegnato nelle lezioni precedenti del modulo.</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Familiarità con gli screen reader su iOS e Android.</li>
          <li>Familiarità con i problemi di accessibilità legati ad alcuni tipi di eventi.</li>
          <li>Tecniche specifiche per meccanismi di input utente più utilizzabili su mobile.</li>
          <li>Sapere che i browser mobili offrono vantaggi specifici di usabilità per determinati tipi di <code>&lt;input&gt;</code> come <code>number</code> o <code>tel</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Accessibilità sui dispositivi mobili

Lo stato dell'accessibilità — e il supporto per gli standard web in generale — è buono nei moderni dispositivi mobili. I tempi in cui i dispositivi mobili utilizzavano tecnologie web completamente diverse rispetto ai browser desktop, costringendo gli sviluppatori a utilizzare il rilevamento del browser e a servire loro siti completamente separati, sono ormai lontani (anche se alcune aziende rilevano ancora l'uso di dispositivi mobili e servono loro un dominio mobile separato).

Oggigiorno, i dispositivi mobili riescono generalmente a gestire siti web completi di funzionalità, e le principali piattaforme includono persino screen reader per consentire agli utenti con deficit visivi di utilizzarli con successo. Anche i browser mobili moderni tendono a supportare bene [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

Per rendere un sito web accessibile e utilizzabile su mobile, è sufficiente seguire le buone pratiche generali di progettazione web e di accessibilità.

Ci sono alcune eccezioni che richiedono una considerazione speciale per il mobile; le principali sono:

- Meccanismi di controllo — Assicurarsi che i controlli dell'interfaccia, come i pulsanti, siano accessibili sui dispositivi mobili (cioè, principalmente touchscreen), nonché sui desktop/laptop (principalmente mouse/tastiera).
- Input utente — Rendere i requisiti di input utente il meno possibile problematici su mobile (ad esempio, nei moduli, mantenendo il testo minimo).
- Design responsive — Assicurarsi che i layout funzionino sui dispositivi mobili, conservare le dimensioni delle immagini scaricate e pensare alla fornitura di immagini per schermi ad alta risoluzione.

## Riepilogo del testing dello screen reader su Android e iOS

Le piattaforme mobili più comuni dispongono di screen reader completamente funzionali. Questi funzionano in gran parte allo stesso modo degli screen reader desktop, salvo che sono ampiamente gestiti utilizzando gesti tattili piuttosto che combinazioni di tasti.

Vediamo i due principali: TalkBack su Android e VoiceOver su iOS.

### Android TalkBack

Lo screen reader TalkBack è integrato nel sistema operativo Android.

Per attivarlo, controlli il modello di telefono e la versione di Android e poi cerchi dove si trova il menu TalkBack. Tende a differire ampliamente tra le versioni di Android e anche tra modelli diversi di telefoni. Alcuni produttori di telefoni (ad esempio, Samsung) non hanno nemmeno TalkBack nei telefoni più recenti, optando invece per il loro screen reader.

Quando ha trovato il menu di TalkBack, prema l'interruttore a scorrimento per attivare TalkBack. Segua eventuali ulteriori prompt a schermo che le vengono presentati.

Quando TalkBack è attivato, i controlli di base del suo dispositivo Android saranno un po' diversi. Ad esempio:

1. Toccare una volta un'app la selezionerà, e il dispositivo leggerà quale app è.
2. Scorrendo verso sinistra e destra si sposterà tra le app o i pulsanti/controlli se si trova in una barra dei controlli. Il dispositivo leggerà ogni opzione.
3. Fare doppio clic su qualsiasi parte aprirà l'app/selezionerà l'opzione.
4. Può anche "esplorare al tocco" — tenga premuto il dito sullo schermo e lo trascini in giro, e il dispositivo leggerà le diverse app/elementi che si spostano.

Se vuole disattivare TalkBack:

1. Navigare di nuovo alla schermata del menu di TalkBack (usando i gesti diversi che sono attualmente abilitati).
2. Navigare fino all'interruttore a scorrimento e attivarlo per spegnerlo.

> [!NOTE]
> Può tornare alla schermata principale in qualsiasi momento scorrendo verso l'alto e verso sinistra in un movimento fluido. Se ha più di una schermata principale, può muoversi tra di esse scorrendo due dita a sinistra e a destra.

Per un elenco più completo dei gesti TalkBack, veda [Use TalkBack gestures](https://support.google.com/accessibility/android/answer/6151827).

#### Sbloccare il telefono

Quando TalkBack è attivato, sbloccare il telefono è un po' diverso.

Può fare uno swipe a due dita verso l'alto dalla parte inferiore della schermata di blocco. Se ha impostato un codice o un pattern per sbloccare il dispositivo, verrà portato alla schermata di inserimento pertinente per immetterlo.

Può anche esplorare al tocco per trovare il pulsante _Sblocca_ in basso al centro dello schermo e poi fare doppio clic.

#### Menu globali e locali

TalkBack le consente di accedere ai menu contestuali globali e locali, ovunque si sia navigato sul dispositivo. Il primo fornisce opzioni globali relative al dispositivo nel suo complesso, e il secondo fornisce opzioni relative solo all'app/schermata corrente in cui si trova.

Per accedere a questi menu:

1. Accedere al menu globale scorrendo rapidamente verso il basso e poi a destra.
2. Accedere al menu locale scorrendo rapidamente verso l'alto e poi a destra.
3. Scorrere a sinistra e a destra per passare tra le diverse opzioni.
4. Una volta selezionata l’opzione desiderata, fare doppio clic per scegliere tale opzione.

Per dettagli su tutte le opzioni disponibili nei menu contestuali globali e locali, veda [Use global and local context menus](https://support.google.com/accessibility/android/answer/6007066).

#### Navigare nelle pagine web

Può usare il menu contestuale locale mentre si trova in un browser web per trovare opzioni per navigare nelle pagine web usando solo le intestazioni, i controlli modulo, o i link, o navigare linea per linea, ecc.

Ad esempio, con TalkBack attivato:

1. Apra il suo browser web.
2. Attivi la barra degli URL.
3. Inserisca una pagina web che abbia un sacco di intestazioni, come la prima pagina di bbc.co.uk. Per inserire il testo dell'URL:

   - Selezioni la barra degli URL scorrendo a sinistra/destra fino a raggiungerla, e poi facendo doppio clic.
   - Tenga il dito premuto sulla tastiera virtuale finché non ottiene il carattere desiderato, e poi rilasci il dito per digitare. Ripeta per ogni carattere.
   - Una volta terminato, trovi il tasto Invio e lo prema.

4. Scorri a sinistra e destra per muoversi tra gli elementi della pagina.
5. Scorra verso l'alto e a destra con un movimento fluido per entrare nel menu dei contenuti locali.
6. Scorri a destra finché non trova l'opzione "Headings and Landmarks".
7. Faccia doppio clic per selezionarlo. Ora sarà in grado di scorrere a sinistra e a destra per muoversi tra le intestazioni e i landmark ARIA.
8. Per tornare alla modalità predefinita, entri di nuovo nel menu contestuale locale scorrendo verso l'alto e a destra, selezioni "Default", e poi faccia doppio clic per attivare.

> [!NOTE]
> Veda [Get started on Android with TalkBack](https://support.google.com/accessibility/android/answer/6283677?hl=en&ref_topic=3529932) per una documentazione più completa.

### iOS VoiceOver

Una versione mobile di VoiceOver è integrata nel sistema operativo iOS.

Per attivarlo, apra l'app _Impostazioni_ e selezioni _Accessibilità > VoiceOver_. Prema l'interruttore _VoiceOver_ per abilitarlo (vedrà anche molte altre opzioni relative a VoiceOver su questa pagina).

> [!NOTE]
> Alcuni dispositivi iOS più vecchi hanno il menu VoiceOver in _Impostazioni_ > _Generali_ > _Accessibilità_ > _VoiceOver_.

Una volta abilitato VoiceOver, i gesti di controllo di base di iOS saranno un po' diversi:

1. Un singolo tocco farà sì che l'elemento su cui tocca venga selezionato; il dispositivo parlerà l'elemento su cui ha toccato.
2. Può anche navigare tra gli elementi sulla schermata scorrendo a sinistra e a destra per muoversi tra di essi, oppure facendo scivolare il dito intorno allo schermo per muoversi tra diversi elementi (quando trova l'elemento che desidera, può sollevare il dito per selezionarlo).
3. Per attivare l'elemento selezionato (ad esempio, aprire un'app selezionata), faccia doppio clic ovunque sullo schermo.
4. Scorra con tre dita per scorrere una pagina.
5. Tocchi con due dita per eseguire un'azione contestualmente rilevante — ad esempio, scattare una foto mentre si trova nell'app della fotocamera.

Per spegnerlo, navighi di nuovo a _Impostazioni > Generali > Accessibilità > VoiceOver_ usando i gesti sopra, e disattivi l'interruttore _VoiceOver_.

#### Sbloccare il telefono

Per sbloccare il telefono, deve premere il pulsante home (o scorrere) come di consueto. Se ha impostato un codice, può selezionare ogni numero scorrendo/scivolando (come spiegato sopra) e poi fare doppio clic per inserire ogni numero quando ha trovato quello giusto.

#### Uso del Rotor

Quando VoiceOver è attivo, dispone di una caratteristica di navigazione chiamata Rotor che le consente di scegliere rapidamente tra una serie di opzioni utili comuni. Per usarlo:

1. Giri due dita sopra lo schermo come se stesse girando una manopola. Ogni opzione verrà letta ad alta voce man mano che gira ulteriormente. Può andare avanti e indietro per scorrere tra le opzioni.
2. Una volta trovata l'opzione desiderata:

   - Rilasci le dita per selezionarla.
   - Se è un'opzione di cui può iterare il valore (come Volume o Velocità di Lettura), può eseguire uno swipe verso l'alto o verso il basso per aumentare o diminuire il valore dell'elemento selezionato.

Le opzioni disponibili sotto il Rotor sono sensibili al contesto — differiranno a seconda dell'app o della visualizzazione in cui ci si trova (vedi sotto per un esempio).

#### Navigare nelle pagine web

Facciamo un giro nella navigazione web con VoiceOver:

1. Apra il suo browser web.
2. Attivi la barra degli URL.
3. Inserisca una pagina web che abbia un sacco di intestazioni, come la prima pagina di bbc.co.uk. Per inserire il testo dell'URL:

   - Selezioni la barra degli URL scorrendo a sinistra/destra finché non ci arriva, e poi faccia doppio clic.
   - Per ogni carattere, tenga il dito premuto sulla tastiera virtuale finché non ottiene il carattere desiderato, e poi rilasci il dito per selezionarlo. Faccia doppio clic per digitare.
   - Una volta terminato, trovi il tasto Invio e lo prema.

4. Scorra a sinistra e a destra per muoversi tra gli elementi della pagina. Può fare doppio clic su un elemento per selezionarlo (ad es., seguire un link).
5. Per impostazione predefinita, l'opzione Rotor selezionata sarà Velocità di Lettura; al momento può scorrere verso l'alto e verso il basso per aumentare o diminuire la velocità di lettura.
6. Giri ora due dita attorno allo schermo come un quadrante per mostrare il rotor e muoversi tra le sue opzioni. Ecco alcuni esempi delle opzioni disponibili:

   - _Velocità di Lettura_: Cambia la velocità di lettura.
   - _Contenitori_: Si muove tra diversi contenitori semantici sulla pagina.
   - _Intestazioni_: Muoversi tra le intestazioni sulla pagina.
   - _Link_: Muoversi tra i link sulla pagina.
   - _Controlli dei moduli_: Muoversi tra i controlli dei moduli sulla pagina.
   - _Lingua_: Si muove tra diverse traduzioni, se sono disponibili.

7. Selezioni _Intestazioni_. Ora sarà in grado di scorrere verso l'alto e verso il basso per muoversi tra le intestazioni sulla pagina.

> [!NOTE]
> Per un riferimento più completo che copre i gesti VoiceOver e altri suggerimenti sui test di accessibilità su iOS, veda [Apple's VoiceOver documentation](https://developer.apple.com/documentation/accessibility/voiceover/).

## Meccanismi di controllo

Nel nostro articolo sull'accessibilità CSS e JavaScript, abbiamo esaminato l'idea degli eventi specifici per un certo tipo di meccanismo di controllo (vedi [Mouse-specific events](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#mouse-specific_events)). Per ricapitolare, causano problemi di accessibilità perché altri meccanismi di controllo non possono attivare la funzionalità associata.

Come esempio, l'evento [click](/it/docs/Web/API/Element/click_event) è buono dal punto di vista dell'accessibilità — un gestore di eventi associato può essere invocato facendo clic sull'elemento su cui è impostato il gestore, selezionandolo tramite tab e premendo Invio/Return, o toccandolo su un dispositivo touch. Provi il nostro esempio [simple-button-example.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/simple-button-example.html) ([vedi l'esempio in esecuzione](https://mdn.github.io/learning-area/accessibility/mobile/simple-button-example.html)) per vedere cosa intendiamo.

In alternativa, eventi specifici del mouse come [mousedown](/it/docs/Web/API/Element/mousedown_event) e [mouseup](/it/docs/Web/API/Element/mouseup_event) creano problemi — i loro gestori di eventi non possono essere invocati usando controlli non basati su mouse.

Se cerca di controllare il nostro esempio [simple-box-drag.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/simple-box-drag.html) ([vedi esempio dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/simple-box-drag.html)) con una tastiera o touch, vedrà il problema. Questo accade perché stiamo usando codice come il seguente:

```js
div.onmousedown = () => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  movePanel();
};

document.onmouseup = stopMove;
```

Per abilitare altre forme di controllo, deve utilizzare eventi diversi, ma equivalenti — ad esempio, gli eventi touch funzionano sui dispositivi touch:

```js
div.ontouchstart = (e) => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  positionHandler(e);
  movePanel();
};

panel.ontouchend = stopMove;
```

Abbiamo fornito un semplice esempio che mostra come usare insieme eventi mouse e touch — veda [multi-control-box-drag.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/multi-control-box-drag.html) ([vedi anche l'esempio dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/multi-control-box-drag.html)).

> [!NOTE]
> Può anche vedere esempi completamente funzionali per implementare diversi meccanismi di controllo in [Implementing game control mechanisms](/it/docs/Games/Techniques/Control_mechanisms).

## Design responsive

Il [design responsive](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) è la pratica di modificare dinamicamente i layout e altre caratteristiche delle sue applicazioni in base a fattori come la dimensione e la risoluzione dello schermo, in modo che siano utilizzabili e accessibili agli utenti di diversi tipi di dispositivo.

In particolare, i problemi più comuni che devono essere affrontati per il mobile sono:

- Adeguatezza dei layout per i dispositivi mobili. Ad esempio, un layout a più colonne non funzionerà altrettanto bene su uno schermo stretto e la dimensione del testo potrebbe dover essere aumentata per essere leggibile. Tali problemi possono essere risolti creando un layout responsive utilizzando tecnologie come [media queries](/it/docs/Web/CSS/CSS_media_queries), [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element) e [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox).
- Conservazione delle dimensioni delle immagini scaricate. In generale, i dispositivi a schermo piccolo non avranno bisogno di immagini grandi quanto le loro controparti desktop e hanno maggiori probabilità di trovarsi su connessioni di rete lente. Pertanto, è consigliabile fornire immagini più piccole ai dispositivi con schermi stretti come appropriato. Può gestire questa situazione utilizzando le [tecniche di immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images).
- Pensare alle alte risoluzioni. Molti dispositivi mobili dispongono di schermi ad alta risoluzione e quindi necessitano di immagini ad alta risoluzione affinché il display continui a sembrare nitido e chiaro. Ancora una volta, può fornire immagini come appropriato utilizzando tecniche di immagini responsive. Inoltre, molte esigenze di immagine possono essere soddisfatte utilizzando il formato SVG per immagini vettoriali, che è ben supportato nei browser odierni. SVG ha una dimensione del file ridotta e manterrà la nitidezza indipendentemente dalla dimensione visualizzata (vedi [Includere grafica vettoriale in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML) per maggiori dettagli).

> [!NOTE]
> Non forniremo una discussione completa delle tecniche di design responsive qui, poiché sono trattate in altri punti su MDN (vedi i link sopra).

### Considerazioni specifiche per i dispositivi mobili

Ci sono altre questioni importanti da considerare quando si rende un sito più accessibile sui dispositivi mobili. Ne abbiamo elencati un paio qui, ma ne aggiungeremo altri quando ne troveremo.

#### Non disabilitare lo zoom

Utilizzando il [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element), è possibile disabilitare lo zoom. Assicurarsi sempre che il ridimensionamento sia abilitato e impostare la larghezza alla larghezza del dispositivo nel {{htmlelement("head")}}:

```html
<meta name="viewport" content="width=device-width; user-scalable=yes" />
```

Non dovrebbe mai impostare `user-scalable=no` se possibile — molte persone si affidano allo zoom per poter vedere il contenuto del suo sito web, quindi togliere questa funzionalità è una pessima idea. Ci sono certe situazioni in cui lo zoom potrebbe rompere l'interfaccia utente; in tali casi, se ritiene necessario disabilitare lo zoom, dovrebbe fornire qualche altro tipo di equivalenza, come un controllo per aumentare la dimensione del testo in modo da non rompere la sua UI.

#### Mantenere i menu accessibili

Poiché lo schermo è molto più stretto sui dispositivi mobili, è molto comune utilizzare media queries e altre tecnologie per far ridurre il menu di navigazione a una piccola icona nella parte superiore del display — che può essere premuta per rivelare il menu solo quando è necessario — quando il sito viene visualizzato sul mobile. Questo è comunemente rappresentato da un'icona "tre linee orizzontali" e il modello di design è quindi noto come "menu hamburger".

Quando implementa un tale menu, deve assicurarsi che il controllo per rivelarlo sia accessibile dai meccanismi di controllo appropriati (normalmente touch per il mobile), come discusso nei [Meccanismi di controllo](#meccanismi_di_controllo) sopra, e che il resto della pagina venga spostato o nascosto in qualche modo mentre il menu è accessibile, per evitare confusione durante la navigazione.

Faccia clic qui per un [buon esempio di menu hamburger](https://fritz-weisshart.de/meg_men/).

## Input utente

Sui dispositivi mobili, inserire dati tende ad essere più fastidioso per gli utenti rispetto all'esperienza equivalente sui computer desktop. È più conveniente digitare testo nei campi di input del modulo utilizzando una tastiera desktop o laptop rispetto a una tastiera virtuale touchscreen o una piccola tastiera fisica mobile.

Per questo motivo, vale la pena cercare di minimizzare la quantità di digitazione necessaria. Ad esempio, invece di far inserire agli utenti la loro professione ogni volta utilizzando un normale campo di testo, potrebbe offrire un menu {{htmlelement("select")}} contenente le opzioni più comuni (che aiuta anche con la coerenza nell'inserimento dati) e offrire un'opzione "Altro" che visualizza un campo di testo in cui inserire eventuali elementi anomali. Può vedere un semplice esempio di questa idea in azione in [common-job-types.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/common-job-types.html) (veda l' [esempio comune di lavori dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/common-job-types.html)).

Vale anche la pena considerare l'uso di tipi di input HTML come la data su piattaforme mobili poiché li gestiscono bene — sia Android che iOS, ad esempio, mostrano widget usabili che si adattano bene all'esperienza del dispositivo. Veda [html5-form-examples.html](https://github.com/mdn/learning-area/blob/main/accessibility/mobile/html5-form-examples.html) per alcuni esempi (veda gli [esempi di moduli HTML5 dal vivo](https://mdn.github.io/learning-area/accessibility/mobile/html5-form-examples.html)) — provi a caricarli e manipolarli su dispositivi mobili. Ad esempio:

- Tipi `number`, `tel` e `email` mostrano tastiere virtuali adatte per inserire numeri/telefoni.
- Tipi `time` e `date` mostrano selettori adatti per selezionare orari e date.

Se vuole fornire una soluzione diversa per desktop, può sempre servire un markup diverso ai suoi dispositivi mobili utilizzando il rilevamento delle funzionalità. Controlli il nostro [articolo sul rilevamento delle funzionalità](/it/docs/Learn_web_development/Extensions/Testing/Feature_detection) per maggiori informazioni.

## Sommario

In questo articolo, le abbiamo fornito alcuni dettagli sui problemi comuni specifici per l'accessibilità mobile e su come superarli. L'abbiamo anche guidata attraverso l'uso degli screen reader più comuni per aiutarla nei test di accessibilità.

## Veda anche

- [Guidelines For Mobile Web Development](https://www.smashingmagazine.com/2012/07/guidelines-for-mobile-web-development/) — Un elenco di articoli su _Smashing Magazine_ che coprono diverse tecniche per il design web mobile.
- [Make your site work on touch devices](https://www.creativebloq.com/javascript/make-your-site-work-touch-devices-51411644) — Articolo utile su come usare eventi touch per far funzionare le interazioni sui dispositivi mobili.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Multimedia","Learn_web_development/Core/Accessibility/Accessibility_troubleshooting", "Learn_web_development/Core/Accessibility")}}
