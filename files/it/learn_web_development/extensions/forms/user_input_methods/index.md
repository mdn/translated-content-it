---
title: Metodi e controlli di input utente
short-title: Metodi e controlli dell'interfaccia utente
slug: Learn_web_development/Extensions/Forms/User_input_methods
l10n:
  sourceCommit: ad9776a6cf53eaf570ac0515402247e82ecefcfe
---

I moduli web richiedono l'input dell'utente. Quando si progettano moduli web, o in realtà qualsiasi contenuto web, è importante considerare il modo in cui gli utenti interagiscono con i propri dispositivi e browser. L'input utente sul web va oltre il semplice mouse e la tastiera: si pensi, per esempio, ai touchscreen.

In questo articolo vengono esaminati i diversi modi in cui gli utenti interagiscono con i moduli e altri contenuti web e vengono fornite raccomandazioni per gestire l'input utente, esempi reali e collegamenti a ulteriori informazioni.

Nello sviluppo di moduli più complessi e interattivi o di altre funzionalità dell'interfaccia utente, esistono molti elementi HTML e API JavaScript che può essere utile approfondire. Per esempio, potrebbe essere necessario creare controlli modulo personalizzati che richiedono che elementi non semantici siano modificabili. Potrebbe essere necessario supportare eventi touch, determinare o controllare l'orientamento dello schermo, visualizzare un modulo a schermo intero oppure abilitare funzionalità di trascinamento e rilascio. Questa guida introduce tutte queste funzionalità e rimanda a maggiori informazioni su ciascun argomento.

Per offrire una buona esperienza al maggior numero possibile di utenti, è necessario supportare più metodi di input, inclusi mouse, tastiera, tocco con le dita e così via. I meccanismi di input disponibili dipendono dalle capacità del dispositivo su cui viene eseguita l'applicazione.

Occorre sempre prestare attenzione all'accessibilità tramite tastiera: molti utenti web usano solo la tastiera per navigare siti web e app, e impedire loro di accedere alle funzionalità non è una buona idea.

## Argomenti trattati

- Per supportare i display touchscreen, gli [eventi touch](/it/docs/Web/API/Touch_events) interpretano l'attività delle dita sulle interfacce utente basate sul tocco, dai dispositivi mobili ai pannelli dei frigoriferi, fino agli schermi dei chioschi museali.
- La [Fullscreen API](/it/docs/Web/API/Fullscreen_API) consente di visualizzare i contenuti in modalità schermo intero, necessaria se il modulo viene visualizzato su un frigorifero o su un chiosco museale.
- Quando è necessario creare un controllo modulo personalizzato, come un editor di testo avanzato, l'attributo [`contentEditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable) consente di creare controlli modificabili a partire da elementi HTML normalmente non modificabili.
- La [Drag and Drop API](/it/docs/Web/API/HTML_Drag_and_Drop_API) consente agli utenti di trascinare elementi in una pagina e rilasciarli in posizioni diverse. Ciò può contribuire a migliorare l'esperienza utente nella selezione di file da caricare o nel riordinamento di moduli di contenuto all'interno di una pagina.
- Quando l'orientamento dello schermo è importante per il layout, è possibile usare le [media query CSS](/it/docs/Web/CSS/Reference/At-rules/@media/orientation) per applicare stili ai moduli in base all'orientamento del browser, oppure usare la [Screen Orientation API](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation) per leggere lo stato dell'orientamento dello schermo ed eseguire altre azioni.

Le sezioni seguenti forniscono un insieme di raccomandazioni e buone pratiche per consentire al più ampio insieme possibile di utenti di utilizzare siti web e applicazioni.

## Supportare i meccanismi di input comuni

### Tastiera

La maggior parte degli utenti utilizzerà una tastiera per immettere dati nei controlli del modulo. Alcuni utilizzeranno inoltre la tastiera per raggiungere tali controlli. Per garantire l'accessibilità e una migliore esperienza utente, è importante [etichettare correttamente tutti i controlli modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form#the_label_input_and_textarea_elements). Quando ciascun controllo modulo ha un {{htmlelement("label")}} associato correttamente, il modulo sarà completamente accessibile a tutti, in particolare a chi naviga il modulo con una tastiera, un lettore di schermo e possibilmente senza alcuno schermo.

Se si desidera aggiungere ulteriore supporto per la tastiera, ad esempio convalidare un controllo modulo quando viene premuto un tasto specifico, è possibile usare event listener per acquisire e gestire gli eventi della tastiera. Per esempio, se si desidera aggiungere controlli quando viene premuto un tasto qualsiasi, occorre aggiungere un event listener all'oggetto window:

```js
window.addEventListener("keydown", handleKeyDown);
window.addEventListener("keyup", handleKeyUp);
```

`handleKeyDown` e `handleKeyUp` sono funzioni che definiscono la logica di controllo da eseguire quando vengono attivati gli eventi `keydown` e `keyup`.

> [!NOTE]
> Consultare la guida agli [eventi DOM](/it/docs/Web/API/Document_Object_Model/Events) e il riferimento a [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent) per ulteriori informazioni sugli eventi della tastiera.

### Mouse

È inoltre possibile acquisire eventi del mouse e di altri puntatori. Gli eventi che si verificano quando l'utente interagisce con un dispositivo di puntamento, come un mouse, sono rappresentati dall'interfaccia DOM [`MouseEvent`](/it/docs/Web/API/MouseEvent). Gli eventi comuni del mouse includono [`click`](/it/docs/Web/API/Element/click_event), [`dblclick`](/it/docs/Web/API/Element/dblclick_event), [`mouseup`](/it/docs/Web/API/Element/mouseup_event) e [`mousedown`](/it/docs/Web/API/Element/mousedown_event). L'elenco di tutti gli eventi che usano l'interfaccia Mouse Event è disponibile nella guida agli [eventi DOM](/it/docs/Web/API/Document_Object_Model/Events#event_index).

Quando il dispositivo di input è un mouse, è inoltre possibile controllare l'input dell'utente tramite la Pointer Lock API e implementare il Drag & Drop (vedere sotto). È anche possibile [usare CSS per verificare il supporto del dispositivo di puntamento](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries#use_of_pointing_devices).

### Tocco con le dita

Per fornire supporto aggiuntivo ai dispositivi touchscreen, è buona pratica tenere in considerazione le diverse capacità in termini di risoluzione dello schermo e input utente. Gli [eventi touch](/it/docs/Web/API/Touch_events) possono aiutare a implementare elementi interattivi e gesti di interazione comuni sui dispositivi touchscreen.

Per usare gli eventi touch, occorre aggiungere event listener e specificare funzioni handler, che verranno chiamate quando l'evento viene attivato:

```js
element.addEventListener("touchstart", handleStart);
element.addEventListener("touchcancel", handleCancel);
element.addEventListener("touchend", handleEnd);
element.addEventListener("touchmove", handleMove);
```

dove `element` è l'elemento DOM su cui si desidera registrare gli eventi touch.

> [!NOTE]
> Per ulteriori informazioni sulle possibilità offerte dagli eventi touch, leggere la nostra [guida agli eventi touch](/it/docs/Web/API/Touch_events).

### Eventi puntatore

I mouse non sono gli unici dispositivi di puntamento. I dispositivi degli utenti possono incorporare più forme di input, come mouse, tocco con le dita e input tramite penna. Ciascuno di questi puntatori ha dimensioni diverse. La [Pointer Events API](/it/docs/Web/API/Pointer_events) può essere utile quando è necessario gestire eventi tra dispositivi diversi normalizzando la gestione di ciascuno. Un puntatore può essere qualsiasi punto di contatto sullo schermo creato da un cursore del mouse, una penna, un tocco, incluso il multi-touch, o un altro dispositivo di input di puntamento.

Gli eventi per la gestione dell'input generico del puntatore sono molto simili a quelli del mouse: `pointerdown`, `pointermove`, `pointerup`, `pointerover`, `pointerout` e così via. L'[interfaccia `PointerEvent`](/it/docs/Web/API/PointerEvent) fornisce tutti i dettagli che può essere necessario acquisire sul dispositivo di puntamento, incluse dimensioni, pressione e angolazione.

## Implementare controlli

### Orientamento dello schermo

Se sono necessari layout leggermente diversi a seconda che l'utente si trovi in modalità verticale o orizzontale, è possibile usare le [media query CSS](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries#media_feature_rules) per definire CSS per diversi layout o larghezze dei controlli modulo in base alle dimensioni o all'orientamento dello schermo durante l'[applicazione di stili ai moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

Quando l'orientamento dello schermo è importante per il modulo, è possibile leggere lo stato dell'orientamento dello schermo, essere informati quando questo stato cambia e bloccare l'orientamento dello schermo in uno stato specifico, solitamente verticale od orizzontale, tramite la [Screen Orientation API](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation).

- I dati sull'orientamento possono essere recuperati tramite [`screenOrientation.type`](/it/docs/Web/API/ScreenOrientation/type) o con CSS attraverso la funzionalità media [`orientation`](/it/docs/Web/CSS/Reference/At-rules/@media/orientation).
- Quando l'orientamento dello schermo cambia, l'evento [`change`](/it/docs/Web/API/ScreenOrientation/change_event) viene attivato sull'oggetto screen.
- Il blocco dell'orientamento dello schermo è possibile invocando il metodo [`ScreenOrientation.lock()`](/it/docs/Web/API/ScreenOrientation/lock).
- Il metodo [`ScreenOrientation.unlock()`](/it/docs/Web/API/ScreenOrientation/unlock) rimuove tutti i blocchi dello schermo impostati in precedenza.

> [!NOTE]
> Maggiori informazioni sulla Screen Orientation API sono disponibili in [Gestire l'orientamento dello schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation).

### Schermo intero

Se è necessario presentare il modulo in modalità schermo intero, come quando viene visualizzato su un chiosco museale, un casello autostradale o, in realtà, qualsiasi interfaccia utente visualizzata pubblicamente, è possibile farlo chiamando [`Element.requestFullscreen()`](/it/docs/Web/API/Element/requestFullscreen) su tale elemento:

```js
const elem = document.getElementById("myForm");
if (elem.requestFullscreen) {
  elem.requestFullscreen();
}
```

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta della funzionalità schermo intero a un'applicazione, leggere la documentazione sull'[uso della modalità schermo intero](/it/docs/Web/API/Fullscreen_API).

### Drag & Drop

Un'interazione utente comune consiste nel trascinamento fisico di elementi per rilasciarli altrove sullo schermo. Il trascinamento e rilascio può contribuire a migliorare l'esperienza utente nella selezione di file da caricare o nel riordinamento di moduli di contenuto all'interno di una pagina. Esiste un'API per questo!

L'API [Drag & Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API) consente agli utenti di fare clic e mantenere premuto il pulsante del mouse su un elemento, trascinarlo in un'altra posizione e rilasciare il pulsante del mouse per rilasciare l'elemento in quel punto.

Ecco un esempio che consente di trascinare una sezione di contenuto.

```html
<div draggable="true">This text <strong>may</strong> be dragged.</div>
```

```js
document.querySelector("div").addEventListener("dragstart", (event) => {
  event.dataTransfer.setData("text/plain", "This text may be dragged.");
});
```

in cui:

- Si imposta l'attributo [`draggable`](/it/docs/Web/HTML/Reference/Global_attributes/draggable) su `true` per l'elemento che si desidera rendere trascinabile.
- Si aggiunge un listener per l'evento [`dragstart`](/it/docs/Web/API/HTMLElement/dragstart_event) e si impostano i dati di trascinamento all'interno di questo listener.

> [!NOTE]
> Ulteriori informazioni sono disponibili nella [documentazione MDN su Drag & Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API).

### contentEditable

In generale, per raccogliere dati dagli utenti è opportuno usare un {{HTMLElement("textarea")}} oppure un tipo {{HTMLElement("input")}} appropriato all'interno di un {{HTMLElement("form")}}, insieme a un {{HTMLElement("label")}} descrittivo. Tuttavia, questi elementi potrebbero non soddisfare le esigenze. Per esempio, gli editor di testo avanzato acquisiscono testo in corsivo, grassetto e normale, ma nessun controllo modulo nativo acquisisce testo avanzato. Questo caso d'uso richiede la creazione di un controllo personalizzato che sia stilizzabile _e_ modificabile. Esiste un attributo per questo!

Qualsiasi elemento DOM può essere reso direttamente modificabile usando l'attributo [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable).

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

L'attributo `contenteditable` aggiunge automaticamente l'elemento all'ordine di navigazione tramite tabulazione predefinito del documento, pertanto non è necessario aggiungere l'attributo [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex). Tuttavia, quando si usano elementi non semantici per l'immissione dei dati durante la [creazione di controlli modulo personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls), sarà necessario aggiungere JavaScript e [ARIA](/it/docs/Web/Accessibility/ARIA) per dotare l'elemento della funzionalità di controllo modulo per tutto il resto.

Per fornire una buona esperienza utente, qualsiasi controllo modulo personalizzato creato deve essere accessibile e funzionare come i controlli modulo nativi:

- Il [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles), l'[etichetta](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) e la [descrizione](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby) dell'elemento devono essere aggiunti con ARIA.
- Devono essere supportati tutti i metodi di input dell'utente, inclusi gli eventi di [tastiera](#tastiera), [mouse](#mouse), [tocco](#tocco_con_le_dita) e [puntatore](#eventi_puntatore), tutti descritti sopra.
- JavaScript è necessario per gestire funzionalità quali [convalida](/it/docs/Learn_web_development/Extensions/Forms/Form_validation), [invio](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) e [salvataggio](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript) dei contenuti aggiornati dall'utente.

{{EmbedLiveSample("contentEditable")}}

> [!NOTE]
> Esempi e altre risorse sono disponibili nella [guida a Content Editable](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable).

## Tutorial

- [Guida agli eventi touch](/it/docs/Web/API/Touch_events)
- [Gestire l'orientamento dello schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation)
- [Usare la modalità schermo intero](/it/docs/Web/API/Fullscreen_API)
- [Guida alle operazioni di trascinamento](/it/docs/Web/API/HTML_Drag_and_Drop_API/Drag_operations)
- [Convalida del modulo](/it/docs/Learn_web_development/Extensions/Forms/Form_validation)
- [Inviare moduli tramite JavaScript](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript)

## Riferimento

- Interfaccia [`MouseEvent`](/it/docs/Web/API/MouseEvent)
- Interfaccia [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent)
- API [Touch events](/it/docs/Web/API/Touch_events)
- API [Pointer Lock](/it/docs/Web/API/Pointer_Lock_API)
- API [Screen Orientation](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation)
- API [Fullscreen](/it/docs/Web/API/Fullscreen_API)
- API [Drag & Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API)
- Attributo HTML [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable)
