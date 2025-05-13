---
title: Moduli Web
slug: Learn_web_development/Extensions/Forms
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions")}}

Questo modulo fornisce una serie di articoli che aiuteranno a comprendere a fondo gli elementi essenziali dei moduli web. I moduli web sono uno strumento molto potente per interagire con gli utenti — più comunemente vengono usati per raccogliere dati dagli utenti o per permettere loro di controllare un'interfaccia utente. Tuttavia, per ragioni storiche e tecniche, non è sempre ovvio come usarli al loro pieno potenziale. Negli articoli elencati di seguito, copriremo tutti gli aspetti essenziali dei moduli Web, inclusa la definizione della loro struttura HTML, la stilizzazione dei controlli del modulo, la validazione dei dati del modulo e l'invio dei dati al server.

## Prerequisiti

Prima di iniziare questo modulo, dovrebbe almeno seguire la nostra [Introduzione a HTML](/it/docs/Learn_web_development/Core/Structuring_content). A questo punto, dovrebbe trovare i [Tutorial introduttivi](#tutorial_introduttivi) facili da capire e dovrebbe essere anche in grado di usare il nostro tutorial sui [Controlli di modulo nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls).

Tuttavia, padroneggiare i moduli richiede più della sola conoscenza di HTML — è necessario anche apprendere alcune tecniche specifiche per stilizzare i controlli del modulo, e avere alcune conoscenze di scripting è necessario per gestire elementi come la validazione e la creazione di controlli del modulo personalizzati. Pertanto, prima di esaminare le altre sezioni elencate di seguito, si consiglia di dedicare del tempo per imparare un po' di [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

Il testo sopra è un buon indicatore del perché abbiamo inserito i moduli web in un modulo autonomo, piuttosto che cercare di mescolare parti di esso nelle aree tematiche di HTML, CSS e JavaScript — gli elementi di modulo sono più complessi della maggior parte degli altri elementi HTML, e richiedono anche un'integrazione stretta con le relative tecniche CSS e JavaScript per ottenere il massimo da essi.

> [!NOTE]
> Se sta lavorando su un computer/tablet/altro dispositivo dove non ha la possibilità di creare i propri file, potrebbe provare (la maggior parte di) gli esempi di codice in un programma di codifica online come [JS Bin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial introduttivi

- [Il suo primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form)
  - : Il primo articolo della nostra serie fornisce la sua prima esperienza nella creazione di un modulo web, includendo la progettazione di un modulo semplice, l'implementazione usando i giusti elementi HTML, l'aggiunta di uno stile molto semplice tramite CSS e come i dati vengono inviati a un server.
- [Come strutturare un modulo web](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form)
  - : Dopo aver affrontato le basi, ora esaminiamo in dettaglio gli elementi utilizzati per fornire struttura e significato alle diverse parti di un modulo.

## I diversi controlli del modulo

- [Controlli di modulo nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls)
  - : Iniziamo questa sezione esaminando in dettaglio la funzionalità dei tipi originali di {{htmlelement("input")}} HTML, guardando quali opzioni sono disponibili per raccogliere diversi tipi di dati.
- [I tipi di input di HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types)
  - : Qui continuiamo la nostra analisi approfondita dell'elemento `<input>`, esaminando i tipi di input aggiuntivi forniti quando HTML5 è stato rilasciato, e i vari controlli dell'interfaccia utente e miglioramenti della raccolta dati che offrono. Inoltre, esaminiamo l'elemento {{htmlelement('output')}}.
- [Altri controlli del modulo](/it/docs/Learn_web_development/Extensions/Forms/Other_form_controls)
  - : Successivamente diamo un'occhiata a tutti i controlli di modulo non-`<input>` e strumenti associati, come {{htmlelement('select')}}, {{htmlelement('textarea')}}, {{htmlelement('meter')}}, e {{htmlelement('progress')}}.

## Tutorial sullo stile dei moduli

- [Stilizzazione dei moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms)
  - : Questo articolo fornisce un'introduzione alla stilizzazione dei moduli con CSS, inclusi tutti i fondamenti che potrebbe aver bisogno di sapere per compiti di stilizzazione di base.
- [Stilizzazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling)
  - : Qui esaminiamo alcune tecniche avanzate di stilizzazione dei moduli che devono essere utilizzate quando si cerca di gestire alcuni degli elementi del modulo più difficili da stilizzare.
- [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select)
  - : Questo articolo spiega come usare caratteristiche moderne dedicate di HTML e CSS insieme per creare elementi `<select>` completamente personalizzati. Questo include avere il pieno controllo sullo stile del pulsante select, del selettore a discesa, dell'icona della freccia, del segno di spunta della selezione corrente e di ogni singolo elemento `<option>`.
- [Pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes)
  - : Un'introduzione alle pseudo-classi UI che consentono di indirizzare i controlli del modulo HTML in base al loro stato attuale.

## Validazione e invio dei dati del modulo

- [Validazione lato client del modulo](/it/docs/Learn_web_development/Extensions/Forms/Form_validation)
  - : Inviare dati non è sufficiente — dobbiamo anche assicurarci che i dati inseriti dagli utenti nei moduli siano nel formato corretto per elaborarli con successo e che non rompano le nostre applicazioni. Vogliamo anche aiutare i nostri utenti a compilare correttamente i nostri moduli e non frustrare durante l'uso delle nostre app. La validazione dei moduli ci aiuta a raggiungere questi obiettivi — questo articolo spiega ciò che deve sapere.
- [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)
  - : Questo articolo esamina cosa accade quando un utente invia un modulo — dove vanno i dati e come li gestiamo una volta che arrivano. Esaminiamo anche alcune preoccupazioni sulla sicurezza associate all'invio dei dati del modulo.

## Articoli aggiuntivi

I seguenti articoli non sono inclusi nel percorso di apprendimento, ma risulteranno interessanti e utili quando avrà padroneggiato le tecniche sopra e vorrà sapere di più.

- [Come costruire controlli modulo personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls)
  - : Potrà incontrare alcuni casi in cui i widget di modulo nativi non offrono ciò di cui ha bisogno, ad esempio, a causa di stile o funzionalità. In tali casi, potrebbe essere necessario costruire il proprio widget di modulo utilizzando HTML grezzo. Questo articolo spiega come fare e le considerazioni di cui deve essere consapevole nel farlo, con uno studio di caso pratico.
- [Invio dei moduli tramite JavaScript](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript)
  - : Questo articolo esamina modi per usare un modulo per assemblare una richiesta HTTP e inviarla tramite JavaScript personalizzato, anziché l'invio standard del modulo. Analizza anche perché potrebbe voler fare questo, e le implicazioni di farlo. (Veda anche [Usare oggetti FormData](/it/docs/Web/API/XMLHttpRequest_API/Using_FormData_Objects).)
- [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select)
  - : Questo articolo spiega come usare caratteristiche moderne dedicate di HTML e CSS insieme per creare elementi {{htmlelement("select")}} completamente personalizzati.

## Vedere anche

- [Riferimento agli elementi del modulo HTML](/it/docs/Web/HTML/Reference/Elements#forms)
- [Riferimento ai tipi di HTML \<input>](/it/docs/Web/HTML/Reference/Elements/input)
- [Riferimento agli attributi HTML](/it/docs/Web/HTML/Reference/Attributes)
- [Metodi e controlli di input utente](/it/docs/Learn_web_development/Extensions/Forms/User_input_methods)

{{NextMenu("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions")}}
