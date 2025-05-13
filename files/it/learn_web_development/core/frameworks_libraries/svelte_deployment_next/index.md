---
title: Distribuzione e prossimi passi
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'articolo precedente abbiamo imparato il supporto TypeScript di Svelte e come utilizzarlo per rendere la vostra applicazione più robusta. In questo ultimo articolo vedremo come distribuire la vostra applicazione e renderla online, e condivideremo anche alcune risorse che dovreste consultare per continuare il vostro percorso di apprendimento di Svelte.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Si consiglia almeno di avere familiarità con i linguaggi fondamentali 
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e 
          di avere conoscenze su 
          <a 
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/command line</a
          >.
        </p>
        <p>
          Lei avrà bisogno di un terminale con node + npm installati per compilare e costruire
          la sua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come preparare la nostra app Svelte per la produzione e quali risorse di apprendimento visitare successivamente.
      </td>
    </tr>
  </tbody>
</table>

## Seguire il codice con noi

### Git

Clonare il repository GitHub (se non l'ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Quindi, per arrivare allo stato attuale dell'app, eseguire

```bash
cd mdn-svelte-tutorial/08-next-steps
```

Oppure scaricare direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/08-next-steps
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

## Compilare la nostra app

Fino ad ora abbiamo eseguito la nostra app in modalità sviluppo con `npm run dev`. Come abbiamo visto prima, questa istruzione dice a Svelte di compilare i nostri componenti e i file JavaScript in un file `public/build/bundle.js` e tutte le sezioni CSS dei nostri componenti in `public/build/bundle.css`. Inoltre avvia un server di sviluppo e osserva le modifiche, ricompilando l'app e aggiornando la pagina quando si verifica una modifica.

I file generati `bundle.js` e `bundle.css` saranno qualcosa del genere (dimensione file a sinistra):

```plain
  504 Jul 13 02:43 bundle.css
95981 Jul 13 02:43 bundle.js
```

Per compilare la nostra applicazione per la produzione dobbiamo invece eseguire `npm run build`. In questo caso, Svelte non avvierà un server web né continuerà a osservare le modifiche. Tuttavia, comprimerà e minimizzerà i nostri file JavaScript usando [terser](https://terser.org/).

Quindi, dopo aver eseguito `npm run build`, i nostri file generati `bundle.js` e `bundle.css` saranno più simili a questo:

```plain
  504 Jul 13 02:43 bundle.css
21782 Jul 13 02:43 bundle.js
```

Provi a eseguire `npm run build` nella directory principale della sua app ora. Potrebbe ottenere un avviso, ma per ora può ignorarlo.

La nostra intera app ora è di soli 21 KB — 8.3 KB una volta compressa con gzip. Non ci sono runtime o dipendenze aggiuntive da scaricare, analizzare, eseguire ed eseguire in memoria. Svelte ha analizzato i nostri componenti e ha compilato il codice in JavaScript nativo.

## Uno sguardo dietro il processo di compilazione di Svelte

Di default, quando crea una nuova app con `npx degit sveltejs/template my-svelte-project`, Svelte utilizzerà [rollup](https://rollupjs.org/) come strumento di moduli.

> [!NOTE]
> C'è anche un template ufficiale per utilizzare [webpack](https://webpack.js.org/) e anche molti [plugin mantenuti dalla comunità](https://github.com/sveltejs/integrations#bundler-plugins) per altri strumenti di moduli.

Nel file `package.json` può vedere che gli script `build` e `dev` stanno semplicemente chiamando rollup:

```json
"scripts": {
  "build": "rollup -c",
  "dev": "rollup -c -w",
  "start": "sirv public"
},
```

Nello script `dev` stiamo passando l'argomento `-w`, che dice a rollup di osservare i file e ricostruire al variare delle modifiche.

Se diamo un'occhiata al file `rollup.config.js`, possiamo vedere che il compilatore di Svelte è solo un plugin di rollup:

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

Più avanti nello stesso file vedrai anche come rollup minimizzi i nostri script in modalità produzione e avvii un server locale in modalità sviluppo:

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

Ci sono [molti plugin per rollup](https://github.com/rollup/awesome) che permettono di personalizzare il suo comportamento. Un plugin particolarmente utile, che è anche mantenuto dal team di Svelte, è [svelte-preprocess](https://github.com/sveltejs/svelte-preprocess), che pre-elabora molti linguaggi diversi nei file Svelte come PostCSS, SCSS, Less, CoffeeScript, SASS e TypeScript.

## Distribuzione della vostra applicazione Svelte

Dal punto di vista di un server web, un'applicazione Svelte non è altro che un insieme di file HTML, CSS e JavaScript. Tutto ciò di cui ha bisogno è un server web in grado di servire file statici, il che significa che ha molte opzioni tra cui scegliere. Diamo un'occhiata a un paio di esempi.

> [!NOTE]
> La sezione seguente potrebbe essere applicata a qualsiasi sito web statico client-side che richiede un passaggio di build, non solo alle app Svelte.

### Distribuzione con Vercel

Uno dei modi più semplici per distribuire un'applicazione Svelte è utilizzare [Vercel](https://vercel.com/home). Vercel è una piattaforma cloud specificamente progettata per siti statici, che offre supporto out-of-the-box per la maggior parte degli strumenti front-end comuni, incluso Svelte.

Per distribuire la nostra app, segua questi passaggi.

1. [registrarsi per un account con Vercel](https://vercel.com/signup).
2. Navigare nella directory principale della sua app ed eseguire `npx vercel`; la prima volta che lo fa, le verrà chiesto di inserire il suo indirizzo e-mail e di seguire i passaggi nell'e-mail inviata a quell'indirizzo, per motivi di sicurezza.
3. Eseguire di nuovo `npx vercel` e le verrà chiesto di rispondere a una serie di domande, come queste:

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

4. Accetti tutte le impostazioni predefinite e andrà bene.
5. Una volta terminata la distribuzione, vada all'URL della "Produzione" nel suo browser e vedrà l'app distribuita!

Può anche [importare un progetto Svelte Git](https://vercel.com/import/svelte) in Vercel da [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/) o [Bitbucket](https://bitbucket.org/product/).

> [!NOTE]
> Può installare globalmente Vercel con `npm i -g vercel` in modo che non debba usare `npx` per eseguirlo.

### Distribuzione automatica su GitLab Pages

Per ospitare file statici ci sono diversi servizi online che consentono di distribuire automaticamente il proprio sito ogni volta che si spingono modifiche a un repository git. La maggior parte di essi prevede la creazione di una pipeline di distribuzione che viene attivata su ogni `git push` e si occupa della costruzione e della distribuzione del sito web.

Per dimostrare questo, distribuiremo la nostra app di TODOs su [GitLab Pages](https://docs.gitlab.com/user/project/pages/).

1. Prima dovrà [registrarsi su GitLab](https://gitlab.com/users/sign_up) e poi [creare un nuovo progetto](https://gitlab.com/projects/new). Gli dia un nome breve e facile come "mdn-svelte-todo". Avrà un URL remoto che punta al suo nuovo repository git su GitLab, tipo `git@gitlab.com:[your-user]/[your-project].git`.
2. Prima di iniziare a caricare contenuti nel suo repository git, è buona prassi aggiungere un file `.gitignore` per dire a git quali file escludere dal controllo del codice sorgente. Nel nostro caso diremo a git di escludere i file nella directory `node_modules` creando un file `.gitignore` nella cartella principale del suo progetto locale, con il seguente contenuto:

   ```bash
   node_modules/
   ```

3. Ora torniamo a GitLab. Dopo aver creato un nuovo repository GitLab la saluterà con un messaggio che spiega le diverse opzioni per caricare i suoi file esistenti. Segua i passaggi elencati sotto il titolo _Push an existing folder_:

   ```bash
   cd your_root_directory # Go into your project's root directory
   git init
   git remote add origin https://gitlab.com/[your-user]/mdn-svelte-todo.git
   git add .
   git commit -m "Initial commit"
   git push -u origin main
   ```

   > [!NOTE]
   > Potrebbe utilizzare [il protocollo `git`](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols#_the_git_protocol) invece di `https`, che è più veloce e la salva dal digitare il suo nome utente e la password ogni volta che accede al suo repository di origine. Per utilizzarlo dovrà [creare una coppia di chiavi SSH](https://docs.gitlab.com/user/ssh/#generate-an-ssh-key-pair). Il suo URL di origine sarà così: `git@gitlab.com:[your-user]/mdn-svelte-todo.git`.

Con queste istruzioni inizializziamo un repository git locale, quindi impostiamo come origine remota (dove invieremo il nostro codice) il nostro repository su GitLab. Successivamente commitiamo tutti i file al repository git locale e poi li spingiamo all'origine remota su GitLab.

GitLab utilizza uno strumento integrato chiamato GitLab CI/CD per costruire il suo sito e pubblicarlo sul server GitLab Pages. La sequenza di script che GitLab CI/CD esegue per completare questo compito è creata da un file chiamato `.gitlab-ci.yml`, che può creare e modificare a piacimento. Un lavoro specifico chiamato `pages` nel file di configurazione renderà GitLab consapevole che sta distribuendo un sito web per GitLab Pages.

Proviamo a farlo ora.

1. Crei un file `.gitlab-ci.yml` nella directory principale del suo progetto e gli dia il seguente contenuto:

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

   Qui stiamo dicendo a GitLab di utilizzare un'immagine con l'ultima versione di node per costruire la nostra app. Poi stiamo dichiarando un lavoro `pages`, per abilitare GitLab Pages. Ogni volta che c'è un push nel nostro repository, GitLab eseguirà `npm install` e `npm run build` per costruire la nostra applicazione. Inoltre stiamo dicendo a GitLab di distribuire i contenuti della cartella `public`. Nell'ultima riga, stiamo configurando GitLab per ristabilire la nostra app solo quando c'è un push nel nostro branch principale.

2. Poiché la nostra app sarà pubblicata in una sottodirectory (come `https://your-user.gitlab.io/mdn-svelte-todo`), dovremo rendere le referenze ai file JavaScript e CSS nel nostro file `public/index.html` relative. Per fare ciò, rimuoviamo semplicemente le barre iniziali (`/`) dagli URL `/global.css`, `/build/bundle.css` e `/build/bundle.js`, così:

   ```html
   <title>Svelte To-Do list</title>

   <link rel="icon" type="image/png" href="favicon.png" />
   <link rel="stylesheet" href="global.css" />
   <link rel="stylesheet" href="build/bundle.css" />

   <script defer src="build/bundle.js"></script>
   ```

   Faccia questo ora.

3. Ora dobbiamo semplicemente commitare e spingere le nostre modifiche su GitLab. Lo faccia eseguendo i seguenti comandi:

   ```bash
   git add public/index.html
   git add .gitlab-ci.yml
   git commit -m "Added .gitlab-ci.yml file and fixed index.html absolute paths"
   git push
   ```

Ogni volta che c'è un lavoro in esecuzione, GitLab mostrerà un'icona che mostra il progresso del lavoro. Facendo clic su di essa potrà ispezionare l'output del lavoro.

![screenshot di GitLab che mostra un commit distribuito, che aggiunge un file ci di GitLab e cambia i percorsi del bundle in relativi](01-gitlab-pages-deploy.png)

Può anche controllare il progresso dei lavori correnti e precedenti dal menu _CI / CD_ > _Jobs_ del suo progetto GitLab.

![un lavoro ci di GitLab mostrato nell'interfaccia utente di GitLab, eseguendo molti comandi](02-gitlab-pages-job.png)

Una volta che GitLab termina di costruire e pubblicare la sua app, sarà accessibile all'indirizzo `https://your-user.gitlab.io/mdn-svelte-todo/`; nel mio caso è `https://opensas.gitlab.io/mdn-svelte-todo/`. Può controllare l'URL della sua pagina nell'UI di GitLab — veda l'opzione di menu _Settings_ > _Pages_.

Con questa configurazione, ogni volta che spinge modifiche al repository GitLab, l'applicazione verrà automaticamente ricostruita e distribuita su GitLab Pages.

## Imparare di più su Svelte

In questa sezione le daremo alcune risorse e progetti da consultare per approfondire il suo apprendimento di Svelte.

### Documentazione di Svelte

Per andare oltre e imparare di più su Svelte, dovrebbe assolutamente visitare la [home page di Svelte](https://svelte.dev/). Lì troverà [molti articoli](https://svelte.dev/blog) che spiegano la filosofia di Svelte. Se non lo ha già fatto, assicuri di seguire [il tutorial interattivo di Svelte](https://learn.svelte.dev/tutorial/welcome-to-svelte). Abbiamo già coperto la maggior parte del suo contenuto, quindi non richiederà molto tempo per completarlo — lo consideri una pratica!

Può anche consultare i [documenti API di Svelte](https://svelte.dev/docs) e gli [esempi disponibili](https://svelte.dev/examples/hello-world).

Per comprendere le motivazioni dietro Svelte, dovrebbe leggere la presentazione di [Rich Harris](https://x.com/Rich_Harris) ["Rethinking reactivity"](https://www.youtube.com/watch?v=AdNJ3fydeao&t=47s) su YouTube. Lui è il creatore di Svelte, quindi ha un paio di cose da dire al riguardo. Ha anche le diapositive interattive disponibili qui, che sono, non sorprendentemente, costruite con Svelte. Se gli è piaciuto, godrà anche della presentazione ["The Return of 'Write Less, Do More'"](https://www.youtube.com/watch?v=BzX4aTRPzno), che Rich Harris ha tenuto allo [JSCAMP 2019](https://jscamp.tech/2019/).

### Progetti correlati

Ci sono altri progetti correlati a Svelte che vale la pena controllare:

- [Sapper](https://sapper.svelte.dev/): Un framework applicativo alimentato da Svelte che fornisce rendering lato server (SSR), suddivisione del codice, routing basato su file e supporto offline, e altro ancora. Lo consideri come [Next.js](https://nextjs.org/) per Svelte. Se sta pianificando di sviluppare un'applicazione web abbastanza complessa dovrebbe assolutamente dare un'occhiata a questo progetto.
- [Svelte Native](https://svelte.nativescript.org/): Un framework per applicazioni mobili alimentato da Svelte. Lo consideri come [React Native](https://reactnative.dev/) per Svelte.
- [Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode): Il plugin ufficiale supportato per VS Code per lavorare con file `.svelte`, che abbiamo esaminato nel nostro [articolo su TypeScript](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript).

### Altre risorse di apprendimento

- C'è [un corso completo su Svelte e Sapper](https://frontendmasters.com/courses/svelte/) di Rich Harris, disponibile su Frontend Masters.
- Anche se Svelte è un progetto relativamente giovane ci sono molti tutorial e [corsi](https://www.udemy.com/topic/svelte-framework/?sort=popularity) disponibili sul web, quindi è difficile fare una raccomandazione.
- Tuttavia, [Svelte.js — La guida completa](https://www.udemy.com/course/sveltejs-the-complete-guide/) di [Academind](https://academind.com/) è un'opzione molto popolare con ottime valutazioni.
- [Il Manuale Svelte](https://www.freecodecamp.org/news/the-svelte-handbook/), di [Flavio Copes](https://flaviocopes.com/), è anche un utile riferimento per apprendere i principali concetti di Svelte.
- Se preferisce leggere libri, c'è [Svelte e Sapper in Azione](https://www.manning.com/books/svelte-and-sapper-in-action) di [Mark Volkman](https://x.com/mark_volkmann), pubblicato nell'ottobre 2020, che [può visualizzare online](https://livebook.manning.com/book/svelte-and-sapper-in-action/welcome) gratuitamente.
- Se vuole approfondire e capire il funzionamento interno del compilatore di Svelte dovrebbe controllare i post del blog di [Tan Li Hau](https://x.com/lihautan) [_Compile Svelte in your head_](https://lihautan.com/compile-svelte-in-your-head).

### Interagire con la comunità

Ci sono diversi modi per ottenere supporto e interagire con la comunità Svelte:

- [svelte.dev/chat](https://discord.com/invite/yy75DKs): Server Discord di Svelte.
- [@sveltejs](https://x.com/sveltejs): L'account Twitter ufficiale.
- [@sveltesociety](https://x.com/sveltesociety): Account Twitter della comunità Svelte.
- [Svelte Recipes](https://github.com/svelte-society/recipes-mvp#recipes-mvp): Repository guidato dalla comunità di ricette, consigli e migliori pratiche per risolvere problemi comuni.
- [Domande su Svelte su Stack Overflow](https://stackoverflow.com/questions/tagged/svelte): Domande con il tag `svelte` su SO.
- [Comunità Svelte su reddit](https://www.reddit.com/r/sveltejs/): Discussione della comunità Svelte e sito di valutazione dei contenuti su reddit.
- [Comunità Svelte DEV](https://dev.to/t/svelte): Una raccolta di articoli e tutorial tecnici relativi a Svelte dalla comunità DEV.to.

## Finito

Congratulazioni! Ha completato il tutorial di Svelte. Negli articoli precedenti siamo passati da zero conoscenza di Svelte a costruire e distribuire un'applicazione completa.

- Abbiamo imparato la filosofia di Svelte e cosa lo distingue dagli altri framework front-end.
- Abbiamo visto come aggiungere comportamento dinamico al nostro sito web, come organizzare la nostra app in componenti e diversi modi per condividere informazioni tra loro.
- Abbiamo sfruttato il sistema di reattività di Svelte e abbiamo imparato come evitare errori comuni.
- Abbiamo anche visto alcuni concetti avanzati e tecniche per interagire con elementi DOM e per estendere programmaticamente le capacità degli elementi HTML utilizzando la direttiva `use`.
- Poi abbiamo visto come usare i negozi per lavorare con un repository centrale di dati e abbiamo creato il nostro negozio personalizzato per persistere i dati della nostra applicazione su Web Storage.
- Abbiamo anche dato uno sguardo al supporto TypeScript di Svelte.

In questo articolo abbiamo appreso un paio di opzioni senza complicazioni per distribuire la nostra app in produzione e abbiamo visto come configurare una pipeline di base per distribuire la nostra app su GitLab ad ogni commit. Poi le abbiamo fornito una lista di risorse Svelte per andare oltre con il suo apprendimento Svelte.

Congratulazioni! Dopo aver completato questa serie di tutorial dovrebbe avere una solida base da cui iniziare a sviluppare applicazioni web professionali con Svelte.

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}
