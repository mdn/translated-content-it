---
title: Distribuire la nostra app
slug: Learn_web_development/Extensions/Client-side_tools/Deployment
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenu("Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}

Nell'articolo finale della nostra serie, prendiamo l'esempio di toolchain che abbiamo costruito nell'articolo precedente e lo espandiamo in modo da poter distribuire la nostra app di esempio. Carichiamo il codice su GitHub, lo distribuiamo utilizzando GitHub Pages e vi mostriamo anche come aggiungere un semplice test nel processo.

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
        Completare il nostro studio di caso sulla toolchain completa, concentrandoci sulla
        distribuzione dell'app.
      </td>
    </tr>
  </tbody>
</table>

## Post sviluppo

C'è potenzialmente una vasta gamma di problemi da risolvere in questa fase del ciclo di vita del progetto. Pertanto, è importante creare una toolchain che gestisca questi problemi in modo da richiedere il minor intervento manuale possibile.

Ecco solo alcune cose da considerare per questo particolare progetto:

- Generazione di una build di produzione: assicurarsi che i file siano minimizzati, suddivisi in blocchi, che sia applicato il tree-shaking e che le versioni siano "cache busted".
- Esecuzione dei test: questi possono variare dal "questo codice è formattato correttamente?" al "questo fa quello che mi aspetto?", assicurandosi che i test falliti impediscano la distribuzione.
- Effettivamente distribuire il codice aggiornato a un URL live: o potenzialmente a un URL di staging in modo che possa essere esaminato in primo luogo.

> [!NOTE]
> Cache busting è un termine nuovo che non abbiamo ancora incontrato nel modulo. Questa è la strategia di interrompere il meccanismo di caching del browser, il che costringe il browser a scaricare una nuova copia del suo codice. Vite (e in realtà molti altri strumenti) genererà nomi di file unici per ogni nuova build. Questo nome di file unico "rompe" la cache del browser, assicurandosi quindi che il browser scarichi il codice aggiornato ogni volta che viene eseguito un aggiornamento del codice distribuito.

I compiti sopra elencati si suddividono ulteriormente in altri compiti; si noti che la maggior parte dei team di sviluppo web avrà i propri termini e processi per almeno una parte della fase post-sviluppo.

Per questo progetto, useremo l'offerta gratuita di hosting statico di [GitHub Pages](https://pages.github.com/) per ospitare il nostro progetto. Non solo serve il nostro sito web su Internet, ma ci fornisce anche un URL al nostro sito web. È fantastico — molti siti web di esempio di MDN sono ospitati su GitHub Pages.

La distribuzione su hosting tende ad essere alla fine del ciclo di vita del progetto, ma con servizi come GitHub Pages che riducono il costo delle distribuzioni (sia in termini finanziari che di tempo richiesto per effettivamente distribuire), è possibile distribuire durante lo sviluppo per condividere il lavoro in corso o avere un pre-release per qualche altro scopo.

GitHub fornisce un flusso di lavoro fluido per trasformare nuovo codice in un sito web attivo:

- Lei carica il suo codice su GitHub.
- Definisce un [GitHub Action](https://docs.github.com/en/actions) che viene attivato quando c'è un nuovo push al branch principale, che costruisce il codice e lo posiziona in una posizione specifica.
- GitHub Pages quindi serve il codice a un URL specifico.

Sono esattamente questi tipi di servizi connessi che La incoraggeremmo a cercare quando decide sulla sua toolchain di costruzione. Possiamo commettere il nostro codice e caricarlo su GitHub e il codice aggiornato attiverà automaticamente l'intera routine di build. Se tutto va bene, otteniamo una modifica attiva distribuita automaticamente. L'unica azione che dobbiamo compiere è quel "push" iniziale.

Tuttavia, dobbiamo configurare questi passi, e ora vedremo come fare.

## Il processo di build

Ancora una volta, poiché stiamo usando Vite per lo sviluppo, l'opzione di build è estremamente semplice da aggiungere. Come abbiamo visto in precedenza, abbiamo già un script personalizzato `npm run build` che permetterà a Vite di costruire tutto pronto per la produzione anziché solo eseguirlo per scopi di sviluppo e test. Questo include la {{Glossary("Minification", "minificazione")}} e il {{Glossary("Tree_shaking", "tree-shaking")}} del codice e la gestione dei nomi di file con cache busting.

È una buona pratica definire sempre uno script `build` nel suo progetto, così possiamo contare su `npm run build` per eseguire sempre il completo step di build, senza dover ricordare gli argomenti specifici del comando di build per ogni progetto.

Il codice di produzione appena creato viene posizionato in una nuova directory chiamata `dist`, che contiene _tutti_ i file necessari per eseguire il sito web, pronti per essere caricati su un server.

Tuttavia, fare questo passo manualmente non è il nostro obiettivo finale — ciò che vogliamo è che la build avvenga automaticamente e che il risultato della directory `dist` sia distribuito live sul nostro sito web.

## Commettere i cambiamenti su GitHub

Questa sezione La porterà a memorizzare il suo codice in un repository git, ma è ben lontano dal essere un tutorial su git. Ci sono molti ottimi tutorial e libri disponibili, e la nostra pagina [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control) è un buon punto di partenza.

Abbiamo inizializzato la nostra directory di lavoro come una directory di lavoro git in precedenza. Un modo rapido per verificare ciò è eseguire il seguente comando:

```bash
git status
```

Dovrebbe ottenere un report di stato sui file che vengono tracciati, quali file sono in fase di staging, e così via — tutti termini che fanno parte della grammatica di git. Se si riceve l'errore `fatal: not a git repository`, allora la directory di lavoro non è una directory di lavoro git e sarà necessario inizializzare git usando `git init`.

Ora abbiamo tre compiti davanti a noi:

- Aggiungere eventuali cambiamenti che abbiamo apportato alla fase di staging (un nome speciale per il luogo da cui git commetterà i file).
- Commettere i cambiamenti al repository.
- Caricare le modifiche su GitHub.

1. Per aggiungere modifiche, eseguire il seguente comando:

   ```bash
   git add .
   ```

   Si noti il punto alla fine, significa "tutto in questa directory". Il comando `git add .` è un approccio un po' drastico — aggiungerà tutte le modifiche locali su cui ha lavorato in un colpo solo. Se desidera un controllo più fine su ciò che aggiunge, usi `git add -p` per un processo interattivo, o aggiunga singoli file usando `git add path/to/file`.

2. Ora che tutto il codice è in fase di staging, possiamo commettere; esegua il seguente comando:

   ```bash
   git commit -m 'committing initial code'
   ```

   > [!NOTE]
   > Anche se è libera di scrivere qualsiasi cosa desideri nel messaggio di commit, ci sono alcuni consigli utili sul web sui buoni messaggi di commit. Mantienili brevi, concisi e descrittivi, in modo che descrivano chiaramente cosa fa la modifica.

3. Infine, il codice deve essere caricato sul suo repository ospitato su GitHub. Facciamolo ora.

   Su GitHub, visiti <https://github.com/new> e crei il proprio repository per ospitare questo codice.

4. Dia al suo repository un nome breve e memorabile, senza spazi (usare i trattini per separare le parole), e una descrizione, quindi faccia clic su _Create repository_ in fondo alla pagina.

   Dovrebbe ora avere un URL "remoto" che punta al suo nuovo repo GitHub.

   ![Screenshot di GitHub che mostra gli URL remoti che si possono usare per distribuire il codice su un repository GitHub](github-quick-setup.png)

5. Questa posizione remota deve essere aggiunta al nostro repository git locale prima di poterlo caricare, altrimenti non sarà in grado di trovarlo. Sarà necessario eseguire un comando con la seguente struttura (usare l'opzione HTTPS fornita per ora — soprattutto se è nuova su GitHub — non l'opzione SSH):

   ```bash
   git remote add origin https://github.com/your-name/repo-name.git
   ```

   Quindi, se il suo URL remoto fosse `https://github.com/remy/super-website.git`, come nello screenshot sopra, il suo comando sarebbe

   ```bash
   git remote add origin https://github.com/remy/super-website.git
   ```

   Cambi l'URL nel suo repository e lo esegua ora.

   > [!NOTE]
   > Dopo aver scelto il nome del repository, si assicuri che l'opzione `base` nel suo `vite.config.js` rifletta questo nome, come menzionato nel [capitolo precedente](/it/docs/Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain#javascript_transformation). Altrimenti, le risorse JavaScript e CSS non saranno collegate correttamente.

6. Ora siamo pronti per caricare il nostro codice su GitHub; esegua il seguente comando ora:

   ```bash
   git push origin main
   ```

   A questo punto, Le verrà chiesto di inserire un nome utente e una password prima che Git permetta di inviare il push. Questo perché abbiamo usato l'opzione HTTPS anziché l'opzione SSH, come visto nello screenshot precedente. Per questo, ha bisogno del suo nome utente GitHub e poi — se non ha attivato l'autenticazione a due fattori (2FA) — la sua password GitHub. Le consiglieremmo sempre di usare il 2FA se possibile, ma tenga presente che se lo fa, avrà anche bisogno di un "token di accesso personale". Le pagine di aiuto di GitHub hanno una [eccellente e semplice guida su come ottenerne uno](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

> [!NOTE]
> Se è interessata a utilizzare l'opzione SSH, evitando così la necessità di inserire il nome utente e la password ogni volta che carica su GitHub, [questo tutorial le spiega come fare](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

Questo comando finale istruisce git a caricare il codice nella posizione "remota" che abbiamo chiamato `origin` (cioè il repository ospitato su github.com — potremmo chiamarlo come vogliamo) usando il branch `main`. Non abbiamo incontrato affatto rami, ma il ramo "main" è il posto predefinito per il nostro lavoro ed è quello su cui git inizia. Quando definiamo l'azione attivata per costruire il sito web, la faremo anche guardare per eventuali modifiche sul ramo "main".

> [!NOTE]
> Fino a ottobre 2020 il ramo predefinito su GitHub era `master`, che per vari motivi sociali è stato cambiato in `main`. Dovrebbe essere consapevole che questo ramo predefinito precedente può apparire in vari progetti che incontra, ma consiglieremmo di usare `main` per i suoi progetti.

Quindi, con il nostro progetto commesso in git e caricato sul nostro repository GitHub, il passo successivo nella toolchain è definire un'azione di build in modo che il nostro progetto possa essere distribuito live sul web!

## Utilizzo di GitHub Actions per la distribuzione

GitHub Actions, come la configurazione di ESLint, è un'altra profonda tana del coniglio in cui addentrarsi. Non è facile farlo giusto al primo tentativo, ma per compiti popolari come "costruire un sito web statico e distribuirlo su GitHub Pages", ci sono molti esempi da copiare e incollare. Può seguire le istruzioni in [Publishing with a custom GitHub Actions workflow](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow). Può controllare il nostro [file di GitHub Action](https://github.com/mdn/client-toolchain-example/blob/main/.github/workflows/github-pages.yml) per un esempio funzionante. (Il nome del file non ha importanza.)

Dopo aver commesso questo file sul ramo principale, dovrebbe vedere un piccolo segno di spunta verde accanto al titolo del commit:

![Screenshot di GitHub che mostra un segno di spunta verde accanto al titolo di un commit](build-action-pass.png)

Se vede un punto giallo, significa che l'azione è in esecuzione e se vede una croce rossa, significa che l'azione è fallita. Faccia clic sull'icona e potrà vedere lo stato e i log della sua azione di build personalizzata (chiamata "Deploy build" nel nostro caso).

Dopo aver atteso per qualche altro minuto, può visitare l'URL di GitHub Pages per vedere il suo sito web live sul web. Il link assomiglia a `https://<your-name>.github.io/<repo-name>`. Per il nostro esempio, è <https://mdn.github.io/client-toolchain-example/>.

Ora per un ultimo collegamento nella nostra toolchain: un test per garantire che il nostro codice funzioni.

## Testing

Il testing stesso è un vasto argomento, anche nel regno dello sviluppo front-end. Le mostrerò come aggiungere un test iniziale al suo progetto e come utilizzare il test per prevenire o consentire la distribuzione del progetto.

Quando si affrontano i test ci sono molti modi per affrontare il problema:

- Test end-to-end, che comporta il clic del visitatore su una cosa e qualcos'altro che accade.
- Test di integrazione, che fondamentalmente chiede "un blocco di codice funziona ancora quando connesso a un altro blocco?"
- Unit testing, dove piccole e specifiche parti funzionali sono testate per vedere se fanno ciò che dovrebbero fare.
- [Molti altri tipi](https://en.wikipedia.org/wiki/Functional_testing). Veda anche il nostro [modulo di test cross browser](/it/docs/Learn_web_development/Extensions/Testing) per una serie di informazioni utili sui test.

Ricordi anche che i test non sono limitati a JavaScript; i test possono essere eseguiti contro il DOM renderizzato, le interazioni con l'utente, il CSS e persino come appare una pagina.

Tuttavia, per questo progetto creeremo un piccolo test che verificherà se i dati dell'API GitHub sono nel formato corretto. In caso contrario, il test fallirà e impedirà al progetto di andare live. Fare altro andrebbe oltre lo scopo di questo modulo — il testing è un argomento enorme che richiede davvero un modulo separato. Speriamo che questa sezione almeno La renda consapevole della necessità di effettuare test, e pianti il seme che La ispiri a imparare di più.

Il test stesso non è ciò che è importante. Ciò che è importante è come viene gestito il fallimento o il successo. Poiché stiamo già scrivendo un'azione di build personalizzata, possiamo aggiungere uno step prima della build che esegua il test. Se il test fallisce, la build fallirà e la distribuzione non avrà luogo.

La buona notizia è che: poiché stiamo usando Vite, Vite offre già un buon strumento integrato per i test: [Vitest](https://vitest.dev/guide/).

Iniziamo.

1. Installi Vitest:

   ```bash
   npm install --save-dev vitest
   ```

2. Nel suo package.json, trovi il suo elemento `scripts`, e lo aggiorni in modo che contenga i seguenti comandi di test e build:

   ```json
   "scripts": {
     // …
     "test": "vitest"
   }
   ```

   > [!NOTE]
   > Ecco la parte buona dell'usare Vite insieme a Vitest: se si utilizzano altri framework di testing, è necessario aggiungere un'altra configurazione che descriva come i file di test debbano essere trasformati, ma Vitest userà automaticamente la configurazione di Vite.

3. Ora chiaramente dobbiamo aggiungere il test al nostro codice. Normalmente, se si sta testando la funzionalità di un file, ad esempio `App.jsx`, si aggiungerebbe un file chiamato `App.test.jsx` accanto ad esso. In questo caso, stiamo solo testando i dati, quindi creiamo un'altra directory per contenere i nostri test. Può aprire il repository di esempio che ha scaricato nel capitolo precedente e copiare la cartella `tests`.

4. Ora per eseguire manualmente il test, dal terminale possiamo eseguire:

   ```bash
   npm run test
   ```

   Dovrebbe vedere un output come questo:

   ```plain
   > client-toolchain-example@1.0.0 test
   > vitest


   DEV  v1.6.0 /Users/joshcena/Desktop/work/Tech/projects/mdn/client-toolchain-example

   ✓ tests/api.test.js (1) 896ms
     ✓ GitHub API returns the right response 896ms

   Test Files  1 passed (1)
        Tests  1 passed (1)
     Start at  23:12:25
     Duration  1.03s (transform 15ms, setup 0ms, collect 5ms, tests 896ms, environment 0ms, prepare 38ms)


   PASS  Waiting for file changes...
         press h to show help, press q to quit
   ```

   Questo significa che il test è passato. Come Vite, monitorerà i cambiamenti e rieseguirà i test quando si salva un file. Possiamo terminare premendo <kbd>q</kbd>.

5. Dobbiamo ancora collegare il test alla nostra azione di build, in modo che blocchi la build se il test fallisce. Apra il file `.github/workflows/github-pages.yml` (o qualsiasi nome di file ha dato alla sua azione di build) e aggiunga il seguente step, proprio prima dello step che esegue `npm run build`:

   ```yaml
   - name: Install deps
     run: npm ci

   # Add this
   - name: Run tests
     run: npm run test

   - name: Build
     run: npm run build
   ```

   Questo eseguirà il test prima dello step di build. Se il test fallisce, la build fallirà e la distribuzione non avrà luogo.

6. Ora carichi il nuovo codice su GitHub, utilizzando comandi simili a quelli usati in precedenza:

   ```bash
   git add .
   git commit -m 'adding test'
   git push origin main
   ```

   In alcuni casi potrebbe voler testare il risultato del codice costruito (dato che questo non è proprio il codice originale che abbiamo scritto), quindi potrebbe essere necessario eseguire il test dopo il comando di build. Sarà necessario considerare tutti questi aspetti individuali mentre lavora sui propri progetti.

Infine, un minuto o due dopo aver caricato, GitHub Pages distribuirà l'aggiornamento del progetto. Ma solo se passa il test che è stato introdotto.

## Riepilogo

Questo è tutto per il nostro caso di studio di esempio, e per il modulo! Speriamo che l'abbia trovato utile. Anche se c'è ancora molta strada da fare prima che possa considerarsi un mago degli strumenti client-side, speriamo che questo modulo Le abbia dato quel primo passo importante verso la comprensione degli strumenti client-side, e la fiducia di imparare di più e provare nuove cose.

Riepiloghiamo tutte le parti della toolchain:

- La qualità e manutenzione del codice sono effettuate da ESLint e Prettier. Questi strumenti sono aggiunti come `devDependencies` al progetto tramite `npm install --dev eslint prettier eslint-plugin-react ...` (il plugin ESLint è necessario perché questo particolare progetto utilizza React).
- Ci sono due file di configurazione che gli strumenti di qualità del codice leggono: `eslint.config.js` e `.prettierrc`.
- Durante lo sviluppo, continuiamo ad aggiungere dipendenze utilizzando npm. Il server di sviluppo Vite è in esecuzione in background per monitorare i cambiamenti e costruire automaticamente la nostra sorgente.
- La distribuzione è gestita caricando le nostre modifiche su GitHub (sul branch "main"), che attiva una build e distribuisce usando GitHub Actions per pubblicare il progetto. Per nostra istanza questo URL è <https://mdn.github.io/client-toolchain-example/>; avrà il suo proprio URL unico.
- Abbiamo anche un semplice test che blocca la costruzione e la distribuzione del sito se il feed API di GitHub non ci dà il formato dati corretto.

Per coloro di voi che cercano una sfida, consideri se può ottimizzare qualche parte di questa toolchain. Alcune domande da porsi:

- Possiamo estrarre solo le funzionalità di plotly.js di cui abbiamo bisogno? Questo ridurrà la dimensione del pacchetto JavaScript.
- Forse vuole aggiungere altri strumenti, come TypeScript per il controllo dei tipi, o stylelint per il linting del CSS?
- React potrebbe essere sostituito con [qualcosa di più piccolo](https://preactjs.com/)?
- Potrebbe aggiungere più test per evitare che una build errata venga distribuita, come [auditing delle prestazioni](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring)?
- Potrebbe configurare una notifica per farle sapere quando un nuovo deploy ha avuto successo o è fallito?

{{PreviousMenu("Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}
