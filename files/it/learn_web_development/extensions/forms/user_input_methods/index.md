---
title: Metodi di input e controlli utente
slug: Learn_web_development/Extensions/Forms/User_input_methods
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I moduli Web richiedono input dall'utente. Quando si progettano moduli Web, o in generale qualsiasi contenuto web, è importante considerare come gli utenti interagiscono con i propri dispositivi e browser. L'input utente sul Web va oltre il semplice mouse e tastiera: pensi agli schermi touchscreen, per esempio.

In questo articolo, esaminiamo i diversi modi in cui gli utenti interagiscono con i moduli e altri contenuti web e forniamo raccomandazioni per la gestione degli input utente, esempi del mondo reale e collegamenti a ulteriori informazioni.

Mentre sviluppa moduli più complessi e interattivi o altre funzionalità dell'interfaccia utente, ci sono molti elementi HTML e API JavaScript che potrebbe voler investigare. Ad esempio, potrebbe desiderare di creare controlli personalizzati per i moduli che richiedono elementi non semantici per essere modificabili nel contenuto. Potrebbe voler supportare eventi touch, determinare o controllare l'orientamento dello schermo, fare in modo che un modulo occupi l'intero schermo, o abilitare funzionalità di drag and drop. Questa guida introduce tutte queste caratteristiche e la indirizza verso ulteriori informazioni su ciascun argomento.

Per fornire una buona esperienza al maggior numero di utenti, è necessario supportare più metodi di input, inclusi mouse, tastiera, tocco delle dita, e così via. I meccanismi di input disponibili dipendono dalle capacità del dispositivo che esegue l'applicazione.

Dovrebbe sempre tenere a mente l'accessibilità tramite tastiera — molti utenti web usano solo la tastiera per navigare su siti e applicazioni, e escluderli dalla sua funzionalità è una cattiva idea.

## Argomenti trattati

- Per supportare display touchscreen, gli [eventi touch](/it/docs/Web/API/Touch_events) interpretano l'attività delle dita sulle interfacce utente touch-based, da dispositivi mobili a pannelli di frigoriferi fino ai display dei chioschi nei musei.
- L'[API Fullscreen](/it/docs/Web/API/Fullscreen_API) le consente di visualizzare i suoi contenuti in modalità fullscreen, cosa necessaria se il suo modulo viene servito su un frigorifero o un chiosco museale.
- Quando ha bisogno di creare un controllo personalizzato per un modulo, come un editor di testo arricchito, l'attributo [`contentEditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable) consente di creare controlli modificabili a partire da elementi HTML normalmente non modificabili.
- L'[API Drag and Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API) permette agli utenti di trascinare elementi su una pagina e rilasciarli in posizioni diverse. Questo può aiutare a migliorare l'esperienza utente quando si tratta di selezionare file da caricare o riordinare moduli di contenuto all'interno di una pagina.
- Quando l'orientamento dello schermo è importante per il suo layout, può utilizzare [media queries CSS](/it/docs/Web/CSS/@media/orientation) per stilare i suoi moduli in base all'orientamento del browser, o anche utilizzare l'[API Screen Orientation](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation) per leggere lo stato di orientamento dello schermo ed eseguire altre azioni.

Le sezioni seguenti forniscono un insieme di raccomandazioni e best practices per consentire al più ampio insieme possibile di utenti di utilizzare i suoi siti web e applicazioni.

## Supportare meccanismi di input comuni

### Tastiera

La maggior parte degli utenti utilizzerà una tastiera per inserire dati nei suoi controlli di modulo. Alcuni utilizzeranno anche la tastiera per navigare verso tali controlli. Per essere accessibile e per una migliore esperienza utente, è importante [etichettare correttamente tutti i controlli di modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form#the_label_input_and_textarea_elements). Quando ciascun controllo di modulo ha un {{htmlelement("label")}} associato correttamente, il suo modulo sarà completamente accessibile a tutti, più notoriamente a chi naviga il suo modulo con una tastiera, un lettore di schermo, e possibilmente senza alcun schermo.

Se desidera aggiungere supporto supplementare per la tastiera, come validare un controllo di modulo quando viene premuto un tasto specifico, può usare i listener di eventi per catturare e reagire agli eventi della tastiera. Ad esempio, se desidera aggiungere controlli quando qualsiasi tasto viene premuto, deve aggiungere un event listener all'oggetto `window`:

```js
window.addEventListener("keydown", handleKeyDown, true);
window.addEventListener("keyup", handleKeyUp, true);
```

`handleKeyDown` e `handleKeyUp` sono funzioni che definiscono la logica del controllo da eseguire quando gli eventi `keydown` e `keyup` vengono attivati.

> [!NOTE]
> Guardi il [riferimento agli eventi](/it/docs/Web/Events) e la guida [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent) per saperne di più sugli eventi da tastiera.

### Mouse

Può anche catturare eventi del mouse e di altre periferiche di puntamento. Gli eventi che si verificano quando l'utente interagisce con un dispositivo di puntamento come un mouse sono rappresentati dall'interfaccia DOM [`MouseEvent`](/it/docs/Web/API/MouseEvent). Gli eventi mouse comuni includono [`click`](/it/docs/Web/API/Element/click_event), [`dblclick`](/it/docs/Web/API/Element/dblclick_event), [`mouseup`](/it/docs/Web/API/Element/mouseup_event), e [`mousedown`](/it/docs/Web/API/Element/mousedown_event). L'elenco di tutti gli eventi usando l'interfaccia Mouse Event è fornito nel [riferimento agli eventi](/it/docs/Web/Events).

Quando il dispositivo di input è un mouse, può anche controllare l'input utente tramite l'API Pointer Lock e implementare il Drag & Drop (vedi sotto). Può anche [usare CSS per testare il supporto per dispositivi di puntamento](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries#use_of_pointing_devices).

### Tocco delle dita

Per fornire ulteriore supporto ai dispositivi touchscreen, è una buona pratica prendere in considerazione le diverse capacità in termini di risoluzione dello schermo e input utente. Gli [eventi touch](/it/docs/Web/API/Touch_events) possono aiutarla a implementare elementi interattivi e gesti di interazione comuni sui dispositivi touchscreen.

Se desidera utilizzare eventi touch, deve aggiungere listener di eventi e specificare funzioni handler, che verranno chiamate quando l'evento viene attivato:

```js
element.addEventListener("touchstart", handleStart, false);
element.addEventListener("touchcancel", handleCancel, false);
element.addEventListener("touchend", handleEnd, false);
element.addEventListener("touchmove", handleMove, false);
```

dove `element` è l'elemento DOM sul quale si desidera registrare gli eventi touch.

> [!NOTE]
> Per ulteriori informazioni su cosa può fare con gli eventi touch, legga la nostra [guida agli eventi touch](/it/docs/Web/API/Touch_events).

### Eventi Pointer

I mouse non sono gli unici dispositivi di puntamento. I dispositivi degli utenti possono incorporare più forme di input, come mouse, tocco delle dita e input con penna. Ognuno di questi puntatori ha una dimensione diversa. L'[API Pointer Events](/it/docs/Web/API/Pointer_events) può essere utile se ha bisogno di gestire gli eventi su diversi dispositivi normalizzando la gestione di ciascuno. Un puntatore può essere qualsiasi punto di contatto sullo schermo fatto da un cursore del mouse, una penna, un tocco (incluso il multi-touch) o un altro dispositivo di input di puntamento.

Gli eventi per la gestione dell'input generico tramite puntatore sembrano molto simili a quelli per il mouse: `pointerdown`, `pointermove`, `pointerup`, `pointerover`, `pointerout`, ecc. L'interfaccia [`PointerEvent`](/it/docs/Web/API/PointerEvent) fornisce tutti i dettagli che potrebbe voler catturare sul dispositivo di puntamento, incluse dimensione, pressione e angolo.

## Implementare controlli

### Orientamento dello Schermo

Se ha bisogno di layout leggermente diversi a seconda che l'utente sia in modalità ritratto o paesaggio, può usare [media queries CSS](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries#media_feature_rules) per definire CSS per layout diversi o larghezze di controllo del modulo basate sulla dimensione o orientamento dello schermo quando [si stilano moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

Quando l'orientamento dello schermo è importante per il suo modulo, può leggere lo stato dell'orientamento dello schermo, essere informato quando questo stato cambia e poter bloccare l'orientamento dello schermo in uno stato specifico (di solito ritratto o paesaggio) tramite l'[API Screen Orientation](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation).

- I dati di orientamento possono essere recuperati tramite [`screenOrientation.type`](/it/docs/Web/API/ScreenOrientation/type) o con CSS tramite la caratteristica media [`orientation`](/it/docs/Web/CSS/@media/orientation).
- Quando l'orientamento dello schermo cambia, l'evento [`change`](/it/docs/Web/API/ScreenOrientation/change_event) viene attivato sull'oggetto schermato.
- È possibile bloccare l'orientamento dello schermo invocando il metodo [`ScreenOrientation.lock()`](/it/docs/Web/API/ScreenOrientation/lock).
- Il metodo [`ScreenOrientation.unlock()`](/it/docs/Web/API/ScreenOrientation/unlock) rimuove tutti i blocchi dello schermo precedenti che sono stati impostati.

> [!NOTE]
> Maggiori informazioni sull'API Screen Orientation possono essere trovate in [Gestire l'orientamento dello schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation).

### Fullscreen

Se ha bisogno di presentare il suo modulo in modalità fullscreen, come quando il suo modulo è visualizzato su un chiosco museale, un casello, o davvero qualsiasi interfaccia utente esposta al pubblico, è possibile farlo chiamando [`Element.requestFullscreen()`](/it/docs/Web/API/Element/requestFullscreen) su quell'elemento:

```js
const elem = document.getElementById("myForm");
if (elem.requestFullscreen) {
  elem.requestFullscreen();
}
```

> [!NOTE]
> Per scoprire di più sull'aggiunta di funzionalità fullscreen alla sua applicazione, legga la nostra documentazione riguardo [l'utilizzo della modalità fullscreen](/it/docs/Web/API/Fullscreen_API).

### Drag & Drop

Un'interazione utente comune è il trascinamento fisico di elementi da rilasciare altrove sullo schermo. Il Drag and drop può aiutare a migliorare l'esperienza utente quando si tratta di selezionare file da caricare o riordinare moduli di contenuto all'interno di una pagina. C'è un'API per questo!

L'[API Drag & Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API) consente agli utenti di fare clic e tenere premuto il pulsante del mouse su un elemento, trascinarlo in un'altra posizione e rilasciare il pulsante del mouse per lasciar cadere l'elemento lì.

Ecco un esempio che consente di trascinare una sezione di contenuto.

```html
<div
  draggable="true"
  ondragstart="event.dataTransfer.setData('text/plain', 'This text may be dragged')">
  This text <strong>may</strong> be dragged.
</div>
```

in cui noi:

- Impostiamo l'attributo [`draggable`](/it/docs/Web/HTML/Reference/Global_attributes/draggable) su `true` sull'elemento che desidera rendere trascinabile.
- Aggiungiamo un listener per l'evento [`dragstart`](/it/docs/Web/API/HTMLElement/dragstart_event) e impostiamo i dati di trascinamento all'interno di questo listener.

> [!NOTE]
> Può trovare ulteriori informazioni nella [documentazione Drag & Drop di MDN](/it/docs/Web/API/HTML_Drag_and_Drop_API).

### contentEditable

In generale, dovrebbe usare un {{HTMLElement("textarea")}} o un tipo appropriato di {{HTMLElement("input")}} all'interno di un {{HTMLElement("form")}} per raccogliere dati dagli utenti, insieme a un {{HTMLElement("label")}} descrittivo. Ma questi elementi potrebbero non soddisfare le sue esigenze. Ad esempio, gli editor di testo arricchito catturano testo in corsivo, grassetto e normale, ma nessun controllo del modulo nativo cattura testo arricchito. Questo caso d'uso richiede la creazione di un controllo personalizzato che sia sia stilabile sia modificabile. C'è un attributo per questo!

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

L'attributo `contenteditable` aggiunge automaticamente l'elemento all'ordine predefinito di tabulazione del documento, significando che l'attributo [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) non è necessario. Tuttavia, quando si usano elementi non semantici per l'inserimento dati quando [si creano controlli personalizzati per i moduli](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls), sarà necessario aggiungere JavaScript e [ARIA](/it/docs/Web/Accessibility/ARIA) per adattare l'elemento alle funzionalità di controllo del modulo per tutto il resto.

Per fornire una buona esperienza utente, qualsiasi controllo personalizzato per i moduli creato deve essere accessibile e funzionare come i controlli del modulo nativi:

- Devono essere aggiunti all'elemento [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles), [label](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby), e [descrizione](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby) con ARIA.
- Tutti i metodi di input utente devono essere supportati, inclusi eventi da [tastiera](#tastiera), [mouse](#mouse), [tocco](#tocco_delle_dita), e [puntatore](#eventi_pointer), tutti descritti sopra.
- È necessario JavaScript per gestire funzionalità come [validazione](/it/docs/Learn_web_development/Extensions/Forms/Form_validation), [invio](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data), e [salvataggio](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript) del contenuto aggiornato dall'utente.

{{EmbedLiveSample("contentEditable")}}

> [!NOTE]
> Esempi e altre risorse possono essere trovate nella [guida al contenuto modificabile](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable).

## Tutorial

- [Guida agli eventi touch](/it/docs/Web/API/Touch_events)
- [Gestire l'orientamento dello schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation)
- [Utilizzo della modalità fullscreen](/it/docs/Web/API/Fullscreen_API)
- [Guida alle operazioni di trascinamento](/it/docs/Web/API/HTML_Drag_and_Drop_API/Drag_operations)
- [Validazione dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Form_validation)
- [Invio di moduli tramite JavaScript](/it/docs/Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript)

## Riferimenti

- Interfaccia [`MouseEvent`](/it/docs/Web/API/MouseEvent) 
- Interfaccia [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent) 
- API [Eventi touch](/it/docs/Web/API/Touch_events) 
- API [Blocco puntatore](/it/docs/Web/API/Pointer_Lock_API) 
- API [Orientamento dello schermo](/it/docs/Web/API/CSS_Object_Model/Managing_screen_orientation) 
- API [Fullscreen](/it/docs/Web/API/Fullscreen_API) 
- API [Drag & Drop](/it/docs/Web/API/HTML_Drag_and_Drop_API) 
- Attributo HTML [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable)
