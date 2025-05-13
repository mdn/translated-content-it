---
title: Strumenti per l'accessibilità e tecnologia assistiva
short-title: Strumenti di accessibilità
slug: Learn_web_development/Core/Accessibility/Tooling
l10n:
  sourceCommit: a6ccdff81d675a5271a670f1e416e79bc546f45e
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_Accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}

Successivamente, rivolgeremo la nostra attenzione agli strumenti di accessibilità, fornendo informazioni sui tipi di strumenti che può utilizzare per aiutare a risolvere i problemi di accessibilità e aiutando a comprendere le **tecnologie assistive** utilizzate dalle persone con disabilità per aiutarle a navigare sul web. Utilizzerà gli strumenti descritti qui negli articoli successivi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Familiarità con il tipo di strumenti che può utilizzare per aiutare a risolvere i problemi di accessibilità, ad esempio strumenti di audit.</li>
          <li>Configurare lettori di schermo e utilizzarli per testare siti web su desktop e mobile.</li>
          <li>Familiarità con altri tipi di tecnologie assistive come tastiere in braille o con testo grande, dispositivi di puntamento alternativi e ingranditori di schermo.</li>
          <li>L'importanza dei test utente insieme ai test automatizzati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti di accessibilità

Esaminiamo gli strumenti e le tecniche che è possibile utilizzare per testare l'accessibilità di un sito web e correggere i problemi che si scoprono.

### Test dell'ordine del sorgente

I contenuti devono avere un senso logico nel loro ordine di sorgente: può sempre visualizzarli diversamente con CSS in seguito, ma dovrebbe ottenere la struttura sottostante corretta fin dall'inizio. Questo perché le tecnologie assistive leggono i contenuti del sito web in base all'ordine del sorgente e le persone con disabilità spesso modificano o disattivano parti di CSS per rendere i contenuti più leggibili (esempi comuni sono aumentare la dimensione del carattere e applicare schemi di colori ad alto contrasto).

Per testare l'ordine del sorgente, può disattivare il CSS di un sito e vedere quanto è comprensibile senza di esso. Potrebbe farlo manualmente rimuovendo semplicemente il CSS dal suo codice, ma il modo più semplice è utilizzare le funzionalità del browser, ad esempio:

- Firefox: selezioni _Visualizza > Stile Pagina > Nessuno Stile_ dal menu principale.
- Safari: [apra gli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser), clicchi il pulsante _Impostazioni Dispositivo_ vicino all'angolo in alto a sinistra del pannello degli strumenti per sviluppatori (sembra un monitor di computer), quindi selezioni la casella di controllo "Disabilita CSS" nel pannello che appare.
- Chrome/Edge: installi l'estensione [Web Developer Toolbar](https://chromewebstore.google.com/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm), poi riavvii il browser. Clicchi l'icona a forma di ingranaggio "Web Developer" che dovrebbe ora essere disponibile nel menu delle estensioni, poi selezioni _CSS > Disable All Styles_.

### Controllori di contrasto dei colori

Quando sceglie uno schema di colori per il suo sito web, dovrebbe assicurarsi che il colore del testo (in primo piano) contrasti bene con il colore di sfondo. Il suo design potrebbe essere bello, ma non serve a nulla se le persone non riescono a leggere il suo contenuto. Utilizzi uno strumento come il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM per verificare se il suo schema cromatico è sufficientemente a contrasto.

Un altro consiglio è di evitare di usare solo il colore per indicazioni o per evidenziare informazioni importanti, poiché ciò potrebbe essere ignorato dalle persone con disabilità visive come il daltonismo. Invece di contrassegnare i campi obbligatori di un modulo in rosso, ad esempio, li contrassegni con un asterisco e in rosso.

> [!NOTE]
> Un rapporto di contrasto elevato consentirà anche a chi utilizza uno smartphone o un tablet con schermo lucido di leggere meglio le pagine in un ambiente luminoso, come alla luce del sole.

### Strumenti di audit

Sono disponibili diversi strumenti di audit nei quali può inserire le sue pagine web. Li esamineranno e restituiranno un elenco di problemi di accessibilità presenti sulla pagina. Esaminiamo [Wave](https://wave.webaim.org/) come esempio, uno strumento di testing online per l'accessibilità che accetta un indirizzo web e restituisce una visualizzazione annotata di quella pagina con i problemi di accessibilità evidenziati.

1. Andare alla [pagina principale di Wave](https://wave.webaim.org/).
2. Inserire l'URL del nostro esempio [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html) nella casella di testo vicino alla parte superiore della pagina. Poi premi invio o clic/tap la freccia all'estremità destra della casella di input.
3. Il sito dovrebbe evidenziare i problemi di accessibilità presenti. Cliccare sulle icone visualizzate per vedere ulteriori informazioni su ciascuno dei problemi identificati dalla valutazione di Wave.

Altri strumenti di audit che vale la pena controllare:

- [Inspector di accessibilità di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html)
- [ANDI bookmarklet](https://www.ssa.gov/accessibility/andi/help/install.html)
- [Auditing di accessibilità di Google Lighthouse](https://developer.chrome.com/docs/lighthouse/accessibility/scoring)

> [!NOTE]
> Tali strumenti non sono sufficienti per risolvere tutti i suoi problemi di accessibilità da soli. Avrà bisogno una combinazione di questi, conoscenza ed esperienza, test utente, ecc., per ottenere un quadro completo.

Lo strumento [aXe di Deque](https://www.deque.com/axe/) va un po' più in là degli strumenti di audit che abbiamo menzionato sopra. Come gli altri, controlla le pagine e restituisce errori di accessibilità. La sua forma probabilmente più immediatamente utile è probabilmente l'estensione del browser:

- [aXe per Chrome](https://chromewebstore.google.com/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd)
- [aXe per Firefox](https://addons.mozilla.org/en-US/firefox/addon/axe-devtools/)

Queste aggiungono una scheda di accessibilità agli strumenti per sviluppatori del browser. Ad esempio, abbiamo installato la versione per Firefox, poi l'abbiamo utilizzata per verificare il nostro esempio [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html). Abbiamo ottenuto i seguenti risultati:

![Uno screenshot dei problemi di accessibilità identificati dallo strumento Axe.](axe-screenshot.png)

aXe è installabile anche utilizzando `npm`, e può essere integrato con task runner come [Grunt](https://gruntjs.com/) e [Gulp](https://gulpjs.com/), framework di automazione come [Selenium](https://www.selenium.dev/) e [Cucumber](https://cucumber.io/), framework di test unitari come [Jasmine](https://jasmine.github.io/) e altro ancora (ancora, vedere la [pagina principale di aXe](https://www.deque.com/axe/) per i dettagli).

## Lettori di schermo

Uno dei tipi più comuni di tecnologia assistiva (AT) che le persone con disabilità utilizzano — e uno dei più comuni che utilizzerà per testare l'accessibilità delle sue pagine web — è rappresentato dai **lettori di schermo**. Questi sono software che leggono il contenuto delle pagine web o il contenuto di altre app installate sul sistema operativo di qualcuno. I lettori di schermo consentono alle persone di usare computer senza dover vedere alcun contenuto visivo.

I browser web espongono informazioni sul contenuto della pagina ai lettori di schermo (e ad altre AT) per comunicarle all'utente tramite una rappresentazione chiamata {{Glossary("Accessibility_tree", "albero di accessibilità")}}. Questo fornisce informazioni semantiche come nomi e descrizioni degli elementi, qual è il loro scopo o ruolo (è un pulsante o un campo di input?), e se si trovano in uno stato particolare (ad esempio, una finestra di dialogo è aperta o chiusa?).

Questa informazione potrebbe essere banale nel caso di un paragrafo di testo, che suona più o meno come è scritto, ma può diventare complicata quando si tratta di funzionalità dell'interfaccia utente come un menu a tendina o un lettore video. Questo è il motivo per cui è molto importante utilizzare correttamente l'HTML semantico, che esaminerà in dettaglio nel prossimo articolo di questo modulo. Se contrassegna il contenuto utilizzando l'elemento sbagliato, potrebbe confondere gli utenti di lettori di schermo.

Assicuri di avere uno o due lettori di schermo installati sul suo computer di sviluppo e provi a utilizzare i suoi siti web preferiti tramite un lettore di schermo, come discusso di seguito. Comprendere come le persone con disabilità visive usano il web è fondamentale per progettare prodotti che funzionano meglio per tutti.

### Quali lettori di schermo sono disponibili?

Esistono diversi lettori di schermo disponibili:

- Alcuni sono prodotti commerciali a pagamento, come [JAWS](https://www.freedomscientific.com/Products/software/JAWS/) (Windows).
- Alcuni sono prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome, Windows e macOS) e [Orca](https://wiki.gnome.org/Projects/Orca) (Linux).
- Alcuni sono integrati nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/vision/) (macOS e iOS), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (su Chromebook), e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

Generalmente, i lettori di schermo sono applicazioni separate che girano sul sistema operativo host e possono leggere le pagine web e i contenuti in altre applicazioni (questo non è sempre il caso; ChromeVox ad esempio è un'estensione del browser). I lettori di schermo tendono ad avere alcune differenze nel comportamento e nei controlli esatti, quindi dovrà consultare la documentazione per il lettore di schermo scelto per ottenere tutti i dettagli. Detto ciò, funzionano tutti fondamentalmente nello stesso modo.

Nelle prossime sezioni, esamineremo alcuni test con un paio di lettori di schermo diversi per darle un'idea generale di come funzionano e come testarli.

> [!NOTE]
> Il documento [Progettare per la compatibilità con i lettori di schermo](https://webaim.org/techniques/screenreader/) di WebAIM fornisce alcune informazioni utili sull'uso dei lettori di schermo e su cosa funziona meglio per essi. Vedere anche [Risultati del sondaggio degli utilizzatori di lettori di schermo #10](https://webaim.org/projects/screenreadersurvey10/#used) per alcune interessanti statistiche sull'uso dei lettori di schermo.

#### VoiceOver

VoiceOver (VO) viene fornito gratuitamente con Mac/iPhone/iPad di Apple, quindi è utile per testare su desktop/mobile se usa prodotti Apple. Lo abbiamo testato su macOS su un MacBook Pro.

Per attivarlo, premi <kbd>Cmd</kbd> + <kbd>F5</kbd>. Se non ha mai utilizzato VO prima, le verrà fornita una schermata di benvenuto dove può scegliere se avviare VO o meno, e fare una utile guida didattica per imparare come usarlo. Per disattivarlo, premi di nuovo <kbd>Cmd</kbd> + <kbd>F5</kbd>.

> [!NOTE]
> Dovrebbe seguire la guida almeno una volta, è un modo davvero utile per imparare VO.

Quando VO è attivo, il display apparirà per lo più lo stesso, ma vedrà una casella nera nell'angolo in basso a sinistra dello schermo che contiene informazioni su ciò che VO ha attualmente selezionato. La selezione corrente sarà anche evidenziata, con un bordo nero — questo evidenziamento è noto come il **cursore VO**.

![Uno screenshot d'esempio che dimostra un test di accessibilità usando VoiceOver sulla homepage MDN. In basso a sinistra dell'immagine è visibile un riquadro che evidenzia l'informazione selezionata sulla pagina web.](voiceover.png)

Per utilizzare VO, farà molto uso del "modificatore VO" — questo è un tasto o una combinazione di tasti che necessita di premere oltre ai comandi della tastiera VO per farli funzionare. L'uso di un modificatore come questo è comune con i lettori di schermo, per evitare che i loro comandi vadano in conflitto con altri comandi. Nel caso di VO, il modificatore può essere <kbd>CapsLock</kbd>, o <kbd>Ctrl</kbd> + <kbd>Option</kbd>.

VO ha molti comandi da tastiera, e non li elencheremo tutti qui. I comandi basilari di cui avrà bisogno per testare le pagine web sono nella tabella seguente. Nei comandi della tastiera, "VO" significa "il modificatore di VoiceOver".

<table class="standard-table no-markdown">
  <caption>
    Comandi comuni da tastiera di VoiceOver
  </caption>
  <thead>
    <tr>
      <th scope="col">Comando da tastiera</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VO + tasti freccia</td>
      <td>Spostare il cursore VO in alto, destra, basso, sinistra.</td>
    </tr>
    <tr>
      <td>VO + barra spaziatrice</td>
      <td>
        Selezionare/attivare elementi evidenziati dal cursore VO. Questo include gli
        elementi selezionati nel Rotor (vedi sotto).
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + freccia giù</td>
      <td>
        Entrare in un gruppo di elementi come una tabella HTML o un modulo. Una
        volta all'interno di un gruppo, può muoversi e selezionare elementi
        all'interno di quel gruppo usando i comandi sopra come normale.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + freccia su</td>
      <td>Uscire da un gruppo.</td>
    </tr>
    <tr>
      <td>VO + <kbd>C</kbd></td>
      <td>(quando dentro una tabella) Leggere l'intestazione dell'attuale colonna.</td>
    </tr>
    <tr>
      <td>VO + <kbd>R</kbd></td>
      <td>(quando dentro una tabella) Leggere l'intestazione dell'attuale riga.</td>
    </tr>
    <tr>
      <td>VO + <kbd>C</kbd> + <kbd>C</kbd> (due C in successione)</td>
      <td>
        (quando dentro una tabella) Leggere l'intero attuale colonna, compresa
        l'intestazione.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>R</kbd> + <kbd>R</kbd> (due R in successione)</td>
      <td>
        (quando dentro una tabella) Leggere l'intera attuale riga, comprese le
        intestazioni che corrispondono a ciascuna cella.
      </td>
    </tr>
    <tr>
      <td>VO + freccia sinistra, VO + freccia destra</td>
      <td>
        (quando dentro alcune opzioni orizzontali, come un selettore di date)
        Spostarsi tra le opzioni.
      </td>
    </tr>
    <tr>
      <td>VO + freccia su, VO + freccia giù</td>
      <td>
        (quando dentro alcune opzioni orizzontali, come un selettore di date)
        Cambiare l'attuale opzione.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>U</kbd></td>
      <td>
        Aprire il Rotor, che visualizza elenchi di intestazioni, link, controlli
        del modulo, ecc. per una facile navigazione.
      </td>
    </tr>
    <tr>
      <td>VO + freccia sinistra, VO + freccia destra</td>
      <td>
        (quando dentro Rotor) Spostarsi tra i diversi elenchi disponibili nel Rotor.
      </td>
    </tr>
    <tr>
      <td>VO + freccia su, VO + freccia giù</td>
      <td>
        (quando dentro Rotor) Spostarsi tra i diversi elementi nell'attuale
        elenco Rotor.
      </td>
    </tr>
    <tr>
      <td><kbd>Esc</kbd></td>
      <td>(quando dentro Rotor) Uscire dal Rotor.</td>
    </tr>
    <tr>
      <td><kbd>Ctrl</kbd></td>
      <td>(quando VO parla) Fermare/Riprendi la lettura.</td>
    </tr>
    <tr>
      <td>VO + <kbd>Z</kbd></td>
      <td>Riavviare l'ultimo discorso pronunciato.</td>
    </tr>
    <tr>
      <td>VO + <kbd>D</kbd></td>
      <td>Entrare nel Dock del Mac, così può selezionare le app da avviare all'interno.</td>
    </tr>
  </tbody>
</table>

Questi sembrano molti comandi, ma non è così difficile quando ci si abitua, e VO offre regolarmente promemoria su quali comandi usare in determinati punti. Giocare con VO ora; può quindi continuare a giocare con alcuni dei nostri esempi nella sezione [Test con lettore di schermo](#test_con_lettore_di_schermo).

#### NVDA

NVDA è solo per Windows, e dovrà installarlo.

1. Scarichi NVDA da [nvaccess.org](https://www.nvaccess.org/), poi lo installi. Può scegliere se effettuare una donazione o scaricarlo gratuitamente; avrà anche bisogno di fornire il proprio indirizzo email prima di poterlo scaricare.
2. Per avviare NVDA una volta installato, fare doppio clic sul file/programma di accesso rapido, oppure usare il comando da tastiera <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>N</kbd>. Vedrà la finestra di benvenuto di NVDA quando la avvia. Qui può scegliere tra un paio di opzioni, poi premere il tasto _OK_ per iniziare.

NVDA sarà ora attivo sul suo computer.

Per usare NVDA, farà molto uso del "modificatore NVDA" — il tasto che deve premere in aggiunta ai comandi da tastiera NVDA per farli funzionare. Il modificatore NVDA può essere <kbd>Insert</kbd> (il default) o <kbd>CapsLock</kbd> (può essere scelto spuntando la prima casella di controllo nella finestra di benvenuto NVDA prima di premere _OK_).

> [!NOTE]
> NVDA è più discreto di VoiceOver in termini di come evidenzia dove si trova e cosa sta facendo. Quando scorre attraverso intestazioni, elenchi, ecc., gli elementi su cui è selezionato saranno generalmente evidenziati con un sottile contorno, ma non sempre questo è il caso per tutte le cose. Se si perde completamente, può premere Ctrl + F5 per aggiornare la pagina corrente e ricominciare dall'alto.

NVDA ha molti comandi da tastiera, e non li elencheremo tutti qui. I comandi basilari di cui avrà bisogno per testare le pagine web sono nella tabella seguente. Nei comandi della tastiera, "NVDA" significa "il modificatore di NVDA".

<table class="standard-table no-markdown">
  <caption>
    I più comuni comandi da tastiera di NVDA
  </caption>
  <thead>
    <tr>
      <th scope="col">Comando da tastiera</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>NVDA + <kbd>Q</kbd></td>
      <td>Spegnere NVDA dopo averlo avviato.</td>
    </tr>
    <tr>
      <td>NVDA + freccia su</td>
      <td>Leggere la riga corrente.</td>
    </tr>
    <tr>
      <td>NVDA + freccia giù</td>
      <td>Iniziare a leggere dalla posizione corrente.</td>
    </tr>
    <tr>
      <td>Freccia su e freccia giù, o <kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>Spostarsi all'elemento precedente/successivo sulla pagina e leggerlo.</td>
    </tr>
    <tr>
      <td>Freccia sinistra e freccia destra</td>
      <td>Spostarsi al carattere precedente/successivo nell'elemento corrente e leggerlo.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>H</kbd> e <kbd>H</kbd></td>
      <td>Spostarsi all'intestazione precedente/successiva e leggerla.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>K</kbd> e <kbd>K</kbd></td>
      <td>Spostarsi al link precedente/successivo e leggerlo.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>D</kbd> e <kbd>D</kbd></td>
      <td>
        Spostarsi al landmark documento precedente/successivo (ad esempio, <code>&#x3C;nav></code>)
        e leggerlo.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>1</kbd>–<kbd>6</kbd> e <kbd>1</kbd>–<kbd>6</kbd></td>
      <td>Spostarsi all'intestazione precedente/successiva (livello 1–6) e leggerla.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>F</kbd> e <kbd>F</kbd></td>
      <td>Spostarsi all'input forma precedente/successivo e concentrarsi su di esso.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>T</kbd> e <kbd>T</kbd></td>
      <td>Spostarsi alla tabella dati precedente/successiva e concentrarsi su di essa.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>B</kbd> e <kbd>B</kbd></td>
      <td>Spostarsi al pulsante precedente/successivo e leggerne l'etichetta.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>L</kbd> e <kbd>L</kbd></td>
      <td>Spostarsi all'elenco precedente/successivo e leggere il primo elemento dell'elenco.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>I</kbd> e <kbd>I</kbd></td>
      <td>Spostarsi all'elemento dell'elenco precedente/successivo e leggerlo.</td>
    </tr>
    <tr>
      <td><kbd>Enter</kbd>/<kbd>Return</kbd></td>
      <td>
        (quando un link o pulsante o un altro elemento attivabile è selezionato) Attivare l'elemento.
      </td>
    </tr>
    <tr>
      <td>NVDA + <kbd>barra spaziatrice</kbd></td>
      <td>
        (quando un modulo è selezionato) Entrare nel modulo in modo che gli elementi
        individuali possano essere selezionati, o uscire dal modulo se si è già dentro.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>(quando dentro un modulo) Spostarsi tra input del modulo.</td>
    </tr>
    <tr>
      <td>Freccia su e freccia giù</td>
      <td>
        (quando dentro un modulo) Cambiare i valori di input del modulo (nel caso di controlli come
        le caselle di selezione).
      </td>
    </tr>
    <tr>
      <td><kbd>barra spaziatrice</kbd></td>
      <td>(quando dentro un modulo) Selezionare il valore scelto.</td>
    </tr>
    <tr>
      <td><kbd>Ctrl</kbd> + <kbd>Alt</kbd> + tasti freccia</td>
      <td>(quando una tabella è selezionata) Spostarsi tra le celle della tabella.</td>
    </tr>
  </tbody>
</table>

### Test con lettore di schermo

Ora che si è abituato a utilizzare un lettore di schermo, vorremmo che lo usasse per fare alcuni test di accessibilità rapidi, per farsi un'idea di come i lettori di schermo gestiscano le caratteristiche buone e cattive delle pagine web:

- Dia un'occhiata a [good-semantics.html](https://mdn.github.io/learning-area/accessibility/html/good-semantics.html), e noti come le intestazioni sono trovate dal lettore di schermo e disponibili per essere utilizzate per la navigazione. Ora dia un'occhiata a [bad-semantics.html](https://mdn.github.io/learning-area/accessibility/html/bad-semantics.html), e noti come il lettore di schermo non ottiene nessuna di queste informazioni. Immagini quanto sarebbe fastidioso cercare di navigare una pagina di testo veramente lunga.
- Dia un'occhiata a [good-links.html](https://mdn.github.io/learning-area/accessibility/html/good-links.html), e noti come i link abbiano senso quando visualizzati fuori contesto, ad esempio nel Rotor di VoiceOver. Questo non è il caso con [bad-links.html](https://mdn.github.io/learning-area/accessibility/html/bad-links.html) — sono tutti semplicemente "clicca qui".
- Dia un'occhiata a [good-form.html](https://mdn.github.io/learning-area/accessibility/html/good-form.html), e noti come gli input del modulo siano descritti utilizzando le loro etichette perché abbiamo aggiunto opportuni elementi {{htmlelement("label")}}. In [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html), ottengono un'etichetta poco utile come "vuoto".
- Dia un'occhiata al nostro esempio [punk-bands-complete.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html) e veda come il lettore di schermo è in grado di associare colonne e righe di contenuti e leggerli tutti assieme perché abbiamo definito correttamente le intestazioni della tabella. In [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html), nessuna delle celle può essere associata affatto. Noti che NVDA sembra comportarsi in modo leggermente strano quando ha solo una singola tabella in una pagina; potrebbe provare la [pagina di test delle tabelle di WebAIM](https://webaim.org/articles/nvda/tables.htm) invece.
- Dia un'occhiata all'[esempio di regioni dal vivo WAI-ARIA](https://www.freedomscientific.com/SurfsUp/AriaLiveRegions.htm), e noti come il lettore di schermo continuerà a leggere la sezione in costante aggiornamento man mano che si aggiorna.

## Altri strumenti

I lettori di schermo sono uno dei tipi più comuni di tecnologie assistive che incontrerà come sviluppatore web, ma esistono altri tipi di AT ed è utile essere familiarizzati con ciò che gli utenti stanno potenzialmente usando per accedere ai suoi contenuti. Questa sezione riassume alcuni di essi.

### Tastiere in caratteri grandi o in braille

È possibile ottenere tastiere progettate con testo grande per essere utilizzate da utenti ipovedenti o anziani, e tastiere in braille progettate per essere utilizzabili da persone cieche e gravemente ipovedenti.

### Dispositivi di puntamento alternativi

Quando pensa ai dispositivi di puntamento, i mouse sono l'esempio ovvio, ma esistono altri dispositivi di puntamento progettati per consentire agli utenti con diverse disabilità motorie di navigare nelle interfacce utente più comodamente:

- Trackball: Un po' come dei mouse rovesciati, i trackball consistono in una sfera montata che rimane fissa sulla scrivania, che può rotolare per spostare il puntatore. Sono considerati più precisi e facili da maneggiare rispetto ai mouse, specialmente per persone con mobilità ridotta della mano.
- Joystick: Un bastone di controllo che può essere spostato per muovere il puntatore. I joystick sono meno precisi dei trackball ma utilizzabili da persone con una vasta gamma di disabilità fisiche, anche gravi.
- Touchpad: La maggior parte dei laptop moderni ha un touchpad (a volte chiamato trackpad) — un sensore tattile piatto che le permette di muovere il puntatore con un dito, nonché di eseguire gesti multi-dito nello stesso modo dei gesti mobili. Può acquistare touchpad esterni per dispositivi che non ne hanno di interni. Alcune persone li trovano più precisi dei mouse.

### Ingranditori di schermo

Gli ingranditori di schermo forniscono agli utenti ipovedenti una vista ingrandita del display del loro dispositivo per consentirgli di comprendere e interagire più facilmente con i contenuti del dispositivo, oltre a fornire altre funzionalità come la regolazione dei colori per aiutare con il daltonismo e l'adattamento delle dimensioni dei puntatori del mouse e dei cursori di testo per renderli più visibili.

Sono disponibili ingranditori di schermo software e hardware:

- La maggior parte dei moderni sistemi operativi ha un'app integrata per ingrandire tutto o parte dello schermo, ad esempio Zoom su Mac o Magnificatore su Windows. Tendono anche a fornire opzioni per aumentare universalmente la dimensione del testo, la dimensione del puntatore del mouse, ecc. Inoltre, sono disponibili opzioni di terze parti.
- Gli ingranditori di schermo hardware tendono a consistere in uno schermo separato che si trova accanto o di fronte al display del suo dispositivo e proietta una versione ingrandita nel suo complesso o una parte di essa.

### Software di riconoscimento vocale

Il software di riconoscimento vocale (o del parlato) consente di pronunciare comandi per controllare il dispositivo e/o dettare il testo di email o documenti e fare in modo che il computer scriva il testo per lei. Questo è molto utile per le persone che non sono in grado di usare una tastiera o altri meccanismi di controllo.

I moderni sistemi operativi hanno funzioni integrate per abilitare ciò (ad esempio, Dictation su Mac o Voice Access su Windows), e ci sono anche app di terze parti disponibili, dalle app desktop alle estensioni del browser.

### Controlli a interruttore

I controlli a interruttore forniscono un meccanismo per interagire con i dispositivi per utenti con mobilità molto limitata o [disabilità cognitive](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility).

Un setup di controllo a interruttore di solito coinvolge due parti:

- Un interruttore o pulsante fisico per attivare le opzioni sul dispositivo. Può anche assegnare funzionalità di interruttore a pulsanti regolari del dispositivo (come i controlli del volume) o tasti su una tastiera.
- Una modalità del dispositivo o un add-on di software di terzi che rende il dispositivo compatibile con il controllo degli interruttori. Ad esempio, Accesso Interruttore su Android è una modalità in cui le diverse opzioni in varie situazioni (ad esempio, le app sulla schermata iniziale) vengono ciclate e la desiderata può essere selezionata con un tasto o interruttore quando raggiunta.

## Pianificazione per l'accessibilità

Bisogna pensare attentamente all'accessibilità fin dall'inizio di ogni progetto. Assicurarsi che l'accessibilità sia considerata durante la fase iniziale di progettazione, in modo da:

- Prendere le basi, ad esempio usando [una buona struttura del documento](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_well-structured_text_content) e fornendo [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) per le immagini.
- Considerare attentamente il miglior approccio per le funzionalità che potrebbero avere problemi di accessibilità. Ad esempio, audio e video saranno sicuramente inaccessibili per alcune persone, quindi dovrebbero essere forniti alternative, come [trascrizioni](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#audio_transcripts) e [tracce di testo](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#video_text_tracks).
- Evitare errori costosi più avanti. I problemi scoperti vicino alla fine di un progetto tendono ad essere molto più dispendiosi in termini di tempo e costi di risoluzione rispetto ai problemi riscontrati all'inizio.

## Test utente

Non può fare affidamento solo sugli strumenti automatizzati per determinare i problemi di accessibilità sul suo sito. Ogni progetto di sito web necessita di una [strategia di testing utente](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies#user_testing) e si raccomanda fortemente di includere alcuni gruppi di utenti con esigenze di accessibilità:

- Cerchi di coinvolgere alcuni utenti di lettori di schermo, alcuni utenti che utilizzano solo la tastiera, alcuni utenti non udenti, utenti con disabilità motorie ecc.
- Chieda a ciascun gruppo di provare a utilizzare il sito web in generale, iniziando con la visione della pagina principale e di altre pagine principali, e provando alcune delle funzionalità principali. Esempi tipici includono l'acquisto di un prodotto o la prenotazione. Chieda loro quali sono state le loro impressioni e quali problemi hanno incontrato.
- Successivamente, li faccia concentrare su funzionalità o flussi di lavoro che ha specifiche preoccupazioni di accessibilità, come complessi controlli dei moduli o lettori video. Chieda loro cosa manca per loro in termini di esperienza utente, e cosa vorrebbero venisse cambiato.

Alcuni progetti avranno un budget per pagare gruppi di test, mentre altri faranno affidamento su volontari non pagati o anche su colleghi e amici.

## Lista di controllo dei test di accessibilità

L'elenco seguente fornisce una lista di controllo che si può seguire per assicurarsi di aver svolto i test di accessibilità raccomandati per il suo progetto:

1. Assicurarsi che il suo HTML sia il più semantico possibile. [Validarlo](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML#html_validation) è un buon inizio, così come usare un [auditing tool](#strumenti_di_audit).
2. Verificare che il contenuto abbia senso quando il CSS è disattivato.
3. Assicurarsi che la funzionalità sia accessibile da tastiera (vedi [Usare controlli UI semantici dove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) per ulteriori dettagli). Testare usando Tab, Return/Enter, ecc.
4. Assicurarsi che i suoi contenuti non testuali abbiano [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives). Un [auditing tool](#strumenti_di_audit) è utile per rilevare tali problemi.
5. Assicurarsi che il [contrasto dei colori](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#color_and_color_contrast) del suo sito sia accettabile, usando uno strumento di controllo adeguato.
6. Assicurarsi che i [contenuti nascosti](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#hiding_things) siano visibili ai lettori di schermo.
7. Assicurarsi che la funzionalità sia utilizzabile senza JavaScript ove possibile.
8. Utilizzare ARIA per migliorare l'accessibilità ove appropriato.
9. Fare passare il suo sito tramite un [auditing tool](#strumenti_di_audit).
10. Testarlo con un lettore di schermo.
11. Includere una politica/dichiarazione di accessibilità in un punto facilmente individuabile sul suo sito per indicare ciò che è stato fatto.

## Sommario

Speriamo che questo articolo le abbia dato un'idea dei tipi di strumenti che può utilizzare per aiutare a risolvere i problemi di accessibilità, delle tecnologie assistive usate dalle persone con disabilità per aiutare ad accedere al web.

Nel prossimo articolo esamineremo come scrivere HTML accessibile.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_Accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}
