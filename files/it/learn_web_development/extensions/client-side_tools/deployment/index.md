---
title: Distribuzione dell'app
slug: Learn_web_development/Extensions/Client-side_tools/Deployment
l10n:
  sourceCommit: 6030ef1aadf967b80e2c79c3d3463cccc8ea0c95
---

{{PreviousMenu("Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}

Nell'articolo conclusivo della nostra serie, prendiamo l'esempio di toolchain creato nell'articolo precedente e lo estendiamo in modo da poter distribuire la nostra app di esempio. Invieremo il codice su GitHub, lo distribuiremo usando GitHub Pages e mostreremo anche come aggiungere un semplice test al processo.

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
        Completare il caso di studio sulla toolchain, concentrandosi sulla
        distribuzione dell'app.
      </td>
    </tr>
  </tbody>
</table>

## Dopo lo sviluppo

In questa fase del ciclo di vita del progetto è potenzialmente necessario risolvere un'ampia gamma di problemi. È quindi importante creare una toolchain che gestisca questi problemi richiedendo il minor intervento manuale possibile.

Ecco solo alcuni aspetti da considerare per questo progetto specifico:

- Generare una build di produzione: assicurarsi che i file siano minimizzati, suddivisi in chunk, sottoposti a tree-shaking e che le versioni siano soggette a "cache busting".
- Eseguire test: possono spaziare da "questo codice è formattato correttamente?" a "questo elemento fa ciò che ci si aspetta?", assicurandosi che i test non riusciti impediscano la distribuzione.
- Distribuire effettivamente il codice aggiornato a un URL pubblico: oppure potenzialmente a un URL di staging, in modo che possa essere prima revisionato.

> [!NOTE]
> Cache busting è un nuovo termine che non abbiamo ancora incontrato nel modulo. È la strategia di aggirare il meccanismo di caching del browser, costringendo il browser a scaricare una nuova copia del codice. Vite (e in effetti molti altri strumenti) genera nomi di file univoci per ogni nuova build. Questo nome file univoco "aggira" la cache del browser, garantendo così che il browser scarichi il codice aggiornato ogni volta che viene effettuato un aggiornamento del codice distribuito.

Le attività precedenti si suddividono inoltre in ulteriori attività; si noti che la maggior parte dei team di sviluppo web avrà termini e processi propri per almeno una parte della fase successiva allo sviluppo.

Per questo progetto useremo l'offerta gratuita di hosting statico di [GitHub Pages](https://pages.github.com/) per ospitare il nostro progetto. Non solo pubblica il nostro sito web su Internet, ma fornisce anche un URL per il sito. È ottimo: molti siti web di esempio di MDN sono ospitati su GitHub Pages.

La distribuzione su un hosting tende a collocarsi alla fine del ciclo di vita del progetto, ma servizi come GitHub Pages riducono il costo delle distribuzioni, sia in termini economici sia in termini di tempo necessario per eseguirle. Questo rende possibile distribuire durante lo sviluppo, sia per condividere il lavoro in corso sia per avere una pre-release per qualche altro scopo.

GitHub offre un flusso di lavoro semplice per trasformare nuovo codice in un sito web pubblico:

- Il codice viene inviato su GitHub.
- Si definisce una [GitHub Action](https://docs.github.com/en/actions) che viene attivata quando viene eseguito un nuovo push sul ramo principale, che compila il codice e lo colloca in una posizione specifica.
- GitHub Pages quindi serve il codice a un URL specifico.

Sono proprio questi tipi di servizi connessi che consigliamo di cercare quando si decide la propria toolchain di build. È possibile effettuare il commit del codice e il push su GitHub e il codice aggiornato attiverà automaticamente l'intera procedura di build. Se tutto va bene, una modifica pubblica viene distribuita automaticamente. L'_unica_ azione da eseguire è quel push iniziale.

Tuttavia, è necessario configurare questi passaggi, ed è ciò che vedremo ora.

## Il processo di build

Ancora una volta, poiché stiamo usando Vite per lo sviluppo, l'opzione di build è estremamente semplice da aggiungere. Come visto in precedenza, abbiamo già uno script personalizzato `npm run build` che consente a Vite di compilare tutto per la produzione, anziché eseguirlo solamente per scopi di sviluppo e test. Questo include la {{Glossary("Minification", "minificazione")}}, il {{Glossary("Tree_shaking", "tree-shaking")}} del codice e il cache busting dei nomi file.

È una buona pratica definire sempre uno script `build` nel progetto, così da poter fare affidamento su `npm run build` per eseguire sempre l'intero passaggio di build, senza dover ricordare gli argomenti specifici del comando di build per ciascun progetto.

Il codice di produzione appena creato viene collocato in una nuova directory chiamata `dist`, che contiene _tutti_ i file necessari per eseguire il sito web, pronti per essere caricati su un server.

Tuttavia, eseguire questo passaggio manualmente non è il nostro obiettivo finale: ciò che vogliamo è che la build avvenga automaticamente e che il risultato della directory `dist` venga distribuito sul sito web.

## Effettuare il commit delle modifiche su GitHub

Questa sezione consente di archiviare il codice in un repository git, ma è ben lontana dall'essere un tutorial su git. Sono disponibili molti ottimi tutorial e libri e la nostra pagina [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control) è un buon punto di partenza.

Abbiamo inizializzato in precedenza la nostra directory di lavoro come directory di lavoro git. Un modo rapido per verificarlo consiste nell'eseguire il comando seguente:

```bash
git status
```

Dovrebbe essere visualizzato un rapporto sullo stato relativo ai file tracciati, ai file in staging e così via: tutti termini che fanno parte della terminologia di git. Se viene restituito l'errore `fatal: not a git repository`, la directory di lavoro non è una directory di lavoro git e sarà necessario inizializzare git usando `git init`.

Ora abbiamo tre attività da svolgere:

- Aggiungere allo staging le modifiche effettuate (un nome speciale per il luogo da cui git effettuerà il commit dei file).
- Effettuare il commit delle modifiche nel repository.
- Inviare le modifiche su GitHub.

1. Per aggiungere le modifiche, eseguire il comando seguente:

   ```bash
   git add .
   ```

   Si noti il punto alla fine: significa "tutto in questa directory". Il comando `git add .` è un approccio piuttosto drastico: aggiunge in un colpo solo tutte le modifiche locali su cui si è lavorato. Per un controllo più preciso su ciò che si aggiunge, usare `git add -p` per un processo interattivo oppure aggiungere singoli file usando `git add path/to/file`.

2. Ora che tutto il codice è in staging, possiamo effettuare il commit; eseguire il comando seguente:

   ```bash
   git commit -m 'committing initial code'
   ```

   > [!NOTE]
   > Sebbene sia possibile scrivere qualunque cosa nel messaggio di commit, sul web sono disponibili alcuni consigli utili per scrivere buoni messaggi di commit. Mantenerli brevi, concisi e descrittivi, in modo che descrivano chiaramente cosa fa la modifica.

3. Infine, il codice deve essere inviato al repository ospitato su GitHub. Facciamolo ora.

   Su GitHub, visitare <https://github.com/new> e creare un repository personale che ospiti questo codice.

4. Assegnare al repository un nome breve e facile da ricordare, senza spazi (usare trattini per separare le parole), e una descrizione, quindi fare clic su _Create repository_ in fondo alla pagina.

   Ora dovrebbe essere disponibile un URL "remote" che punta al nuovo repository GitHub.

   ![Schermata di GitHub che mostra gli URL remoti utilizzabili per distribuire codice a un repository GitHub](github-quick-setup.png)

5. Questa posizione remota deve essere aggiunta al repository git locale prima di potervi eseguire il push, altrimenti non sarà possibile trovarla. Sarà necessario eseguire un comando con la struttura seguente (per ora usare l'opzione HTTPS fornita, soprattutto per chi è nuovo a GitHub, non l'opzione SSH):

   ```bash
   git remote add origin https://github.com/your-name/repo-name.git
   ```

   Quindi, se l'URL remoto fosse `https://github.com/remy/super-website.git`, come nello screenshot precedente, il comando sarebbe:

   ```bash
   git remote add origin https://github.com/remy/super-website.git
   ```

   Modificare l'URL con quello del proprio repository ed eseguire ora il comando.

   > [!NOTE]
   > Dopo aver scelto il nome del repository, assicurarsi che l'opzione `base` in `vite.config.js` rifletta questo nome, come indicato nel [capitolo precedente](/it/docs/Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain#javascript_transformation). Altrimenti, le risorse JavaScript e CSS non verranno collegate correttamente.

6. Ora è tutto pronto per eseguire il push del codice su GitHub; eseguire il comando seguente:

   ```bash
   git push origin main
   ```

   A questo punto verrà richiesto di inserire un nome utente e una password prima che Git consenta l'invio del push. Questo avviene perché abbiamo usato l'opzione HTTPS anziché l'opzione SSH, come visto nello screenshot precedente. A questo scopo occorrono il nome utente GitHub e, se non è attivata l'autenticazione a due fattori (2FA), la password GitHub. È sempre consigliabile usare la 2FA quando possibile, ma occorre tenere presente che, in tal caso, sarà necessario usare anche un "personal access token". Le pagine di aiuto di GitHub offrono un'[eccellente e semplice guida su come ottenerne uno](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

> [!NOTE]
> Per chi fosse interessato a usare l'opzione SSH, evitando così di inserire nome utente e password ogni volta che viene effettuato un push su GitHub, [questo tutorial spiega come fare](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

Quest'ultimo comando indica a git di inviare il codice alla posizione "remota" chiamata `origin` (ovvero il repository ospitato su github.com; avremmo potuto chiamarlo come preferiamo) usando il ramo `main`. Non abbiamo ancora incontrato i rami, ma il ramo "main" è la posizione predefinita per il nostro lavoro ed è quello da cui git parte. Quando definiremo l'azione attivata per compilare il sito web, la configureremo anche per monitorare le modifiche sul ramo "main".

> [!NOTE]
> Fino a ottobre 2020 il ramo predefinito su GitHub era `master`, che per varie ragioni sociali è stato cambiato in `main`. Occorre essere consapevoli che questo vecchio ramo predefinito può apparire in vari progetti, ma consigliamo di usare `main` per i propri progetti.

Con il progetto sottoposto a commit in git e inviato al repository GitHub, il passaggio successivo della toolchain consiste nel definire un'azione di build affinché il progetto possa essere distribuito sul web.

## Usare GitHub Actions per la distribuzione

GitHub Actions, come la configurazione di ESLint, è un altro argomento molto vasto in cui addentrarsi. Non è semplice configurarlo correttamente al primo tentativo, ma per attività comuni come "compilare un sito web statico e distribuirlo su GitHub Pages" esistono molti esempi da copiare e incollare. È possibile seguire le istruzioni in [Publishing with a custom GitHub Actions workflow](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow). È possibile consultare il [nostro file GitHub Action](https://github.com/mdn/client-toolchain-example/blob/main/.github/workflows/github-pages.yml) per un esempio funzionante. Il nome del file non è importante.

Dopo aver effettuato il commit di questo file sul ramo principale, dovrebbe essere visualizzato un piccolo segno di spunta verde accanto al titolo del commit:

![Schermata di GitHub che mostra un segno di spunta verde accanto al titolo di un commit](build-action-pass.png)

Se è visibile un punto giallo, significa che l'azione è in esecuzione; se è visibile una croce rossa, significa che l'azione non è riuscita. Fare clic sull'icona per visualizzare lo stato e i log della propria azione di build (nel nostro caso denominata "Deploy build").

Dopo aver atteso qualche altro minuto, è possibile visitare l'URL di GitHub Pages per vedere il sito web pubblicato sul web. Il collegamento ha questo aspetto: `https://<your-name>.github.io/<repo-name>`. Per il nostro esempio, si trova su <https://mdn.github.io/client-toolchain-example/>.

Ora manca un ultimo collegamento nella toolchain: un test per assicurarsi che il codice funzioni.

## Test

Il testing è di per sé un argomento vastissimo, anche nell'ambito dello sviluppo front-end. Mostreremo come aggiungere un test iniziale al progetto e come usare il test per impedire o consentire la distribuzione del progetto.

Esistono molti modi per affrontare il problema dei test:

- Test end-to-end, in cui un visitatore fa clic su un elemento e accade qualcos'altro.
- Test di integrazione, che in sostanza chiedono: "un blocco di codice funziona ancora quando è collegato a un altro blocco?"
- Test unitari, in cui vengono testate piccole e specifiche funzionalità per verificare se fanno ciò che dovrebbero fare.
- [Molti altri tipi](https://en.wikipedia.org/wiki/Functional_testing). Consultare anche il nostro [modulo sui test cross-browser](/it/docs/Learn_web_development/Extensions/Testing) per molte informazioni utili sui test.

Ricordare inoltre che i test non sono limitati a JavaScript: è possibile eseguire test sul DOM renderizzato, sulle interazioni utente, sul CSS e perfino sull'aspetto di una pagina.

Tuttavia, per questo progetto creeremo un piccolo test che verificherà se i dati dell'API GitHub sono nel formato corretto. In caso contrario, il test non riuscirà e impedirà che il progetto venga pubblicato. Fare qualcos'altro andrebbe oltre lo scopo di questo modulo: il testing è un argomento enorme che richiede realmente un modulo separato. Ci auguriamo che questa sezione renda almeno consapevoli della necessità dei test e ispiri ad approfondire l'argomento.

Il test in sé non è ciò che importa. Ciò che importa è come viene gestito il fallimento o il successo. Poiché stiamo già scrivendo un'azione di build personalizzata, possiamo aggiungere un passaggio prima della build che esegua il test. Se il test non riesce, la build non riesce e la distribuzione non avviene.

La buona notizia è che, poiché stiamo usando Vite, Vite offre già un buon strumento integrato per il testing: [Vitest](https://vitest.dev/guide/).

Iniziamo.

1. Installare Vitest:

   ```bash
   npm install --save-dev vitest
   ```

2. Nel file package.json, trovare il membro `scripts` e aggiornarlo in modo che contenga i seguenti comandi di test e build:

   ```json
   {
     "scripts": {
       // …
       "test": "vitest"
     }
   }
   ```

   > [!NOTE]
   > Ecco l'aspetto positivo dell'uso di Vite insieme a Vitest: con altri framework di testing, è necessario aggiungere un'altra configurazione che descriva come devono essere trasformati i file di test, ma Vitest userà automaticamente la configurazione di Vite.

3. Ora, naturalmente, dobbiamo aggiungere il test alla codebase. Normalmente, se si sta testando la funzionalità di un file, ad esempio `App.jsx`, si aggiungerebbe accanto a esso un file chiamato `App.test.jsx`. In questo caso stiamo solo testando i dati, quindi creiamo un'altra directory che contenga i test. È possibile aprire il repository di esempio scaricato nel capitolo precedente e copiare la cartella `tests`.

4. Ora, per eseguire manualmente il test dalla riga di comando, possiamo eseguire:

   ```bash
   npm run test
   ```

   Dovrebbe essere visualizzato un output simile a questo:

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

   Questo significa che il test è stato superato. Come Vite, monitorerà le modifiche e rieseguirà i test quando viene salvato un file. È possibile uscire premendo <kbd>q</kbd>.

5. Dobbiamo ancora collegare il test all'azione di build, affinché blocchi la build se il test non riesce. Aprire il file `.github/workflows/github-pages.yml` (o qualunque nome file sia stato assegnato all'azione di build) e aggiungere il passaggio seguente, subito prima del passaggio che esegue `npm run build`:

   ```yaml
   - name: Install deps
     run: npm ci

   # Add this
   - name: Run tests
     run: npm run test

   - name: Build
     run: npm run build
   ```

   Questo eseguirà il test prima del passaggio di build. Se il test non riesce, la build non riesce e la distribuzione non avviene.

6. Ora carichiamo il nuovo codice su GitHub, usando comandi simili a quelli usati in precedenza:

   ```bash
   git add .
   git commit -m 'adding test'
   git push origin main
   ```

   In alcuni casi potrebbe essere utile testare il risultato del codice compilato, poiché non corrisponde esattamente al codice originale scritto, quindi il test potrebbe dover essere eseguito dopo il comando di build. Sarà necessario considerare tutti questi singoli aspetti mentre si lavora sui propri progetti.

Infine, circa un minuto dopo il push, GitHub Pages distribuirà l'aggiornamento del progetto. Ma solo se supera il test introdotto.

## Riepilogo

Questo conclude il nostro caso di studio di esempio e il modulo. Ci auguriamo che sia stato utile. Sebbene ci sia ancora molta strada da fare prima di potersi considerare esperti di strumenti client-side, speriamo che questo modulo abbia fornito quel primo importante passo verso la comprensione degli strumenti client-side, oltre alla fiducia necessaria per approfondire e provare nuove soluzioni.

Riassumiamo tutte le parti della toolchain:

- La qualità e la manutenzione del codice sono gestite da ESLint e Prettier. Questi strumenti vengono aggiunti come `devDependencies` al progetto tramite `npm install --dev eslint prettier eslint-plugin-react ...` (il plugin ESLint è necessario perché questo particolare progetto usa React).
- Esistono due file di configurazione letti dagli strumenti per la qualità del codice: `eslint.config.js` e `.prettierrc`.
- Durante lo sviluppo, continuiamo ad aggiungere dipendenze usando npm. Il server di sviluppo Vite viene eseguito in background per monitorare le modifiche e compilare automaticamente il codice sorgente.
- La distribuzione viene gestita inviando le modifiche su GitHub, nel ramo "main", che attiva una build e una distribuzione tramite GitHub Actions per pubblicare il progetto. Per la nostra istanza, questo URL è <https://mdn.github.io/client-toolchain-example/>; sarà disponibile un URL univoco personale.
- Abbiamo anche un semplice test che blocca la build e la distribuzione del sito se il feed dell'API GitHub non fornisce il formato dati corretto.

Per chi desidera una sfida, considerare se è possibile ottimizzare qualche parte di questa toolchain. Alcune domande da porsi:

- È possibile estrarre solo le funzionalità di plotly.js necessarie? Questo ridurrà la dimensione del bundle JavaScript.
- Forse si desidera aggiungere altri strumenti, come TypeScript per il controllo dei tipi o stylelint per il linting CSS?
- React potrebbe essere sostituito con [qualcosa di più leggero](https://preactjs.com/)?
- È possibile aggiungere altri test per impedire la distribuzione di una build errata, come gli [audit delle prestazioni](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring)?
- È possibile configurare una notifica per sapere quando una nuova distribuzione ha avuto successo o non è riuscita?

{{PreviousMenu("Learn_web_development/Extensions/Client-side_tools/Introducing_complete_toolchain", "Learn_web_development/Extensions/Client-side_tools")}}
