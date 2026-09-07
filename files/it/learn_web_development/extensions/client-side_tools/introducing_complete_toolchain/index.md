---
title: Introduzione a una toolchain completa
short-title: Toolchain di esempio
slug: Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain
l10n:
  sourceCommit: 324c613947adaa5e19ad0f409c5f4c535ee8cf6b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Package_management","Learn_web_development/Extensions/Client-side_tools/Deployment", "Learn_web_development/Extensions/Client-side_tools")}}

Negli ultimi due articoli della serie, consolideremo le conoscenze sugli strumenti illustrando il processo di creazione di una toolchain per un caso di studio di esempio. Si partirà dalla configurazione di un ambiente di sviluppo adeguato e dall'introduzione di strumenti di trasformazione, fino all'effettivo deployment dell'app. In questo articolo, presenteremo il caso di studio, configureremo l'ambiente di sviluppo e gli strumenti di trasformazione del codice.

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
        Consolidare quanto appreso finora affrontando un caso di studio
        completo su una toolchain.
      </td>
    </tr>
  </tbody>
</table>

Esistono davvero combinazioni illimitate di strumenti e modi per utilizzarli; ciò che viene mostrato in questo articolo e nel successivo è soltanto _uno_ dei modi in cui gli strumenti presentati possono essere usati per un progetto.

> [!NOTE]
> Vale anche la pena ripetere che non tutti questi strumenti devono essere eseguiti dalla riga di comando. Molti degli editor di codice odierni, come VS Code, offrono il supporto di integrazione per _moltissimi_ strumenti tramite plugin.

## Presentazione del caso di studio

La toolchain creata in questo articolo sarà utilizzata per creare e distribuire un mini-sito che visualizza dati sul repository [mdn/content](https://github.com/mdn/content), ottenendo i dati dalla [GitHub API](https://docs.github.com/en/rest/metrics/community).

## Strumenti usati nella toolchain

In questo articolo verranno usati gli strumenti e le funzionalità seguenti:

- [JSX](https://react.dev/learn/writing-markup-with-jsx), un insieme di estensioni della sintassi correlate a [React](https://react.dev/) che consentono di fare cose come definire strutture di componenti all'interno di JavaScript. Non è necessario conoscere React per seguire questo tutorial, ma è stato incluso per dare un'idea di come un linguaggio web non nativo possa essere integrato in una toolchain.
- Le più recenti funzionalità integrate di JavaScript, al momento della scrittura, come [`import`](/it/docs/Web/JavaScript/Reference/Statements/import).
- Strumenti di sviluppo utili come [Prettier](https://prettier.io/) per la formattazione ed [ESLint](https://eslint.org/) per il linting.
- [PostCSS](https://postcss.org/) per fornire funzionalità di nesting CSS.
- [Vite](https://vite.dev/) per creare e minimizzare il codice e per scrivere automaticamente per noi gran parte del contenuto dei file di configurazione.
- [GitHub](/it/docs/Learn_web_development/Core/Version_control) per gestire il controllo del codice sorgente e infine distribuire il sito, usando GitHub Pages.

Potrebbe non esserci familiarità con tutte le funzionalità e gli strumenti precedenti o con ciò che fanno, ma niente panico: ogni parte verrà spiegata nel corso dell'articolo.

## Toolchain e complessità intrinseca

Come per qualsiasi catena, più anelli sono presenti nella toolchain, più questa risulta complessa e potenzialmente fragile: per esempio, potrebbe essere più complessa da configurare e più facile da compromettere. Al contrario, meno anelli sono presenti, più probabilmente la toolchain sarà resiliente.

Tutti i progetti web sono diversi e occorre valutare quali parti della toolchain siano necessarie, considerando attentamente ciascuna parte.

La toolchain più piccola è quella che non ha alcun anello. Si scriverebbe manualmente l'HTML, si userebbe "vanilla JavaScript" — cioè nessun framework o linguaggio intermedio — e si caricherebbe tutto manualmente su un server per l'hosting.

Tuttavia, requisiti software più complessi probabilmente beneficeranno dell'uso di strumenti che semplificano il processo di sviluppo. Inoltre, è consigliabile includere test prima del deployment sul server di produzione, per assicurarsi che il software funzioni come previsto: questa sembra già una toolchain necessaria.

Per il progetto di esempio, verrà utilizzata una toolchain progettata specificamente per facilitare lo sviluppo software e supportare le scelte tecniche effettuate durante la fase di progettazione del software. Verrà però evitato qualsiasi strumento superfluo, con l'obiettivo di mantenere la complessità al minimo.

## Verifica dei prerequisiti

La maggior parte del software dovrebbe già essere disponibile se sono stati seguiti i capitoli precedenti. Ecco cosa occorre avere prima di procedere ai passaggi di configurazione veri e propri. Questi devono essere eseguiti una sola volta e non devono essere ripetuti per progetti futuri.

### Creazione di un account GitHub

Oltre agli strumenti che verranno installati e che contribuiscono alla toolchain, occorre creare un account GitHub per completare il tutorial. È comunque possibile seguire la parte relativa allo sviluppo locale senza un account. Come già detto, GitHub è un servizio di repository di codice sorgente che aggiunge funzionalità di comunità come il tracciamento dei problemi, il monitoraggio delle release dei progetti e molto altro. Nel capitolo successivo, verrà eseguito il push a un repository di codice GitHub, causando un effetto a cascata che dovrebbe distribuire tutto il software in una sede sul web.

Registrarsi a [GitHub](https://github.com/) facendo clic sul collegamento _Sign Up_ nella homepage, se non si possiede già un account, quindi seguire le istruzioni.

### Installazione di git

Verrà installato un altro software, git, per facilitare il controllo delle revisioni.

È possibile che si sia già sentito parlare di "git". [Git](https://git-scm.com/) è attualmente lo strumento di controllo delle revisioni del codice sorgente più diffuso tra gli sviluppatori. Il controllo delle revisioni offre molti vantaggi, come un modo per eseguire il backup del lavoro in una posizione remota e un meccanismo per lavorare in team sullo stesso progetto senza il timore di sovrascrivere il codice altrui.

Potrebbe essere ovvio per alcuni, ma vale la pena ripeterlo: Git non è la stessa cosa di GitHub. Git è lo strumento di controllo delle revisioni, mentre [GitHub](https://github.com/) è un archivio online per repository git, oltre a numerosi strumenti utili per lavorare con essi. Si noti che, anche se in questo capitolo viene usato GitHub, esistono varie alternative, tra cui [GitLab](https://about.gitlab.com/) e [Bitbucket](https://www.atlassian.com/software/bitbucket); è persino possibile ospitare i propri repository git.

Usare il controllo delle revisioni nei progetti e includerlo nella toolchain aiuta a gestire l'evoluzione del codice. Offre un modo per eseguire il "commit" di blocchi di lavoro durante l'avanzamento, insieme a commenti come "Implementata nuova funzionalità X" oppure "Bug Z corretto grazie alle modifiche Y".

Il controllo delle revisioni consente anche di creare un _branch_ del codice del progetto, creando una versione separata su cui provare nuove funzionalità, senza che tali modifiche influenzino il codice originale.

Infine, può aiutare ad annullare modifiche o a ripristinare il codice a un momento "in cui funzionava" se è stato introdotto un errore da qualche parte e risulta difficile correggerlo: qualcosa che tutti gli sviluppatori devono fare di tanto in tanto.

Git può essere [scaricato e installato tramite il sito web git-scm](https://git-scm.com/downloads/): scaricare l'installer adatto al sistema, eseguirlo e seguire le istruzioni sullo schermo. Per ora è tutto ciò che occorre fare.

È possibile interagire con git in diversi modi: dalla riga di comando per impartire comandi, all'uso di un'[app GUI per git](https://git-scm.com/downloads/guis) per impartire gli stessi comandi premendo pulsanti, oppure direttamente dall'interno dell'editor di codice, come mostrato nell'esempio di Visual Studio Code seguente:

![Integrazione Git mostrata in VS Code](vscode-git.png)

### Progetto esistente

Si lavorerà sul progetto già avviato nel capitolo precedente, quindi assicurarsi di seguire le istruzioni in [Gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per configurare prima il progetto. Per ricapitolare, ecco cosa dovrebbe essere disponibile:

- Node.js e npm installati.
- Un nuovo progetto chiamato `npm-experiment` o con un altro nome.
- Vite installato come dev dependency.
- Il pacchetto `plotly.js-dist-min` installato come dependency.
- Alcuni script personalizzati definiti in package.json.
- I file `index.html` e `src/main.jsx` creati.

Come illustrato nel [Capitolo 1](/it/docs/Learn_web_development/Extensions/Client-side_tools/Overview), la toolchain sarà strutturata nelle seguenti fasi:

- **Ambiente di sviluppo**: gli strumenti più fondamentali per eseguire il codice. Questa parte è già stata configurata nel capitolo precedente.
- **Rete di sicurezza**: rende l'esperienza di sviluppo software stabile e più efficiente. Si può fare riferimento a questa parte anche come ambiente di sviluppo.
- **Trasformazione**: strumenti che consentono di usare le funzionalità più recenti di un linguaggio, ad esempio JavaScript, o un linguaggio completamente diverso, ad esempio JSX o TypeScript, nel processo di sviluppo, quindi trasformano il codice affinché la versione di produzione continui a funzionare su un'ampia varietà di browser, moderni e meno recenti.
- **Post-sviluppo**: strumenti che entrano in gioco dopo aver completato la parte principale dello sviluppo, per assicurarsi che il software arrivi sul web e continui a funzionare. In questo caso di studio verranno esaminati l'aggiunta di test al codice e il deployment dell'app tramite GitHub Pages, affinché sia disponibile sul web per tutti.

Iniziamo a lavorare su questi aspetti, partendo dall'ambiente di sviluppo. Verranno seguiti gli stessi passaggi impiegati per configurare un progetto reale, così che in futuro, durante la configurazione di un nuovo progetto, sarà possibile fare riferimento a questo capitolo e seguire nuovamente i passaggi.

## Creazione di un ambiente di sviluppo

A volte questa parte della toolchain viene considerata un ritardo del lavoro effettivo, ed è molto facile cadere in una "tana del coniglio" degli strumenti, spendendo molto tempo per rendere l'ambiente "perfetto".

Tuttavia, è possibile guardare a questo processo nello stesso modo in cui si configura l'ambiente di lavoro fisico. La sedia deve essere comoda e collocata in una buona posizione per favorire la postura. Servono alimentazione, Wi-Fi e porte USB. Potrebbero essere importanti decorazioni o musica che aiutano lo stato mentale: tutti questi elementi sono importanti per svolgere il miglior lavoro possibile e, se configurati correttamente, dovrebbero richiedere una sola configurazione.

Allo stesso modo, configurare bene l'ambiente di sviluppo richiede di farlo una sola volta e dovrebbe renderlo riutilizzabile in molti progetti futuri. Probabilmente sarà opportuno riesaminare questa parte della toolchain a intervalli semi-regolari e valutare se introdurre aggiornamenti o modifiche, ma ciò non dovrebbe essere necessario troppo spesso.

La toolchain dipenderà dalle esigenze specifiche, ma per questo esempio di una toolchain abbastanza completa, gli strumenti che verranno installati o inizializzati in anticipo sono:

- Strumenti per l'installazione di librerie, per aggiungere dependency.
- Controllo delle revisioni del codice.
- Strumenti per riordinare il codice, per riordinare JavaScript, CSS e HTML.
- Strumenti di linting del codice, per eseguire il linting del codice.

### Strumenti per l'installazione di librerie

Questa operazione è già stata eseguita, ma per praticità, ecco i comandi da eseguire nella radice della directory `npm-experiment` per inizializzare un pacchetto npm e installare le dependency necessarie:

```bash
npm init
npm install --save-dev vite
npm install plotly.js-dist-min
```

### Controllo delle revisioni del codice

Inserire il comando seguente per avviare la funzionalità di controllo del codice sorgente di git nella directory:

```bash
git init
```

Per impostazione predefinita, git tiene traccia delle modifiche di tutti i file. Esistono tuttavia alcuni file generati che non è necessario tracciare, poiché non sono codice scritto manualmente e possono essere rigenerati in qualsiasi momento. È possibile comunicare a git di ignorare questi file creando un file `.gitignore` nella radice della directory del progetto. Aggiungere il seguente contenuto al file:

```plain
node_modules
dist
```

### Strumenti per riordinare il codice

Verrà usato Prettier, incontrato per la prima volta nel Capitolo 2, per riordinare il codice di questo progetto. Prettier verrà installato nuovamente nel progetto. Installarlo usando il comando seguente:

```bash
npm install --save-dev prettier
```

Si noti nuovamente che viene usato `--save-dev` per aggiungerlo come dev dependency, perché viene utilizzato solo durante lo sviluppo.

Come molti strumenti creati più di recente, Prettier è dotato di "impostazioni predefinite sensate". Ciò significa che sarà possibile usare Prettier senza configurare nulla, se si è soddisfatti delle [impostazioni predefinite](https://prettier.io/docs/configuration.html). Questo consente di concentrarsi su ciò che è importante: il lavoro creativo. A scopo dimostrativo, verrà aggiunto un file di configurazione. Creare un file nella radice della directory `npm-experiment` chiamato `.prettierrc.json`. Aggiungere il seguente contenuto:

```json
{
  "bracketSameLine": true
}
```

Con questa impostazione, Prettier stamperà il `>` di un tag di apertura HTML su più righe, ossia HTML, JSX, Vue o Angular, alla fine dell'ultima riga anziché lasciarlo da solo sulla riga successiva. Questo è il formato utilizzato da MDN stesso. Per ulteriori informazioni sulla [configurazione di Prettier](https://prettier.io/docs/configuration.html), consultare la relativa documentazione.

Per impostazione predefinita, Prettier formatta tutti i file specificati. Tuttavia, ancora una volta, non è necessario formattare i file generati, oppure potrebbe esserci del codice legacy che non si desidera modificare. È possibile dire a Prettier di ignorare sempre questi file creando un file `.prettierignore` nella radice della directory del progetto. Aggiungere il seguente contenuto al file:

```plain
node_modules
dist
```

Il contenuto è lo stesso di `.gitignore`, ma in un progetto reale si potrebbero voler ignorare file diversi per Prettier rispetto a git.

Ora che Prettier è installato e configurato, è possibile eseguire la formattazione e il riordino del codice dalla riga di comando, ad esempio:

```bash
npx prettier --write ./index.html
```

> [!NOTE]
> Nel comando precedente, Prettier viene usato con il flag `--write`. Prettier interpreta questo flag nel senso di "se nel formato del codice c'è qualche problema, correggilo e poi salva il file". Questo va bene per il processo di sviluppo, ma è anche possibile usare `prettier` senza il flag e controllerà soltanto il file. Controllare il file, senza salvarlo, è utile per controlli eseguiti prima di una release, ad esempio: "non distribuire codice che non sia stato formattato correttamente".

È inoltre possibile sostituire `./index.html` con qualsiasi altro file o cartella per formattarli. Per esempio, `.` formatterà tutto ciò che si trova nella directory corrente. Per evitare di dimenticare la sintassi, è possibile aggiungerla anche come script personalizzato in package.json:

```json
{
  "scripts": {
    // …
    "format": "prettier --write ."
  }
}
```

Ora è possibile eseguire quanto segue per formattare la directory:

```bash
npm run format
```

Può comunque essere oneroso eseguire il comando ogni volta che viene modificato qualcosa, ed esistono alcuni modi per automatizzare questo processo:

- Usare speciali "git hook" per verificare se il codice è formattato prima di un commit.
- Usare plugin dell'editor di codice per eseguire comandi Prettier ogni volta che viene salvato un file.

> [!NOTE]
> Cos'è un git hook? Git, non GitHub, fornisce un sistema che permette di associare azioni precedenti e successive alle attività eseguite con git, come il commit del codice. Sebbene i git hook possano risultare un po' troppo complicati, secondo l'autore, una volta configurati possono essere molto potenti. Per chi fosse interessato a usare gli hook, [Husky](https://github.com/typicode/husky) offre un percorso notevolmente semplificato.

Per VS Code, un'estensione utile è [Prettier Code Formatter di Esben Petersen](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode), che consente a VS Code di formattare automaticamente il codice al salvataggio. Ciò significa che ogni file del progetto su cui si lavora viene formattato correttamente, inclusi HTML, CSS, JavaScript, JSON, markdown e altro ancora. L'editor deve soltanto avere abilitata l'opzione "Format On Save".

### Strumenti di linting del codice

Il linting aiuta la qualità del codice, ma è anche un modo per rilevare potenziali errori più presto durante lo sviluppo. È un ingrediente fondamentale di una buona toolchain e molti progetti di sviluppo lo includono per impostazione predefinita.

Gli strumenti di linting per lo sviluppo web esistono soprattutto per JavaScript, anche se ne esistono alcuni per HTML e CSS. Questo ha senso: se viene usato un elemento HTML sconosciuto o una proprietà CSS non valida, a causa della natura resiliente di questi due linguaggi è improbabile che qualcosa si interrompa. JavaScript è molto più fragile: chiamare per errore una funzione che non esiste, per esempio, causa l'interruzione di JavaScript. Eseguire il linting di JavaScript è quindi molto importante, specialmente per i progetti più grandi.

Lo strumento di riferimento per il linting JavaScript è [ESLint](https://eslint.org/). È uno strumento estremamente potente e versatile, ma può essere difficile configurarlo correttamente e si potrebbero facilmente consumare molte ore per ottenere una configurazione _perfetta_.

ESLint viene installato tramite npm, quindi, come discusso nel Capitolo 2, è possibile scegliere se installare lo strumento localmente o globalmente, ma è vivamente consigliata un'installazione locale, poiché è comunque necessario avere un file di configurazione per ogni progetto. Ricordare il comando da eseguire:

```bash
npm install --save-dev eslint@9 @eslint/js@9 globals
```

> [!NOTE]
> Lo specificatore `@9` installa la release più recente della versione major v9. Mantenere allineate le versioni major di `eslint` e `@eslint/js`, affinché le configurazioni predefinite rimangano compatibili. Al momento della scrittura, l'ultima versione di ESLint è v10. Tuttavia, di solito i plugin impiegano un po' di tempo per recuperare, quindi per ora viene mantenuta la v9. Quando problemi come la [compatibilità di `eslint-plugin-react` con ESLint v10](https://github.com/jsx-eslint/eslint-plugin-react/issues/3977) saranno risolti, sono benvenuti contributi per aggiornare l'articolo all'uso delle versioni più recenti.

Il pacchetto `@eslint/js` fornisce una configurazione ESLint predefinita, mentre il pacchetto `globals` fornisce un elenco di nomi globali noti in ogni ambiente. Verranno usati successivamente nella configurazione. Senza ulteriori configurazioni, ESLint segnalerà di non trovare il file di configurazione se viene eseguito con `npx eslint`:

```plain
Oops! Something went wrong! :(

ESLint: 9.39.4

ESLint couldn't find an eslint.config.(js|mjs|cjs) file.

...
```

Ecco un esempio minimo funzionante, in un file chiamato `eslint.config.js` nella radice del progetto:

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

La configurazione ESLint precedente:

- Abilita le impostazioni ESLint "recommended".
- Indica a ESLint di ignorare i file generati, come già fatto per gli altri strumenti.
- Indica a ESLint di includere i file `.js` e `.jsx` nel linting.
- Indica a ESLint l'esistenza delle variabili globali del browser, usate da regole di lint come `no-undef` per controllare variabili inesistenti.

Il parser ESLint non comprende JSX per impostazione predefinita e le relative regole consigliate non gestiscono la semantica specifica di React. Verrà quindi aggiunta ulteriore configurazione per supportare correttamente JSX e React. Prima di tutto, installare `eslint-plugin-react` ed `eslint-plugin-react-hooks`, che forniscono regole per scrivere React corretto e idiomatico:

```bash
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks
```

Quindi aggiornare il file di configurazione ESLint per includere la configurazione consigliata di questi plugin, che carica sia le regole consigliate sia le opzioni del parser per JSX:

```js
import js from "@eslint/js";
import globals from "globals";
import reactPlugin from "eslint-plugin-react";
import reactHooks from "eslint-plugin-react-hooks";

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
  reactPlugin.configs.flat.recommended,
  reactPlugin.configs.flat["jsx-runtime"],
  reactHooks.configs.flat.recommended,
];
```

Esiste un [elenco completo delle regole ESLint](https://eslint.org/docs/latest/rules/) che può essere modificato e configurato a piacere; molte aziende e team hanno inoltre pubblicato le [proprie configurazioni ESLint](https://www.npmjs.com/search?q=keywords:eslintconfig), talvolta utili per trovare ispirazione o per sceglierne una adatta ai propri standard. Un avvertimento, tuttavia: la configurazione ESLint è una tana del coniglio molto profonda.

Per semplicità, in questo capitolo non verranno esplorate tutte le funzionalità di ESLint, poiché questa configurazione funziona per il progetto e i relativi requisiti specifici. Tenere comunque presente che, se si desidera perfezionare e applicare una regola relativa all'aspetto o alla validazione del codice, è molto probabile che ciò possa essere fatto con la configurazione ESLint corretta.

Come per gli altri strumenti, il supporto di integrazione con gli editor di codice è generalmente valido per ESLint e potenzialmente più utile perché può fornire feedback in tempo reale quando emergono problemi:

![Integrazione degli errori ESLint mostrata in VS Code](eslint-error.png)

A questo punto la configurazione dell'ambiente di sviluppo è completa. Ora, finalmente, è quasi tutto pronto per scrivere codice.

## Strumenti di build e trasformazione

### Trasformazione JavaScript

Per questo progetto, come già detto, verrà usato React, il che significa anche che nel codice sorgente verrà usato JSX. Il progetto utilizzerà inoltre le funzionalità JavaScript più recenti. Un problema immediato è che nessun browser supporta JSX in modo nativo: è un linguaggio intermedio destinato a essere compilato in linguaggi compresi dal browser nel codice di produzione. Se il browser tenta di eseguire il JavaScript sorgente, segnalerà immediatamente un problema; il progetto necessita di uno strumento di build per trasformare il codice sorgente in qualcosa che il browser possa usare senza problemi.

Esistono diverse opzioni per gli strumenti di trasformazione e, sebbene Babel sia particolarmente diffuso, in Vite verrà usato un plugin integrato: `@vitejs/plugin-react`. Installarlo usando il comando seguente:

```bash
npm install --save-dev @vitejs/plugin-react
```

Non esiste ancora una configurazione Vite. Aggiungerne una in `vite.config.js` nella radice della directory del progetto:

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  base: "/npm-experiment/",
});
```

Leggere la [documentazione di Vite](https://vite.dev/guide/) per ulteriori informazioni su come configurare Vite. Poiché il sito viene distribuito su GitHub Pages, sarà ospitato all'indirizzo `https://your-username.github.io/your-repo-name`, quindi occorre impostare l'opzione `base` in base al nome del repository GitHub. È comunque possibile modificarla in seguito, quando si arriverà al [deployment](/it/docs/Learn_web_development/Extensions/Client-side_tools/Deployment).

### Trasformazione CSS

Anche il CSS può usare sintassi non compresa dai browser. Per esempio, potrebbe essere usata una sintassi implementata solo nelle ultime versioni di alcuni browser, il che significa che i browser meno recenti non riusciranno a interpretarla e visualizzeranno stili non corretti. È possibile usare uno strumento per trasformare il CSS in un formato comprensibile da tutti i browser di destinazione.

[PostCSS](https://postcss.org/) è uno strumento di post-elaborazione CSS. Rispetto agli strumenti di build come [Sass](https://sass-lang.com/), PostCSS è destinato alla scrittura di CSS _standard_, ovvero sintassi CSS che potrebbe arrivare nei browser un giorno, mentre Sass è un linguaggio personalizzato a sé stante che compila in CSS. PostCSS è più vicino al web e ha una curva di apprendimento molto più bassa. [Vite supporta PostCSS per impostazione predefinita](https://vite.dev/guide/features.html#postcss), quindi è sufficiente [configurare PostCSS](https://github.com/postcss/postcss#usage) se si desidera compilare delle funzionalità. Consultare [cssdb](https://preset-env.cssdb.org/features/) per vedere quali funzionalità sono supportate.

Per questo scopo, verrà mostrata un'altra trasformazione CSS: i [CSS module](https://vite.dev/guide/features.html#css-modules). È uno dei modi per ottenere la _modularizzazione CSS_. Ricordare che tutti i selettori CSS sono globali, quindi se esiste un nome di classe come `.button`, tutti gli elementi con il nome di classe `button` verranno stilizzati allo stesso modo. Ciò porta spesso a conflitti di nomi: immaginare che tutte le variabili JavaScript siano definite nello scope globale. I CSS module risolvono questo problema rendendo il nome della classe univoco per le pagine che lo usano. Per comprendere come funziona, dopo aver scaricato il codice sorgente, è possibile controllare come vengono usati i file `.module.css` e leggere anche la [documentazione dei CSS module](https://github.com/css-modules/css-modules).

Sebbene questa fase della toolchain possa essere piuttosto dolorosa, poiché è stato scelto uno strumento che cerca intenzionalmente di ridurre configurazione e complessità, non c'è davvero altro da fare durante la fase di sviluppo. I moduli vengono importati correttamente, il CSS annidato viene trasformato correttamente in "CSS normale" e lo sviluppo non viene ostacolato dal processo di build.

Ora il software è pronto per essere scritto!

## Scrittura del codice sorgente

Ora che la toolchain di sviluppo completa è configurata, di solito è il momento di iniziare a scrivere codice reale: la parte in cui investire effettivamente la maggior parte del tempo. Per questo scopo, tuttavia, verrà soltanto copiato del codice sorgente esistente e fatto finta di averlo scritto. Non verrà insegnato come funziona, poiché non è questo lo scopo del capitolo. Il codice è presente esclusivamente per eseguire gli strumenti su di esso e insegnare come funzionano _questi ultimi_.

Per ottenere i file di codice, visitare <https://github.com/mdn/client-toolchain-example> e scaricare e decomprimere il contenuto di questo repository in una posizione sul disco locale. È possibile scaricare l'intero progetto come file zip selezionando _Clone or download_ > _Download ZIP_.

![Il repository di esempio su GitHub](github-repo.png)

Ora copiare il contenuto della directory `src` del progetto e usarlo per sostituire l'attuale directory `src`. Non è necessario preoccuparsi degli altri file.

Installare inoltre alcune dependency usate dal codice sorgente:

```bash
npm install react react-dom @tanstack/react-query
```

I file del progetto sono ora al loro posto. Per ora è tutto ciò che occorre fare.

## Esecuzione della trasformazione

Per iniziare a lavorare con il progetto, verrà eseguito il server Vite dalla riga di comando. Nella modalità predefinita osserverà le modifiche del codice e aggiornerà il server. Questo è utile perché non è necessario passare continuamente dal codice alla riga di comando.

1. Per avviare Vite in background, aprire il terminale ed eseguire il comando seguente, usando lo script personalizzato definito in precedenza:

   ```bash
   npm run dev
   ```

   Dovrebbe essere visualizzato un output simile a questo, dopo l'installazione delle dependency:

   ```plain
   > client-toolchain-example@1.0.0 dev
   > vite

   Re-optimizing dependencies because lockfile has changed

     VITE v5.2.13  ready in 157 ms

     ➜  Local:   http://localhost:5173/
     ➜  Network: use --host to expose
     ➜  press h + enter to show help
   ```

   Il server ora è in esecuzione all'URL stampato, in questo caso localhost:5173.

2. Aprire questo URL nel browser per vedere l'app di esempio in esecuzione.

Ora è possibile apportare alcune modifiche e visualizzarne gli effetti in tempo reale.

1. Aprire il file `src/App.jsx` nell'editor di testo preferito.
2. Sostituire tutte le occorrenze di `mdn/content` con il repository GitHub preferito, ad esempio `facebook/react`.
3. Salvare il file, quindi tornare direttamente all'app in esecuzione nel browser. Si noterà che il browser si è aggiornato automaticamente e che i grafici sono cambiati.

È inoltre possibile provare a usare ESLint e Prettier: rimuovere deliberatamente gran parte degli spazi bianchi da uno dei file ed eseguire Prettier su di esso per ripulirlo, oppure introdurre un errore di sintassi in uno dei file JavaScript e vedere quali errori restituisce ESLint quando viene eseguito il comando `eslint`, o nell'editor.

## Riepilogo

In questo capitolo è stata fatta molta strada, creando un ambiente di sviluppo locale piuttosto valido per sviluppare un'applicazione.

A questo punto dello sviluppo di software web, normalmente si scriverebbe il codice per il software che si intende creare. Poiché questo modulo riguarda gli strumenti attorno allo sviluppo web, e non il codice di sviluppo web stesso, non verrà insegnata alcuna programmazione effettiva: queste informazioni sono disponibili nel resto di MDN.

È stato invece scritto un progetto di esempio su cui usare gli strumenti. Si suggerisce di affrontare il resto del capitolo usando il codice di esempio, quindi provare a sostituire il contenuto della directory src con un proprio progetto e pubblicarlo invece su GitHub Pages. Infatti, il deployment su GitHub Pages sarà l'obiettivo finale del prossimo capitolo.

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_tools/Package_management","Learn_web_development/Extensions/Client-side_tools/Deployment", "Learn_web_development/Extensions/Client-side_tools")}}
