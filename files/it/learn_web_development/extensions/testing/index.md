---
title: Testing
slug: Learn_web_development/Extensions/Testing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Testing/Introduction", "Learn_web_development/Extensions")}}

Ogni base di codice, una volta raggiunto un certo livello di complessità, necessita di un sistema di test associato, per garantire che, con l'aggiunta di nuovo codice, il codice esistente continui a funzionare correttamente e in modo performante, e continui a soddisfare le esigenze degli utenti. Questo modulo elenca i fondamenti con cui si dovrebbe iniziare.

> [!NOTE]
> Questo modulo era originariamente dedicato interamente al testing cross-browser, ma stiamo lavorando per rifocalizzarlo in modo da coprire il testing in generale. Quando avremo il tempo, intendiamo aggiornare il materiale per includere i fondamenti del testing generale, il testing funzionale e di compatibilità, e il testing di usabilità.

## Prerequisiti

Prima di iniziare questo modulo, Lei dovrebbe aver già appreso i fondamenti di [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics), e [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

## Tutorial

- [Introduzione al testing cross-browser](/it/docs/Learn_web_development/Extensions/Testing/Introduction)
  - : Questo articolo avvia il modulo fornendo una panoramica sull’argomento del testing cross-browser, rispondendo a domande come "cos'è il testing cross-browser?", "quali sono i tipi più comuni di problemi che incontrerà?", e "quali sono i principali approcci per testare, identificare e risolvere problemi?"
- [Strategie per eseguire il testing](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies)
  - : Successivamente, entriamo nel dettaglio dell’esecuzione dei test, identificando un pubblico di destinazione (ad esempio, quali browser, dispositivi e altri segmenti dovrebbero essere testati), strategie di testing a basso livello tecnologico (procurarsi una gamma di dispositivi e alcune macchine virtuali ed eseguire test ad hoc quando necessario), strategie tecnologiche di livello superiore (automazione, uso di app dedicate al testing), e testing con gruppi di utenti.
- [Gestire i problemi comuni di HTML e CSS](/it/docs/Learn_web_development/Extensions/Testing/HTML_and_CSS)
  - : Con l'ambiente ormai definito, ora esamineremo specificamente i problemi cross-browser comuni che incontrerà nel codice HTML e CSS, e quali strumenti possono essere utilizzati per prevenire il verificarsi dei problemi, o risolvere i problemi esistenti. Ciò include il linting del codice, la gestione dei prefissi CSS, l'uso degli strumenti di sviluppo del browser per individuare i problemi, l'utilizzo di polyfill per aggiungere supporto ai browser, affrontare i problemi di design responsivo, e altro ancora.
- [Implementare la feature detection](/it/docs/Learn_web_development/Extensions/Testing/Feature_detection)
  - : La feature detection consiste nel determinare se un browser supporta un determinato blocco di codice ed eseguire diverso codice a seconda del fatto che lo supporti o meno, in modo che il browser possa sempre fornire un'esperienza di funzionamento piuttosto che arrestarsi o generare errori in alcuni browser. Questo articolo spiega come scrivere la propria semplice feature detection, come utilizzare una libreria per velocizzare l'implementazione, e le funzionalità native per la feature detection come `@supports`.
- [Introduzione al testing automatizzato](/it/docs/Learn_web_development/Extensions/Testing/Automated_testing)
  - : Eseguire manualmente test su diversi browser e dispositivi, più volte al giorno, può diventare tedioso e richiedere molto tempo. Per affrontare questo problema in modo efficiente, dovrebbe familiarizzare con gli strumenti di automazione. In questo articolo, esaminiamo ciò che è disponibile, come utilizzare i task runner, e i fondamenti di come utilizzare app commerciali di automazione per il testing su browser come Sauce Labs e Browser Stack.
- [Configurare il proprio ambiente di automazione dei test](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment)
  - : In questo articolo, Le insegneremo come installare il proprio ambiente di automazione e eseguire i propri test utilizzando Selenium/WebDriver e una libreria per il testing come selenium-webdriver per Node. Esamineremo anche come integrare il proprio ambiente di testing locale con app commerciali come quelle discusse nell'articolo precedente.

{{NextMenu("Learn_web_development/Extensions/Testing/Introduction", "Learn_web_development/Extensions")}}
