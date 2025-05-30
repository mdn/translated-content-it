---
title: Impostare un ambiente di sviluppo Django
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Django/development_environment
l10n:
  sourceCommit: cb25e0acbd9f0af27c4a99965cb962230d49a35d
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che sai a cosa serve Django, ti mostreremo come configurare e testare un ambiente di sviluppo Django su Windows, Linux (Ubuntu) e macOS — qualunque sistema operativo comune tu stia utilizzando, questo articolo dovrebbe fornirti tutto il necessario per iniziare a sviluppare applicazioni Django.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base dell'uso di un terminale/riga di comando e di come installare pacchetti software sul sistema operativo del tuo computer di sviluppo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Avere un ambiente di sviluppo per Django (4.*) funzionante sul tuo computer.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica dell'ambiente di sviluppo Django

Django rende molto facile configurare il tuo computer in modo da poter iniziare a sviluppare applicazioni web. Questa sezione spiega cosa ottieni con l'ambiente di sviluppo, e fornisce una panoramica di alcune delle tue opzioni di configurazione. Il resto dell'articolo spiega il metodo _raccomandato_ per installare l'ambiente di sviluppo Django su Ubuntu, macOS e Windows, e come puoi testarlo.

### Cos'è l'ambiente di sviluppo Django?

L'ambiente di sviluppo è un'installazione di Django sul tuo computer locale che puoi utilizzare per sviluppare e testare applicazioni Django prima di distribuirle in un ambiente di produzione.

Gli strumenti principali che Django stesso fornisce sono una serie di script Python per creare e lavorare con i progetti Django, insieme a un semplice _server web di sviluppo_ che puoi usare per testare applicazioni web Django locali (cioè, sul tuo computer, non su un server web esterno) nel browser web del tuo computer.

Ci sono altri strumenti periferici, che spesso fanno parte dell'ambiente di sviluppo, che non tratteremo qui. Questi includono cose come un [editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o un IDE per modificare il codice, linters per la formattazione automatica, e così via. Presumiamo che tu abbia già installato un editor di testo.

### Quali sono le opzioni di configurazione di Django?

Django è estremamente flessibile in termini di modalità e luogo in cui può essere installato e configurato. Django può essere:

- Installato su diversi sistemi operativi.
- Installato da sorgente, dall'Indice dei Pacchetti Python (PyPi) e, in molti casi, dal gestore dei pacchetti del computer host.
- Configurato per utilizzare uno di diversi database, che potrebbero anche dover essere installati e configurati separatamente.
- Eseguito nel principale ambiente Python del sistema o all'interno di ambienti virtuali Python separati.

Ognuna di queste opzioni richiede una configurazione e configurazione leggermente diversa. Le seguenti sottosezioni spiegano alcune delle tue scelte. Per il resto dell'articolo, ti mostreremo come configurare Django su un piccolo numero di sistemi operativi, e quella configurazione sarà assunta per il resto di questo modulo.

> [!NOTE]
> Altre possibili opzioni di installazione sono trattate nella documentazione ufficiale di Django. Colleghiamo ai [documenti appropriati sotto](#vedi_anche).

#### Quali sistemi operativi sono supportati?

Le applicazioni web Django possono essere eseguite su quasi qualsiasi macchina in grado di eseguire il linguaggio di programmazione Python 3: Windows, macOS, Linux/Unix, Solaris, per citarne solo alcuni.
Quasi qualsiasi computer dovrebbe avere le prestazioni necessarie per eseguire Django durante lo sviluppo.

In questo articolo, forniremo istruzioni per Windows, macOS e Linux/Unix.

#### Quale versione di Python dovrebbe essere usata?

Puoi usare qualsiasi versione di Python supportata dalla versione di Django che stai prendendo di mira.
Per Django 5.0 le versioni consentite sono Python 3.10 fino a 3.12 (vedi [FAQ:Installation](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django)).

Il progetto Django _raccomanda_ (e "supporta ufficialmente") l'uso della versione più recente disponibile della release Python supportata.

#### Da dove possiamo scaricare Django?

Ci sono tre luoghi per scaricare Django:

- L'Indice dei Pacchetti Python (PyPi), usando lo strumento _pip_. Questo è il modo migliore per ottenere l'ultima versione stabile di Django.
- Usare una versione dal gestore dei pacchetti del tuo computer. Le distribuzioni di Django che sono incluse nei sistemi operativi offrono un meccanismo di installazione noto. Nota però che la versione pacchettizzata potrebbe essere piuttosto vecchia, e può essere installata solo nell'ambiente Python del sistema (che potrebbe non essere quello che vuoi).
- Installare dai sorgenti. Puoi ottenere e installare l'ultima versione all'avanguardia di Django dai sorgenti. Questo non è consigliato per i principianti ma è necessario quando sei pronto per iniziare a contribuire direttamente a Django stesso.

Questo articolo mostra come installare Django da PyPi, per ottenere l'ultima versione stabile.

#### Quale database?

Django supporta ufficialmente i database PostgreSQL, MariaDB, MySQL, Oracle, e SQLite, e ci sono librerie comunitarie che forniscono vari livelli di supporto per altri popolari database SQL e NoSQL. Raccomandiamo di selezionare lo stesso database per produzione e sviluppo (sebbene Django astrae molte delle differenze del database utilizzando il suo Object-Relational Mapper (ORM), ci sono ancora [problemi potenziali](https://docs.djangoproject.com/en/5.0/ref/databases/) che è meglio evitare).

Per questo articolo (e per la maggior parte di questo modulo) useremo il database _SQLite_, che memorizza i suoi dati in un file. SQLite è destinato all'uso come database leggero e non può supportare un alto livello di concorrenza. È, tuttavia, un'ottima scelta per applicazioni che sono principalmente in sola lettura.

> [!NOTE]
> Django è configurato per utilizzare SQLite per impostazione predefinita quando si avvia il progetto del sito web utilizzando gli strumenti standard (_django-admin_). È un'ottima scelta quando si inizia perché non richiede ulteriore configurazione o installazione.

#### Installazione a livello di sistema o in un ambiente virtuale Python?

Quando installi Python3 ottieni un singolo ambiente globale che è condiviso da tutto il codice Python3. Sebbene tu possa installare qualsiasi pacchetto Python ti piaccia nell'ambiente, puoi installare solo una particolare versione di ciascun pacchetto alla volta.

> [!NOTE]
> Le applicazioni Python installate nell'ambiente globale possono potenzialmente entrare in conflitto tra loro (cioè, se dipendono da diverse versioni dello stesso pacchetto).

Se installi Django nell'ambiente predefinito/globale, potrai usare solo una versione di Django sul computer. Questo potrebbe essere un problema se vuoi creare nuovi siti web (utilizzando l'ultima versione di Django) mentre mantieni i siti web che si basano su versioni più vecchie.

Di conseguenza, gli sviluppatori esperti di Python/Django in genere eseguono le applicazioni Python all'interno di _ambienti virtuali Python_ indipendenti. Questo consente di avere più ambienti Django diversi su un singolo computer. Il team di sviluppo di Django stesso raccomanda l'uso degli ambienti virtuali Python!

Questo modulo presuppone che tu abbia installato Django in un ambiente virtuale, e ti mostreremo come di seguito.

## Installazione di Python 3

Per utilizzare Django è necessario avere Python 3 sul proprio sistema operativo.
Avrai bisogno anche dello strumento [Python Package Index](https://pypi.org/) — _pip3_ — che è utilizzato per gestire (installare, aggiornare e rimuovere) i pacchetti/librerie Python usati da Django e dalle tue altre app Python.

Questa sezione spiega brevemente come puoi verificare quali versioni di Python sono presenti e installare nuove versioni se necessario, per Ubuntu Linux 20.04, macOS e Windows 10.

> [!NOTE]
> A seconda della tua piattaforma, potresti anche essere in grado di installare Python/pip dal gestore dei pacchetti del sistema operativo o tramite altri meccanismi. Per la maggior parte delle piattaforme, puoi scaricare i file di installazione richiesti da <https://www.python.org/downloads/> e installarli utilizzando il metodo specifico per la piattaforma appropriata.

### Ubuntu 22.04

Ubuntu Linux 22.04 LTS include Python 3.10.12 di default.
Puoi confermarlo eseguendo il seguente comando nel terminale bash:

```bash
python3 -V
# Output: Python 3.10.12
```

Tuttavia, lo strumento Python Package Index (_pip3_) di cui avrai bisogno per installare pacchetti per Python 3 (incluso Django) **non** è disponibile di default.
Puoi installare _pip3_ nel terminale bash usando:

```bash
sudo apt install python3-pip
```

> [!NOTE]
> Python 3.10 è la versione più vecchia [supportata da Django 5.0](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django).
> Non è _necessario_ usare l'ultima versione di Python per questo tutorial, ma se vuoi ci sono istruzioni su internet.

### macOS

macOS non include Python 3 di default (Python 2 è incluso nelle versioni più vecchie).
Puoi confermarlo eseguendo il seguente comando nel terminale:

```bash
python3 -V
```

Questo mostrerà il numero di versione di Python, che indica che Python 3 è installato, o `python3: command not found`, che indica che Python 3 non è stato trovato.

Puoi facilmente installare Python 3 (insieme allo strumento _pip3_) da [python.org](https://www.python.org/):

1. Scarica l'installer richiesto:

   1. Vai su <https://www.python.org/downloads/macos/>
   2. Scarica il rilascio stabile della versione più recente [supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) che funzioni con Django 5.0.
      (al momento della scrittura, questa è Python 3.11.8).

2. Individua il file usando il _Finder_ e fai doppio clic sul file del pacchetto. Segui le istruzioni di installazione.

Puoi ora confermare l'installazione riuscita eseguendo nuovamente `python3 -V` e verificando il numero di versione di Python.

Puoi controllare in modo simile che _pip3_ sia installato elencando i pacchetti disponibili:

```bash
pip3 list
```

### Windows 10 o 11

Windows non include Python di default, ma puoi installarlo facilmente (insieme allo strumento _pip3_) da [python.org](https://www.python.org/):

1. Scarica l'installer richiesto:

   1. Vai su <https://www.python.org/downloads/windows/>
   2. Scarica il rilascio stabile della versione più recente [supportata](https://docs.djangoproject.com/en/5.0/faq/install/#what-python-version-can-i-use-with-django) che funzioni con Django 5.0.
      (al momento della scrittura, questa è Python 3.11.8).

2. Installa Python facendo doppio clic sul file scaricato e seguendo le istruzioni di installazione.
3. Assicurati di selezionare la casella "Add Python to PATH"

Puoi quindi verificare che Python 3 sia stato installato inserendo il seguente testo nel prompt dei comandi:

```bash
py -3 -V
```

L'installer di Windows incorpora _pip3_ (il gestore dei pacchetti Python) di default.
Puoi elencare i pacchetti installati come mostrato:

```bash
py -3 -m pip list
```

> [!NOTE]
> L'installer dovrebbe configurare tutto il necessario affinché il comando sopra funzioni.
> Se però ricevi un messaggio che indica che Python non può essere trovato, potresti aver dimenticato di aggiungerlo al percorso di sistema.
> Puoi farlo eseguendo nuovamente l'installer, selezionando "Modify", e selezionando la casella "Add Python to environment variables" sulla seconda pagina.

## Chiamare Python 3 e pip3

Noterai che nelle sezioni precedenti usiamo comandi diversi per chiamare Python 3 e pip su diversi sistemi operativi.

Se hai installato solo Python 3 (e non Python 2), i comandi semplici `python` e `pip` possono generalmente essere usati per eseguire Python e pip su qualsiasi sistema operativo.
Se questo è consentito sul tuo sistema otterrai una stringa di "versione 3" quando esegui `-V` con i comandi semplici, come mostrato:

```bash
python -V
pip -V
```

Se Python 2 è installato, per usare la versione 3 dovresti prefissare i comandi con `python3` e `pip3` su Linux/macOS, e `py -3` e `py -3 -m pip` su Windows:

```bash
# Linux/macOS
python3 -V
pip3 -V

# Windows
py -3 -V
py -3 -m pip list
```

Le istruzioni di seguito mostrano i comandi specifici della piattaforma come funzionano su più sistemi.

## Utilizzare Django all'interno di un ambiente virtuale Python

Le librerie che useremo per creare i nostri ambienti virtuali sono [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) (Linux e macOS) e [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) (Windows), che usano entrambe lo strumento [virtualenv](https://virtualenv.pypa.io/en/latest/). Gli strumenti wrapper creano un'interfaccia coerente per la gestione delle interfacce su tutte le piattaforme.

### Installare il software dell'ambiente virtuale

#### Configurazione dell'ambiente virtuale su Ubuntu

Dopo aver installato Python e pip, puoi installare _virtualenvwrapper_ (che include _virtualenv_). Puoi controllare la [guida ufficiale all'installazione](https://virtualenvwrapper.readthedocs.io/en/latest/install.html), o seguire le istruzioni sotto.

Installa lo strumento utilizzando _pip3_:

```bash
sudo pip3 install virtualenvwrapper
```

Quindi aggiungi le seguenti righe alla fine del tuo file di avvio shell (questo è un file nascosto chiamato **.bashrc** nella tua directory home). Queste impostano la posizione in cui dovrebbero risiedere gli ambienti virtuali, la posizione delle directory dei tuoi progetti di sviluppo, e la posizione dello script installato con questo pacchetto:

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS=' -p /usr/bin/python3 '
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> Le variabili `VIRTUALENVWRAPPER_PYTHON` e `VIRTUALENVWRAPPER_VIRTUALENV_ARGS` puntano alla normale posizione di installazione di Python 3, e `source /usr/local/bin/virtualenvwrapper.sh` punta alla posizione normale dello script `virtualenvwrapper.sh`. Se _virtualenv_ non funziona quando lo testi, una cosa da controllare è che Python e lo script siano nella posizione prevista (e quindi modificare il file di avvio di conseguenza).
>
> Puoi trovare le posizioni corrette per il tuo sistema usando i comandi `which virtualenvwrapper.sh` e `which python3`.

Quindi ricarica il file di avvio eseguendo il seguente comando nel terminale:

```bash
source ~/.bashrc
```

A questo punto dovresti vedere un sacco di script eseguiti come mostrato di seguito:

```bash
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/premkproject
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postmkproject
# …
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/preactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/get_env_details
```

Ora puoi creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

#### Configurazione dell'ambiente virtuale su macOS

Impostare _virtualenvwrapper_ su macOS è quasi esattamente lo stesso che su Ubuntu (ancora una volta, puoi seguire le istruzioni sia dalla [guida ufficiale all'installazione](https://virtualenvwrapper.readthedocs.io/en/latest/install.html) che qui sotto).

Installa _virtualenvwrapper_ (e _virtualenv_ incluso) utilizzando _pip_ come mostrato.

```bash
sudo pip3 install virtualenvwrapper
```

Quindi aggiungi le seguenti righe alla fine del tuo file di avvio shell (queste sono le stesse righe di Ubuntu).
Se stai usando la _zsh shell_ allora il file di avvio sarà un file nascosto chiamato **.zshrc** nella tua directory home. Se stai usando la _bash shell_ allora sarà un file nascosto chiamato **.bash_profile**. Potresti dover creare il file se non esiste ancora.

```bash
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> La variabile `VIRTUALENVWRAPPER_PYTHON` punta alla normale posizione di installazione di Python 3, e `source /usr/local/bin/virtualenvwrapper.sh` punta alla normale posizione dello script `virtualenvwrapper.sh`. Se _virtualenv_ non funziona quando lo testi, una cosa da controllare è che Python e lo script siano nella posizione prevista (e quindi modificare il file di avvio di conseguenza).
>
> Per esempio, un test di installazione su macOS ha fatto sì che fossero necessarie le seguenti righe nel file di avvio:
>
> ```bash
> export WORKON_HOME=$HOME/.virtualenvs
> export VIRTUALENVWRAPPER_PYTHON=/Library/Frameworks/Python.framework/Versions/3.7/bin/python3
> export PROJECT_HOME=$HOME/Devel
> source /Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh
> ```
>
> Puoi trovare le posizioni corrette per il tuo sistema usando i comandi `which virtualenvwrapper.sh` e `which python3`.

Quindi ricarica il file di avvio facendo la seguente chiamata nel terminale:

```bash
source ~/.bash_profile
```

A questo punto, potresti vedere un sacco di script eseguiti (gli stessi script dell'installazione di Ubuntu). Dovresti ora essere in grado di creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

> [!NOTE]
> Se non riesci a trovare il file di avvio da modificare nel finder, puoi anche aprirlo nel terminale usando nano.
>
> Supponendo che stai usando bash, i comandi assomigliano a questi:
>
> ```bash
> cd ~  # Navigare alla mia directory home
> ls -la # Mostra il contenuto della directory. Dovresti vedere .bash_profile
> nano .bash_profile # Apri il file nell'editor di testo nano, all'interno del terminale
> # Scorri fino alla fine del file, e copia le righe sopra
> # Usa Ctrl+X per uscire da nano, scegli Y per salvare il file.
> ```

#### Configurazione dell'ambiente virtuale su Windows

Installare [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/) è ancora più semplice che configurare _virtualenvwrapper_ perché non hai bisogno di configurare dove lo strumento memorizza le informazioni sull'ambiente virtuale (esiste un valore predefinito). Tutto ciò che devi fare è eseguire il seguente comando nel prompt dei comandi:

```bash
py -3 -m pip install virtualenvwrapper-win
```

Ora puoi creare un nuovo ambiente virtuale con il comando `mkvirtualenv`.

### Creare un ambiente virtuale

Una volta installato _virtualenvwrapper_ o _virtualenvwrapper-win_, lavorare con gli ambienti virtuali è molto simile su tutte le piattaforme.

Ora puoi creare un nuovo ambiente virtuale con il comando `mkvirtualenv`. Quando questo comando viene eseguito vedrai l'ambiente in fase di configurazione (ciò che vedi è leggermente specifico della piattaforma). Quando il comando viene completato, il nuovo ambiente virtuale sarà attivo — puoi vederlo perché l'inizio del prompt sarà il nome dell'ambiente tra parentesi (di seguito lo mostriamo per Ubuntu, ma la riga finale è simile per Windows/macOS).

```bash
mkvirtualenv my_django_environment
```

Dovresti vedere un output simile al seguente:

```plain
Running virtualenv with interpreter /usr/bin/python3
# …
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/t_env7/bin/get_env_details
(my_django_environment) ubuntu@ubuntu:~$
```

Ora che sei all'interno dell'ambiente virtuale, puoi installare Django e iniziare a sviluppare.

> [!NOTE]
> Da ora in poi in questo articolo (e in effetti nel modulo) si prega di assumere che qualsiasi comando sia eseguito all'interno di un ambiente virtuale Python come quello che abbiamo configurato sopra.

### Utilizzare un ambiente virtuale

Ci sono solo pochi altri comandi utili che dovresti conoscere (ce ne sono altri nella documentazione dello strumento, ma questi sono quelli che userai regolarmente):

- `deactivate` — Esci dall'ambiente virtuale Python corrente
- `workon` — Elenca gli ambienti virtuali disponibili
- `workon name_of_environment` — Attiva l'ambiente virtuale Python specificato
- `rmvirtualenv name_of_environment` — Rimuove l'ambiente specificato.

## Installazione di Django

Una volta creato un ambiente virtuale, e eseguito `workon` per entrare in esso, puoi usare _pip3_ per installare Django.

```bash
# Linux/macOS
python3 -m pip install django~=4.2

# Windows
py -3 -m pip install django~=4.2
```

Puoi testare che Django sia installato eseguendo il seguente comando (questo testa solo che Python possa trovare il modulo Django):

```bash
# Linux/macOS
python3 -m django --version

# Windows
py -3 -m django --version
```

> [!NOTE]
> Se il suddetto comando Windows non mostra un modulo django presente, prova:
>
> ```bash
> py -m django --version
> ```
>
> In Windows gli script _Python 3_ sono lanciati anteponendo il comando con `py -3`, sebbene questo possa variare a seconda della tua installazione specifica.
> Prova a omettere il modificatore `-3` se incontri problemi con i comandi.
> In Linux/macOS, il comando è `python3`.

> [!WARNING]
> Il resto di questo **modulo** utilizza il comando _Linux_ per invocare Python 3 (`python3`). Se stai lavorando su _Windows_, sostituisci questo prefisso con: `py -3`

## Gestione del codice sorgente con Git e GitHub

Gli strumenti di Gestione del Codice Sorgente (SCM) e versionamento ti permettono di memorizzare e recuperare in modo affidabile le versioni del tuo codice sorgente, provare cambiamenti, e condividere il codice tra i tuoi esperimenti e "codice noto e funzionante" quando ne hai bisogno.

Ci sono molti strumenti SCM diversi, tra cui git, Mercurial, Perforce, SVN (Subversion), CVS (Concurrent Versions System), ecc., e sorgenti di hosting SCM cloud come Bitbucket, GitHub e GitLab.
Per questo tutorial ospiteremo il nostro codice su [GitHub](https://github.com/), uno dei servizi di hosting del codice sorgente basati su cloud più popolari, e useremo lo strumento **git** per gestire il nostro codice sorgente localmente e inviarlo a GitHub quando necessario.

> [!NOTE]
> Usare gli strumenti SCM è una buona pratica di sviluppo software!
> Queste istruzioni forniscono una breve introduzione a git e GitHub.
> Per saperne di più, vedi [Learning Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

### Concetti chiave

Git (e GitHub) utilizzano i repository ("repo") come "contenitore" di livello superiore per memorizzare il codice, dove ogni repo contiene normalmente il codice sorgente per una sola applicazione o modulo.
I repository possono essere pubblici, nel qual caso il codice è visibile a tutti su internet, o privati, nel qual caso sono limitati all'account dell'organizzazione o dell'utente proprietario.

Tutto il lavoro viene svolto su un particolare "ramo" di codice nel tuo repo.
Quando vuoi fare il backup di alcune modifiche su un ramo, puoi creare un "commit", che memorizza tutte le modifiche dall'ultimo commit al ramo corrente.

Il repo è creato con un ramo predefinito chiamato "main". Puoi generare altri rami da questo usando git, che inizialmente hanno tutti i commit del ramo originale.
Puoi sviluppare i rami separatamente aggiungendo commit e poi successivamente utilizzare una "Pull Request" (PR) su GitHub per unire le modifiche da un ramo all'altro.
Puoi anche usare git per passare tra rami sul tuo computer locale, ad esempio per provare cose diverse.

Oltre ai rami, è possibile creare `tag` su qualsiasi ramo e successivamente recuperare quel ramo in quel punto.

### Creare un account e un repository su GitHub

Per prima cosa creeremo un account su GitHub (è gratuito).
Quindi creiamo e configuriamo un repository chiamato "django_local_library" per memorizzare il [sito web della libreria locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) mentre lo sviluppiamo nel resto di questo tutorial.

I passaggi sono:

1. Visita <https://github.com/> e crea un account.
2. Una volta effettuato l'accesso, clicca sul link **+** nella barra degli strumenti superiore e seleziona **New repository**.
3. Compila tutti i campi su questo modulo.
   Sebbene non siano obbligatori, sono fortemente raccomandati.

   - Inserisci un nome per il repository: "django_local_library".
   - Inserisci una nuova descrizione del repository: "Sito web della Libreria Locale scritto in Django".
   - Seleziona "Public" per il repository (il predefinito).

     > [!WARNING]
     > Questo renderà _tutto_ il codice sorgente visibile.
     > Ricorda di non memorizzare le credenziali o altro materiale sensibile nel tuo repo a meno che non sia privato.

   - Scegli **Python** nella lista di selezione _Add .gitignore_.
   - Scegli la tua licenza preferita nella lista di selezione _Add license_.
     MDN usa "Creative Commons Zero v1.0 Universal" per questo esempio.
   - Seleziona **Initialize this repository with a README**.

4. Premi **Create repository**.

   Il repository verrà creato, contenendo solo i file `README.txt` e `.gitignore`.

### Clonare il repo sul tuo computer locale

Ora che il repository ("repo") è creato su GitHub, vogliamo clonarlo (copiarlo) sul nostro computer locale:

1. Su GitHub, clicca sul pulsante verde **Code**.
   Nella sezione "Clone", seleziona la scheda "HTTPS" e copia l'URL.
   Se hai usato il nome del repository "django_local_library", l'URL dovrebbe essere qualcosa come: `https://github.com/<your_git_user_id>/django_local_library.git`.

2. Installa _git_ per il tuo computer locale ([guida ufficiale al download di Git](https://git-scm.com/downloads)).
3. Apri un prompt dei comandi/terminale e clona il tuo repo usando l'URL copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/django_local_library.git
   ```

   Questo creerà il repository all'interno della directory corrente.

4. Naviga nella cartella del repo.

   ```bash
   cd django_local_library
   ```

### Modificare e sincronizzare le modifiche

Ora modificheremo il file `.gitignore` sul computer locale, eseguiremo il commit della modifica e aggiorneremo il repository su GitHub.
Questo è un cambiamento utile da fare, ma principalmente lo facciamo per mostrarti come prelevare modifiche da GitHub, apportare modifiche localmente e quindi inviarle a GitHub.

1. Nel prompt dei comandi/terminale, prima "fetch" (ottenere) e poi pull (ottenere e unire nel ramo corrente) l'ultima versione del sorgente da GitHub:

   > [!NOTE]
   > Questo passaggio non è strettamente necessario poiché abbiamo appena clonato il sorgente e sappiamo che è aggiornato.
   > Tuttavia in generale dovresti aggiornare i tuoi sorgenti da GitHub prima di apportare modifiche.

   ```bash
   git fetch origin main
   git pull origin main
   ```

   L'"origine" è un _remoto_, che rappresenta la posizione del repo in cui si trova il sorgente, e "main" è il ramo.
   Puoi verificare che l'origine sia il nostro repo su GitHub usando il comando: `git remote -v`.

2. Successivamente, creiamo un nuovo ramo per memorizzare le nostre modifiche:

   ```bash
   git checkout -b update_gitignore
   ```

   Il comando `checkout` viene utilizzato per cambiare un ramo in modo che diventi il ramo corrente su cui stai lavorando.
   Il flag `-b` indica che intendiamo creare un nuovo ramo chiamato "update_gitignore" invece di selezionare un ramo esistente con quel nome.

3. Apri il file **.gitignore**, copia le seguenti righe in fondo ad esso e poi salva:

   ```plain
   # Text backup files
   *.bak

   # Database
   *.sqlite3
   ```

   Nota che `.gitignore` viene utilizzato per indicare i file che non dovrebbero essere automaticamente copiati da git, come file temporanei e altri artefatti di build.

4. Usa il comando `add` per aggiungere tutti i file modificati (che non sono ignorati dal file **.gitignore**) all'"area di staging" per il ramo corrente.

   ```bash
   git add -A
   ```

5. Usa il comando `status` per controllare che tutti i file che stai per `commit` siano corretti (vuoi includere i file sorgente, non i binari, i file temporanei ecc.).
   Dovrebbe sembrare un po' come l'elenco seguente.

   ```bash
   > git status
   On branch main
   Your branch is up-to-date with 'origin/update_gitignore'.
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

           modified:   .gitignore
   ```

6. Quando sei soddisfatto, esegui il `commit` dei file nel tuo repo locale, usando il flag `-m` per specificare un messaggio di commit conciso ma chiaro.
   Questo è equivalente a firmare le modifiche e renderle una parte ufficiale del repo locale.

   ```bash
   git commit -m ".gitignore: add .bak and .sqlite3"
   ```

7. A questo punto, il repo remoto non è stato modificato.
   Possiamo spingere il ramo `update_gitignore` al repo "origin" (GitHub) usando il seguente comando:

   ```bash
   git push origin update_gitignore
   ```

8. Torna alla pagina su GitHub dove hai creato il tuo repo e aggiorna la pagina.

   Dovrebbe apparire un banner con un pulsante su cui premere se vuoi "Compare and pull request" del ramo appena caricato.
   Seleziona il pulsante e poi segui le istruzioni per creare e poi unire una pull request.

   ![Banner che chiede se l'utente vuole confrontare e unire recenti aggiornamenti del ramo](github_compare_and_pull_banner.png)

   Dopo l'unione, il ramo "main" sul repo su GitHub conterrà le tue modifiche al file `.gitignore`.

9. Puoi continuare ad aggiornare il tuo repo locale man mano che i file cambiano utilizzando questo ciclo aggiungere/commit/spingere.

Nel prossimo argomento useremo questo repo per memorizzare il codice sorgente del nostro sito web della libreria locale.

## Altri strumenti Python

Gli sviluppatori Python esperti possono installare strumenti aggiuntivi, come i linters (che aiutano a rilevare errori comuni nel codice).

Nota che dovresti usare un linter consapevole di Django come [pylint-django](https://pypi.org/project/pylint-django/), perché alcuni linters Python comuni (come `pylint`) segnalano erroneamente errori nei file standard generati per Django.

## Testare l'installazione

Il test sopra funziona, ma non è molto divertente. Un test più interessante è creare un progetto scheletro e vederlo funzionare. Per fare ciò, prima naviga nel tuo prompt dei comandi/terminale dove vuoi memorizzare le tue app Django. Crea una cartella per il tuo sito di test e naviga in essa.

```bash
mkdir django_test
cd django_test
```

Puoi quindi creare un nuovo sito scheletro chiamato "_mytestsite_" utilizzando lo strumento **django-admin** come mostrato. Dopo aver creato il sito, puoi navigare nella cartella dove troverai lo script principale per la gestione dei progetti, chiamato **manage.py**.

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
> Puoi ignorare gli avvisi su "migrazioni non applicate" a questo punto!

Una volta che il server è in esecuzione, puoi visualizzare il sito navigando verso il seguente URL sul tuo browser web locale: `http://127.0.0.1:8000/`. Dovresti vedere un sito che appare così:

![La home page dell'app scheletro Django](django_skeleton_app_homepage_django_4_0.png)

## Riepilogo

Ora hai un ambiente di sviluppo Django funzionante sul tuo computer.

Nella sezione testing hai anche visto brevemente come possiamo creare un nuovo sito web Django usando `django-admin startproject` e eseguirlo nel tuo browser usando il server web di sviluppo (`python3 manage.py runserver`). Nel prossimo articolo, espanderemo questo processo, costruendo un'applicazione web semplice ma completa.

## Vedi anche

- [Quick Install Guide](https://docs.djangoproject.com/en/5.0/intro/install/) (documenti Django)
- [How to install Django — Complete guide](https://docs.djangoproject.com/en/5.0/topics/install/) (documenti Django) — copre anche come rimuovere Django
- [How to install Django on Windows](https://docs.djangoproject.com/en/5.0/howto/windows/) (documenti Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django")}}
