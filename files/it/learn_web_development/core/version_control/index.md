---
title: Controllo di versione
slug: Learn_web_development/Core/Version_control
l10n:
  sourceCommit: c655f38c10ba17b853b0e66b43cf4cf2b176e424
---

{{PreviousMenu("Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core")}}

Gli strumenti di controllo di versione (spesso chiamati **Version Control Systems** o **VCS**) sono una parte essenziale dei moderni flussi di lavoro di programmazione: consentono di eseguire il backup del codice, collaborare su codebase e ripristinare versioni precedenti quando necessario.

[Git](https://git-scm.com/) è un esempio di strumento di controllo di versione. [GitHub](https://github.com/), invece, è un sito web e un'infrastruttura che fornisce un server Git insieme a vari strumenti utili per lavorare con repository Git, sia individualmente sia in team. GitHub consente di segnalare problemi nel codice, revisionare il codice in modo collaborativo e offre funzionalità di gestione dei progetti come la classificazione dei problemi, l'assegnazione delle attività, la pianificazione dei progetti e altro ancora.

Questo modulo illustra gli elementi essenziali del controllo di versione con Git e GitHub.

## Prerequisiti

- Un computer desktop con Git installato (consultare la [pagina dei download di Git](https://git-scm.com/downloads/)).
- Uno strumento per usare Git. A seconda delle preferenze di lavoro, è possibile usare:
  - Un [client GUI per Git](https://git-scm.com/downloads/guis/) (si consigliano [GitHub Desktop](https://desktop.github.com/download/), [SourceTree](https://www.sourcetreeapp.com/) o [Git Kraken](https://www.gitkraken.com/)).
  - Una finestra della riga di comando/terminale (consultare il nostro [corso intensivo sulla riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) per un'introduzione).
- Un [account GitHub](https://github.com/signup). Se non ne è già disponibile uno, registrarsi ora usando il link fornito.

## Guida

- [Che cos'è GitHub? (da GitHub)](https://docs.github.com/en/get-started/start-your-journey/what-is-github)
  - : Descrive cosa sono Git e GitHub, come funzionano insieme e come iniziare.
- [Hello, World (da GitHub)](https://docs.github.com/en/get-started/using-github/hello-world)
  - : Questa guida pratica passa subito all'uso di GitHub, insegnando le basi di Git, come creare repository e branch, effettuare commit e aprire e unire pull request.
- [Usare Git (da GitHub)](https://docs.github.com/en/get-started/using-git)
  - : Il manuale di Git approfondisce leggermente l'argomento, spiegando che cos'è uno strumento di controllo di versione, che cos'è un repository, come funziona il modello di base di GitHub, i comandi Git e relativi esempi, e altro ancora.
- [Contribuire a un progetto (da GitHub)](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project)
  - : Il fork dei progetti è essenziale quando si desidera contribuire al codice di qualcun altro. Questa guida spiega come farlo.
- [Informazioni sulle Pull Request (da GitHub)](https://docs.github.com/en/pull-requests/reference/pull-requests)
  - : Una guida utile per gestire le pull request. Queste richieste consentono di proporre modifiche al codice del repository di qualcun altro, affinché possano essere revisionate ed eventualmente unite alla codebase principale.
- [Informazioni sugli issue (da GitHub)](https://docs.github.com/en/issues/tracking-your-work-with-issues/learning-about-issues/about-issues)
  - : Gli issue sono simili a un forum per il progetto GitHub, in cui le persone possono porre domande e segnalare problemi, mentre è possibile gestire gli aggiornamenti (ad esempio assegnando persone per risolvere gli issue, chiarendo il problema o informando che i problemi sono stati risolti). Questo articolo spiega tutto ciò che occorre sapere sugli issue.

> [!NOTE]
> Man mano che si approfondisce Git, diventa evidente che è facile commettere errori quando si iniziano a usare comandi più complessi. Non preoccuparti: anche gli sviluppatori web professionisti trovano Git talvolta confuso e spesso risolvono i problemi cercando soluzioni sul web o consultando siti come [Flight rules for Git](https://github.com/k88hudson/git-flight-rules) e [Dangit, git!](https://dangitgit.com/).

> [!NOTE]
> [Intro to Git](https://scrimba.com/intro-to-git-c0l4grs2sa) di Scrimba <sup>[_partner per l'apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre un'introduzione pratica all'uso di Git e GitHub.

## Vedere anche

- Altri argomenti utili trattati nella documentazione di GitHub includono:
  - [Comprendere il flusso GitHub](https://docs.github.com/en/get-started/using-github/github-flow)
  - [Risolvere i conflitti di merge](https://docs.github.com/en/pull-requests/how-tos/merge-and-close-pull-requests)
  - [Ignorare file con .gitignore](https://docs.github.com/en/get-started/git-basics/ignoring-files)
  - [Autenticazione su GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github)
  - [Padroneggiare Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) (il formato di testo usato nelle PR, nei commenti degli issue e nei file `.md`).
  - [Iniziare a usare GitHub Pages](https://docs.github.com/en/pages/quickstart) (come pubblicare demo e siti web su GitHub).
- [Elenco dei comandi Git](https://git-scm.com/docs)
- [Imparare il branching Git](https://learngitbranching.js.org/)
- [Flight rules for Git](https://github.com/k88hudson/git-flight-rules) (un utilissimo compendio di modi per ottenere risultati specifici con Git, incluso come correggere gli errori).
- [Dangit, git!](https://dangitgit.com/) (un altro utile compendio, specificamente dedicato ai modi per correggere gli errori).

{{PreviousMenu("Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core")}}
