---
title: Strumenti per l'accessibilità e tecnologie assistive
short-title: Strumenti per l'accessibilità
slug: Learn_web_development/Core/Accessibility/Tooling
l10n:
  sourceCommit: 483ce811e1ea52cb2d9d2a5af0c4d1c4d591ea4a
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}

Ora l'attenzione si sposta sugli strumenti per l'accessibilità, fornendo informazioni sui tipi di strumenti utilizzabili per aiutare a risolvere i problemi di accessibilità e aiutando a comprendere le **tecnologie assistive** usate dalle persone con disabilità per navigare il web. Gli strumenti descritti qui verranno usati negli articoli successivi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">conoscenza di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Familiarità con il tipo di strumenti utilizzabili per aiutare a risolvere i problemi di accessibilità, ad esempio gli strumenti di audit.</li>
          <li>Configurazione degli screen reader e loro utilizzo per testare siti web su desktop e dispositivi mobili.</li>
          <li>Familiarità con altri tipi di tecnologie assistive, quali tastiere a caratteri grandi o braille, dispositivi di puntamento alternativi e ingranditori dello schermo.</li>
          <li>L'importanza dei test con gli utenti insieme ai test automatizzati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti per l'accessibilità

Vediamo gli strumenti e le tecniche utilizzabili per testare l'accessibilità dei siti web e risolvere i problemi individuati.

### Test dell'ordine sorgente

Il contenuto dovrebbe avere senso logico nel suo ordine sorgente: in seguito sarà sempre possibile visualizzarlo diversamente con CSS, ma è opportuno impostare correttamente la struttura sottostante fin dall'inizio. Questo perché le tecnologie assistive leggono il contenuto dei siti web in base all'ordine del sorgente e le persone con disabilità spesso modificano o disattivano parti del CSS per rendere il contenuto più leggibile (esempi comuni sono l'aumento della dimensione del carattere e l'applicazione di schemi di colori ad alto contrasto).

Per testare l'ordine sorgente, è possibile disattivare il CSS di un sito e verificare quanto sia comprensibile senza di esso. Si potrebbe farlo manualmente semplicemente rimuovendo il CSS dal codice, ma il modo più semplice consiste nell'usare le funzionalità del browser, ad esempio:

- Firefox: selezionare _Visualizza > Stile pagina > Nessuno stile_ dal menu principale.
- Safari: [aprire gli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser), fare clic sul pulsante _Device Settings_ vicino all'angolo superiore sinistro del pannello degli strumenti di sviluppo (assomiglia a un monitor), quindi selezionare la casella di controllo "Disable CSS" nel pannello visualizzato.
- Chrome/Edge: installare l'estensione [Web Developer Toolbar](https://chromewebstore.google.com/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm), quindi riavviare il browser. Fare clic sull'icona a ingranaggio "Web Developer", che ora dovrebbe essere disponibile nel menu delle estensioni, quindi selezionare _CSS > Disable All Styles_.

### Verificatori del contrasto di colore

Quando si sceglie uno schema di colori per un sito web, occorre assicurarsi che il colore del testo (primo piano) contrasti bene con il colore dello sfondo. Un design può apparire interessante, ma non serve a nulla se le persone non riescono a leggere il contenuto. Usare uno strumento come [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM per verificare se lo schema presenta un contrasto sufficiente.

Un altro suggerimento consiste nell'evitare di usare esclusivamente il colore per indicazioni o per evidenziare informazioni importanti, poiché potrebbe non essere percepito da persone con disabilità visive come il daltonismo. Invece di contrassegnare in rosso i campi obbligatori di un modulo, ad esempio, contrassegnarli con un asterisco e in rosso.

> [!NOTE]
> Un elevato rapporto di contrasto consente inoltre a chiunque usi uno smartphone o un tablet con schermo lucido di leggere meglio le pagine in un ambiente luminoso, ad esempio sotto la luce del sole.

### Strumenti di audit

Sono disponibili diversi strumenti di audit a cui è possibile fornire pagine web. Essi le analizzano e restituiscono un elenco dei problemi di accessibilità presenti nella pagina. Consideriamo [Wave](https://wave.webaim.org/) come esempio: uno strumento online per il test dell'accessibilità che accetta un indirizzo web e restituisce una visualizzazione annotata di quella pagina con i problemi di accessibilità evidenziati.

1. Andare alla [pagina iniziale di Wave](https://wave.webaim.org/).
2. Inserire l'URL del nostro esempio [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html) nella casella di input del testo vicino alla parte superiore della pagina. Quindi premere Invio o fare clic/toccare la freccia sul bordo destro della casella di input.
3. Il sito dovrebbe evidenziare i problemi di accessibilità presenti. Fare clic sulle icone visualizzate per ottenere maggiori informazioni su ciascuno dei problemi identificati dalla valutazione di Wave.

Altri strumenti di audit che vale la pena esaminare:

- [Firefox Accessibility Inspector](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html)
- [ANDI bookmarklet](https://www.ssa.gov/accessibility/andi/help/install.html)
- [Audit di accessibilità Google Lighthouse](https://developer.chrome.com/docs/lighthouse/accessibility/scoring)

> [!NOTE]
> Questi strumenti non sono sufficienti, da soli, a risolvere tutti i problemi di accessibilità. Per ottenere un quadro completo servirà una combinazione di questi strumenti, conoscenze ed esperienza, test con gli utenti e così via.

Lo [strumento aXe di Deque](https://www.deque.com/axe/) va un po' oltre gli strumenti di audit menzionati sopra. Come gli altri, controlla le pagine e restituisce errori di accessibilità. La sua forma più immediatamente utile è probabilmente rappresentata dalle estensioni del browser:

- [aXe per Chrome](https://chromewebstore.google.com/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd)
- [aXe per Firefox](https://addons.mozilla.org/en-US/firefox/addon/axe-devtools/)

Queste aggiungono una scheda per l'accessibilità agli strumenti di sviluppo del browser. Ad esempio, abbiamo installato la versione per Firefox e poi l'abbiamo usata per eseguire un audit del nostro esempio [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html). Abbiamo ottenuto i risultati seguenti:

![Schermata dei problemi di accessibilità identificati dallo strumento Axe.](axe-screenshot.png)

aXe può anche essere installato tramite `npm` e può essere integrato con task runner come [Grunt](https://gruntjs.com/) e [Gulp](https://gulpjs.com/), framework di automazione come [Selenium](https://www.selenium.dev/) e [Cucumber](https://cucumber.io/), framework per test unitari come [Jasmine](https://jasmine.github.io/) e altro ancora (di nuovo, consultare la [pagina principale di aXe](https://www.deque.com/axe/) per i dettagli).

## Screen reader

Uno dei tipi più comuni di tecnologie assistive (AT) utilizzati dalle persone con disabilità — e uno di quelli che verrà usato più spesso per testare l'accessibilità delle pagine web — è costituito dagli **screen reader**. Si tratta di software che leggono ad alta voce il contenuto delle pagine web o il contenuto di altre app installate sul sistema operativo di una persona. Gli screen reader consentono alle persone di usare i computer senza dover vedere alcun contenuto visivo.

I browser web espongono informazioni sul contenuto della pagina affinché gli screen reader (e altre AT) possano comunicarle all'utente attraverso una rappresentazione chiamata {{Glossary("Accessibility_tree", "albero dell'accessibilità")}}. Questo fornisce informazioni semantiche quali nomi e descrizioni degli elementi, il loro scopo o ruolo (è un pulsante o un campo di input?) e se si trovano in uno stato particolare (ad esempio, una finestra di dialogo è aperta o chiusa?).

Queste informazioni possono essere banali nel caso di un paragrafo di testo, che suona più o meno come è scritto, ma possono diventare complesse quando si tratta di funzionalità dell'interfaccia utente come un menu a discesa o un lettore video. Questo è il motivo per cui è molto importante usare correttamente HTML semantico, che verrà esaminato in dettaglio nel prossimo articolo di questo modulo. Contrassegnare il contenuto usando l'elemento sbagliato può confondere gli utenti di screen reader.

Assicurarsi di avere installato uno o due screen reader sul computer di sviluppo e provare a usare i siti web preferiti tramite uno screen reader, come descritto di seguito. Comprendere come le persone con disabilità visive usano il web è fondamentale per progettare prodotti che funzionano meglio per tutti.

### Quali screen reader sono disponibili?

Sono disponibili diversi screen reader:

- Alcuni sono prodotti commerciali a pagamento, come [JAWS](https://vispero.com/jaws-screen-reader-software/) (Windows).
- Alcuni sono prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome, Windows e macOS) e [Orca](https://wiki.gnome.org/Projects/Orca) (Linux).
- Alcuni sono integrati nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/features/?vision) (macOS e iOS), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (sui Chromebook) e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

In generale, gli screen reader sono app separate eseguite sul sistema operativo host e possono leggere anche pagine web e contenuti in altre app (non sempre è così; ChromeVox, ad esempio, è un'estensione del browser). Gli screen reader tendono a presentare alcune differenze nel comportamento e nei controlli specifici, perciò sarà necessario consultare la documentazione dello screen reader scelto per tutti i dettagli. Detto questo, funzionano tutti sostanzialmente nello stesso modo.

Nelle prossime sezioni verranno eseguiti alcuni test con due screen reader diversi per fornire un'idea generale di come funzionano e di come eseguire i test con essi.

> [!NOTE]
> [Designing for Screen Reader Compatibility](https://webaim.org/techniques/screenreader/) di WebAIM fornisce alcune informazioni utili sull'uso degli screen reader e su ciò che funziona meglio per essi. Consultare anche [Screen Reader User Survey #10 Results](https://webaim.org/projects/screenreadersurvey10/#used) per alcune interessanti statistiche sull'uso degli screen reader.

#### VoiceOver

VoiceOver (VO) è incluso gratuitamente con Apple mac/iPhone/iPad, quindi è utile per i test su desktop e dispositivi mobili quando si usano prodotti Apple. È stato testato su macOS su un MacBook Pro.

Per attivarlo, premere <kbd>Cmd</kbd> + <kbd>F5</kbd>. Se VO non è stato usato in precedenza, verrà visualizzata una schermata di benvenuto in cui è possibile scegliere se avviare VO o meno e seguire un tutorial piuttosto utile per imparare a usarlo. Per disattivarlo, premere nuovamente <kbd>Cmd</kbd> + <kbd>F5</kbd>.

> [!NOTE]
> È consigliabile seguire il tutorial almeno una volta: è un modo davvero utile per imparare VO.

Quando VO è attivo, il display apparirà perlopiù uguale, ma nell'angolo inferiore sinistro dello schermo verrà visualizzato un riquadro nero contenente informazioni su ciò che VO ha attualmente selezionato. Anche la selezione corrente verrà evidenziata con un bordo nero: questa evidenziazione è nota come **cursore VO**.

![Schermata di esempio che mostra il test dell'accessibilità con VoiceOver sulla homepage di MDN. Nella parte inferiore sinistra dell'immagine è evidenziata l'informazione selezionata nella pagina web.](voiceover.png)

Per usare VO, sarà necessario fare ampio uso del "modificatore VO": si tratta di un tasto o di una combinazione di tasti da premere insieme alle effettive scorciatoie da tastiera di VO per farle funzionare. L'uso di un modificatore di questo tipo è comune con gli screen reader, per impedire che i loro comandi entrino in conflitto con altri comandi. Nel caso di VO, il modificatore può essere <kbd>CapsLock</kbd> oppure <kbd>Ctrl</kbd> + <kbd>Option</kbd>.

VO dispone di molti comandi da tastiera e non verranno elencati tutti qui. Quelli di base necessari per testare le pagine web sono nella tabella seguente. Nelle scorciatoie da tastiera, "VO" significa "il modificatore VoiceOver".

<table class="standard-table no-markdown">
  <caption>
    Scorciatoie da tastiera comuni di VoiceOver
  </caption>
  <thead>
    <tr>
      <th scope="col">Scorciatoia da tastiera</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VO + tasti cursore</td>
      <td>Sposta il cursore VO in alto, a destra, in basso, a sinistra.</td>
    </tr>
    <tr>
      <td>VO + barra spaziatrice</td>
      <td>
        Seleziona/attiva gli elementi evidenziati dal cursore VO. Include gli elementi
        selezionati nel Rotor (vedere sotto).
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + cursore giù</td>
      <td>
        Entra in un gruppo di elementi, ad esempio una tabella o un modulo HTML. Una volta
        all'interno di un gruppo, è possibile spostarsi e selezionare elementi nel gruppo
        usando normalmente i comandi precedenti.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + cursore su</td>
      <td>Esce da un gruppo.</td>
    </tr>
    <tr>
      <td>VO + <kbd>C</kbd></td>
      <td>(all'interno di una tabella) Legge l'intestazione della colonna corrente.</td>
    </tr>
    <tr>
      <td>VO + <kbd>R</kbd></td>
      <td>(all'interno di una tabella) Legge l'intestazione della riga corrente.</td>
    </tr>
    <tr>
      <td>VO + <kbd>C</kbd> + <kbd>C</kbd> (due C in successione)</td>
      <td>
        (all'interno di una tabella) Legge l'intera colonna corrente, inclusa l'intestazione.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>R</kbd> + <kbd>R</kbd> (due R in successione)</td>
      <td>
        (all'interno di una tabella) Legge l'intera riga corrente, incluse le intestazioni
        corrispondenti a ciascuna cella.
      </td>
    </tr>
    <tr>
      <td>VO + cursore sinistro, VO + cursore destro</td>
      <td>
        (all'interno di alcune opzioni orizzontali, come un selettore di data)
        Si sposta tra le opzioni.
      </td>
    </tr>
    <tr>
      <td>VO + cursore su, VO + cursore giù</td>
      <td>
        (all'interno di alcune opzioni orizzontali, come un selettore di data)
        Modifica l'opzione corrente.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>U</kbd></td>
      <td>
        Apre il Rotor, che visualizza elenchi di intestazioni, link, controlli dei moduli,
        ecc. per una navigazione semplice.
      </td>
    </tr>
    <tr>
      <td>VO + cursore sinistro, VO + cursore destro</td>
      <td>
        (all'interno del Rotor) Si sposta tra i diversi elenchi disponibili nel Rotor.
      </td>
    </tr>
    <tr>
      <td>VO + cursore su, VO + cursore giù</td>
      <td>
        (all'interno del Rotor) Si sposta tra i diversi elementi nell'elenco corrente
        del Rotor.
      </td>
    </tr>
    <tr>
      <td><kbd>Esc</kbd></td>
      <td>(all'interno del Rotor) Esce dal Rotor.</td>
    </tr>
    <tr>
      <td><kbd>Ctrl</kbd></td>
      <td>(quando VO parla) Mette in pausa/riprende la voce.</td>
    </tr>
    <tr>
      <td>VO + <kbd>Z</kbd></td>
      <td>Ripete l'ultima parte pronunciata.</td>
    </tr>
    <tr>
      <td>VO + <kbd>D</kbd></td>
      <td>Accede al Dock del mac, per poter selezionare le app da eseguire al suo interno.</td>
    </tr>
  </tbody>
</table>

Questi comandi possono sembrare molti, ma non sono così difficili una volta acquisita familiarità, e VO fornisce regolarmente promemoria sui comandi da usare in determinate situazioni. Provare ora VO; quindi sarà possibile provare alcuni degli esempi nella sezione [Test con screen reader](#test_con_screen_reader).

#### NVDA

NVDA è disponibile solo per Windows e deve essere installato.

1. Scaricare NVDA da [nvaccess.org](https://www.nvaccess.org/), quindi installarlo. È possibile scegliere se effettuare una donazione o scaricarlo gratuitamente; sarà inoltre necessario fornire il proprio indirizzo email prima di poterlo scaricare.
2. Per avviare NVDA dopo l'installazione, fare doppio clic sul file/scorciatoia del programma oppure usare la scorciatoia da tastiera <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>N</kbd>. All'avvio verrà visualizzata la finestra di dialogo di benvenuto di NVDA. Qui è possibile scegliere tra alcune opzioni, quindi premere il pulsante _OK_ per iniziare.

NVDA sarà ora attivo sul computer.

Per usare NVDA, sarà necessario fare ampio uso del "modificatore NVDA": il tasto da premere insieme alle effettive scorciatoie da tastiera di NVDA per farle funzionare. Il modificatore NVDA può essere <kbd>Insert</kbd> (l'impostazione predefinita) oppure <kbd>CapsLock</kbd> (può essere scelto selezionando la prima casella di controllo nella finestra di dialogo di benvenuto di NVDA prima di premere _OK_).

> [!NOTE]
> NVDA è più discreto di VoiceOver nel modo in cui evidenzia dove si trova e cosa sta facendo. Quando si scorrono intestazioni, elenchi e così via, gli elementi selezionati saranno generalmente evidenziati con un contorno discreto, ma questo non avviene sempre per ogni elemento. Se ci si perde completamente, è possibile premere Ctrl + F5 per aggiornare la pagina corrente e ricominciare dall'alto.

NVDA dispone di molti comandi da tastiera e non verranno elencati tutti qui. Quelli di base necessari per testare le pagine web sono nella tabella seguente. Nelle scorciatoie da tastiera, "NVDA" significa "il modificatore NVDA".

<table class="standard-table no-markdown">
  <caption>
    Scorciatoie da tastiera NVDA più comuni
  </caption>
  <thead>
    <tr>
      <th scope="col">Scorciatoia da tastiera</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>NVDA + <kbd>Q</kbd></td>
      <td>Disattiva nuovamente NVDA dopo averlo avviato.</td>
    </tr>
    <tr>
      <td>NVDA + cursore su</td>
      <td>Legge la riga corrente.</td>
    </tr>
    <tr>
      <td>NVDA + cursore giù</td>
      <td>Inizia a leggere dalla posizione corrente.</td>
    </tr>
    <tr>
      <td>Cursore su e cursore giù, oppure <kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>Si sposta all'elemento precedente/successivo nella pagina e lo legge.</td>
    </tr>
    <tr>
      <td>Cursore sinistro e cursore destro</td>
      <td>Si sposta al carattere precedente/successivo nell'elemento corrente e lo legge.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>H</kbd> e <kbd>H</kbd></td>
      <td>Si sposta all'intestazione precedente/successiva e la legge.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>K</kbd> e <kbd>K</kbd></td>
      <td>Si sposta al link precedente/successivo e lo legge.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>D</kbd> e <kbd>D</kbd></td>
      <td>
        Si sposta al landmark del documento precedente/successivo (ad esempio, <code>&#x3C;nav></code>)
        e lo legge.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>1</kbd>–<kbd>6</kbd> e <kbd>1</kbd>–<kbd>6</kbd></td>
      <td>Si sposta all'intestazione precedente/successiva (livello 1–6) e la legge.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>F</kbd> e <kbd>F</kbd></td>
      <td>Si sposta all'input del modulo precedente/successivo e gli assegna il focus.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>T</kbd> e <kbd>T</kbd></td>
      <td>Si sposta alla tabella di dati precedente/successiva e le assegna il focus.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>B</kbd> e <kbd>B</kbd></td>
      <td>Si sposta al pulsante precedente/successivo e ne legge l'etichetta.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>L</kbd> e <kbd>L</kbd></td>
      <td>Si sposta all'elenco precedente/successivo e ne legge il primo elemento.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>I</kbd> e <kbd>I</kbd></td>
      <td>Si sposta all'elemento dell'elenco precedente/successivo e lo legge.</td>
    </tr>
    <tr>
      <td><kbd>Enter</kbd>/<kbd>Return</kbd></td>
      <td>
        (quando è selezionato un link/pulsante o altro elemento attivabile) Attiva l'elemento.
      </td>
    </tr>
    <tr>
      <td>NVDA + <kbd>Spacebar</kbd></td>
      <td>
        (quando è selezionato un modulo) Entra nel modulo affinché sia possibile selezionare
        i singoli elementi, oppure esce dal modulo se ci si trova già al suo interno.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>(all'interno di un modulo) Si sposta tra gli input del modulo.</td>
    </tr>
    <tr>
      <td>Cursore su e cursore giù</td>
      <td>
        (all'interno di un modulo) Modifica i valori degli input del modulo (nel caso di controlli
        come le caselle select).
      </td>
    </tr>
    <tr>
      <td><kbd>Spacebar</kbd></td>
      <td>(all'interno di un modulo) Seleziona il valore scelto.</td>
    </tr>
    <tr>
      <td><kbd>Ctrl</kbd> + <kbd>Alt</kbd> + tasti cursore</td>
      <td>(quando è selezionata una tabella) Si sposta tra le celle della tabella.</td>
    </tr>
  </tbody>
</table>

### Test con screen reader

Ora che è stata acquisita familiarità con l'uso di uno screen reader, usarlo per eseguire alcuni rapidi test di accessibilità, così da farsi un'idea di come gli screen reader gestiscono funzionalità di pagine web corrette e non corrette:

- Osservare [good-semantics.html](https://mdn.github.io/learning-area/accessibility/html/good-semantics.html) e notare come le intestazioni vengono individuate dallo screen reader e sono disponibili per la navigazione. Ora osservare [bad-semantics.html](https://mdn.github.io/learning-area/accessibility/html/bad-semantics.html) e notare come lo screen reader non riceve alcuna di queste informazioni. Immaginare quanto sarebbe fastidioso durante la navigazione in una pagina di testo molto lunga.
- Osservare [good-links.html](https://mdn.github.io/learning-area/accessibility/html/good-links.html) e notare come i link abbiano senso quando vengono visualizzati fuori contesto, ad esempio nel Rotor di VoiceOver. Questo non accade con [bad-links.html](https://mdn.github.io/learning-area/accessibility/html/bad-links.html): sono tutti semplicemente "fai clic qui".
- Osservare [good-form.html](https://mdn.github.io/learning-area/accessibility/html/good-form.html) e notare come gli input del modulo vengono descritti usando le loro etichette perché sono stati aggiunti elementi {{htmlelement("label")}} appropriati. In [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html), ricevono un'etichetta poco utile come "vuoto".
- Osservare il nostro esempio [punk-bands-complete.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html) e verificare come lo screen reader riesca ad associare colonne e righe di contenuto e a leggerle tutte insieme perché le intestazioni della tabella sono state definite correttamente. In [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html), nessuna delle celle può essere associata. Notare che NVDA sembra comportarsi in modo leggermente strano quando è presente una sola tabella in una pagina; si potrebbe invece provare la [pagina di test delle tabelle di WebAIM](https://webaim.org/articles/nvda/tables.htm).

## Altri strumenti

Gli screen reader sono uno dei tipi più comuni di tecnologie assistive che si incontreranno come sviluppatori web, ma esistono altri tipi di AT ed è utile conoscere ciò che gli utenti potrebbero usare per accedere ai contenuti. Questa sezione ne riassume alcuni.

### Tastiere a caratteri grandi o braille

È possibile ottenere tastiere a caratteri grandi progettate per l'uso da parte di utenti ipovedenti o anziani e tastiere braille progettate per essere usabili da persone cieche e gravemente ipovedenti.

### Dispositivi di puntamento alternativi

Quando si pensa ai dispositivi di puntamento, l'esempio più ovvio è il mouse, ma esistono altri dispositivi di puntamento progettati per consentire agli utenti con diverse disabilità motorie di navigare più facilmente nelle interfacce utente:

- Trackball: simili a mouse capovolti, le trackball sono costituite da una sfera montata che rimane ferma sulla scrivania e che può essere fatta rotolare per spostare il puntatore. Sono considerate più precise e più facili da gestire dei mouse, soprattutto per le persone con movimenti limitati delle mani.
- Joystick: una leva di controllo che può essere spostata per muovere il puntatore. I joystick sono meno precisi delle trackball, ma utilizzabili da persone con un'ampia gamma di disabilità fisiche, incluse disabilità gravi.
- Touchpad: la maggior parte dei laptop moderni dispone di un touchpad (talvolta chiamato trackpad), un sensore tattile piatto che permette di spostare il puntatore con un dito, oltre a eseguire gesti con più dita analogamente ai gesti sui dispositivi mobili. È possibile acquistare touchpad esterni per dispositivi che non ne hanno uno interno. Alcune persone li trovano più precisi dei mouse.

### Ingranditori dello schermo

Gli ingranditori dello schermo forniscono agli utenti ipovedenti una visualizzazione ingrandita del display del proprio dispositivo, per consentire loro di comprendere e interagire più facilmente con il contenuto del dispositivo, oltre a offrire altre funzionalità come la regolazione del colore per aiutare in caso di daltonismo e la regolazione delle dimensioni dei puntatori del mouse e dei cursori del testo per renderli più facili da vedere.

Sono disponibili ingranditori dello schermo software e hardware:

- La maggior parte dei sistemi operativi moderni dispone di un'app integrata per ingrandire tutto o parte dello schermo, ad esempio Zoom su mac o Magnifier su Windows. Tendono inoltre a offrire opzioni per aumentare universalmente la dimensione del testo, del cursore del mouse e così via. Sono disponibili anche opzioni di terze parti.
- Gli ingranditori dello schermo hardware tendono a consistere in uno schermo separato collocato accanto o davanti allo schermo del dispositivo, che proietta una versione più grande dello stesso oppure una versione ingrandita di una sua parte.

### Software di riconoscimento vocale

Il software di riconoscimento vocale consente di pronunciare comandi per controllare il dispositivo e/o dettare il testo di email o documenti affinché il computer scriva il testo. Questo è molto utile per le persone che non sono in grado di usare una tastiera o altri meccanismi di controllo.

I sistemi operativi moderni dispongono di funzionalità integrate per consentire questo utilizzo, ad esempio Dictation su mac oppure Voice Access su Windows; sono disponibili anche app di terze parti, dalle app desktop alle estensioni del browser.

### Controlli a interruttore

I controlli a interruttore forniscono un meccanismo per interagire con i dispositivi agli utenti con mobilità molto limitata o [disabilità cognitive](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility).

Una configurazione di controllo a interruttore comporta solitamente due parti:

- Un interruttore o pulsante fisico per attivare le opzioni sul dispositivo. È inoltre possibile assegnare la funzionalità di interruttore ai normali pulsanti del dispositivo, come i controlli del volume, o ai tasti di una tastiera.
- Una modalità del dispositivo o un componente aggiuntivo software di terze parti che rende il dispositivo compatibile con il controllo tramite interruttore o pulsante. Ad esempio, Switch Access su Android è una modalità in cui vengono scorse le diverse opzioni in varie situazioni, come le app nella schermata iniziale, ed è poi possibile selezionare quella desiderata con un pulsante o un interruttore quando viene raggiunta.

## Pianificare l'accessibilità

È opportuno riflettere attentamente sull'accessibilità fin dall'inizio di ogni progetto. Assicurarsi che l'accessibilità sia considerata durante la fase iniziale di progettazione, in modo da poter:

- Impostare correttamente le basi, ad esempio usando una [buona struttura del documento](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_well-structured_text_content) e fornendo [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) per le immagini.
- Considerare attentamente l'approccio migliore per le funzionalità che probabilmente presenteranno problemi di accessibilità. Ad esempio, audio e video saranno sicuramente inaccessibili per alcune persone, quindi occorre fornire alternative quali [trascrizioni](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#audio_transcripts) e [tracce di testo](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#video_text_tracks).
- Evitare errori costosi in seguito. I problemi individuati verso la fine di un progetto tendono a richiedere molto più tempo e denaro per essere risolti rispetto ai problemi rilevati nelle fasi iniziali.

## Test con gli utenti

Non è possibile affidarsi esclusivamente agli strumenti automatizzati per determinare i problemi di accessibilità di un sito. Ogni progetto web necessita di una [strategia di test con gli utenti](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies#user_testing) ed è fortemente consigliato includere alcuni gruppi di utenti con esigenze di accessibilità:

- Cercare di coinvolgere alcuni utenti di screen reader, alcuni utenti che usano solo la tastiera, alcuni utenti non udenti, utenti con disabilità motorie e così via.
- Chiedere a ciascun gruppo di provare a usare il sito web in generale, iniziando dalla homepage e da altre pagine principali e provando alcune delle funzionalità primarie. Esempi tipici includono l'acquisto di un prodotto o l'effettuazione di una prenotazione. Chiedere quali sono state le loro impressioni e quali problemi hanno incontrato.
- Successivamente, chiedere loro di concentrarsi sulle funzionalità o sui flussi di lavoro per cui esistono specifiche preoccupazioni in materia di accessibilità, come controlli complessi dei moduli o lettori video. Chiedere cosa manca in termini di esperienza utente e cosa vorrebbero fosse modificato.

Alcuni progetti avranno un budget per retribuire i gruppi di test, mentre altri si affideranno a volontari non retribuiti o persino a colleghi e amici.

## Lista di controllo per il test dell'accessibilità

L'elenco seguente fornisce una lista di controllo da seguire per assicurarsi di aver eseguito i test di accessibilità consigliati per il progetto:

1. Assicurarsi che HTML sia semanticamente corretto quanto più possibile. [Convalidarlo](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML#html_validation) è un buon inizio, così come usare uno [strumento di audit](#strumenti_di_audit).
2. Verificare che il contenuto abbia senso quando il CSS è disattivato.
3. Assicurarsi che le funzionalità siano accessibili da tastiera (consultare [Usare controlli UI semantici quando possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) per maggiori dettagli). Eseguire i test usando Tab, Return/Enter e così via.
4. Assicurarsi che il contenuto non testuale disponga di [alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives). Uno [strumento di audit](#strumenti_di_audit) è utile per rilevare problemi di questo tipo.
5. Assicurarsi che il [contrasto di colore](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#color_and_color_contrast) del sito sia accettabile, usando uno strumento di verifica adeguato.
6. Assicurarsi che il [contenuto nascosto](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#hiding_things) sia visibile agli screen reader.
7. Assicurarsi che le funzionalità siano utilizzabili senza JavaScript ovunque possibile.
8. Usare ARIA per migliorare l'accessibilità ove appropriato.
9. Eseguire il sito tramite uno [strumento di audit](#strumenti_di_audit).
10. Testarlo con uno screen reader.
11. Includere in un punto individuabile del sito una politica/dichiarazione di accessibilità che indichi cosa è stato fatto.

## Riepilogo

Si spera che questo articolo abbia fornito un'idea dei tipi di strumenti utilizzabili per aiutare a risolvere i problemi di accessibilità e delle tecnologie assistive usate dalle persone con disabilità per accedere al web.

Nel prossimo articolo verrà esaminato come scrivere HTML accessibile.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}
