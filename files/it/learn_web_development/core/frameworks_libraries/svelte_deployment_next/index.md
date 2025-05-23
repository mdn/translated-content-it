---
title: Distribuzione e passi successivi
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'articolo precedente abbiamo appreso il supporto di Svelte per TypeScript e come utilizzarlo per rendere la tua applicazione più robusta. In questo articolo finale vedremo come distribuire la tua applicazione e metterla online, e inoltre condivideremo alcune risorse da cui proseguire per continuare il tuo percorso di apprendimento di Svelte.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Come minimo, si raccomanda che tu sia familiare con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          che tu abbia conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          Avrai bisogno di un terminale con node + npm installati per compilare e costruire
          la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a preparare la nostra app Svelte per la produzione e quali risorse di apprendimento consultare in seguito.
      </td>
    </tr>
  </tbody>
</table>

## Codifica con noi

### Git

Clona il repository GitHub (se non l'hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Quindi, per arrivare allo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/08-next-steps
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/08-next-steps
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

## Compilare la nostra app

Finora abbiamo eseguito la nostra app in modalità sviluppo con `npm run dev`. Come abbiamo visto prima, questa istruzione dice a Svelte di compilare i nostri componenti e file JavaScript in un file `public/build/bundle.js` e tutte le sezioni CSS dei nostri componenti in `public/build/bundle.css`. Inoltre avvia un server di sviluppo e osserva le modifiche, ricompilando l'app e aggiornando la pagina quando si verifica una modifica.

I tuoi file `bundle.js` e `bundle.css` generati saranno qualcosa del genere (dimensioni del file a sinistra):

```plain
  504 Jul 13 02:43 bundle.css
95981 Jul 13 02:43 bundle.js
```

Per compilare la nostra applicazione per la produzione dobbiamo eseguire invece `npm run build`. In questo caso, Svelte non avvierà un server web né continuerà a monitorare le modifiche. Tuttavia, comprimerà e comprimerà i nostri file JavaScript usando [terser](https://terser.org/).

Quindi, dopo aver eseguito `npm run build`, i nostri file `bundle.js` e `bundle.css` generati saranno più simili a questo:

```plain
  504 Jul 13 02:43 bundle.css
21782 Jul 13 02:43 bundle.js
```

Prova a eseguire `npm run build` nella directory principale della tua app ora. Potresti ricevere un avviso, ma per ora puoi ignorarlo.

Tutta la nostra app ora è solo 21 KB — 8.3 KB quando compressa. Non ci sono runtime aggiuntivi o dipendenze da scaricare, analizzare, eseguire e mantenere in memoria. Svelte ha analizzato i nostri componenti e compilato il codice in JavaScript puro.

## Uno sguardo al processo di compilazione di Svelte

Per impostazione predefinita, quando crei una nuova app con `npx degit sveltejs/template my-svelte-project`, Svelte userà [rollup](https://rollupjs.org/) come modulo bundler.

> [!NOTE]
> Esiste anche un modello ufficiale per l'uso di [webpack](https://webpack.js.org/) e anche molti [plugin mantenuti dalla comunità](https://github.com/sveltejs/integrations#bundler-plugins) per altri bundler.

Nel file `package.json` puoi vedere che gli script `build` e `dev` stanno semplicemente chiamando rollup:

```json
"scripts": {
  "build": "rollup -c",
  "dev": "rollup -c -w",
  "start": "sirv public"
},
```

Nello script `dev` stiamo passando l'argomento `-w`, che dice a rollup di osservare i file e ricostruire in caso di modifiche.

Se diamo un'occhiata al file `rollup.config.js`, possiamo vedere che il compilatore Svelte è semplicemente un plugin di rollup:

```js
import svelte from "rollup-plugin-svelte";
// …
import { terser } from "rollup-plugin-terser";

const production = !process.env.ROLLUP_WATCH;

export default {
  input: "src/main.js",
  output: {
    sourcemap: true,
    format: "iife",
    name: "app",
    file: "public/build/bundle.js",
  },
  plugins: [
    svelte({
      // enable run-time checks when not in production
      dev: !production,
      // we'll extract any component CSS out into
      // a separate file - better for performance
      css: (css) => {
        css.write("public/build/bundle.css");
      },
    }),
    // More plugins…
  ],
  // …
};
```

Più avanti nello stesso file vedrai anche come rollup minimizza i nostri script in modalità produzione e avvia un server locale in modalità sviluppo:

```js
export default {
  // …
  plugins: [
    // …
    // In dev mode, call `npm run start` once
    // the bundle has been generated
    !production && serve(),

    // Watch the `public` directory and refresh the
    // browser on changes when not in production
    !production && livereload("public"),

    // If we're building for production (npm run build
    // instead of npm run dev), minify
    production && terser(),
  ],
  // …
};
```

Ci sono [molti plugin per rollup](https://github.com/rollup/awesome) che ti permettono di personalizzare il suo comportamento. Un plugin particolarmente utile che è anche mantenuto dal team Svelte è [svelte-preprocess](https://github.com/sveltejs/svelte-preprocess), che pre-elabora molti linguaggi diversi nei file Svelte come PostCSS, SCSS, Less, CoffeeScript, SASS e TypeScript.

## Distribuire la tua applicazione Svelte

Dal punto di vista di un server web, un'applicazione Svelte non è altro che un insieme di file HTML, CSS e JavaScript. Tutto ciò di cui hai bisogno è un server web in grado di servire file statici, il che significa che hai molte opzioni tra cui scegliere. Diamo un'occhiata a un paio di esempi.

> [!NOTE]
> La sezione seguente potrebbe essere applicata a qualsiasi sito web statico client-side che richieda una fase di build, non solo alle app Svelte.

### Distribuire con Vercel

Uno dei modi più semplici per distribuire un'applicazione Svelte è utilizzare [Vercel](https://vercel.com/home). Vercel è una piattaforma cloud specificamente progettata per siti statici, che ha un supporto integrato per la maggior parte degli strumenti front-end comuni, Svelte essendo uno di questi.

Per distribuire la nostra app, segui questi passaggi.

1. [registrati per un account con Vercel](https://vercel.com/signup).
2. Naviga nella radice della tua app ed esegui `npx vercel`; la prima volta che lo fai, ti verrà chiesto di inserire il tuo indirizzo email e seguire i passaggi nell'email inviata a quell'indirizzo, per motivi di sicurezza.
3. Esegui di nuovo `npx vercel`, e ti verrà chiesto di rispondere a qualche domanda, come questa:

   ```bash
   npx vercel
   ```

   ```plain
   Vercel CLI 19.1.2
   ? Set up and deploy "./mdn-svelte-tutorial"? [Y/n] y
   ? Which scope do you want to deploy to? opensas
   ? Link to existing project? [y/N] n
   ? What's your project's name? mdn-svelte-tutorial
   ? In which directory is your code located? ./
   Auto-detected Project Settings (Svelte):
   - Build Command: `npm run build` or `rollup -c`
   - Output Directory: public
   - Development Command: sirv public --single --dev --port $PORT
   ? Want to override the settings? [y/N] n
      Linked to opensas/mdn-svelte-tutorial (created .vercel)
      Inspect: https://vercel.com/opensas/mdn-svelte-tutorial/[...] [1s]
   ✅  Production: https://mdn-svelte-tutorial.vercel.app [copied to clipboard] [19s]
      Deployed to production. Run `vercel --prod` to overwrite later (https://vercel.link/2F).
      To change the domain or build command, go to https://zeit.co/opensas/mdn-svelte-tutorial/settings
   ```

4. Accetta tutti i valori predefiniti e sarai a posto.
5. Una volta terminata la distribuzione, vai all'URL di "Produzione" nel tuo browser e vedrai l'app distribuita!

Puoi anche [importare un progetto Svelte da git](https://vercel.com/import/svelte) in Vercel da [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/) o [Bitbucket](https://bitbucket.org/product/).

> [!NOTE]
> Puoi installare globalmente Vercel con `npm i -g vercel` così non dovrai usare `npx` per eseguirlo.

### Distribuzione automatica su GitLab pages

Per l'hosting di file statici ci sono diversi servizi online che ti consentono di distribuire automaticamente il tuo sito ogni volta che apporti modifiche a un repository git. La maggior parte di essi prevede la configurazione di una pipeline di distribuzione che viene attivata ad ogni `git push` e si occupa di costruire e distribuire il tuo sito web.

Per dimostrare questo, distribuiremo la nostra app todos su [GitLab Pages](https://docs.gitlab.com/user/project/pages/).

1. Prima dovrai [registrarti su GitLab](https://gitlab.com/users/sign_up) e poi [creare un nuovo progetto](https://gitlab.com/projects/new). Dai al tuo nuovo progetto un nome breve e facile come "mdn-svelte-todo". Avrai un URL remoto che punta al tuo nuovo repository git GitLab, come `git@gitlab.com:[your-user]/[your-project].git`.
2. Prima di iniziare a caricare contenuti nel tuo repository git, è una buona pratica aggiungere un file `.gitignore` per dire a git quali file escludere dal controllo del codice sorgente. Nel nostro caso diremo a git di escludere i file nella directory `node_modules` creando un file `.gitignore` nella cartella principale del tuo progetto locale, con il seguente contenuto:

   ```bash
   node_modules/
   ```

3. Ora torniamo a GitLab. Dopo aver creato un nuovo repo GitLab ti accoglierà con un messaggio che spiega diverse opzioni per caricare i tuoi file esistenti. Segui i passaggi elencati sotto l'intestazione _Push an existing folder_:

   ```bash
   cd your_root_directory # Go into your project's root directory
   git init
   git remote add origin https://gitlab.com/[your-user]/mdn-svelte-todo.git
   git add .
   git commit -m "Initial commit"
   git push -u origin main
   ```

   > [!NOTE]
   > Potresti usare [il protocollo `git`](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols#_the_git_protocol) invece di `https`, che è più veloce e ti risparmia di digitare il tuo nome utente e la tua password ogni volta che accedi al tuo repo origin. Per utilizzarlo dovrai [creare una coppia di chiavi SSH](https://docs.gitlab.com/user/ssh/#generate-an-ssh-key-pair). Il tuo URL origin sarà simile a questo: `git@gitlab.com:[your-user]/mdn-svelte-todo.git`.

Con queste istruzioni inizializziamo un repository git locale, quindi impostiamo il nostro origin remoto (dove invieremo il nostro codice) come il nostro repo su GitLab. Successivamente inviamo tutti i file al repository git locale e quindi li inviamo al origin remoto su GitLab.

GitLab utilizza uno strumento integrato chiamato GitLab CI/CD per costruire il tuo sito e pubblicarlo sul server di GitLab Pages. La sequenza di script che GitLab CI/CD esegue per eseguire questo compito viene creata da un file chiamato `.gitlab-ci.yml`, che puoi creare e modificare a piacere. Un lavoro specifico chiamato `pages` nel file di configurazione renderà GitLab consapevole che stai distribuendo un sito web GitLab Pages.

Proviamo a farlo ora.

1. Crea un file `.gitlab-ci.yml` all'interno della radice del tuo progetto e dagli il seguente contenuto:

   ```yaml
   image: node:latest
   pages:
     stage: deploy
     script:
       - npm install
       - npm run build
     artifacts:
       paths:
         - public
     only:
       - main
   ```

   Qui stiamo dicendo a GitLab di utilizzare un'immagine con l'ultima versione di node per costruire la nostra app. Successivamente dichiariamo un lavoro `pages`, per abilitare GitLab Pages. Ogni volta che c'è un push nel nostro repo, GitLab eseguirà `npm install` e `npm run build` per costruire la nostra applicazione. Stiamo anche dicendo a GitLab di distribuire i contenuti della cartella `public`. All'ultima riga, stiamo configurando GitLab per ridistribuire la nostra app solo quando c'è un push al nostro ramo principale.

2. Poiché la nostra app sarà pubblicata in una sottodirectory (come `https://your-user.gitlab.io/mdn-svelte-todo`), dovremo rendere i riferimenti ai file JavaScript e CSS nel nostro file `public/index.html` relativi. Per farlo, rimuoviamo semplicemente le barre iniziali (`/`) dagli URL di `/global.css`, `/build/bundle.css` e `/build/bundle.js`, come questo:

   ```html
   <title>Svelte To-Do list</title>

   <link rel="icon" type="image/png" href="favicon.png" />
   <link rel="stylesheet" href="global.css" />
   <link rel="stylesheet" href="build/bundle.css" />

   <script defer src="build/bundle.js"></script>
   ```

   Fai questo ora.

3. Ora dobbiamo solo inviare e pushare le nostre modifiche su GitLab. Fai questo eseguendo i seguenti comandi:

   ```bash
   git add public/index.html
   git add .gitlab-ci.yml
   git commit -m "Added .gitlab-ci.yml file and fixed index.html absolute paths"
   git push
   ```

Ogni volta che c'è un lavoro in esecuzione, GitLab visualizzerà un'icona che mostra il processo del lavoro. Facendo clic su di essa potrai ispezionare l'output del lavoro.

![screenshot di gitlab che mostra un commit distribuito, che aggiunge un file di configurazione gitlab ci e cambia i percorsi dei bundle in relativi](01-gitlab-pages-deploy.png)

Puoi anche controllare il progresso dei lavori correnti e precedenti dall'opzione del menu _CI / CD_ > _Jobs_ del tuo progetto GitLab.

![un lavoro di gitlab ci mostrato nell'interfaccia utente di gitlab, esegue molti comandi](02-gitlab-pages-job.png)

Una volta che GitLab termina di costruire e pubblicare la tua app, sarà accessibile su `https://your-user.gitlab.io/mdn-svelte-todo/`; nel mio caso è `https://opensas.gitlab.io/mdn-svelte-todo/`. Puoi controllare l'URL della tua pagina nell'interfaccia utente di GitLab — vedi l'opzione del menu _Settings_ > _Pages_.

Con questa configurazione, ogni volta che invii modifiche al repo su GitLab, l'applicazione verrà automaticamente ricostruita e distribuita su GitLab Pages.

## Imparare di più su Svelte

In questa sezione ti forniremo alcune risorse e progetti da esaminare per approfondire il tuo apprendimento su Svelte.

### Documentazione Svelte

Per andare oltre e imparare di più su Svelte, dovresti sicuramente visitare la [homepage di Svelte](https://svelte.dev/). Lì troverai [molti articoli](https://svelte.dev/blog) che spiegano la filosofia di Svelte. Se non l'hai ancora fatto, assicurati di completare il [tutorial interattivo di Svelte](https://learn.svelte.dev/tutorial/welcome-to-svelte). Abbiamo già coperto la maggior parte del suo contenuto, quindi non ti ci vorrà molto tempo per completarlo — dovresti considerarlo come pratica!

Puoi anche consultare i [documenti dell'API di Svelte](https://svelte.dev/docs) e gli [esempi](https://svelte.dev/examples/hello-world) disponibili.

Per capire le motivazioni dietro Svelte, dovresti leggere la presentazione su YouTube di [Rich Harris](https://x.com/Rich_Harris), [Rethinking reactivity](https://www.youtube.com/watch?v=AdNJ3fydeao&t=47s). È il creatore di Svelte, quindi ha un paio di cose da dire al riguardo. Hai anche le diapositive interattive disponibili qui che sono, senza sorprese, costruite con Svelte. Se ti è piaciuta, ti piacerà anche la presentazione [The Return of 'Write Less, Do More'](https://www.youtube.com/watch?v=BzX4aTRPzno), che Rich Harris ha tenuto a [JSCAMP 2019](https://jscamp.tech/2019/).

### Progetti correlati

Ci sono altri progetti correlati a Svelte che vale la pena esaminare:

- [Sapper](https://sapper.svelte.dev/): Un framework per applicazioni basato su Svelte che fornisce rendering sul lato server (SSR), suddivisione del codice, routing basato sui file, supporto offline e altro ancora. Pensalo come [Next.js](https://nextjs.org/) per Svelte. Se stai pianificando di sviluppare un'applicazione web abbastanza complessa, dovresti sicuramente dare un'occhiata a questo progetto.
- [Svelte Native](https://svelte.nativescript.org/): Un framework per applicazioni mobili basato su Svelte. Pensalo come [React Native](https://reactnative.dev/) per Svelte.
- [Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode): Il plugin ufficialmente supportato per VS Code per lavorare con i file `.svelte`, che abbiamo esaminato nel nostro [articolo su TypeScript](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript).

### Altre risorse didattiche

- C'è un [corso completo su Svelte e Sapper](https://frontendmasters.com/courses/svelte/) di Rich Harris, disponibile su Frontend Masters.
- Anche se Svelte è un progetto relativamente giovane, ci sono molti tutorial e [corsi](https://www.udemy.com/topic/svelte-framework/?sort=popularity) disponibili sul web, quindi è difficile fare una raccomandazione.
- Tuttavia, [Svelte.js — The Complete Guide](https://www.udemy.com/course/sveltejs-the-complete-guide/) di [Academind](https://academind.com/) è un'opzione molto popolare con ottime valutazioni.
- [The Svelte Handbook](https://www.freecodecamp.org/news/the-svelte-handbook/), di [Flavio Copes](https://flaviocopes.com/), è anche un riferimento utile per apprendere i concetti principali di Svelte.
- Se preferisci leggere libri, c'è [Svelte and Sapper in Action](https://www.manning.com/books/svelte-and-sapper-in-action) di [Mark Volkman](https://x.com/mark_volkmann), pubblicato nell'ottobre 2020, che [puoi visualizzare online](https://livebook.manning.com/book/svelte-and-sapper-in-action/welcome) gratuitamente.
- Se desideri approfondire e comprendere il funzionamento interno del compilatore di Svelte, dovresti dare un'occhiata ai post del blog di [Tan Li Hau](https://x.com/lihautan), [_Compile Svelte in your head_](https://lihautan.com/compile-svelte-in-your-head).

### Interagire con la comunità

Ci sono diversi modi per ottenere supporto e interagire con la comunità Svelte:

- [svelte.dev/chat](https://discord.com/invite/yy75DKs): Il server Discord di Svelte.
- [@sveltejs](https://x.com/sveltejs): L'account Twitter ufficiale.
- [@sveltesociety](https://x.com/sveltesociety): Account Twitter della comunità Svelte.
- [Svelte Recipes](https://github.com/svelte-society/recipes-mvp#recipes-mvp): Repository guidato dalla comunità di ricette, consigli e migliori pratiche per risolvere problemi comuni.
- [Domande su Svelte su Stack Overflow](https://stackoverflow.com/questions/tagged/svelte): Domande con il tag `svelte` su SO.
- [Comunità Svelte su reddit](https://www.reddit.com/r/sveltejs/): Discussione sulla comunità Svelte e sito di valutazione dei contenuti su reddit.
- [Comunità Svelte su DEV](https://dev.to/t/svelte): Una raccolta di articoli tecnici e tutorial relativi a Svelte dalla comunità DEV.to.

## Finito

Congratulazioni! Hai completato il tutorial su Svelte. Negli articoli precedenti siamo passati da zero conoscenze su Svelte alla costruzione e distribuzione di un'applicazione completa.

- Abbiamo appreso la filosofia di Svelte e cosa lo distingue da altri framework front-end.
- Abbiamo visto come aggiungere comportamenti dinamici al nostro sito web, come organizzare la nostra app in componenti e diversi modi per condividere informazioni tra di essi.
- Abbiamo sfruttato il sistema di reattività di Svelte e appreso come evitare errori comuni.
- Abbiamo anche visto alcuni concetti avanzati e tecniche per interagire con elementi DOM e per estendere le capacità degli elementi HTML in modo programmatico utilizzando la direttiva `use`.
- Poi abbiamo visto come utilizzare gli store per lavorare con un repository di dati centrale e abbiamo creato il nostro store personalizzato per conservare i dati della nostra applicazione in Web Storage.
- Abbiamo anche dato un'occhiata al supporto TypeScript di Svelte.

In questo articolo abbiamo appreso un paio di opzioni semplici per distribuire la nostra app in produzione e abbiamo visto come impostare una pipeline di base per distribuire la nostra app su GitLab ad ogni commit. Poi ti abbiamo fornito un elenco di risorse Svelte per approfondire ulteriormente il tuo apprendimento su Svelte.

Congratulazioni! Dopo aver completato questa serie di tutorial dovresti avere una solida base da cui partire per sviluppare applicazioni web professionali con Svelte.

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}
