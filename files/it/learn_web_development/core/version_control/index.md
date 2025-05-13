---
title: Controllo di versione
slug: Learn_web_development/Core/Version_control
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core")}}

Gli strumenti di controllo di versione (spesso chiamati **Version Control Systems**, o **VCS**) sono una parte essenziale dei moderni flussi di lavoro, per effettuare backup e collaborare su codici sorgente. Questo modulo ti guida attraverso gli elementi essenziali del controllo di versione utilizzando Git e GitHub.

## Panoramica

Gli strumenti di controllo di versione sono essenziali per lo sviluppo software:

- È raro lavorare a un progetto completamente da solo e, non appena si inizia a lavorare con altre persone, si corre il rischio di entrare in conflitto con il lavoro degli altri — questo succede quando entrambi cercano di aggiornare lo stesso pezzo di codice allo stesso tempo. È necessario disporre di un meccanismo per gestire queste occorrenze e aiutare a evitare la perdita di lavoro come risultato.
- Quando si lavora a un progetto da soli o con altri, sarà utile poter eseguire un backup del codice in un luogo centrale, in modo che non vada perso se il computer si rompe.
- Sarà anche utile poter tornare a versioni precedenti se viene scoperto un problema successivamente. Potresti aver già iniziato a fare questo nel tuo lavoro creando versioni diverse dello stesso file, ad esempio, `myCode.js`, `myCode_v2.js`, `myCode_v3.js`, `myCode_final.js`, `myCode_really_really_final.js`, ecc., ma questo è veramente soggetto a errori e poco affidabile.
- I diversi membri del team comunemente vogliono creare le proprie versioni separate del codice (chiamate **branche** in Git), lavorare su una nuova funzionalità in quella versione, e poi integrarla in modo controllato (su GitHub utilizziamo le **pull request**) con la versione master quando hanno finito.

Gli strumenti di controllo di versione soddisfano queste necessità. [Git](https://git-scm.com/) è un esempio di strumento di controllo di versione, e [GitHub](https://github.com/) è un sito web + infrastruttura che fornisce un server Git più una serie di strumenti davvero utili per lavorare con i repository Git individualmente o in team, come la segnalazione dei problemi con il codice, strumenti di revisione, funzioni di gestione dei progetti come assegnare compiti e stati delle attività, e altro ancora.

> [!NOTE]
> Git è in realtà uno strumento di controllo di versione _distribuito_, il che significa che una copia completa del repository contenente il codice sorgente viene creata sul tuo computer (e su quello di tutti gli altri). Si effettuano modifiche alla propria copia e poi si inviano tali modifiche al server, dove un amministratore deciderà se unire le modifiche con la copia master.

## Prerequisiti

Per utilizzare Git e GitHub, è necessario:

- Un computer desktop con Git installato (vedere la [pagina di download di Git](https://git-scm.com/downloads)).
- Uno strumento per utilizzare Git. A seconda di come preferisce lavorare, potrebbe utilizzare un [client GUI Git](https://git-scm.com/downloads/guis/) (consigliamo [GitHub Desktop](https://desktop.github.com/download/), [SourceTree](https://www.sourcetreeapp.com/) o [Git Kraken](https://www.gitkraken.com/)) o semplicemente continuare a utilizzare una finestra terminale. Infatti, è probabilmente utile conoscere almeno le basi dei comandi terminale di git, anche se si intende utilizzare una GUI.
- Un [account GitHub](https://github.com/signup). Se non lo possiede già, si iscriva ora utilizzando il link fornito.

In termini di conoscenze prerequisiti, non è necessario sapere nulla di sviluppo web, Git/GitHub, o controllo di versione per iniziare questo modulo. Tuttavia, è consigliato che sappia programmare, in modo da avere una discreta alfabetizzazione informatica e qualche codice da conservare nei suoi repository!

È anche preferibile che abbia una conoscenza di base del terminale, ad esempio muoversi tra le directory e creare file. Può trovare tutte le basi nel nostro [Corso intensivo di linea di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line).

> [!NOTE]
> GitHub non è l'unico sito/strumento che può utilizzare con Git. Esistono altre alternative come [GitLab](https://about.gitlab.com/) che potrebbe provare, e potrebbe anche provare a configurare un proprio server Git e usarlo al posto di GitHub. Abbiamo scelto solo GitHub in questo corso per fornire un modo unico che funzioni.

## Risultati di apprendimento

- Perché i sistemi di controllo di versione sono necessari.
- La differenza tra Git e siti web come GitHub e GitLab.
- Comprendere che siti web come GitHub e GitLab abilitano il lavoro di squadra e la collaborazione che non sono così facili solo con il semplice Git.
- Configurazione di base — installazione di git, iscrizione a un account per il sito di social coding scelto.
- Gestione dei requisiti di sicurezza, come chiavi SSH/GPG.
- Creazione di un repository e push delle modifiche.
- Contribuire ai repository di altri: forking, creazione di un nuovo branch, creazione di una PR, e flusso di revisione.
- Buona manutenzione:
  - Aggiornamento regolare dei repository locali in modo che siano sincronizzati con le loro controparti remote.
  - Usare `.gitignore` per ignorare tutto ciò che non si vuole eseguire il commit.
  - Eliminare i branch terminati.
- Gestione dei conflitti di merge.

## Guide

Si noti che i link qui sotto portano a risorse su siti esterni. Alla fine, puntiamo ad avere un corso dedicato a Git/GitHub, ma per ora, questi link aiuteranno ad apprendere l'argomento in questione.

- [Hello, World (da GitHub)](https://docs.github.com/en/get-started/start-your-journey/hello-world)
  - : Questo è un buon punto di partenza — questa guida pratica incoraggia l'utente a immergersi subito nell'uso di GitHub, imparando le basi di Git come creare repository e branch, effettuare commit, e aprire e unire pull request.
- [Git Handbook (da GitHub)](https://docs.github.com/en/get-started/using-git/about-git)
  - : Questo Git Handbook approfondisce un po', spiegando cos'è uno strumento di controllo di versione, cos'è un repository, come funziona il modello di base GitHub, comandi Git ed esempi, e altro.
- [Forking Projects (da GitHub)](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project)
  - : Fare forking dei progetti è essenziale quando si vuole contribuire al codice di qualcun altro. Questa guida spiega come fare.
- [About Pull Requests (da GitHub)](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
  - : Una guida utile per gestire le pull request, il modo in cui i cambiamenti suggeriti nel codice vengono consegnati ai repository delle persone per essere considerati.
- [Mastering issues (da GitHub)](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues)
  - : I problemi sono come un forum per il tuo progetto GitHub, dove le persone possono fare domande e segnalare problemi, e puoi gestire gli aggiornamenti (ad esempio assegnare persone per risolvere i problemi, chiarire i problemi, far sapere alle persone che le cose sono risolte). Questo articolo dice ciò che devi sapere sui problemi.

> [!NOTE]
> Ci sono **molte più cose** che puoi fare con Git e GitHub, ma riteniamo che quanto sopra rappresenti il minimo necessario per iniziare a utilizzare Git in modo efficace. Man mano che si approfondisce Git, si inizierà a rendersi conto che è facile sbagliare quando si iniziano a utilizzare comandi più complicati. Non preoccuparti, anche gli sviluppatori web professionisti trovano Git talvolta confuso e spesso risolvono i problemi cercando soluzioni sul web o consultando siti come [Regole di volo per Git](https://github.com/k88hudson/git-flight-rules) e [Maledetto, git!](https://dangitgit.com/)

## Vedi anche

- [Understanding the GitHub flow](https://docs.github.com/en/get-started/using-github/github-flow)
- [Elenco comandi Git](https://git-scm.com/docs)
- [Mastering markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) (il formato testo che si utilizza nei commenti delle PR, problemi e file `.md`).
- [Iniziare con GitHub Pages](https://docs.github.com/en/pages/quickstart) (come pubblicare demo e siti web su GitHub).
- [Learn Git branching](https://learngitbranching.js.org/)
- [Regole di volo per Git](https://github.com/k88hudson/git-flight-rules) (un compendio molto utile di modi per ottenere cose specifiche in Git, incluso come correggere errori).
- [Maledetto, git!](https://dangitgit.com/) (un altro compendio utile, specificamente di modi per correggere errori).

{{PreviousMenu("Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core")}}
