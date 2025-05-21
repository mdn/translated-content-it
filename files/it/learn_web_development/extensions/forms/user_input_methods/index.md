---
title: Metodi di input e controlli utente
slug: Learn_web_development/Extensions/Forms/User_input_methods
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I moduli web richiedono l'input dell'utente. Quando si progettano moduli web o qualsiasi contenuto web, è importante considerare come gli utenti interagiscono con i loro dispositivi e browser. L'input degli utenti web va oltre il semplice mouse e tastiera: ad esempio, pensa agli schermi tattili.

In questo articolo, diamo un'occhiata ai diversi modi in cui gli utenti interagiscono con i moduli e altri contenuti web e forniamo raccomandazioni per gestire l'input dell'utente, esempi reali e link a ulteriori informazioni.

Quando sviluppi moduli più complessi e interattivi o altre funzionalità dell'interfaccia utente, ci sono molti elementi HTML e API JavaScript che potresti voler esplorare. Ad esempio, potresti voler creare controlli per moduli personalizzati che richiedono elementi non semantici per essere modificabili come contenuto. Potresti voler supportare gli eventi touch, determinare o controllare l'orientamento dello schermo, fare in modo che un modulo occupi l'intero schermo o abilitare funzionalità di trascinamento e rilascio. Questa guida introduce tutte queste funzionalità e ti indirizza a maggiori informazioni su ciascun argomento.

Per offrire una buona esperienza al maggior numero di utenti possibile, devi supportare diversi metodi di input, inclusi mouse, tastiera, tocco del dito e così via. I meccanismi di input disponibili dipendono dalle capacità del dispositivo che esegue l'applicazione.

È sempre necessario tenere conto dell'accessibilità tramite tastiera: molti utenti web utilizzano solo la tastiera per navigare sui siti web e sulle app, e precludere loro l'uso di alcune funzionalità è una cattiva idea.

## Argomenti trattati

- Per supportare gli schermi touch, gli [eventi touch](/it/docs/Web/API/Touch_events) interpretano l'attività delle dita su interfacce utente touch-based, dai dispositivi mobili ai pannelli dei frigoriferi, fino ai display dei chioschi nei musei.
- L'[API Fullscreen](/it/docs/Web/API/Fullscreen_API) ti permette di visualizzare il tuo contenuto in modalità a schermo intero, necessaria se il tuo modulo viene utilizzato su un frigorifero o un chiosco museale.
- Quando hai bisogno di creare un controllo modulo personalizzato, come un editor di testo arricchito, l'attributo [`contentEditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable) consente la creazione di controlli modificabili partendo da elementi HTML normalmente non modificabili.
- L'[API Drag and Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API) permette agli utenti di trascinare elementi in una pagina e rilasciarli in posizioni diverse. Questo può aiutare a migliorare l'esperienza utente, ad esempio per selezionare file da caricare o riordinare moduli di contenuto all'interno di una pagina.
- Quando l'orientamento dello schermo è importante per il tuo layout, puoi utilizzare le [media query CSS](/it/docs/Web/CSS/@media/orientation) per stilizzare i tuoi moduli in base all'orientamento del browser, o persino utilizzare l'[API Screen Orientation](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation) per leggere lo stato di orientamento dello schermo ed eseguire altre azioni.

Le seguenti sezioni forniscono un insieme di raccomandazioni e migliori pratiche per abilitare il maggior numero possibile di utenti ad utilizzare i tuoi siti web e applicazioni.

## Supporto ai meccanismi di input comuni

### Tastiera

La maggior parte degli utenti utilizzerà una tastiera per inserire dati nei tuoi controlli modulo. Alcuni utilizzeranno anche la tastiera per navigare verso quei controlli del modulo. Per essere accessibili e per una migliore esperienza utente, è importante etichettare correttamente [tutti i controlli del modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form#the_label_input_and_textarea_elements). Quando ogni controllo del modulo ha un'etichetta {{htmlelement("label")}} correttamente associata, il tuo modulo sarà completamente accessibile a tutti, in particolare a chi naviga nel modulo con una tastiera, un lettore di schermo e possibilmente nessuno schermo.

Se desideri aggiungere supporto tastiera aggiuntivo, per esempio validare un controllo modulo quando viene premuto un tasto specifico, puoi utilizzare i gestori di eventi per catturare e rispondere agli eventi della tastiera. Ad esempio, se desideri aggiungere controlli quando viene premuto un tasto, è necessario aggiungere un listener di eventi all'oggetto finestra:

```js
window.addEventListener("keydown", handleKeyDown, true);
window.addEventListener("keyup", handleKeyUp, true);
```

`handleKeyDown` e `handleKeyUp` sono funzioni che definiscono la logica di controllo da eseguire quando vengono attivati gli eventi `keydown` e `keyup`.

> [!NOTE]
> Dai un'occhiata alla [Guida agli eventi](/it/docs/Web/Events) e alla guida su [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent) per ulteriori informazioni sugli eventi tastiera.

### Mouse

Puoi anche catturare eventi del mouse e altri eventi puntatore. Gli eventi che si verificano quando l'utente interagisce con un dispositivo di puntamento come un mouse sono rappresentati dall'interfaccia DOM [`MouseEvent`](/it/docs/Web/API/MouseEvent). Gli eventi comuni del mouse includono [`click`](/it/docs/Web/API/Element/click_event), [`dblclick`](/it/docs/Web/API/Element/dblclick_event), [`mouseup`](/it/docs/Web/API/Element/mouseup_event) e [`mousedown`](/it/docs/Web/API/Element/mousedown_event). L'elenco di tutti gli eventi che utilizzano l'interfaccia MouseEvent è fornito nella [Guida agli eventi](/it/docs/Web/Events).

Quando il dispositivo di input è un mouse, puoi anche controllare l'input utente tramite l'API Pointer Lock e implementare Drag & Drop (vedi sotto). Puoi anche [usare CSS per testare il supporto ai dispositivi di puntamento](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries#use_of_pointing_devices).

### Tocco del dito

Per fornire supporto supplementare ai dispositivi touchscreen, è buona pratica considerare le diverse capacità in termini di risoluzione dello schermo e input dell'utente. Gli [eventi touch](/it/docs/Web/API/Touch_events) possono aiutarti a implementare elementi interattivi e gesti di interazione comuni sui dispositivi touchscreen.

Se desideri usare gli eventi touch, devi aggiungere listener di eventi e specificare le funzioni gestore, che verranno chiamate quando l'evento viene attivato:

```js
element.addEventListener("touchstart", handleStart, false);
element.addEventListener("touchcancel", handleCancel, false);
element.addEventListener("touchend", handleEnd, false);
element.addEventListener("touchmove", handleMove, false);
```

dove `element` è l'elemento DOM su cui desideri registrare gli eventi touch.

> [!NOTE]
> Per ulteriori informazioni su cosa puoi fare con gli eventi touch, leggi la nostra [guida sugli eventi touch](/it/docs/Web/API/Touch_events).

### Eventi Puntatore

I mouse non sono gli unici dispositivi di puntamento. I dispositivi degli utenti possono incorporare diverse forme di input, come mouse, tocco del dito e input tramite penna. Ognuno di questi puntatori ha una dimensione diversa. L'[API Pointer Events](/it/docs/Web/API/Pointer_events) può risultare utile se hai bisogno di gestire eventi su diversi dispositivi normalizzando la gestione di ciascuno. Un puntatore può essere qualsiasi punto di contatto sullo schermo fatto da un cursore del mouse, penna, tocco (incluso il multi-touch) o altro dispositivo di input puntatore.

Gli eventi per gestire l'input puntatore generico assomigliano molto a quelli per il mouse: `pointerdown`, `pointermove`, `pointerup`, `pointerover`, `pointerout`, ecc. L'interfaccia [`PointerEvent`](/it/docs/Web/API/PointerEvent) fornisce tutti i dettagli che puoi desiderare di catturare riguardo al dispositivo di puntamento, inclusa la sua dimensione, pressione e angolazione.

## Implementare controlli

### Orientamento dello Schermo

Se hai bisogno di layout leggermente diversi a seconda che l'utente sia in modalità verticale o orizzontale, puoi utilizzare le [media query CSS](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries#media_feature_rules) per definire CSS per layout diversi o larghezze di controllo modulo in base alla dimensione o orientamento dello schermo quando [stili moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

Quando l'orientamento dello schermo è importante per il tuo modulo, puoi leggere lo stato di orientamento dello schermo, essere informato quando questo stato cambia, e essere in grado di bloccare l'orientamento dello schermo in un determinato stato (di solito verticale o orizzontale) tramite l'[API di Orientamento dello Schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation).

- I dati di orientamento possono essere recuperati tramite [`screenOrientation.type`](/it/docs/Web/API/ScreenOrientation/type) o con CSS tramite la funzionalità media [`orientation`](/it/docs/Web/CSS/@media/orientation).
- Quando l'orientamento dello schermo cambia, l'evento [`change`](/it/docs/Web/API/ScreenOrientation/change_event) viene attivato sull'oggetto schermo.
- Bloccare l'orientamento dello schermo è possibile invocando il metodo [`ScreenOrientation.lock()`](/it/docs/Web/API/ScreenOrientation/lock).
- Il metodo [`ScreenOrientation.unlock()`](/it/docs/Web/API/ScreenOrientation/unlock) rimuove tutti i blocchi schermo precedentemente impostati.

> [!NOTE]
> Maggiori informazioni sull'API di Orientamento dello Schermo possono essere trovate in [Gestire l'orientamento dello schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation).

### Schermo Intero

Se hai bisogno di presentare il tuo modulo in modalità a schermo intero, come quando il tuo modulo è visualizzato su un chiosco museale, casello o veramente qualsiasi interfaccia utente pubblica, è possibile farlo chiamando [`Element.requestFullscreen()`](/it/docs/Web/API/Element/requestFullscreen) su quell'elemento:

```js
const elem = document.getElementById("myForm");
if (elem.requestFullscreen) {
  elem.requestFullscreen();
}
```

> [!NOTE]
> Per saperne di più sull'aggiunta di funzionalità a schermo intero alla tua applicazione, leggi la nostra documentazione sull'[uso della modalità a schermo intero](/it/docs/Web/API/Fullscreen_API).

### Drag & Drop

Un'interazione utente comune è il drag fisico di elementi da essere lasciati altrove sullo schermo. Drag and drop può aiutare a migliorare l'esperienza utente in merito alla selezione di file da caricare o al riordinamento di moduli di contenuto all'interno di una pagina. C'è un'API per questo!

L'[API di Drag & Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API) consente agli utenti di cliccare e tenere premuto il pulsante del mouse su un elemento, trascinarlo in un'altra posizione e rilasciare il pulsante del mouse per lasciare l'elemento lì.

Ecco un esempio che consente di trascinare una sezione di contenuto.

```html
<div
  draggable="true"
  ondragstart="event.dataTransfer.setData('text/plain', 'This text may be dragged')">
  This text <strong>may</strong> be dragged.
</div>
```

in cui:

- Si imposta l'attributo [`draggable`](/it/docs/Web/HTML/Reference/Global_attributes/draggable) su `true` sull'elemento che si desidera rendere trascinabile.
- Si aggiunge un listener per l'evento [`dragstart`](/it/docs/Web/API/HTMLElement/dragstart_event) e si imposta i dati di trascinamento all'interno di questo listener.

> [!NOTE]
> Puoi trovare maggiori informazioni nella [documentazione Drag & Drop di MDN](/it/docs/Web/API/HTML_Drag_and_Drop_API).

### contentEditable

In generale, dovresti usare un {{HTMLElement("textarea")}} o un tipo {{HTMLElement("input")}} appropriato all'interno di un {{HTMLElement("form")}} per raccogliere dati dagli utenti, insieme a una descrittiva {{HTMLElement("label")}}. Tuttavia, questi elementi potrebbero non soddisfare le tue esigenze. Ad esempio, gli editor di testo arricchito catturano testo in corsivo, grassetto e normale, ma nessun controllo modulo nativo cattura testo arricchito. Questo caso richiede la creazione di un controllo personalizzato che sia stilizzabile _e_ modificabile. C'è un attributo per questo!

Qualsiasi elemento DOM può essere reso direttamente modificabile utilizzando l'attributo [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable).

```css hidden
div {
  width: 300px;
  height: 130px;
  border: 1px solid gray;
}
```

```html
<div contenteditable="true">This text can be edited by the user.</div>
```

L'attributo `contenteditable` aggiunge automaticamente l'elemento all'ordine di tabulazione predefinito del documento, il che significa che l'attributo [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) non deve essere aggiunto. Tuttavia, quando si utilizzano elementi non semantici per l'inserimento dei dati durante la [creazione dei propri controlli modulo](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls), sarà necessario aggiungere JavaScript e [ARIA](/it/docs/Web/Accessibility/ARIA) per adattare l'elemento con funzionalità di controllo del modulo per tutto il resto.

Per offrire una buona esperienza utente, qualsiasi controllo modulo personalizzato che crei deve essere accessibile e funzionare come i controlli modulo nativi:

- Il [`ruolo`](/it/docs/Web/Accessibility/ARIA/Reference/Roles), l'[etichetta](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) e la [descrizione](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby) dell'elemento devono essere aggiunti con ARIA.
- Tutti i metodi di input utente devono essere supportati, inclusi eventi di [tastiera](#tastiera), [mouse](#mouse), [tocco](#tocco_del_dito) e [puntatore](#eventi_puntatore), come descritto sopra.
- È necessario JavaScript per gestire funzionalità come [validazione](/it/docs/Learn_web_development/Extensions/Forms/Form_validation), [invio](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) e [salvataggio](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript) dei contenuti aggiornati dall'utente.

{{EmbedLiveSample("contentEditable")}}

> [!NOTE]
> Esempi e altre risorse possono essere trovati nella [Guida al Content Editable](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable).

## Tutorial

- [Guida agli eventi touch](/it/docs/Web/API/Touch_events)
- [Gestire l'orientamento dello schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation)
- [Usare la modalità a schermo intero](/it/docs/Web/API/Fullscreen_API)
- [Guida alle operazioni di trascinamento](/it/docs/Web/API/HTML_Drag_and_Drop_API/Drag_operations)
- [Validazione del modulo](/it/docs/Learn_web_development/Extensions/Forms/Form_validation)
- [Inviare moduli tramite JavaScript](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript)

## Riferimenti

- Interfaccia [`MouseEvent`](/it/docs/Web/API/MouseEvent)
- Interfaccia [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent)
- API [Eventi Touch](/it/docs/Web/API/Touch_events)
- API [Pointer Lock](/it/docs/Web/API/Pointer_Lock_API)
- API [Orientamento dello Schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation)
- API [Fullscreen](/it/docs/Web/API/Fullscreen_API)
- API [Drag & Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API)
- Attributo HTML [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable)
