---
title: Panoramica degli strumenti lato client
short-title: Overview
slug: Learn_web_development/Extensions/Client-side_tools/Overview
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Client-side_tools/Package_management", "Learn_web_development/Extensions/Client-side_tools")}}

In questo articolo, forniamo una panoramica degli strumenti moderni per il web, quali tipi di strumenti sono disponibili, dove li incontrerai nel ciclo di sviluppo delle applicazioni web e come trovare aiuto per strumenti specifici.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Capire quali tipi di strumenti lato client esistono e come
        trovarli e ottenere aiuto con essi.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica sugli strumenti moderni

Scrivere software per il web è diventato più sofisticato nel corso del tempo. Sebbene sia ancora del tutto ragionevole scrivere HTML, CSS e JavaScript "manualmente", ora esiste una vasta gamma di strumenti che gli sviluppatori possono utilizzare per accelerare il processo di costruzione di un sito web o un'app.

Esistono alcuni strumenti estremamente affermati che sono diventati veri e propri "nomi comuni" tra la comunità degli sviluppatori, e nuovi strumenti vengono scritti e rilasciati ogni giorno per risolvere problemi specifici. Potresti persino trovarti a scrivere un software per agevolare il tuo processo di sviluppo, per risolvere un problema specifico che gli strumenti esistenti non sembrano già gestire.

È facile sentirsi sopraffatti dal numero enorme di strumenti che possono essere inclusi in un unico progetto. Allo stesso modo, un unico file di configurazione per uno strumento come [webpack](https://webpack.js.org/) può contenere centinaia di righe, la maggior parte delle quali sono incantesimi magici che sembrano fare il loro lavoro, ma che solo un maestro ingegnere comprenderà pienamente!

Di tanto in tanto, persino gli sviluppatori web più esperti si bloccano su un problema di strumenti; è possibile perdere ore tentando di far funzionare una pipeline di strumenti prima ancora di toccare una singola riga di codice dell'applicazione. Se hai avuto difficoltà in passato, non preoccuparti: non sei solo.

In questi articoli, non risponderemo a tutte le domande sugli strumenti web, ma ti forniremo un punto di partenza utile per comprendere le basi, su cui potrai poi costruire. Come con qualsiasi argomento complesso, è bene iniziare in piccolo e gradualmente passare a utilizzi più avanzati.

## L'ecosistema degli strumenti moderni

L'ecosistema degli strumenti per sviluppatori moderni di oggi è enorme, quindi è utile avere una visione generale dei principali problemi che gli strumenti stanno risolvendo. Se cerchi "strumenti per sviluppatori front-end" sul tuo motore di ricerca preferito, otterrai una vasta gamma di risultati che vanno dagli editor di testo, ai browser, ai tipi di penne che puoi usare per prendere appunti.

Anche se la scelta del tuo editor di codice è certamente una scelta di strumenti, questa serie di articoli andrà oltre, concentrandosi sugli strumenti per sviluppatori che aiutano a produrre codice web più efficiente. Consiglieremo alcuni strumenti particolari e i seguenti tutorial ti mostreranno come usarli. Sono strumenti popolari e standard al momento della scrittura. Ciò non ti esclude dall'utilizzare altri strumenti, se sei consapevole dei loro vantaggi relativi.

Da una prospettiva di alto livello, puoi suddividere gli strumenti lato client nelle seguenti quattro ampie categorie di problemi da risolvere:

- **Ambiente** — Strumenti che ti aiutano a configurare il tuo ambiente di sviluppo, come installare ed eseguire altri strumenti.
- **Rete di sicurezza** — Strumenti utili durante lo sviluppo del tuo codice.
- **Trasformazione** — Strumenti che trasformano il codice in qualche modo, ad esempio, trasformando un linguaggio intermedio in JavaScript che un browser può comprendere.
- **Post-sviluppo** — Strumenti utili dopo che hai scritto il tuo codice, come strumenti di testing e di distribuzione.

Esaminiamo ciascuna di queste categorie in dettaglio.

### Ambiente

L'editor, il sistema operativo e il browser sono tutti ambienti di sviluppo. Presumiamo che tu abbia già fatto una scelta con cui ti senti più a tuo agio. Tuttavia, prima di installare ed eseguire altri strumenti, ci sono ancora due scelte da fare:

- Dove eseguirai gli strumenti. La maggior parte degli strumenti che vengono eseguiti localmente sono scritti in JavaScript, quindi hai bisogno di un interprete JavaScript sul tuo computer che possa essere invocato dalla linea di comando (non quello nel tuo browser). [Node.js](https://nodejs.org/) rimane lo standard industriale e lo useremo. [Bun](https://bun.sh/) è progettato come un sostituto di Node.js, noto per la sua velocità e API potenti.
- Come installerai gli strumenti, in altre parole, il _gestore dei pacchetti_. Node fornisce [npm](https://www.npmjs.com/) di default, quindi lo useremo. [Yarn](https://yarnpkg.com/) e [pnpm](https://pnpm.io/) sono altre scelte popolari, ognuna con i propri vantaggi come velocità, gestione dei progetti, ecc.

### Rete di sicurezza

Questi sono strumenti che migliorano un po' il codice che scrivi.

Questa parte degli strumenti dovrebbe essere specifica per il tuo ambiente di sviluppo, anche se non è raro che le aziende abbiano una sorta di politica o configurazione preconfezionata disponibile per l'installazione in modo che tutti i loro sviluppatori utilizzino gli stessi processi.

Questa categoria include qualsiasi cosa renda il tuo processo di sviluppo più semplice per generare codice stabile e affidabile. Gli strumenti della rete di sicurezza dovrebbero anche aiutarti a prevenire errori o correggere errori automaticamente senza dover ricostruire il tuo codice da zero ogni volta.

Alcuni tipi di strumenti di rete di sicurezza molto comuni che troverai essere utilizzati dagli sviluppatori sono i seguenti.

#### Linters

I **Linters** sono strumenti che controllano il tuo codice e ti informano su eventuali errori presenti, che tipo di errori sono e su quali righe di codice si trovano. Spesso i linters possono essere configurati non solo per segnalare errori, ma anche per segnalare eventuali violazioni di una guida di stile specificata che il tuo team potrebbe utilizzare (ad esempio codice che utilizza il numero sbagliato di spazi per l'indentazione, o utilizza [template literals](/it/docs/Web/JavaScript/Reference/Template_literals) piuttosto che stringhe letterali regolari).

[ESLint](https://eslint.org/) è il linter JavaScript standard nel settore — un strumento altamente configurabile per rilevare potenziali errori di sintassi e incoraggiare le "migliori pratiche" in tutto il tuo codice. Alcune aziende e progetti hanno anche [condiviso le loro configurazioni ESLint](https://www.npmjs.com/search?q=keywords:eslintconfig).

Puoi trovare anche strumenti di linting per altri linguaggi, come [stylelint](https://stylelint.io/).

#### Controllo del codice sorgente

Conosciuto anche come **sistemi di controllo delle versioni** (VCS), il **controllo del codice sorgente** è essenziale per fare il backup del lavoro e lavorare in team. Un VCS tipico prevede l'avere una versione locale del codice su cui apporti modifiche. Poi "pussi" le modifiche a una versione "master" del codice all'interno di un repository remoto memorizzato su un server. Di solito c'è un modo per controllare e coordinare quali modifiche vengono fatte alla copia "master" del codice e quando, in modo che un team di sviluppatori non finisca per sovrascrivere continuamente i lavori degli altri.

[Git](https://git-scm.com/) è il sistema di controllo del codice sorgente che la maggior parte delle persone utilizza oggi. È accessibile principalmente tramite la linea di comando ma può essere accessibile tramite interfacce utente più amichevoli. Con il tuo codice in un repository git, puoi "spingerlo" al tuo server istanze, oppure utilizzare un sito di controllo sorgente ospitato come [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/) o [Bitbucket](https://bitbucket.org/product/).

Utilizzeremo GitHub in questo modulo. Puoi trovare maggiori informazioni a riguardo in [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).

#### Formattatori di codice

I formattatori di codice sono in qualche modo correlati ai linters, tranne che anziché indicare errori nel codice, di solito tendono a garantire che il tuo codice sia formattato correttamente, secondo le tue regole di stile, correggendo automaticamente gli errori che trovano.

[Prettier](https://prettier.io/) è un esempio molto popolare di formattatore di codice, che utilizzeremo successivamente nel modulo.

#### Checker di tipo

I checker di tipo sono strumenti che aiutano a scrivere codice più affidabile controllando che il tuo codice utilizzi correttamente i tipi di dati nei posti giusti. Questo previene classi comuni di bug come l'accesso a proprietà inesistenti, `undefined` inattesi, ecc.

[TypeScript](https://www.typescriptlang.org/) è lo standard di fatto come checker di tipo per JavaScript. Fornisce una propria sintassi di annotazione dei tipi e rappresenta in qualche modo un linguaggio a sé stante, quindi non lo copriremo in questo modulo.

### Trasformazione

Questa fase del ciclo di vita della tua app web di solito ti consente di scrivere codice in "codice futuro" (come le ultime funzionalità CSS o JavaScript che potrebbero non avere ancora supporto nativo nei browser) o di codificare utilizzando un altro linguaggio interamente, come TypeScript. Gli strumenti di trasformazione genereranno quindi codice compatibile con i browser per te, da utilizzare in produzione.

Generalmente, lo sviluppo web è pensato come tre linguaggi: [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting), e ci sono strumenti di trasformazione per tutti questi linguaggi. La trasformazione offre tre principali benefici (tra gli altri):

1. La possibilità di scrivere codice utilizzando le ultime funzionalità del linguaggio e trasformarlo in codice che funziona sui dispositivi di tutti i giorni. Ad esempio, potresti voler scrivere JavaScript utilizzando le nuove funzionalità all'avanguardia del linguaggio, ma avere comunque il tuo codice di produzione finale funzionante su browser più vecchi che non supportano quelle funzionalità. Buoni esempi qui includono:

   - [Babel](https://babeljs.io/): Un compilatore JavaScript che consente agli sviluppatori di scrivere il loro codice utilizzando JavaScript all'avanguardia, che Babel poi converte in JavaScript vecchio stile che più browser possono capire. Gli sviluppatori possono anche scrivere e pubblicare [plugin per Babel](https://babeljs.io/docs/plugins).
   - [PostCSS](https://postcss.org/): Fa lo stesso tipo di cosa di Babel, ma per funzionalità CSS all'avanguardia. Se non esiste un modo equivalente per fare qualcosa utilizzando funzionalità CSS più vecchie, PostCSS installerà un polyfill JavaScript per emulare l'effetto CSS desiderato.

2. L'opzione per scrivere il tuo codice in un linguaggio interamente diverso e trasformarlo in un linguaggio compatibile con il web. Ad esempio:

   - [Sass/SCSS](https://sass-lang.com/): Questa estensione del CSS ti permette di utilizzare variabili, regole nidificate, mixin, funzioni e molte altre funzionalità, alcune delle quali sono disponibili nel CSS nativo (come le variabili), e alcune delle quali non lo sono.
   - [TypeScript](https://www.typescriptlang.org/): TypeScript è un superset di JavaScript che offre una serie di funzionalità aggiuntive. Il compilatore TypeScript converte il codice TypeScript in JavaScript durante la build per la produzione.
   - Frameworks come [React](https://react.dev/), [Ember](https://emberjs.com/) e [Vue](https://vuejs.org/): I framework offrono moltissima funzionalità gratuitamente e ti permettono di utilizzarla tramite una sintassi personalizzata costruita sopra JavaScript puro. In background, il codice JavaScript del framework lavora duramente per interpretare questa sintassi personalizzata e renderla come applicazione web finale.

3. Ottimizzazione. Questa viene fornita dai _bundlers_, che sono strumenti che preparano il tuo codice per la produzione, ad esempio attraverso il "{{Glossary("Tree_shaking", "tree-shaking")}}" per assicurarsi che solo le parti delle librerie di codice che realmente utilizzi vengano messe nel tuo codice di produzione finale, oppure con la "{{Glossary("Minification", "minificazione")}}" per rimuovere tutto lo spazio bianco nel tuo codice di produzione, rendendolo il più piccolo possibile prima che venga caricato su un server. Ad esempio:

   - [Webpack](https://webpack.js.org/) è stato il bundler più popolare per lungo tempo, con un numero enorme di plugin e un sistema di configurazione potente. Tuttavia, è anche noto per essere piuttosto complesso da configurare, ed è lento rispetto ad alternative più moderne.
   - [Vite](https://vite.dev/) è uno strumento di build più moderno che è popolare per la sua velocità, semplicità e ricchezza di funzionalità.

### Post-sviluppo

Gli strumenti post-sviluppo assicurano che il tuo software arrivi sul web e continui a funzionare. Include i processi di distribuzione, i framework di testing, gli strumenti di auditing e altro ancora.

Questa fase del processo di sviluppo è quella con cui desideri avere la minore quantità di interazione attiva possibile, in modo che una volta configurata, funzioni principalmente automaticamente, apparendo solo per dirti qualcosa se qualcosa è andato storto.

#### Strumenti di test

Questi generalmente prendono la forma di uno strumento che eseguirà automaticamente test sul tuo codice per assicurarsi che sia corretto prima che tu vada oltre (ad esempio, quando tenti di spingere le modifiche su un repo di GitHub). Questo può includere il linting, ma anche procedure più sofisticate come i test unitari, dove esegui parte del tuo codice per assicurarti che si comportino come dovrebbero.

- I framework per scrivere test includono [Jest](https://jestjs.io/), [Mocha](https://mochajs.org/) e [Jasmine](https://jasmine.github.io/).
- I sistemi di esecuzione automatica dei test e di notifica includono [Travis CI](https://www.travis-ci.com/), [Jenkins](https://www.jenkins.io/), [Circle CI](https://circleci.com/) e [altri](https://en.wikipedia.org/wiki/List_of_build_automation_software#Continuous_integration).

#### Strumenti di distribuzione

I sistemi di distribuzione ti consentono di pubblicare il tuo sito web, sono disponibili sia per siti statici che dinamici, e comunemente tendono a lavorare insieme ai sistemi di test. Ad esempio, una tipica toolchain attenderà che tu spinga le modifiche su un repo remoto, esegua alcuni test per vedere se le modifiche sono a posto, e quindi, se i test passano, distribuisce automaticamente la tua app su un sito di produzione.

[GitHub Pages](https://pages.github.com/) è ben integrato con GitHub stesso ed è gratuito per tutti i repo pubblici. Altri servizi, come [Netlify](https://www.netlify.com/) e [Vercel](https://vercel.com/), sono anche molto popolari, con quote generose nei livelli gratuiti, workflow di distribuzione fluidi e integrazione con GitHub.

#### Altri

Ci sono diversi altri tipi di strumenti disponibili da usare nella fase di post-sviluppo, tra cui [Code Climate](https://codeclimate.com/) per raccogliere metriche di qualità del codice, la [Webhint browser extension](https://webhint.io/docs/user-guide/extensions/extension-browser/) per eseguire analisi di runtime della compatibilità cross-browser e altri controlli, [GitHub bots](https://probot.github.io/) per fornire funzionalità più potenti su GitHub, [Updown](https://updown.io/) per fornire monitoraggio della disponibilità delle app, e molti altri ancora!

### Alcune considerazioni sui tipi di strumenti

C'è certamente un ordine in cui i diversi tipi di strumenti si applicano nel ciclo di sviluppo, ma stai certo che non _devi_ avere tutti questi strumenti in atto per rilasciare un sito web. In effetti, non hai bisogno di nessuno di questi strumenti. Tuttavia, includere alcuni di questi strumenti nel tuo processo migliorerà la tua esperienza di sviluppo e probabilmente migliorerà la qualità complessiva del tuo codice.

Spesso ci vuole del tempo affinché nuovi strumenti per sviluppatori si stabilizzino nella loro complessità. Uno degli strumenti più noti, webpack, ha la reputazione di essere eccessivamente complicato da gestire, ma nell'ultimo grande rilascio c'è stata una grande spinta per semplificare l'uso comune in modo che la configurazione richiesta sia ridotta al minimo indispensabile.

Non c'è un proiettile d'argento che garantisca il successo con gli strumenti, ma man mano che aumenta la tua esperienza troverai flussi di lavoro che funzionano per _te_ o per il tuo team e i loro progetti. Una volta che tutte le difficoltà nel processo sono appianate, la tua toolchain dovrebbe essere qualcosa che puoi dimenticare e _dovrebbe_ semplicemente funzionare.

## Come scegliere e ottenere aiuto con uno strumento in particolare

La maggior parte degli strumenti tende a essere scritta e rilasciata in isolamento, quindi anche se c'è quasi certamente aiuto disponibile non è mai nello stesso posto o formato. Può quindi essere difficile trovare aiuto con l'uso di uno strumento, o anche scegliere quale strumento utilizzare. La conoscenza su quali siano i migliori strumenti da usare è un po' tribale, il che significa che se non sei già nella comunità web, è difficile scoprire esattamente quali andare a cercare! Questo è uno dei motivi per cui abbiamo scritto questa serie di articoli, per fornire sperabilmente quel primo passo che è difficile da trovare altrimenti.

Probabilmente avrai bisogno di una combinazione delle seguenti cose:

- Insegnanti esperti, mentori, compagni di studio o colleghi che abbiano qualche esperienza, che abbiano risolto tali problemi prima e che possano dare consigli.
- Un posto specifico utile da cui cercare. Le ricerche web generali per strumenti per sviluppatori front-end sono generalmente inutili a meno che tu non conosca già il nome dello strumento che stai cercando.

  - Se stai usando il gestore dei pacchetti `npm` per gestire le tue dipendenze, ad esempio, è una buona idea andare sull'[homepage di npm](https://www.npmjs.com/) e cercare il tipo di strumento che stai cercando, ad esempio prova a cercare "date" se vuoi un'utilità di formattazione delle date, o "formatter" se stai cercando un formattatore di codice generale. Presta attenzione alla popolarità, alla qualità, e ai punteggi di manutenzione, e a quando è stato aggiornato l'ultima volta il pacchetto. Fai anche clic sulle pagine degli strumenti per scoprire quanti download mensili ha un pacchetto e se ha una buona documentazione che puoi usare per capire se fa quello che ti serve che faccia. In base a questi criteri, la [libreria date-fns](https://www.npmjs.com/package/date-fns) sembra essere un buon strumento di formattazione delle date da usare. Vedrai questo strumento in azione e imparerai di più sui gestori di pacchetti in generale nel Capitolo 3 di questo modulo.
  - Se stai cercando un plugin per integrare la funzionalità degli strumenti nel tuo editor di codice, guarda la pagina dei plugin/estensioni dell'editor di codice — vedi ad esempio [VS Code extensions](https://marketplace.visualstudio.com/vscode). Dai un'occhiata alle estensioni in evidenza sulla pagina iniziale e, di nuovo, prova a cercare il tipo di estensione che desideri (o il nome dello strumento, ad esempio cerca "ESLint" sulla pagina delle estensioni di VS Code). Quando ottieni risultati, dai un'occhiata a informazioni come quanti stelle o download ha un'estensione, come indicazione della sua qualità.

- Forum legati allo sviluppo su cui porre domande su quali strumenti utilizzare, ad esempio [MDN Learn Discourse](https://discourse.mozilla.org/c/mdn/learn/250), o [Stack Overflow](https://stackoverflow.com/).

Quando hai scelto uno strumento da usare, il primo punto di riferimento dovrebbe essere la homepage del progetto dello strumento. Questo potrebbe essere un sito web completo o potrebbe essere un singolo documento readme in un repository di codice. Ad esempio, i [documenti di date-fns](https://date-fns.org/docs/Getting-Started) sono piuttosto buoni, completi e facili da seguire. Tuttavia, alcune documentazioni possono essere piuttosto tecniche e accademiche e non adatte alle tue esigenze di apprendimento.

Invece, potresti voler trovare qualche tutorial dedicato per iniziare con determinati tipi di strumenti. Un ottimo punto di partenza è cercare su siti web come [CSS Tricks](https://css-tricks.com/), [Dev](https://dev.to/), [freeCodeCamp](https://www.freecodecamp.org/) e [Smashing Magazine](https://www.smashingmagazine.com/), poiché sono orientati all'industria dello sviluppo web.

Ancora, probabilmente passerai attraverso diversi strumenti mentre cerchi quelli giusti per te, provandoli per vedere se hanno senso, sono ben supportati e fanno ciò che vuoi che facciano. Questo va bene — è tutto utile per l'apprendimento, e la strada diventerà più agevole man mano che acquisirai più esperienza.

## Sommario

Questo conclude la nostra introduzione gentile al tema degli strumenti web lato client, da un alto livello. Il prossimo passo sarà esaminare i gestori di pacchetti.

{{NextMenu("Learn_web_development/Extensions/Client-side_tools/Package_management", "Learn_web_development/Extensions/Client-side_tools")}}
