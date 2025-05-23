---
title: Distribuire la nostra app
slug: Learn_web_development/Extensions/Client-side_tools/Deployment
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenu("Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}

Nell'articolo finale della nostra serie, prendiamo la toolchain di esempio che abbiamo costruito nell'articolo precedente e la espandiamo per distribuire la nostra app di esempio. Carichiamo il codice su GitHub, lo distribuiamo utilizzando GitHub Pages e mostriamo anche come aggiungere un semplice test nel processo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Completare lo studio di caso sulla nostra toolchain completa, con un focus sulla distribuzione dell'app.
      </td>
    </tr>
  </tbody>
</table>

## Post sviluppo

C'è un potenziale ampio ventaglio di problemi da risolvere in questa sezione del ciclo di vita del progetto. Pertanto, è importante creare una toolchain che gestisca questi problemi in modo da richiedere il meno possibile intervento manuale.

Ecco alcune cose da considerare per questo progetto specifico:

- Generare una build di produzione: Assicurarsi che i file siano minimizzati, suddivisi, con alberi scossi e che le versioni siano "cache bustate".
- Esecuzione dei test: Questi possono variare da "questo codice è formattato correttamente?" a "questa cosa funziona come mi aspetto?", assicurandosi che i test falliti impediscano la distribuzione.
- Effettivamente distribuire il codice aggiornato a un URL live: O potenzialmente a un URL di staging per poterlo rivedere prima.

> [!NOTE]
> Il "cache busting" è un nuovo termine che non abbiamo incontrato prima nel modulo. È la strategia di rompere il meccanismo di caching del browser, che forza il browser a scaricare una nuova copia del tuo codice. Vite (e in effetti molti altri strumenti) genererà nomi di file unici per ogni nuova build. Questo nome file unico " rompe" la cache del browser, assicurandosi così che il browser scarichi il codice aggiornato ogni volta che viene fatto un aggiornamento al codice distribuito.

Questi compiti si suddividono ulteriormente in altri compiti; si noti che la maggior parte dei team di sviluppo web avrà i propri termini e processi per almeno una parte della fase post-sviluppo.

Per questo progetto, useremo l'offerta di hosting statico gratuito di [GitHub Pages](https://pages.github.com/) per ospitare il nostro progetto. Non solo serve il nostro sito web su Internet, ma ci fornisce anche un URL per il nostro sito. È fantastico — molti siti di esempio di MDN sono ospitati su GitHub Pages.

Distribuire su hosting tende ad essere alla fine del ciclo di vita del progetto, ma con servizi come GitHub Pages che riducono il costo delle distribuzioni (sia in termini finanziari sia in termini di tempo necessario per distribuire effettivamente), è possibile distribuire durante lo sviluppo per condividere il lavoro in corso o per avere una pre-release per qualche altro scopo.

GitHub fornisce un flusso di lavoro senza soluzione di continuità per trasformare il nuovo codice in un sito web live:

- Pubblichi il tuo codice su GitHub.
- Definisci un [GitHub Action](https://docs.github.com/en/actions) che viene attivato quando c'è un nuovo push al ramo principale, che costruisce il codice e lo posiziona in una posizione specifica.
- GitHub Pages quindi serve il codice a un URL specifico.

Sono esattamente questi tipi di servizi connessi che ti incoraggeremmo a cercare quando decidi sulla tua toolchain di build. Possiamo impegnare il nostro codice e caricarlo su GitHub e il codice aggiornato attiverà automaticamente l'intera routine di build. Se tutto va bene, ci viene distribuito automaticamente un cambiamento live. L'unica azione che dobbiamo eseguire è quel push iniziale.

Tuttavia, dobbiamo impostare questi passaggi, e ora ne parleremo.

## Il processo di build

Ancora, poiché stiamo usando Vite per lo sviluppo, l'opzione di build è estremamente semplice da aggiungere. Come abbiamo visto in precedenza, abbiamo già uno script personalizzato `npm run build` che permetterà a Vite di costruire tutto pronto per la produzione invece di farlo solo per lo sviluppo e scopi di test. Questo include la {{Glossary("Minification", "minificazione")}} e il {{Glossary("Tree_shaking", "tree-shaking")}} del codice, e il cache-busting sui nomi dei file.

È una buona pratica definire sempre uno script di `build` nel tuo progetto, così possiamo poi fare affidamento su `npm run build` per eseguire sempre il passaggio completo della build, senza dover ricordare gli argomenti specifici del comando di build per ciascun progetto.

Il codice di produzione appena creato viene posizionato in una nuova directory chiamata `dist`, che contiene _tutti_ i file richiesti per eseguire il sito web, pronto per essere caricato su un server.

Tuttavia, fare questo passaggio manualmente non è il nostro obiettivo finale — quello che vogliamo è che la build avvenga automaticamente e che il risultato della directory `dist` venga distribuito live sul nostro sito web.

## Commettere le modifiche su GitHub

Questa sezione ti porterà fino al punto di memorizzare il tuo codice in un repository git, ma è lontano dall'essere un tutorial su git. Ci sono molti ottimi tutorial e libri disponibili, e la nostra pagina [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control) è un buon punto di partenza.

Abbiamo inizializzato la nostra directory di lavoro come una directory di lavoro git in precedenza. Un modo rapido per verificarlo è eseguire il seguente comando:

```bash
git status
```

Dovresti ottenere un rapporto di stato su quali file vengono tracciati, quali file sono in staging e così via — tutti termini che fanno parte della grammatica di git. Se ricevi l'errore `fatal: not a git repository`, allora la directory di lavoro non è una directory di lavoro git e dovrai inizializzare git usando `git init`.

Ora abbiamo tre compiti davanti a noi:

- Aggiungere tutte le modifiche che abbiamo fatto allo stage (un nome speciale per il posto da cui git impegnerà i file).
- Commettere le modifiche al repository.
- Caricare le modifiche su GitHub.

1. Per aggiungere le modifiche, esegui il seguente comando:

   ```bash
   git add .
   ```

   Nota il punto alla fine, significa "tutto in questa directory". Il comando `git add .` è un po' un approccio pesante — aggiungerà tutte le modifiche locali su cui hai lavorato in un colpo solo. Se vuoi avere un controllo più fine su cosa aggiungere, usa `git add -p` per un processo interattivo, o aggiungi singoli file usando `git add path/to/file`.

2. Ora che tutto il codice è in staging, possiamo commettere; esegui il seguente comando:

   ```bash
   git commit -m 'committing initial code'
   ```

   > [!NOTE]
   > Anche se sei libero di scrivere quello che vuoi nel messaggio del commit, ci sono alcuni consigli utili sul web sui messaggi di commit buoni. Mantienili brevi, concisi e descrittivi, in modo che descrivano chiaramente cosa fa la modifica.

3. Infine, il codice deve essere caricato nel tuo repository ospitato su GitHub. Facciamolo ora.

   Su GitHub, visita <https://github.com/new> e crea il tuo repository per ospitare questo codice.

4. Dai al tuo repository un nome breve e memorabile, senza spazi (usa trattini per separare le parole), e una descrizione, quindi fai clic su _Create repository_ in fondo alla pagina.

   Dovresti ora avere un URL "remoto" che punta al tuo nuovo repo su GitHub.

   ![Schermata di GitHub che mostra gli URL remoti che puoi usare per distribuire codice a un repo GitHub](github-quick-setup.png)

5. Questa posizione remota deve essere aggiunta al nostro repository git locale prima di poterlo caricare lì, altrimenti non sarà in grado di trovarlo. Dovrai eseguire un comando con la seguente struttura (usa l'opzione HTTPS fornita per ora — soprattutto se sei nuovo su GitHub — non l'opzione SSH):

   ```bash
   git remote add origin https://github.com/your-name/repo-name.git
   ```

   Quindi, se il tuo URL remoto fosse `https://github.com/remy/super-website.git`, come nella schermata sopra, il tuo comando sarebbe

   ```bash
   git remote add origin https://github.com/remy/super-website.git
   ```

   Cambia l'URL con il tuo repository, ed eseguilo ora.

   > [!NOTE]
   > Dopo aver scelto il nome del tuo repository, assicurati che l'opzione `base` nel tuo `vite.config.js` rifletta questo nome, come menzionato nel [capitolo precedente](/it/docs/Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain#javascript_transformation). Altrimenti, le risorse JavaScript e CSS non saranno collegate correttamente.

6. Ora siamo pronti per caricare il nostro codice su GitHub; esegui il comando seguente ora:

   ```bash
   git push origin main
   ```

   A questo punto, ti verrà richiesto di inserire un nome utente e una password prima che Git permetta di inviare il push. Questo perché abbiamo utilizzato l'opzione HTTPS invece dell'opzione SSH, come visto nella schermata precedente. Per questo, hai bisogno del tuo nome utente GitHub e poi — se non hai attivato l'autenticazione a due fattori (2FA) — la tua password GitHub. Ti incoraggeremmo sempre a utilizzare 2FA se possibile, ma tieni presente che se lo fai, avrai anche bisogno di usare un "token di accesso personale". Le pagine di aiuto di GitHub hanno una [eccellente e semplice procedura dettagliata su come ottenerne uno](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

> [!NOTE]
> Se sei interessato a utilizzare l'opzione SSH, evitando così la necessità di inserire il tuo nome utente e password ogni volta che esegui un push a GitHub, [questo tutorial ti spiega come fare](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

Questo comando finale istruisce git a caricare il codice nella posizione "remota" che abbiamo chiamato `origin` (quello è il repository ospitato su github.com — potremmo averlo chiamato come vogliamo) utilizzando il ramo `main`. Non abbiamo incontrato affatto i rami, ma il ramo "main" è il posto predefinito per il nostro lavoro ed è su cosa git inizia. Quando definiamo l'azione attivata per costruire il sito web, la lasceremo anche guardare per modifiche sul ramo "main".

> [!NOTE]
> Fino a ottobre 2020 il ramo predefinito su GitHub era `master`, che per vari motivi sociali è stato cambiato in `main`. Dovresti essere consapevole che questo vecchio ramo predefinito potrebbe comparire in vari progetti che incontri, ma suggeriremmo di usare `main` per i tuoi progetti.

Quindi, con il nostro progetto commesso in git e caricato nel nostro repository GitHub, il prossimo passo nella toolchain è definire un'azione di build in modo che il nostro progetto possa essere distribuito live sul web!

## Utilizzare GitHub Actions per la distribuzione

GitHub Actions, come la configurazione di ESLint, è un altro argomento profondo da esplorare. Non è facile farlo bene al primo tentativo, ma per attività popolari come "costruire un sito web statico e distribuirlo su GitHub Pages", ci sono molti esempi da copiare e incollare. Puoi seguire le istruzioni in [Publishing with a custom GitHub Actions workflow](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow). Puoi controllare [il nostro file GitHub Action](https://github.com/mdn/client-toolchain-example/blob/main/.github/workflows/github-pages.yml) per un esempio funzionante. (Il nome del file non importa.)

Dopo che hai commesso questo file sul ramo principale, dovresti vedere un piccolo segno di spunta verde accanto al titolo del commit:

![Schermata di GitHub che mostra un segno di spunta verde accanto al titolo di un commit](build-action-pass.png)

Se vedi un punto giallo, significa che l'azione è in esecuzione, e se vedi una croce rossa, significa che l'azione è fallita. Clicca sull'icona e puoi vedere lo stato e i log della tua azione di build (denominata "Deploy build" nel nostro caso).

Dopo aver aspettato qualche minuto in più, puoi visitare il tuo URL di GitHub Pages per vedere il tuo sito web live sul web. Il link appare come `https://<your-name>.github.io/<repo-name>`. Per il nostro esempio, è su <https://mdn.github.io/client-toolchain-example/>.

Ora per un ultimo collegamento nella nostra toolchain: un test per garantire che il nostro codice funzioni.

## Test

Il collaudo in sé è un vasto argomento, anche nel regno dello sviluppo front-end. Ti mostrerò come aggiungere un test iniziale al tuo progetto e come utilizzare il test per impedire o consentire che la distribuzione del progetto avvenga.

Quando si affrontano i test, ci sono molti modi per approcciare il problema:

- Test end-to-end, che coinvolgono il tuo visitatore che clicca su una cosa e qualche altra cosa che accade.
- Test di integrazione, che fondamentalmente dice "un blocco di codice funziona ancora quando collegato a un altro blocco?"
- Test unitari, dove piccoli e specifici pezzi di funzionalità vengono testati per vedere se fanno ciò che dovrebbero fare.
- [E molti altri tipi](https://en.wikipedia.org/wiki/Functional_testing). Vedi anche il nostro [modulo di collaudo cross-browser](/it/docs/Learn_web_development/Extensions/Testing) per tante informazioni utili sul collaudo.

Ricorda anche che i test non sono limitati a JavaScript; i test possono essere eseguiti contro il DOM renderizzato, le interazioni con l'utente, il CSS e persino l'aspetto di una pagina.

Tuttavia, per questo progetto creeremo un piccolo test che verificherà se i dati delle API di GitHub sono nel formato corretto. Se non lo sono, il test fallirà e impedirà al progetto di diventare live. Fare qualsiasi altra cosa sarebbe oltre lo scopo di questo modulo — il collaudo è un argomento enorme che richiede realmente un proprio modulo separato. Speriamo che questa sezione ti renda almeno consapevole della necessità di test, e pianti il seme che ti ispira a saperne di più.

Il test in sé non è importante. Ciò che è importante è come viene gestito il fallimento o il successo. Poiché stiamo già scrivendo un'azione di build personalizzata, possiamo aggiungere un passaggio prima della build che esegue il test. Se il test fallisce, la build fallirà, e la distribuzione non avverrà.

La buona notizia è: poiché stiamo usando Vite, Vite offre già un buono strumento integrato per i test: [Vitest](https://vitest.dev/guide/).

Iniziamo.

1. Installa Vitest:

   ```bash
   npm install --save-dev vitest
   ```

2. Nel tuo package.json, trova il tuo membro `scripts`, e aggiornalo in modo che contenga i seguenti comandi di test e build:

   ```json
   "scripts": {
     // …
     "test": "vitest"
   }
   ```

   > [!NOTE]
   > Ecco la parte buona di usare Vite insieme a Vitest: se utilizzi altri framework di test, devi aggiungere un'altra configurazione che descriva come i file di test devono essere trasformati, ma Vitest utilizzerà automaticamente la configurazione di Vite.

3. Ora ovviamente dobbiamo aggiungere il test alla nostra base di codice. Normalmente, se stai testando la funzionalità di un file, ad esempio `App.jsx`, aggiungeresti un file chiamato `App.test.jsx` accanto ad esso. In questo caso, stiamo solo testando i dati, quindi creiamo un'altra directory per contenere i nostri test. Puoi aprire il repository di esempio che hai scaricato nel capitolo precedente e copiare la cartella `tests`.

4. Ora per eseguire manualmente il test, dalla riga di comando possiamo eseguire:

   ```bash
   npm run test
   ```

   Dovresti vedere un output come questo:

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

   Questo significa che il test è passato. Come Vite, guarderà per i cambiamenti e rieseguirà i test quando salvi un file. Possiamo uscire premendo <kbd>q</kbd>.

5. Abbiamo ancora bisogno di collegare il test alla nostra azione di build, in modo che blocchi la build se il test fallisce. Apri il file `.github/workflows/github-pages.yml` (o qualsiasi nome tu abbia dato alla tua azione di build) e aggiungi il seguente passaggio, proprio prima del passaggio che esegue `npm run build`:

   ```yaml
   - name: Install deps
     run: npm ci

   # Add this
   - name: Run tests
     run: npm run test

   - name: Build
     run: npm run build
   ```

   Questo eseguirà il test prima del passaggio di build. Se il test fallisce, la build fallirà e la distribuzione non avverrà.

6. Ora carichiamo il nuovo codice su GitHub, usando comandi simili a quelli che hai usato prima:

   ```bash
   git add .
   git commit -m 'adding test'
   git push origin main
   ```

   In alcuni casi potresti voler testare il risultato del codice costruito (dato che questo non è esattamente il codice originale che abbiamo scritto), quindi potrebbe essere necessario eseguire il test dopo il comando di build. Dovrai considerare tutti questi aspetti individuali mentre lavori sui tuoi progetti.

Infine, un minuto o due dopo il push, GitHub Pages distribuisce l'aggiornamento del progetto. Ma solo se passa il test che è stato introdotto.

## Riepilogo

Questo è tutto per il nostro caso di studio di esempio, e per il modulo! Speriamo che tu l'abbia trovato utile. Anche se c'è ancora molta strada da fare prima di poterti considerare un mago degli strumenti lato client, speriamo che questo modulo abbia dato quel primo passo importante verso la comprensione degli strumenti lato client, e la fiducia per imparare di più e provare nuove cose.

Riassumiamo tutte le parti della toolchain:

- La qualità e la manutenzione del codice sono gestite da ESLint e Prettier. Questi strumenti sono aggiunti come `devDependencies` al progetto tramite `npm install --dev eslint prettier eslint-plugin-react ...` (il plugin di ESLint è necessario perché questo particolare progetto utilizza React).
- Ci sono due file di configurazione che gli strumenti di qualità del codice leggono: `eslint.config.js` e `.prettierrc`.
- Durante lo sviluppo, continuiamo ad aggiungere dipendenze usando npm. Il server di sviluppo Vite è in esecuzione in background per monitorare i cambiamenti e per costruire automaticamente la nostra sorgente.
- La distribuzione viene gestita caricando le nostre modifiche su GitHub (sul ramo "main"), che attiva una build e una distribuzione usando GitHub Actions per pubblicare il progetto. Per la nostra istanza, questo URL è <https://mdn.github.io/client-toolchain-example/>; avrai il tuo URL univoco.
- Abbiamo anche un semplice test che blocca la costruzione e la distribuzione del sito se il feed API di GitHub non ci fornisce il formato dei dati corretto.

Per coloro che cercano una sfida, considerate se potete ottimizzare qualche parte di questa toolchain. Alcune domande da porsi:

- Possiamo estrarre solo le funzionalità di plotly.js di cui abbiamo bisogno? Questo ridurrà la dimensione del bundle JavaScript.
- Forse vuoi aggiungere altri strumenti, come TypeScript per il controllo del tipo, o stylelint per il linting CSS?
- Potrebbe React essere sostituito con [qualcosa di più piccolo](https://preactjs.com/)?
- Potresti aggiungere più test per prevenire la distribuzione di una build difettosa, come [audit delle prestazioni](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring)?
- Potresti impostare una notifica per farti sapere quando una nuova distribuzione è riuscita o fallita?

{{PreviousMenu("Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}
