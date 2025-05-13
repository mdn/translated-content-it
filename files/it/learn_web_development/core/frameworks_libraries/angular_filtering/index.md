---
title: Filtrare i nostri elementi di attività
slug: Learn_web_development/Core/Frameworks_libraries/Angular_filtering
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_item_component","Learn_web_development/Core/Frameworks_libraries/Angular_building", "Learn_web_development/Core/Frameworks_libraries")}}

Ora passiamo ad aggiungere la funzionalità per consentire agli utenti di filtrare i loro elementi di attività, così possono visualizzare gli elementi attivi, completati o tutti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
        conoscenza del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminale/linea di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Aggiungere la funzionalità di filtro alla nostra applicazione.</td>
    </tr>
  </tbody>
</table>

## Il nostro codice di filtraggio

Il filtraggio degli elementi si basa sulla proprietà `filter`, che avete precedentemente aggiunto a `app.component.ts`:

```ts
export class AppComponent {
  // …
  filter: "all" | "active" | "done" = "all";
  // …
}
```

Il valore predefinito per il filtro è `all`, ma può essere anche `active` o `done`.

## Aggiungere i controlli di filtro

In `app.component.html`, aggiunga il seguente HTML sotto il pulsante **Aggiungi** ma sopra la sezione che elenca gli elementi. Nel seguente frammento, le sezioni esistenti nel suo HTML sono nei commenti in modo che possa vedere esattamente dove posizionare i pulsanti.

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

Cliccando sui pulsanti cambia i valori di `filter`, che determinano gli `items` visualizzati così come gli stili che Angular applica al pulsante attivo.

- Se l'utente clicca il pulsante **Tutti**, tutti gli elementi vengono visualizzati.
- Se l'utente clicca il pulsante **Da fare**, vengono visualizzati solo gli elementi con un valore `done` di `false`.
- Se l'utente clicca il pulsante **Completati**, vengono visualizzati solo gli elementi con un valore `done` di `true`.

Un binding dell'attributo class, utilizzando parentesi quadre, `[]`, controlla il colore del testo dei pulsanti.
Il binding della classe, `[class.active]`, applica la classe `active` quando il valore di `filter` corrisponde all'espressione.
Ad esempio, quando l'utente clicca il pulsante **Completati**, che imposta il valore di `filter` su `done`, l'espressione del binding della classe `filter == 'done'` viene valutata come `true`.
Quando il valore di `filter` è `done`, Angular applica la classe `active` al pulsante **Completati** per rendere il colore del testo verde.
Non appena l'utente clicca su uno degli altri pulsanti, il valore di `filter` non è più `done`, quindi il colore verde del testo non viene più applicato.

## Sommario

È stato veloce! Poiché aveva già il codice `filter` in `app.component.ts`, tutto quello che ha dovuto fare è stato modificare il modello per fornire i controlli per filtrare gli elementi. Il nostro prossimo — e ultimo — articolo esamina come costruire la sua app Angular pronta per la produzione, e fornisce ulteriori risorse per proseguire il suo percorso di apprendimento.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_item_component","Learn_web_development/Core/Frameworks_libraries/Angular_building", "Learn_web_development/Core/Frameworks_libraries")}}
