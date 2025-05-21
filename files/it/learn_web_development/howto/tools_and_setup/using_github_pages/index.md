---
title: Come faccio a usare GitHub Pages?
slug: Learn_web_development/Howto/Tools_and_setup/Using_GitHub_pages
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

[GitHub](https://github.com/) è un sito di "social coding". Ti permette di caricare repository di codice per l'archiviazione nel sistema di **controllo di versione** [Git](https://git-scm.com/). Puoi poi collaborare su progetti di codice, e il sistema è open-source per impostazione predefinita, il che significa che chiunque nel mondo può trovare il tuo codice su GitHub, usarlo, imparare da esso e migliorarlo. Puoi fare lo stesso con il codice di altre persone! Questo articolo fornisce una guida di base alla pubblicazione di contenuti utilizzando la funzionalità gh-pages di GitHub.

## Pubblicazione del contenuto

GitHub è una comunità molto importante e utile nella quale è consigliabile essere coinvolti, e Git/GitHub è un sistema di [controllo di versione](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) molto popolare — la maggior parte delle aziende tecnologiche lo utilizza ora nel proprio flusso di lavoro. GitHub ha una funzione molto utile chiamata [GitHub Pages](https://pages.github.com/), che ti consente di pubblicare il codice del sito web direttamente sul Web.

### Configurazione base di GitHub

1. Prima di tutto, [installa Git](https://git-scm.com/downloads) sul tuo computer. Questo è il software di controllo di versione sottostante su cui lavora GitHub.
2. Successivamente, [iscriviti per un account GitHub](https://github.com/signup). È semplice e facile.
3. Una volta che ti sei iscritto, accedi a [github.com](https://github.com/) con il tuo nome utente e la tua password.

### Preparare il tuo codice per il caricamento

Puoi archiviare qualsiasi codice desideri in un repository GitHub, ma per utilizzare al meglio la funzione GitHub Pages, il tuo codice dovrebbe essere strutturato come un sito web tipico, ad esempio, con il punto di ingresso principale che è un file HTML chiamato `index.html`.

Un'altra cosa che devi fare prima di procedere è inizializzare la directory del tuo codice come un repository Git. Per farlo:

1. Punta la riga di comando verso la tua directory `test-site` (o qualsiasi nome tu abbia dato alla directory contenente il tuo sito web). Per questo, usa il comando `cd` (cioè "**c**hange **d**irectory"). Ecco cosa scriveresti se hai posizionato il tuo sito web in una directory chiamata `test-site` sul tuo desktop:

   ```bash
   cd Desktop/test-site
   ```

2. Quando la riga di comando punta all'interno della tua directory del sito web, digita il seguente comando, che indica al tool `git` di trasformare la directory in un repository git:

   ```bash
   git init
   ```

#### Una parentesi sulle interfacce a riga di comando

Il modo migliore per caricare il tuo codice su GitHub è tramite la riga di comando — questa è una finestra dove digiti i comandi per fare cose come creare file e eseguire programmi, piuttosto che cliccare in un'interfaccia utente. Sarà simile a questo:

![Terminale/prompt dei comandi aperto. Nessun comando è stato inserito.](command-line.png)

> [!NOTE]
> Potresti anche considerare di usare una [interfaccia grafica utente per Git](https://git-scm.com/downloads/guis) per fare lo stesso lavoro, se ti senti a disagio con la riga di comando.

Ogni sistema operativo dispone di uno strumento a riga di comando:

- **Windows**: **Prompt dei Comandi** è accessibile premendo il tasto Windows, digitando _Prompt dei Comandi_ e scegliendolo dall'elenco che appare. Nota che Windows ha le proprie convenzioni di comando diverse da Linux e macOS, quindi i comandi qui sotto potrebbero variare sulla tua macchina.
- **macOS**: **Terminale** può essere trovato in _Applicazioni > Utility_.
- **Linux**: Generalmente puoi aprire un terminale con _Ctrl + Alt + T_. Se questo non funziona, cerca **Terminale** in una barra delle applicazioni o menu.

Questo potrebbe sembrare un po' spaventoso all'inizio, ma non preoccuparti — presto prenderai confidenza con le basi. Comunichi al computer di fare qualcosa nel terminale digitando un comando e premendo Invio, come visto sopra.

### Creare un repository per il tuo codice

1. Successivamente, devi creare un nuovo repository per i tuoi file. Clicca sul Plus (+) nell'angolo in alto a destra della home page di GitHub, quindi scegli _Nuovo repository_.
2. In questa pagina, nella casella _Nome repository_, inserisci un nome per il tuo repository di codice, ad esempio _my-repository_.
3. Compila anche una descrizione per indicare cosa conterrà il tuo repository. Lo schermo dovrebbe apparire così:
   ![Pagina nuovo repository aperta nel browser, con il campo del proprietario del repository e il nome del repository compilati, e lo stesso per il campo opzionale della descrizione. La checkbox pubblica è selezionata, quella privata no, lo stesso vale per l'inizializzare questo repository con un file di documentazione (README).](create-new-repo.png)
4. Clicca _Crea repository_; questo dovrebbe portarti alla seguente pagina:
   ![La pagina del repository è aperta nel browser, sotto l'intestazione di GitHub composta da una barra di ricerca e link di navigazione al pull request, issue e gist del repository. Accanto ai link di navigazione, una notifica a campanello e un link al tuo account. Sotto, il nome del proprietario del repository seguito da una barra e il nome del repository. Sotto una barra di navigazione orizzontale composta da tab relativi al tuo repository, il tab codice è selezionato mostrando una documentazione che spiega come creare un repository o come effettuare un push utilizzando la riga di comando.](github-repo.png)

### Caricare i tuoi file su GitHub

1. Nella pagina corrente, ti interessa la sezione _…o effettua un push di un repository esistente dalla riga di comando_. Dovresti vedere due righe di codice elencate in questa sezione. Copia tutta la prima riga, incollala nella riga di comando e premi Invio. Il comando dovrebbe sembrare qualcosa del genere:

   ```bash
   git remote add origin https://github.com/chrisdavidmills/my-repository.git
   ```

2. Successivamente, digita i seguenti due comandi, premendo Invio dopo ciascuno. Questi preparano il codice per il caricamento su GitHub e chiedono a Git di gestire questi file.

   ```bash
   git add --all
   git commit -m 'adding my files to my repository'
   ```

3. Infine, effettua il push del codice su GitHub andando alla pagina web di GitHub su cui ti trovi e inserendo nel terminale il secondo dei due comandi che abbiamo visto nella sezione _…o effettua un push di un repository esistente dalla riga di comando_:

   ```bash
   git push -u origin main
   ```

4. Ora devi attivare GitHub Pages per il tuo repository. Per fare ciò, dalla homepage del tuo repository scegli _Impostazioni_, quindi seleziona _Pages_ dalla barra laterale sulla sinistra. Sotto _Source_, scegli il branch "main". La pagina dovrebbe aggiornarsi.
5. Vai di nuovo alla sezione GitHub Pages, e dovresti vedere una riga del tipo "Il tuo sito è pronto per essere pubblicato su `https://xxxxxx`."
6. Se clicchi su questo URL, dovresti andare a una versione live del tuo esempio, a condizione che la homepage si chiami `index.html` — va per impostazione predefinita a questo punto di ingresso. Se il punto di ingresso del tuo sito è chiamato in modo diverso, ad esempio `myPage.html`, dovrai andare su `https://xxxxxx/myPage.html`.

### Ulteriori conoscenze su GitHub

Se vuoi apportare ulteriori modifiche al tuo sito di test e caricarle su GitHub, devi fare la modifica ai tuoi file proprio come hai fatto prima. Poi, devi inserire i seguenti comandi (premendo Invio dopo ciascuno) per effettuare il push di tali modifiche su GitHub:

```bash
git add --all
git commit -m 'another commit'
git push
```

Puoi sostituire _another commit_ con un messaggio più adatto a descrivere quale cambiamento hai appena effettuato.

Abbiamo appena graffiato la superficie di Git. Per saperne di più, consulta la nostra [pagina su Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).
