---
title: Configurazione di un ambiente di sviluppo Django
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Django/development_environment
l10n:
  sourceCommit: 324c613947adaa5e19ad0f409c5f4c535ee8cf6b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che si conosce lo scopo di Django, verrà mostrato come configurare e testare un ambiente di sviluppo Django su Windows, Linux (Ubuntu) e macOS: indipendentemente dal sistema operativo comune in uso, questo articolo dovrebbe fornire tutto il necessario per iniziare a sviluppare app Django.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base dell'uso di un terminale/riga di comando e dell'installazione di pacchetti software sul sistema operativo del computer di sviluppo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Avere un ambiente di sviluppo per Django (4.*) in esecuzione sul proprio computer.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica dell'ambiente di sviluppo Django

Django semplifica molto la configurazione del proprio computer per iniziare a sviluppare applicazioni web. Questa sezione spiega cosa offre l'ambiente di sviluppo e fornisce una panoramica di alcune opzioni di installazione e configurazione. Il resto dell'articolo illustra il metodo _consigliato_ per installare l'ambiente di sviluppo Django su Ubuntu, macOS e Windows, e come testarlo.

### Cos'è l'ambiente di sviluppo Django?

L'ambiente di sviluppo è un'installazione di Django sul computer locale che può essere usata per sviluppare e testare app Django prima di distribuirle in un ambiente di produzione.

Gli strumenti principali forniti da Django stesso sono un insieme di script Python per creare e gestire progetti Django, insieme a un semplice _server web di sviluppo_ che può essere usato per testare nel browser web del computer applicazioni web Django locali (cioè sul computer, non su un server web esterno).

Esistono altri strumenti periferici, che spesso fanno parte dell'ambiente di sviluppo, ma che non verranno trattati qui. Questi includono elementi come un [editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o un IDE per modificare il codice, linter per la formattazione automatica e così via. Si presume che sia già installato un editor di testo.

### Quali sono le opzioni di configurazione di Django?

Django è estremamente flessibile in termini di modalità e luogo di installazione e configurazione. Django può essere:

- Installato su diversi sistemi operativi.
- Installato dal sorgente, dal Python Package Index (PyPi) e, in molti casi, dall'applicazione per la gestione dei pacchetti del computer host.
- Configurato per usare uno dei vari database, che potrebbero anch'essi dover essere installati e configurati separatamente.
- Eseguito nell'ambiente Python principale del sistema oppure in ambienti virtuali Python separati.

Ciascuna di queste opzioni richiede una configurazione e un'installazione leggermente diverse. Le sottosezioni seguenti spiegano alcune delle possibilità disponibili. Nel resto dell'articolo verrà mostrato come configurare Django su un numero limitato di sistemi operativi; tale configurazione sarà considerata valida per tutto il resto di questo modulo.

> [!NOTE]
> Altre possibili opzioni di installazione sono trattate nella documentazione ufficiale di Django. I collegamenti ai [documenti pertinenti sono riportati di seguito](#vedere_anche).

#### Quali sistemi operativi sono supportati?

Le applicazioni web Django possono essere eseguite su quasi tutte le macchine in grado di eseguire il linguaggio di programmazione Python 3: Windows, macOS, Linux/Unix, Solaris, solo per citarne alcuni.
Quasi tutti i computer dovrebbero avere le prestazioni necessarie per eseguire Django durante lo sviluppo.

In questo articolo verranno fornite istruzioni per Windows, macOS e Linux/Unix.

#### Quale versione di Python dovrebbe essere usata?

È possibile usare qualsiasi versione di Python supportata dalla versione di Django di destinazione.
Per Django 5.0 le versioni consentite vanno da Python 3.10 a 3.12 (vedere [FAQ:Installation](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django)).

Il progetto Django _consiglia_ (e "supporta ufficialmente") l'uso della versione più recente disponibile della release Python supportata.

#### Dove è possibile scaricare Django?

Esistono tre luoghi da cui scaricare Django:

- Il Python Package Repository (PyPi), usando lo strumento _pip_. Questo è il modo migliore per ottenere l'ultima versione stabile di Django.
- Usare una versione dal gestore di pacchetti del computer. Le distribuzioni di Django incluse con i sistemi operativi offrono un meccanismo di installazione familiare. Si noti tuttavia che la versione pacchettizzata potrebbe essere piuttosto vecchia e può essere installata solo nell'ambiente Python di sistema, che potrebbe non essere ciò che si desidera.
- Installare dal sorgente. È possibile ottenere e installare dal sorgente l'ultima versione all'avanguardia di Django. Questo non è consigliato ai principianti, ma è necessario quando si è pronti a iniziare a contribuire a Django stesso.

Questo articolo mostra come installare Django da PyPi, in modo da ottenere l'ultima versione stabile.

#### Quale database?

Django supporta ufficialmente i database PostgreSQL, MariaDB, MySQL, Oracle e SQLite; inoltre, esistono librerie della comunità che forniscono vari livelli di supporto per altri database SQL e NoSQL diffusi. Si consiglia di selezionare lo stesso database sia per la produzione sia per lo sviluppo (sebbene Django astragga molte differenze tra database tramite il suo Object-Relational Mapper (ORM), esistono comunque [potenziali problemi](https://docs.djangoproject.com/en/5.0/ref/databases/) che è meglio evitare).

Per questo articolo (e per la maggior parte di questo modulo) verrà usato il database _SQLite_, che memorizza i propri dati in un file. SQLite è pensato per essere usato come database leggero e non può supportare un alto livello di concorrenza. Tuttavia, è un'ottima scelta per applicazioni principalmente in sola lettura.

> [!NOTE]
> Django è configurato per usare SQLite per impostazione predefinita quando si avvia un progetto di sito web usando gli strumenti standard (_django-admin_). È un'ottima scelta quando si inizia perché non richiede configurazioni o installazioni aggiuntive.

#### Installazione a livello di sistema o in un ambiente virtuale Python?

Quando si installa Python3, si ottiene un singolo ambiente globale condiviso da tutto il codice Python3. Sebbene sia possibile installare nell'ambiente tutti i pacchetti Python desiderati, è possibile installare una sola versione specifica di ciascun pacchetto alla volta.

> [!NOTE]
> Le applicazioni Python installate nell'ambiente globale possono potenzialmente entrare in conflitto tra loro, ad esempio se dipendono da versioni diverse dello stesso pacchetto.

Se Django viene installato nell'ambiente predefinito/globale, sarà possibile scegliere come destinazione una sola versione di Django sul computer. Questo può rappresentare un problema se si desidera creare nuovi siti web, usando l'ultima versione di Django, continuando al tempo stesso a mantenere siti web che dipendono da versioni precedenti.

Di conseguenza, gli sviluppatori Python/Django esperti eseguono in genere le app Python in _ambienti virtuali Python_ indipendenti. Questo consente di avere più ambienti Django diversi su un singolo computer. Il team di sviluppo di Django stesso consiglia di usare gli ambienti virtuali Python.

Questo modulo presuppone che Django sia stato installato in un ambiente virtuale e verrà mostrato come farlo di seguito.

## Installazione di Python 3

Per usare Django, Python 3 deve essere presente sul sistema operativo.
Sarà inoltre necessario lo strumento [Python Package Index](https://pypi.org/) — _pip3_ — usato per gestire (installare, aggiornare e rimuovere) i pacchetti/librerie Python usati da Django e dalle altre app Python.

Questa sezione spiega brevemente come controllare quali versioni di Python sono presenti e come installare nuove versioni se necessario, per Ubuntu Linux 20.04, macOS e Windows 10.

> [!NOTE]
> A seconda della piattaforma, potrebbe essere possibile installare Python/pip anche dal gestore di pacchetti del sistema operativo o tramite altri meccanismi. Per la maggior parte delle piattaforme, è possibile scaricare i file di installazione richiesti da <https://www.python.org/downloads/> e installarli usando il metodo specifico appropriato per la piattaforma.

### Ubuntu 22.04

Ubuntu Linux 22.04 LTS include Python 3.10.12 per impostazione predefinita.
È possibile confermarlo eseguendo il comando seguente nel terminale bash:

```bash
python3 -V
# Output: Python 3.10.12
```

Tuttavia, lo strumento Python Package Index (_pip3_) necessario per installare pacchetti per Python 3, incluso Django, **non** è disponibile per impostazione predefinita.
È possibile installare _pip3_ nel terminale bash usando:

```bash
sudo apt install python3-pip
```

> [!NOTE]
> Python 3.10 è la versione più vecchia [supportata da Django 5.0](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django).
> Non è _necessario_ usare l'ultima versione di Python per questo tutorial, ma se lo si desidera, sono disponibili istruzioni su internet.

### macOS

macOS non include Python 3 per impostazione predefinita (Python 2 è incluso nelle versioni meno recenti).
È possibile confermarlo eseguendo il comando seguente nel terminale:

```bash
python3 -V
```

Verrà visualizzato il numero di versione di Python, a indicare che Python 3 è installato, oppure `python3: command not found`, a indicare che Python 3 non è stato trovato.

È possibile installare facilmente Python 3, insieme allo strumento _pip3_, da [python.org](https://www.python.org/):

1. Scaricare il programma di installazione richiesto:
   1. Andare a <https://www.python.org/downloads/macos/>
   2. Scaricare la release stabile della più recente [versione supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) che funziona con Django 5.0.
      (al momento della stesura, si tratta di Python 3.11.8).

2. Individuare il file usando _Finder_ e fare doppio clic sul file del pacchetto. Seguire le istruzioni di installazione.

Ora è possibile confermare che l'installazione è riuscita eseguendo nuovamente `python3 -V` e controllando il numero di versione di Python.

Analogamente, è possibile verificare che _pip3_ sia installato elencando i pacchetti disponibili:

```bash
pip3 list
```

### Windows 10 o 11

Windows non include Python per impostazione predefinita, ma è possibile installarlo facilmente, insieme allo strumento _pip3_, da [python.org](https://www.python.org/):

1. Scaricare il programma di installazione richiesto:
   1. Andare a <https://www.python.org/downloads/windows/>
   2. Scaricare la release stabile della più recente [versione supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) che funziona con Django 5.0.
      (al momento della stesura, si tratta di Python 3.11.8).

2. Installare Python facendo doppio clic sul file scaricato e seguendo le istruzioni di installazione.
3. Assicurarsi di selezionare la casella denominata "Add Python to PATH".

È quindi possibile verificare che Python 3 sia stato installato inserendo il testo seguente nel prompt dei comandi:

```bash
py -3 -V
```

Il programma di installazione di Windows include _pip3_, il gestore di pacchetti Python, per impostazione predefinita.
È possibile elencare i pacchetti installati come mostrato:

```bash
py -3 -m pip list
```

> [!NOTE]
> Il programma di installazione dovrebbe configurare tutto il necessario affinché il comando precedente funzioni.
> Tuttavia, se viene visualizzato un messaggio che indica che Python non può essere trovato, potrebbe essere stato dimenticato di aggiungerlo al percorso di sistema.
> È possibile farlo eseguendo di nuovo il programma di installazione, selezionando "Modify" e selezionando la casella denominata "Add Python to environment variables" nella seconda pagina.

## Richiamare Python 3 e pip3

Nelle sezioni precedenti vengono usati comandi diversi per richiamare Python 3 e pip su sistemi operativi diversi.

Se è installato solo Python 3, e non Python 2, i comandi semplici `python` e `pip` possono generalmente essere usati per eseguire Python e pip su qualsiasi sistema operativo.
Se il sistema lo consente, eseguendo `-V` con i comandi semplici verrà ottenuta una stringa di versione "3", come mostrato:

```bash
python -V
pip -V
```

Se Python 2 è installato, per usare la versione 3 è necessario anteporre ai comandi `python3` e `pip3` su Linux/macOS, e `py -3` e `py -3 -m pip` su Windows:

```bash
# Linux/macOS
python3 -V
pip3 -V

# Windows
py -3 -V
py -3 -m pip list
```

Le istruzioni seguenti mostrano i comandi specifici della piattaforma, poiché funzionano su un maggior numero di sistemi.

## Usare Django in un ambiente virtuale Python

Le librerie che verranno usate per creare gli ambienti virtuali sono [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) per Linux e macOS e [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) per Windows; entrambe usano a loro volta lo strumento [virtualenv](https://virtualenv.pypa.io/en/latest/). Gli strumenti wrapper creano un'interfaccia coerente per gestire gli ambienti su tutte le piattaforme.

### Installazione del software per gli ambienti virtuali

#### Configurazione dell'ambiente virtuale Ubuntu

Dopo aver installato Python e pip, è possibile installare _virtualenvwrapper_, che include _virtualenv_. È possibile consultare [la guida di installazione ufficiale](https://virtualenvwrapper.readthedocs.io/en/latest/install.html) oppure seguire le istruzioni riportate di seguito.

Installare lo strumento usando _pip3_:

```bash
sudo pip3 install virtualenvwrapper
```

Quindi aggiungere le righe seguenti alla fine del file di avvio della shell, un file nascosto denominato **.bashrc** nella directory home. Queste impostano la posizione in cui devono risiedere gli ambienti virtuali, la posizione delle directory dei progetti di sviluppo e la posizione dello script installato con questo pacchetto:

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS=' -p /usr/bin/python3 '
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> Le variabili `VIRTUALENVWRAPPER_PYTHON` e `VIRTUALENVWRAPPER_VIRTUALENV_ARGS` puntano alla posizione di installazione normale di Python 3, mentre `source /usr/local/bin/virtualenvwrapper.sh` punta alla posizione normale dello script `virtualenvwrapper.sh`. Se _virtualenv_ non funziona durante il test, una cosa da controllare è che Python e lo script si trovino nella posizione prevista, quindi modificare il file di avvio in modo appropriato.
>
> È possibile trovare le posizioni corrette per il sistema usando i comandi `which virtualenvwrapper.sh` e `which python3`.

Quindi ricaricare il file di avvio eseguendo il comando seguente nel terminale:

```bash
source ~/.bashrc
```

A questo punto dovrebbe essere visualizzato un insieme di script in esecuzione, come mostrato di seguito:

```bash
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/premkproject
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postmkproject
# …
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/preactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/get_env_details
```

Ora è possibile creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

#### Configurazione dell'ambiente virtuale macOS

La configurazione di _virtualenvwrapper_ su macOS è quasi esattamente la stessa di Ubuntu. È possibile seguire le istruzioni della [guida di installazione ufficiale](https://virtualenvwrapper.readthedocs.io/en/latest/install.html) oppure quelle riportate di seguito.

Installare _virtualenvwrapper_, che include _virtualenv_, usando _pip_ come mostrato.

```bash
sudo pip3 install virtualenvwrapper
```

Quindi aggiungere le righe seguenti alla fine del file di avvio della shell, le stesse righe usate per Ubuntu.
Se viene usata la _zsh shell_, il file di avvio sarà un file nascosto denominato **.zshrc** nella directory home. Se viene usata la _bash shell_, sarà un file nascosto denominato **.bash_profile**. Potrebbe essere necessario creare il file se non esiste ancora.

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> La variabile `VIRTUALENVWRAPPER_PYTHON` punta alla posizione di installazione normale di Python 3, mentre `source /usr/local/bin/virtualenvwrapper.sh` punta alla posizione normale dello script `virtualenvwrapper.sh`. Se _virtualenv_ non funziona durante il test, una cosa da controllare è che Python e lo script si trovino nella posizione prevista, quindi modificare il file di avvio in modo appropriato.
>
> Ad esempio, un test di installazione su macOS ha richiesto le righe seguenti nel file di avvio:
>
> ```bash
> export WORKON_HOME=$HOME/.virtualenvs
> export VIRTUALENVWRAPPER_PYTHON=/Library/Frameworks/Python.framework/Versions/3.7/bin/python3
> export PROJECT_HOME=$HOME/Devel
> source /Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh
> ```
>
> È possibile trovare le posizioni corrette per il sistema usando i comandi `which virtualenvwrapper.sh` e `which python3`.

Quindi ricaricare il file di avvio effettuando la chiamata seguente nel terminale:

```bash
source ~/.bash_profile
```

A questo punto potrebbe essere visualizzato un insieme di script in esecuzione, gli stessi script dell'installazione Ubuntu. Ora dovrebbe essere possibile creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

> [!NOTE]
> Se non è possibile trovare il file di avvio da modificare nel Finder, è possibile aprirlo anche nel terminale usando nano.
>
> Supponendo di usare bash, i comandi sono simili ai seguenti:
>
> ```bash
> cd ~  # Navigate to my home directory
> ls -la #List the content of the directory. You should see .bash_profile
> nano .bash_profile # Open the file in the nano text editor, within the terminal
> # Scroll to the end of the file, and copy in the lines above
> # Use Ctrl+X to exit nano, choose Y to save the file.
> ```

#### Configurazione dell'ambiente virtuale Windows

L'installazione di [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) è ancora più semplice della configurazione di _virtualenvwrapper_ perché non è necessario configurare dove lo strumento memorizza le informazioni sugli ambienti virtuali, poiché esiste un valore predefinito. È sufficiente eseguire il comando seguente nel prompt dei comandi:

```bash
py -3 -m pip install virtualenvwrapper-win
```

Ora è possibile creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

### Creazione di un ambiente virtuale

Dopo aver installato _virtualenvwrapper_ o _virtualenvwrapper-win_, il lavoro con gli ambienti virtuali è molto simile su tutte le piattaforme.

Ora è possibile creare un nuovo ambiente virtuale con il comando `mkvirtualenv`. Durante l'esecuzione del comando verrà visualizzata la configurazione dell'ambiente, che è leggermente specifica della piattaforma. Al termine del comando, il nuovo ambiente virtuale sarà attivo: ciò è visibile perché l'inizio del prompt conterrà il nome dell'ambiente tra parentesi. Di seguito viene mostrato questo comportamento per Ubuntu, ma la riga finale è simile per Windows/macOS.

```bash
mkvirtualenv my_django_environment
```

Dovrebbe essere visualizzato un output simile al seguente:

```plain
Running virtualenv with interpreter /usr/bin/python3
# …
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/t_env7/bin/get_env_details
(my_django_environment) ubuntu@ubuntu:~$
```

Ora che ci si trova nell'ambiente virtuale, è possibile installare Django e iniziare lo sviluppo.

> [!NOTE]
> Da questo punto in poi nell'articolo, e nel modulo in generale, si consideri che tutti i comandi vengano eseguiti all'interno di un ambiente virtuale Python come quello configurato in precedenza.

### Uso di un ambiente virtuale

Esistono solo alcuni altri comandi utili da conoscere. Ce ne sono altri nella documentazione dello strumento, ma questi sono quelli usati regolarmente:

- `deactivate` — Esce dall'ambiente virtuale Python corrente.
- `workon` — Elenca gli ambienti virtuali disponibili.
- `workon name_of_environment` — Attiva l'ambiente virtuale Python specificato.
- `rmvirtualenv name_of_environment` — Rimuove l'ambiente specificato.

## Installazione di Django

Dopo aver creato un ambiente virtuale e aver richiamato `workon` per accedervi, è possibile usare _pip3_ per installare Django.

```bash
# Linux/macOS
python3 -m pip install django~=4.2

# Windows
py -3 -m pip install django~=4.2
```

È possibile verificare che Django sia installato eseguendo il comando seguente; questo verifica semplicemente che Python riesca a trovare il modulo Django:

```bash
# Linux/macOS
python3 -m django --version

# Windows
py -3 -m django --version
```

> [!NOTE]
> Se il comando Windows precedente non mostra la presenza di un modulo django, provare:
>
> ```bash
> py -m django --version
> ```
>
> In Windows gli script _Python 3_ vengono avviati anteponendo al comando `py -3`, anche se ciò può variare a seconda dell'installazione specifica.
> Provare a omettere il modificatore `-3` se si riscontrano problemi con i comandi.
> In Linux/macOS, il comando è `python3`.

> [!WARNING]
> Il resto di questo **modulo** usa il comando _Linux_ per richiamare Python 3 (`python3`). Se si lavora su _Windows_, sostituire questo prefisso con: `py -3`

## Gestione del codice sorgente con Git e GitHub

Gli strumenti di gestione del codice sorgente (SCM) e di versionamento consentono di archiviare e recuperare in modo affidabile le versioni del codice sorgente, provare modifiche e condividere il codice tra esperimenti e "codice noto come funzionante" quando necessario.

Esistono molti strumenti SCM diversi, tra cui git, Mercurial, Perforce, SVN (Subversion), CVS (Concurrent Versions System) e così via, nonché servizi cloud di hosting SCM come Bitbucket, GitHub e GitLab.
Per questo tutorial, il codice verrà ospitato su [GitHub](https://github.com/), uno dei più popolari servizi cloud per l'hosting del codice sorgente, e verrà usato lo strumento **git** per gestire il codice sorgente localmente e inviarlo a GitHub quando necessario.

> [!NOTE]
> L'uso di strumenti SCM è una buona pratica nello sviluppo software.
> Queste istruzioni forniscono un'introduzione di base a git e GitHub.
> Per ulteriori informazioni, vedere [Learning Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

### Concetti chiave

Git, e GitHub, usano i repository, o "repo", come contenitore di livello superiore per archiviare il codice; ogni repo contiene normalmente il codice sorgente di una sola applicazione o modulo.
I repository possono essere pubblici, nel qual caso il codice è visibile a tutti su internet, oppure privati, nel qual caso sono limitati all'organizzazione o all'account utente proprietario.

Tutto il lavoro viene svolto su uno specifico "branch" di codice nel repo.
Quando si desidera eseguire il backup di alcune modifiche a un branch, è possibile creare un "commit", che memorizza tutte le modifiche dalla precedente commit nel branch corrente.

Il repo viene creato con un branch predefinito denominato "main". Usando git è possibile creare altri branch a partire da questo, che inizialmente contengono tutti i commit del branch originale.
È possibile sviluppare i branch separatamente aggiungendo commit, quindi usare in seguito una "Pull Request" (PR) su GitHub per unire le modifiche da un branch a un altro.
È inoltre possibile usare git per passare da un branch all'altro sul computer locale, ad esempio per provare cose diverse.

Oltre ai branch, è possibile creare `tags` su qualsiasi branch e recuperare in seguito quel branch a quel punto.

### Creare un account e un repository su GitHub

Per prima cosa verrà creato un account su GitHub, che è gratuito.
Quindi verrà creato e configurato un repository denominato "django_local_library" per memorizzare il [sito web della biblioteca locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) durante lo sviluppo nel resto di questo tutorial.

I passaggi sono:

1. Visitare <https://github.com/> e creare un account.
2. Dopo aver effettuato l'accesso, fare clic sul collegamento **+** nella barra degli strumenti superiore e selezionare **New repository**.
3. Compilare tutti i campi di questo modulo.
   Sebbene non siano obbligatori, sono fortemente consigliati.
   - Inserire un nome per il repository: "django_local_library".
   - Inserire una nuova descrizione per il repository: "Local Library website written in Django".
   - Selezionare "Public" per il repository, che è l'impostazione predefinita.

     > [!WARNING]
     > Questo renderà visibile _tutto_ il codice sorgente.
     > Ricordare di non memorizzare credenziali o altro materiale sensibile nel repo, a meno che non sia privato.

   - Scegliere **Python** nell'elenco di selezione _Add .gitignore_.
   - Scegliere la licenza preferita nell'elenco di selezione _Add license_.
     MDN usa "Creative Commons Zero v1.0 Universal" per questo esempio.
   - Selezionare **Initialize this repository with a README**.

4. Premere **Create repository**.

   Il repository verrà creato e conterrà solo i file `README.txt` e `.gitignore`.

### Clonare il repo sul computer locale

Ora che il repository, o "repo", è stato creato su GitHub, sarà necessario clonarlo, cioè copiarlo, sul computer locale:

1. Su GitHub, fare clic sul pulsante verde **Code**.
   Nella sezione "Clone", selezionare la scheda "HTTPS" e copiare l'URL.
   Se è stato usato il nome del repository "django_local_library", l'URL dovrebbe essere simile a: `https://github.com/<your_git_user_id>/django_local_library.git`.

2. Installare _git_ per il computer locale ([guida ufficiale per il download di Git](https://git-scm.com/downloads/)).
3. Aprire un prompt dei comandi/terminale e clonare il repo usando l'URL copiato in precedenza:

   ```bash
   git clone https://github.com/<your_git_user_id>/django_local_library.git
   ```

   Questo creerà il repository all'interno della directory corrente.

4. Passare alla cartella del repo.

   ```bash
   cd django_local_library
   ```

### Modificare e sincronizzare le modifiche

Ora verrà modificato il file `.gitignore` sul computer locale, verrà eseguito il commit della modifica e verrà aggiornato il repository su GitHub.
Questa è una modifica utile da effettuare, ma soprattutto serve a mostrare come recuperare modifiche da GitHub, apportare modifiche localmente e poi inviarle a GitHub.

1. Nel prompt dei comandi/terminale, prima si esegue un "fetch", cioè si ottiene, e poi un pull, cioè si ottengono e si uniscono al branch corrente, dell'ultima versione del sorgente da GitHub:

   > [!NOTE]
   > Questo passaggio non è strettamente necessario, poiché il sorgente è appena stato clonato e si sa che è aggiornato.
   > Tuttavia, in generale è opportuno aggiornare i sorgenti da GitHub prima di apportare modifiche.

   ```bash
   git fetch origin main
   git pull origin main
   ```

   L'"origin" è un _remote_, che rappresenta la posizione del repo in cui si trova il sorgente, mentre "main" è il branch.
   È possibile verificare che origin sia il repo su GitHub usando il comando: `git remote -v`.

2. Successivamente, viene effettuato il checkout di un nuovo branch per memorizzare le modifiche:

   ```bash
   git checkout -b update_gitignore
   ```

   Il comando `checkout` viene usato per passare a un branch come branch corrente su cui si sta lavorando.
   Il flag `-b` indica che si intende creare un nuovo branch denominato "update_gitignore" invece di selezionare un branch esistente con quel nome.

3. Aprire il file **.gitignore**, copiare le righe seguenti nella parte inferiore del file, quindi salvare:

   ```plain
   # Text backup files
   *.bak

   # Database
   *.sqlite3
   ```

   Si noti che `.gitignore` viene usato per indicare i file che non devono essere salvati automaticamente da git, come file temporanei e altri artefatti di build.

4. Usare il comando `add` per aggiungere tutti i file modificati, che non sono ignorati dal file **.gitignore**, alla "staging area" del branch corrente.

   ```bash
   git add -A
   ```

5. Usare il comando `status` per controllare che tutti i file su cui si sta per eseguire il `commit` siano corretti: devono essere inclusi file sorgente, non file binari, file temporanei e così via.
   Dovrebbe assomigliare all'elenco seguente.

   ```bash
   git status
   ```

   ```plain
   On branch update_gitignore
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)

           modified:   .gitignore
   ```

6. Quando si è soddisfatti, eseguire il `commit` dei file nel repo locale, usando il flag `-m` per specificare un messaggio di commit conciso ma chiaro.
   Questo equivale ad approvare le modifiche e renderle parte ufficiale del repo locale.

   ```bash
   git commit -m ".gitignore: add .bak and .sqlite3"
   ```

7. A questo punto, il repo remoto non è stato modificato.
   È possibile inviare il branch `update_gitignore` al repo "origin", cioè GitHub, usando il comando seguente:

   ```bash
   git push origin update_gitignore
   ```

8. Tornare alla pagina su GitHub in cui è stato creato il repo e aggiornare la pagina.

   Dovrebbe apparire un banner con un pulsante da premere per "Compare and pull request" il branch appena caricato.
   Selezionare il pulsante, quindi seguire le istruzioni per creare e poi unire una pull request.

   ![Banner che chiede se l'utente desidera confrontare e unire gli aggiornamenti recenti del branch](github_compare_and_pull_banner.png)

   Dopo l'unione, il branch "main" del repo su GitHub conterrà le modifiche apportate a `.gitignore`.

9. È possibile continuare ad aggiornare il repo locale man mano che i file cambiano usando questo ciclo add/commit/push.

Nel prossimo argomento verrà usato questo repo per memorizzare il codice sorgente del sito web della biblioteca locale.

## Altri strumenti Python

Gli sviluppatori Python esperti possono installare strumenti aggiuntivi, come i linter, che aiutano a rilevare errori comuni nel codice.

Si noti che dovrebbe essere usato un linter compatibile con Django, come [pylint-django](https://pypi.org/project/pylint-django/), perché alcuni linter Python comuni, come `pylint`, segnalano erroneamente errori nei file standard generati per Django.

## Test dell'installazione

Il test precedente funziona, ma non è particolarmente interessante. Un test più interessante consiste nel creare un progetto scheletro e vederlo in funzione. Per farlo, passare prima nel prompt dei comandi/terminale alla posizione in cui si desidera memorizzare le app Django. Creare una cartella per il sito di test e accedervi.

```bash
mkdir django_test
cd django_test
```

È quindi possibile creare un nuovo sito scheletro denominato "_mytestsite_" usando lo strumento **django-admin**, come mostrato. Dopo aver creato il sito, è possibile passare alla cartella in cui si trova lo script principale per gestire i progetti, denominato **manage.py**.

```bash
django-admin startproject mytestsite
cd mytestsite
```

È possibile eseguire il _server web di sviluppo_ da questa cartella usando **manage.py** e il comando `runserver`, come mostrato.

```bash
# Linux/macOS
python3 manage.py runserver

# Windows
py -3 manage.py runserver
```

> [!NOTE]
> A questo punto è possibile ignorare gli avvisi relativi a "unapplied migration(s)".

Dopo l'avvio del server, è possibile visualizzare il sito accedendo al seguente URL nel browser web locale: `http://127.0.0.1:8000/`. Dovrebbe essere visibile un sito simile a questo:

![La home page dell'app Django scheletro](django_skeleton_app_homepage_django_4_0.png)

## Riepilogo

Ora sul computer è disponibile un ambiente di sviluppo Django funzionante.

Nella sezione di test è stato inoltre brevemente mostrato come creare un nuovo sito web Django usando `django-admin startproject` ed eseguirlo nel browser usando il server web di sviluppo (`python3 manage.py runserver`). Nel prossimo articolo, questo processo verrà approfondito creando un'applicazione web semplice ma completa.

## Vedere anche

- [Guida all'installazione rapida](https://docs.djangoproject.com/en/5.0/intro/install/) (documentazione Django)
- [Come installare Django — Guida completa](https://docs.djangoproject.com/en/5.0/topics/install/) (documentazione Django) — include anche come rimuovere Django
- [Come installare Django su Windows](https://docs.djangoproject.com/en/5.0/howto/windows/) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}
