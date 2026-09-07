---
title: Nozioni di base sulla gestione dei pacchetti
short-title: Gestione dei pacchetti
slug: Learn_web_development/Extensions/Client-side_tools/Package_management
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Overview","Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}

In questo articolo verranno esaminati in dettaglio i gestori di pacchetti, per comprendere come utilizzarli nei propri progetti: per installare le dipendenze degli strumenti del progetto, mantenerle aggiornate e altro ancora.

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
        Comprendere cosa sono i gestori di pacchetti e i repository di pacchetti,
        perché sono necessari e le nozioni di base per utilizzarli.
      </td>
    </tr>
  </tbody>
</table>

## Una dipendenza nel progetto

Una **dipendenza** è una componente software di terze parti, probabilmente scritta da qualcun altro, che idealmente risolve un singolo problema. Un progetto web può avere un numero qualsiasi di dipendenze, da nessuna a molte, e le dipendenze possono includere sotto-dipendenze che non sono state installate esplicitamente: le dipendenze possono avere dipendenze proprie.

Un semplice esempio di dipendenza utile di cui un progetto potrebbe avere bisogno è del codice per calcolare date relative come testo leggibile dalle persone. Sarebbe certamente possibile scriverlo autonomamente, ma è molto probabile che qualcun altro abbia già risolto questo problema: perché sprecare tempo reinventando la ruota? Inoltre, una dipendenza affidabile di terze parti sarà probabilmente stata testata in molte situazioni diverse, rendendola più robusta e compatibile tra browser rispetto a una soluzione propria.

Una dipendenza di progetto può essere un'intera libreria o framework JavaScript, come React o Vue, oppure una piccolissima utility come la libreria di date leggibili dalle persone, o ancora uno strumento da riga di comando come Prettier o ESLint, di cui si è parlato negli articoli precedenti.

Senza strumenti di build moderni, dipendenze come queste potrebbero essere incluse nel progetto mediante un semplice elemento [`<script>`](/it/docs/Web/HTML/Reference/Elements/script), ma potrebbero non funzionare subito e sarà probabilmente necessario usare strumenti moderni per raggruppare il codice e le dipendenze quando vengono pubblicati sul web. Un bundle è un termine generalmente usato per riferirsi a un singolo file sul server web che contiene tutto il JavaScript del software, in genere compresso il più possibile per ridurre il tempo necessario per scaricare e visualizzare il software nel browser dei visitatori.

Inoltre, cosa succede se viene trovato uno strumento migliore da usare al posto di quello corrente, oppure se viene rilasciata una nuova versione della dipendenza che si desidera aggiornare? Questo non è troppo problematico per un paio di dipendenze, ma in progetti più grandi con molte dipendenze può diventare davvero difficile tenere traccia di tutto. Ha più senso usare un **gestore di pacchetti** come npm, poiché garantisce che il codice venga aggiunto e rimosso in modo pulito, oltre a offrire numerosi altri vantaggi.

## Che cos'è esattamente un gestore di pacchetti?

[npm](https://www.npmjs.com/) è già stato incontrato, ma facendo un passo indietro rispetto a npm stesso, un gestore di pacchetti è un sistema che gestisce le dipendenze di un progetto.

Il gestore di pacchetti fornisce un metodo per installare nuove dipendenze, chiamate anche "pacchetti", gestire dove i pacchetti sono archiviati nel file system e offrire funzionalità per pubblicare i propri pacchetti.

In teoria, potrebbe non essere necessario un gestore di pacchetti e si potrebbero scaricare e archiviare manualmente le dipendenze del progetto, ma un gestore di pacchetti si occupa senza difficoltà dell'installazione e della disinstallazione dei pacchetti. Senza usarne uno, sarebbe necessario gestire manualmente:

- La ricerca di tutti i file JavaScript corretti del pacchetto.
- Il controllo che non presentino vulnerabilità note.
- Il download e il posizionamento nelle ubicazioni corrette del progetto.
- La scrittura del codice per includere i pacchetti nell'applicazione (questa operazione tende a essere eseguita usando i [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules), un altro argomento che vale la pena approfondire e comprendere).
- La stessa operazione per tutte le sotto-dipendenze dei pacchetti, che potrebbero essere decine o centinaia.
- La rimozione di tutti i file se si desidera rimuovere i pacchetti.

Inoltre, i gestori di pacchetti gestiscono le dipendenze duplicate, un aspetto importante e comune nello sviluppo front-end.

Nel caso di npm, e dei gestori di pacchetti basati su JavaScript e Node, esistono due opzioni per l'installazione delle dipendenze. Come accennato nell'articolo precedente, le dipendenze possono essere installate globalmente oppure localmente nel progetto. Sebbene l'installazione globale presenti generalmente più vantaggi, quelli dell'installazione locale sono più importanti, come la portabilità del codice e il blocco delle versioni.

Ad esempio, se un progetto dipendesse da webpack con una certa configurazione, sarebbe opportuno assicurarsi che tale configurazione continui a funzionare installando il progetto su un'altra macchina o tornando a lavorarci molto tempo dopo. Se fosse installata una versione diversa di webpack, potrebbe non essere compatibile. Per attenuare questo problema, le dipendenze vengono installate localmente in un progetto.

Per vedere davvero i vantaggi delle dipendenze locali, basta provare a scaricare ed eseguire un progetto esistente: se funziona e tutte le dipendenze funzionano subito, è grazie alle dipendenze locali che il codice è portabile.

> [!NOTE]
> npm non è l'unico gestore di pacchetti disponibile. Un'alternativa di successo e popolare è [Yarn](https://yarnpkg.com/). Yarn risolve le dipendenze usando un algoritmo diverso, che può offrire un'esperienza utente più rapida. Esistono anche numerosi altri client emergenti, come [pnpm](https://pnpm.js.org/).

## Registry di pacchetti

Affinché un gestore di pacchetti funzioni, deve sapere da dove installare i pacchetti; questo avviene tramite un registry di pacchetti. Il registry è un luogo centrale in cui un pacchetto viene pubblicato e dal quale può quindi essere installato. npm, oltre a essere un gestore di pacchetti, è anche il nome del registry di pacchetti più comunemente usato per i pacchetti JavaScript. Il registry npm si trova su [npmjs.com](https://www.npmjs.com/).

npm non è l'unica opzione. Sarebbe possibile gestire un registry di pacchetti personale: prodotti come [Microsoft Azure](https://azure.microsoft.com/) consentono di creare proxy per il registry npm, in modo da poter sovrascrivere o bloccare determinati pacchetti; [GitHub offre anch'esso un servizio di registry di pacchetti](https://docs.github.com/en/packages), e probabilmente appariranno ulteriori opzioni con il passare del tempo.

L'importante è assicurarsi di avere scelto il registry più adatto. Molti progetti usano npm e questo sarà utilizzato negli esempi per il resto del modulo.

## Uso dell'ecosistema dei pacchetti

Vediamo un esempio per iniziare a usare un gestore di pacchetti e un registry per installare un'utility da riga di comando.

Verrà usato [Vite](https://vite.dev/) per creare un sito web vuoto. Nel prossimo articolo, la toolchain verrà ampliata per includere altri strumenti e verrà mostrato come distribuire il sito.

Vite fornisce alcuni [template di inizializzazione](https://vite.dev/guide/#scaffolding-your-first-vite-project), con tutte le dipendenze e configurazioni necessarie, per iniziare rapidamente un progetto reale. A scopo dimostrativo, ne verrà configurato uno da zero, usando come riferimento il [template React](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react).

### Configurare l'app come pacchetto npm

Per prima cosa, creare una nuova directory in cui archiviare l'app sperimentale, in un luogo sensato che sia possibile ritrovare. Verrà chiamata `npm-experiment`, ma può avere qualsiasi nome:

```bash
mkdir npm-experiment
cd npm-experiment
```

Successivamente, inizializzare l'app come pacchetto npm. Verrà creato un file di configurazione, `package.json`, che consente di salvare i dettagli della configurazione nel caso sia necessario ricreare questo ambiente in seguito, o persino pubblicare il pacchetto nel registry npm, anche se questo non è rilevante per l'articolo perché si sta sviluppando un'applicazione, non una libreria riutilizzabile.

Digitare il comando seguente, assicurandosi di trovarsi nella directory `npm-experiment`:

```bash
npm init
```

Verranno ora poste alcune domande; npm creerà quindi un file `package.json` predefinito in base alle risposte. Nessuna di queste è rilevante per gli scopi attuali, poiché vengono usate solo se il pacchetto viene pubblicato in un registry e altre persone desiderano installarlo e importarlo.

- `name`: un nome per identificare l'app. Premere semplicemente <kbd>Invio</kbd> per accettare il valore predefinito `npm-experiment`.
- `version`: il numero di versione iniziale dell'app. Anche in questo caso, premere <kbd>Invio</kbd> per accettare il valore predefinito `1.0.0`.
- `description`: una breve descrizione dello scopo dell'app. Verrà omessa qui, ma è anche possibile inserire qualsiasi testo. Premere <kbd>Invio</kbd>.
- `entry point`: sarà il file JavaScript eseguito quando altri importano il pacchetto. Non è utile in questo caso, quindi premere semplicemente <kbd>Invio</kbd>.
- `test command`, `git repository` e `keywords`: premere <kbd>Invio</kbd> per lasciare vuoto ciascuno di questi campi per il momento.
- `author`: l'autore del progetto. Digitare il proprio nome e premere <kbd>Invio</kbd>.
- `license`: la licenza con cui pubblicare il pacchetto. Premere <kbd>Invio</kbd> per accettare per ora il valore predefinito.

Premere <kbd>Invio</kbd> ancora una volta per accettare queste impostazioni.

Entrare nella directory `npm-experiment`; ora dovrebbe essere presente un file package.json. Aprirlo: dovrebbe apparire simile al seguente:

```json
{
  "name": "npm-experiment",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Your name",
  "license": "ISC"
}
```

Verranno aggiunte altre due righe a package.json:

- `"type": "module"`, che fa sì che Node interpreti tutti i file `.js` come [moduli ES](/it/docs/Web/JavaScript/Guide/Modules) anziché come i vecchi moduli CommonJS. È generalmente una buona abitudine.
- `"private": true`, che impedisce di pubblicare accidentalmente il pacchetto nel registry npm.

Aggiungere queste righe subito sotto `"name"`:

```json
{
  "name": "npm-experiment",
  "type": "module",
  "private": true
  // …
}
```

Questo è dunque il file di configurazione che definisce il pacchetto. Per ora va bene così, quindi si può proseguire.

> [!NOTE]
> [Il file package.json](https://scrimba.com/intro-to-git-c0l4grs2sa) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba offre un'introduzione pratica all'uso dei file `package.json`.

### Installare Vite

Per prima cosa verrà installato Vite, lo strumento di build per il sito web. È responsabile del raggruppamento dei file HTML, CSS e JavaScript in un bundle ottimizzato per il browser.

```bash
npm install --save-dev vite
```

Quando avrà terminato di fare _Tutte Le Cose_, dare un'altra occhiata al file package.json. npm avrà aggiunto un nuovo campo, `devDependencies`:

```json
{
  "devDependencies": {
    "vite": "^5.2.13"
  }
}
```

Questa è parte della magia di npm: se in futuro la codebase viene spostata in un'altra ubicazione, su un'altra macchina, sarà possibile ricreare la stessa configurazione eseguendo il comando `npm install`; npm esaminerà le dipendenze e le installerà.

Uno svantaggio è che Vite è disponibile solo all'interno dell'app `npm-experiment`; non sarà possibile eseguirlo in una directory diversa. Tuttavia, i vantaggi superano gli svantaggi.

Notare che `vite` è stato installato come dipendenza di sviluppo. Questa differenza raramente è importante per un'applicazione, ma per una libreria significa che, quando altre persone installano il pacchetto, non installeranno implicitamente Vite. In genere, per le applicazioni, qualsiasi pacchetto importato nel codice sorgente è una dipendenza effettiva, mentre qualsiasi pacchetto usato per lo sviluppo, generalmente come strumento da riga di comando, è una dipendenza di sviluppo. Per installare dipendenze effettive, rimuovere il flag `--save-dev`.

Verranno creati anche numerosi nuovi file:

- `node_modules`: i file delle dipendenze necessari per eseguire Vite. npm li ha scaricati tutti.
- `package-lock.json`: un lockfile che memorizza le informazioni esatte necessarie per riprodurre la directory `node_modules`. Questo assicura che, finché il lockfile rimane invariato, la directory `node_modules` sia la stessa su macchine diverse.

Non è necessario preoccuparsi di questi file, poiché sono gestiti da npm. Se si usa Git, aggiungere `node_modules` al file `.gitignore`, ma in genere è opportuno mantenere `package-lock.json`, perché, come già indicato, viene usato per sincronizzare lo stato di `node_modules` tra macchine diverse.

### Configurare l'app di esempio

In ogni caso, proseguiamo con la configurazione.

In Vite, il file `index.html` è centrale. Definisce il punto di partenza dell'app e Vite lo userà per trovare gli altri file necessari per compilare l'app. Creare un file `index.html` nella directory `npm-experiment` e assegnargli il seguente contenuto:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>My test page</title>
    <meta name="viewport" content="width=device-width" />
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

Notare che l'elemento `<script>` crea una dipendenza da un file denominato `src/main.jsx`, che dichiara il punto di ingresso della logica JavaScript dell'app. Creare la cartella `src` e creare `main.jsx` in questa cartella, lasciandolo però vuoto per il momento.

> [!NOTE]
> L'attributo [`type="module"`](/it/docs/Web/HTML/Reference/Elements/script/type) è importante. Indica al browser di trattare lo script come un modulo ES, consentendo di usare la sintassi `import` ed `export` nel codice JavaScript. L'estensione del file è `.jsx` perché, nel prossimo articolo, verrà aggiunta la sintassi React JSX. I browser non comprendono JSX, ma Vite lo trasformerà in JavaScript normale, come se i browser lo comprendessero!

### Divertirsi con Vite

Ora verrà eseguito lo strumento Vite appena installato. Nel terminale, eseguire il comando seguente:

```bash
npx vite
```

Nel terminale dovrebbe essere visualizzato qualcosa di simile:

```plain
VITE v5.2.13  ready in 326 ms

➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
➜  press h + enter to show help
```

Ora è possibile sfruttare l'intero ecosistema di pacchetti JavaScript. Per iniziare, è ora in esecuzione un server web locale su `http://localhost:5173`. Per il momento non verrà visualizzato nulla, ma l'aspetto interessante è che quando si apportano modifiche all'app, Vite la ricompilerà e aggiornerà automaticamente il server, in modo da poter vedere immediatamente l'effetto dell'aggiornamento.

È possibile arrestare il server di sviluppo in qualsiasi momento con <kbd>Ctrl</kbd> + <kbd>C</kbd> e riavviarlo con lo stesso comando. Se si decide di lasciarlo in esecuzione, è possibile aprire una nuova finestra del terminale per eseguire altri comandi.

Ora aggiungiamo del contenuto alla pagina. Come dimostrazione, aggiungiamo un grafico alla pagina. Verrà usato il pacchetto [plotly.js](https://www.npmjs.com/package/plotly.js), una libreria di visualizzazione dei dati. Installarlo eseguendo il comando seguente:

```bash
npm install plotly.js-dist-min
```

Notare che l'installazione avviene senza il flag `--save-dev`. Come già indicato, ciò avviene perché il pacchetto verrà effettivamente usato nel codice sorgente, non solo come strumento da riga di comando. Questo comando aggiungerà un nuovo oggetto `"dependencies"` al file `package.json`, contenente `plotly.js-dist-min`.

> [!NOTE]
> In questo caso, il pacchetto è stato scelto per completare l'attività. Quando si scrive il proprio codice, considerare le seguenti domande durante la ricerca e l'installazione di una dipendenza:
>
> - È davvero necessaria una dipendenza? È possibile farlo con funzionalità integrate oppure è abbastanza semplice da scrivere autonomamente?
> - Cosa è necessario fare esattamente? Più si è dettagliati, più sarà probabile trovare un pacchetto che faccia esattamente ciò che serve. È possibile cercare parole chiave su npm o Google. Preferire inoltre pacchetti piccoli rispetto a quelli grandi, poiché questi ultimi possono causare problemi di prestazioni durante l'installazione, l'esecuzione e così via.
> - La dipendenza è affidabile e ben mantenuta? Verificare quando è stata pubblicata l'ultima versione, chi è l'autore e quanti download settimanali ha il pacchetto. Stabilire l'affidabilità di un pacchetto è una competenza che si acquisisce con l'esperienza, poiché occorre considerare fattori come la probabilità che il pacchetto necessiti di aggiornamenti o quante persone potrebbero averne bisogno.

Nel file `src/main.jsx`, aggiungere il codice seguente e salvare:

```js
import Plotly from "plotly.js-dist-min";

const root = document.getElementById("root");
Plotly.newPlot(
  root,
  [
    {
      x: [1, 2, 3, 4, 5],
      y: [1, 2, 4, 8, 16],
    },
  ],
  {
    margin: { t: 0 },
  },
);
```

Tornare a `http://localhost:5173`: verrà visualizzato un grafico nella pagina. Modificare i vari numeri e osservare il grafico aggiornarsi ogni volta che il file viene salvato.

### Compilare il codice per la produzione

Tuttavia, questo codice non è pronto per la produzione. La maggior parte dei sistemi di strumenti di build, incluso Vite, ha una "modalità di sviluppo" e una "modalità di produzione". La differenza importante è che molte delle funzionalità utili usate durante lo sviluppo non sono necessarie nel sito finale e vengono quindi rimosse per la produzione, ad esempio "hot module replacement", "live reloading" e "codice sorgente non compresso e commentato". Sebbene l'elenco non sia esaustivo, queste sono alcune delle funzionalità comuni dello sviluppo web che sono molto utili nella fase di sviluppo ma non in produzione. In produzione, aumenterebbero solo inutilmente le dimensioni del sito.

Ora arrestare il server di sviluppo Vite in esecuzione con <kbd>Ctrl</kbd> + <kbd>C</kbd>.

Ora è possibile preparare il sito di esempio essenziale per una distribuzione immaginaria. Vite fornisce un comando aggiuntivo, `build`, per generare file adatti alla pubblicazione.

Eseguire il comando seguente:

```bash
npx vite build
```

Dovrebbe essere visualizzato un output simile a questo:

```plain
vite v5.2.13 building for production...
✓ 6 modules transformed.
dist/index.html                    0.32 kB │ gzip:     0.24 kB
dist/assets/index-BlYAJQFz.js  3,723.18 kB │ gzip: 1,167.74 kB

(!) Some chunks are larger than 500 kB after minification. Consider:
- Using dynamic import() to code-split the application
- Use build.rollupOptions.output.manualChunks to improve chunking: https://rollupjs.org/configuration-options/#output-manualchunks
- Adjust chunk size limit for this warning via build.chunkSizeWarningLimit.
✓ built in 4.36s
```

Vite creerà una directory denominata `dist`. Al suo interno contiene un file `index.html`, molto simile a quello nella root, tranne per il fatto che il sorgente dello `script` è stato sostituito con un percorso alla cartella `assets`. La cartella `assets` contiene l'output JavaScript trasformato, ora minificato e ottimizzato per la produzione.

> [!NOTE]
> L'avviso relativo a un chunk troppo grande potrebbe destare preoccupazione. È previsto, perché viene caricata una libreria che svolge molte operazioni dietro le quinte, come scrivere autonomamente tutto il codice per disegnare lo stesso grafico. Per ora non è necessario preoccuparsene.

## Una guida approssimativa ai client dei gestori di pacchetti

Questo tutorial ha installato il pacchetto Vite usando npm, ma, come già indicato, esistono alcune alternative. Vale la pena sapere almeno che esistono e avere un'idea generale dei comandi comuni tra i vari strumenti. Alcuni sono già stati visti in azione, ma esaminiamo gli altri.

L'elenco crescerà nel tempo, ma al momento della scrittura sono disponibili i seguenti principali gestori di pacchetti:

- npm su [npmjs.org](https://www.npmjs.com/)
- pnpm su [pnpm.js.org](https://pnpm.js.org/)
- Yarn su [yarnpkg.com](https://yarnpkg.com/)

npm e pnpm sono simili dal punto di vista della riga di comando: infatti, pnpm mira ad avere piena parità nelle opzioni degli argomenti offerte da npm. Si differenzia perché usa un metodo diverso per scaricare e archiviare i pacchetti sul computer, con l'obiettivo di ridurre lo spazio su disco complessivamente richiesto.

Dove npm viene mostrato negli esempi seguenti, può essere sostituito con pnpm e il comando funzionerà.

Yarn è spesso considerato più rapido di npm nel processo di installazione, anche se i risultati possono variare. Questo è importante per gli sviluppatori perché può essere sprecata una quantità significativa di tempo nell'attesa dell'installazione delle dipendenze e della copia sul computer.

Tuttavia, è importante notare che il gestore di pacchetti npm **non** è necessario per installare pacchetti dal registry npm. pnpm e Yarn possono usare lo stesso formato `package.json` di npm e possono installare qualsiasi pacchetto dal registry npm e da altri registry di pacchetti.

Rivediamo le azioni comuni che sarà necessario eseguire con i gestori di pacchetti.

> [!NOTE]
> Verranno mostrati i comandi sia di npm sia di Yarn. Non sono pensati per essere eseguiti nello stesso progetto. Il progetto dovrebbe essere configurato con npm o Yarn e i comandi di quel gestore di pacchetti dovrebbero essere usati in modo coerente.

### Inizializzare un nuovo progetto

```bash
npm init
yarn init
```

Come mostrato sopra, questo comando proporrà una serie di domande per descrivere il progetto, come nome, licenza, descrizione e così via, quindi genererà un file `package.json` contenente meta-informazioni sul progetto e sulle sue dipendenze.

### Installare dipendenze

```bash
npm install vite
yarn add vite
```

Anche `install` è già stato visto in azione. Questo aggiungerebbe direttamente il pacchetto `vite` alla directory di lavoro in una sottodirectory denominata `node_modules`, insieme alle dipendenze di `vite`.

Per impostazione predefinita, questo comando installerà la versione più recente di `vite`, ma è possibile controllare anche questo aspetto. Si può richiedere `vite@4`, che fornisce l'ultima versione 4.x, ovvero 4.5.3. Oppure si può provare `vite@^4.0.0`, che indica l'ultima versione successiva o uguale alla 4.0.0, con lo stesso significato dell'esempio precedente.

### Aggiornare dipendenze

```bash
npm update
yarn upgrade
```

Questo comando esaminerà le dipendenze attualmente installate e le aggiornerà, se è disponibile un aggiornamento, entro l'intervallo specificato nel pacchetto.

L'intervallo è specificato nella versione della dipendenza nel file `package.json`, ad esempio `"vite": "^5.2.13"`: in questo caso, il carattere accento circonflesso `^` indica tutte le release minor e patch successive o uguali alla 5.2.13, fino alla 6.0.0 esclusa.

Questo viene determinato usando un sistema chiamato [semver](https://semver.org/), che dalla documentazione può sembrare un po' complicato, ma può essere semplificato considerando solo le informazioni riepilogative e che una versione è rappresentata da `MAJOR.MINOR.PATCH`; ad esempio, 2.0.1 è la versione major 2 con versione patch 1. Un ottimo modo per provare i valori semver è usare il [calcolatore semver](https://semver.npmjs.com/).

È importante ricordare che `npm update` non aggiornerà le dipendenze oltre l'intervallo definito nel file `package.json`; per farlo sarà necessario installare specificamente quella versione.

### Altri comandi

È possibile trovare ulteriori informazioni sui singoli comandi di [npm](https://docs.npmjs.com/cli-documentation/) e [yarn](https://classic.yarnpkg.com/en/docs/cli/) online. Anche i comandi di [pnpm](https://pnpm.io/cli/add) hanno parità con npm, con alcune aggiunte.

## Creare comandi personalizzati

I gestori di pacchetti supportano anche la creazione di comandi personalizzati e la loro esecuzione dalla riga di comando. Ad esempio, in precedenza è stato invocato il comando `vite` con `npx` per avviare il server di sviluppo Vite. Potrebbe essere creato il seguente comando:

```bash
npm run dev
# or yarn run dev
```

Questo eseguirebbe uno script personalizzato per avviare il progetto in "modalità di sviluppo". In effetti, viene regolarmente incluso in tutti i progetti, poiché la configurazione di sviluppo locale tende a funzionare in modo leggermente diverso rispetto alla produzione.

Provando a eseguire questo comando nel progetto di test precedente, probabilmente verrebbe segnalato che lo "script dev è mancante". Questo accade perché npm, Yarn e strumenti simili cercano una proprietà chiamata `dev` nella proprietà `scripts` del file `package.json`. Creiamo quindi un comando abbreviato personalizzato, "dev", nel file `package.json`. Se è stato seguito il tutorial precedente, dovrebbe esserci un file `package.json` nella directory npm-experiment. Aprirlo: il membro `scripts` dovrebbe avere un aspetto simile al seguente:

```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

Aggiornarlo in modo che appaia così e salvare il file:

```json
{
  "scripts": {
    "dev": "vite"
  }
}
```

È stato aggiunto un comando `dev` personalizzato come script npm.

Ora provare a eseguire quanto segue nel terminale, assicurandosi di trovarsi nella directory `npm-experiment`:

```bash
npm run dev
```

Questo dovrebbe avviare Vite e lo stesso server di sviluppo locale visto in precedenza.

Notare che lo script definito qui non necessita più del prefisso `npx`. Questo perché i comandi npm, e yarn, sono intelligenti: cercheranno gli strumenti da riga di comando installati localmente nel progetto prima di tentare di trovarli mediante metodi convenzionali, ovvero dove normalmente il computer archivia e consente di trovare il software. È possibile [approfondire le complessità tecniche del comando `run`](https://docs.npmjs.com/cli/commands/npm-run/), sebbene nella maggior parte dei casi gli script personali funzioneranno senza problemi.

Questo comando specifico potrebbe sembrare superfluo: `npm run dev` richiede più caratteri da digitare rispetto a `npx vite`, ma è una forma di _astrazione_. Consente di aggiungere più operazioni al comando `dev` in futuro, come impostare variabili d'ambiente, generare file temporanei e così via, senza complicare il comando.

Alla proprietà `scripts` possono essere aggiunti tutti i tipi di elementi che aiutano a svolgere il proprio lavoro. Ad esempio, ecco cosa Vite raccomanda nel template:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

## Riepilogo

Questo conclude la panoramica dei gestori di pacchetti. Il prossimo passo consiste nel creare una toolchain di esempio, mettendo in pratica tutto ciò che è stato appreso finora.

## Vedi anche

- [Riferimento degli script npm](https://docs.npmjs.com/cli/v8/using-npm/scripts/)
- [Riferimento di package.json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json/)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Overview","Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}
