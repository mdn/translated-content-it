---
title: Panoramica degli strumenti lato client
short-title: Overview
slug: Learn_web_development/Extensions/Client-side_tools/Overview
l10n:
  sourceCommit: 710372d69095aaeadfba6c892f3e39ed63df4c54
---

{{NextMenu("Learn_web_development/Extensions/Client-side_tools/Package_management", "Learn_web_development/Extensions/Client-side_tools")}}

In questo articolo viene fornita una panoramica degli strumenti web moderni, dei tipi di strumenti disponibili e di dove vengono utilizzati nel ciclo di vita dello sviluppo di app web, nonché di come trovare assistenza per i singoli strumenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere quali tipi di strumenti lato client esistono e come
        trovare strumenti e ottenere assistenza per utilizzarli.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica degli strumenti moderni

Scrivere software per il web è diventato più sofisticato nel corso del tempo. Sebbene sia ancora del tutto ragionevole scrivere HTML, CSS e JavaScript "a mano", oggi esiste una grande quantità di strumenti che gli sviluppatori possono usare per velocizzare il processo di creazione di un sito web o di un'app.

Esistono strumenti estremamente consolidati che sono diventati nomi comuni nella comunità di sviluppo, e ogni giorno vengono scritti e pubblicati nuovi strumenti per risolvere problemi specifici. Potrebbe perfino capitare di scrivere un software per agevolare il proprio processo di sviluppo, risolvendo un problema specifico che gli strumenti esistenti non sembrano già gestire.

È facile sentirsi sopraffatti dal semplice numero di strumenti che possono essere inclusi in un singolo progetto. Allo stesso modo, un singolo file di configurazione per uno strumento come [webpack](https://webpack.js.org/) può essere lungo centinaia di righe, la maggior parte delle quali sono formule magiche che sembrano fare il proprio lavoro ma che solo un ingegnere esperto comprenderà pienamente.

Di tanto in tanto, anche gli sviluppatori web più esperti si bloccano davanti a un problema relativo agli strumenti; è possibile sprecare ore nel tentativo di far funzionare una pipeline di strumenti prima ancora di toccare una sola riga di codice dell'applicazione. Se questo è già capitato in passato, non c'è da preoccuparsi: non si è soli.

In questi articoli non verrà data risposta a ogni domanda sugli strumenti web, ma verrà fornito un utile punto di partenza per comprendere i fondamenti, sui quali sarà poi possibile costruire. Come per qualsiasi argomento complesso, è bene iniziare in piccolo e avanzare gradualmente verso utilizzi più avanzati.

## L'ecosistema degli strumenti moderni

L'ecosistema moderno di strumenti per sviluppatori è oggi enorme, quindi è utile avere un'idea generale dei principali problemi risolti dagli strumenti. Cercando "front-end developer tools" con il motore di ricerca preferito, si otterrà un'enorme varietà di risultati, che vanno dagli editor di testo ai browser, fino al tipo di penne utilizzabili per prendere appunti.

Sebbene la scelta dell'editor di codice sia certamente una scelta relativa agli strumenti, questa serie di articoli andrà oltre, concentrandosi sugli strumenti per sviluppatori che aiutano a produrre codice web in modo più efficiente. Verranno consigliati alcuni strumenti specifici e i tutorial seguenti mostreranno come usarli. Si tratta di strumenti popolari e standard al momento della stesura. Questo non impedisce di usare altri strumenti, se si conoscono i relativi vantaggi.

Da una prospettiva generale, gli strumenti lato client possono essere suddivisi nelle seguenti quattro ampie categorie di problemi da risolvere:

- **Ambiente** — Strumenti che aiutano a configurare l'ambiente di sviluppo, ad esempio installando ed eseguendo altri strumenti.
- **Rete di sicurezza** — Strumenti utili durante lo sviluppo del codice.
- **Trasformazione** — Strumenti che trasformano il codice in qualche modo, ad esempio convertendo un linguaggio intermedio in JavaScript comprensibile da un browser.
- **Post-sviluppo** — Strumenti utili dopo aver scritto il codice, come strumenti di testing e deployment.

Esaminiamo ciascuna categoria più nel dettaglio.

### Ambiente

L'editor, il sistema operativo e il browser sono tutti ambienti di sviluppo. Si presuppone che sia già stata scelta l'opzione più adatta alle proprie esigenze. Tuttavia, prima di installare ed eseguire altri strumenti, restano ancora due scelte da fare:

- Dove eseguire gli strumenti. La maggior parte degli strumenti eseguiti localmente è scritta in JavaScript, quindi è necessario un interprete JavaScript sul computer che possa essere richiamato dalla riga di comando, non quello presente nel browser. [Node.js](https://nodejs.org/) rimane lo standard del settore e verrà utilizzato qui. [Bun](https://bun.com/) è pensato come sostituzione diretta di Node.js ed è noto per la sua velocità e le sue potenti API.
- Come installare gli strumenti, ovvero il _package manager_. Node fornisce [npm](https://www.npmjs.com/) per impostazione predefinita, quindi verrà utilizzato. [Yarn](https://yarnpkg.com/) e [pnpm](https://pnpm.io/) sono altre scelte popolari, ciascuna con i propri vantaggi, come velocità, gestione dei progetti e così via.

### Rete di sicurezza

Si tratta di strumenti che rendono il codice scritto leggermente migliore.

Questa parte degli strumenti dovrebbe essere specifica dell'ambiente di sviluppo personale, anche se non è raro che le aziende dispongano di qualche tipo di criterio o configurazione predefinita da installare, in modo che tutti gli sviluppatori utilizzino gli stessi processi.

Include tutto ciò che semplifica il processo di sviluppo per generare codice stabile e affidabile. Gli strumenti di rete di sicurezza dovrebbero inoltre aiutare a prevenire errori o a correggerli automaticamente senza dover ricompilare il codice da zero ogni volta.

Di seguito sono riportati alcuni tipi molto comuni di strumenti di rete di sicurezza usati dagli sviluppatori.

#### Linters

I **linter** sono strumenti che analizzano il codice e segnalano gli eventuali errori presenti, i relativi tipi e le righe di codice in cui si trovano. Spesso i linter possono essere configurati non solo per segnalare gli errori, ma anche per segnalare eventuali violazioni di una guida di stile specificata usata dal team, ad esempio codice che utilizza un numero errato di spazi per il rientro o che usa [template literals](/it/docs/Web/JavaScript/Reference/Template_literals) anziché string literal regolari.

[ESLint](https://eslint.org/) è il linter JavaScript standard del settore: uno strumento altamente configurabile per individuare potenziali errori di sintassi e incoraggiare le "best practice" in tutto il codice. Alcune aziende e progetti hanno inoltre [condiviso le proprie configurazioni ESLint](https://www.npmjs.com/search?q=keywords:eslintconfig).

È anche possibile trovare strumenti di linting per altri linguaggi, come [stylelint](https://stylelint.io/).

#### Controllo del codice sorgente

Conosciuto anche come **sistema di controllo versione** (VCS), il **controllo del codice sorgente** è essenziale per effettuare il backup del lavoro e lavorare in team. Un VCS tipico prevede una versione locale del codice su cui apportare modifiche. Le modifiche vengono poi sottoposte tramite "push" a una versione "master" del codice all'interno di un repository remoto archiviato su un server. Solitamente esiste un modo per controllare e coordinare quali modifiche vengono apportate alla copia "master" del codice e quando, così da evitare che un team di sviluppatori sovrascriva continuamente il lavoro altrui.

[Git](https://git-scm.com/) è il sistema di controllo del codice sorgente usato oggi dalla maggior parte delle persone. Si accede principalmente tramite la riga di comando, ma è possibile accedervi anche tramite interfacce utente intuitive. Con il codice in un repository git, è possibile eseguire il push verso un'istanza del proprio server oppure usare un sito di controllo del codice sorgente ospitato, come [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/) o [Bitbucket](https://bitbucket.org/product/).

In questo modulo verrà usato GitHub. Maggiori informazioni sono disponibili in [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).

#### Formattatori di codice

I formattatori di codice sono in qualche modo correlati ai linter, ma anziché segnalare errori nel codice, tendono solitamente a garantire che il codice sia formattato correttamente secondo le regole di stile, idealmente correggendo automaticamente gli errori rilevati.

[Prettier](https://prettier.io/) è un esempio molto popolare di formattatore di codice, che verrà usato più avanti nel modulo.

#### Controllori di tipi

I controllori di tipi sono strumenti che aiutano a scrivere codice più affidabile verificando che il codice usi i tipi di dati corretti nei punti appropriati. Questo impedisce classi comuni di bug, come l'accesso a proprietà inesistenti, valori `undefined` imprevisti e così via.

[TypeScript](https://www.typescriptlang.org/) è lo standard di fatto per il controllo dei tipi in JavaScript. Fornisce una propria sintassi per le annotazioni di tipo ed è in parte un linguaggio a sé stante, quindi non verrà trattato in questo modulo.

### Trasformazione

Questa fase del ciclo di vita dell'app web consente in genere di scrivere codice "del futuro", ad esempio usando le ultime funzionalità CSS o JavaScript che potrebbero non avere ancora supporto nativo nei browser, oppure codice in un linguaggio completamente diverso, come TypeScript. Gli strumenti di trasformazione genereranno quindi codice compatibile con i browser da usare in produzione.

In generale, lo sviluppo web è considerato basato su tre linguaggi: [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting), ed esistono strumenti di trasformazione per tutti questi linguaggi. La trasformazione offre tre vantaggi principali, tra gli altri:

1. La possibilità di scrivere codice usando le più recenti funzionalità del linguaggio e trasformarlo in codice che funziona sui dispositivi di uso quotidiano. Ad esempio, potrebbe essere necessario scrivere JavaScript usando nuove funzionalità all'avanguardia del linguaggio, mantenendo però il codice finale di produzione funzionante su browser meno recenti che non supportano tali funzionalità. Buoni esempi includono:
   - [Babel](https://babeljs.io/): un compilatore JavaScript che consente agli sviluppatori di scrivere codice usando JavaScript all'avanguardia, che Babel poi converte in JavaScript tradizionale comprensibile da più browser. Gli sviluppatori possono anche scrivere e pubblicare [plugin per Babel](https://babeljs.io/docs/plugins).
   - [PostCSS](https://postcss.org/): svolge lo stesso tipo di lavoro di Babel, ma per le funzionalità CSS all'avanguardia. Se non esiste un modo equivalente per ottenere un risultato usando funzionalità CSS meno recenti, PostCSS installerà un polyfill JavaScript per emulare l'effetto CSS desiderato.

2. L'opzione di scrivere il codice in un linguaggio completamente diverso e trasformarlo in un linguaggio compatibile con il web. Ad esempio:
   - [Sass/SCSS](https://sass-lang.com/): questa estensione CSS consente di usare variabili, regole annidate, mixin, funzioni e molte altre funzionalità, alcune delle quali sono disponibili nel CSS nativo, come le variabili, e altre no.
   - [TypeScript](https://www.typescriptlang.org/): TypeScript è un superset di JavaScript che offre numerose funzionalità aggiuntive. Il compilatore TypeScript converte il codice TypeScript in JavaScript durante la compilazione per la produzione.
   - Framework come [React](https://react.dev/), [Ember](https://emberjs.com/) e [Vue](https://vuejs.org/): i framework forniscono gratuitamente molte funzionalità e permettono di usarle tramite una sintassi personalizzata basata su JavaScript vanilla. In background, il codice JavaScript del framework lavora per interpretare questa sintassi personalizzata ed eseguirne il rendering come app web finale.

3. Ottimizzazione. Questa è fornita dai _bundler_, ovvero strumenti che preparano il codice per la produzione, ad esempio tramite "{{Glossary("Tree_shaking", "tree-shaking")}}" per assicurarsi che nel codice finale di produzione vengano incluse solo le parti delle librerie di codice effettivamente utilizzate, oppure tramite "{{Glossary("Minification", "minificazione")}}" per rimuovere tutti gli spazi bianchi dal codice di produzione, rendendolo il più piccolo possibile prima che venga caricato su un server. Ad esempio:
   - [webpack](https://webpack.js.org/) è stato per molto tempo il bundler più popolare, con un numero enorme di plugin e un potente sistema di configurazione. Tuttavia, è anche noto per essere piuttosto complesso da configurare e lento rispetto ad alternative più moderne.
   - [Vite](https://vite.dev/) è uno strumento di build più moderno, popolare per velocità, semplicità e ricchezza di funzionalità.

### Post-sviluppo

Gli strumenti di post-sviluppo garantiscono che il software venga pubblicato sul web e continui a funzionare. Ciò include processi di deployment, framework di testing, strumenti di audit e altro ancora.

Questa fase del processo di sviluppo è quella in cui si desidera avere il minor numero possibile di interazioni attive, in modo che, una volta configurata, funzioni per lo più automaticamente, intervenendo solo per segnalare eventuali problemi.

#### Strumenti di testing

Questi assumono generalmente la forma di uno strumento che esegue automaticamente test sul codice per assicurarsi che sia corretto prima di procedere ulteriormente, ad esempio quando si tenta di eseguire il push delle modifiche in un repository GitHub. Possono includere il linting, ma anche procedure più sofisticate come gli unit test, in cui viene eseguita una parte del codice per assicurarsi che si comporti come previsto.

- I framework per scrivere test includono [Jest](https://jestjs.io/), [Mocha](https://mochajs.org/) e [Jasmine](https://jasmine.github.io/).
- I sistemi automatizzati per l'esecuzione di test e le notifiche includono [Travis CI](https://www.travis-ci.com/), [Jenkins](https://www.jenkins.io/), [Circle CI](https://circleci.com/) e [altri](https://en.wikipedia.org/wiki/List_of_build_automation_software#Continuous_integration).

#### Strumenti di deployment

I sistemi di deployment consentono di pubblicare il sito web, sono disponibili sia per siti statici sia dinamici e tendono comunemente a funzionare insieme ai sistemi di testing. Ad esempio, una toolchain tipica attenderà il push delle modifiche verso un repository remoto, eseguirà alcuni test per verificare che le modifiche siano corrette e, se i test vengono superati, distribuirà automaticamente l'app su un sito di produzione.

[GitHub Pages](https://pages.github.com/) è ben integrato con GitHub stesso ed è gratuito per tutti i repository pubblici. Anche altri servizi, come [Netlify](https://www.netlify.com/) e [Vercel](https://vercel.com/), sono molto popolari e offrono quote generose nei piani gratuiti, flussi di lavoro di deployment fluidi e integrazione con GitHub.

#### Altri

Esistono diversi altri tipi di strumenti utilizzabili nella fase di post-sviluppo, tra cui [Code Climate](https://codeclimate.com/) per raccogliere metriche sulla qualità del codice, l'[estensione del browser Webhint](https://webhint.io/docs/user-guide/extensions/extension-browser/) per eseguire analisi a runtime della compatibilità tra browser e altri controlli, i [bot GitHub](https://probot.github.io/) per fornire funzionalità GitHub più potenti, [Updown](https://updown.io/) per il monitoraggio dell'uptime delle app e molti altri.

### Alcune considerazioni sui tipi di strumenti

Esiste certamente un ordine in cui i diversi tipi di strumenti si applicano nel ciclo di vita dello sviluppo, ma non è _necessario_ disporre di tutti questi elementi per pubblicare un sito web. In effetti, non è necessario averne nessuno. Tuttavia, includere alcuni di questi strumenti nel processo migliorerà l'esperienza di sviluppo e probabilmente anche la qualità complessiva del codice.

Spesso i nuovi strumenti per sviluppatori richiedono del tempo per stabilizzarsi in termini di complessità. Uno degli strumenti più noti, webpack, ha la reputazione di essere eccessivamente complicato da usare, ma nell'ultima versione principale è stato fatto un grande sforzo per semplificare l'uso comune, riducendo al minimo assoluto la configurazione necessaria.

Non esiste certamente una soluzione miracolosa che garantisca il successo con gli strumenti, ma con l'aumentare dell'esperienza si troveranno flussi di lavoro adatti _alle proprie esigenze_ oppure al proprio team e ai suoi progetti. Una volta risolti tutti i problemi del processo, la toolchain dovrebbe essere qualcosa di cui ci si può dimenticare e _dovrebbe_ semplicemente funzionare.

## Come scegliere e ottenere assistenza per uno strumento specifico

La maggior parte degli strumenti viene sviluppata e pubblicata isolatamente, quindi, anche se quasi certamente è disponibile assistenza, questa non si trova mai nello stesso posto o formato. Può quindi essere difficile trovare assistenza per usare uno strumento, o perfino scegliere quale strumento usare. La conoscenza dei migliori strumenti da utilizzare è in parte tramandata all'interno della comunità, il che significa che, se non si fa già parte della comunità web, è difficile scoprire esattamente quali scegliere. Questo è uno dei motivi per cui è stata scritta questa serie di articoli: fornire, si spera, quel primo passo che altrimenti è difficile da trovare.

Probabilmente sarà necessaria una combinazione dei seguenti elementi:

- Insegnanti, mentori, compagni di studio o colleghi esperti che abbiano affrontato problemi simili in precedenza e possano offrire consigli.
- Un luogo specifico e utile in cui effettuare la ricerca. Le ricerche web generiche sugli strumenti per sviluppatori front-end sono generalmente inutili, a meno che non si conosca già il nome dello strumento cercato.
  - Se, ad esempio, si usa il package manager npm per gestire le dipendenze, è una buona idea visitare la [homepage di npm](https://www.npmjs.com/) e cercare il tipo di strumento desiderato. Ad esempio, provare a cercare "date" se serve un'utilità per la formattazione delle date, oppure "formatter" se si cerca un formattatore di codice generico. Prestare attenzione ai punteggi di popolarità, qualità e manutenzione, nonché a quanto recentemente il pacchetto è stato aggiornato l'ultima volta. Fare inoltre clic sulle pagine degli strumenti per scoprire quanti download mensili ha un pacchetto e se dispone di buona documentazione, utile per capire se fa ciò che serve. In base a questi criteri, la [libreria date-fns](https://www.npmjs.com/package/date-fns) sembra essere un buon strumento per la formattazione delle date. Questo strumento verrà mostrato in azione e si apprenderà di più sui package manager in generale nel Capitolo 3 di questo modulo.
  - Se si cerca un plugin per integrare la funzionalità di uno strumento nell'editor di codice, consultare la pagina dei plugin/delle estensioni dell'editor: vedere, ad esempio, [le estensioni di VS Code](https://marketplace.visualstudio.com/vscode). Esaminare le estensioni in evidenza nella pagina iniziale e, di nuovo, provare a cercare il tipo di estensione desiderata oppure il nome dello strumento; ad esempio, cercare "ESLint" nella pagina delle estensioni di VS Code. Quando si ottengono risultati, esaminare informazioni quali il numero di stelle o download dell'estensione, come indicatore della sua qualità.

- Forum relativi allo sviluppo su cui porre domande sugli strumenti da usare, come [MDN Learn Discourse](https://discourse.mozilla.org/c/mdn/learn/250) o [Stack Overflow](https://stackoverflow.com/).

Dopo aver scelto uno strumento da usare, il primo punto di riferimento dovrebbe essere la homepage del progetto dello strumento. Potrebbe essere un sito web completo oppure un singolo documento readme in un repository di codice. La [documentazione di date-fns](https://date-fns.org/docs/Getting-Started), ad esempio, è piuttosto valida, completa e facile da seguire. Alcune documentazioni, tuttavia, possono essere piuttosto tecniche e accademiche, e non adatte alle esigenze di apprendimento.

Potrebbe invece essere preferibile trovare tutorial dedicati per iniziare a usare tipi specifici di strumenti. Un ottimo punto di partenza è cercare su siti web come [CSS Tricks](https://css-tricks.com/), [Dev](https://dev.to/), [freeCodeCamp](https://www.freecodecamp.org/) e [Smashing Magazine](https://www.smashingmagazine.com/), poiché sono pensati per il settore dello sviluppo web.

Ancora una volta, probabilmente verranno provati diversi strumenti nella ricerca di quelli più adatti, per verificare se sono sensati, ben supportati e in grado di fare ciò che serve. Questo va bene: è utile per l'apprendimento e il percorso diventerà più agevole con l'aumentare dell'esperienza.

## Riepilogo

Con questo si conclude l'introduzione generale e graduale al tema degli strumenti web lato client. Successivamente verranno esaminati i package manager.

{{NextMenu("Learn_web_development/Extensions/Client-side_tools/Package_management", "Learn_web_development/Extensions/Client-side_tools")}}
