---
title: Moduli web
slug: Learn_web_development/Extensions/Forms
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions")}}

Questo modulo fornisce una serie di articoli che ti aiuteranno a padroneggiare gli elementi essenziali dei moduli web. I moduli web sono uno strumento molto potente per interagire con gli utenti — più comunemente vengono utilizzati per raccogliere dati dagli utenti o per permettere loro di controllare un'interfaccia utente. Tuttavia, per motivi storici e tecnici, non è sempre ovvio come utilizzarli al loro pieno potenziale. Negli articoli elencati di seguito, copriremo tutti gli aspetti essenziali dei moduli web, inclusa la creazione della loro struttura HTML, la formattazione dei controlli modulo, la validazione dei dati dei moduli e l'invio dei dati al server.

## Prerequisiti

Prima di iniziare questo modulo, dovresti almeno seguire la nostra [Introduzione all'HTML](/it/docs/Learn_web_development/Core/Structuring_content). A questo punto dovresti trovare i [Tutorial introduttivi](#tutorial_introduttivi) facili da comprendere, e riuscire anche a utilizzare il nostro tutorial sui [Controlli di base dei moduli nativi](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls).

Padroneggiare i moduli richiede però più della sola conoscenza dell'HTML — è necessario anche imparare alcune tecniche specifiche per formattare i controlli dei moduli, e alcune conoscenze di scripting sono necessarie per gestire cose come la validazione e la creazione di controlli modulo personalizzati. Pertanto, prima di esaminare le altre sezioni elencate di seguito, ti consiglieremmo di studiare un po' di [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

Il testo sopra è un buon indicatore del motivo per cui abbiamo inserito i moduli web in un modulo autonomo, piuttosto che cercare di mescolare parti di esso nelle aree tematiche di HTML, CSS e JavaScript — gli elementi di modulo sono più complessi della maggior parte degli altri elementi HTML e richiedono anche un forte legame con le tecniche correlate di CSS e JavaScript per sfruttarli al meglio.

> [!NOTE]
> Se stai lavorando su un computer/tablet/altro dispositivo dove non puoi creare i tuoi file, puoi provare (la maggior parte) degli esempi di codice in un programma di coding online come [JS Bin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial introduttivi

- [Il tuo primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form)
  - : Il primo articolo della nostra serie ti offre la tua prima esperienza nella creazione di un modulo web, incluso il design di un modulo semplice, la sua implementazione utilizzando gli elementi HTML corretti, l'aggiunta di uno stile molto semplice tramite CSS e come i dati vengono inviati a un server.
- [Come strutturare un modulo web](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form)
  - : Messo da parte il minimo indispensabile, ora esaminiamo in dettaglio gli elementi utilizzati per fornire struttura e significato alle diverse parti di un modulo.

## I diversi controlli modulo

- [Controlli di base dei moduli nativi](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls)
  - : Iniziamo questa sezione esaminando in dettaglio la funzionalità dei tipi originali di {{htmlelement("input")}} dell'HTML, osservando quali opzioni sono disponibili per raccogliere diversi tipi di dati.
- [I tipi di input di HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types)
  - : Qui continuiamo il nostro approfondimento sull'elemento `<input>`, esaminando i tipi di input aggiuntivi forniti con il rilascio di HTML5 e i vari controlli dell'interfaccia utente e i miglioramenti nella raccolta dati che offrono. Inoltre, esaminiamo l'elemento {{htmlelement('output')}}.
- [Altri controlli modulo](/it/docs/Learn_web_development/Extensions/Forms/Other_form_controls)
  - : Successivamente esaminiamo tutti i controlli di modulo non-`<input>` e gli strumenti associati, come {{htmlelement('select')}}, {{htmlelement('textarea')}}, {{htmlelement('meter')}}, e {{htmlelement('progress')}}.

## Tutorial sulla formattazione dei moduli

- [Formattare i moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms)
  - : Questo articolo fornisce un'introduzione alla formattazione dei moduli con CSS, includendo tutte le basi che potresti dover sapere per compiti di formattazione di base.
- [Formattazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling)
  - : Qui esploriamo alcune tecniche avanzate di formattazione dei moduli che devono essere utilizzate quando si cerca di gestire alcuni degli elementi di modulo più difficili da formattare.
- [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select)
  - : Questo articolo spiega come utilizzare insieme funzionalità HTML e CSS moderne e dedicate per creare elementi `<select>` completamente personalizzati. Questo include avere il pieno controllo sulla formattazione del pulsante select, del menu a tendina, dell'icona della freccia, del segno di spunta della selezione corrente e di ciascun elemento `<option>`.
- [Pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes)
  - : Un'introduzione alle pseudo-classi UI che permettono di indirizzare i controlli modulo HTML in base al loro stato attuale.

## Validazione e invio dei dati dei moduli

- [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation)
  - : Inviare dati non basta — dobbiamo anche assicurarci che i dati inseriti dagli utenti nei moduli siano nel formato corretto per essere processati correttamente, e che non mandino in crash le nostre applicazioni. Vogliamo anche aiutare i nostri utenti a compilare correttamente i nostri moduli e non farli frustrate mentre cercano di utilizzare le nostre app. La validazione dei moduli ci aiuta a raggiungere questi obiettivi — questo articolo ti dice ciò che devi sapere.
- [Invio dei dati dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)
  - : Questo articolo esamina ciò che accade quando un utente invia un modulo — dove vanno i dati e come li gestiamo quando arrivano? Esaminiamo anche alcune delle preoccupazioni di sicurezza associate all'invio dei dati dei moduli.

## Articoli aggiuntivi

I seguenti articoli non sono inclusi nel percorso di apprendimento, ma si riveleranno interessanti e utili quando avrai padroneggiato le tecniche sopra e vorrai saperne di più.

- [Come costruire controlli modulo personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls)
  - : Ti imbatterai in alcuni casi in cui i widget modulo nativi non forniscono ciò di cui hai bisogno, ad esempio a causa della formattazione o della funzionalità. In questi casi, potresti dover costruire il tuo widget modulo a partire da HTML grezzo. Questo articolo spiega come farlo e le considerazioni di cui devi essere a conoscenza mentre lo fai, con un caso di studio pratico.
- [Inviando moduli tramite JavaScript](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript)
  - : Questo articolo esamina i modi per utilizzare un modulo per assemblare una richiesta HTTP e inviarla tramite JavaScript personalizzato, piuttosto che la sottomissione standard del modulo. Esamina anche perché potresti voler farlo e le implicazioni di farlo. (Vedi anche [Utilizzare gli oggetti FormData](/it/docs/Web/API/XMLHttpRequest_API/Using_FormData_Objects).)
- [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select)
  - : Questo articolo spiega come utilizzare insieme funzionalità HTML e CSS moderne e dedicate per creare elementi {{htmlelement("select")}} completamente personalizzati.

## Vedi anche

- [Riferimento agli elementi dei moduli HTML](/it/docs/Web/HTML/Reference/Elements#forms)
- [Riferimento ai tipi di \<input> HTML](/it/docs/Web/HTML/Reference/Elements/input)
- [Riferimento agli attributi HTML](/it/docs/Web/HTML/Reference/Attributes)
- [Metodi di input utente e controlli](/it/docs/Learn_web_development/Extensions/Forms/User_input_methods)

{{NextMenu("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions")}}
