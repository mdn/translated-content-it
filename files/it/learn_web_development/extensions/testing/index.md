---
title: Testare
slug: Learn_web_development/Extensions/Testing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Testing/Introduction", "Learn_web_development/Extensions")}}

Qualsiasi codice, una volta raggiunto un certo livello di complessità, necessita di un sistema di test associato per assicurarsi che, con l'aggiunta di nuovo codice, continui a funzionare correttamente e in modo performante, e soddisfi le esigenze degli utenti. Questo modulo elenca i fondamenti con cui dovresti iniziare.

> [!NOTE]
> Questo modulo era originariamente dedicato interamente al test cross-browser, ma stiamo lavorando per rifocalizzarlo affinché copra i test in generale. Quando avremo il tempo, intendiamo aggiornare il materiale per coprire i Fondamenti generali dei test, i test Funzionali e di compatibilità, e i test di Usabilità.

## Prerequisiti

Prima di iniziare questo modulo, dovresti aver già appreso i fondamenti di [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics), e [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

## Tutorial

- [Introduzione al test cross-browser](/it/docs/Learn_web_development/Extensions/Testing/Introduction)
  - : Questo articolo inizia il modulo fornendo una panoramica sull'argomento del test cross-browser, rispondendo a domande come "che cos'è il test cross-browser?", "quali sono i tipi di problemi più comuni che incontrerai?", e "quali sono i principali approcci per testare, identificare e risolvere i problemi?"
- [Strategie per eseguire i test](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies)
  - : Successivamente, ci concentriamo sull'esecuzione dei test, identificando un pubblico target (ad esempio, quali browser, dispositivi e altri segmenti dovresti assicurarti che siano testati), strategie di test a basso costo (procurati una gamma di dispositivi e alcune macchine virtuali ed esegui test ad hoc quando necessario), strategie più tecniche (automazione, uso di app di test dedicate), e test con gruppi di utenti.
- [Gestire i problemi comuni di HTML e CSS](/it/docs/Learn_web_development/Extensions/Testing/HTML_and_CSS)
  - : Con il quadro impostato, ora esamineremo specificamente i problemi comuni cross-browser che incontrerai nel codice HTML e CSS e quali strumenti possono essere usati per prevenire problemi o risolverli. Questo include linting del codice, gestione dei prefissi CSS, utilizzo degli strumenti di sviluppo del browser per individuare i problemi, utilizzo di polyfill per aggiungere supporto nei browser, affrontare i problemi di design responsive e altro ancora.
- [Implementazione del rilevamento delle funzionalità](/it/docs/Learn_web_development/Extensions/Testing/Feature_detection)
  - : Il rilevamento delle funzionalità implica verificare se un browser supporta un certo blocco di codice ed eseguire codice diverso a seconda che lo faccia (o no), in modo che il browser possa sempre fornire un'esperienza funzionante piuttosto che bloccarsi/dare errori in alcuni browser. Questo articolo dettaglia come scrivere il proprio semplice rilevamento delle funzionalità, come usare una libreria per velocizzare l'implementazione, e funzionalità native per il rilevamento delle funzionalità come `@supports`.
- [Introduzione al test automatizzato](/it/docs/Learn_web_development/Extensions/Testing/Automated_testing)
  - : Eseguire manualmente test su diversi browser e dispositivi, più volte al giorno, può diventare tedioso e richiedere molto tempo. Per gestire questo in modo efficiente, dovresti familiarizzare con gli strumenti di automazione. In questo articolo, vediamo cosa è disponibile, come usare i task runners e i fondamenti di come utilizzare le app commerciali per l'automazione dei test dei browser come Sauce Labs e Browser Stack.
- [Impostare il proprio ambiente di test automatizzato](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment)
  - : In questo articolo, ti insegneremo come installare il tuo ambiente di automazione ed eseguire i tuoi test utilizzando Selenium/WebDriver e una libreria di test come selenium-webdriver per Node. Vedremo anche come integrare il tuo ambiente di test locale con app commerciali come quelle discusse nell'articolo precedente.

{{NextMenu("Learn_web_development/Extensions/Testing/Introduction", "Learn_web_development/Extensions")}}
