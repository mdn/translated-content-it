---
title: Controllo di versione
slug: Learn_web_development/Core/Version_control
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core")}}

Gli strumenti di controllo di versione (spesso chiamati **Version Control Systems**, o **VCS**) sono una parte essenziale dei flussi di lavoro moderni, per fare il backup e collaborare su basi di codice. Questo modulo vi guida attraverso gli elementi essenziali del controllo di versione utilizzando Git e GitHub.

## Panoramica

Gli strumenti di controllo di versione sono essenziali per lo sviluppo software:

- È raro lavorare su un progetto completamente da soli e, non appena iniziate a collaborare con altre persone, c'è il rischio di entrare in conflitto con il lavoro altrui — questo succede quando entrambi cercate di aggiornare lo stesso pezzo di codice allo stesso tempo. È necessario avere un qualche tipo di meccanismo in atto per gestire tali situazioni e aiutare a evitare la perdita di lavoro.
- Quando lavorate a un progetto, da soli o con altri, vorrete essere in grado di fare il backup del codice in un luogo centrale, in modo che non venga perso se il vostro computer si rompe.
- Vorrete anche poter tornare a versioni precedenti se in seguito si scopre un problema. Potreste aver iniziato a fare ciò nel vostro lavoro creando diverse versioni dello stesso file, ad esempio `myCode.js`, `myCode_v2.js`, `myCode_v3.js`, `myCode_final.js`, `myCode_really_really_final.js`, ecc., ma questo è davvero soggetto a errori e inaffidabile.
- I membri del team vorranno spesso creare le proprie versioni separate del codice (chiamate **rami** in Git), lavorare su una nuova funzionalità in quella versione e poi integrarla in modo controllato (su GitHub utilizziamo le **pull request**) con la versione principale quando hanno finito.

Gli strumenti di controllo di versione soddisfano le esigenze sopra elencate. [Git](https://git-scm.com/) è un esempio di strumento di controllo di versione, e [GitHub](https://github.com/) è un sito web + infrastruttura che fornisce un server Git più una serie di strumenti davvero utili per lavorare con repository git in modo individuale o in team, come la segnalazione di problemi con il codice, strumenti di revisione, funzioni di gestione del progetto come l'assegnazione di compiti e stati dei compiti, e altro ancora.

> [!NOTE]
> Git è in realtà un _strumento di controllo di versione distribuito_, il che significa che una copia completa del repository contenente la base di codice viene creata sul vostro computer (e su quello di tutti gli altri). Apportate le modifiche alla vostra copia e quindi inviate tali modifiche al server, dove un amministratore deciderà se unire le vostre modifiche con la copia principale.

## Prerequisiti

Per utilizzare Git e GitHub, è necessario:

- Un computer desktop con Git installato (vedere la [pagina dei download di Git](https://git-scm.com/downloads)).
- Uno strumento per utilizzare Git. A seconda di come vi piace lavorare, potreste utilizzare un [client GUI Git](https://git-scm.com/downloads/guis/) (consigliamo [GitHub Desktop](https://desktop.github.com/download/), [SourceTree](https://www.sourcetreeapp.com/) o [Git Kraken](https://www.gitkraken.com/)) o semplicemente usare una finestra terminale. In effetti, è probabilmente utile conoscere almeno le basi dei comandi terminale di git, anche se intendete utilizzare una GUI.
- Un [account GitHub](https://github.com/signup). Se non ne avete già uno, iscrivetevi ora utilizzando il link fornito.

In termini di conoscenze preliminari, non è necessario sapere nulla di sviluppo web, Git/GitHub o controllo di versione per iniziare questo modulo. Tuttavia, è consigliato conoscere un po' di programmazione per avere una buona alfabetizzazione informatica e un po' di codice da memorizzare nei vostri repository!

È anche preferibile avere una conoscenza di base del terminale, ad esempio spostarsi tra le directory e creare file. Potete trovare tutte le nozioni di base nel nostro [Corso intensivo di riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line).

> [!NOTE]
> GitHub non è l'unico sito/strumento che potete utilizzare con Git. Ci sono altre alternative come [GitLab](https://about.gitlab.com/) che potreste provare, e potreste anche provare a configurare il vostro server Git e utilizzarlo invece di GitHub. Abbiamo scelto di restare solo su GitHub in questo corso per fornire un'unica modalità che funzioni.

## Obiettivi di apprendimento

- Perché i sistemi di controllo di versione sono necessari.
- La differenza tra Git e siti web come GitHub e GitLab.
- Comprendere che siti web come GitHub e GitLab abilitano il lavoro di squadra e la collaborazione in modi non così facili solo con il semplice Git.
- Configurazione di base — installare git, iscriversi a un account per il sito di social coding scelto.
- Gestione dei requisiti di sicurezza, come chiavi SSH/GPG.
- Creare un repository e inviare modifiche.
- Contribuire ai repository di altri: fare il fork, creare un nuovo ramo, creare una pull request, e il flusso di revisione.
- Buone pratiche:
  - Aggiornare regolarmente i repository locali in modo che siano sincronizzati con le loro controparti remote.
  - Utilizzare `.gitignore` per ignorare tutto ciò che non si vuole integrare.
  - Eliminare i rami che avete finito di utilizzare.
- Gestione dei conflitti di unione.

## Guida

Si noti che i link sottostanti vi porteranno a risorse su siti esterni. Alla fine, miriamo ad avere un nostro corso dedicato su Git/GitHub, ma per ora, questi vi aiuteranno a comprendere l'argomento in questione.

- [Hello, World (da GitHub)](https://docs.github.com/en/get-started/start-your-journey/hello-world)
  - : Questo è un buon punto di partenza — questa guida pratica vi permette di iniziare subito a usare GitHub, apprendendo le basi di Git come creare repository e rami, fare commit e aprire e unire pull request.
- [Manuale Git (da GitHub)](https://docs.github.com/en/get-started/using-git/about-git)
  - : Questo Manuale Git va un po' più in profondità, spiegando cos'è uno strumento di controllo di versione, cos'è un repository, come funziona il modello di base di GitHub, comandi Git e esempi, e altro ancora.
- [Fork dei Progetti (da GitHub)](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project)
  - : Fare il fork dei progetti è essenziale quando si desidera contribuire al codice di qualcun altro. Questa guida spiega come fare.
- [Informazioni sulle Pull Requests (da GitHub)](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
  - : Una guida utile per gestire le pull request, il modo in cui le vostre modifiche al codice suggerite vengono consegnate ai repository delle persone per la considerazione.
- [Gestione dei problemi (da GitHub)](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues)
  - : I problemi sono come un forum per il vostro progetto GitHub, dove le persone possono fare domande e segnalare problemi, e voi potete gestire gli aggiornamenti (ad esempio assegnare persone per correggere problemi, chiarire il problema, informare le persone che le cose sono risolte). Questo articolo vi dice cosa c'è da sapere sui problemi.

> [!NOTE]
> C'è **molto di più** che potete fare con Git e GitHub, ma riteniamo che quanto sopra rappresenti il minimo necessario per iniziare a usare Git efficacemente. Man mano che approfondirete in Git, inizierete a capire che è facile commettere errori quando iniziate a usare comandi più complicati. Non preoccupatevi, anche gli sviluppatori web professionisti trovano Git a volte confuso e spesso risolvono i problemi cercando soluzioni sul web o consultando siti come [Regole di volo per Git](https://github.com/k88hudson/git-flight-rules) e [Maledetto, git!](https://dangitgit.com/)

## Vedi anche

- [Comprendere il flusso di GitHub](https://docs.github.com/en/get-started/using-github/github-flow)
- [Elenco dei comandi Git](https://git-scm.com/docs)
- [Padroneggiare il markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) (il formato di testo che scrivete su PR, commenti sui problemi e file `.md`).
- [Iniziare con GitHub Pages](https://docs.github.com/en/pages/quickstart) (come pubblicare demo e siti web su GitHub).
- [Imparare il branching di Git](https://learngitbranching.js.org/)
- [Regole di volo per Git](https://github.com/k88hudson/git-flight-rules) (un compendio molto utile su come raggiungere determinati obiettivi in Git, incluso come correggere cose quando si è sbagliato).
- [Maledetto, git!](https://dangitgit.com/) (un altro compendio utile, specificamente sui modi per correggere cose quando si è sbagliato).

{{PreviousMenu("Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core")}}
