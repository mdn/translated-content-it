---
title: Filtrare gli elementi della lista
slug: Learn_web_development/Core/Frameworks_libraries/Angular_filtering
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_item_component","Learn_web_development/Core/Frameworks_libraries/Angular_building", "Learn_web_development/Core/Frameworks_libraries")}}

Ora passiamo ad aggiungere funzionalità per permettere agli utenti di filtrare i loro elementi di lavoro, così da poter visualizzare elementi attivi, completati o tutti gli elementi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
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
      <td>Aggiungere la funzionalità di filtro alla nostra app.</td>
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

Il valore predefinito per il filtro è `all`, ma può essere anche `active` o `done`.

## Aggiungere i controlli di filtro

In `app.component.html`, aggiungi il seguente HTML sotto il pulsante **Aggiungi** ma sopra la sezione che elenca gli elementi. Nel seguente frammento, le sezioni esistenti nel tuo HTML sono in commento così puoi vedere esattamente dove inserire i pulsanti.

```html
<!-- <button class="btn-primary" (click)="addItem(newItem.value)">Add</button>
 -->

<!-- Buttons that show all, still to do, or done items on click -->
<div class="btn-wrapper">
  <button
    class="btn btn-menu"
    [class.active]="filter == 'all'"
    (click)="filter = 'all'">
    All
  </button>

  <button
    class="btn btn-menu"
    [class.active]="filter == 'active'"
    (click)="filter = 'active'">
    To Do
  </button>

  <button
    class="btn btn-menu"
    [class.active]="filter == 'done'"
    (click)="filter = 'done'">
    Done
  </button>
</div>

<!-- <h2>\{{items.length}} item(s)</h2>
         <ul>... -->
```

Cliccando i pulsanti si cambiano i valori di `filter`, che determinano gli `items` da mostrare così come gli stili che Angular applica al pulsante attivo.

- Se l'utente clicca sul pulsante **Tutti**, vengono mostrati tutti gli elementi.
- Se l'utente clicca sul pulsante **Da fare**, vengono mostrati solo gli elementi con un valore `done` di `false`.
- Se l'utente clicca sul pulsante **Fatto**, vengono mostrati solo gli elementi con un valore `done` di `true`.

Un binding dell'attributo class, usando parentesi quadre, `[]`, controlla il colore del testo dei pulsanti. Il binding della classe, `[class.active]`, applica la classe `active` quando il valore di `filter` corrisponde all'espressione. Ad esempio, quando l'utente clicca sul pulsante **Fatto**, che imposta il valore del `filter` su `done`, l'espressione di binding della classe `filter == 'done'` si valuta come `true`. Quando il valore del `filter` è `done`, Angular applica la classe `active` al pulsante **Fatto** per rendere il colore del testo verde. Non appena l'utente clicca su uno degli altri pulsanti, il valore di `filter` non è più `done`, quindi il colore verde del testo non si applica più.

## Sommario

È stato veloce! Poiché avevi già il codice del `filter` in `app.component.ts`, tutto ciò che dovevi fare era modificare il template per fornire controlli per filtrare gli elementi. Il nostro prossimo — e ultimo — articolo esplorerà come costruire la tua app Angular pronta per la produzione e fornirà ulteriori risorse per continuare il tuo percorso di apprendimento.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_item_component","Learn_web_development/Core/Frameworks_libraries/Angular_building", "Learn_web_development/Core/Frameworks_libraries")}}
