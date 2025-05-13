---
title: Panoramica degli strumenti lato client
short-title: Overview
slug: Learn_web_development/Extensions/Client-side_tools/Overview
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Client-side_tools/Package_management", "Learn_web_development/Extensions/Client-side_tools")}}

In questo articolo, forniamo una panoramica degli strumenti web moderni, quali tipi di strumenti sono disponibili e dove incontrarli nel ciclo di vita dello sviluppo di app web, e come trovare aiuto con strumenti individuali.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere quali tipi di strumenti lato client ci sono, e come trovare strumenti e ricevere aiuto su di essi.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica degli strumenti moderni

Scrivere software per il web è diventato più sofisticato col tempo. Sebbene sia ancora del tutto ragionevole scrivere HTML, CSS e JavaScript "a mano", esiste ora una ricchezza di strumenti che gli sviluppatori possono utilizzare per accelerare il processo di creazione di un sito web o di un'app.

Ci sono alcuni strumenti estremamente ben consolidati che sono diventati nomi familiari tra la comunità di sviluppo, e nuovi strumenti vengono creati e rilasciati ogni giorno per risolvere problemi specifici. Potrebbe anche capitarle di scrivere un pezzo di software per aiutare il suo stesso processo di sviluppo, per risolvere un problema specifico che gli strumenti esistenti non sembrano ancora gestire.

È facile sentirsi sopraffatti dal numero elevato di strumenti che possono essere inclusi in un singolo progetto. Allo stesso modo, un singolo file di configurazione per uno strumento come [webpack](https://webpack.js.org/) può essere lungo centinaia di righe, la maggior parte delle quali sono incantesimi magici che sembrano fare il loro lavoro ma che solo un ingegnere esperto comprenderà appieno!

Di tanto in tanto, anche gli sviluppatori web più esperti rimangono bloccati su un problema di strumenti; è possibile sprecare ore cercando di far funzionare una pipeline di strumenti prima ancora di toccare una singola riga di codice dell'applicazione. Se in passato ha avuto difficoltà, non si preoccupi — non è sola.

In questi articoli, non risponderemo a ogni domanda riguardante gli strumenti web, ma le forniremo un punto di partenza utile per comprendere i fondamenti, da cui poi potrà sviluppare ulteriormente. Come con qualsiasi argomento complesso, è bene iniziare in piccolo e poi, gradualmente, passare a usi più avanzati.

## L'ecosistema degli strumenti moderni

L'ecosistema odierno degli strumenti di sviluppo è enorme, quindi è utile avere un'idea generale sui principali problemi che gli strumenti stanno risolvendo. Se si avvicina al suo motore di ricerca preferito e cerca "strumenti per sviluppatori front-end", troverà una vasta gamma di risultati che vanno dagli editor di testo, ai browser, al tipo di penne che può usare per prendere appunti.

Anche se la sua scelta di editor di codice è certamente una scelta di strumenti, questa serie di articoli andrà oltre, concentrandosi sugli strumenti per sviluppatori che l'aiutano a produrre codice web in modo più efficiente. Consiglieremo alcuni strumenti particolari e i prossimi tutorial le mostreranno come usarli. Sono strumenti popolari e standard al momento della scrittura. Ciò non le impedisce di utilizzare altri strumenti, se è consapevole dei loro vantaggi relativi.

Da una prospettiva di alto livello, può suddividere gli strumenti lato client nelle seguenti quattro categorie generali di problemi da risolvere:

- **Ambiente** — Strumenti che l'aiutano a configurare l'ambiente di sviluppo, come l'installazione e l'esecuzione di altri strumenti.
- **Rete di sicurezza** — Strumenti utili durante lo sviluppo del suo codice.
- **Trasformazione** — Strumenti che trasformano il codice in qualche modo, ad esempio, convertendo un linguaggio intermedio in JavaScript che un browser può comprendere.
- **Post-sviluppo** — Strumenti utili dopo aver scritto il suo codice, come strumenti di test e distribuzione.

Esaminiamo ciascuna di queste categorie in modo più dettagliato.

### Ambiente

L'editor, il sistema operativo e il browser sono tutti ambienti di sviluppo. Supporremo che lei abbia già fatto una scelta con cui si sente più a suo agio. Tuttavia, prima di installare ed eseguire altri strumenti, ci sono due scelte ancora da fare:

- Dove eseguire gli strumenti. La maggior parte degli strumenti che vengono eseguiti localmente sono scritti in JavaScript, quindi ha bisogno di un interprete JavaScript sul suo computer che possa essere invocato dalla riga di comando (non quello nel suo browser). [Node.js](https://nodejs.org/) rimane lo standard del settore e lo useremo. [Bun](https://bun.sh/) è progettato come un sostituto per Node.js, noto per la sua velocità e API potenti.
- Come installare gli strumenti, in altre parole, il _gestore di pacchetti_. Node fornisce [npm](https://www.npmjs.com/) di default, quindi lo useremo. [Yarn](https://yarnpkg.com/) e [pnpm](https://pnpm.io/) sono altre scelte popolari, ciascuna con i propri vantaggi come la velocità, la gestione dei progetti, ecc.

### Rete di sicurezza

Questi sono strumenti che rendono il codice che scrive un po' migliore.

Questa parte degli strumenti dovrebbe essere specifica per il suo ambiente di sviluppo, anche se non è raro che le aziende abbiano una qualche politica o configurazione preconfezionata disponibile per l'installazione in modo che tutti i loro sviluppatori utilizzino gli stessi processi.

Questo include qualsiasi cosa che renda più facile il suo processo di sviluppo per generare codice stabile e affidabile. Gli strumenti di rete di sicurezza dovrebbero anche aiutarla a prevenire errori o correggerli automaticamente senza dover costruire il suo codice da zero ogni volta.

Alcuni tipi di strumenti di rete di sicurezza molto comuni che troverà in uso dai sviluppatori sono i seguenti.

#### Linters

I **Linters** sono strumenti che controllano il suo codice e le dicono se sono presenti errori, di che tipo sono e su quali righe di codice si trovano. Spesso i linters possono essere configurati per non solo riportare errori, ma anche riportare qualsiasi violazione di una guida di stile specificata che il suo team potrebbe utilizzare (ad esempio codice che usa il numero sbagliato di spazi per l'indentazione, o l'uso di [template literals](/it/docs/Web/JavaScript/Reference/Template_literals) invece di stringhe regolari).

[ESLint](https://eslint.org/) è il linter JavaScript standard del settore — uno strumento altamente configurabile per individuare potenziali errori di sintassi e incoraggiare le "best practices" nel suo codice. Alcune aziende e progetti hanno anche [condiviso le loro configurazioni ESLint](https://www.npmjs.com/search?q=keywords:eslintconfig).

Può trovare anche strumenti di linting per altri linguaggi, come [stylelint](https://stylelint.io/).

#### Controllo del codice sorgente

Conosciuto anche come **sistemi di controllo delle versioni** (VCS), il **controllo del codice sorgente** è essenziale per fare il backup del lavoro e lavorare in team. Un tipico VCS prevede di avere una versione locale del codice su cui apportare modifiche. Poi "spinge" le modifiche a una versione "master" del codice all'interno di un repository remoto archiviato su un server da qualche parte. Di solito c'è un modo per controllare e coordinare quali modifiche siano apportate alla copia "master" del codice e quando, in modo che un team di sviluppatori non si ritrovi a sovrascrivere continuamente il lavoro degli altri.

[Git](https://git-scm.com/) è il sistema di controllo del codice sorgente più usato oggi. È principalmente accessibile tramite la riga di comando ma può essere accessibile tramite interfacce utente amichevoli. Con il suo codice in un repository git, può spingerlo alla sua istanza di server, o usare un sito web di controllo del codice sorgente ospitato come [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/) o [Bitbucket](https://bitbucket.org/product/).

Utilizzeremo GitHub in questo modulo. Può trovare maggiori informazioni a riguardo in [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).

#### Formattatori di codice

I formattatori di codice sono in qualche modo correlati ai linters, ma invece di segnalare errori nel codice, di solito tendono a garantire che il codice sia formattato correttamente, secondo le regole di stile, idealmente correggendo automaticamente gli errori che trovano.

[Prettier](https://prettier.io/) è un esempio molto popolare di formattatore di codice, che utilizzeremo più avanti nel modulo.

#### Controllori di tipo

I controllori di tipo sono strumenti che l'aiutano a scrivere codice più affidabile verificando che il suo codice utilizzi i tipi di dati giusti nei posti giusti. Ciò previene classi comuni di bug come l'accesso a proprietà inesistenti, `undefined` inaspettati, ecc.

[TypeScript](https://www.typescriptlang.org/) è lo standard de facto per i controllori di tipo per JavaScript. Offre una propria sintassi di annotazione di tipo ed è in qualche modo un linguaggio a sé stante, quindi non lo tratteremo in questo modulo.

### Trasformazione

Questa fase del ciclo di vita della sua app web le consente in genere di programmare in "codice futuro" (come le ultime funzionalità CSS o JavaScript che potrebbero non avere ancora supporto nativo nei browser) o di utilizzare un altro linguaggio interamente, come TypeScript. Gli strumenti di trasformazione genereranno quindi per lei codice compatibile con il browser, da utilizzare in produzione.

Generalmente, lo sviluppo web è considerato come l'uso di tre linguaggi: [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting), e ci sono strumenti di trasformazione per tutti questi linguaggi. La trasformazione offre tre benefici principali (tra gli altri):

1. La possibilità di scrivere codice usando le ultime funzionalità del linguaggio e farlo trasformare in codice che funziona su dispositivi di uso quotidiano. Ad esempio, potrebbe voler scrivere JavaScript utilizzando nuove funzionalità avanzate del linguaggio, ma far funzionare comunque il suo codice finale di produzione su browser più vecchi che non supportano quelle funzionalità. Buoni esempi qui includono:

   - [Babel](https://babeljs.io/): Un compilatore JavaScript che consente agli sviluppatori di scrivere il loro codice utilizzando JavaScript avanzato, che poi Babel prende e converte in JavaScript tradizionale che più browser possono comprendere. Gli sviluppatori possono anche scrivere e pubblicare [plugin per Babel](https://babeljs.io/docs/plugins).
   - [PostCSS](https://postcss.org/): Fa lo stesso tipo di cosa di Babel, ma per funzionalità CSS avanzate. Se non c'è un modo equivalente per fare qualcosa usando funzionalità CSS più vecchie, PostCSS installerà un polyfill JavaScript per emulare l'effetto CSS che si desidera.

2. L'opzione di scrivere il proprio codice in un linguaggio completamente diverso e trasformarlo in un linguaggio compatibile con il web. Ad esempio:

   - [Sass/SCSS](https://sass-lang.com/): Questa estensione CSS le consente di usare variabili, regole annidate, mixin, funzioni e molte altre caratteristiche, alcune delle quali disponibili nel CSS nativo (come le variabili), e altre no.
   - [TypeScript](https://www.typescriptlang.org/): TypeScript è un superset di JavaScript che offre una serie di funzionalità aggiuntive. Il compilatore TypeScript converte il codice TypeScript in JavaScript durante la costruzione per la produzione.
   - Frameworks come [React](https://react.dev/), [Ember](https://emberjs.com/) e [Vue](https://vuejs.org/): I framework forniscono molte funzionalità gratuitamente e le consentono di usarle tramite una sintassi personalizzata basata su JavaScript standard. In background, il codice JavaScript del framework lavora duramente per interpretare questa sintassi personalizzata e visualizzarla come un'app web finale.

3. Ottimizzazione. Questa è fornita dai _bundler_, che sono strumenti che preparano il suo codice per la produzione, ad esempio attraverso il "{{Glossary("Tree_shaking", "tree-shaking")}}" per assicurarsi che solo le parti delle librerie di codice che sta effettivamente usando siano inserite nel suo codice di produzione finale, o attraverso la "{{Glossary("Minification", "minificazione")}}" per rimuovere tutto lo spazio bianco nel suo codice di produzione, rendendolo il più piccolo possibile prima che sia caricato su un server. Ad esempio:

   - [Webpack](https://webpack.js.org/) è stato il bundler più popolare per lungo tempo, caratterizzato da un numero enorme di plugin e un potente sistema di configurazione. Tuttavia, è anche noto per essere piuttosto complesso da configurare e lento rispetto alle alternative più moderne.
   - [Vite](https://vite.dev/) è uno strumento di build più moderno che è popolare per la sua velocità, semplicità, e la ricchezza delle sue funzionalità.

### Post-sviluppo

Gli strumenti post-sviluppo assicurano che il suo software arrivi sul web e continui a funzionare. Questo include i processi di distribuzione, i framework di test, gli strumenti di audit e altro.

Questa fase del processo di sviluppo è quella con cui lei vuole avere il minimo di interazione attiva, in modo che una volta configurata, funzioni principalmente in modo automatico, apparendo solo se qualcosa va storto.

#### Strumenti di test

Questi strumenti generalmente prendono la forma di uno strumento che esegue automaticamente test sul suo codice per garantirne la correttezza prima che lei vada oltre (ad esempio, quando tenta di spingere modifiche su un repository GitHub). Questo può includere il linting, ma anche procedure più sofisticate come i test unitari, dove esegue una parte del suo codice, assicurandosi che si comporti come dovrebbe.

- I framework per la scrittura di test includono [Jest](https://jestjs.io/), [Mocha](https://mochajs.org/) e [Jasmine](https://jasmine.github.io/).
- I sistemi di esecuzione e notifica dei test automatizzati includono [Travis CI](https://www.travis-ci.com/), [Jenkins](https://www.jenkins.io/), [Circle CI](https://circleci.com/) e [altri](https://en.wikipedia.org/wiki/List_of_build_automation_software#Continuous_integration).

#### Strumenti di distribuzione

I sistemi di distribuzione le consentono di pubblicare il suo sito web, sono disponibili per siti statici e dinamici, e tendono comunemente a lavorare insieme ai sistemi di test. Ad esempio, un tipico toolchain attenderà che lei spinga modifiche su un repository remoto, eseguirà alcuni test per vedere se le modifiche vanno bene, e poi, se i test passano, distribuirà automaticamente la sua app su un sito di produzione.

[GitHub Pages](https://pages.github.com/) è ben integrato con GitHub stesso ed è gratuito per tutti i repository pubblici. Altri servizi, come [Netlify](https://www.netlify.com/) e [Vercel](https://vercel.com/), sono anche molto popolari, con quote generose per i piani gratuiti, flussi di lavoro di distribuzione fluidi e integrazione con GitHub.

#### Altri

Ci sono diversi altri tipi di strumenti disponibili da utilizzare nella fase post-sviluppo, tra cui [Code Climate](https://codeclimate.com/) per raccogliere metriche di qualità del codice, l'[estensione browser Webhint](https://webhint.io/docs/user-guide/extensions/extension-browser/) per eseguire analisi runtime sulla compatibilità browser e altri controlli, i [bot di GitHub](https://probot.github.io/) per fornire una funzionalità maggiormente potente per GitHub, [Updown](https://updown.io/) per fornire il monitoraggio del uptime dell'app, e molti altri!

### Alcune considerazioni sui tipi di strumenti

C'è sicuramente un ordine in cui i diversi tipi di strumenti si applicano nel ciclo di vita dello sviluppo, ma si tranquillizzi sul fatto che non _deve_ avere tutti questi strumenti in atto per rilasciare un sito web. In effetti, non ha bisogno di nessuno di questi. Tuttavia, includere alcuni di questi strumenti nel suo processo migliorerà la sua esperienza di sviluppo e probabilmente migliorerà la qualità complessiva del suo codice.

Spesso ci vuole un po' di tempo prima che i nuovi strumenti per sviluppatori si stabilizzino nella loro complessità. Uno degli strumenti più noti, webpack, ha la reputazione di essere eccessivamente complicato da lavorare, ma nell'ultima grande release, c'è stato un grande sforzo per semplificare l'uso comune in modo che la configurazione necessaria sia ridotta all'assoluto minimo.

Non esiste una bacchetta magica che garantisca il successo con gli strumenti, ma man mano che aumenta la sua esperienza troverà flussi di lavoro che funzionano _per lei_ o per il suo team e i loro progetti. Quando tutte le pieghe del processo vengono appianate, la sua toolchain dovrebbe essere qualcosa di cui può dimenticarsi e _dovrebbe_ semplicemente funzionare.

## Come scegliere e ottenere aiuto con un particolare strumento

La maggior parte degli strumenti tende ad essere scritta e rilasciata in modo isolato, quindi anche se quasi certamente c'è aiuto disponibile, non è mai nello stesso posto o formato. Può quindi essere difficile trovare aiuto con l'uso di uno strumento, o perfino scegliere quale strumento usare. La conoscenza su quali siano i migliori strumenti da usare è un po' tribale, il che significa che se non è già parte della comunità web, è difficile scoprire esattamente a quali puntare! Ecco perché abbiamo scritto questa serie di articoli, per fornire quel primo passo che è difficile trovare altrimenti.

Probabilmente avrà bisogno di una combinazione delle seguenti cose:

- Insegnanti esperti, mentori, colleghi studenti o colleghi che hanno esperienza, hanno risolto tali problemi in passato e possono dare consigli.
- Un specifico luogo utile dove cercare. Le ricerche web generali per strumenti sviluppatori front-end sono generalmente inutili a meno che non conosca già il nome dello strumento che sta cercando.

  - Se sta usando il gestore di pacchetti npm per gestire le sue dipendenze, ad esempio, è una buona idea andare alla [homepage di npm](https://www.npmjs.com/) e cercare il tipo di strumento che sta cercando, ad esempio provi a cercare "date" se desidera un'utilità di formattazione della data, o "formatter" se sta cercando un formattatore di codice generale. Presti attenzione alla popolarità, alla qualità e ai punteggi di manutenzione, e a quanto recentemente il pacchetto è stato aggiornato. Clicchi anche sulle pagine degli strumenti per scoprire quante volte un pacchetto è stato scaricato mensilmente, e se ha una buona documentazione che può usare per capire se fa ciò che deve fare. Basandosi su questi criteri, la [libreria date-fns](https://www.npmjs.com/package/date-fns) sembra essere uno strumento di formattazione della data valido da utilizzare. Vedrà questo strumento in azione e imparerà di più sui gestori di pacchetti in generale nel Capitolo 3 di questo modulo.
  - Se cerca un plugin per integrare la funzionalità dello strumento nel suo editor di codice, guardi alla pagina dei plugin/estensioni dell'editor di codice — veda ad esempio [le estensioni di VS Code](https://marketplace.visualstudio.com/vscode). Dà un'occhiata alle estensioni in vetrina sulla pagina principale, e di nuovo, provi a cercare il tipo di estensione che desidera (o il nome dello strumento, ad esempio cerchi "ESLint" sulla pagina delle estensioni di VS Code). Quando ottiene risultati, guardi informazioni come quante stelle o download ha l'estensione, come indicatore della sua qualità.

- Forum di sviluppo per fare domande su quali strumenti usare, ad esempio [MDN Learn Discourse](https://discourse.mozilla.org/c/mdn/learn/250) o [Stack Overflow](https://stackoverflow.com/).

Quando ha scelto uno strumento da usare, il primo punto di riferimento dovrebbe essere la homepage del progetto dello strumento. Questo potrebbe essere un sito web completo o potrebbe essere un singolo documento readme in un repository di codice. I [documenti date-fns](https://date-fns.org/docs/Getting-Started), ad esempio, sono piuttosto buoni, completi e facili da seguire. Tuttavia, alcune documentazioni possono essere piuttosto tecniche e accademiche e non essere adatte alle sue esigenze di apprendimento.

Invece, potrebbe voler trovare alcuni tutorial dedicati per iniziare con particolari tipi di strumenti. Un eccellente punto di partenza è cercare su siti web come [CSS Tricks](https://css-tricks.com/), [Dev](https://dev.to/), [freeCodeCamp](https://www.freecodecamp.org/) e [Smashing Magazine](https://www.smashingmagazine.com/), in quanto sono orientati all'industria dello sviluppo web.

Ancora una volta, probabilmente passerà attraverso diversi strumenti diversi mentre cerca quelli giusti per lei, provandoli per vedere se hanno senso, sono ben supportati e fanno ciò che si vuole. Va bene così — è tutto utile per l'apprendimento, e la strada diventerà più agevole man mano che acquisirà più esperienza.

## Riassunto

Questo conclude la nostra introduzione al tema degli strumenti web lato client, da una prospettiva di alto livello. Successivamente esamineremo i gestori di pacchetti.

{{NextMenu("Learn_web_development/Extensions/Client-side_tools/Package_management", "Learn_web_development/Extensions/Client-side_tools")}}
