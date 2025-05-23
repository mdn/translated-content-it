---
title: Introduzione a una toolchain completa
short-title: Esempio di toolchain
slug: Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Package_management","Learn_web_development/Extensions/Client-side_tools/Deployment", "Learn_web_development/Extensions/Client-side_tools")}}

Negli ultimi articoli della serie, consolidiamo le conoscenze sugli strumenti guidandoti attraverso il processo di costruzione di una toolchain di studio del caso. Andremo dall'impostare un ambiente di sviluppo sensato e mettere in atto strumenti di trasformazione fino a distribuire effettivamente la tua app. In questo articolo, introdurremo lo studio del caso, configureremo il nostro ambiente di sviluppo e imposteremo i nostri strumenti di trasformazione del codice.

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
        Consolidare ciò che abbiamo imparato finora lavorando su uno studio del caso completo
        della toolchain.
      </td>
    </tr>
  </tbody>
</table>

Esistono combinazioni illimitate di strumenti e modi per usarli, quello che vedi in questo articolo e nel successivo è solo _un_ modo in cui gli strumenti presentati possono essere utilizzati per un progetto.

> [!NOTE]
> Vale anche la pena ripetere che non tutti questi strumenti devono essere eseguiti da riga di comando. Molti degli editor di codice di oggi (come VS Code) supportano l'integrazione di _molti_ strumenti tramite plugin.

## Introduzione al nostro case study

La toolchain che stiamo creando in questo articolo verrà utilizzata per costruire e distribuire un mini-sito che visualizza dati sul repository [mdn/content](https://github.com/mdn/content), ricavando i suoi dati dall'[API di GitHub](https://docs.github.com/en/rest/metrics/community).

## Strumenti utilizzati nella nostra toolchain

In questo articolo useremo i seguenti strumenti e funzionalità:

- [JSX](https://react.dev/learn/writing-markup-with-jsx), un set di estensioni di sintassi relative a [React](https://react.dev/) che ti permettono di fare cose come definire strutture di componenti dentro JavaScript. Non sarà necessario conoscere React per seguire questo tutorial, ma lo abbiamo incluso per darti un'idea di come un linguaggio web non nativo potrebbe essere integrato in una toolchain.
- Le ultime caratteristiche del JavaScript integrato (al momento della scrittura), come [`import`](/it/docs/Web/JavaScript/Reference/Statements/import).
- Utili strumenti di sviluppo come [Prettier](https://prettier.io/) per la formattazione e [ESLint](https://eslint.org/) per il linting.
- [PostCSS](https://postcss.org/) per fornire capacità di annidamento CSS.
- [Vite](https://vite.dev/) per costruire e minimizzare il nostro codice, e per scrivere automaticamente una serie di contenuti di file di configurazione per noi.
- [GitHub](/it/docs/Learn_web_development/Core/Version_control) per gestire il controllo del codice sorgente e per distribuire eventualmente il nostro sito (utilizzando GitHub Pages).

Potresti non essere familiare con tutte le funzionalità e strumenti sopra elencati o cosa stanno facendo, ma non preoccuparti — spiegheremo ogni parte man mano che procederemo con questo articolo.

## Toolchain e la loro complessità intrinseca

Come con qualsiasi catena, più collegamenti hai nella tua toolchain, più complessa e potenzialmente fragile diventa — ad esempio potrebbe essere più complessa da configurare e più facile da rompere. Al contrario, meno collegamenti hai, più resiliente è probabile che sia la toolchain.

Tutti i progetti web saranno diversi, e bisogna considerare quali parti della toolchain sono necessarie e valutarle attentamente.

La toolchain più piccola è quella che non ha collegamenti. Codificheresti manualmente l'HTML, useresti "JavaScript puro" (cioè senza framework o linguaggi intermedi) e caricheresti manualmente tutto su un server per l'hosting.

Tuttavia, requisiti software più complicati probabilmente trarranno beneficio dall'uso di strumenti per aiutare a semplificare il processo di sviluppo. Inoltre, è opportuno includere test prima di distribuire al server di produzione per garantire che il software funzioni come previsto — sembra già una toolchain necessaria.

Nel nostro progetto di esempio, utilizzeremo una toolchain appositamente progettata per supportare il nostro sviluppo software e supportare le scelte tecniche effettuate durante la fase di progettazione del software. Eviteremo tuttavia qualsiasi strumento superfluo, con l'obiettivo di mantenere la complessità al minimo.

## Verifica dei prerequisiti

Dovresti avere la maggior parte dei pezzi di software già a disposizione se hai seguito i capitoli precedenti. Ecco cosa dovresti avere prima di procedere ai veri passaggi di configurazione. Devono essere eseguiti solo una volta e non è necessario ripeterli per progetti futuri.

### Creazione di un account GitHub

Oltre agli strumenti che installeremo e che contribuiscono alla nostra toolchain, sarà necessario creare un account con GitHub se desideri completare il tutorial. Tuttavia, puoi comunque seguire la parte di sviluppo locale senza di esso. Come menzionato in precedenza, GitHub è un servizio di repository di codice sorgente che aggiunge funzionalità comunitarie come il tracciamento dei problemi, il monitoraggio delle versioni del progetto e molto altro. Nel prossimo capitolo, pubblicheremo su un repository di codice GitHub, il che causerà un effetto a cascata che (dovrebbe) distribuire tutto il software su un sito web.

Iscriviti a [GitHub](https://github.com/) cliccando sul link _Sign Up_ nella homepage se non hai già un account e segui le istruzioni.

### Installazione di git

Installeremo un altro software, git, per aiutare con il controllo delle revisioni.

È possibile che tu abbia sentito parlare di "git" prima. [Git](https://git-scm.com/) è attualmente lo strumento di controllo delle revisioni del codice sorgente più popolare a disposizione degli sviluppatori — il controllo delle revisioni offre molti vantaggi, come un modo per eseguire il backup del proprio lavoro in un luogo remoto e un meccanismo per lavorare in team sullo stesso progetto senza il timore di sovrascrivere il codice degli altri.

Potrebbe essere ovvio per alcuni, ma è bene ripetere: Git non è la stessa cosa di GitHub. Git è lo strumento di controllo delle revisioni, mentre [GitHub](https://github.com/) è un archivio online per i repository git (più una serie di strumenti utili per lavorarci). Nota che, sebbene stiamo usando GitHub in questo capitolo, ci sono diverse alternative tra cui [GitLab](https://about.gitlab.com/) e [Bitbucket](https://www.atlassian.com/software/bitbucket), e potresti persino ospitare i tuoi repository git.

Usare il controllo delle revisioni nei tuoi progetti e includerlo come parte della toolchain ti aiuterà a gestire l'evoluzione del tuo codice. Offre un modo per "commettere" blocchi di lavoro man mano che procedi, insieme a commenti come "Implementata nuova funzionalità X", o "Risolto bug Z grazie alle modifiche Y".

Il controllo delle revisioni può anche permetterti di _ramificare_ il codice del tuo progetto, creando una versione separata e provando nuove funzionalità senza che queste modifiche influenzino il tuo codice originale.

Infine, può aiutarti a annullare modifiche o ripristinare il codice a un momento in cui "funzionava" se è stato introdotto un errore da qualche parte e hai difficoltà a risolverlo — qualcosa che tutti gli sviluppatori devono fare di tanto in tanto!

Git può essere [scaricato e installato attraverso il sito web di git-scm](https://git-scm.com/downloads) — scarica l'installer pertinente per il tuo sistema, eseguilo e segui le istruzioni sullo schermo. Questo è tutto ciò che devi fare per ora.

Puoi interagire con git in diversi modi, dai comandi tramite riga di comando, all'uso di un'app GUI git per emettere gli stessi comandi premendo pulsanti, o persino direttamente all'interno del tuo editor di codice, come visto nell'esempio di Visual Studio Code qui sotto:

![Integrazione di Git mostrata in VS Code](vscode-git.png)

### Progetto esistente

Costruiremo sul progetto che abbiamo già iniziato nel capitolo precedente, quindi assicurati di seguire le istruzioni in [Package management](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per configurare prima il progetto. Per ricapitolare, ecco cosa dovresti avere:

- Node.js e npm installati.
- Un nuovo progetto chiamato `npm-experiment` (o un altro nome).
- Vite installato come dipendenza di sviluppo.
- Il pacchetto `plotly.js-dist-min` installato come dipendenza.
- Alcuni script personalizzati definiti in package.json.
- I file `index.html` e `src/main.jsx` creati.

Come abbiamo discusso in [Capitolo 1](/it/docs/Learn_web_development/Extensions/Client-side_tools/Overview), la toolchain verrà strutturata nelle seguenti fasi:

- **Ambiente di sviluppo**: Gli strumenti che sono più fondamentali per eseguire il tuo codice. Questa parte è già configurata nel capitolo precedente.
- **Rete di sicurezza**: Rende l'esperienza di sviluppo del software stabile e più efficiente. Potremmo anche riferirci a questa come al nostro ambiente di sviluppo.
- **Trasformazione**: Strumenti che ci permettono di usare le ultime funzionalità di un linguaggio (e.g., JavaScript) o un altro linguaggio completamente (e.g., JSX o TypeScript) nel nostro processo di sviluppo, e poi trasforma il nostro codice in modo che la versione di produzione funzioni ancora su un'ampia varietà di browser, moderni e vecchi.
- **Post sviluppo**: Strumenti che entrano in gioco dopo aver terminato la parte principale dello sviluppo per garantire che il tuo software arrivi sul web e continui a funzionare. In questo caso di studio vedremo come aggiungere test al tuo codice e distribuire la tua app utilizzando GitHub Pages in modo che sia disponibile per tutto il web.

Iniziamo a lavorare su questi, a partire dal nostro ambiente di sviluppo. Seguiremo gli stessi passaggi di come verrebbe impostato un progetto reale, quindi in futuro, se stai configurando un nuovo progetto, puoi fare riferimento a questo capitolo e seguire nuovamente i passaggi.

## Creazione di un ambiente di sviluppo

Questa parte della toolchain a volte è vista come un ritardo rispetto al lavoro effettivo, e può essere molto facile cadere in un "buco del coniglio" di strumenti in cui si passa molto tempo a cercare di ottenere l'ambiente "proprio giusto".

Ma puoi vederlo nello stesso modo in cui imposteresti il tuo ambiente di lavoro fisico. La sedia deve essere comoda e posizionata in modo da aiutare la tua postura. Hai bisogno di energia, Wi-Fi e porte USB! Potrebbero esserci decorazioni importanti o musica che aiutano il tuo stato mentale — sono tutte importanti per fare il tuo lavoro migliore possibile e dovrebbero anche avere bisogno di essere impostate una sola volta, se fatte correttamente.

Allo stesso modo, l'impostazione del tuo ambiente di sviluppo, se fatta bene, deve essere fatta solo una volta e dovrebbe essere riutilizzabile in molti progetti futuri. Vorrai probabilmente rivedere questa parte della toolchain in modo semi-regolare e considerare se ci sono aggiornamenti o modifiche che dovresti introdurre, ma non dovrebbe essere necessario troppo spesso.

La tua toolchain dipenderà dalle tue esigenze, ma per questo esempio di una toolchain abbastanza completa, gli strumenti che verranno installati/inzializzati inizialmente saranno:

- Strumenti di installazione delle librerie — per l'aggiunta di dipendenze.
- Controllo delle revisioni del codice.
- Strumenti per il riordino del codice — per riordinare JavaScript, CSS e HTML.
- Strumenti di linting del codice — per il linting del nostro codice.

### Strumenti di installazione delle librerie

Lo hai già fatto, ma per facile riferimento, ecco i comandi (eseguiti nella radice della directory `npm-experiment`) per inizializare un pacchetto npm e installare le dipendenze necessarie:

```bash
npm init
npm install --save-dev vite
npm install plotly.js-dist-min
```

### Controllo delle revisioni del codice

Inserisci il seguente comando per iniziare a far funzionare la funzionalità di controllo del codice sorgente di git nella directory:

```bash
git init
```

Per impostazione predefinita, git traccia le modifiche di tutti i file. Tuttavia, ci sono alcuni file generati che non abbiamo bisogno di tracciare, poiché non sono codice che abbiamo scritto e possono essere rigenerati in qualsiasi momento. Possiamo dire a git di ignorare questi file creando un file `.gitignore` nella radice della directory del progetto. Aggiungi il seguente contenuto al file:

```plain
node_modules
dist
```

### Strumenti per il riordino del codice

Utilizzeremo Prettier, che abbiamo incontrato per la prima volta nel Capitolo 2, per ordinare il nostro codice in questo progetto. Installeremo di nuovo Prettier in questo progetto. Installalo usando il seguente comando:

```bash
npm install --save-dev prettier
```

Nota di nuovo che stiamo usando `--save-dev` per aggiungerlo come dipendenza di sviluppo, perché lo usiamo solo durante lo sviluppo.

Come molti strumenti creati più di recente, Prettier viene fornito con "default sensati". Ciò significa che potrai usare Prettier senza dover configurare nulla (se sei soddisfatto dei [default](https://prettier.io/docs/configuration.html)). Questo ti permette di occuparti di ciò che è importante: il lavoro creativo. Per dimostrazione, aggiungeremo un file di configurazione. Crea un file nella radice della tua directory `npm-experiment` chiamato `.prettierrc.json`. Aggiungi il seguente contenuto:

```json
{
  "bracketSameLine": true
}
```

Con questa impostazione, Prettier stamperà il `>` di un tag di apertura HTML (HTML, JSX, Vue, Angular) multi-line alla fine dell'ultima linea invece di essere da solo sulla linea successiva. Questo è il formato che lo stesso MDN utilizza. Puoi trovare ulteriori informazioni su [come configurare Prettier](https://prettier.io/docs/configuration.html) nella sua documentazione.

Per impostazione predefinita, Prettier formatta tutti i file che specifichi. Tuttavia, di nuovo, non abbiamo bisogno di formattare i file generati, o potrebbero esserci certi codici legacy che non vogliamo toccare. Possiamo dire a Prettier di ignorare sempre questi file creando un `.prettierignore` file nella radice della directory del progetto. Aggiungi il seguente contenuto al file:

```plain
node_modules
dist
```

Ha lo stesso contenuto del `.gitignore`, ma in un progetto reale, potresti voler ignorare file diversi per Prettier di quanto fai per git.

Ora che Prettier è installato e configurato, eseguire e ordinare il tuo codice può essere fatto da riga di comando, ad esempio:

```bash
npx prettier --write ./index.html
```

> [!NOTE]
> Nel comando sopra, usiamo Prettier con il flag `--write`. Prettier lo interpreta come "se c'è qualche problema nel formato del mio codice, vai avanti e risolvilo, quindi salva il mio file". Questo va bene per il nostro processo di sviluppo, ma possiamo anche usare `prettier` senza il flag e controllerà solo il file. Controllare il file (e non salvarlo) è utile per scopi come i controlli che si eseguono prima di un rilascio — cioè "non rilasciare nessun codice che non sia stato formattato correttamente."

Puoi anche sostituire `./index.html` con qualsiasi altro file o cartella per formattarli. Ad esempio, `.` formatterà tutto nella directory corrente. Nel caso potessi dimenticare la sintassi, puoi aggiungerlo anche come uno script personalizzato nel tuo package.json:

```json
"scripts": {
  // …
  "format": "prettier --write ."
},
```

Ora puoi eseguire il seguente comando per formattare la directory:

```bash
npm run format
```

Può comunque essere arduo eseguire il comando ogni volta che cambiamo qualcosa, e ci sono alcuni modi per automatizzare questo processo:

- Usare "git hook" speciali per testare se il codice è formattato prima di un commit.
- Usare plugin per editor di codice per eseguire i comandi di Prettier ogni volta che un file viene salvato.

> [!NOTE]
> Cos'è un git hook? Git (non GitHub) fornisce un sistema che ci permette di allegare azioni pre e post ai compiti che eseguiamo con git (come il commit del tuo codice). Sebbene i git hook possano essere un po' troppo complicati (secondo l'opinione di questo autore), una volta impostati possono essere molto potenti. Se sei interessato a usare i hook, [Husky](https://github.com/typicode/husky) è una via enormemente semplificata per usare i hook.

Per VS Code, un'estensione utile è [Prettier Code Formatter by Esben Petersen](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode), che consente a VS Code di formattare automaticamente il codice al momento del salvataggio. Ciò significa che qualsiasi file nel progetto su cui stiamo lavorando viene formattato in modo piacevole, inclusi HTML, CSS, JavaScript, JSON, markdown e altro. Tutto ciò di cui l'editor ha bisogno è "Format On Save" abilitato.

### Strumenti di linting del codice

Il linting aiuta con la qualità del codice, ma è anche un modo per catturare potenziali errori in anticipo durante lo sviluppo. È un ingrediente chiave di una buona toolchain e quello che molti progetti di sviluppo includeranno di default.

Gli strumenti di linting per lo sviluppo web esistono principalmente per JavaScript (anche se ce ne sono alcuni disponibili per HTML e CSS). Questo ha senso: se viene utilizzato un elemento HTML sconosciuto o una proprietà CSS non valida, a causa della natura resiliente di questi due linguaggi, è improbabile che qualcosa si rompa. JavaScript è molto più fragile — chiamare per errore una funzione che non esiste, ad esempio, provoca la rottura del tuo JavaScript; il linting del JavaScript è quindi molto importante, specialmente per progetti di grandi dimensioni.

Lo strumento di riferimento per il linting del JavaScript è [ESLint](https://eslint.org/). È uno strumento estremamente potente e versatile, ma può essere difficile da configurare correttamente e potresti facilmente consumare molte ore cercando di ottenere una configurazione _proprio giusta_!

ESLint è installato tramite npm, quindi come discusso nel Capitolo 2, hai la possibilità di installare questo strumento localmente o globalmente, ma è altamente raccomandata un'installazione locale, perché hai bisogno di un file di configurazione per ciascun progetto comunque. Ricorda il comando da eseguire:

```bash
npm install --save-dev eslint@8 @eslint/js globals
```

> **Nota:** `eslint@8` installa la versione 8 di ESLint, mentre l'ultima è la v9. Questo perché `eslint-plugin-react`, che utilizzeremo più tardi, [non supporta ancora la v9](https://github.com/jsx-eslint/eslint-plugin-react/issues/3699).

Il pacchetto `@eslint/js` fornisce una configurazione ESLint predefinita, mentre il pacchetto `globals` fornisce un elenco di nomi globali noti in ciascun ambiente. Li useremo più tardi nella configurazione. Fuori dalla scatola, ESLint si lamenterà se non riesce a trovare il file di configurazione se lo esegui con `npx eslint`:

```plain
Oops! Something went wrong! :(

ESLint: 8.57.0

ESLint couldn't find a configuration file. To set up a configuration file for this project, please run:

...
```

Ecco un esempio minimale che funziona (in un file chiamato `eslint.config.js`, nella radice del progetto):

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

La configurazione ESLint sopra:

- Abilita le impostazioni "consigliate" di ESLint
- Informa ESLint di ignorare i file generati come abbiamo già fatto per gli altri strumenti
- Informa ESLint di includere i file `.js` e `.jsx` nel linting
- Informa ESLint sull'esistenza delle variabili globali del browser (utilizzate dalle regole di linting come `no-undef` per controllare variabili inesistenti).

Il parser ESLint non comprende il JSX per impostazione predefinita, e le sue regole consigliate non gestiscono specificità semanticamente corretta per React. Pertanto, aggiungeremo un po' più di configurazione per fargli supportare correttamente JSX e React. Prima installa `eslint-plugin-react` e `eslint-plugin-react-hooks`, che forniscono regole per scrivere corrette ed idiomatiche il codice React:

```bash
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks
```

Quindi, aggiorna il file di configurazione ESLint per includere la configurazione consigliata di questi plugin, che carica entrambe le regole consigliate e imposta le opzioni del parser per JSX:

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
> La nostra configurazione per `eslint-plugin-react-hooks` è un po' scomoda, rispetto alle aggiunte con una sola riga per `eslint-plugin-react`. Questo perché `eslint-plugin-react-hooks` non supporta ancora il nuovo formato di configurazione ESLint. Vedi [facebook/react#28313](https://github.com/facebook/react/issues/28313) per maggiori informazioni.

C'è un elenco completo di [regole ESLint](https://eslint.org/docs/latest/rules/) che puoi modificare e configurare a tuo piacimento e molte aziende e team hanno pubblicato le loro [configurazioni ESLint](https://www.npmjs.com/search?q=keywords:eslintconfig), che a volte possono essere utili sia per trarre ispirazione che per selezionare una che si adatti ai tuoi standard. Una premessa però: la configurazione di ESLint è un coniglio molto profondo!

Per semplicità, in questo capitolo, non esploreremo tutte le funzionalità di ESLint, poiché questa configurazione funziona per il nostro progetto particolare e le sue esigenze. Tuttavia, tieni presente che se vuoi affinare e imporre una regola su come appare il tuo codice (o convalida), è molto probabile che possa essere fatto con la giusta configurazione ESLint.

Come con gli altri strumenti, il supporto dell'integrazione degli editor di codice è tipicamente buono per ESLint, e potenzialmente più utile poiché può darci un feedback in tempo reale quando emergono problemi:

![Integrazione degli errori ESLint mostrata in VS Code](eslint-error.png)

A questo punto il nostro ambiente di sviluppo è completo. Ora, finalmente siamo (quasi) pronti a codificare.

## Costruzione e strumenti di trasformazione

### Trasformazione JavaScript

Per questo progetto, come sopra menzionato, verrà usato React, il che significa anche che JSX verrà impiegato nel codice sorgente. Il progetto impiegherà anche le ultime funzionalità di JavaScript. Un problema immediato è che nessun browser supporta nativamente JSX; è un linguaggio intermedio destinato a essere compilato in linguaggi che il browser comprende nel codice di produzione. Se il browser tenta di eseguire il sorgente JavaScript, si lamenterà immediatamente; il progetto necessita di uno strumento di costruzione per trasformare il codice sorgente in qualcosa che il browser possa consumare senza problemi.

Ci sono diverse opzioni per gli strumenti di trasformazione e sebbene Babel sia particolarmente popolare, in Vite, useremo un plugin integrato: `@vitejs/plugin-react`. Installalo utilizzando il seguente comando:

```bash
npm install --save-dev @vitejs/plugin-react
```

Non abbiamo ancora una configurazione Vite! Aggiungine una a `vite.config.js` nella radice della directory del progetto:

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  base: "/npm-experiment/",
});
```

Leggi la [documentazione di Vite](https://vite.dev/guide/) per maggiori informazioni su come configurare Vite. Poiché il nostro sito è distribuito su GitHub pages, sarà ospitato a `https://tuo-username.github.io/tuo-nome-repo`, quindi dovresti impostare l'opzione `base` in base al nome del tuo repository GitHub — ma puoi sempre regolarlo in seguito quando arriviamo alla [distribuzione](/it/docs/Learn_web_development/Extensions/Client-side_tools/Deployment).

### Trasformazione CSS

Anche il nostro CSS potrebbe usare sintassi non compresa dai browser. Ad esempio, potresti usare una sintassi che è stata implementata solo nelle ultime versioni di alcuni browser, il che significa che i browser più vecchi falliranno su di essa e mostreranno uno stile rotto. Possiamo usare uno strumento per trasformare il nostro CSS in un formato che tutti i browser che puntiamo possano comprendere.

[PostCSS](https://postcss.org/) è uno strumento di post-elaborazione CSS. Rispetto agli strumenti di costruzione come [Sass](https://sass-lang.com/), PostCSS è inteso per scrivere _CSS standard_ (cioè, sintassi CSS che potrebbe essere inclusa nei browser un giorno), mentre Sass è un linguaggio personalizzato di per sé che si compila in CSS. PostCSS è più vicino al web e ha una curva di apprendimento molto più bassa. [Vite supporta PostCSS di default](https://vite.dev/guide/features.html#postcss), quindi basta [configurare PostCSS](https://github.com/postcss/postcss#usage) se si desidera compilare qualsiasi caratteristica. Dà un'occhiata al [cssdb](https://preset-env.cssdb.org/features/) per scoprire quali funzionalità sono supportate.

Per i nostri scopi, dimostreremo un'altra trasformazione CSS: i [moduli CSS](https://vite.dev/guide/features.html#css-modules). È uno dei modi per ottenere la _modularizzazione del CSS_. Ricorda che i selettori CSS sono tutti globali, quindi se hai un nome di classe come `.button`, tutti gli elementi con il nome di classe `button` verranno stilizzati allo stesso modo. Questo spesso porta a conflitti di nomi — immagina tutte le tue variabili JavaScript definite nello scope globale! I moduli CSS risolvono questo problema rendendo il nome della classe unico per le pagine che li utilizzano. Per capire come funziona, dopo aver scaricato il codice sorgente, puoi verificare come utilizziamo i file `.module.css`, e leggi anche la [documentazione sui moduli CSS](https://github.com/css-modules/css-modules).

Sebbene questa fase della nostra toolchain possa essere piuttosto dolorosa, poiché abbiamo scelto uno strumento che cerca volutamente di ridurre la configurazione e la complessità, non c'è davvero null'altro che dobbiamo fare durante la fase di sviluppo. I moduli sono correttamente importati, il CSS annidato è correttamente trasformato in "CSS regolare", e il nostro sviluppo non è ostacolato dal processo di compilazione.

Ora il nostro software è pronto per essere scritto!

## Scrittura del codice sorgente

Ora che abbiamo impostato l'intera toolchain di sviluppo, solitamente è il momento di iniziare a scrivere codice reale — la parte in cui dovresti effettivamente investire la maggior parte del tempo. Per il nostro scopo, tuttavia, copieremo semplicemente del codice sorgente esistente e fingeremo di averlo scritto. Non ti insegneremo come funzionano, poiché non è il punto di questo capitolo. Sono qui solo per eseguire gli strumenti, per insegnarti come _loro_ funzionano.

Per ottenere i file del codice, visita <https://github.com/mdn/client-toolchain-example> e scarica e decomprimi il contenuto di questo repo sul tuo disco locale da qualche parte. Puoi scaricare l'intero progetto come file zip selezionando _Clone or download_ > _Download ZIP_.

![Il repository di esempio di GitHub](github-repo.png)

Ora copia il contenuto della directory `src` del progetto e usalo per sostituire la tua attuale directory `src`. Non c'è bisogno di preoccuparsi degli altri file.

Installa anche alcune dipendenze che il codice sorgente utilizza:

```bash
npm install react react-dom @tanstack/react-query
```

Abbiamo i file del nostro progetto al loro posto. Questo è tutto ciò che dobbiamo fare per ora!

## Esecuzione della trasformazione

Per iniziare a lavorare con il nostro progetto, eseguiremo il server Vite da riga di comando. Nella modalità predefinita guarderà per modifiche nel tuo codice e aggiornerà il server. Questo è bello perché non dobbiamo fluttuare avanti e indietro tra il codice e la riga di comando.

1. Per avviare Vite in secondo piano, vai al tuo terminale ed esegui il seguente comando (usando lo script personalizzato che abbiamo definito in precedenza):

   ```bash
   npm run dev
   ```

   Dovresti vedere un output come questo (una volta che le dipendenze sono state installate):

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

2. Vai a questo URL nel tuo browser e vedrai l'app di esempio in esecuzione!

Ora possiamo apportare alcune modifiche e visualizzare i loro effetti dal vivo.

1. Carica il file `src/App.jsx` nel tuo editor di testo preferito.
2. Sostituisci tutte le occorrenze di `mdn/content` con il tuo repo GitHub preferito, ad esempio `facebook/react`.
3. Salva il file, poi torna direttamente all'app in esecuzione nel tuo browser. Noterai che il browser si è aggiornato automaticamente e i grafici sono cambiati!

Puoi anche provare a usare ESLint e Prettier — prova a rimuovere deliberatamente un sacco di spazi bianchi da uno dei tuoi file ed esegui Prettier su di esso per pulirlo, o introduce un errore di sintassi in uno dei tuoi file JavaScript e vedi quali errori ti dà ESLint quando esegui il comando `eslint`, o nel tuo editor.

## Riepilogo

Abbiamo fatto molta strada in questo capitolo, costruendo un ambiente di sviluppo locale piuttosto bello per creare un'applicazione.

A questo punto durante lo sviluppo del software web, solitamente saresti a creare il tuo codice per il software che intendi costruire. Poiché questo modulo riguarda l'apprendimento degli strumenti intorno allo sviluppo web, non il codice di sviluppo web stesso, non ti insegneremo alcuna codifica reale — troverai quell'informazione nel resto di MDN!

Invece, abbiamo scritto un progetto di esempio per te da utilizzare con i tuoi strumenti. Ti suggeriremmo di lavorare attraverso il resto del capitolo utilizzando il nostro codice di esempio, e poi puoi provare a cambiare il contenuto della directory src in un tuo progetto e pubblicarlo invece su GitHub Pages! E infatti, distribuire su GitHub Pages sarà l'obiettivo finale del prossimo capitolo!

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Package_management","Learn_web_development/Extensions/Client-side_tools/Deployment", "Learn_web_development/Extensions/Client-side_tools")}}
