---
title: Moduli web
slug: Learn_web_development/Extensions/Forms
l10n:
  sourceCommit: 76936e1d9ff271ac59307a0f858d0d7b57f3866a
---

{{NextMenu("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions")}}

Questo modulo fornisce una serie di articoli che aiuteranno a padroneggiare gli aspetti essenziali dei moduli web. I moduli web sono uno strumento molto potente per interagire con gli utenti: vengono usati principalmente per raccogliere dati dagli utenti o per consentire loro di controllare un'interfaccia utente. Tuttavia, per ragioni storiche e tecniche, non è sempre evidente come utilizzarli al massimo delle loro potenzialità. Negli articoli elencati di seguito verranno trattati tutti gli aspetti essenziali dei moduli web, inclusi il markup della loro struttura HTML, lo stile dei controlli dei moduli, la convalida dei dati dei moduli e l'invio dei dati al server.

## Prerequisiti

Prima di iniziare questo modulo, è consigliabile aver almeno completato la nostra [Introduzione a HTML](/it/docs/Learn_web_development/Core/Structuring_content). A questo punto, i [tutorial introduttivi](#tutorial-introduttivi) dovrebbero risultare facili da comprendere e dovrebbe essere possibile utilizzare anche il nostro tutorial sui [controlli dei moduli nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls).

Padroneggiare i moduli richiede tuttavia più della sola conoscenza di HTML: è necessario imparare anche alcune tecniche specifiche per applicare lo stile ai controlli dei moduli, oltre a conoscenze di scripting per gestire aspetti quali la convalida e la creazione di controlli dei moduli personalizzati. Pertanto, prima di consultare le altre sezioni elencate di seguito, è consigliabile approfondire prima [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

Il testo precedente è un buon indicatore del motivo per cui i moduli web sono stati inseriti in un modulo autonomo, invece di cercare di distribuirne parti nelle aree tematiche HTML, CSS e JavaScript: gli elementi dei moduli sono più complessi della maggior parte degli altri elementi HTML e richiedono inoltre una stretta combinazione di tecniche CSS e JavaScript correlate per sfruttarli al meglio.

> [!NOTE]
> Se si lavora su un computer, tablet o altro dispositivo su cui non è possibile creare file, è possibile provare a eseguire il codice in un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/).

## Tutorial introduttivi

- [Il primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form)
  - : Il primo articolo della serie offre la prima esperienza nella creazione di un modulo web, inclusa la progettazione di un modulo semplice, la sua implementazione tramite gli elementi HTML appropriati, l'aggiunta di uno stile molto semplice con CSS e l'invio dei dati a un server.
- [Come strutturare un modulo web](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form)
  - : Dopo aver trattato le basi, vengono ora esaminati più in dettaglio gli elementi usati per fornire struttura e significato alle diverse parti di un modulo.

## I diversi controlli dei moduli

- [Controlli dei moduli nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls)
  - : Questa sezione inizia esaminando in dettaglio la funzionalità dei tipi HTML {{htmlelement("input")}} originali, osservando quali opzioni sono disponibili per raccogliere diversi tipi di dati.
- [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types)
  - : Qui continua l'analisi approfondita dell'elemento `<input>`, esaminando i tipi di input aggiuntivi introdotti con HTML5 e i vari controlli dell'interfaccia utente e miglioramenti per la raccolta dei dati che offrono. Inoltre, viene esaminato l'elemento {{htmlelement('output')}}.
- [Altri controlli dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Other_form_controls)
  - : Successivamente vengono esaminati tutti i controlli dei moduli non `<input>` e gli strumenti associati, come {{htmlelement('select')}}, {{htmlelement('textarea')}}, {{htmlelement('meter')}} e {{htmlelement('progress')}}.

## Tutorial sullo stile dei moduli

- [Applicare lo stile ai moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms)
  - : Questo articolo fornisce un'introduzione all'applicazione dello stile ai moduli con CSS, incluse tutte le basi necessarie per le attività di styling di base.
- [Stile avanzato dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling)
  - : Qui vengono esaminate alcune tecniche più avanzate per lo stile dei moduli, necessarie quando si cerca di gestire alcuni degli elementi dei moduli più difficili da stilizzare.
- [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select)
  - : Questo articolo spiega come usare insieme funzionalità HTML e CSS moderne e dedicate per creare elementi `<select>` completamente personalizzati. Ciò include il controllo completo dello stile del pulsante select, del selettore a discesa, dell'icona della freccia, del segno di spunta della selezione corrente e di ogni singolo elemento `<option>`.
- [Listbox select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select_listboxes)
  - : Questo articolo prosegue il precedente, esaminando come applicare lo stile agli elementi `<select>` listbox personalizzabili.
- [Pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes)
  - : Un'introduzione alle pseudo-classi UI che consentono di selezionare i controlli dei moduli HTML in base al loro stato attuale.

## Convalida e invio dei dati dei moduli

- [Convalida dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation)
  - : Inviare dati non è sufficiente: occorre anche assicurarsi che i dati inseriti dagli utenti nei moduli siano nel formato corretto per poterli elaborare correttamente e che non compromettano le applicazioni. Si desidera inoltre aiutare gli utenti a compilare correttamente i moduli, evitando che si frustrino durante l'uso delle app. La convalida dei moduli aiuta a raggiungere questi obiettivi: questo articolo spiega ciò che è necessario sapere.
- [Inviare i dati dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)
  - : Questo articolo esamina cosa accade quando un utente invia un modulo: dove vanno i dati e come vengono gestiti quando arrivano a destinazione? Vengono inoltre esaminate alcune problematiche di sicurezza associate all'invio dei dati dei moduli.

## Tutorial aggiuntivi

I seguenti articoli non sono inclusi nel percorso di apprendimento, ma risulteranno interessanti e utili una volta padroneggiate le tecniche precedenti e si desideri saperne di più.

- [Come creare controlli dei moduli personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls)
  - : Possono presentarsi casi in cui i widget dei moduli nativi non offrono ciò che serve, ad esempio per ragioni di stile o funzionalità. In tali casi, potrebbe essere necessario creare un proprio widget per moduli a partire da HTML puro. Questo articolo spiega come farlo e quali aspetti considerare, con un caso di studio pratico.
- [Inviare moduli tramite JavaScript](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript)
  - : Questo articolo esamina i modi per usare un modulo per assemblare una richiesta HTTP e inviarla tramite JavaScript personalizzato, invece dell'invio standard dei moduli. Esamina inoltre i motivi per cui potrebbe essere opportuno farlo e le relative implicazioni. (Vedere anche [Usare gli oggetti FormData](/it/docs/Web/API/XMLHttpRequest_API/Using_FormData_Objects).)
- [Moduli HTML nei browser legacy](/it/docs/Learn_web_development/Extensions/Forms/HTML_forms_in_legacy_browsers)
  - : Questo articolo fornisce suggerimenti e trucchi per ridurre le difficoltà nel caso in cui sia necessario supportare browser legacy con i moduli HTML.
- [Metodi e controlli di input dell'utente](/it/docs/Learn_web_development/Extensions/Forms/User_input_methods)
  - : Questo articolo illustra i diversi modi in cui gli utenti interagiscono con i moduli e altri contenuti web, e fornisce raccomandazioni per la gestione dell'input dell'utente, esempi reali e collegamenti a ulteriori informazioni.

## Vedere anche

- [Riferimento agli elementi dei moduli HTML](/it/docs/Web/HTML/Reference/Elements#forms)
- [Riferimento ai tipi HTML `<input>`](/it/docs/Web/HTML/Reference/Elements/input)
- [Riferimento agli attributi HTML](/it/docs/Web/HTML/Reference/Attributes)

{{NextMenu("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions")}}
