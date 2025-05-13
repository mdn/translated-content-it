---
title: Configurare un ambiente di sviluppo Django
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Django/development_environment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che sa a cosa serve Django, le mostreremo come configurare e testare un ambiente di sviluppo Django su Windows, Linux (Ubuntu) e macOS — qualunque sistema operativo comune stia usando, questo articolo dovrebbe fornirle ciò di cui ha bisogno per iniziare a sviluppare applicazioni Django.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base dell'uso di un terminale/linea di comando e di come installare pacchetti software sul sistema operativo del computer di sviluppo.
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

Django rende molto facile configurare il proprio computer in modo che si possa iniziare a sviluppare applicazioni web. Questa sezione spiega cosa si ottiene con l'ambiente di sviluppo e fornisce una panoramica di alcune delle opzioni di configurazione e impostazione disponibili. Il resto dell'articolo spiega il metodo _raccomandato_ per installare l'ambiente di sviluppo Django su Ubuntu, macOS e Windows, e come testarlo.

### Cos'è l'ambiente di sviluppo Django?

L'ambiente di sviluppo è un'installazione di Django sul suo computer locale che può utilizzare per sviluppare e testare le applicazioni Django prima di distribuirle in un ambiente di produzione.

Gli strumenti principali che Django fornisce sono una serie di script Python per creare e lavorare con i progetti Django, insieme a un semplice _server web di sviluppo_ che si può usare per testare le applicazioni web Django locali (cioè, sul computer e non su un server web esterno) sul browser web del computer.

Ci sono altri strumenti periferici, che spesso fanno parte dell'ambiente di sviluppo, che non tratteremo qui. Questi includono cose come un [editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o IDE per modificare il codice, linters per la formattazione automatica, e così via. Stiamo assumendo che abbia già un editor di testo installato.

### Quali sono le opzioni di configurazione di Django?

Django è estremamente flessibile in termini di come e dove può essere installato e configurato. Django può essere:

- Installato su diversi sistemi operativi.
- Installato da sorgente, dal Python Package Index (PyPi) e, in molti casi, dall'applicazione del gestore dei pacchetti del computer host.
- Configurato per usare uno dei diversi database, che potrebbe anche dover essere installato e configurato separatamente.
- Eseguito nell'ambiente Python principale del sistema o all'interno di ambienti virtuali Python separati.

Ognuna di queste opzioni richiede una configurazione e impostazione leggermente diversa. Le seguenti sottosezioni spiegano alcune delle sue scelte. Per il resto dell'articolo, le mostreremo come impostare Django su un numero limitato di sistemi operativi, e quella configurazione sarà presupposta per tutto il resto di questo modulo.

> [!NOTE]
> Altre possibili opzioni di installazione sono coperte nella documentazione ufficiale di Django. Colleghiamo i [documenti appropriati qui sotto](#vedere_anche).

#### Quali sistemi operativi sono supportati?

Le applicazioni web Django possono essere eseguite su quasi qualsiasi macchina che può eseguire il linguaggio di programmazione Python 3: Windows, macOS, Linux/Unix, Solaris, per citarne solo alcuni.
Quasi ogni computer dovrebbe avere le prestazioni necessarie per eseguire Django durante lo sviluppo.

In questo articolo, forniremo istruzioni per Windows, macOS, e Linux/Unix.

#### Quale versione di Python dovrebbe essere usata?

Può usare qualsiasi versione di Python supportata dal rilascio Django di destinazione.
Per Django 5.0 le versioni consentite sono Python 3.10 a 3.12 (vedi [FAQ:Installazione](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django)).

Il progetto Django _raccomanda_ (e "supporta ufficialmente") l'uso della versione più nuova disponibile della release Python supportata.

#### Dove possiamo scaricare Django?

Ci sono tre luoghi da cui è possibile scaricare Django:

- Il Python Package Repository (PyPi), utilizzando lo strumento _pip_. Questo è il modo migliore per ottenere l'ultima versione stabile di Django.
- Usare una versione dal gestore dei pacchetti del computer. Le distribuzioni di Django che sono fornite con i sistemi operativi offrono un meccanismo di installazione familiare. Nota però che la versione impacchettata potrebbe essere piuttosto vecchia, e può essere installata solo nell'ambiente Python di sistema (che potrebbe non essere quello che si desidera).
- Installare da sorgente. Si può ottenere e installare l'ultima versione all'avanguardia di Django dalla sorgente. Questo non è raccomandato per i principianti ma è necessario quando si è pronti a iniziare a contribuire direttamente a Django stesso.

Questo articolo mostra come installare Django da PyPi, al fine di ottenere l'ultima versione stabile.

#### Quale database?

Django supporta ufficialmente i database PostgreSQL, MariaDB, MySQL, Oracle e SQLite, e ci sono librerie della comunità che forniscono vari livelli di supporto per altri popolari database SQL e NoSQL. Raccomandiamo di selezionare lo stesso database sia per la produzione che per lo sviluppo (anche se Django astrae molte delle differenze tra database usando il suo Object-Relational Mapper (ORM), ci sono comunque [problemi potenziali](https://docs.djangoproject.com/en/5.0/ref/databases/) che è meglio evitare).

Per questo articolo (e la maggior parte di questo modulo) useremo il database _SQLite_, che memorizza i suoi dati in un file. SQLite è pensato per essere usato come un database leggero e non può supportare un alto livello di concorrenza. Tuttavia, è un'ottima scelta per applicazioni che sono principalmente in sola lettura.

> [!NOTE]
> Django è configurato per usare SQLite di default quando si inizia il progetto del sito web usando gli strumenti standard (_django-admin_). È una grande scelta quando si inizia perché non richiede configurazione o impostazione aggiuntiva.

#### Installazione a livello di sistema o in un ambiente virtuale Python?

Quando si installa Python3 si ottiene un singolo ambiente globale che è condiviso da tutto il codice Python3. Anche se si possono installare i pacchetti Python che si desidera nell'ambiente, si può installare solo una particolare versione di ogni pacchetto alla volta.

> [!NOTE]
> Le applicazioni Python installate nell'ambiente globale possono potenzialmente confliggere tra loro (cioè, se dipendono da versioni diverse dello stesso pacchetto).

Se installa Django nell'ambiente di default/globale, potrà solo bersagliare una versione di Django sul computer. Questo può essere un problema se vuole creare nuovi siti web (usando l'ultima versione di Django) mentre sta ancora mantenendo siti web che si basano su versioni più vecchie.

Di conseguenza, gli sviluppatori Python/Django esperti in genere eseguono le app Python all'interno di _ambienti virtuali Python_ indipendenti. Questo consente di avere diversi ambienti Django su un singolo computer. Il team di sviluppo di Django stesso raccomanda di usare ambienti virtuali Python!

Questo modulo presume che abbia installato Django in un ambiente virtuale, e le mostreremo come fare di seguito.

## Installare Python 3

Per usare Django deve avere Python 3 sul sistema operativo.
Avrà anche bisogno dello strumento [Python Package Index](https://pypi.org/) — _pip3_ — che viene usato per gestire (installare, aggiornare e rimuovere) i pacchetti/librerie Python usati da Django e dalle altre app Python.

Questa sezione spiega brevemente come può controllare quali versioni di Python sono presenti e installare nuove versioni, se necessario, per Ubuntu Linux 20.04, macOS e Windows 10.

> [!NOTE]
> A seconda della sua piattaforma, potrebbe anche essere in grado di installare Python/pip dal gestore dei pacchetti del sistema operativo o tramite altri meccanismi. Per la maggior parte delle piattaforme, può scaricare i file di installazione richiesti da <https://www.python.org/downloads/> e installarli usando il metodo specifico per la piattaforma appropriata.

### Ubuntu 22.04

Ubuntu Linux 22.04 LTS include di default Python 3.10.12.
Può confermarlo eseguendo il seguente comando nel terminale bash:

```bash
python3 -V
# Output: Python 3.10.12
```

Tuttavia, lo strumento del Python Package Index (_pip3_) che le servirà per installare i pacchetti per Python 3 (incluso Django) **non** è disponibile di default.
Può installare _pip3_ nel terminale bash usando:

```bash
sudo apt install python3-pip
```

> [!NOTE]
> Python 3.10 è la versione più vecchia [supportata da Django 5.0](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django).
> Non è _necessario_ usare l'ultima versione di Python per questo tutorial, ma se vuole ci sono istruzioni su internet.

### macOS

macOS non include di default Python 3 (su versioni più vecchie è incluso Python 2).
Può confermarlo eseguendo il seguente comando nel terminale:

```bash
python3 -V
```

Questo mostrerà il numero di versione di Python, il che indica che Python 3 è installato, oppure `python3: comando non trovato`, il che indica che Python 3 non è stato trovato.

Può installare facilmente Python 3 (insieme allo strumento _pip3_) da [python.org](https://www.python.org/):

1. Scaricare l'installer richiesto:

   1. Andare su <https://www.python.org/downloads/macos/>
   2. Scaricare la release stabile della [versione supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) più recente che funziona con Django 5.0.
      (Al momento della scrittura, è Python 3.11.8).

2. Individuare il file usando _Finder_, e fare doppio clic sul file del pacchetto. Seguire le indicazioni dell'installazione.

Ora può confermare l'installazione avvenuta con successo eseguendo di nuovo `python3 -V` e controllando il numero di versione di Python.

Può controllare anche che _pip3_ sia installato elencando i pacchetti disponibili:

```bash
pip3 list
```

### Windows 10 o 11

Windows non include Python di default, ma può installarlo facilmente (insieme allo strumento _pip3_) da [python.org](https://www.python.org/):

1. Scaricare l'installer richiesto:

   1. Andare su <https://www.python.org/downloads/windows/>
   2. Scaricare la release stabile della [versione supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) più recente che funziona con Django 5.0.
      (Al momento della scrittura, è Python 3.11.8).

2. Installare Python facendo doppio clic sul file scaricato e seguendo le indicazioni dell'installazione
3. Assicurarsi di selezionare la casella etichettata "Aggiungi Python a PATH"

Può quindi verificare che Python 3 sia stato installato inserendo il seguente testo nel prompt dei comandi:

```bash
py -3 -V
```

L'installer Windows incorpora _pip3_ (il gestore dei pacchetti Python) di default.
Può elencare i pacchetti installati come mostrato:

```bash
py -3 -m pip list
```

> [!NOTE]
> L'installer dovrebbe configurare tutto il necessario affinché il comando precedente funzioni.
> Se tuttavia riceve un messaggio che Python non può essere trovato, potrebbe aver dimenticato di aggiungerlo al percorso di sistema.
> Può farlo eseguendo di nuovo l'installer, selezionando "Modifica" e spuntando la casella etichettata "Aggiungi Python alle variabili di ambiente" nella seconda pagina.

## Chiamare Python 3 e pip3

Noterà che nelle sezioni precedenti usiamo comandi diversi per richiamare Python 3 e pip su diversi sistemi operativi.

Se ha installato solo Python 3 (e non Python 2), di solito i comandi `python` e `pip` possono essere usati per eseguire Python e pip su qualsiasi sistema operativo.
Se ciò è consentito sul suo sistema, otterrà una stringa di versione "3" quando esegue `-V` con i comandi base, come mostrato:

```bash
python -V
pip -V
```

Se Python 2 è installato, per usare la versione 3 dovrebbe prefissare i comandi con `python3` e `pip3` su Linux/macOS, e con `py -3` e `py -3 -m pip` su Windows:

```bash
# Linux/macOS
python3 -V
pip3 -V

# Windows
py -3 -V
py -3 -m pip list
```

Le istruzioni qui sotto mostrano i comandi specifici per piattaforma, poiché funzionano su più sistemi.

## Usare Django all'interno di un ambiente virtuale Python

Le librerie che useremo per creare i nostri ambienti virtuali sono [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) (Linux e macOS) e [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) (Windows), che usano entrambe lo strumento [virtualenv](https://virtualenv.pypa.io/en/latest/). Gli strumenti wrapper creano un'interfaccia coerente per gestire le interfacce su tutte le piattaforme.

### Installare il software per l'ambiente virtuale

#### Configurazione dell'ambiente virtuale su Ubuntu

Dopo aver installato Python e pip può installare _virtualenvwrapper_ (che include _virtualenv_). La guida ufficiale all'installazione può essere trovata [qui](https://virtualenvwrapper.readthedocs.io/en/latest/install.html), oppure può seguire le istruzioni qui sotto.

Installare lo strumento usando _pip3_:

```bash
sudo pip3 install virtualenvwrapper
```

Poi aggiungere le seguenti righe alla fine del file di avvio della shell (questo è un file nascosto chiamato **.bashrc** nella sua directory home). Queste righe impostano il luogo in cui dovrebbero trovarsi gli ambienti virtuali, la posizione delle sue directory di progetto di sviluppo, e la posizione dello script installato con questo pacchetto:

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS=' -p /usr/bin/python3 '
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> Le variabili `VIRTUALENVWRAPPER_PYTHON` e `VIRTUALENVWRAPPER_VIRTUALENV_ARGS` puntano alla posizione di installazione normale per Python 3, e `source /usr/local/bin/virtualenvwrapper.sh` punta alla posizione normale dello script `virtualenvwrapper.sh`. Se il _virtualenv_ non funziona quando lo prova, una cosa da controllare è che Python e lo script siano nella posizione prevista (e poi cambiare il file di avvio di conseguenza).
>
> Può trovare le posizioni corrette per il suo sistema usando i comandi `which virtualenvwrapper.sh` e `which python3`.

Quindi ricaricare il file di avvio eseguendo il seguente comando nel terminale:

```bash
source ~/.bashrc
```

A questo punto dovrebbe vedere una serie di script in esecuzione come mostrato di seguito:

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

Configurare _virtualenvwrapper_ su macOS è praticamente lo stesso che su Ubuntu (ancora, può seguire le istruzioni sia dalla [guida ufficiale all'installazione](https://virtualenvwrapper.readthedocs.io/en/latest/install.html) sia da quelle qui sotto).

Installare _virtualenvwrapper_ (e includendo _virtualenv_) usando _pip_ come mostrato.

```bash
sudo pip3 install virtualenvwrapper
```

Poi aggiungere le seguenti righe alla fine del file di avvio della shell (sono le stesse righe che per Ubuntu).
Se sta usando la _zsh shell_, il file di avvio sarà un file nascosto chiamato **.zshrc** nella sua directory home. Se sta usando la _bash shell_, invece sarà un file nascosto chiamato **.bash_profile**. Potrebbe dover creare il file se non esiste ancora.

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> La variabile `VIRTUALENVWRAPPER_PYTHON` punta alla posizione di installazione normale per Python 3, e `source /usr/local/bin/virtualenvwrapper.sh` punta alla posizione normale dello script `virtualenvwrapper.sh`. Se il _virtualenv_ non funziona quando lo prova, una cosa da controllare è che Python e lo script siano nella posizione prevista (e poi cambiare il file di avvio di conseguenza).
>
> Ad esempio, un test di installazione su macOS ha richiesto le seguenti righe nel file di avvio:
>
> ```bash
> export WORKON_HOME=$HOME/.virtualenvs
> export VIRTUALENVWRAPPER_PYTHON=/Library/Frameworks/Python.framework/Versions/3.7/bin/python3
> export PROJECT_HOME=$HOME/Devel
> source /Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh
> ```
>
> Può trovare le posizioni corrette per il suo sistema usando i comandi `which virtualenvwrapper.sh` e `which python3`.

Poi ricaricare il file di avvio facendo la seguente chiamata nel terminale:

```bash
source ~/.bash_profile
```

A questo punto, potrebbe vedere una serie di script in esecuzione (gli stessi script dell'installazione su Ubuntu). Dovrebbe ora essere in grado di creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

> [!NOTE]
> Se non riesce a trovare il file di avvio da modificare nel finder, può anche aprirlo nel terminale usando nano.
>
> Supponendo che stia usando bash, i comandi assomigliano a questo:
>
> ```bash
> cd ~  # Navigare alla mia directory home
> ls -la #Elencare il contenuto della directory. Dovrebbe vedere .bash_profile
> nano .bash_profile # Aprire il file nell'editor di testo nano, all’interno del terminale
> # Scorrere fino alla fine del file, e copiare le righe sopra
> # Usare Ctrl+X per uscire da nano, scegliere Y per salvare il file.
> ```

#### Configurazione dell'ambiente virtuale su Windows

Installare [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) è ancora più semplice che configurare _virtualenvwrapper_ perché non è necessario configurare il luogo in cui lo strumento memorizza le informazioni dell'ambiente virtuale (c'è un valore di default). Tutto quello che deve fare è eseguire il seguente comando nel prompt dei comandi:

```bash
py -3 -m pip install virtualenvwrapper-win
```

Ora può creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

### Creare un ambiente virtuale

Una volta installato _virtualenvwrapper_ o _virtualenvwrapper-win_, lavorare con gli ambienti virtuali è molto simile su tutte le piattaforme.

Ora può creare un nuovo ambiente virtuale con il comando `mkvirtualenv`. Quando questo comando viene eseguito, vedrà l'ambiente che viene configurato (ciò che vedrà è leggermente specifico della piattaforma). Quando il comando si completa, il nuovo ambiente virtuale sarà attivo — può vederlo perché l'inizio del prompt sarà il nome dell'ambiente tra parentesi (sotto lo mostriamo per Ubuntu, ma la riga finale è simile per Windows/macOS).

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

Ora è all'interno dell'ambiente virtuale e può installare Django e iniziare a sviluppare.

> [!NOTE]
> D'ora in poi, in questo articolo (e in effetti nel modulo) si presuppone che qualsiasi comando venga eseguito all'interno di un ambiente virtuale Python come quello che abbiamo configurato sopra.

### Utilizzare un ambiente virtuale

Ci sono solo pochi altri comandi utili che dovrebbe conoscere (ce ne sono di più nella documentazione dello strumento, ma questi sono quelli che userà regolarmente):

- `deactivate` — Uscire dall'ambiente virtuale Python corrente
- `workon` — Elencare gli ambienti virtuali disponibili
- `workon nome_dell_ambiente` — Attivare l'ambiente virtuale Python specificato
- `rmvirtualenv nome_dell_ambiente` — Rimuovere l'ambiente specificato.

## Installare Django

Una volta creato un ambiente virtuale, e chiamato `workon` per entrarci, può usare _pip3_ per installare Django.

```bash
# Linux/macOS
python3 -m pip install django~=4.2

# Windows
py -3 -m pip install django~=4.2
```

Può verificare che Django sia installato eseguendo il seguente comando (questo testa solo che Python possa trovare il modulo Django):

```bash
# Linux/macOS
python3 -m django --version

# Windows
py -3 -m django --version
```

> [!NOTE]
> Se il comando Windows precedente non mostra un modulo django presente, provi:
>
> ```bash
> py -m django --version
> ```
>
> Su Windows gli script di _Python 3_ sono lanciati prefissando il comando con `py -3`, anche se questo può variare a seconda dell'installazione specifica.
> Provi a omettere il modificatore `-3` se ha problemi con i comandi.
> Su Linux/macOS, il comando è `python3.`

> [!WARNING]
> Il resto di questo **modulo** usa il comando _Linux_ per invocare Python 3 (`python3`). Se sta lavorando su _Windows_ sostituisca questo prefisso con: `py -3`

## Gestione del codice sorgente con Git e GitHub

Gli strumenti di Gestione del Codice Sorgente (SCM) e di versionamento le permettono di memorizzare e recuperare in modo affidabile le versioni del suo codice sorgente, provare modifiche e condividere il codice tra i suoi esperimenti e il "codice affidabile" quando ne ha bisogno.

Ci sono molti strumenti SCM diversi, tra cui git, Mercurial, Perforce, SVN (Subversion), CVS (Concurrent Versions System), ecc., e fonti di hosting SCM cloud come Bitbucket, GitHub e GitLab.
Per questo tutorial ospiteremo il nostro codice su [GitHub](https://github.com/), uno dei servizi di hosting del codice sorgente basato su cloud più popolari, e useremo l'**strumento git** per gestire il nostro codice sorgente localmente e inviarlo a GitHub quando necessario.

> [!NOTE]
> Usare strumenti SCM è una buona pratica nello sviluppo software!
> Queste istruzioni forniscono un'introduzione di base a git e GitHub.
> Per saperne di più, veda [Learning Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

### Concetti chiave

Git (e GitHub) usano repository ("repo") come "contenitore" di livello superiore per memorizzare il codice, dove ogni repo normalmente contiene il codice sorgente per una sola applicazione o un solo modulo.
I repository possono essere pubblici, in tal caso il codice è visibile a tutti su internet, o privati, nel qual caso sono riservati all'organizzazione o al profilo utente proprietario.

Tutto il lavoro è svolto su un particolare "branch" di codice nel suo repo.
Quando vuole eseguire il backup di alcune modifiche a un branch può creare un "commit", che memorizza tutte le modifiche dall'ultimo commit al branch corrente.

Il repo è creato con un branch predefinito chiamato "main". Può generare altri branch usando git, che inizialmente avranno tutti i commit del branch originale.
Possono essere sviluppati in modo indipendente aggiungendo commit, e poi successivamente usare una "Pull Request" (PR) su GitHub per unire le modifiche da un branch a un altro.
Può anche usare git per passare tra i branch sul suo computer locale, ad esempio per provare cose diverse.

Oltre ai branch, è possibile creare `tag` su qualsiasi branch e poi recuperare quel branch in quel punto.

### Creare un account e un repository su GitHub

Per prima cosa creeremo un account su GitHub (questo è gratuito).
Poi creeremo e configureremo un repository chiamato "django_local_library" per memorizzare il [sito web della libreria locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) mentre lo sviluppiamo nel resto di questo tutorial.

I passaggi sono:

1. Visiti <https://github.com/> e crei un account.
2. Una volta effettuato l'accesso, clicchi sul link **+** nella barra degli strumenti in alto e selezioni **Nuovo repository**.
3. Compili tutti i campi su questo modulo.
   Anche se non sono obbligatori, sono fortemente raccomandati.

   - Inserisca un nome per il repository: "django_local_library".
   - Inserisca una nuova descrizione del repository: "Sito web della libreria locale scritto in Django".
   - Selezioni "Pubblico" per il repository (il default).

     > [!WARNING]
     > Questo renderà visibile _tutto_ il codice sorgente.
     > Ricordi di non memorizzare credenziali o altro materiale sensibile nel suo repo a meno che non sia privato.

   - Scelga **Python** nell'elenco di selezione _Aggiungere .gitignore_.
   - Scelga la licenza preferita nell'elenco di selezione _Aggiungere licenza_.
     MDN usa "Creative Commons Zero v1.0 Universal" per questo esempio.
   - Spunti **Inizializzare questo repository con un README**.

4. Premere **Crea repository**.

   Il repository sarà creato, contenente solo i file `README.txt` e `.gitignore`.

### Clonare il repository sul suo computer locale

Ora che il repository ("repo") è stato creato su GitHub, vogliamo clonarlo (copiarlo) sul nostro computer locale:

1. Su GitHub, clicchi sul bottone verde **Codice**.
   Nella sezione "Clona", selezioni la scheda "HTTPS" e copi l'URL.
   Se ha usato il nome del repository "django_local_library", l'URL dovrebbe essere qualcosa come: `https://github.com/<your_git_user_id>/django_local_library.git`.

2. Installi _git_ sul suo computer locale (può trovare le versioni per diverse piattaforme [qui](https://git-scm.com/downloads)).
3. Apra un prompt dei comandi/terminale e cloni il suo repository usando l'URL che ha copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/django_local_library.git
   ```

   Questo creerà il repository all'interno della directory corrente.

4. Navighi nella cartella del repository.

   ```bash
   cd django_local_library
   ```

### Modificare e sincronizzare le modifiche

Ora modificheremo il file `.gitignore` sul computer locale, comprometteremo la modifica, e aggiorneremo il repository su GitHub.
Questa è una modifica utile da fare, ma soprattutto la stiamo facendo per mostrarle come prendere cambiamenti da GitHub, apportare modifiche localmente, e poi inviarle a GitHub.

1. Nel prompt dei comandi/terminale per prima cosa "fetch" (ottiene) e poi "pull" (ottiene e unisce nel branch corrente) l'ultima versione del sorgente da GitHub:

   > [!NOTE]
   > Questo passaggio non è strettamente necessario poiché abbiamo appena clonato il sorgente e sappiamo che è aggiornato.
   > Tuttavia, in generale, dovrebbe aggiornare il codice sorgente da GitHub prima di apportare modifiche.

   ```bash
   git fetch origin main
   git pull origin main
   ```

   L'"origin" è un _remote_, che rappresenta la posizione del repo in cui si trova il sorgente, e "main" è il branch.
   Può verificare che origin sia il nostro repo su GitHub usando il comando: `git remote -v`.

2. Successivamente facciamo il checkout di un nuovo branch per memorizzare le nostre modifiche:

   ```bash
   git checkout -b update_gitignore
   ```

   Il comando `checkout` è usato per passare un branch a essere il branch corrente su cui sta lavorando.
   Il flag `-b` indica che intendiamo creare un nuovo branch chiamato "update_gitignore" invece di selezionare un branch esistente con quel nome.

3. Apra il file **.gitignore**, copi le seguenti righe in fondo e poi salvi:

   ```plain
   # Text backup files
   *.bak

   # Database
   *.sqlite3
   ```

   Noti che `.gitignore` è usato per indicare i file che non dovrebbero essere automaticamente salvati da git, come i file temporanei e altri artefatti di compilazione.

4. Usi il comando `add` per aggiungere tutti i file modificati (che non sono ignorati dal file **.gitignore**) all'"area di staging" per il branch corrente.

   ```bash
   git add -A
   ```

5. Usi il comando `status` per controllare che tutti i file che sta per `commit` siano corretti (vuole includere i file sorgente, non i binari, i file temporanei ecc.).
   Dovrebbe assomigliare un po' all'elenco qui sotto.

   ```bash
   > git status
   On branch main
   Your branch is up-to-date with 'origin/update_gitignore'.
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

           modified:   .gitignore
   ```

6. Quando è convinto, `commit` i file nel suo repository locale, usando il flag `-m` per specificare un messaggio di commit conciso ma chiaro.
   Questo è l'equivalente di firmare le modifiche e renderle una parte ufficiale del repository locale.

   ```bash
   git commit -m ".gitignore: add .bak and .sqlite3"
   ```

7. A questo punto, il repository remoto non è stato modificato.
   Possiamo premere il branch `update_gitignore` sul repository "origin" (GitHub) usando il seguente comando:

   ```bash
   git push origin update_gitignore
   ```

8. Torni alla pagina su GitHub dove ha creato il suo repository e aggiorni la pagina.

   Dovrebbe apparire un banner con un pulsante da premere se vuole "Confrontare e fare una richiesta di pull" sul branch che ha appena caricato.
   Selezioni il pulsante e poi segua le istruzioni per creare e poi unire una richiesta di pull.

   ![Banner che chiede se l'utente vuole confrontare e unire gli aggiornamenti recenti del branch](github_compare_and_pull_banner.png)

   Dopo l'unione, il branch "main" sul repository su GitHub conterrà le sue modifiche a `.gitignore`.

9. Può continuare ad aggiornare il suo repository locale man mano che i file cambiano usando questo ciclo di add/commit/push.

Nel prossimo argomento useremo questo repository per memorizzare il codice sorgente del sito web della nostra libreria locale.

## Altri strumenti Python

Gli sviluppatori Python esperti possono installare strumenti aggiuntivi, come i linters (che aiutano a rilevare errori comuni nel codice).

Noti che dovrebbe usare un linter consapevole di Django come [pylint-django](https://pypi.org/project/pylint-django/), perché alcuni linter Python comuni (come `pylint`) segnalano erroneamente errori nei file standard generati per Django.

## Testare l'installazione

Il test sopra funziona, ma non è molto divertente. Un test più interessante è creare un progetto scheletrico e vederlo funzionare. Per farlo, per prima cosa navighi nel prompt dei comandi/terminale dove vuole memorizzare le sue app Django. Crei una cartella per il suo sito di prova e navighi in essa.

```bash
mkdir django_test
cd django_test
```

Può quindi creare un nuovo sito scheletrico chiamato "_mytestsite_" usando lo strumento **django-admin** come mostrato. Dopo aver creato il sito, può navigare nella cartella dove troverà lo script principale per gestire i progetti, chiamato **manage.py**.

```bash
django-admin startproject mytestsite
cd mytestsite
```

Possiamo eseguire il _server web di sviluppo_ da questa cartella usando **manage.py** e il comando `runserver`, come mostrato.

```bash
# Linux/macOS
python3 manage.py runserver

# Windows
py -3 manage.py runserver
```

> [!NOTE]
> Può ignorare gli avvisi su "migrazioni non applicate" a questo punto!

Una volta che il server è in esecuzione, può visualizzare il sito navigando al seguente URL sul suo browser web locale: `http://127.0.0.1:8000/`. Dovrebbe vedere un sito che assomiglia a questo:

![La home page dell'app scheletro Django](django_skeleton_app_homepage_django_4_0.png)

## Riepilogo

Ora ha un ambiente di sviluppo Django in esecuzione sul suo computer.

Nella sezione di testing ha anche visto brevemente come possiamo creare un nuovo sito web Django usando `django-admin startproject`, e eseguirlo nel suo browser usando il server web di sviluppo (`python3 manage.py runserver`). Nel prossimo articolo, espanderemo questo processo, costruendo un'applicazione web semplice ma completa.

## Vedere anche

- [Guida rapida all'installazione](https://docs.djangoproject.com/en/5.0/intro/install/) (documenti Django)
- [Come installare Django — Guida completa](https://docs.djangoproject.com/en/5.0/topics/install/) (documenti Django) — copre anche come rimuovere Django
- [Come installare Django su Windows](https://docs.djangoproject.com/en/5.0/howto/windows/) (documenti Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}
