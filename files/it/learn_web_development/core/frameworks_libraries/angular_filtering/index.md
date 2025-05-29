---
title: Filtrare gli elementi della lista di cose da fare
slug: Learn_web_development/Core/Frameworks_libraries/Angular_filtering
l10n:
  sourceCommit: ffa6f5871f50856c60983a125cef7de267be7aeb
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_item_component","Learn_web_development/Core/Frameworks_libraries/Angular_building", "Learn_web_development/Core/Frameworks_libraries")}}

Ora passiamo ad aggiungere funzionalità per consentire agli utenti di filtrare gli elementi della loro lista di cose da fare, in modo che possano visualizzare gli elementi attivi, completati o tutti gli elementi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
        conoscenza del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminale/riga di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Aggiungere funzionalità di filtraggio alla nostra app.</td>
    </tr>
  </tbody>
</table>

## Il nostro codice di filtraggio

Il filtraggio degli elementi si basa sulla proprietà `filter`, che hai precedentemente aggiunto a `app.component.ts`:

```ts
export class AppComponent {
  // …
  filter: "all" | "active" | "done" = "all";
  // …
}
```

Il valore predefinito di filter è `all`, ma può anche essere `active` o `done`.

## Aggiungere controlli di filtro

In `app.component.html`, aggiungere il seguente HTML sotto al pulsante **Add** ma sopra la sezione che elenca gli elementi. Nel frammento seguente, le sezioni esistenti nel tuo HTML sono commentate, in modo da poter vedere esattamente dove posizionare i pulsanti.

```html
<!-- <button class="btn-primary" (click)="addItem(newItem.value)">Add</button>
 -->

<!-- Buttons that show all, still to do, or done items on click -->
<div class="btn-wrapper">
  <button
    class="btn btn-menu"
    [class.active]="filter === 'all'"
    (click)="filter = 'all'">
    All
  </button>

  <button
    class="btn btn-menu"
    [class.active]="filter === 'active'"
    (click)="filter = 'active'">
    To Do
  </button>

  <button
    class="btn btn-menu"
    [class.active]="filter === 'done'"
    (click)="filter = 'done'">
    Done
  </button>
</div>

<!-- <h2>\{{items.length}} item(s)</h2>
         <ul>... -->
```

Cliccando sui pulsanti, i valori di `filter` cambiano, determinando gli `items` che vengono mostrati, così come gli stili che Angular applica al pulsante attivo.

- Se l'utente clicca sul pulsante **All**, vengono mostrati tutti gli elementi.
- Se l'utente clicca sul pulsante **To do**, vengono mostrati solo gli elementi con un valore di `done` uguale a `false`.
- Se l'utente clicca sul pulsante **Done**, vengono mostrati solo gli elementi con un valore di `done` uguale a `true`.

Un binding di attributo della classe, usando parentesi quadre, `[]`, controlla il colore del testo dei pulsanti. Il binding della classe, `[class.active]`, applica la classe `active` quando il valore di `filter` corrisponde all'espressione. Ad esempio, quando l'utente clicca sul pulsante **Done**, che imposta il valore di `filter` su `done`, l'espressione di binding della classe `filter === 'done'` valuta come `true`. Quando il valore di `filter` è `done`, Angular applica la classe `active` al pulsante **Done** per rendere il colore del testo verde. Appena l'utente clicca su uno degli altri pulsanti, il valore `filter` non è più `done`, quindi il colore del testo verde non si applica più.

## Sommario

È stato veloce! Poiché avevi già il codice `filter` in `app.component.ts`, tutto ciò che dovevi fare era modificare il template per fornire controlli per filtrare gli elementi. Il nostro prossimo — e ultimo — articolo esamina come costruire la tua app Angular pronta per la produzione e fornisce ulteriori risorse per continuare il tuo percorso di apprendimento.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_item_component","Learn_web_development/Core/Frameworks_libraries/Angular_building", "Learn_web_development/Core/Frameworks_libraries")}}
