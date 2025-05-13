---
title: Come usare GitHub Pages?
slug: Learn_web_development/Howto/Tools_and_setup/Using_GitHub_pages
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

[GitHub](https://github.com/) è un sito di "social coding". Consente di caricare repository di codice per la memorizzazione nel sistema di **controllo di versione** [Git](https://git-scm.com/). Può collaborare a progetti di codice, e il sistema è open-source per impostazione predefinita, il che significa che chiunque nel mondo può trovare il suo codice GitHub, usarlo, imparare da esso e migliorarlo. Può fare la stessa cosa con il codice degli altri! Questo articolo fornisce una guida di base alla pubblicazione di contenuti utilizzando la funzione gh-pages di GitHub.

## Pubblicare contenuti

GitHub è una comunità molto importante e utile in cui coinvolgersi, e Git/GitHub è un [sistema di controllo di versione](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) molto popolare — la maggior parte delle aziende tecnologiche lo utilizza ora nel loro flusso di lavoro. GitHub ha una funzione molto utile chiamata [GitHub Pages](https://pages.github.com/), che le consente di pubblicare il codice del sito web direttamente sul Web.

### Configurazione di base di GitHub

1. Prima di tutto, [installi Git](https://git-scm.com/downloads) sul suo computer. Questo è il software di base del sistema di controllo di versione su cui GitHub si basa.
2. Successivamente, [si registri per ottenere un account GitHub](https://github.com/signup). È semplice e facile.
3. Una volta registrata, acceda a [github.com](https://github.com/) con il suo nome utente e password.

### Preparazione del codice per il caricamento

Può memorizzare qualsiasi codice le piaccia in un repository GitHub, ma per utilizzare al meglio la funzione GitHub Pages, il suo codice dovrebbe essere strutturato come un tipico sito web, ad esempio, con il punto di ingresso principale come un file HTML chiamato `index.html`.

Un'altra cosa da fare prima di procedere è inizializzare la sua directory di codice come un repository Git. Per fare questo:

1. Punti il prompt dei comandi alla sua directory `test-site` (o come ha chiamato la directory contenente il suo sito web). Per fare ciò, usi il comando `cd` (cioè, "**c**hange **d**irectory"). Ecco cosa digiterà se ha messo il suo sito web in una directory chiamata `test-site` sul suo desktop:

   ```bash
   cd Desktop/test-site
   ```

2. Quando il prompt dei comandi punta all'interno della sua directory del sito web, digiti il seguente comando, che informa lo strumento `git` di trasformare la directory in un repository git:

   ```bash
   git init
   ```

#### Una nota sulle interfacce a riga di comando

Il modo migliore per caricare il suo codice su GitHub è tramite la riga di comando — questa è una finestra in cui digita comandi per fare cose come creare file ed eseguire programmi, piuttosto che cliccare all'interno di un'interfaccia utente. Assomiglierà a questo:

![Terminale/prompt dei comandi aperto. Nessun comando è stato inserito.](command-line.png)

> [!NOTE]
> Potrebbe anche considerare l'uso di un'interfaccia grafica [Git](https://git-scm.com/downloads/guis) per fare lo stesso lavoro, se si sente a disagio con la riga di comando.

Ogni sistema operativo viene fornito con uno strumento da riga di comando:

- **Windows**: **Prompt dei comandi** può essere accessibile premendo il tasto Windows, digitando _Prompt dei comandi_, e scegliendo dall'elenco che appare. Noti che Windows ha le proprie convenzioni di comando diverse da Linux e macOS, quindi i comandi di seguito potrebbero variare sulla sua macchina.
- **macOS**: **Terminale** è reperibile in _Applicazioni > Utility_.
- **Linux**: Di solito può aprire un terminale con _Ctrl + Alt + T_. Se non funziona, cerchi **Terminale** in una barra delle app o menu.

Può sembrare un po' spaventoso all'inizio, ma non si preoccupi — presto prenderà confidenza con le basi. Dice al computer di fare qualcosa nel terminale digitando un comando e premendo Invio, come visto sopra.

### Creare un repository per il suo codice

1. Successivamente, deve creare un nuovo repository per i suoi file. Clicchi sul Plus (+) in alto a destra della homepage di GitHub, quindi scelga _Nuovo repository_.
2. In questa pagina, nella casella _Nome del repository_, inserisca un nome per il suo repository di codice, ad esempio _my-repository_.
3. Compili anche una descrizione per indicare cosa conterrà il suo repository. Il suo schermo dovrebbe apparire in questo modo:
   ![Pagina del nuovo repository aperta nel browser, l'input del proprietario del repository e il nome del repository sono compilati, lo stesso per l'input opzionale della descrizione. La casella di controllo pubblica è selezionata, la casella di controllo privata non lo è, lo stesso vale per l'inizializzazione di questo repository con un readme.](create-new-repo.png)
4. Clicchi su _Crea repository_; questo dovrebbe portarla alla seguente pagina:
   ![La pagina del repository è aperta nel browser, sotto l'intestazione di GitHub composta dalla barra di ricerca e dai link di navigazione alle richieste pull, ai problemi e ai gist del repository. Accanto ai link di navigazione, una notifica a campana e un link al suo account. Sotto, il nome del proprietario del repository seguito da una barra con il nome del repository. Sotto una barra di navigazione orizzontale composta da diverse schede relative al suo repository, la scheda del codice selezionata visualizza una documentazione che spiega come creare un repository o come effettuare il push utilizzando la riga di comando.](github-repo.png)

### Caricamento dei suoi file su GitHub

1. Nella pagina corrente, è interessata alla sezione _…o effettua il push di un repository esistente dalla riga di comando_. Dovrebbe vedere due righe di codice elencate in questa sezione. Copi l'intera della prima riga, la incolli nella riga di comando e prema Invio. Il comando dovrebbe apparire in questo modo:

   ```bash
   git remote add origin https://github.com/chrisdavidmills/my-repository.git
   ```

2. Successivamente, digiti i seguenti due comandi, premendo Invio dopo ciascuno. Questi preparano il codice per il caricamento su GitHub e chiedono a Git di gestire questi file.

   ```bash
   git add --all
   git commit -m 'adding my files to my repository'
   ```

3. Infine, effettui il push del codice su GitHub andando alla pagina web di GitHub in cui si trova e inserendo nel terminale il secondo dei due comandi che abbiamo visto nella sezione _…o effettua il push di un repository esistente dalla riga di comando_:

   ```bash
   git push -u origin main
   ```

4. Ora deve attivare GitHub Pages per il suo repository. Per fare ciò, dalla homepage del suo repository scelga _Impostazioni_, quindi selezioni _Pages_ dalla barra laterale a sinistra. Sotto _Sorgente_, scelga il ramo "main". La pagina dovrebbe aggiornarsi.
5. Torni alla sezione GitHub Pages e dovrebbe vedere una riga del tipo "Il suo sito è pronto per essere pubblicato a `https://xxxxxx`."
6. Se clicca su questo URL, dovrebbe andare a una versione live del suo esempio, a condizione che la homepage si chiami `index.html` — va a questo punto di ingresso per impostazione predefinita. Se il punto di ingresso del suo sito si chiama diversamente, ad esempio `myPage.html`, dovrà andare a `https://xxxxxx/myPage.html`.

### Ulteriori conoscenze su GitHub

Se desidera apportare più modifiche al suo sito di test e caricarle su GitHub, deve apportare la modifica ai suoi file così come ha fatto prima. Quindi, deve inserire i seguenti comandi (premendo Invio dopo ciascuno) per pushare queste modifiche su GitHub:

```bash
git add --all
git commit -m 'another commit'
git push
```

Può sostituire _another commit_ con un messaggio più adatto per descrivere quale modifica ha appena apportato.

Abbiamo appena scalfito la superficie di Git. Per saperne di più, consulti la nostra pagina [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).
