---
title: Come si usano GitHub Pages?
slug: Learn_web_development/Howto/Tools_and_setup/Using_GitHub_pages
l10n:
  sourceCommit: 6722199b4d63fad3c33db1146af380fc98b6c202
---

[GitHub](https://github.com/) è un sito di "social coding". Consente di caricare repository di codice per archiviarli nel **sistema di controllo versione** [Git](https://git-scm.com/). È quindi possibile collaborare a progetti di codice e il sistema è open source per impostazione predefinita, il che significa che chiunque nel mondo può trovare il proprio codice su GitHub, usarlo, imparare da esso e migliorarlo. È possibile fare lo stesso anche con il codice di altre persone. Questo articolo fornisce una guida di base alla pubblicazione di contenuti mediante la funzionalità gh-pages di GitHub.

## Pubblicazione di contenuti

GitHub è una comunità molto importante e utile a cui partecipare, e Git/GitHub è un popolare [sistema di controllo versione](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control): oggi la maggior parte delle aziende tecnologiche lo utilizza nel proprio flusso di lavoro. GitHub dispone di una funzionalità molto utile chiamata [GitHub Pages](https://pages.github.com/), che consente di pubblicare sul Web il codice di un sito web.

### Configurazione di base di GitHub

1. Prima di tutto, [installare Git](https://git-scm.com/downloads/) sul proprio computer. Si tratta del software di controllo versione sottostante su cui si basa GitHub.
2. Successivamente, [registrare un account GitHub](https://github.com/signup). È semplice e facile.
3. Dopo la registrazione, accedere a [github.com](https://github.com/) con il proprio nome utente e la password.

### Preparazione del codice per il caricamento

È possibile archiviare qualsiasi codice in un repository GitHub, ma per usare al meglio la funzionalità GitHub Pages, il codice dovrebbe essere strutturato come un tipico sito web, ad esempio con il punto di ingresso principale costituito da un file HTML denominato `index.html`.

L'altra operazione necessaria prima di procedere è inizializzare la directory del codice come repository Git. Per farlo:

1. Puntare la riga di comando alla directory `test-site` (o al nome assegnato alla directory contenente il sito web). A tale scopo, usare il comando `cd` (ossia, "**c**hange **d**irectory"). Ecco cosa digitare se il sito web è stato inserito in una directory denominata `test-site` sul desktop:

   ```bash
   cd Desktop/test-site
   ```

2. Quando la riga di comando punta all'interno della directory del sito web, digitare il comando seguente, che indica allo strumento `git` di trasformare la directory in un repository git:

   ```bash
   git init
   ```

#### Nota sulle interfacce a riga di comando

Il modo migliore per caricare il codice su GitHub è tramite la riga di comando: una finestra in cui si digitano comandi per svolgere operazioni come creare file ed eseguire programmi, invece di fare clic in un'interfaccia utente. L'aspetto sarà simile al seguente:

![Terminale/prompt dei comandi aperto. Non è stato inserito alcun comando.](command-line.png)

> [!NOTE]
> È anche possibile considerare l'uso di un'[interfaccia utente grafica per Git](https://git-scm.com/downloads/guis) per svolgere lo stesso lavoro, se la riga di comando risulta poco confortevole.

Ogni sistema operativo dispone di uno strumento a riga di comando:

- **Windows**: è possibile accedere al **Prompt dei comandi** premendo il tasto Windows, digitando _Prompt dei comandi_ e selezionandolo dall'elenco visualizzato. Windows utilizza convenzioni di comando proprie, diverse da quelle di Linux e macOS, quindi i comandi riportati di seguito potrebbero variare sul proprio computer.
- **macOS**: **Terminale** si trova in _Applicazioni > Utility_.
- **Linux**: di solito è possibile aprire un terminale con _Ctrl + Alt + T_. Se non funziona, cercare **Terminale** nella barra o nel menu delle applicazioni.

All'inizio potrebbe sembrare un po' intimidatorio, ma non c'è da preoccuparsi: presto diventeranno chiari i concetti di base. È possibile indicare al computer di fare qualcosa nel terminale digitando un comando e premendo Invio, come mostrato sopra.

### Creazione di un repository per il codice

1. Successivamente, è necessario creare un nuovo repository in cui inserire i file. Fare clic sul segno più (+) in alto a destra nella pagina iniziale di GitHub, quindi scegliere _New Repository_.
2. In questa pagina, nella casella _Repository name_, inserire un nome per il repository di codice, ad esempio _my-repository_.
3. Compilare anche una descrizione che indichi il contenuto del repository. La schermata dovrebbe avere un aspetto simile al seguente:
   ![Pagina per la creazione di un nuovo repository aperta nel browser; i campi del proprietario del repository e del nome del repository sono compilati, così come il campo facoltativo della descrizione. La casella di controllo public è selezionata, quella private non lo è, come anche quella per inizializzare il repository con un readme.](create-new-repo.png)
4. Fare clic su _Create repository_; dovrebbe essere visualizzata la pagina seguente:
   ![La pagina del repository è aperta nel browser. Sotto l'intestazione GitHub, composta dalla barra di ricerca e dai collegamenti di navigazione per pull request, issues e gist del repository, sono presenti una notifica a campana e un collegamento all'account. Sotto, il nome del repository del proprietario seguito da una barra e dal nome del repository. Sotto una barra di navigazione orizzontale composta da diverse schede relative al repository, è selezionata la scheda code, che mostra documentazione su come creare un repository o come eseguire il push usando la riga di comando.](github-repo.png)

### Caricamento dei file su GitHub

1. Nella pagina corrente, interessa la sezione _…or push an existing repository from the command line_. In questa sezione dovrebbero essere elencate due righe di codice. Copiare interamente la prima riga, incollarla nella riga di comando e premere Invio. Il comando dovrebbe avere un aspetto simile al seguente:

   ```bash
   git remote add origin https://github.com/chrisdavidmills/my-repository.git
   ```

2. Successivamente, digitare i due comandi seguenti, premendo Invio dopo ciascuno. Questi preparano il codice per il caricamento su GitHub e chiedono a Git di gestire questi file.

   ```bash
   git add --all
   git commit -m 'adding my files to my repository'
   ```

3. Infine, eseguire il push del codice su GitHub andando alla pagina web di GitHub aperta e inserendo nel terminale il secondo dei due comandi presenti nella sezione _…or push an existing repository from the command line_:

   ```bash
   git push -u origin main
   ```

4. Ora è necessario attivare GitHub Pages per il repository. Per farlo, dalla pagina iniziale del repository scegliere _Settings_, quindi selezionare _Pages_ dalla barra laterale a sinistra. In _Source_, scegliere il branch "main". La pagina dovrebbe aggiornarsi.
5. Tornare alla sezione GitHub Pages: dovrebbe essere visualizzata una riga nella forma "Your site is ready to be published at `https://xxxxxx`."
6. Facendo clic su questo URL, dovrebbe essere visualizzata una versione pubblicata dell'esempio, a condizione che la pagina iniziale sia denominata `index.html`, poiché per impostazione predefinita viene usato questo punto di ingresso. Se il punto di ingresso del sito ha un nome diverso, ad esempio `myPage.html`, sarà necessario accedere a `https://xxxxxx/myPage.html`.

### Ulteriori conoscenze su GitHub

Per apportare altre modifiche al sito di prova e caricarle su GitHub, è necessario modificare i file come fatto in precedenza. Successivamente, inserire i comandi seguenti, premendo Invio dopo ciascuno, per eseguire il push delle modifiche su GitHub:

```bash
git add --all
git commit -m 'another commit'
git push
```

È possibile sostituire _another commit_ con un messaggio più appropriato per descrivere la modifica appena effettuata.

Abbiamo appena scalfito la superficie di Git. Per ulteriori informazioni, consultare la pagina [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).
