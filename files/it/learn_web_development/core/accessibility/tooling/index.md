---
title: Strumenti di accessibilità e tecnologia assistiva
short-title: Strumenti di accessibilità
slug: Learn_web_development/Core/Accessibility/Tooling
l10n:
  sourceCommit: eaec5c4226ac64696a95314a7bce995165a4d124
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_Accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}

Proseguendo, passeremo a parlare degli strumenti di accessibilità, fornendo informazioni sui tipi di strumenti che può utilizzare per aiutare a risolvere i problemi di accessibilità e aiutandola a comprendere le **tecnologie assistive** utilizzate dalle persone con disabilità per navigare sul web. Gli strumenti descritti qui verranno utilizzati in articoli successivi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Familiarità con il tipo di strumenti che può utilizzare per aiutare a risolvere problemi di accessibilità, ad esempio strumenti di auditing.</li>
          <li>Impostare gli screen reader e usarli per testare i siti web su desktop e mobile.</li>
          <li>Familiarità con altri tipi di tecnologia assistiva come grandi tastiere in braille, dispositivi di puntamento alternativi e ingranditori di schermo.</li>
          <li>L'importanza del testing con gli utenti insieme al test automatizzato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti di accessibilità

Esaminiamo gli strumenti e le tecniche che può utilizzare per testare l'accessibilità del sito web e risolvere i problemi che scopre.

### Test dell'ordine della sorgente

Il suo contenuto dovrebbe avere senso logico nel suo ordine sorgente: può sempre visualizzarlo diversamente con CSS in seguito, ma dovrebbe iniziare con la struttura sottostante corretta. Questo perché le tecnologie assistive leggono il contenuto del sito web basandosi sull'ordine della sorgente e le persone con disabilità spesso modificano o disattivano parti del CSS per rendere il contenuto più leggibile (esempi comuni sono l'aumento della dimensione del carattere e l'applicazione di schemi di colore ad alto contrasto).

Per testare l'ordine della sorgente, può disattivare il CSS di un sito e vedere quanto sia comprensibile senza di esso. Potrebbe farlo manualmente rimuovendo il CSS dal suo codice, ma il modo più semplice è utilizzare le funzionalità del browser, ad esempio:

- Firefox: Selezioni _Visualizza > Stile pagina > Nessuno stile_ dal menu principale.
- Safari: [Apra gli strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser), faccia clic sul pulsante _Impostazioni dispositivo_ vicino all'angolo in alto a sinistra del pannello degli strumenti per sviluppatori (che sembra un monitor del computer), quindi selezioni la casella "Disabilita CSS" nel pannello che appare.
- Chrome/Edge: Installi l'estensione [Web Developer Toolbar](https://chromewebstore.google.com/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm), quindi riavvii il browser. Faccia clic sull'icona dell'ingranaggio "Web Developer" che dovrebbe ora essere disponibile nel menu delle estensioni, poi selezioni _CSS > Disabilita tutti gli stili_.

### Controllori di contrasto di colore

Quando sceglie uno schema di colore per il suo sito web, dovrebbe assicurarsi che il colore del testo (in primo piano) contrasti bene con il colore di sfondo. Il suo design potrebbe sembrare interessante, ma non serve a nulla se le persone non possono leggere il suo contenuto. Usi uno strumento come il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM per verificare se il suo schema è sufficientemente contrastante.

Un altro consiglio è evitare di usare solo il colore per segnalazioni o per evidenziare informazioni importanti, poiché ciò potrebbe essere trascurato dalle persone con disabilità visive come il daltonismo. Invece di contrassegnare i campi obbligatori in rosso, ad esempio, li contrassegni con un asterisco e in rosso.

> [!NOTE]
> Un alto rapporto di contrasto consentirà anche a chi utilizza uno smartphone o tablet con uno schermo lucido di leggere meglio le pagine in un ambiente luminoso, come alla luce del sole.

### Strumenti di auditing

Esistono diversi strumenti di auditing disponibili in cui può inserire le sue pagine web. Li esamineranno e restituiranno un elenco di problemi di accessibilità presenti sulla pagina. Prendiamo ad esempio [Wave](https://wave.webaim.org/), uno strumento online di testing dell'accessibilità che accetta un indirizzo web e restituisce una vista annotata di quella pagina con i problemi di accessibilità evidenziati.

1. Visiti la [pagina principale di Wave](https://wave.webaim.org/).
2. Inserisca l'URL del nostro esempio [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html) nella casella di input di testo vicino alla parte superiore della pagina. Poi prema invio o faccia clic/tacchi l'arco sul lato destro dell'input box.
3. Il sito dovrebbe evidenziare i problemi di accessibilità presenti. Faccia clic sulle icone visualizzate per vedere maggiori informazioni su ciascuno dei problemi identificati dalla valutazione di Wave.

Altri strumenti di auditing che meritano di essere esaminati:

- [Inspector di accessibilità di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html)
- [Bookmarklet ANDI](https://www.ssa.gov/accessibility/andi/help/install.html)
- [Audit di accessibilità di Google Lighthouse](https://developer.chrome.com/docs/lighthouse/accessibility/scoring)

> [!NOTE]
> Tali strumenti non sono sufficienti per risolvere tutti i suoi problemi di accessibilità da soli. Avrà bisogno di una combinazione di questi strumenti, conoscenza ed esperienza, test con utenti, ecc. per ottenere un quadro completo.

[Lo strumento aXe di Deque](https://www.deque.com/axe/) va un po' oltre rispetto agli strumenti di auditing menzionati sopra. Come gli altri, controlla le pagine e restituisce errori di accessibilità. La forma più immediatamente utile è probabilmente le estensioni del browser:

- [aXe per Chrome](https://chromewebstore.google.com/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd)
- [aXe per Firefox](https://addons.mozilla.org/en-US/firefox/addon/axe-devtools/)

Queste aggiungono una scheda di accessibilità agli strumenti per sviluppatori del browser. Ad esempio, abbiamo installato la versione di Firefox, poi l'abbiamo utilizzata per auditare il nostro esempio [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html). Abbiamo ottenuto i seguenti risultati:

![Uno screenshot dei problemi di accessibilità identificati dallo strumento Axe.](axe-screenshot.png)

aXe è anche installabile utilizzando `npm` e può essere integrato con task runner come [Grunt](https://gruntjs.com/) e [Gulp](https://gulpjs.com/), framework di automazione come [Selenium](https://www.selenium.dev/) e [Cucumber](https://cucumber.io/), framework di test unitari come [Jasmine](https://jasmine.github.io/) e altro ancora (per ulteriori dettagli, consulti la [pagina principale di aXe](https://www.deque.com/axe/)).

## Screen reader

Uno dei tipi di tecnologia assistiva (AT) più comuni che le persone con disabilità utilizzano – e uno dei più comuni che utilizzerà per testare l'accessibilità delle sue pagine web – sono gli **screen reader**. Questi sono software che leggono il contenuto delle pagine web o il contenuto di altre applicazioni installate sul sistema operativo di qualcuno. Gli screen reader consentono alle persone di usare i computer senza dover vedere alcun contenuto visivo.

I browser web espongono informazioni sul contenuto della pagina per gli screen reader (e altre AT) per comunicare con l'utente attraverso una rappresentazione chiamata {{Glossary("Accessibility_tree", "albero di accessibilità")}}. Questo fornisce informazioni semantiche come nomi e descrizioni degli elementi, quale sia il loro scopo o ruolo (è un pulsante o un campo di input?) e se sono in uno stato particolare (ad esempio, una finestra di dialogo è aperta o chiusa?).

Queste informazioni potrebbero essere banali nel caso di un paragrafo di testo, che suona praticamente come è scritto, ma può diventare complicato quando si tratta di funzionalità dell'interfaccia utente come un menu a tendina o un lettore video. Questo è il motivo per cui è molto importante utilizzare correttamente l'HTML semantico, di cui parlerà in dettaglio nel prossimo articolo di questo modulo. Se contrassegna il contenuto utilizzando l'elemento sbagliato, può confondere gli utenti di screen reader.

Assicuri di avere uno o due screen reader installati sulla sua macchina di sviluppo e provi a utilizzare i suoi siti web preferiti tramite uno screen reader, come discusso di seguito. Capire come le persone ipovedenti utilizzano il web è fondamentale per progettare prodotti che funzionino meglio per tutti.

### Quali screen reader sono disponibili?

Sono disponibili diversi screen reader:

- Alcuni sono prodotti commerciali a pagamento, come [JAWS](https://www.freedomscientific.com/Products/software/JAWS/) (Windows).
- Alcuni sono prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome, Windows e macOS) e [Orca](https://wiki.gnome.org/Projects/Orca) (Linux).
- Alcuni sono integrati nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/features/?vision) (macOS e iOS), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (su Chromebook) e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

In generale, gli screen reader sono app separate che funzionano sul sistema operativo host e possono leggere pagine web e contenuti in altre app (non è sempre il caso; ChromeVox ad esempio è un'estensione del browser). Gli screen reader tendono ad avere alcune differenze nel comportamento e nei controlli esatti, quindi dovrà consultare la documentazione del suo screen reader scelto per ottenere tutti i dettagli. Detto ciò, funzionano tutti fondamentalmente nello stesso modo.

Nelle prossime sezioni, passeremo attraverso alcuni test con un paio di screen reader diversi per darle un'idea generale di come funzionano e come testarli.

> [!NOTE] > [Progettare per la compatibilità degli screen reader](https://webaim.org/techniques/screenreader/) di WebAIM fornisce alcune informazioni utili sull'uso degli screen reader e su cosa funziona meglio per loro. Veda anche [Risultati dell'indagine sugli utenti di screen reader #10](https://webaim.org/projects/screenreadersurvey10/#used) per alcune interessanti statistiche sull'uso degli screen reader.

#### VoiceOver

VoiceOver (VO) è disponibile gratuitamente con Apple mac/iPhone/iPad, quindi è utile per il test su desktop/mobile se utilizza prodotti Apple. Lo abbiamo testato su macOS su un MacBook Pro.

Per attivarlo, prema <kbd>Cmd</kbd> + <kbd>F5</kbd>. Se non ha mai usato VO prima, verrà visualizzata una schermata di benvenuto in cui può scegliere di avviare VO o meno e passare attraverso un tutorial piuttosto utile per imparare a usarlo. Per disattivarlo, prema di nuovo <kbd>Cmd</kbd> + <kbd>F5</kbd>.

> [!NOTE]
> Dovrebbe passare attraverso il tutorial almeno una volta – è un modo davvero utile per imparare VO.

Quando VO è attivato, il display apparirà sostanzialmente lo stesso, ma vedrà una casella nera nell'angolo in basso a sinistra dello schermo che contiene informazioni su ciò che VO ha attualmente selezionato. La selezione corrente sarà anche evidenziata, con un bordo nero - questo evidenziatore è noto come **cursore VO**.

![Uno screenshot di esempio che dimostra il testing dell'accessibilità utilizzando VoiceOver sulla homepage di MDN. In basso a sinistra nell'immagine è evidenziata l'informazione selezionata sulla pagina web.](voiceover.png)

Per usare VO, farà ampio uso del "modificatore VO" - questo è un tasto o una combinazione di tasti che deve premere in aggiunta ai veri e propri collegamenti di tastiera VO per farli funzionare. L'uso di un modificatore come questo è comune con gli screen reader, per consentire loro di mantenere i loro comandi distinti dagli altri comandi. Nel caso di VO, il modificatore può essere <kbd>CapsLock</kbd> o <kbd>Ctrl</kbd> + <kbd>Opzione</kbd>.

VO ha molti comandi da tastiera e non li elencheremo tutti qui. Quelli di base di cui avrà bisogno per il test delle pagine web sono nella seguente tabella. Nei collegamenti da tastiera, "VO" significa "il modificatore VoiceOver".

<table class="standard-table no-markdown">
  <caption>
    Collegamenti da tastiera comuni di VoiceOver
  </caption>
  <thead>
    <tr>
      <th scope="col">Collegamento da tastiera</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VO + tasti cursore</td>
      <td>Sposta il cursore VO su, destra, giù, sinistra.</td>
    </tr>
    <tr>
      <td>VO + Barra spaziatrice</td>
      <td>
        Seleziona/attiva elementi evidenziati dal cursore VO. Questo include elementi
        selezionati nel Rotor (vedi sotto).
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + cursore giù</td>
      <td>
        Entra in un gruppo di elementi come una tabella HTML o un modulo. Una volta
        all'interno di un gruppo, può spostarsi e selezionare elementi all'interno di quel gruppo
        utilizzando i comandi di cui sopra come al solito.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>Shift</kbd> + cursore su</td>
      <td>Esci da un gruppo.</td>
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
      <td>VO + <kbd>C</kbd> + <kbd>C</kbd> (due C in successione)</td>
      <td>
        (quando all'interno di una tabella) Leggi l'intera colonna corrente, inclusa l'intestazione.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>R</kbd> + <kbd>R</kbd> (due R in successione)</td>
      <td>
        (quando all'interno di una tabella) Leggi l'intera riga corrente, incluse le intestazioni
        che corrispondono a ciascuna cella.
      </td>
    </tr>
    <tr>
      <td>VO + cursore sinistra, VO + cursore destra</td>
      <td>
        (quando all'interno di alcune opzioni orizzontali, come un selettore di data)
        Spostati tra le opzioni.
      </td>
    </tr>
    <tr>
      <td>VO + cursore su, VO + cursore giù</td>
      <td>
        (quando all'interno di alcune opzioni orizzontali, come un selettore di data)
        Cambia l'opzione corrente.
      </td>
    </tr>
    <tr>
      <td>VO + <kbd>U</kbd></td>
      <td>
        Apri il Rotor, che visualizza elenchi di intestazioni, collegamenti, controlli del modulo,
        ecc. per una navigazione facile.
      </td>
    </tr>
    <tr>
      <td>VO + cursore sinistra, VO + cursore destra</td>
      <td>
        (quando all'interno del Rotor) Spostati tra diversi elenchi disponibili nel Rotor.
      </td>
    </tr>
    <tr>
      <td>VO + cursore su, VO + cursore giù</td>
      <td>
        (quando all'interno del Rotor) Spostati tra diversi elementi nell'elenco corrente del Rotor.
      </td>
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
      <td>Riavvia l'ultimo discorso.</td>
    </tr>
    <tr>
      <td>VO + <kbd>D</kbd></td>
      <td>Accedi al Dock del mac, così può selezionare app da eseguire al suo interno.</td>
    </tr>
  </tbody>
</table>

Sembra un sacco di comandi, ma non è così male quando ci si abitua e VO le darà regolarmente promemoria sui comandi da usare in certi posti. Provi VO ora; poi può passare a provare alcuni dei nostri esempi nella sezione [Test con screen reader](#test_con_screen_reader).

#### NVDA

NVDA è disponibile solo per Windows e dovrà installarlo.

1. Scarichi NVDA da [nvaccess.org](https://www.nvaccess.org/), quindi lo installi. Può scegliere se fare una donazione o scaricarlo gratuitamente; dovrà anche fornire il suo indirizzo email prima di poterlo scaricare.
2. Per avviare NVDA una volta installato, faccia doppio clic sul file del programma/scorciatoia o usi il collegamento da tastiera <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>N</kbd>. Vedrà la finestra di benvenuto di NVDA quando lo avvia. Qui può scegliere tra un paio di opzioni, quindi premere il pulsante _OK_ per iniziare.

NVDA sarà ora attivo sul suo computer.

Per utilizzare NVDA, farà largo uso del "modificatore NVDA" – il tasto che deve premere in aggiunta ai veri e propri collegamenti da tastiera NVDA per farli funzionare. Il modificatore NVDA può essere <kbd>Insert</kbd> (impostazione predefinita), o <kbd>CapsLock</kbd> (può essere scelto selezionando la prima casella di controllo nella finestra di benvenuto di NVDA prima di premere _OK_).

> [!NOTE]
> NVDA è più sottile di VoiceOver in termini di come evidenzia dove si trova e cosa sta facendo. Quando scorre tra intestazioni, elenchi, ecc., gli elementi su cui è selezionato saranno generalmente evidenziati con un contorno sottile, ma non è sempre così per tutte le cose. Se si perde completamente, può premere Ctrl + F5 per aggiornare la pagina corrente e iniziare di nuovo dall'alto.

NVDA ha molti comandi da tastiera, e non li elencheremo tutti qui. Quelli di base di cui avrà bisogno per il test delle pagine web sono nella seguente tabella. Nei collegamenti da tastiera, "NVDA" significa "il modificatore NVDA".

<table class="standard-table no-markdown">
  <caption>
    Collegamenti da tastiera più comuni di NVDA
  </caption>
  <thead>
    <tr>
      <th scope="col">Collegamento da tastiera</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>NVDA + <kbd>Q</kbd></td>
      <td>Disattivi NVDA di nuovo dopo averlo avviato.</td>
    </tr>
    <tr>
      <td>NVDA + cursore su</td>
      <td>Leggi la riga corrente.</td>
    </tr>
    <tr>
      <td>NVDA + cursore giù</td>
      <td>Inizia a leggere dalla posizione corrente.</td>
    </tr>
    <tr>
      <td>Cursore su e cursore giù, o <kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>Vai all'elemento precedente/successivo sulla pagina e leggilo.</td>
    </tr>
    <tr>
      <td>Cursore sinistra e cursore destra</td>
      <td>Vai al carattere precedente/successivo nell'elemento corrente e leggilo.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>H</kbd> e <kbd>H</kbd></td>
      <td>Vai all'intestazione precedente/successiva e leggila.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>K</kbd> e <kbd>K</kbd></td>
      <td>Vai al collegamento precedente/successivo e leggilo.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>D</kbd> e <kbd>D</kbd></td>
      <td>
        Vai al landmark precedente/successivo del documento (es. <code>&#x3C;nav></code>)
        e leggilo.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>1</kbd>–<kbd>6</kbd> e <kbd>1</kbd>–<kbd>6</kbd></td>
      <td>Vai all'intestazione precedente/successiva (livello 1-6) e leggila.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>F</kbd> e <kbd>F</kbd></td>
      <td>Vai all'input del modulo precedente/successivo e mettici a fuoco.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>T</kbd> e <kbd>T</kbd></td>
      <td>Vai alla tabella di dati precedente/successiva e mettici a fuoco.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>B</kbd> e <kbd>B</kbd></td>
      <td>Vai al pulsante precedente/successivo e leggi la sua etichetta.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>L</kbd> e <kbd>L</kbd></td>
      <td>Vai all'elenco precedente/successivo e leggi il suo primo elemento.</td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>I</kbd> e <kbd>I</kbd></td>
      <td>Vai all'elemento dell'elenco precedente/successivo e leggilo.</td>
    </tr>
    <tr>
      <td><kbd>Enter</kbd>/<kbd>Return</kbd></td>
      <td>
        (quando il collegamento/pulsante o altro elemento attivabile è selezionato) Attiva l'elemento.
      </td>
    </tr>
    <tr>
      <td>NVDA + <kbd>Barra spaziatrice</kbd></td>
      <td>
        (quando il modulo è selezionato) Inserisci il modulo per selezionare elementi individuali,
        o esci dal modulo se ci sei già dentro.
      </td>
    </tr>
    <tr>
      <td><kbd>Shift</kbd> + <kbd>Tab</kbd> e <kbd>Tab</kbd></td>
      <td>(quando all'interno del modulo) Muoviti tra gli input del modulo.</td>
    </tr>
    <tr>
      <td>Cursore su e cursore giù</td>
      <td>
        (quando all'interno del modulo) Cambia i valori dell'input del modulo (nel caso di controlli come
        box di selezione).
      </td>
    </tr>
    <tr>
      <td><kbd>Barra spaziatrice</kbd></td>
      <td>(quando all'interno del modulo) Seleziona il valore scelto.</td>
    </tr>
    <tr>
      <td><kbd>Ctrl</kbd> + <kbd>Alt</kbd> + tasti cursore</td>
      <td>(quando una tabella è selezionata) Spostati tra le celle della tabella.</td>
    </tr>
  </tbody>
</table>

### Test con screen reader

Ora che si è abituato a usare uno screen reader, desideriamo che lo utilizzi per fare alcuni test rapidi di accessibilità, per avere un'idea di come gli screen reader gestiscono le buone e le cattive funzionalità delle pagine web:

- Guardi [good-semantics.html](https://mdn.github.io/learning-area/accessibility/html/good-semantics.html) e noti come le intestazioni sono trovate dallo screen reader e disponibili per la navigazione. Ora guardi [bad-semantics.html](https://mdn.github.io/learning-area/accessibility/html/bad-semantics.html) e noti come lo screen reader non riceve nessuna di queste informazioni. Immagini quanto sarebbe fastidioso quando si cerca di navigare in una pagina molto lunga di testo.
- Guardi [good-links.html](https://mdn.github.io/learning-area/accessibility/html/good-links.html) e noti come abbiano senso quando vengono visualizzati fuori contesto, ad esempio nel Rotor di VoiceOver. Questo non è il caso di [bad-links.html](https://mdn.github.io/learning-area/accessibility/html/bad-links.html) – sono tutti solo "fai clic qui".
- Guardi [good-form.html](https://mdn.github.io/learning-area/accessibility/html/good-form.html) e noti come gli input del modulo siano descritti utilizzando le loro etichette perché abbiamo aggiunto gli appropriati elementi {{htmlelement("label")}}. In [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html), ottengono un'etichetta poco utile come "vuoto".
- Guardi il nostro esempio [punk-bands-complete.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html) e veda come lo screen reader è in grado di associare colonne e righe di contenuto e leggerle tutte insieme perché abbiamo definito correttamente le intestazioni della tabella. In [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html), nessuna delle celle può essere associata. Si noti che NVDA sembra comportarsi in modo leggermente strano quando ha solo una singola tabella su una pagina; potrebbe provare invece la pagina di test della tabella di [WebAIM](https://webaim.org/articles/nvda/tables.htm).
- Provi a guardare l'esempio di [WAI-ARIA live regions](https://www.freedomscientific.com/SurfsUp/AriaLiveRegions.htm) e noti come lo screen reader continuerà a leggere la sezione che si aggiorna costantemente man mano che si aggiorna.

## Altri strumenti

Gli screen reader sono uno dei tipi più comuni di tecnologia assistiva che incontrerà come sviluppatore web, ma esistono altri tipi di AT, ed è utile familiarizzare con ciò che gli utenti usano potenzialmente per accedere ai suoi contenuti. Questa sezione riassume alcuni di essi.

### Tastiere a grandi caratteri o in braille

È possibile ottenere tastiere a caratteri grandi progettate per essere utilizzate da utenti ipovedenti o anziani e tastiere in braille progettate per essere utilizzabili da persone cieche e ipovedenti gravi.

### Dispositivi di puntamento alternativi

Quando pensa ai dispositivi di puntamento, i mouse sono l'esempio ovvio, ma ci sono altri dispositivi di puntamento progettati per consentire agli utenti con diversi tipi di disabilità motoria di navigare più facilmente nelle interfacce utente:

- Trackball: simili ai mouse capovolti, le trackball consistono in una sfera montata che rimane ferma sulla scrivania, che può essere fatta rotolare per muovere il cursore. Sono considerati più precisi e facili da maneggiare rispetto ai mouse, specialmente per persone con movimenti limitati della mano.
- Joystick: Un controllo che può essere spostato per muovere il cursore. I joystick sono meno precisi delle trackball ma utilizzabili da persone con una vasta gamma di disabilità fisiche, anche gravi.
- Touchpad: La maggior parte dei moderni laptop ha un touchpad (a volte chiamato trackpad) — un sensore tattile piatto che le permette di muovere il cursore con un dito, così come eseguire gesti con più dita nello stesso modo dei gesti mobili. Può acquistare touchpad esterni per dispositivi che non ne hanno di interni. Alcuni li trovano più precisi dei mouse.

### Ingranditori di schermo

Gli ingranditori di schermo forniscono agli utenti ipovedenti una vista ingrandita del display del loro dispositivo per consentire loro di comprendere e interagire più facilmente con il contenuto del dispositivo, nonché fornire altre funzionalità come la regolazione del colore per aiutare nel daltonismo e l'adattamento delle dimensioni dei cursori del mouse e dei cursori di testo per renderli più facili da vedere.

Gli ingranditori di schermo software e hardware sono disponibili:

- La maggior parte dei moderni sistemi operativi ha un'app integrata per ingrandire l'intero schermo o parte di esso, ad esempio Zoom su mac o Magnifier su Windows. Tendono anche a fornire opzioni per aumentare universalmente la dimensione del testo, la dimensione del cursore del mouse, ecc. Sono disponibili anche opzioni di terze parti.
- Gli ingranditori di schermo hardware tendono a consistere in uno schermo separato che si trova accanto o di fronte allo schermo del suo dispositivo e proietta una versione più grande di esso, o una versione ingrandita di una parte di esso.

### Software di riconoscimento vocale

Il software di riconoscimento vocale (o di riconoscimento del discorso) consente di impartire comandi vocali per controllare il dispositivo e/o di dettare il testo di email o documenti lasciando che il computer scriva il testo per lei. Questo è molto utile per le persone che non possono utilizzare una tastiera o altri meccanismi di controllo.

I moderni sistemi operativi hanno funzionalità integrate per abilitare questo (ad esempio, Dettatura su mac o Accesso vocale su Windows), e sono disponibili anche app di terze parti, dalle app desktop alle estensioni del browser.

### Controlli a interruttore

I controlli a interruttore forniscono un meccanismo per interagire con i dispositivi per gli utenti con mobilità molto limitata o [cognitive impairment](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility).

Un setup di controllo a interruttore di solito coinvolge due parti:

- Un interruttore fisico o un pulsante per attivare le opzioni sul dispositivo. Può anche assegnare funzionalità a interruttore ai tasti normali del dispositivo (come i controlli del volume) o ai tasti su una tastiera.
- Una modalità di dispositivo o un add-on software di terze parti che rende il dispositivo compatibile con il controllo a interruttore o pulsante. Ad esempio, Accesso tramite interruttore su Android è una modalità in cui le diverse opzioni in varie situazioni (ad esempio, le app sulla schermata iniziale) vengono ciclate e poi quella desiderata può essere selezionata con un pulsante o interruttore quando raggiunta.

## Pianificazione per l'accessibilità

Dovrebbe pensare attentamente all'accessibilità quasi dall'inizio di ogni progetto. Assicurarsi che l'accessibilità sia considerata durante la fase iniziale di progettazione, in modo da poter:

- Fare le basi bene, ad esempio utilizzando una [buona struttura del documento](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_well-structured_text_content) e fornendo [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) per le immagini.
- Considerare attentamente il miglior approccio per le funzionalità che probabilmente avranno problemi di accessibilità. Ad esempio, audio e video saranno sicuramente inaccessibili a alcune persone, quindi dovrebbe fornire alternative come [trascrizioni](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#audio_transcripts) e [tracce di testo](/it/docs/Learn_web_development/Core/Accessibility/Multimedia#video_text_tracks).
- Evitare costosi errori successivamente. I problemi scoperti verso la fine di un progetto tendono a essere molto più costosi in termini di tempo e denaro per essere risolti rispetto ai problemi individuati precocemente.

## Test con gli utenti

Non può fare affidamento solo sugli strumenti automatici per determinare i problemi di accessibilità sul suo sito. Ogni progetto web necessita di una [strategia di test con gli utenti](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies#user_testing) e si consiglia vivamente di includere alcuni gruppi di utenti con disabilità:

- Cerchi di coinvolgere alcuni utenti di screen reader, alcuni utenti solo tastiera, alcuni utenti non udenti, utenti con disabilità motorie, ecc.
- Inviti ogni gruppo a provare a utilizzare il sito web in generale, iniziando con la visualizzazione della pagina principale e di altre pagine principali, e provando alcune delle funzionalità principali. Esempi tipici includono l'acquisto di un prodotto o una prenotazione. Chieda loro quali erano le loro impressioni e quali problemi hanno incontrato.
- Successivamente, faccia concentrare sulle funzionalità o sui flussi di lavoro con cui ha specifiche preoccupazioni di accessibilità, come i controlli complessi del modulo o i lettori video. Chieda loro cosa manca in termini di esperienza utente e cosa vorrebbero vedere cambiato.

Alcuni progetti avranno un budget per pagare i gruppi di test, mentre altri si affideranno a volontari non pagati o persino a colleghi e amici.

## Lista di controllo del testing dell'accessibilità

Il seguente elenco fornisce una lista di controllo per seguirla e assicurarsi di aver effettuato il testing di accessibilità raccomandato per il suo progetto:

1. Assicurarsi che il suo HTML sia il più semanticamente corretto possibile. [Validarlo](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML#html_validation) è un buon inizio, come lo è l'utilizzo di uno [strumento di auditing](#strumenti_di_auditing).
2. Verifichi che il suo contenuto abbia senso quando il CSS è disattivato.
3. Si assicuri che la sua funzionalità sia accessibile tramite tastiera (vedere [Usare i controlli UI semantici dove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) per maggiori dettagli). Testare usando Tab, Return/Enter, ecc.
4. Assicurarsi che il suo contenuto non testuale abbia [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives). Uno [strumento di auditing](#strumenti_di_auditing) è utile per identificare tali problemi.
5. Verifichi che il contrasto dei colori del suo sito sia accettabile, utilizzando uno strumento di controllo appropriato.
6. Assicurarsi che il [contenuto nascosto](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#hiding_things) sia visibile dagli screen reader.
7. Assicurarsi che la funzionalità sia utilizzabile senza JavaScript dove possibile.
8. Utilizzi ARIA per migliorare l'accessibilità dove appropriato.
9. Esegua il suo sito tramite uno [strumento di auditing](#strumenti_di_auditing).
10. Lo testi con uno screen reader.
11. Includa una politica/dichiarazione di accessibilità da qualche parte facilmente reperibile sul suo sito per dire cosa è stato fatto.

## Riepilogo

Ci auguriamo che questo articolo le abbia dato un'idea delle tipologie di strumenti che può usare per aiutare a risolvere i problemi di accessibilità, le tecnologie assistive utilizzate dalle persone con disabilità per accedere al web.

Nel prossimo articolo vedremo come scrivere HTML accessibile.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/What_is_Accessibility","Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility")}}
