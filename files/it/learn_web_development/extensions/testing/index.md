---
title: Test
slug: Learn_web_development/Extensions/Testing
l10n:
  sourceCommit: 6030ef1aadf967b80e2c79c3d3463cccc8ea0c95
---

{{NextMenu("Learn_web_development/Extensions/Testing/Introduction", "Learn_web_development/Extensions")}}

Qualsiasi codebase che supera un certo livello di complessità deve disporre di un sistema di test associato, per garantire che, con l'aggiunta di nuovo codice, la codebase continui a funzionare correttamente e in modo efficiente e continui a soddisfare le esigenze degli utenti. Questo modulo elenca i principi fondamentali da cui iniziare.

> [!NOTE]
> Questo modulo era originariamente dedicato interamente al cross-browser testing, ma è in corso una riorganizzazione per trattare il testing in generale. Quando sarà possibile, il materiale verrà aggiornato per coprire i principi fondamentali del testing generale, il testing funzionale e di compatibilità e il testing di usabilità.

## Prerequisiti

Prima di iniziare questo modulo, è necessario aver appreso i fondamenti di [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

## Tutorial

- [Introduzione al cross-browser testing](/it/docs/Learn_web_development/Extensions/Testing/Introduction)
  - : Questo articolo introduce il modulo fornendo una panoramica dell'argomento del cross-browser testing e rispondendo a domande quali "che cos'è il cross-browser testing?", "quali sono i tipi di problemi più comuni che si possono incontrare?" e "quali sono i principali approcci per testare, identificare e risolvere i problemi?"
- [Strategie per eseguire i test](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies)
  - : Successivamente, viene approfondita l'esecuzione dei test, esaminando l'identificazione di un pubblico di destinazione (ad esempio, quali browser, dispositivi e altri segmenti devono essere sottoposti a test), strategie di testing a bassa tecnologia (procurarsi una serie di dispositivi e alcune macchine virtuali ed eseguire test ad hoc quando necessario), strategie più avanzate (automazione, utilizzo di app dedicate al testing) e test con gruppi di utenti.
- [Gestire i problemi comuni di HTML e CSS](/it/docs/Learn_web_development/Extensions/Testing/HTML_and_CSS)
  - : Dopo aver definito il contesto, verranno esaminati nello specifico i problemi cross-browser comuni che si incontrano nel codice HTML e CSS e gli strumenti che possono essere utilizzati per impedire che si verifichino problemi o per risolvere quelli che si presentano. Ciò include il linting del codice, la gestione dei prefissi CSS, l'uso degli strumenti di sviluppo del browser per individuare i problemi, l'uso di polyfill per aggiungere supporto nei browser, l'affrontare i problemi del responsive design e altro ancora.
- [Implementare il feature detection](/it/docs/Learn_web_development/Extensions/Testing/Feature_detection)
  - : Il feature detection consiste nello stabilire se un browser supporta un determinato blocco di codice e nell'eseguire codice diverso a seconda che lo supporti o meno, in modo che il browser possa sempre offrire un'esperienza funzionante anziché bloccarsi o generare errori in alcuni browser. Questo articolo descrive come scrivere un semplice feature detection, come utilizzare una libreria per velocizzarne l'implementazione e le funzionalità native per il feature detection, come `@supports`.
- [Introduzione al testing automatizzato](/it/docs/Learn_web_development/Extensions/Testing/Automated_testing)
  - : Eseguire manualmente i test su diversi browser e dispositivi, più volte al giorno, può diventare noioso e richiedere molto tempo. Per gestire questa attività in modo efficiente, è opportuno acquisire familiarità con gli strumenti di automazione. In questo articolo vengono esaminati gli strumenti disponibili, come utilizzare i task runner e le nozioni di base sull'uso di app commerciali per l'automazione dei test nei browser, come Sauce Labs e Browser Stack.
- [Configurare il proprio ambiente di automazione dei test](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment)
  - : In questo articolo verrà spiegato come installare un ambiente di automazione e come eseguire i propri test utilizzando Selenium/WebDriver e una libreria di testing come selenium-webdriver per Node. Verrà inoltre esaminato come integrare l'ambiente di testing locale con app commerciali come quelle trattate nell'articolo precedente.

{{NextMenu("Learn_web_development/Extensions/Testing/Introduction", "Learn_web_development/Extensions")}}
