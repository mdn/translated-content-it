---
title: Configurazione di un ambiente di sviluppo Django
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Django/development_environment
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che sa a cosa serve Django, le mostreremo come configurare e testare un ambiente di sviluppo Django su Windows, Linux (Ubuntu) e macOS — qualunque sistema operativo comune stia utilizzando, questo articolo dovrebbe fornirle le informazioni necessarie per iniziare a sviluppare applicazioni Django.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenze di base su come utilizzare un terminale/linea di comando e su come installare pacchetti software sul sistema operativo del suo computer di sviluppo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Avere un ambiente di sviluppo per Django (4.*) in esecuzione sul suo computer.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica dell'ambiente di sviluppo Django

Django rende molto facile configurare il proprio computer in modo da poter iniziare a sviluppare applicazioni web. Questa sezione spiega cosa si ottiene con l'ambiente di sviluppo e fornisce una panoramica di alcune delle opzioni di configurazione e impostazione. Il resto dell'articolo spiega il metodo _consigliato_ per installare l'ambiente di sviluppo Django su Ubuntu, macOS e Windows, e come è possibile testarlo.

### Che cos'è l'ambiente di sviluppo Django?

L'ambiente di sviluppo è un'installazione di Django sul suo computer locale che può utilizzare per sviluppare e testare applicazioni Django prima di distribuirle in un ambiente di produzione.

Gli strumenti principali forniti da Django sono un insieme di script Python per creare e lavorare con progetti Django, insieme a un semplice _server web di sviluppo_ che può utilizzare per testare applicazioni web Django locali (cioè, sul suo computer, non su un server web esterno) sul browser web del suo computer.

Ci sono altri strumenti periferici, che spesso fanno parte dell'ambiente di sviluppo, che qui non copriremo. Questi includono strumenti come un [editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o IDE per modificare il codice, linters per il formattazione automatica, e così via. Presumiamo che abbia già installato un editor di testo.

### Quali sono le opzioni di configurazione di Django?

Django è estremamente flessibile in termini di come e dove può essere installato e configurato. Django può essere:

- Installato su diversi sistemi operativi.
- Installato da sorgente, dal Python Package Index (PyPi) e in molti casi dal gestore dei pacchetti del computer host.
- Configurato per utilizzare uno dei diversi database, che potrebbero anche dover essere installati e configurati separatamente.
- Eseguito nell'ambiente Python del sistema principale o in ambienti virtuali Python separati.

Ciascuna di queste opzioni richiede una configurazione e un'installazione leggermente diverse. Le sottosezioni seguenti spiegano alcune delle sue scelte. Per il resto dell'articolo, le mostreremo come configurare Django su un piccolo numero di sistemi operativi, e tale configurazione sarà presupposta per il resto di questo modulo.

> [!NOTE]
> Altre opzioni di installazione possibili sono trattate nella documentazione ufficiale di Django. Forniamo i collegamenti ai [documenti appropriati qui sotto](#vedere_anche).

#### Quali sistemi operativi sono supportati?

Le applicazioni web Django possono essere eseguite su quasi qualsiasi macchina che possa eseguire il linguaggio di programmazione Python 3: Windows, macOS, Linux/Unix, Solaris, tanto per citarne alcuni. Quasi qualsiasi computer dovrebbe avere le prestazioni necessarie per eseguire Django durante lo sviluppo.

In questo articolo, forniremo istruzioni per Windows, macOS e Linux/Unix.

#### Quale versione di Python dovrebbe essere utilizzata?

Può utilizzare qualsiasi versione di Python supportata dalla versione di Django di destinazione. Per Django 5.0 le versioni consentite sono Python 3.10 a 3.12 (vedere [FAQ:Installazione](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django)).

Il progetto Django _consiglia_ (e "supporta ufficialmente") l'utilizzo della versione più recente disponibile della versione supportata di Python.

#### Dove possiamo scaricare Django?

Ci sono tre luoghi dove può scaricare Django:

- Il Python Package Repository (PyPi), utilizzando lo strumento _pip_. Questo è il modo migliore per ottenere la versione stabile più recente di Django.
- Utilizzare una versione dal gestore dei pacchetti del suo computer. Le distribuzioni di Django che sono incluse nei sistemi operativi offrono un meccanismo di installazione familiare. Si noti però che la versione inclusa potrebbe essere piuttosto vecchia, e può essere installata solo nell'ambiente Python di sistema (che potrebbe non essere ciò che desidera).
- Installare da sorgente. Può ottenere e installare l'ultima versione all'avanguardia di Django dalla sorgente. Questo non è raccomandato per i principianti ma è necessario quando è pronto a iniziare a contribuire direttamente a Django stesso.

Questo articolo mostra come installare Django da PyPi, per ottenere l'ultima versione stabile.

#### Quale database?

Django supporta ufficialmente i database PostgreSQL, MariaDB, MySQL, Oracle e SQLite, e ci sono librerie comunitarie che forniscono livelli variabili di supporto per altri database SQL e NoSQL popolari. Si consiglia di selezionare lo stesso database sia per la produzione che per lo sviluppo (sebbene Django astragga molte delle differenze del database utilizzando il suo Object-Relational Mapper (ORM), ci sono ancora [problemi potenziali](https://docs.djangoproject.com/en/5.0/ref/databases/) che è meglio evitare).

Per questo articolo (e la maggior parte di questo modulo) utilizzeremo il database _SQLite_, che memorizza i suoi dati in un file. SQLite è destinato ad essere utilizzato come un database leggero e non può supportare un alto livello di concorrenza. È, però, un'ottima scelta per applicazioni che sono principalmente di sola lettura.

> [!NOTE]
> Django è configurato per utilizzare SQLite per impostazione predefinita quando si avvia un progetto del sito web utilizzando gli strumenti standard (_django-admin_). È un'ottima scelta quando si inizia perché non richiede configurazioni o installazioni aggiuntive.

#### Installare a livello di sistema o in un ambiente virtuale Python?

Quando si installa Python3 si ottiene un unico ambiente globale condiviso da tutto il codice di Python3. Mentre si possono installare i pacchetti Python che si desiderano nell'ambiente, si può installare solo una particolare versione di ciascun pacchetto alla volta.

> [!NOTE]
> Le applicazioni Python installate nell'ambiente globale possono potenzialmente entrare in conflitto tra loro (cioè, se dipendono da versioni diverse dello stesso pacchetto).

Se installa Django nell'ambiente predefinito/globale, allora potrà solo utilizzare una versione di Django sul computer. Questo può essere un problema se vuole creare nuovi siti web (utilizzando l'ultima versione di Django) mentre continua a mantenere siti web che si basano su versioni più vecchie.

Di conseguenza, gli sviluppatori esperti di Python/Django solitamente eseguono le applicazioni Python in _ambienti virtuali Python_ indipendenti. Questo consente di avere più ambienti Django diversi su un singolo computer. Il team di sviluppo di Django stesso consiglia di utilizzare ambienti virtuali Python!

Questo modulo presume che abbia installato Django in un ambiente virtuale, e le mostreremo come fare qui sotto.

## Installazione di Python 3

Per poter utilizzare Django, deve avere Python 3 sul suo sistema operativo. Avrà anche bisogno dello strumento [Python Package Index](https://pypi.org/) — _pip3_ — che viene utilizzato per gestire (installare, aggiornare e rimuovere) pacchetti/librerie Python utilizzati da Django e dalle sue altre applicazioni Python.

Questa sezione spiega brevemente come può controllare quali versioni di Python sono presenti e installare nuove versioni se necessario, per Ubuntu Linux 20.04, macOS e Windows 10.

> [!NOTE]
> A seconda della sua piattaforma, può anche essere in grado di installare Python/pip dal gestore dei pacchetti del sistema operativo o tramite altri meccanismi. Per la maggior parte delle piattaforme, può scaricare i file di installazione richiesti da <https://www.python.org/downloads/> e installarli utilizzando il metodo appropriato specifico per la piattaforma.

### Ubuntu 22.04

Ubuntu Linux 22.04 LTS include Python 3.10.12 per impostazione predefinita. Può confermarlo eseguendo il seguente comando nel terminale bash:

```bash
python3 -V
# Output: Python 3.10.12
```

Tuttavia, lo strumento Python Package Index (_pip3_) di cui avrà bisogno per installare pacchetti per Python 3 (incluso Django) **non** è disponibile per impostazione predefinita. Può installare _pip3_ nel terminale bash utilizzando:

```bash
sudo apt install python3-pip
```

> [!NOTE]
> Python 3.10 è la versione più vecchia [supportata da Django 5.0](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django).
> Non è _necessario_ utilizzare l'ultima versione di Python per questo tutorial, ma se lo desidera, ci sono istruzioni su internet.

### macOS

macOS non include Python 3 per impostazione predefinita (Python 2 è incluso nelle versioni più vecchie). Può confermarlo eseguendo il seguente comando nel terminale:

```bash
python3 -V
```

Questo visualizzerà il numero della versione di Python, che indica che Python 3 è installato, o `python3: command not found`, che indica che Python 3 non è stato trovato.

Può facilmente installare Python 3 (insieme allo strumento _pip3_) da [python.org](https://www.python.org/):

1. Scaricare il programma di installazione richiesto:

   1. Vai su <https://www.python.org/downloads/macos/>
   2. Scaricare la versione stabile della versione recente [supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) che funziona con Django 5.0.
      (al momento della scrittura questa è Python 3.11.8).

2. Localizzare il file utilizzando _Finder_ e fare doppio clic sul file del pacchetto. Seguire le istruzioni di installazione.

3. Ora può confermare l'installazione riuscita eseguendo nuovamente `python3 -V` e controllando il numero della versione di Python.

Può verificare allo stesso modo che _pip3_ sia installato elencando i pacchetti disponibili:

```bash
pip3 list
```

### Windows 10 o 11

Windows non include Python per impostazione predefinita, ma può installarlo facilmente (insieme allo strumento _pip3_) da [python.org](https://www.python.org/):

1. Scaricare il programma di installazione richiesto:

   1. Vai su <https://www.python.org/downloads/windows/>
   2. Scaricare la versione stabile della versione recente [supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) che funziona con Django 5.0.
      (al momento della scrittura questa è Python 3.11.8).

2. Installare Python facendo doppio clic sul file scaricato e seguendo le istruzioni di installazione
3. Assicurarsi di selezionare la casella etichettata "Add Python to PATH"

Può quindi verificare che Python 3 sia installato digitando il seguente testo nel prompt dei comandi:

```bash
py -3 -V
```

L'installer di Windows incorpora _pip3_ (il gestore dei pacchetti Python) per impostazione predefinita. Può elencare i pacchetti installati come mostrato di seguito:

```bash
py -3 -m pip list
```

> [!NOTE]
> L'installer dovrebbe configurare tutto ciò di cui ha bisogno affinché il comando sopra funzioni.
> Se tuttavia riceve un messaggio che indica che Python non può essere trovato, potrebbe aver dimenticato di aggiungerlo al percorso di sistema.
> Può farlo eseguendo nuovamente l'installer, selezionando "Modify" e spuntando la casella etichettata "Add Python to environment variables" nella seconda pagina.

## Chiamare Python 3 e pip3

Noterà che nelle sezioni precedenti utilizziamo comandi diversi per chiamare Python 3 e pip su diversi sistemi operativi.

Se ha installato solo Python 3 (e non Python 2), i comandi "semplici" `python` e `pip` possono generalmente essere utilizzati per eseguire Python e pip su qualsiasi sistema operativo. Se questo è consentito sul suo sistema otterrà una stringa di versione "3" quando esegue `-V` con i comandi semplici, come mostrato:

```bash
python -V
pip -V
```

Se Python 2 è installato, per utilizzare la versione 3 dovrebbe prefissare i comandi con `python3` e `pip3` su Linux/macOS, e con `py -3` e `py -3 -m pip` su Windows:

```bash
# Linux/macOS
python3 -V
pip3 -V

# Windows
py -3 -V
py -3 -m pip list
```

Le istruzioni seguenti mostrano i comandi specifici per la piattaforma in quanto funzionano su più sistemi.

## Utilizzare Django all'interno di un ambiente virtuale Python

Le librerie che utilizzeremo per creare i nostri ambienti virtuali sono [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) (Linux e macOS) e [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) (Windows), che a loro volta utilizzano lo strumento [virtualenv](https://virtualenv.pypa.io/en/latest/). Gli strumenti wrapper creano un'interfaccia coerente per gestire gli ambienti su tutte le piattaforme.

### Installazione del software dell'ambiente virtuale

#### Configurazione dell'ambiente virtuale su Ubuntu

Dopo aver installato Python e pip, può installare _virtualenvwrapper_ (che include _virtualenv_). Può controllare [la guida ufficiale per l'installazione](https://virtualenvwrapper.readthedocs.io/en/latest/install.html) o seguire le istruzioni riportate di seguito.

Installare lo strumento utilizzando _pip3_:

```bash
sudo pip3 install virtualenvwrapper
```

Quindi, aggiungere le seguenti righe alla fine del file di avvio della shell (questo è un file nascosto chiamato **.bashrc** nella sua directory home). Queste impostano la posizione in cui gli ambienti virtuali dovrebbero risiedere, la posizione delle directory del progetto di sviluppo e la posizione dello script installato con questo pacchetto:

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS=' -p /usr/bin/python3 '
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> Le variabili `VIRTUALENVWRAPPER_PYTHON` e `VIRTUALENVWRAPPER_VIRTUALENV_ARGS` puntano alla normale posizione di installazione per Python 3, e `source /usr/local/bin/virtualenvwrapper.sh` punta alla normale posizione dello script `virtualenvwrapper.sh`. Se _virtualenv_ non funziona quando lo testa, una cosa da controllare è che Python e lo script siano nella posizione attesa (e quindi cambiare il file di avvio in modo appropriato).
>
> Può trovare le posizioni corrette per il suo sistema utilizzando i comandi `which virtualenvwrapper.sh` e `which python3`.

Quindi ricaricare il file di avvio eseguendo il seguente comando nel terminale:

```bash
source ~/.bashrc
```

A questo punto dovrebbe vedere una quantità di script in esecuzione come mostrato sotto:

```bash
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/premkproject
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postmkproject
# …
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/preactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/get_env_details
```

Ora può creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

#### Configurazione dell'ambiente virtuale su macOS

Impostare _virtualenvwrapper_ su macOS è quasi esattamente lo stesso di Ubuntu (può seguire le istruzioni sia dalla [guida ufficiale per l'installazione](https://virtualenvwrapper.readthedocs.io/en/latest/install.html) che da quelle riportate di seguito).

Installare _virtualenvwrapper_ (e _virtualenv_ in bundle) utilizzando _pip_ come mostrato.

```bash
sudo pip3 install virtualenvwrapper
```

Quindi aggiungere le seguenti righe alla fine del file di avvio della shell (queste sono le stesse righe di Ubuntu). Se sta utilizzando la _zsh shell_, il file di avvio sarà un file nascosto chiamato **.zshrc** nella sua directory home. Se sta utilizzando la _bash shell_, sarà un file nascosto chiamato **.bash_profile**. Potrebbe dover creare il file se non esiste ancora.

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> La variabile `VIRTUALENVWRAPPER_PYTHON` punta alla normale posizione di installazione per Python 3, e `source /usr/local/bin/virtualenvwrapper.sh` punta alla normale posizione dello script `virtualenvwrapper.sh`. Se _virtualenv_ non funziona quando lo testa, una cosa da controllare è che Python e lo script siano nella posizione attesa (e quindi cambiare il file di avvio in modo appropriato).
>
> Ad esempio, un test di installazione su macOS ha prodotto la necessità di aggiungere le seguenti righe nel file di avvio:
>
> ```bash
> export WORKON_HOME=$HOME/.virtualenvs
> export VIRTUALENVWRAPPER_PYTHON=/Library/Frameworks/Python.framework/Versions/3.7/bin/python3
> export PROJECT_HOME=$HOME/Devel
> source /Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh
> ```
>
> Può trovare le posizioni corrette per il suo sistema utilizzando i comandi `which virtualenvwrapper.sh` e `which python3`.

Poi ricaricare il file di avvio effettuando la seguente chiamata nel terminale:

```bash
source ~/.bash_profile
```

A questo punto, potrebbe vedere una serie di script in esecuzione (gli stessi script dell'installazione di Ubuntu). Ora dovrebbe essere in grado di creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

> [!NOTE]
> Se non riesce a trovare il file di avvio per modificarlo nel Finder, può anche aprirlo nel terminale utilizzando nano.
>
> Assumendo che stia utilizzando bash, i comandi appaiono simili a questo:
>
> ```bash
> cd ~  # Naviga alla mia directory home
> ls -la # Elenca il contenuto della directory. Dovrebbe vedere .bash_profile
> nano .bash_profile # Apre il file nell'editor di testo nano, all'interno del terminale
> # Scorrere fino alla fine del file, e copiare le righe sopra
> # Utilizzare Ctrl+X per uscire da nano, scegliere Y per salvare il file.
> ```

#### Configurazione dell'ambiente virtuale su Windows

L'installazione di [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) è ancora più semplice che configurare _virtualenvwrapper_ perché non è necessario configurare dove l'ambiente memorizza le informazioni sugli ambienti virtuali (esiste un valore predefinito). Tutto ciò che deve fare è eseguire il seguente comando nel prompt dei comandi:

```bash
py -3 -m pip install virtualenvwrapper-win
```

Ora può creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

### Creazione di un ambiente virtuale

Una volta installato _virtualenvwrapper_ o _virtualenvwrapper-win_, lavorare con gli ambienti virtuali è molto simile su tutte le piattaforme.

Ora può creare un nuovo ambiente virtuale con il comando `mkvirtualenv`. Quando questo comando viene eseguito, vedrà l'ambiente essere configurato (quello che si vede è leggermente specifico della piattaforma). Quando il comando è completato, il nuovo ambiente virtuale sarà attivo — può vedere questo perché l'inizio del prompt sarà il nome dell'ambiente tra parentesi (di seguito lo mostriamo per Ubuntu, ma la riga finale è simile per Windows/macOS).

```bash
mkvirtualenv my_django_environment
```

Dovrebbe vedere un output simile al seguente:

```plain
Running virtualenv with interpreter /usr/bin/python3
# …
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/t_env7/bin/get_env_details
(my_django_environment) ubuntu@ubuntu:~$
```

Ora che è all'interno dell'ambiente virtuale, può installare Django e iniziare a sviluppare.

> [!NOTE]
> Da ora in poi in questo articolo (e in effetti nel modulo) si presume che qualsiasi comando venga eseguito all'interno di un ambiente virtuale Python come quello che abbiamo configurato sopra.

### Utilizzare un ambiente virtuale

Ci sono solo alcuni altri comandi utili che dovrebbe conoscere (ci sono più nella documentazione dello strumento, ma questi sono quelli che userà regolarmente):

- `deactivate` — Esce dall'ambiente virtuale Python corrente
- `workon` — Elenca gli ambienti virtuali disponibili
- `workon name_of_environment` — Attiva l'ambiente virtuale Python specificato
- `rmvirtualenv name_of_environment` — Rimuove l'ambiente specificato.

## Installazione di Django

Una volta creato un ambiente virtuale, e utilizzato `workon` per accedervi, può utilizzare _pip3_ per installare Django.

```bash
# Linux/macOS
python3 -m pip install django~=4.2

# Windows
py -3 -m pip install django~=4.2
```

Può verificare che Django sia installato eseguendo il seguente comando (questo verifica solo che Python possa trovare il modulo Django):

```bash
# Linux/macOS
python3 -m django --version

# Windows
py -3 -m django --version
```

> [!NOTE]
> Se il comando Windows sopra non mostra un modulo django presente, provi:
>
> ```bash
> py -m django --version
> ```
>
> In Windows, gli script di _Python 3_ sono avviati prefissando il comando con `py -3`, anche se questo può variare a seconda della sua specifica installazione.
> Provi a omettere il modificatore `-3` se incontra problemi con i comandi.
> Su Linux/macOS, il comando è `python3.`

> [!WARNING]
> Il resto di questo **modulo** utilizza il comando _Linux_ per invocare Python 3 (`python3`). Se sta lavorando su _Windows_, sostituisca questo prefisso con: `py -3`

## Gestione del codice sorgente con Git e GitHub

Gli strumenti per la gestione del codice sorgente (SCM) e la versione consentono di memorizzare e recuperare in modo affidabile versioni del suo codice sorgente, provare modifiche e condividere il codice tra i suoi esperimenti e il "codice noto e buono" quando necessario.

Ci sono molti diversi strumenti SCM, inclusi git, Mercurial, Perforce, SVN (Subversion), CVS (Concurrent Versions System), ecc., e fonti di hosting SCM cloud come Bitbucket, GitHub e GitLab. Per questo tutorial ospitiamo il nostro codice su [GitHub](https://github.com/), uno dei servizi di hosting di codice sorgente cloud basati più popolari, e utilizziamo lo strumento **git** per gestire il nostro codice sorgente localmente e inviarlo a GitHub quando necessario.

> [!NOTE]
> Utilizzare strumenti SCM è una buona pratica di sviluppo software!
> Queste istruzioni forniscono un'introduzione base a git e GitHub.
> Per saperne di più, consultare [Learning Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

### Concetti chiave

Git (e GitHub) utilizzano repository ("repo") come i livelli superiori "bucket" per memorizzare il codice, dove ogni repo normalmente contiene il codice sorgente per una sola applicazione o modulo. I repository possono essere pubblici, nel qual caso il codice è visibile a tutti su internet, o privati, nel qual caso sono limitati all'organizzazione o al conto utente proprietario.

Tutto il lavoro viene effettuato su un particolare "branch" del codice nel suo repo. Quando desidera eseguire il backup di alcune modifiche su un branch può creare un "commit", che memorizza tutte le modifiche dalla sua ultima commit al branch corrente.

Il repo viene creato con un branch predefinito chiamato "main". Può generare altri branch da questo utilizzando git, che inizialmente hanno tutte le commit del branch originale. Può evolvere i branch separatamente aggiungendo commit e poi successivamente utilizzare una "Pull Request" (PR) su GitHub per unire le modifiche da un branch a un altro. Può anche utilizzare git per passare tra i branch sul suo computer locale, ad esempio per provare cose diverse.

Oltre ai branch, è possibile creare `tag` su qualsiasi branch e successivamente recuperare quel branch in quel punto.

### Creare un account e un repository su GitHub

Per prima cosa creeremo un account su GitHub (questo è gratuito). Poi creiamo e configuriamo un repository denominato "django_local_library" per memorizzare il [sito web della biblioteca locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) mentre lo evolviamo nel resto di questo tutorial.

I passaggi sono:

1. Visiti <https://github.com/> e crei un account.
2. Una volta loggato, clicchi sul link **+** nella barra degli strumenti superiore e selezioni **New repository**.
3. Compili tutti i campi in questo modulo.
   Mentre questi non sono obbligatori, sono fortemente raccomandati.

   - Inserisca un nome per il repository: "django_local_library".
   - Inserisca una nuova descrizione del repository: "Sito web della biblioteca locale scritto in Django".
   - Selezioni "Pubblico" per il repository (l'impostazione predefinita).

     > [!WARNING]
     > Questo renderà visibile _tutto_ il codice sorgente.
     > Si ricordi di non memorizzare credenziali o altro materiale sensibile nel suo repo a meno che non sia privato.

   - Scelga **Python** nella lista di selezione _Add .gitignore_.
   - Scelga la sua licenza preferita nella lista di selezione _Add license_.
     MDN utilizza "Creative Commons Zero v1.0 Universal" per questo esempio.
   - Spunti **Initialize this repository with a README**.

4. Premere **Create repository**.

   Il repository verrà creato, contenente solo i file `README.txt` e `.gitignore`.

### Clonare il repo sul suo computer locale

Ora che il repository ("repo") è stato creato su GitHub, vorremo clonarlo (copiarlo) sul nostro computer locale:

1. Su GitHub, clicchi sul pulsante verde **Code**.
   Nella sezione "Clone", selezioni la scheda "HTTPS" e copi l'URL.
   Se ha utilizzato il nome del repository "django_local_library", l'URL dovrebbe essere simile a: `https://github.com/<suo_id_utente_di_git>/django_local_library.git`.

2. Installare _git_ per il suo computer locale ([guida ufficiale al download di Git](https://git-scm.com/downloads)).
3. Aprire un prompt dei comandi/terminale e clonare il suo repo utilizzando l'URL copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/django_local_library.git
   ```

   Questo creerà il repository all'interno della directory corrente.

4. Navigare nella cartella del repo.

   ```bash
   cd django_local_library
   ```

### Modificare e sincronizzare le modifiche

Ora modificheremo il file `.gitignore` sul computer locale, faremo il commit della modifica e aggiorneremo il repository su GitHub. Questa è una modifica utile da fare, ma principalmente la stiamo facendo per mostrarLe come estrarre modifiche da GitHub, fare modifiche localmente e quindi inviarle a GitHub.

1. Nel prompt dei comandi/terminale dobbiamo prima "fetch" (ottenere) e poi "pull" (ottenere e unire nel branch corrente) l'ultima versione del sorgente da GitHub:

   > [!NOTE]
   > Questo passaggio non è strettamente necessario dal momento che abbiamo appena clonato il sorgente e sappiamo che è aggiornato.
   > Tuttavia, in generale, dovrebbe aggiornare i suoi sorgenti da GitHub prima di apportare modifiche.

   ```bash
   git fetch origin main
   git pull origin main
   ```

   L' "origin" è un _remote_, che rappresenta la posizione del repo dove è situato il codice sorgente, e "main" è il branch. Può verificare che l'origine sia il nostro repo su GitHub utilizzando il comando: `git remote -v`.

2. Successivamente passiamo a un nuovo branch per memorizzare le nostre modifiche:

   ```bash
   git checkout -b update_gitignore
   ```

   Il comando `checkout` viene utilizzato per passare tra alcuni branch per essere il branch corrente su cui sta lavorando. Il flag `-b` indica che intendiamo creare un nuovo branch chiamato "update_gitignore" anziché selezionare un branch esistente con quel nome.

3. Aprire il file **.gitignore**, copiare le seguenti righe alla fine di esso e quindi salvare:

   ```plain
   # Text backup files
   *.bak

   # Database
   *.sqlite3
   ```

   Si noti che `.gitignore` viene utilizzato per indicare file che non dovrebbero essere salvati automaticamente da git, come file temporanei e altri artefatti di build.

4. Utilizzare il comando `add` per aggiungere tutti i file modificati (non ignorati dal file **.gitignore**) all' "area di staging" per il branch corrente.

   ```bash
   git add -A
   ```

5. Utilizzare il comando `status` per controllare che tutti i file che sta per `commit` siano corretti (vuole includere i file sorgente, non binari, file temporanei ecc.). Dovrebbe apparire un po' come l'elenco di seguito.

   ```bash
   > git status
   On branch main
   Your branch is up-to-date with 'origin/update_gitignore'.
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

           modified:   .gitignore
   ```

6. Quando è soddisfatto, `commit` i file nel suo repository locale, utilizzando il flag `-m` per specificare un messaggio di commit conciso ma chiaro. Questo è equivalente a firmare le modifiche e renderle una parte ufficiale del repository locale.

   ```bash
   git commit -m ".gitignore: add .bak and .sqlite3"
   ```

7. A questo punto, il repository remoto non è stato modificato. Possiamo inviare il branch `update_gitignore` al repo "origin" (GitHub) utilizzando il seguente comando:

   ```bash
   git push origin update_gitignore
   ```

8. Torni alla pagina su GitHub dove ha creato il suo repo e aggiorni la pagina.

   Dovrebbe apparire un banner con un pulsante per premere se desidera "Compare and pull request" il branch che ha appena caricato. Selezioni il pulsante e segua quindi le istruzioni per creare e quindi unire una richiesta pull.

   ![Banner che chiede se l'utente desidera confrontare e unire aggiornamenti recenti del branch](github_compare_and_pull_banner.png)

   Dopo l'unione, il branch "main" sul repo su GitHub conterrà le sue modifiche a `.gitignore`.

9. Può continuare ad aggiornare il suo repo locale man mano che i file cambiano utilizzando questo ciclo add/commit/push.

Nel prossimo argomento utilizzeremo questo repo per memorizzare il codice sorgente del nostro sito web della biblioteca locale.

## Altri strumenti Python

Gli sviluppatori Python esperti possono installare strumenti aggiuntivi, come linters (che aiutano a rilevare errori comuni nel codice).

Si noti che dovrebbe utilizzare un linter consapevole di Django come [pylint-django](https://pypi.org/project/pylint-django/), poiché alcuni linters Python comuni (come `pylint`) segnalano erroneamente errori nei file standard generati per Django.

## Testare la sua installazione

Il test precedente funziona, ma non è molto divertente. Un test più interessante è creare un progetto scheletrico e vederlo funzionare. Per fare ciò, prima si sposti nel prompt dei comandi/terminale dove desidera memorizzare le sue applicazioni Django. Crei una cartella per il suo sito di test e ci vada dentro.

```bash
mkdir django_test
cd django_test
```

Può quindi creare un nuovo sito scheletrico chiamato "_mytestsite_" utilizzando lo strumento **django-admin** come mostrato. Dopo aver creato il sito può navigare nella cartella dove troverà lo script principale per gestire i progetti, chiamato **manage.py**.

```bash
django-admin startproject mytestsite
cd mytestsite
```

Possiamo eseguire _il server web di sviluppo_ all'interno di questa cartella utilizzando **manage.py** e il comando `runserver`, come mostrato.

```bash
# Linux/macOS
python3 manage.py runserver

# Windows
py -3 manage.py runserver
```

> [!NOTE]
> Può ignorare gli avvisi su "migrationi non applicate" a questo punto!

Una volta che il server sta funzionando, può vedere il sito navigando al seguente URL nel suo browser web locale: `http://127.0.0.1:8000/`. Dovrebbe vedere un sito che appare così:

![La home page dell'app scheletro Django](django_skeleton_app_homepage_django_4_0.png)

## Sommario

Ora ha un ambiente di sviluppo Django configurato e funzionante sul suo computer.

Nella sezione di test ha visto anche brevemente come possiamo creare un nuovo sito web Django utilizzando `django-admin startproject` e visualizzarlo nel browser utilizzando il server web di sviluppo (`python3 manage.py runserver`). Nel prossimo articolo, espanderemo questo processo, costruendo un'applicazione web semplice ma completa.

## Vedere anche

- [Guida rapida all'installazione](https://docs.djangoproject.com/en/5.0/intro/install/) (Documentazione Django)
- [Come installare Django — Guida completa](https://docs.djangoproject.com/en/5.0/topics/install/) (Documentazione Django) — tratta anche di come rimuovere Django
- [Come installare Django su Windows](https://docs.djangoproject.com/en/5.0/howto/windows/) (Documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}
