---
title: Concetti di base sulla gestione dei pacchetti
short-title: Gestione dei pacchetti
slug: Learn_web_development/Extensions/Client-side_tools/Package_management
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Overview","Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}

In questo articolo, esamineremo i gestori di pacchetti in dettaglio per capire come possiamo utilizzarli nei nostri progetti per installare le dipendenze degli strumenti del progetto, mantenerli aggiornati e altro ancora.

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
        Comprendere cosa sono i gestori di pacchetti e i repository di pacchetti, perché sono necessari e le basi di come usarli.
      </td>
    </tr>
  </tbody>
</table>

## Una dipendenza nel suo progetto

Una **dipendenza** è un software di terze parti che probabilmente è stato scritto da qualcun altro e idealmente risolve un singolo problema per lei. Un progetto web può avere un numero qualsiasi di dipendenze, che vanno da nessuna a molte, e le sue dipendenze possono includere sotto-dipendenze che non ha installato esplicitamente: le sue dipendenze possono avere le proprie dipendenze.

Un semplice esempio di una dipendenza utile che il suo progetto potrebbe necessitare è del codice per calcolare le date relative come testo leggibile. Potrebbe certamente scrivere questo codice da sola, ma c'è una buona possibilità che qualcun altro abbia già risolto questo problema: perché perdere tempo a reinventare la ruota? Inoltre, una dipendenza di terze parti affidabile sarà probabilmente stata testata in molte situazioni diverse, rendendola più robusta e compatibile tra browser rispetto alla sua soluzione.

Una dipendenza di progetto può essere un'intera libreria o un framework JavaScript — come React o Vue — o una piccola utility come la nostra libreria di date leggibili, o può essere uno strumento da riga di comando come Prettier o ESLint, di cui abbiamo parlato in articoli precedenti.

Senza strumenti di costruzione moderni, dipendenze come queste potrebbero essere incluse nel suo progetto utilizzando un semplice elemento [`<script>`](/it/docs/Web/HTML/Reference/Elements/script), ma potrebbe non funzionare immediatamente e probabilmente avrà bisogno di alcuni strumenti moderni per raggruppare il suo codice e le dipendenze insieme quando vengono rilasciate sul web. Un bundle è un termine generalmente usato per riferirsi a un singolo file sul suo server web che contiene tutto il JavaScript per il suo software, tipicamente compresso il più possibile per aiutare a ridurre il tempo necessario per scaricare e visualizzare il suo software nel browser dei visitatori.

Inoltre, cosa succede se trova uno strumento migliore che vuole utilizzare al posto di quello attuale, o viene rilasciata una nuova versione della sua dipendenza che vuole aggiornare? Questo non è troppo doloroso per un paio di dipendenze, ma in progetti più grandi con molte dipendenze, questo tipo di gestione può diventare davvero impegnativa. Ha più senso usare un **gestore di pacchetti** come npm, poiché questo garantirà che il codice venga aggiunto e rimosso in modo pulito, oltre a una serie di altri vantaggi.

## Che cos'è esattamente un gestore di pacchetti?

Abbiamo già incontrato [npm](https://www.npmjs.com/), ma facendo un passo indietro rispetto a npm stesso, un gestore di pacchetti è un sistema che gestirà le dipendenze del suo progetto. 

Il gestore di pacchetti fornirà un metodo per installare nuove dipendenze (denominate anche "pacchetti"), gestire dove i pacchetti sono memorizzati sul suo file system e offrire funzionalità per pubblicare i suoi pacchetti.

In teoria, potrebbe non aver bisogno di un gestore di pacchetti e potrebbe scaricare e memorizzare manualmente le dipendenze del suo progetto, ma un gestore di pacchetti gestirà senza problemi l'installazione e la disinstallazione dei pacchetti. Se non ne utilizzasse uno, dovrebbe gestire manualmente:

- Trovare tutti i file JavaScript del pacchetto corretti.
- Verificarli per assicurarsi che non abbiano vulnerabilità note.
- Scaricarli e metterli nelle posizioni corrette nel suo progetto.
- Scrivere il codice per includere il/i pacchetto/i nella sua applicazione (questo tenderebbe a essere fatto usando [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules), un altro argomento che vale la pena approfondire e comprendere).
- Fare lo stesso per tutte le sotto-dipendenze dei pacchetti, che potrebbero essere decine o centinaia.
- Rimuovere nuovamente tutti i file se desidera rimuovere i pacchetti.

Inoltre, i gestori di pacchetti gestiscono le dipendenze duplicate (qualcosa che diventa importante e comune nello sviluppo frontend). 

Nel caso di npm (e gestori di pacchetti basati su JavaScript e Node) ha due opzioni per dove installare le sue dipendenze. Come abbiamo toccato nell'articolo precedente, le dipendenze possono essere installate globalmente o localmente nel suo progetto. Anche se ci sono molti più vantaggi nell'installare globalmente, i vantaggi dell'installazione locale sono più importanti, come la portabilità del codice e il blocco delle versioni.

Ad esempio, se il suo progetto si basava su webpack con una certa configurazione, vorrebbe garantire che se installasse quel progetto su un'altra macchina o lo rivisitasse molto più tardi, la configurazione funzionerebbe ancora. Se venisse installata una versione diversa di webpack, potrebbe non essere compatibile. Per mitigare questo, le dipendenze vengono installate localmente in un progetto.

Per vedere le dipendenze locali brillare davvero, tutto quello che deve fare è provare a scaricare ed eseguire un progetto esistente: se funziona e tutte le dipendenze funzionano immediatamente, allora deve ringraziare le dipendenze locali per il fatto che il codice è portabile.

> [!NOTE]
> npm non è l'unico gestore di pacchetti disponibile. Un'alternativa popolare e di successo è [Yarn](https://yarnpkg.com/). Yarn risolve le dipendenze utilizzando un diverso algoritmo che può significare un'esperienza utente più veloce. Esistono anche una serie di altri client emergenti, come [pnpm](https://pnpm.js.org/).

## Registri di pacchetti

Per funzionare, un gestore di pacchetti ha bisogno di sapere da dove installare i pacchetti, e questo avviene sotto forma di un registro dei pacchetti. Il registro è un luogo centrale dove un pacchetto viene pubblicato e quindi può essere installato. npm, oltre ad essere un gestore di pacchetti, è anche il nome del registro dei pacchetti più comunemente usato per i pacchetti JavaScript. Il registro npm esiste su [npmjs.com](https://www.npmjs.com/).

npm non è l'unica opzione. Potrebbe gestire il proprio registro dei pacchetti — prodotti come [Microsoft Azure](https://azure.microsoft.com/) le consentono di creare proxy per il registro npm (quindi può sovrascrivere o bloccare determinati pacchetti), [GitHub offre anche un servizio di registro dei pacchetti](https://docs.github.com/en/packages), e ci saranno probabilmente più opzioni disponibili man mano che il tempo passa.

Ciò che è importante è che garantisca di aver scelto il registro migliore per lei. Molti progetti utilizzeranno npm e noi ci atterremo a questo nei nostri esempi per il resto del modulo.

## Utilizzare l'ecosistema dei pacchetti

Facciamo un esempio per aiutarla a iniziare a utilizzare un gestore di pacchetti e un registro per installare un'utilità da riga di comando.

Usiamo [Vite](https://vite.dev/) per creare un sito Web vuoto. Nel prossimo articolo, espanderemo la catena di strumenti per includere più strumenti e mostrarle come distribuire il sito.

Vite fornisce alcuni [template di inizializzazione](https://vite.dev/guide/#scaffolding-your-first-vite-project), con tutte le dipendenze e configurazioni necessarie, per farla partire rapidamente in un vero progetto. Per dimostrazione, configureremo uno da zero, utilizzando il [template React](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react) come riferimento.

### Configurazione dell'app come pacchetto npm

Prima di tutto, crei una nuova directory per memorizzare la nostra app sperimentale, in un luogo appropriato che ritroverà facilmente. La chiameremo `npm-experiment`, ma può chiamarla come preferisce:

```bash
mkdir npm-experiment
cd npm-experiment
```

Successivamente, inizializziamo la nostra app come pacchetto npm, il che crea un file di configurazione — `package.json` — che ci consente di salvare i dettagli della nostra configurazione nel caso vogliamo ricreare questo ambiente in seguito, o addirittura pubblicare il pacchetto nel registro npm (anche se non è rilevante per il nostro articolo, perché stiamo sviluppando un'applicazione, non una libreria riutilizzabile).

Digiti il seguente comando, assicurandosi di trovarsi all'interno della directory `npm-experiment`:

```bash
npm init
```

Ora le verranno poste alcune domande; npm creerà quindi un file `package.json` predefinito in base alle risposte. Nota che nessuna di queste è rilevante per i nostri scopi perché vengono utilizzate solo se pubblica il pacchetto in un registro e altri vogliono installarlo e importarlo.

- `name`: Un nome per identificare l'app. Basta premere <kbd>Invio</kbd> per accettare il valore predefinito `npm-experiment`.
- `version`: Il numero di versione iniziale per l'app. Di nuovo, basta premere <kbd>Invio</kbd> per accettare il valore predefinito `1.0.0`.
- `description`: Una breve descrizione dello scopo dell'app. Lo ometteremo qui, ma può inserire qualsiasi cosa desidera. Premi <kbd>Invio</kbd>.
- `entry point`: Questo sarà il file JavaScript che viene eseguito quando altri importano il suo pacchetto. Non ci interessa, quindi premo <kbd>Invio</kbd>.
- `test command`, `git repository` e `keywords`: premi <kbd>Invio</kbd> per lasciare ognuno di questi vuoto per ora.
- `author`: L'autore del progetto. Digiti il suo nome, e premi <kbd>Invio</kbd>.
- `license`: La licenza per pubblicare il pacchetto. Premi <kbd>Invio</kbd> per accettare il valore predefinito per ora.

Premi <kbd>Invio</kbd> un'altra volta per accettare queste impostazioni.

Entri nella sua directory `npm-experiment` e dovrebbe trovare un file package.json all'interno. Lo apra e dovrebbe assomigliare a questo:

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

Aggiungeremo altre due righe al file package.json:

- `"type": "module"`, che fa sì che Node interpreti tutti i file `.js` come [moduli ES](/it/docs/Web/JavaScript/Guide/Modules) anziché i vecchi moduli CommonJS. È buona abitudine prendere l'abitudine di farlo.
- `"private": true`, che impedisce di pubblicare accidentalmente il pacchetto nel registro npm.

Aggiunga queste righe subito sotto `"name"`:

```json
"name": "npm-experiment",
"type": "module",
"private": true,
```

Quindi, questo è il file di configurazione che definisce il suo pacchetto. Questo è sufficiente per ora, quindi procediamo.

### Installazione di Vite

Installeremo prima Vite, lo strumento di build per il nostro sito web. È responsabile di raggruppare i nostri file HTML, CSS e JavaScript in un bundle ottimizzato per il browser.

```bash
npm install --save-dev vite
```

Una volta completato tutto, dia un'altra occhiata al suo file package.json. Vedrà che npm ha aggiunto un nuovo campo, `devDependencies`:

```json
"devDependencies": {
  "vite": "^5.2.13"
}
```

Questa è parte della magia di npm — se in futuro sposta il suo codice in un'altra posizione, su un'altra macchina, può ricreare la stessa configurazione eseguendo il comando `npm install`, e npm esaminerà le dipendenze e le installerà per lei.

Uno svantaggio è che Vite è disponibile solo all'interno della nostra app `npm-experiment`; non potrà eseguirlo in una directory diversa. Ma i vantaggi superano gli svantaggi.

Nota che abbiamo scelto di installare `vite` come dipendenza di sviluppo. Questa differenza raramente importa per un'applicazione, ma per una libreria, significa che quando altri installano il suo pacchetto, non installeranno implicitamente Vite. Di solito, per le applicazioni, qualsiasi pacchetto importato nel codice sorgente è una vera dipendenza, mentre qualsiasi pacchetto utilizzato per lo sviluppo (solitamente come strumenti da riga di comando) è una dipendenza di sviluppo. Installi le vere dipendenze rimuovendo il flag `--save-dev`.

Troverà anche una serie di nuovi file creati:

- `node_modules`: I file di dipendenza richiesti per eseguire Vite. npm li ha scaricati tutti per lei.
- `package-lock.json`: Questo è un file di blocco che memorizza le informazioni esatte necessarie per riprodurre la directory `node_modules`. Questo assicura che fintanto che il file di blocco non viene modificato, la directory `node_modules` sarà la stessa su diversi computer.

Non dovrebbe preoccuparsi di questi file, poiché sono gestiti da npm. Dovrebbe aggiungere `node_modules` al suo file `.gitignore` se sta utilizzando Git, ma dovrebbe generalmente mantenere `package-lock.json`, perché come menzionato viene utilizzato per sincronizzare lo stato di `node_modules` su diverse macchine.

### Configurazione della nostra app di esempio

Comunque, continuiamo con la configurazione.

In Vite, il file `index.html` è centrale. Definisce il punto di partenza della sua app, e Vite lo utilizzerà per trovare altri file necessari per costruire la sua app. Crei un file `index.html` nella sua directory `npm-experiment`, e gli dia il seguente contenuto:

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

Nota che gli elementi `<script>` creano una dipendenza su un file chiamato `src/main.jsx`, che dichiara il punto di ingresso della logica JavaScript per l'app. Crei la cartella `src` e crei `main.jsx` in questa cartella, ma per ora lo lasci vuoto.

> [!NOTE]
> L'attributo [`type="module"`](/it/docs/Web/HTML/Reference/Elements/script/type) è importante. Dice al browser di trattare lo script come un modulo ES, il che ci consente di utilizzare la sintassi `import` e `export` nel nostro codice JavaScript. L'estensione del file è `.jsx`, perché nel prossimo articolo aggiungeremo la sintassi JSX di React. I browser non comprendono JSX, ma Vite lo trasformerà in semplice JavaScript per noi, come se i browser lo facessero!

### Divertirsi con Vite

Ora eseguiamo il nostro strumento Vite appena installato. Nel suo terminale, esegua il seguente comando:

```bash
npx vite
```

Dovrebbe vedere qualcosa del genere stampato nel suo terminale:

```plain
VITE v5.2.13  ready in 326 ms

➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
➜  press h + enter to show help
```

Ora siamo pronti per beneficiare dell'intero ecosistema dei pacchetti JavaScript. Per cominciare, c'è ora un server web locale in esecuzione su `http://localhost:5173`. Non vedrà nulla per ora, ma ciò che è interessante è che quando apporta modifiche alla sua app, Vite la ricomporrà e aggiornerà il server automaticamente in modo che possa vedere istantaneamente l'effetto delle sue modifiche.

Può interrompere il server di sviluppo in qualsiasi momento con <kbd>Ctrl</kbd> + <kbd>C</kbd> e avviarlo nuovamente con lo stesso comando. Se decide di tenerlo in esecuzione, può aprire una nuova finestra del terminale per eseguire altri comandi.

Ora per qualche contenuto della pagina. Come dimostrazione, aggiungiamo un grafico alla pagina. Useremo il pacchetto [plotly.js](https://www.npmjs.com/package/plotly.js), una libreria di visualizzazione dei dati. Lo installi eseguendo il seguente comando:

```bash
npm install plotly.js-dist-min
```

Nota come lo stiamo installando senza il flag `--save-dev`. Come accennato in precedenza, questo è perché useremo effettivamente questo pacchetto nel nostro codice sorgente, non solo come strumento da riga di comando. Questo comando aggiungerà un nuovo oggetto `"dependencies"` al suo file `package.json`, con all'interno `plotly.js-dist-min`.

> [!NOTE]
> Qui, abbiamo scelto il pacchetto per lei per completare il nostro compito. Quando scrive il proprio codice, pensi alle seguenti domande quando trova e installa una dipendenza:
>
> - Ho davvero bisogno di una dipendenza? È possibile farlo con le funzioni integrate o è abbastanza semplice da scrivere da sola?
> - Cosa esattamente devo fare? Più dettagliata è la sua descrizione, più è probabile che trovi un pacchetto che fa esattamente ciò di cui ha bisogno. Può cercare parole chiave su npm o su Google. Preferisca anche pacchetti piccoli a quelli grandi, poiché questi ultimi possono portare a problemi di performance durante l'installazione, l'esecuzione, ecc.
> - La dipendenza è affidabile e ben mantenuta? Controlli quando è stata pubblicata l'ultima versione, chi è l'autore e quante sono le installazioni settimanali del pacchetto. Determinare l'affidabilità di un pacchetto è una competenza che si acquisisce con l'esperienza, perché deve tener conto di fattori come la probabilità che il pacchetto necessiti di aggiornamenti o quante persone potrebbero averne bisogno.

Nel file `src/main.jsx`, aggiunga il seguente codice e salvi:

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

Torni su `http://localhost:5173` e vedrà un grafico sulla pagina. Modifichi i diversi numeri e vedrà il grafico aggiornato ogni volta che salva il suo file.

### Preparare il codice per la produzione

Tuttavia, questo codice non è pronto per la produzione. La maggior parte dei sistemi di strumenti di build, inclusi Vite, ha una "modalità di sviluppo" e una "modalità di produzione". La differenza importante è che molte delle funzionalità utili che utilizzerà nello sviluppo non sono necessarie nel sito finale, quindi verranno rimosse per la produzione, ad esempio, "sostituzione dei moduli a caldo", "ricaricamento live" e "codice sorgente non compresso e commentato". Anche se lontano dall'essere esaustivo, queste sono alcune delle funzionalità comuni per lo sviluppo web che sono molto utili nella fase di sviluppo ma non molto utili in produzione. In produzione, faranno solo aumentare il peso del suo sito.

Ora interrompa il server di sviluppo Vite in esecuzione utilizzando <kbd>Ctrl</kbd> + <kbd>C</kbd>.

Possiamo ora preparare il nostro sito di esempio semplice per una distribuzione immaginaria. Vite fornisce un comando aggiuntivo `build` per generare file adatti alla pubblicazione.

Esegua il seguente comando:

```bash
npx vite build
```

Dovrebbe vedere un'output come questa:

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

Vite creerà una directory chiamata `dist`. Se la guarda all'interno, contiene un `index.html`, che sembra molto simile a quello della radice, tranne per il fatto che l'origine dello `script` è ora sostituita con un percorso alla cartella `assets`. La cartella `assets` contiene l'output JavaScript trasformato, che ora è minificato e ottimizzato per la produzione.

> [!NOTE]
> Potrebbe essere preoccupata per l'avviso che un chunk è troppo grande. Questo è previsto perché stiamo caricando una libreria che fa molte cose dietro le quinte (immagini scrivere tutto il codice da sola per disegnare lo stesso grafico). Per ora, non dobbiamo preoccuparcene.

## Una guida sommaria ai client gestori di pacchetti

Questo tutorial ha installato il pacchetto Vite usando npm, ma come menzionato in precedenza ci sono alcune alternative. Vale la pena almeno sapere che esistono e avere un'idea vaga dei comandi comuni tra gli strumenti. Ha già visto alcuni in azione, ma diamo un'occhiata agli altri.

L'elenco crescerà nel tempo, ma al momento della scrittura sono disponibili i seguenti principali gestori di pacchetti:

- npm su [npmjs.org](https://www.npmjs.com/)
- pnpm su [pnpm.js.org](https://pnpm.js.org/)
- Yarn su [yarnpkg.com](https://yarnpkg.com/)

npm e pnpm sono simili dal punto di vista della riga di comando — infatti, pnpm punta ad avere la piena parità sulle opzioni di argomenti che npm offre. Si differenzia in quanto utilizza un metodo diverso per scaricare e memorizzare i pacchetti sul suo computer, puntando a ridurre lo spazio su disco complessivo richiesto.

Dove npm è mostrato negli esempi di seguito, pnpm può essere sostituito e il comando funzionerà.

Yarn è spesso pensato per essere più veloce di npm in termini di processo di installazione (anche se la sua esperienza personale può variare). Questo è importante per gli sviluppatori perché ci può essere una quantità significativa di tempo perso nell'attesa che le dipendenze vengano installate (e copiate sul computer).

Tuttavia, è importante notare che il gestore di pacchetti npm **non** è richiesto per installare pacchetti dal registro npm. pnpm e Yarn possono utilizzare lo stesso formato `package.json` di npm e possono installare qualsiasi pacchetto dal registro npm e da altri registri di pacchetti.

Rivediamo le azioni comuni che desidererà eseguire con i gestori di pacchetti.

> [!NOTE]
> Dimostreremo sia i comandi npm che Yarn. Non sono destinati a essere eseguiti nello stesso progetto. Dovrebbe impostare il suo progetto con npm o Yarn e utilizzare i comandi di quel gestore di pacchetti in modo coerente.

### Inizializzare un nuovo progetto

```bash
npm init
yarn init
```

Come mostrato sopra, questo la guiderà attraverso una serie di domande per descrivere il suo progetto (nome, licenza, descrizione, ecc.) e genererà un `package.json` per lei che contiene meta-informazioni sul suo progetto e le sue dipendenze.

### Installazione delle dipendenze

```bash
npm install vite
yarn add vite
```

Abbiamo visto `install` in azione sopra. Questo aggiungerebbe direttamente il pacchetto `vite` alla directory di lavoro in una sottodirectory chiamata `node_modules`, insieme alle proprie dipendenze di `vite`.

Per impostazione predefinita, questo comando installerà l'ultima versione di `vite`, ma può controllarlo anche lei. Può richiedere `vite@4`, che le darà l'ultima versione 4.x (che è 4.5.3). Oppure potrebbe provare `vite@^4.0.0`, che significa l'ultima versione dopo o inclusa 4.0.0 (lo stesso significato di sopra).

### Aggiornamento delle dipendenze

```bash
npm update
yarn upgrade
```

Questo controllerà le dipendenze attualmente installate e le aggiornerà, se c'è un aggiornamento disponibile, all'interno dell'intervallo specificato nel pacchetto.

L'intervallo è specificato nella versione della dipendenza nel suo `package.json`, come `"vite": "^5.2.13"` — in questo caso, il carattere circunflesso `^` significa tutti i rilasci minori e patch successivi e inclusi 5.2.13, fino a ma escluso 6.0.0.

Questo è determinato utilizzando un sistema chiamato [semver](https://semver.org/), che potrebbe sembrare un po' complicato dalla documentazione ma può essere semplificato considerando solo le informazioni di sintesi e che una versione è rappresentata da `MAJOR.MINOR.PATCH`, come 2.0.1 che è la versione principale 2 con versione patch 1. Un ottimo modo per provare i valori semver è utilizzare il [calcolatore semver](https://semver.npmjs.com/).

È importante ricordare che `npm update` non aggiornerà le dipendenze oltre l'intervallo definito nel `package.json` — per farlo sarà necessario installare quella versione specificamente.

### Altri comandi

Può trovare ulteriori informazioni sui comandi individuali per [npm](https://docs.npmjs.com/cli-documentation/) e [yarn](https://classic.yarnpkg.com/en/docs/cli/) online. Di nuovo, i comandi [pnpm](https://pnpm.io/cli/add) avranno parità con npm, con un paio di aggiunte.

## Creare i propri comandi

I gestori di pacchetti supportano anche la creazione dei propri comandi e la loro esecuzione dalla riga di comando. Ad esempio, in precedenza abbiamo invocato il comando `vite` con `npx` per avviare il server di sviluppo Vite. Potremmo creare il seguente comando:

```bash
npm run dev
# or yarn run dev
```

Questo eseguirebbe uno script personalizzato per avviare il nostro progetto in "modalità sviluppo". In effetti, lo includiamo regolarmente in tutti i progetti poiché la configurazione di sviluppo locale tende a funzionare in modo leggermente diverso da come funzionerebbe in produzione.

Se provasse a eseguire questo nel suo progetto di test precedente, probabilmente reclamerebbe che "lo script dev manca". Questo perché npm, Yarn (e simili) stanno cercando una proprietà chiamata `dev` nella proprietà `scripts` del suo file `package.json`. Quindi, creiamo un comando shorthand personalizzato — "dev" — nel nostro `package.json`. Se ha seguito il tutorial di prima, dovrebbe avere un file `package.json` all'interno della sua directory npm-experiment. Lo apra, e il suo membro `scripts` dovrebbe assomigliare a questo:

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
},
```

Lo aggiorni in modo che assomigli a questo, e salvi il file:

```json
"scripts": {
  "dev": "vite"
},
```

Abbiamo aggiunto un comando `dev` personalizzato come uno script npm.

Ora provi a eseguire il seguente nel suo terminale, assicurandosi di trovarsi all'interno della directory `npm-experiment`:

```bash
npm run dev
```

Questo dovrebbe avviare Vite e avviare lo stesso server di sviluppo locale, come abbiamo visto prima.

Nota che lo script che abbiamo definito qui non necessita più del prefisso `npx`. Questo perché i comandi npm (e yarn) sono intelligenti in quanto cercheranno strumenti da riga di comando installati localmente nel progetto prima di provare a trovarli attraverso metodi convenzionali (dove il suo computer normalmente memorizzerà e consentirà di trovare il software). Può [approfondire le complessità tecniche del comando `run`](https://docs.npmjs.com/cli/run-script/), anche se nella maggior parte dei casi i suoi script funzioneranno bene.

Questo particolare potrebbe sembrare non necessario — `npm run dev` sono più caratteri da digitare rispetto a `npx vite`, ma è una forma di _astrazione_. Ci permette di aggiungere più lavoro al comando `dev` in futuro, come l'impostazione di variabili d'ambiente, la generazione di file temporanei, ecc., senza complicare il comando.

Può aggiungere ogni sorta di cose alla proprietà `scripts` che la aiuteranno a svolgere il suo lavoro. Ad esempio, ecco cosa raccomanda Vite nel template:

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview"
},
```

## Riepilogo

Con questo arriviamo alla fine del nostro tour dei gestori di pacchetti. La nostra prossima mossa è costruire una catena di strumenti di esempio, mettendo in pratica tutto ciò che abbiamo imparato finora.

## Vedi anche

- [Riferimento agli script di npm](https://docs.npmjs.com/cli/v8/using-npm/scripts/)
- [Riferimento a package.json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json/)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Overview","Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}
