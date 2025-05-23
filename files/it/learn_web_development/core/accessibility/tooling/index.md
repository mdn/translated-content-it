---
title: Strumenti di accessibilità e tecnologie assistive
short-title: Strumenti di accessibilità
slug: Learn_web_development/Core/Accessibility/Tooling
l10n:
  sourceCommit: eaec5c4226ac64696a95314a7bce995165a4d124
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_Accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}

Successivamente, ci concentreremo sugli strumenti di accessibilità, fornendo informazioni sui vari tipi di strumenti che si possono utilizzare per risolvere i problemi di accessibilità, aiutando a comprendere le **tecnologie assistive** utilizzate dalle persone con disabilità per navigare sul web. Utilizzerai gli strumenti descritti qui negli articoli successivi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Familiarità con il tipo di strumenti che si possono usare per risolvere i problemi di accessibilità, ad esempio strumenti di audit.</li>
          <li>Impostare i lettori di schermo e utilizzarli per testare i siti web su desktop e mobile.</li>
          <li>Familiarità con altri tipi di tecnologie assistive come tastiere braille o con caratteri grandi, dispositivi di puntamento alternativi e ingranditori di schermo.</li>
          <li>L'importanza dei test utente accanto ai test automatizzati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti di accessibilità

Esaminiamo gli strumenti e le tecniche che puoi usare per testare l'accessibilità di un sito web e correggere i problemi che scopri.

### Test dell'ordine del sorgente

Il tuo contenuto dovrebbe avere senso logico nell'ordine del sorgente: puoi sempre visualizzarlo diversamente con il CSS in seguito, ma dovresti iniziare a ottenere la struttura sottostante corretta. Questo perché le tecnologie assistive leggono il contenuto del sito in base all'ordine del sorgente, e le persone con disabilità spesso modificano o disattivano parti del CSS per rendere il contenuto più leggibile (esempi comuni sono l'aumento delle dimensioni del carattere e l'applicazione di schemi di colore ad alto contrasto).

Per testare l'ordine del sorgente, puoi disattivare il CSS di un sito e vedere quanto è comprensibile senza di esso. Potresti fare ciò manualmente rimuovendo il CSS dal tuo codice, ma il modo più semplice è usare le funzionalità del browser, per esempio:

- Firefox: Seleziona _View > Page Style > No Style_ dal menu principale.
- Safari: [Apri gli strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser), clicca sul pulsante _Device Settings_ vicino alla parte superiore sinistra del pannello degli strumenti per sviluppatori (sembra un monitor del computer), quindi seleziona la casella "Disable CSS" nel pannello che appare.
- Chrome/Edge: Installa l'estensione [Web Developer Toolbar](https://chromewebstore.google.com/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm), quindi riavvia il browser. Clicca sull'icona a forma di ingranaggio "Web Developer" che ora dovrebbe essere disponibile nel tuo menu delle estensioni, quindi seleziona _CSS > Disable All Styles_.

### Strumenti di verifica del contrasto dei colori

Quando scegli una combinazione di colori per il tuo sito web, devi assicurarti che il colore del testo (in primo piano) contrasti bene con il colore di sfondo. Il tuo design potrebbe sembrare fantastico, ma non serve se le persone non possono leggere il tuo contenuto. Usa uno strumento come il [Controllo del contrasto dei colori di WebAIM](https://webaim.org/resources/contrastchecker/) per verificare se la tua combinazione ha un contrasto sufficiente.

Un altro consiglio è evitare di usare solo il colore per segnalare o evidenziare informazioni importanti, poiché potrebbe essere ignorato da persone con disabilità visive come il daltonismo. Invece di contrassegnare i campi obbligatori del modulo in rosso, ad esempio, segnali con un asterisco e in rosso.

> [!NOTE]
> Un alto rapporto di contrasto permetterà anche a chiunque utilizzi uno smartphone o un tablet con uno schermo lucido di leggere meglio le pagine in un ambiente luminoso, come sotto la luce del sole.

### Strumenti di audit

Sono disponibili diversi strumenti di audit in cui puoi inserire le tue pagine web. Questi analizzeranno le pagine e restituiranno un elenco dei problemi di accessibilità presenti sulla pagina. Esaminiamo [Wave](https://wave.webaim.org/) come esempio, uno strumento di test dell'accessibilità online che accetta un indirizzo web e restituisce una vista annotata di quella pagina con i problemi di accessibilità evidenziati.

1. Vai alla [homepage di Wave](https://wave.webaim.org/).
2. Inserisci l'URL del nostro esempio [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html) nel campo di testo vicino alla parte superiore della pagina. Premi poi invio o fai clic/tocca la freccia al margine destro del campo di input.
3. Il sito dovrebbe evidenziare i problemi di accessibilità presenti. Clicca sulle icone visualizzate per vedere maggiori informazioni su ciascun problema identificato dalla valutazione di Wave.

Altri strumenti di audit che vale la pena controllare:

- [Inspector di accessibilità di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html)
- [ANDI bookmarklet](https://www.ssa.gov/accessibility/andi/help/install.html)
- [Audit di accessibilità di Google Lighthouse](https://developer.chrome.com/docs/lighthouse/accessibility/scoring)

> [!NOTE]
> Tali strumenti non sono sufficienti per risolvere tutti i problemi di accessibilità da soli. Avrai bisogno di una combinazione di questi, conoscenze ed esperienza, test utente, ecc. per ottenere un quadro completo.

Lo strumento [aXe di Deque](https://www.deque.com/axe/) va un po' oltre rispetto agli strumenti di audit sopracitati. Come gli altri, controlla le pagine e restituisce errori di accessibilità. La sua forma probabilmente più immediatamente utile sono le estensioni del browser:

- [aXe per Chrome](https://chromewebstore.google.com/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd)
- [aXe per Firefox](https://addons.mozilla.org/en-US/firefox/addon/axe-devtools/)

Queste aggiungono una scheda di accessibilità agli strumenti per sviluppatori del browser. Ad esempio, abbiamo installato la versione per Firefox, quindi l'abbiamo utilizzata per eseguire un audit sul nostro esempio [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html). Abbiamo ottenuto i seguenti risultati:

![Una schermata dei problemi di accessibilità identificati dallo strumento Axe.](axe-screenshot.png)

aXe è anche installabile tramite `npm` e può essere integrato con task runner come [Grunt](https://gruntjs.com/) e [Gulp](https://gulpjs.com/), framework di automazione come [Selenium](https://www.selenium.dev/) e [Cucumber](https://cucumber.io/), framework di test unitari come [Jasmine](https://jasmine.github.io/) e altro ancora (nuovamente, vedi la [pagina principale di aXe](https://www.deque.com/axe/) per i dettagli).

## Lettori di schermo

Uno dei tipi più comuni di tecnologia assistiva (AT) che le persone con disabilità utilizzano — e uno di quelli più comuni che utilizzerai per testare l'accessibilità delle tue pagine web — è il **lettore di schermo**. Questi sono software che leggono il contenuto delle pagine web o il contenuto di altre app installate sul sistema operativo di una persona. I lettori di schermo consentono alle persone di utilizzare i computer senza dover vedere alcun contenuto visivo.

I browser web espongono informazioni sul contenuto della pagina per i lettori di schermo (e altre AT) per comunicare all'utente tramite una rappresentazione chiamata {{Glossary("Accessibility_tree", "accessibility tree")}}. Questo fornisce informazioni semantiche come nomi e descrizioni di elementi, quale sia il loro scopo o ruolo (è un pulsante, o un campo di input?), e se si trovano in uno stato particolare (ad esempio, una finestra di dialogo è aperta o chiusa?).

Queste informazioni possono sembrare banali nel caso di un paragrafo di testo, che suona praticamente come è scritto, ma possono complicarsi quando si tratta di funzionalità dell'interfaccia utente come un menu a discesa o un lettore video. Ecco perché è molto importante utilizzare correttamente HTML semantico, che vedrai in dettaglio nel prossimo articolo di questo modulo. Se contrassegni il contenuto utilizzando l'elemento sbagliato, può confondere gli utenti del lettore di schermo.

Assicurati di avere installato un lettore di schermo o due sulla tua macchina di sviluppo e prova a utilizzare i tuoi siti web preferiti tramite un lettore di schermo, come discusso di seguito. Comprendere come le persone con disabilità visive utilizzano il web è fondamentale per progettare prodotti che funzionino meglio per tutti.

### Quali lettori di schermo sono disponibili?

Sono disponibili diversi lettori di schermo:

- Alcuni sono prodotti commerciali a pagamento, come [JAWS](https://www.freedomscientific.com/Products/software/JAWS/) (Windows).
- Alcuni sono prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome, Windows, e macOS), e [Orca](https://wiki.gnome.org/Projects/Orca) (Linux).
- Alcuni sono integrati nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/features/?vision) (macOS e iOS), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (su Chromebooks), e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

In generale, i lettori di schermo sono app separate che girano sul sistema operativo host e possono leggere pagine web e il contenuto in altre app (questo non è sempre il caso; ChromeVox ad esempio è un'estensione del browser). I lettori di schermo tendono ad avere alcune differenze nel comportamento esatto e nei controlli, quindi dovrai consultare la documentazione per il lettore di schermo scelto per ottenere tutti i dettagli. Detto ciò, tutti funzionano fondamentalmente nello stesso modo.

Nelle prossime sezioni, passeremo attraverso alcuni test con un paio di diversi lettori di schermo per darti un'idea generale di come funzionano e come testarli.

> [!NOTE]
> La guida [Designing for Screen Reader Compatibility](https://webaim.org/techniques/screenreader/) di WebAIM fornisce alcune informazioni utili sull'uso dei lettori di schermo e su cosa funziona meglio per essi. Consulta anche i [Risultati del sondaggio #10 sull'uso dei lettori di schermo](https://webaim.org/projects/screenreadersurvey10/#used) per alcune statistiche interessanti sull'uso dei lettori di schermo.

#### VoiceOver

VoiceOver (VO) è gratuito con i prodotti Apple mac/iPhone/iPad, quindi è utile per testare su desktop/mobile se usi prodotti Apple. Lo abbiamo testato su macOS su un MacBook Pro.

Per accenderlo, premi <kbd>Cmd</kbd> + <kbd>F5</kbd>. Se non hai mai usato VO prima, ti verrà presentata una schermata di benvenuto dove puoi scegliere di avviare VO o meno, e seguire un tutorial piuttosto utile per imparare come usarlo. Per spegnerlo, premi di nuovo <kbd>Cmd</kbd> + <kbd>F5</kbd>.

> [!NOTE]
> Dovresti seguire il tutorial almeno una volta: è un modo davvero utile per imparare VO.

Quando VO è acceso, la visualizzazione sarà per lo più uguale, ma vedrai una casella nera nell'angolo in basso a sinistra dello schermo che contiene informazioni su ciò che VO ha attualmente selezionato. La selezione corrente sarà anche evidenziata con un bordo nero — questo evidenziamento è noto come il **cursore VO**.

![Una schermata di esempio che dimostra il collaudo dell'accessibilità usando VoiceOver sulla homepage di MDN. L'angolo in basso a sinistra dell'immagine è un'illuminazione delle informazioni selezionate sulla pagina web.](voiceover.png)

Per usare VO, farai molto uso del "modificatore VO" — questo è un tasto o una combinazione di tasti che devi premere oltre ai veri e propri scorciatoie da tastiera VO affinché funzionino. L'uso di un modificatore come questo è comune con i lettori di schermo, per evitare che i loro comandi vadano in conflitto con altri comandi. Nel caso di VO, il modificatore può essere <kbd>CapsLock</kbd>, o <kbd>Ctrl</kbd> + <kbd>Option</kbd>.

VO ha molti comandi da tastiera e non li elencheremo tutti qui. Quelli fondamentali di cui avrai bisogno per il test delle pagine web sono nella tabella seguente. Nelle scorciatoie da tastiera, "VO" significa "il modificatore VoiceOver".

<table class="standard-table no-markdown">
  <caption>
    Comuni scorciatoie da tastiera di VoiceOver
  </caption>
  <thead>
    <tr>
      <th scope="col">Scorciatoia da tastiera</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VO + Tasti cursore</td>
      <td>Sposta il cursore VO in su, destra, giù, sinistra.</td>
    </tr>
    <tr>
      <td>VO + Barra spaziatrice</td>
      <td>Seleziona/attiva gli elementi evidenziati dal cursore VO. Questo include elementi selezionati nel Rotor (vedi sotto).</td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + cursore giù</td>
      <td>Entra in un gruppo di elementi come una tabella HTML o un modulo. Una volta all'interno di un gruppo, puoi muoverti e selezionare elementi all'interno di quel gruppo usando i comandi di cui sopra come al solito.</td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + cursore su</td>
      <td>Esce da un gruppo.</td>
    </tr>
    <tr>
      <td>VO + <kbd>C</kbd></td>
      <td>(quando all'interno di una tabella) Leggi l'intestazione della colonna corrente.</td>
    </tr>
    <tr>
      <td>VO + <kbd>R</kbd></td>
      <td>(quando all'interno di una tabella) Leggi l'intestazione della riga corrente.</td>
    </tr>
    <tr>
      <td>VO + <kbd>C</kbd> + <kbd>C</kbd> (due C di seguito)</td>
      <td>(quando all'interno di una tabella) Leggi l'intera colonna corrente, intestazione inclusa.</td>
    </tr>
    <tr>
      <td>VO + <kbd>R</kbd> + <kbd>R</kbd> (due R di seguito)</td>
      <td>(quando all'interno di una tabella) Leggi l'intera riga corrente, comprese le intestazioni corrispondenti a ciascuna cella.</td>
    </tr>
    <tr>
      <td>VO + cursore sinistra, VO + cursore destra</td>
      <td>(quando all'interno di alcune opzioni orizzontali, come un selettore di date) Muovi tra le opzioni.</td>
    </tr>
    <tr>
      <td>VO + cursore su, VO + cursore giù</td>
      <td>(quando all'interno di alcune opzioni orizzontali, come un selettore di date) Cambia l'opzione corrente.</td>
    </tr>
    <tr>
      <td>VO + <kbd>U</kbd></td>
      <td>Apri il Rotor, che mostra elenchi di intestazioni, collegamenti, controlli modulo, ecc. per una navigazione facile.</td>
    </tr>
    <tr>
      <td>VO + cursore sinistra, VO + cursore destra</td>
      <td>(quando all'interno del Rotor) Muovi tra i diversi elenchi disponibili nel Rotor.</td>
    </tr>
    <tr>
      <td>VO + cursore su, VO + cursore giù</td>
      <td>(quando all'interno del Rotor) Muovi tra i diversi elementi nell'elenco corrente del Rotor.</td>
    </tr>
    <tr>
      <td><kbd>Esc</kbd></td>
      <td>(quando all'interno del Rotor) Esci dal Rotor.</td>
    </tr>
    <tr>
      <td><kbd>Ctrl</kbd></td>
      <td>(quando VO sta parlando) Metti in pausa/Riprendi il discorso.</td>
    </tr>
    <tr>
      <td>VO + <kbd>Z</kbd></td>
      <td>Ricomincia l'ultimo spezzone di discorso.</td>
    </tr>
    <tr>
      <td>VO + <kbd>D</kbd></td>
      <td>Entra nel Dock del mac, così puoi selezionare le app da eseguire al suo interno.</td>
    </tr>
  </tbody>
</table>

Sembrerebbe un sacco di comandi, ma non è così male una volta che ci si abitua, e VO regolarmente ti dà promemoria su quali comandi usare in certi luoghi. Gioca un po' con VO ora; potrai poi provare alcuni dei nostri esempi nella sezione [Test del lettore di schermo](#test_del_lettore_di_schermo).

#### NVDA

NVDA è solo per Windows, e dovrai installarlo.

1. Scarica NVDA da [nvaccess.org](https://www.nvaccess.org/), quindi installalo. Puoi scegliere se fare una donazione o scaricarlo gratuitamente; dovrai anche fornire loro il tuo indirizzo email prima di poterlo scaricare.
2. Per avviare NVDA una volta installato, fai doppio clic sul file/collegamento del programma, o usa la scorciatoia da tastiera <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>N</kbd>. All'avvio, vedrai la finestra di dialogo di benvenuto di NVDA. Qui puoi scegliere da un paio di opzioni, poi premi il pulsante _OK_ per iniziare.

NVDA sarà ora attivo sul tuo computer.

Per usare NVDA, farai molto uso del "modificatore NVDA" — il tasto che devi premere in aggiunta alle vere scorciatoie da tastiera di NVDA affinché funzionino. Il modificatore NVDA può essere <kbd>Insert</kbd> (predefinito), o <kbd>CapsLock</kbd> (può essere scelto selezionando la prima casella di controllo nella finestra di dialogo di benvenuto di NVDA prima di premere _OK_).

> [!NOTE]
> NVDA è più sottile di VoiceOver in termini di come evidenzia dove si trova e cosa sta facendo. Quando scorri tra intestazioni, elenchi, ecc., gli elementi su cui sei selezionato saranno generalmente evidenziati con un contorno sottile, ma questo non è sempre il caso per tutte le cose. Se ti senti completamente perso, puoi premere Ctrl + F5 per aggiornare la pagina corrente e ricominciare dall'inizio.

NVDA ha molti comandi da tastiera e non li elencheremo tutti qui. Quelli fondamentali di cui avrai bisogno per il test delle pagine web sono nella tabella seguente. Nelle scorciatoie da tastiera, "NVDA" significa "il modificatore NVDA".

<table class="standard-table no-markdown">
  <caption>
    Le scorciatoie da tastiera più comuni di NVDA
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
      <td>Spegni NVDA dopo averlo avviato.</td>
    </tr>
    <tr>
      <td>NVDA + cursore su</td>
      <td>Leggi la linea corrente.</td>
    </tr>
    <tr>
      <td>NVDA + cursore giù</td>
      <td>Inizia a leggere dalla posizione corrente.</td>
    </tr>
    <tr>
      <td>Cursore su e cursore giù, o <kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>Sposta all'elemento precedente/successivo sulla pagina e leggilo.</td>
    </tr>
    <tr>
      <td>Cursore sinistra e cursore destra</td>
      <td>Sposta al carattere precedente/successivo nell'elemento corrente e leggilo.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>H</kbd> e <kbd>H</kbd></td>
      <td>Sposta all'intestazione precedente/successiva e leggila.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>K</kbd> e <kbd>K</kbd></td>
      <td>Sposta al collegamento precedente/successivo e leggilo.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>D</kbd> e <kbd>D</kbd></td>
      <td>
        Sposta al punto di riferimento del documento precedente/successivo (es. <code>&#x3C;nav></code>)
        e leggilo.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>1</kbd>–<kbd>6</kbd> e <kbd>1</kbd>–<kbd>6</kbd></td>
      <td>Sposta all'intestazione precedente/successiva (livello 1–6) e leggila.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>F</kbd> e <kbd>F</kbd></td>
      <td>Sposta all'input modulo precedente/successivo e focalizza su di esso.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>T</kbd> e <kbd>T</kbd></td>
      <td>Sposta alla tabella di dati precedente/successiva e focalizza su di essa.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>B</kbd> e <kbd>B</kbd></td>
      <td>Sposta al pulsante precedente/successivo e leggine l'etichetta.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>L</kbd> e <kbd>L</kbd></td>
      <td>Sposta all'elenco precedente/successivo e leggine il primo elemento.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>I</kbd> e <kbd>I</kbd></td>
      <td>Sposta all'elemento di elenco precedente/successivo e leggilo.</td>
    </tr>
    <tr>
      <td><kbd>Enter</kbd>/<kbd>Return</kbd></td>
      <td>
        (quando un collegamento/pulsante o altro elemento attivabile è selezionato) Attiva l'elemento.
      </td>
    </tr>
    <tr>
      <td>NVDA + <kbd>Spacebar</kbd></td>
      <td>
        (quando un modulo è selezionato) Entra nel modulo in modo che singoli elementi possano essere selezionati,
        o esci dal modulo se ci sei già dentro.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>(quando all'interno di un modulo) Muovi tra gli input del modulo.</td>
    </tr>
    <tr>
      <td>Cursore su e cursore giù</td>
      <td>
        (quando all'interno di un modulo) Cambia i valori di input del modulo (nel caso di controlli come
        box di selezione).
      </td>
    </tr>
    <tr>
      <td><kbd>Spacebar</kbd></td>
      <td>(quando all'interno di un modulo) Seleziona il valore scelto.</td>
    </tr>
    <tr>
      <td><kbd>Ctrl</kbd> + <kbd>Alt</kbd> + tasti cursore</td>
      <td>(quando una tabella è selezionata) Muovi tra le celle della tabella.</td>
    </tr>
  </tbody>
</table>

### Test del lettore di schermo

Ora che hai preso familiarità con l'utilizzo di un lettore di schermo, ti invitiamo a usarlo per fare alcuni test di accessibilità rapidi, per avere un'idea di come i lettori di schermo gestiscono funzionalità buone e cattive delle pagine web:

- Guarda [good-semantics.html](https://mdn.github.io/learning-area/accessibility/html/good-semantics.html) e nota come le intestazioni vengono trovate dal lettore di schermo e sono disponibili per la navigazione. Ora guarda [bad-semantics.html](https://mdn.github.io/learning-area/accessibility/html/bad-semantics.html) e nota come il lettore di schermo non riceve nessuna di queste informazioni. Immagina quanto sarebbe fastidioso quando stai cercando di navigare in una pagina di testo molto lunga.
- Guarda [good-links.html](https://mdn.github.io/learning-area/accessibility/html/good-links.html) e nota come abbiano senso se visti fuori contesto, ad esempio nel Rotor di VoiceOver. Questo non è il caso di [bad-links.html](https://mdn.github.io/learning-area/accessibility/html/bad-links.html) — tutti sono solo "clicca qui".
- Guarda [good-form.html](https://mdn.github.io/learning-area/accessibility/html/good-form.html) e nota come gli input del modulo sono descritti utilizzando le loro etichette perché abbiamo aggiunto elementi {{htmlelement("label")}} appropriati. In [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html), ricevono un'etichetta poco utile del tipo "vuoto".
- Guarda il nostro esempio [punk-bands-complete.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html) e guarda come il lettore di schermo sia in grado di associare colonne e righe di contenuti e leggerle tutte insieme perché abbiamo definito correttamente le intestazioni della tabella. In [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html), nessuna cella può essere associata affatto. Nota che NVDA sembra comportarsi in modo leggermente strano quando hai solo una singola tabella su una pagina; potresti provare la [pagina di test delle tabelle di WebAIM](https://webaim.org/articles/nvda/tables.htm) invece.
- Dai un'occhiata all'[esempio di live regions WAI-ARIA](https://www.freedomscientific.com/SurfsUp/AriaLiveRegions.htm) e nota come il lettore di schermo continuerà a leggere la sezione costantemente aggiornata man mano che viene aggiornata.

## Altri strumenti

I lettori di schermo sono uno dei tipi più comuni di tecnologia assistiva che incontrerai come sviluppatore web, ma esistono altri tipi di AT, ed è utile essere familiari con ciò che gli utenti possono potenzialmente utilizzare per accedere ai tuoi contenuti. Questa sezione riassume alcuni di essi.

### Tastiere in braille o con testo grande

È possibile ottenere tastiere con testo grande progettate per essere utilizzabili da utenti con problemi visivi o anziani e tastiere in braille progettate per essere utilizzabili da persone cieche e con gravi problemi visivi.

### Dispositivi di puntamento alternativi

Quando pensi ai dispositivi di puntamento, i mouse sono l'esempio ovvio, ma ci sono altri dispositivi di puntamento progettati per permettere agli utenti con diversi problemi di mobilità di navigare le interfacce utente più facilmente:

- Trackball: Una sorta di mouse capovolto, i trackball consistono in una sfera montata che rimane ferma sulla tua scrivania, che puoi far rotolare per muovere il cursore intorno. Sono considerati più precisi e facili da gestire rispetto ai mouse, specialmente per persone con movimento della mano limitato.
- Joystick: Una leva di controllo che può essere mossa per muovere il cursore intorno. I joystick sono meno precisi rispetto ai trackball ma utilizzabili da persone con una vasta gamma di disabilità fisiche, persino disabilità gravi.
- Touchpad: La maggior parte dei laptop moderni ha un touchpad (talvolta chiamato trackpad) — un sensore tattile piatto che permette di muovere il cursore intorno con un dito, nonché di eseguire gesti multi-dito allo stesso modo dei gesti mobili. Puoi acquistare touchpad esterni per dispositivi che non ne hanno interni. Alcune persone li trovano più precisi dei mouse.

### Ingranditori di schermo

Gli ingranditori di schermo forniscono agli utenti ipovedenti una visualizzazione ingrandita dello schermo del dispositivo per consentire loro di comprendere e interagire con il contenuto del dispositivo più facilmente, oltre a fornire altre funzioni come la regolazione dei colori per aiutare con il daltonismo, e la regolazione delle dimensioni dei cursori del mouse e del testo per renderli più facili da vedere.

Sono disponibili ingranditori di schermo software e hardware:

- La maggior parte dei sistemi operativi moderni ha un'app integrata per ingrandire tutto o parte dello schermo, ad esempio Zoom su mac o Magnifier su Windows. Tendono anche a fornire opzioni per aumentare universalmente la dimensione del testo, la dimensione del cursore del mouse, ecc. Sono disponibili anche opzioni di terze parti.
- Gli ingranditori di schermo hardware tendono a consistere in uno schermo separato che si trova accanto o davanti allo schermo del tuo dispositivo e proietta una versione più grande di esso, o una versione ingrandita di una parte di esso.

### Software di riconoscimento vocale

Il software di riconoscimento vocale (o parlato) consente di dare comandi vocali per controllare il dispositivo e/o di parlare il testo di email o documenti e far sì che il computer scriva il testo per te. Questo è molto utile per le persone che non possono utilizzare una tastiera o altri meccanismi di controllo.

I sistemi operativi moderni hanno funzionalità integrate per abilitare questo (ad esempio Dictation su mac o Voice Access su Windows), e ci sono disponibili anche app di terze parti, dalle app desktop alle estensioni del browser.

### Controlli interruttore

I controlli interruttore offrono un meccanismo per interagire con dispositivi per utenti con mobilità molto limitata o [disabilità cognitiva](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility).

Un setup di controllo interruttore solitamente coinvolge due parti:

- Un interruttore fisico o pulsante per attivare opzioni sul dispositivo. Puoi anche assegnare funzionalità di interruttore a pulsanti regolari del dispositivo (come i controlli del volume) o tasti su una tastiera.
- Una modalità dispositivo o add-in software di terze parti che rende il dispositivo compatibile con il controllo interruttore o pulsante. Ad esempio, Switch Access su Android è una modalità in cui le diverse opzioni in varie situazioni (ad esempio, app sulla schermata iniziale) vengono ciclate e quella che desideri può essere selezionata con un pulsante o interruttore quando raggiunta.

## Pianificazione per l'accessibilità

Dovresti pensare attentamente all'accessibilità già nelle prime fasi di ciascun progetto. Assicurati che l'accessibilità sia considerata durante la fase di progettazione iniziale, in modo da poter:

- Raggiungere i fondamenti corretti, ad esempio utilizzando una [buona struttura del documento](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_well-structured_text_content) e fornendo [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) per le immagini.
- Considerare attentamente il migliore approccio per funzionalità che potrebbero avere problemi di accessibilità. Ad esempio, audio e video saranno sicuramente inaccessibili per alcune persone, quindi dovresti fornire alternative come [trascrizioni](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#audio_transcripts) e [tracce di testo](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#video_text_tracks).
- Evitare errori costosi più avanti. I problemi scoperti verso la fine di un progetto tendono a richiedere molto più tempo e denaro per essere corretti rispetto ai problemi rilevati al principio.

## Test utente

Non puoi affidarti solo agli strumenti automatizzati per determinare problemi di accessibilità sul tuo sito. Ogni progetto web necessita di una [strategia di test utente](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies#user_testing), ed è altamente raccomandato includere alcuni gruppi di utenti accessibili:

- Cerca di coinvolgere alcuni utenti di lettori di schermo, alcuni utenti solo tastiera, alcuni utenti senza capacità uditiva, utenti con problemi di mobilità, ecc.
- Fai provare a ciascun gruppo a utilizzare il sito web in generale, iniziando con la visualizzazione della pagina principale e di altre pagine principali, e provando alcune delle funzionalità principali. Esempi tipici includono l'acquisto di un prodotto o la prenotazione. Chiedi loro quali impressioni hanno avuto e quali problemi hanno riscontrato.
- Successivamente, falli concentrare su funzionalità o flussi di lavoro su cui hai preoccupazioni specifiche di accessibilità, come controlli modulo complessi o lettori video. Chiedi loro cosa manca in termini di esperienza utente e cosa vorrebbero vedere modificato.

Alcuni progetti avranno un budget per pagare i gruppi di test, mentre altri si affideranno a volontari non retribuiti o persino colleghi e amici.

## Lista di controllo per i test di accessibilità

L'elenco seguente fornisce una lista di controllo da seguire per assicurarti di avere eseguito i test di accessibilità consigliati per il tuo progetto:

1. Assicurati che il tuo HTML sia il più semanticamente corretto possibile. [Validarlo](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML#html_validation) è un buon punto di partenza, come anche l'uso di uno [strumento di audit](#strumenti_di_audit).
2. Controlla che il tuo contenuto abbia senso quando il CSS è disattivato.
3. Assicurati che la tua funzionalità sia accessibile tramite tastiera (vedi [Usa controlli UI semantici quando possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) per ulteriori dettagli). Testa usando Tab, Return/Enter, ecc.
4. Assicurati che il tuo contenuto non testuale abbia [alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives). Uno [strumento di audit](#strumenti_di_audit) è utile per rilevare tali problemi.
5. Assicurati che il [contrasto dei colori](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#color_and_color_contrast) del tuo sito sia accettabile, utilizzando uno strumento di verifica adeguato.
6. Assicurati che i contenuti [nascosti](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#hiding_things) siano visibili dai lettori di schermo.
7. Assicurati che la funzionalità sia utilizzabile senza JavaScript dove possibile.
8. Utilizza ARIA per migliorare l'accessibilità dove opportuno.
9. Esegui il tuo sito attraverso uno [strumento di audit](#strumenti_di_audit).
10. Provalo con un lettore di schermo.
11. Includi una politica/dichiarazione di accessibilità reperibile sul tuo sito per spiegare cosa hai fatto.

## Riepilogo

Si spera che questo articolo ti abbia dato un'idea dei tipi di strumenti che puoi usare per aiutare a risolvere i problemi di accessibilità, delle tecnologie assistive utilizzate da persone con disabilità per accedere al web.

Nel prossimo articolo esamineremo come scrivere HTML accessibile.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_Accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}
