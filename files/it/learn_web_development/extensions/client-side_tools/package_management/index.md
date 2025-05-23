---
title: Nozioni di base sulla gestione dei pacchetti
short-title: Gestione dei pacchetti
slug: Learn_web_development/Extensions/Client-side_tools/Package_management
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Overview","Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}

In questo articolo, esamineremo in dettaglio i gestori di pacchetti per capire come possiamo usarli nei nostri progetti — per installare le dipendenze degli strumenti del progetto, mantenerle aggiornate e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le lingue fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere cosa sono i gestori di pacchetti e i repository di pacchetti, perché sono necessari e i fondamenti su come utilizzarli.
      </td>
    </tr>
  </tbody>
</table>

## Una dipendenza nel tuo progetto

Una **dipendenza** è un software di terze parti che probabilmente è stato scritto da qualcun altro e idealmente risolve un singolo problema per te. Un progetto web può avere un numero qualsiasi di dipendenze, da nessuna a molte, e le tue dipendenze potrebbero includere sub-dipendenze che non hai installato esplicitamente — le tue dipendenze possono avere le proprie dipendenze.

Un semplice esempio di una dipendenza utile di cui il tuo progetto potrebbe aver bisogno è del codice per calcolare date relative come testo leggibile. Potresti certamente scrivere questo codice da solo, ma è molto probabile che qualcun altro abbia già risolto questo problema — perché sprecare tempo a reinventare la ruota? Inoltre, una dipendenza di terze parti affidabile sarà probabilmente stata testata in molte diverse situazioni, rendendola più robusta e compatibile con i diversi browser rispetto alla tua soluzione personale.

Una dipendenza del progetto può essere un'intera libreria o framework JavaScript — come React o Vue — o una piccola utility come la nostra libreria di date leggibili, o può essere uno strumento da riga di comando come Prettier o ESLint, di cui abbiamo parlato in articoli precedenti.

Senza gli strumenti di build moderni, dipendenze come queste potrebbero essere incluse nel tuo progetto usando un semplice elemento [`<script>`](/it/docs/Web/HTML/Reference/Elements/script), ma questo potrebbe non funzionare immediatamente e probabilmente avresti bisogno di strumenti moderni per impacchettare il tuo codice e le dipendenze insieme quando vengono rilasciate sul web. Un bundle è un termine generalmente usato per riferirsi a un singolo file sul tuo server web che contiene tutto il JavaScript per il tuo software — tipicamente compresso il più possibile per aiutare a ridurre il tempo necessario per scaricare e visualizzare il tuo software nel browser dei tuoi visitatori.

Inoltre, cosa succede se trovi un miglior strumento che vuoi usare al posto di quello attuale, o viene rilasciata una nuova versione della tua dipendenza che vuoi aggiornare? Questo non è troppo doloroso per un paio di dipendenze, ma in progetti più grandi con molte dipendenze, questo tipo di cosa può diventare davvero complicata da tenere traccia. Ha più senso usare un **gestore di pacchetti** come npm, poiché questo garantirà che il codice sia aggiunto e rimosso in modo pulito, oltre a offrire una serie di altri vantaggi.

## Che cos'è esattamente un gestore di pacchetti?

Abbiamo già incontrato [npm](https://www.npmjs.com/), ma prendendo le distanze da npm stesso, un gestore di pacchetti è un sistema che gestirà le dipendenze del tuo progetto.

Il gestore di pacchetti fornirà un metodo per installare nuove dipendenze (note anche come "pacchetti"), gestire dove i pacchetti sono memorizzati nel tuo file system, e offrire capacità per pubblicare i tuoi pacchetti.

In teoria, potresti non aver bisogno di un gestore di pacchetti e potresti scaricare manualmente e memorizzare le dipendenze del tuo progetto, ma un gestore di pacchetti gestirà senza soluzione di continuità l'installazione e la disinstallazione dei pacchetti. Se non ne usassi uno, dovresti gestire manualmente:

- Trovare tutti i file JavaScript corretti del pacchetto.
- Controllarli per assicurarti che non abbiano vulnerabilità conosciute.
- Scaricarli e metterli nelle posizioni corrette nel tuo progetto.
- Scrivere il codice per includere il/i pacchetto/i nella tua applicazione (tende ad essere fatto usando [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules), un altro argomento su cui vale la pena documentarsi e comprendere).
- Fare la stessa cosa per tutte le sub-dipendenze dei pacchetti, delle quali potrebbero esserci decine o centinaia.
- Rimuovere di nuovo tutti i file se vuoi rimuovere i pacchetti.

Inoltre, i gestori di pacchetti gestiscono le dipendenze duplicate (qualcosa che diventa importante e comune nello sviluppo front-end).

Nel caso di npm (e dei gestori di pacchetti basati su JavaScript e Node) hai due opzioni per dove installare le tue dipendenze. Come abbiamo toccato nell'articolo precedente, le dipendenze possono essere installate globalmente o localmente nel tuo progetto. Anche se ci sono più pro nei confronti dell'installazione globale, i pro per l'installazione locale sono più importanti — come la portabilità del codice e il blocco delle versioni.

Ad esempio, se il tuo progetto dipende da webpack con una certa configurazione, vorresti assicurarti che se installi quel progetto su un'altra macchina o ci ritorni molto più tardi, la configurazione funzionerebbe ancora. Se una versione diversa di webpack fosse installata, potrebbe non essere compatibile. Per mitigare questo, le dipendenze sono installate localmente in un progetto.

Per vedere davvero brillare le dipendenze locali, tutto ciò che devi fare è provare a scaricare ed eseguire un progetto esistente — se funziona e tutte le dipendenze funzionano immediatamente, allora devi ringraziare le dipendenze locali per il fatto che il codice è portatile.

> [!NOTE]
> npm non è l'unico gestore di pacchetti disponibile. Un'alternativa popolare e di successo è [Yarn](https://yarnpkg.com/). Yarn risolve le dipendenze usando un algoritmo diverso che può significare un'esperienza utente più veloce. Ci sono anche un certo numero di altri client emergenti, come [pnpm](https://pnpm.js.org/).

## Registri dei pacchetti

Affinché un gestore di pacchetti funzioni, deve sapere da dove installare i pacchetti, e questo avviene sotto forma di un registro dei pacchetti. Il registro è un luogo centrale dove un pacchetto è pubblicato e quindi può essere installato. npm, oltre ad essere un gestore di pacchetti, è anche il nome del registro di pacchetti più comunemente usato per i pacchetti JavaScript. Il registro npm esiste su [npmjs.com](https://www.npmjs.com/).

npm non è l'unica opzione. Potresti gestire il tuo registro dei pacchetti — prodotti come [Microsoft Azure](https://azure.microsoft.com/) ti permettono di creare proxy al registro npm (quindi puoi sostituire o bloccare determinati pacchetti), [GitHub offre anche un servizio di registrazione dei pacchetti](https://docs.github.com/en/packages), e potrebbero esserci più opzioni che appariranno col tempo.

Ciò che è importante è che assicuri di aver scelto il miglior registro per te. Molti progetti useranno npm, e continueremo a farlo nei nostri esempi per il resto del modulo.

## Usare l'ecosistema dei pacchetti

Passiamo attraverso un esempio per iniziare a usare un gestore di pacchetti e il registro per installare un'utilità da riga di comando.

Useremo [Vite](https://vite.dev/) per creare un sito web vuoto. Nel prossimo articolo, espanderemo la toolchain per includere più strumenti e ti mostreremo come distribuire il sito.

Vite fornisce alcuni [template di inizializzazione](https://vite.dev/guide/#scaffolding-your-first-vite-project), con tutte le dipendenze e le configurazioni necessarie, per aiutarti a iniziare rapidamente in un progetto reale. A scopo dimostrativo, ne configureremo uno da zero, utilizzando il [template React](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react) come riferimento.

### Configurare l'app come un pacchetto npm

Prima di tutto, crea una nuova directory in cui conservare la nostra app sperimentale, in un posto sensato facile da ritrovare. La chiameremo `npm-experiment`, ma puoi chiamarla come preferisci:

```bash
mkdir npm-experiment
cd npm-experiment
```

Successivamente, iniziamo la nostra app come un pacchetto npm, che crea un file di configurazione — `package.json` — che ci consente di salvare i dettagli della configurazione nel caso in cui volessimo ricreare questo ambiente in seguito, o anche pubblicare il pacchetto nel registro npm (anche se non è rilevante per il nostro articolo, poiché stiamo sviluppando un'applicazione, non una libreria riutilizzabile).

Digita il seguente comando, assicurandoti di essere dentro la directory `npm-experiment`:

```bash
npm init
```

Ora ti verrà chiesto di rispondere a delle domande; npm creerà quindi un file predefinito `package.json` basato sulle risposte. Nota che nessuna di queste è rilevante per i nostri scopi perché vengono utilizzate solo se pubblichi il tuo pacchetto in un registro e altri vogliono installarlo e importarlo.

- `name`: Un nome per identificare l'app. Premi semplicemente <kbd>Return</kbd> per accettare il predefinito `npm-experiment`.
- `version`: Il numero di versione iniziale per l'app. Anche qui, premi <kbd>Return</kbd> per accettare il valore predefinito `1.0.0`.
- `description`: Una piccola descrizione dello scopo dell'app. Lo ometteremo qui, ma puoi inserire anche qualsiasi cosa ti venga in mente. Premi <kbd>Return</kbd>.
- `entry point`: Questo sarà il file JavaScript che verrà eseguito quando altri importano il tuo pacchetto. Non è utile per noi, quindi premi semplicemente <kbd>Return</kbd>.
- `test command`, `git repository`, e `keywords`: premi <kbd>Return</kbd> per lasciare ciascuno di questi campi vuoto per ora.
- `author`: L'autore del progetto. Digita il tuo nome e premi <kbd>Return</kbd>.
- `license`: La licenza sotto cui pubblicare il pacchetto. Premi <kbd>Return</kbd> per accettare il predefinito per ora.

Premi <kbd>Return</kbd> un'altra volta per accettare queste impostazioni.

Entra nella tua directory `npm-experiment` e ora dovresti trovare un file package.json. Aprilo e dovrebbe apparire qualcosa di simile a questo:

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

Aggiungeremo due altre righe a package.json:

- `"type": "module"`, che fa sì che Node interpreti tutti i file `.js` come [moduli ES](/it/docs/Web/JavaScript/Guide/Modules) piuttosto che i vecchi moduli CommonJS. È una buona abitudine da prendere.
- `"private": true`, che impedisce di pubblicare accidentalmente il tuo pacchetto nel registro npm.

Aggiungi queste righe subito sotto `"name"`:

```json
"name": "npm-experiment",
"type": "module",
"private": true,
```

Quindi questo è il file di configurazione che definisce il tuo pacchetto. Per ora è a posto, quindi andiamo avanti.

### Installazione di Vite

Inizieremo installando Vite, lo strumento di costruzione per il nostro sito web. È responsabile dell'impacchettamento dei file HTML, CSS e JavaScript in un bundle ottimizzato per il browser.

```bash
npm install --save-dev vite
```

Una volta fatto tutto il necessario, dai un'altra occhiata al tuo file package.json. Vedrai che npm ha aggiunto un nuovo campo, `devDependencies`:

```json
"devDependencies": {
  "vite": "^5.2.13"
}
```

Questa è parte della magia di npm — se in futuro sposti il tuo codice in un'altra posizione, su un'altra macchina, puoi ricreare la stessa configurazione eseguendo il comando `npm install`, e npm guarderà le dipendenze e le installerà per te.

Uno svantaggio è che Vite è disponibile solo all'interno della nostra app `npm-experiment`; non sarai in grado di eseguirla in una directory diversa. Ma i vantaggi superano gli svantaggi.

Nota che abbiamo scelto di installare `vite` come dipendenza di sviluppo. Questa differenza raramente importa per un'applicazione, ma per una libreria, significa che quando altri installano il tuo pacchetto, non installeranno implicitamente Vite. Di solito, per le applicazioni, qualsiasi pacchetto importato nel codice sorgente è una dipendenza reale, mentre qualsiasi pacchetto utilizzato per lo sviluppo (di solito come strumenti da riga di comando) è una dipendenza di sviluppo. Installa le dipendenze reali rimuovendo il flag `--save-dev`.

Troverai anche una serie di nuovi file creati:

- `node_modules`: I file di dipendenza richiesti per eseguire Vite. npm li ha scaricati tutti per te.
- `package-lock.json`: Questo è un file di blocco che memorizza le informazioni esatte necessarie per riprodurre la directory `node_modules`. Ciò assicura che finché il file di blocco rimane invariato, la directory `node_modules` sarà la stessa su macchine diverse.

Non devi preoccuparti di questi file, poiché sono gestiti da npm. Dovresti aggiungere `node_modules` al tuo file `.gitignore` se stai usando Git, ma dovresti generalmente mantenere `package-lock.json`, perché come detto viene usato per sincronizzare lo stato di `node_modules` su macchine diverse.

### Configurare la nostra app di esempio

Comunque, continuiamo con la configurazione.

In Vite, il file `index.html` è centrale. Definisce il punto di partenza della tua app, e Vite lo userà per trovare altri file necessari per costruire la tua app. Crea un file `index.html` nella tua directory `npm-experiment`, e dagli il seguente contenuto:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>My test page</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

Nota che gli elementi `<script>` creano una dipendenza da un file chiamato `src/main.jsx`, che dichiara il punto di ingresso della logica JavaScript per l'app. Crea la cartella `src` e crea `main.jsx` in questa cartella, ma lascialo vuoto per ora.

> [!NOTE]
> L'attributo [`type="module"`](/it/docs/Web/HTML/Reference/Elements/script/type) è importante. Indica al browser di trattare lo script come un modulo ES, il che ci permette di usare la sintassi `import` e `export` nel nostro codice JavaScript. L'estensione del file è `.jsx`, perché nel prossimo articolo aggiungeremo la sintassi JSX di React. I browser non comprendono JSX, ma Vite lo trasformerà in JavaScript regolare per noi, come se i browser lo comprendessero!

### Divertendoci con Vite

Ora faremo girare il nostro nuovo strumento Vite appena installato. Nel tuo terminale, esegui il comando seguente:

```bash
npx vite
```

Dovresti vedere qualcosa del genere stampato nel tuo terminale:

```plain
VITE v5.2.13  ready in 326 ms

➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
➜  press h + enter to show help
```

Ora siamo pronti per beneficiare dell'intero ecosistema dei pacchetti JavaScript. Per cominciare, c'è ora un server web locale in esecuzione su `http://localhost:5173`. Non vedrai niente per ora, ma ciò che è interessante è che quando apporti cambiamenti alla tua app, Vite la ricostruirà e aggiornerà automaticamente il server in modo da poter vedere istantaneamente l'effetto del tuo aggiornamento.

Puoi fermare il server di sviluppo in qualsiasi momento con <kbd>Ctrl</kbd> + <kbd>C</kbd> e riavviarlo con lo stesso comando. Se decidi di tenerlo in esecuzione, puoi aprire una nuova finestra del terminale per eseguire altri comandi.

Ora per un po' di contenuto della pagina. Come dimostrazione, aggiungiamo un grafico alla pagina. Useremo il pacchetto [plotly.js](https://www.npmjs.com/package/plotly.js), una libreria di visualizzazione dati. Installalo eseguendo il seguente comando:

```bash
npm install plotly.js-dist-min
```

Nota come lo stiamo installando senza il flag `--save-dev`. Come accennato in precedenza, questo è perché useremo effettivamente questo pacchetto nel nostro codice sorgente, non solo come strumento da riga di comando. Questo comando aggiungerà un nuovo oggetto `"dependencies"` al tuo file `package.json`, con `plotly.js-dist-min` al suo interno.

> [!NOTE]
> Qui, abbiamo scelto il pacchetto per te per completare il nostro compito. Quando stai scrivendo il tuo codice, pensa alle seguenti domande quando trovi e installi una dipendenza:
>
> - Ho davvero bisogno di una dipendenza? È possibile farlo con funzioni integrate, o è abbastanza semplice da scrivere io stesso?
> - Cosa esattamente devo fare? Più sei dettagliato, più è probabile che tu trovi un pacchetto che faccia esattamente ciò di cui hai bisogno. Puoi cercare parole chiave su npm o Google. Inoltre, preferisci pacchetti piccoli rispetto a quelli grandi, poiché i secondi potrebbero portare a problemi di prestazioni durante l'installazione, l'esecuzione, ecc.
> - La dipendenza è affidabile e ben mantenuta? Controlla quando è stata pubblicata l'ultima versione, chi è l'autore e quante volte il pacchetto viene scaricato settimanalmente. Determinare l'affidabilità di un pacchetto è una competenza che si acquisisce con l'esperienza, perché devi considerare fattori come la probabilità che il pacchetto abbia bisogno di aggiornamenti o quante persone potrebbero averne bisogno.

Nel file `src/main.jsx`, aggiungi il seguente codice e salva:

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

Torna su `http://localhost:5173` e vedrai un grafico sulla pagina. Modifica i diversi numeri e vedrai il grafico aggiornato ogni volta che salvi il tuo file.

### Compilare il nostro codice per la produzione

Tuttavia, questo codice non è pronto per la produzione. La maggior parte dei sistemi di build tooling, incluso Vite, hanno una "modalità di sviluppo" e una "modalità di produzione". La principale differenza è che molte delle caratteristiche utili che utilizzerai durante lo sviluppo non sono necessarie nel sito finale, quindi verranno rimosse per la produzione, ad esempio "sostituzione a caldo dei moduli", "ricaricamento live", e "codice sorgente non compresso e commentato". Anche se ben lontano dall'essere esaustivo, queste sono alcune delle comuni caratteristiche dello sviluppo web che sono molto utili in fase di sviluppo ma non molto utili in produzione. In produzione, saranno solo un peso per il tuo sito.

Ora fermiamo il server di sviluppo Vite in esecuzione utilizzando <kbd>Ctrl</kbd> + <kbd>C</kbd>.

Ora possiamo preparare il nostro sito di esempio di base per un'impegnativa distribuzione. Vite fornisce un comando `build` aggiuntivo per generare file adatti alla pubblicazione.

Esegui il seguente comando:

```bash
npx vite build
```

Dovresti vedere un output come questo:

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

Vite creerà una directory chiamata `dist`. Se la guardi, contiene un `index.html`, che sembra molto simile a quello principale, tranne che il sorgente dello `script` ora è sostituito con un percorso nella cartella `assets`. La cartella `assets` contiene l'output JavaScript trasformato, che ora è minimizzato e ottimizzato per la produzione.

> [!NOTE]
> Potresti essere preoccupato dell'avviso che c'è un chunk troppo grande. Questo è previsto perché stiamo caricando una libreria che fa molte cose dietro le quinte (immagina di scrivere tutto il codice da solo per disegnare lo stesso grafico). Per ora, non dobbiamo preoccuparcene.

## Una guida approssimativa ai client dei gestori di pacchetti

Questo tutorial ha installato il pacchetto Vite utilizzando npm, ma come menzionato in precedenza ci sono alcune alternative. Vale almeno la pena sapere che esistono e avere una qualche vaga idea dei comandi comuni tra gli strumenti. Hai già visto alcuni in azione, ma diamo un'occhiata agli altri.

L'elenco crescerà nel tempo, ma al momento della scrittura, i seguenti principali gestori di pacchetti sono disponibili:

- npm su [npmjs.org](https://www.npmjs.com/)
- pnpm su [pnpm.js.org](https://pnpm.js.org/)
- Yarn su [yarnpkg.com](https://yarnpkg.com/)

npm e pnpm sono simili dal punto di vista della riga di comando — in realtà, pnpm mira ad avere piena parità sulle opzioni degli argomenti che npm offre. Si differenzia per il fatto che utilizza un diverso metodo per scaricare e memorizzare i pacchetti sul tuo computer, mirato a ridurre lo spazio disco complessivo richiesto.

Dove npm è mostrato negli esempi seguenti, pnpm può essere scambiato e il comando funzionerà.

Yarn è spesso considerato più veloce di npm in termini di processo di installazione (anche se le tue esperienze potrebbero variare). Questo è importante per gli sviluppatori perché può esserci una quantità significativa di tempo sprecato aspettando che le dipendenze si installino (e si copino sul computer).

Tuttavia, vale la pena notare che il gestore di pacchetti npm **non** è obbligatorio per installare i pacchetti dal registro npm. pnpm e Yarn possono consumare lo stesso formato `package.json` di npm, e possono installare qualsiasi pacchetto dal registro npm e altri registri di pacchetti.

Rivediamo le azioni comuni che vorrai eseguire con i gestori di pacchetti.

> [!NOTE]
> Mostreremo sia i comandi npm che Yarn. Non sono destinati a essere eseguiti nello stesso progetto. Dovresti configurare il tuo progetto con npm o Yarn e utilizzare i comandi di quel gestore di pacchetti in modo coerente.

### Inizializzare un nuovo progetto

```bash
npm init
yarn init
```

Come mostrato sopra, ti chiederà e ti guiderà attraverso una serie di domande per descrivere il tuo progetto (nome, licenza, descrizione, e così via) e generare un `package.json` per te che contiene informazioni meta sul tuo progetto e le sue dipendenze.

### Installare le dipendenze

```bash
npm install vite
yarn add vite
```

Abbiamo anche visto `install` in azione sopra. Questo aggiungerebbe direttamente il pacchetto `vite` alla directory di lavoro in una sottodirectory chiamata `node_modules`, insieme alle dipendenze di `vite`.

Per impostazione predefinita, questo comando installerà l'ultima versione di `vite`, ma puoi controllarla anche tu. Puoi richiedere `vite@4`, che ti dà l'ultima versione 4.x (che è 4.5.3). Oppure potresti provare `vite@^4.0.0`, che significa l'ultima versione dopo o inclusa 4.0.0 (lo stesso significato di sopra).

### Aggiornamento delle dipendenze

```bash
npm update
yarn upgrade
```

Questo controllerà le dipendenze attualmente installate e le aggiornerà, se c'è un aggiornamento disponibile, entro l'intervallo specificato nel pacchetto.

L'intervallo è specificato nella versione della dipendenza nel tuo `package.json`, come `"vite": "^5.2.13"` — in questo caso, il carattere capello `^` significa tutti i rilasci minori e di patch successivi e inclusi al 5.2.13, fino a ma non compreso il 6.0.0.

Questo viene determinato usando un sistema chiamato [semver](https://semver.org/), che potrebbe sembrare un po' complicato dalla documentazione ma può essere semplificato considerando solo le informazioni di riepilogo e che una versione è rappresentata da `MAGGIORE.MINORE.CORREZIONE`, come 2.0.1 essendo la versione maggiore 2 con la correzione 1. Un modo eccellente per provare valori semver è usare il [calcolatore semver](https://semver.npmjs.com/).

È importante ricordare che `npm update` non aggiornerà le dipendenze oltre l'intervallo definito nel `package.json` — per farlo dovrai installare quella versione specificamente.

### Altri comandi

Puoi saperne di più sui singoli comandi per [npm](https://docs.npmjs.com/cli-documentation/) e [yarn](https://classic.yarnpkg.com/en/docs/cli/) online. Ancora, i comandi [pnpm](https://pnpm.io/cli/add) avranno parità con npm, con un certo numero di aggiunte.

## Creare i propri comandi

I gestori di pacchetti supportano anche la creazione dei propri comandi e la loro esecuzione dalla riga di comando. Per esempio, in precedenza abbiamo invocato il comando `vite` con `npx` per avviare il server di sviluppo Vite. Potremmo creare il seguente comando:

```bash
npm run dev
# or yarn run dev
```

Questo eseguirebbe uno script personalizzato per avviare il nostro progetto in "modalità sviluppo". In effetti, includiamo regolarmente questo in tutti i progetti poiché la configurazione di sviluppo locale tende a funzionare in modo leggermente diverso rispetto a come funzionerebbe in produzione.

Se provassi ad eseguirlo nel tuo progetto di test da prima, reclamerebbe (probabilmente) che lo script di "dev" manca. Questo perchè npm, Yarn (e simili) stanno cercando una proprietà chiamata `dev` nella proprietà `scripts` del tuo file `package.json`. Quindi, creiamo un comando scorciatoia personalizzato — "dev" — nel nostro `package.json`. Se hai seguito il tutorial da prima, dovresti avere un file `package.json` dentro la tua directory npm-experiment. Aprilo, e il suo membro `scripts` dovrebbe apparire così:

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
},
```

Aggiorna in modo che sembri così e salva il file:

```json
"scripts": {
  "dev": "vite"
},
```

Abbiamo aggiunto un comando `dev` personalizzato come uno script npm.

Ora prova a eseguire il seguente comando nel tuo terminale, assicurandoti di essere dentro la directory `npm-experiment`:

```bash
npm run dev
```

Questo dovrebbe avviare Vite e avviare lo stesso server di sviluppo locale, come visto in precedenza.

Nota che lo script che abbiamo definito qui non necessita più del prefisso `npx`. Questo perché i comandi npm (e yarn) sono intelligenti e cercheranno strumenti da riga di comando che sono installati localmente nel progetto prima di provare a trovarli attraverso metodi convenzionali (dove il tuo computer normalmente memorizza e consente al software di essere trovato). Puoi [sapere di più sui dettagli tecnici del comando `run`](https://docs.npmjs.com/cli/run-script/), sebbene nella maggior parte dei casi i tuoi propri script funzioneranno bene.

Questo particolare potrebbe sembrare non necessario — `npm run dev` sono più caratteri da digitare rispetto a `npx vite`, ma è una forma di _astrazione_. Ci permette di aggiungere più lavoro al comando `dev` in futuro, come impostare variabili d'ambiente, generare file temporanei, ecc., senza complicare il comando.

Puoi aggiungere ogni sorta di cose alla proprietà `scripts` che ti aiutano a fare il tuo lavoro. Ad esempio, ecco cosa raccomanda Vite nel template:

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview"
},
```

## Sommario

Questo ci porta alla fine del nostro tour sui gestori di pacchetti. La nostra prossima mossa è costruire una toolchain di esempio, mettendo in pratica tutto ciò che abbiamo imparato finora.

## Vedi anche

- [Riferimento agli script npm](https://docs.npmjs.com/cli/v8/using-npm/scripts/)
- [Riferimento al file package.json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json/)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Overview","Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}
