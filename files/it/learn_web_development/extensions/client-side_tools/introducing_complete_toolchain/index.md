---
title: Introduzione a una catena di strumenti completa
short-title: Catena di strumenti di esempio
slug: Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Package_management","Learn_web_development/Extensions/Client-side_tools/Deployment", "Learn_web_development/Extensions/Client-side_tools")}}

Negli ultimi due articoli della serie, consolidiamo le sue conoscenze sugli strumenti guidandola nel processo di costruzione di una catena di strumenti di caso studio di esempio. Passeremo dall'impostazione di un ambiente di sviluppo sensato e dall'implementazione di strumenti di trasformazione fino al reale deployment della sua app. In questo articolo, introdurremo il caso studio, imposteremo il nostro ambiente di sviluppo e configureremo i nostri strumenti di trasformazione del codice.

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
        Consolidare ciò che abbiamo imparato finora lavorando attraverso uno studio di caso di una catena di strumenti completa.
      </td>
    </tr>
  </tbody>
</table>

Esistono davvero combinazioni illimitate di strumenti e modi per usarli; ciò che vede in questo articolo e nel prossimo è solo _uno_ dei modi in cui gli strumenti presentati possono essere utilizzati per un progetto.

> [!NOTE]
> Vale anche la pena ripetere che non tutti questi strumenti devono essere eseguiti tramite la riga di comando. Molti degli editor di codice attuali (come VS Code) dispongono di supporto per un'integrazione con _molti_ strumenti tramite plugin.

## Introduzione al nostro caso di studio

La catena di strumenti che stiamo creando in questo articolo sarà utilizzata per costruire e distribuire un mini-sito che mostra dati riguardanti il repository [mdn/content](https://github.com/mdn/content), ottenendo i suoi dati dall'[API di GitHub](https://docs.github.com/en/rest/metrics/community).

## Strumenti usati nella nostra catena di strumenti

In questo articolo utilizzeremo i seguenti strumenti e funzionalità:

- [JSX](https://react.dev/learn/writing-markup-with-jsx), un set di estensioni sintattiche correlato a [React](https://react.dev/) che le permette di definire, ad esempio, strutture di componenti all'interno di JavaScript. Non è necessario conoscere React per seguire questo tutorial, ma lo abbiamo incluso per darle un'idea di come un linguaggio web non nativo possa essere integrato in una catena di strumenti.
- Le ultime funzionalità integrate di JavaScript (al momento della scrittura), come [`import`](/it/docs/Web/JavaScript/Reference/Statements/import).
- Utili strumenti di sviluppo come [Prettier](https://prettier.io/) per la formattazione e [ESLint](https://eslint.org/) per il linting.
- [PostCSS](https://postcss.org/) per fornire capacità di nesting CSS.
- [Vite](https://vite.dev/) per costruire e minimizzare il nostro codice e per scrivere automaticamente una serie di contenuti di file di configurazione.
- [GitHub](/it/docs/Learn_web_development/Core/Version_control) per gestire il controllo del codice sorgente e infine per distribuire il nostro sito (utilizzando GitHub Pages).

Potrebbe non avere familiarità con tutte le caratteristiche e gli strumenti sopra indicati o con il loro funzionamento, ma non si preoccupi: spiegheremo ciascuna parte man mano che procediamo con questo articolo.

## Catene di strumenti e la loro complessità intrinseca

Come per qualsiasi catena, più legami ci sono nella sua catena di strumenti, più è complessa e potenzialmente fragile: ad esempio, potrebbe essere più complesso da configurare e più facile da interrompere. Al contrario, meno legami ci sono, più la catena di strumenti è probabilmente resiliente.

Tutti i progetti web saranno diversi e lei deve considerare quali parti della sua catena di strumenti sono necessarie e considerare attentamente ciascuna parte.

La più piccola catena di strumenti è quella che non ha alcun legame. Codificherebbe manualmente l'HTML, utilizzerebbe "vanilla JavaScript" (significa nessun framework o linguaggi intermediari) e caricherebbe manualmente tutto su un server per l'hosting.

Tuttavia, requisiti software più complicati trarranno probabilmente vantaggio dall'uso di strumenti per aiutare a semplificare il processo di sviluppo. Inoltre, dovrebbe includere test prima di distribuire sul suo server di produzione per garantire che il suo software funzioni come previsto: questo già suona come una catena di strumenti necessaria.

Per il nostro progetto di esempio, utilizzeremo una catena di strumenti specificamente progettata per supportare il nostro sviluppo software e supportare le scelte tecniche fatte durante la fase di progettazione del software. Tuttavia, eviteremo qualsiasi strumento superfluo, con l'obiettivo di mantenere la complessità al minimo.

## Verifica dei prerequisiti

Se ha seguito i capitoli precedenti, dovrebbe già avere la maggior parte dei componenti software. Ecco cosa dovrebbe avere prima di procedere con i passaggi reali di configurazione. Devono essere effettuati solo una volta e non necessita di ripeterli per progetti futuri.

### Creazione di un account GitHub

Oltre agli strumenti che installeremo che contribuiscono alla nostra catena di strumenti, dovrà creare un account con GitHub se desidera completare il tutorial. Tuttavia, può ancora seguire la parte di sviluppo locale senza di esso. Come detto in precedenza, GitHub è un servizio di repository di codice sorgente che aggiunge funzionalità comunitarie come il tracciamento delle issue, il seguire i rilasci del progetto e molto altro. Nel prossimo capitolo, caricheremo su un repository di codice GitHub, il che causerà un effetto a cascata che (dovrebbe) distribuire tutto il software su una casa sul web.

Registrarsi su [GitHub](https://github.com/) cliccando sul link _Sign Up_ sulla homepage se non ha già un account e seguire le istruzioni.

### Installazione di git

Installeremo un altro software, git, per aiutare con il controllo delle revisioni.

È possibile che abbia già sentito parlare di "git". [Git](https://git-scm.com/) è attualmente lo strumento di controllo delle revisioni del codice sorgente più popolare disponibile per gli sviluppatori: il controllo delle revisioni offre molti vantaggi, come un modo per fare il backup del suo lavoro in un luogo remoto e un meccanismo per lavorare in un team sullo stesso progetto senza il timore di sovrascrivere il codice reciproco.

Potrebbe sembrare ovvio per alcuni, ma vale la pena ripeterlo: Git non è la stessa cosa di GitHub. Git è lo strumento di controllo delle revisioni, mentre [GitHub](https://github.com/) è un negozio online per repository git (oltre a una serie di strumenti utili per lavorare con essi). Noti che, sebbene stiamo utilizzando GitHub in questo capitolo, ci sono diverse alternative tra cui [GitLab](https://about.gitlab.com/) e [Bitbucket](https://www.atlassian.com/software/bitbucket), e potrebbe anche ospitare i suoi repository git.

Usare il controllo delle revisioni nei suoi progetti e includerlo come parte della catena di strumenti aiuterà a gestire l'evoluzione del suo codice. Offre un modo per "collegare" blocchi di lavoro man mano che si procede, con commenti come "X nuova funzione implementata" o "Bug Z ora risolto grazie a Y modifiche".

Il controllo delle revisioni può anche permetterle di _ramificare_ il codice del suo progetto, creando una versione separata e provando nuove funzionalità, senza che quelle modifiche influenzino il codice originale.

Infine, può aiutarla a annullare le modifiche o ripristinare il codice a un tempo "quando funzionava" se è stato introdotto un errore da qualche parte e sta avendo difficoltà a risolverlo: qualcosa che tutti gli sviluppatori devono fare di tanto in tanto!

Git può essere [scaricato e installato tramite il sito web git-scm](https://git-scm.com/downloads) — scarichi il programma di installazione pertinente per il suo sistema, lo esegua e segua le istruzioni a schermo. Questo è tutto ciò che deve fare per ora.

Può interagire con git in diversi modi, dall'utilizzo della riga di comando per emettere comandi, all'utilizzo di un [applicazione GUI git](https://git-scm.com/downloads/guis) per emettere gli stessi comandi premendo pulsanti o persino direttamente all'interno del suo editor di codice, come visto nell'esempio di Visual Studio Code di seguito:

![Integrazione di Git mostrata in VS Code](vscode-git.png)

### Progetto esistente

Costruiremo sul progetto che abbiamo già iniziato nel capitolo precedente, quindi si assicuri di seguire le istruzioni in [Gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per configurare prima il progetto. Per ricapitolare, ecco cosa dovrebbe avere:

- Node.js e npm installati.
- Un nuovo progetto chiamato `npm-experiment` (o qualche altro nome).
- Vite installato come dipendenza di sviluppo.
- Il pacchetto `plotly.js-dist-min` installato come dipendenza.
- Alcuni script personalizzati definiti in package.json.
- I file `index.html` e `src/main.jsx` creati.

Come abbiamo parlato in [Capitolo 1](/it/docs/Learn_web_development/Extensions/Client-side_tools/Overview), la catena di strumenti sarà strutturata nelle seguenti fasi:

- **Ambiente di sviluppo**: Gli strumenti più fondamentali per eseguire il suo codice. Questa parte è già configurata nel capitolo precedente.
- **Rete di sicurezza**: Rendere l'esperienza di sviluppo software stabile e più efficiente. Potremmo anche riferirci a questo come il nostro ambiente di sviluppo.
- **Trasformazione**: Strumenti che ci permettono di usare le ultime funzionalità di un linguaggio (ad es. JavaScript) o un altro linguaggio interamente (ad es. JSX o TypeScript) nel nostro processo di sviluppo, e poi trasformano il nostro codice in modo che la versione di produzione funzioni ancora su una vasta gamma di browser, moderni e più vecchi.
- **Post sviluppo**: Strumenti che entrano in gioco dopo che ha finito il corpo dello sviluppo per garantire che il suo software arrivi sul web e continui a funzionare. In questo caso studio, guarderemo all'aggiunta di test al suo codice e alla distribuzione della sua app usando GitHub Pages in modo che sia disponibile per tutto il web.

Cominciamo a lavorare su questi, iniziando dal nostro ambiente di sviluppo. Seguiremo gli stessi passaggi di come un progetto reale verrebbe configurato, quindi in futuro, se sta configurando un nuovo progetto, può riferirsi a questo capitolo e ripetere i passaggi.

## Creare un ambiente di sviluppo

Questa parte della catena di strumenti è a volte vista come un ritardo del lavoro effettivo, e può essere molto facile cadere in una "tana del coniglio" degli strumenti dove passa molto tempo cercando di ottenere l'ambiente "appena giusto".

Ma può guardare a questo nello stesso modo in cui imposta il suo ambiente di lavoro fisico. La sedia deve essere comoda e posizionata in una buona posizione per aiutare la sua postura. Ha bisogno di corrente, Wi-Fi e porte USB! Ci potrebbero essere decorazioni importanti o musica che aiutano il suo stato mentale: questi sono tutti importanti per fare il suo lavoro migliore possibile e dovrebbero dover essere configurati solo una volta, se fatti correttamente.

Allo stesso modo, la configurazione del suo ambiente di sviluppo, se fatta bene, deve essere fatta solo una volta e dovrebbe essere riutilizzabile in molti progetti futuri. Probabilmente vorrà rivedere questa parte della catena di strumenti con una certa regolarità e considerare se ci sono aggiornamenti o modifiche che dovrebbe introdurre, ma ciò non dovrebbe essere richiesto troppo spesso.

La sua catena di strumenti dipenderà dalle sue esigenze personali, ma per questo esempio di una catena di strumenti abbastanza completa, gli strumenti che saranno installati/inizializzati all'avvio saranno:

- Strumenti di installazione della libreria – per aggiungere dipendenze.
- Controllo delle revisioni del codice.
- Strumenti di pulizia del codice – per sistemare JavaScript, CSS e HTML.
- Strumenti di linting del codice – per il linting del nostro codice.

### Strumenti di installazione della libreria

Ha già fatto questo, ma per un facile riferimento, ecco i comandi (eseguiti alla radice della directory `npm-experiment`) per inizializzare un pacchetto npm e installare le dipendenze necessarie:

```bash
npm init
npm install --save-dev vite
npm install plotly.js-dist-min
```

### Controllo delle revisioni del codice

Inserisca il seguente comando per attivare la funzionalità di controllo del codice sorgente di git sulla directory:

```bash
git init
```

Di default, git tiene traccia delle modifiche di tutti i file. Tuttavia, ci sono alcuni file generati che non abbiamo bisogno di tracciare, poiché non sono codice che abbiamo scritto e possono essere rigenerati in qualsiasi momento. Possiamo dire a git di ignorare questi file creando un file `.gitignore` nella radice della directory del progetto. Aggiunga il seguente contenuto al file:

```plain
node_modules
dist
```

### Strumenti di pulizia del codice

Utilizzeremo Prettier, che abbiamo incontrato per la prima volta nel Capitolo 2, per sistemare il nostro codice in questo progetto. Installeremo nuovamente Prettier in questo progetto. Lo installi usando il seguente comando:

```bash
npm install --save-dev prettier
```

Noti ancora che stiamo usando `--save-dev` per aggiungerlo come dipendenza di sviluppo, poiché lo usiamo solo durante lo sviluppo.

Come molti strumenti realizzati più recentemente, Prettier viene fornito con "impostazioni predefinite sensibili". Ciò significa che sarà in grado di usare Prettier senza dover configurare nulla (se è soddisfatto dei [predefiniti](https://prettier.io/docs/configuration.html)). Questo le permette di concentrarsi su ciò che è importante: il lavoro creativo. Per dimostrazione, aggiungeremo un file di configurazione. Crei un file nella radice della sua directory `npm-experiment` chiamato `.prettierrc.json`. Aggiunga il seguente contenuto:

```json
{
  "bracketSameLine": true
}
```

Con questa impostazione, Prettier stamperà il `>` di un tag di apertura HTML (HTML, JSX, Vue, Angular) multilinea alla fine dell'ultima riga invece di essere da solo sulla riga successiva. Questo è il formato che lo stesso MDN utilizza. Può trovare di più sulla [configurazione di Prettier](https://prettier.io/docs/configuration.html) nella sua documentazione.

Per impostazione predefinita, Prettier formatta tutti i file che specifica. Tuttavia, ancora una volta, non abbiamo bisogno di formattare i file generati o ci potrebbe essere un certo codice legacy che non vogliamo toccare. Possiamo dire a Prettier di ignorare sempre questi file creando un file `.prettierignore` nella radice della directory del progetto. Aggiunga il seguente contenuto al file:

```plain
node_modules
dist
```

Ha lo stesso contenuto di `.gitignore`, ma in un progetto reale, potrebbe voler ignorare diversi file per Prettier rispetto a quelli per git.

Ora che Prettier è installato e configurato, eseguire e sistemare il suo codice può essere fatto dalla riga di comando, ad esempio:

```bash
npx prettier --write ./index.html
```

> [!NOTE]
> Nel comando sopra, usiamo Prettier con il flag `--write`. Prettier lo interpreta come "se c'è qualche problema nel formato del mio codice, vada avanti e li risolva, poi salvi il mio file". Questo va bene per il nostro processo di sviluppo, ma possiamo anche usare `prettier` senza il flag e verificherà solo il file. Verificare il file (e non salvarlo) è utile per scopi come i controlli che girano prima di una release - ad esempio, "non rilasciare alcun codice che non sia stato formattato correttamente."

Può anche sostituire `./index.html` con qualsiasi altro file o cartella per formattarli. Ad esempio, `.` formatterà tutto nella directory corrente. Nel caso in cui possa dimenticare la sintassi, può aggiungerlo come script personalizzato nel suo package.json:

```json
"scripts": {
  // …
  "format": "prettier --write ."
},
```

Ora può eseguire il seguente comando per formattare la directory:

```bash
npm run format
```

Può ancora essere arduo eseguire il comando ogni volta che cambia qualcosa, e ci sono alcuni modi per automatizzare questo processo:

- Usando speciali "hook di git" per testare se il codice è formattato prima di un commit.
- Usando plugin per l'editor di codice per eseguire comandi di Prettier ogni volta che un file viene salvato.

> [!NOTE]
> Cos'è un hook di git? Git (non GitHub) fornisce un sistema che ci permette di collegare azioni pre e post ai compiti che eseguiamo con git (come il commit del suo codice). Sebbene gli hook di git possano essere un po' troppo complicati (secondo l'opinione di questo autore), una volta in atto, possono essere molto potenti. Se è interessato a usare gli hook, [Husky](https://github.com/typicode/husky) è una via semplificata all'uso degli hook.

Per VS Code, un'estensione utile è [Prettier Code Formatter di Esben Petersen](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode), che permette a VS Code di formattare automaticamente il codice al momento del salvataggio. Questo significa che qualsiasi file nel progetto su cui stiamo lavorando viene formattato correttamente, inclusi HTML, CSS, JavaScript, JSON, markdown e altro. Tutto ciò di cui l'editor ha bisogno è che "Format On Save" sia abilitato.

### Strumenti di linting del codice

Il linting aiuta con la qualità del codice, ma è anche un modo per individuare potenziali errori in anticipo durante lo sviluppo. È un ingrediente chiave di una buona catena di strumenti e uno che molti progetti di sviluppo includeranno di default.

Gli strumenti di linting per lo sviluppo web esistono principalmente per JavaScript (anche se ce ne sono alcuni disponibili per HTML e CSS). Questo ha senso: se viene utilizzato un elemento HTML sconosciuto o una proprietà CSS non valida, a causa della natura resiliente di questi due linguaggi non è probabile che nulla si rompa. JavaScript è molto più fragile — chiamare per errore una funzione che non esiste, ad esempio, provoca la rottura del suo JavaScript; il linting di JavaScript è quindi molto importante, specialmente per progetti più grandi.

Lo strumento di riferimento per il linting di JavaScript è [ESLint](https://eslint.org/). È uno strumento estremamente potente e versatile, ma può risultare difficile da configurare correttamente e potrebbe facilmente consumare molte ore cercando di ottenere una configurazione _proprio giusta_!

ESLint è installato tramite npm, quindi, come discusso nel Capitolo 2, ha la possibilità di installare questo strumento localmente o globalmente, ma un'installazione locale è altamente raccomandata, poiché ha bisogno di un file di configurazione per ogni progetto comunque. Ricordi il comando da eseguire:

```bash
npm install --save-dev eslint@8 @eslint/js globals
```

> **Nota:** `eslint@8` installa la versione 8 di ESLint, mentre l'ultima è v9. Questo perché `eslint-plugin-react`, che utilizzeremo successivamente, [non supporta ancora v9](https://github.com/jsx-eslint/eslint-plugin-react/issues/3699).

Il pacchetto `@eslint/js` fornisce la configurazione predefinita di ESLint, mentre il pacchetto `globals` fornisce un elenco di nomi globali noti in ciascun ambiente. Li utilizzeremo successivamente nella configurazione. Di base, ESLint si lamenterà che non riesce a trovare il file di configurazione se lo esegue con `npx eslint`:

```plain
Oops! Something went wrong! :(

ESLint: 8.57.0

ESLint couldn't find a configuration file. To set up a configuration file for this project, please run:

...
```

Ecco un esempio minimo che funziona (in un file chiamato `eslint.config.js`, alla radice del progetto):

```js
import js from "@eslint/js";
import globals from "globals";

export default [
  js.configs.recommended,
  {
    ignores: ["node_modules", "dist"],
  },
  {
    files: ["**/*.{js,jsx}"],
    languageOptions: {
      globals: {
        ...globals.browser,
      },
    },
  },
];
```

La configurazione di ESLint sopra:

- Abilita le impostazioni "raccomandate" di ESLint
- Dice a ESLint di ignorare i file generati come abbiamo già fatto per gli altri strumenti
- Dice a ESLint di includere i file `.js` e `.jsx` nel linting
- Dice a ESLint dell'esistenza delle variabili globali del browser (utilizzato dalle regole di lint come `no-undef` per controllare le variabili inesistenti).

Il parser di ESLint non comprende il JSX per default, e le sue regole raccomandate non gestiscono le semantiche specifiche di React. Pertanto, aggiungeremo ulteriore configurazione per farlo supportare correttamente JSX e React. Prima, installi `eslint-plugin-react` e `eslint-plugin-react-hooks`, che forniscono regole per scrivere in modo corretto e idiomatico in React:

```bash
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks
```

Poi, aggiorni il file di configurazione di ESLint per includere la configurazione raccomandata di questi plugin, che carica sia le regole raccomandate sia le opzioni del parser per il JSX:

```js
import js from "@eslint/js";
import globals from "globals";
import reactRecommended from "eslint-plugin-react/configs/recommended.js";
import reactJSXRuntime from "eslint-plugin-react/configs/jsx-runtime.js";
import reactHooksPlugin from "eslint-plugin-react-hooks";

export default [
  js.configs.recommended,
  {
    ignores: ["node_modules", "dist"],
  },
  {
    files: ["**/*.{js,jsx}"],
    languageOptions: {
      globals: {
        ...globals.browser,
      },
    },
    settings: {
      react: {
        version: "detect",
      },
    },
  },
  reactRecommended,
  reactJSXRuntime,
  {
    plugins: {
      "react-hooks": reactHooksPlugin,
    },
    rules: reactHooksPlugin.configs.recommended.rules,
  },
];
```

> [!NOTE]
> La nostra configurazione per `eslint-plugin-react-hooks` è un po' goffa, rispetto alle aggiunte di una riga per le configurazioni di `eslint-plugin-react`. Questo perché `eslint-plugin-react-hooks` non supporta ancora il nuovo formato di config ESLint. Veda [facebook/react#28313](https://github.com/facebook/react/issues/28313) per maggiori informazioni.

C'è un elenco completo delle [regole di ESLint](https://eslint.org/docs/latest/rules/) che può modificare e configurare a sua discrezione e molte aziende e team hanno pubblicato le loro [configurazioni di ESLint](https://www.npmjs.com/search?q=keywords:eslintconfig), che a volte possono essere utili sia per ottenere ispirazione sia per selezionarne una che ritiene adatta ai suoi standard. Un avvertimento però: la configurazione di ESLint è una tana di coniglio molto profonda!

Per semplicità, in questo capitolo, non esploreremo tutte le funzionalità di ESLint, poiché questa configurazione funziona per il nostro progetto particolare e le sue esigenze. Tuttavia, tenete presente che se vuole affinare e imporre una regola su come il suo codice appare (o è convalidato), è molto probabile che possa essere fatto con la giusta configurazione di ESLint.

Come con altri strumenti, il supporto dell'integrazione dell'editor di codice è generalmente buono per ESLint e potenzialmente più utile poiché può fornire feedback in tempo reale quando si verificano problemi:

![Integrazione dell'errore di ESLint mostrata in VS Code](eslint-error.png)

Ecco completo il nostro ambiente di sviluppo a questo punto. Ora, finalmente siamo (quasi) pronti per codificare.

## Strumenti di costruzione e trasformazione

### Trasformazione JavaScript

Per questo progetto, come menzionato sopra, React verrà usato, il che significa anche che verrà usato JSX nel codice sorgente. Il progetto utilizzerà anche le ultime funzionalità di JavaScript. Un problema immediato è che nessun browser ha il supporto nativo per JSX; è un linguaggio intermedio che è destinato a essere compilato in linguaggi che il browser comprende nel codice di produzione. Se il browser tenta di eseguire il JavaScript sorgente si lamenterà immediatamente; il progetto ha bisogno di uno strumento di costruzione per trasformare il codice sorgente in qualcosa che il browser possa consumare senza problemi.

Ci sono diverse scelte di strumenti di trasformazione e anche se Babel è particolarmente popolare, in Vite, useremo un plugin integrato: `@vitejs/plugin-react`. Installi utilizzando il seguente comando:

```bash
npm install --save-dev @vitejs/plugin-react
```

Non abbiamo ancora una configurazione di Vite! Ne aggiunga una in `vite.config.js` nella radice della directory del progetto:

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  base: "/npm-experiment/",
});
```

Legga la [documentazione di Vite](https://vite.dev/guide/) per ulteriori informazioni su come configurare Vite. Poiché il nostro sito è distribuito su GitHub Pages, sarà ospitato su `https://your-username.github.io/your-repo-name`, quindi dovrebbe impostare l'opzione `base` in base al nome del repository GitHub — ma può sempre modificarlo successivamente quando arriveremo al [deployment](/it/docs/Learn_web_development/Extensions/Client-side_tools/Deployment).

### Trasformazione CSS

Il nostro CSS può anche utilizzare una sintassi non compresa dai browser. Ad esempio, può utilizzare una sintassi che è stata implementata solo nelle ultime versioni del browser, il che significa che i browser

più vecchi falliranno e visualizzeranno uno stile rotto. Possiamo usare uno strumento per trasformare il nostro CSS in un formato che tutti i browser che targetizziamo possano comprendere.

[PostCSS](https://postcss.org/) è uno strumento di post-processing CSS. Rispetto a strumenti di costruzione come [Sass](https://sass-lang.com/), PostCSS è inteso per scrivere _CSS standard_ (cioè, sintassi CSS che potrebbe arrivare nei browser un giorno), mentre Sass è un linguaggio personalizzato di per sé che compila in CSS. PostCSS è più vicino al web e ha una curva di apprendimento molto più bassa. [Vite supporta PostCSS per default](https://vite.dev/guide/features.html#postcss), quindi deve solo [configurare PostCSS](https://github.com/postcss/postcss#usage) se vuole compilare tutte le funzionalità. Controlli il [cssdb](https://preset-env.cssdb.org/features/) per vedere quali funzionalità sono supportate.

Per i nostri scopi, andremo a dimostrare un'altra trasformazione CSS: [CSS modules](https://vite.dev/guide/features.html#css-modules). È uno dei modi per ottenere _modularizzazione CSS_. Ricordi che i selettori CSS sono tutti globali, quindi se ha un nome di classe come `.button`, tutti gli elementi con il nome di classe `button` verranno stilizzati allo stesso modo. Questo spesso porta a conflitti di nomi — immagini tutte le sue variabili JavaScript definite a livello globale! I modules CSS risolvono questo problema rendendo il nome della classe unico per le pagine che li utilizzano. Per capire come funziona, dopo aver scaricato il codice sorgente, può controllare come usiamo i file `.module.css`, e leggere anche la [documentazione sui modules CSS](https://github.com/css-modules/css-modules).

Sebbene questa fase della nostra catena di strumenti possa essere piuttosto dolorosa, poiché abbiamo scelto uno strumento che cerca deliberatamente di ridurre la configurazione e la complessità, non c'è davvero nulla di più che dobbiamo fare durante la fase di sviluppo. I moduli sono correttamente importati, il CSS annidato è correttamente trasformato in "CSS normale", e il nostro sviluppo non è ostacolato dal processo di build.

Ora il nostro software è pronto per essere scritto!

## Scrivere il codice sorgente

Ora che abbiamo impostato l'intera catena di strumenti di sviluppo, di solito è tempo di iniziare a scrivere codice reale — la parte su cui dovrebbe effettivamente investire più tempo. Per il nostro scopo, però, copi

eremo solo un codice sorgente esistente e faremo finta di averlo scritto. Non le insegneremo come funzionano, poiché questo non è il punto di questo capitolo. Sono semplicemente qui per eseguire gli strumenti, per insegnarle come funzionano _loro_.

Per ottenere i file di codice, visiti <https://github.com/mdn/client-toolchain-example> e scarichi e decomprima il contenuto di questo repository da qualche parte sul suo disco locale. Può scaricare l'intero progetto come file zip selezionando _Clone or download_ > _Download ZIP_.

![Il repository di esempio su GitHub](github-repo.png)

Ora copi il contenuto della directory `src` del progetto e lo utilizzi per sostituire la sua attuale directory `src`. Non ha bisogno di preoccuparsi degli altri file.

Inoltre installi alcune dipendenze che il codice sorgente utilizza:

```bash
npm install react react-dom @tanstack/react-query
```

Abbiamo i nostri file di progetto a posto. Questo è tutto ciò che dobbiamo fare per ora!

## Esecuzione della trasformazione

Per iniziare a lavorare con il nostro progetto, avvieremo il server di Vite dalla riga di comando. Nella sua modalità predefinita, monitorerà le modifiche nel suo codice e aggiornerà il server. Questo è piacevole perché non dobbiamo saltare avanti e indietro tra il codice e la riga di comando.

1. Per avviare Vite in background, vada al suo terminale ed esegua il seguente comando (usando lo script personalizzato che abbiamo definito in precedenza):

   ```bash
   npm run dev
   ```

   Dovrebbe vedere un output simile a questo (una volta che le dipendenze sono state installate):

   ```plain
   > client-toolchain-example@1.0.0 dev
   > vite

   Re-optimizing dependencies because lockfile has changed

     VITE v5.2.13  ready in 157 ms

     ➜  Local:   http://localhost:5173/
     ➜  Network: use --host to expose
     ➜  press h + enter to show help
   ```

   Il server è ora in esecuzione sull'URL che è stato stampato (in questo caso localhost:5173).

2. Vada a questo URL nel suo browser e vedrà l'app di esempio in esecuzione!

Ora possiamo fare alcune modifiche e vedere i loro effetti in tempo reale.

1. Carichi il file `src/App.jsx` nel suo editor di testo preferito.
2. Sostituisca tutte le occorrenze di `mdn/content` con il suo repository GitHub preferito, come `facebook/react`.
3. Salvi il file, quindi torni direttamente all'app in esecuzione nel suo browser. Noterà che il browser si è aggiornato automaticamente e i grafici sono cambiati!

Potrebbe anche provare a usare ESLint e Prettier — provi a rimuovere deliberatamente una parte del whitespace da uno dei suoi file ed eseguire Prettier su di esso per pulirlo, oppure introduca un errore di sintassi in uno dei suoi file JavaScript e veda quali errori ESLint le dà quando esegue il comando `eslint` o nel suo editor.

## Riepilogo

Abbiamo fatto molta strada in questo capitolo, costruendo un ambiente di sviluppo locale piuttosto piacevole per creare un'applicazione.

A questo punto, durante lo sviluppo del software web, di solito creerebbe il codice per il software che intende costruire. Poiché questo modulo riguarda l'apprendimento degli strumenti intorno allo sviluppo web, non il codice di sviluppo web stesso, non le insegneremo alcun codice effettivo — troverà queste informazioni nel resto di MDN!

Invece, abbiamo scritto un progetto di esempio per lei da usare con i suoi strumenti. Le suggeriremmo di lavorare attraverso il resto del capitolo usando il codice di esempio e poi potrebbe provare a cambiare i contenuti della directory src nel suo progetto personale e pubblicarlo su GitHub Pages invece! E infatti, distribuire su GitHub Pages sarà l'obiettivo finale del capitolo successivo!

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Package_management","Learn_web_development/Extensions/Client-side_tools/Deployment", "Learn_web_development/Extensions/Client-side_tools")}}
