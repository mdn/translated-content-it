---
title: Soluzioni alle sfide
slug: Learn_web_development/Core/Challenges
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questa pagina fornisce soluzioni alle sfide poste nel modulo [Nozioni di base sullo styling CSS](/it/docs/Learn_web_development/Core/Styling_basics). Queste non sono le uniche soluzioni possibili. Le sezioni seguenti corrispondono ai titoli delle sezioni del tutorial.

## Cascata e ereditarietà

Le sfide nella pagina [Gestione dei conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts) sono:

### Stili ereditati

- Sfida
  - : Cambi il suo foglio di stile in modo che solo le lettere rosse siano sottolineate.
- Soluzione

  - : Sposti la dichiarazione per la sottolineatura dalla regola per {{ HTMLElement("p") }} a quella per {{ HTMLElement("strong") }}. Il file risultante appare così:

    ```css
    p {
      color: blue;
    }
    strong {
      color: orange;
      text-decoration: underline;
    }
    ```

Le sezioni successive di questo tutorial descrivono le regole di stile e le dichiarazioni in maggiore dettaglio.

## Selettori

Le sfide nella pagina [Selettori di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) sono:

### Secondo paragrafo blu

- Sfida
  - : Senza modificare il suo file HTML, aggiunga una sola regola al suo file CSS che mantiene tutte le lettere iniziali dello stesso colore attuale, ma fa sì che tutto il resto del testo nel secondo paragrafo diventi blu.
- Soluzione

  - : Aggiunga una regola con un selettore ID `#second` e una dichiarazione `color: blue;`, come mostrato di seguito:

    ```css
    #second {
      color: blue;
    }
    ```

    Anche un selettore più specifico, `p#second` funziona.

### Entrambi i paragrafi blu

- Sfida
  - : Ora cambi la regola che ha appena aggiunto (senza cambiare nient'altro), per fare in modo che anche il primo paragrafo diventi blu.
- Soluzione

  - : Cambi il selettore della nuova regola per essere un selettore di tag usando `p`:

    ```css
    p {
      color: blue;
    }
    ```

Le regole per gli altri colori hanno selettori più specifici, quindi sovrascrivono il blu del paragrafo.

## CSS leggibile

### Commentare una regola

- Sfida
  - : Commenti una parte del suo foglio di stile, senza cambiare nient'altro, per far diventare rossa la primissima lettera del suo documento.
- Soluzione

  - : Un modo per farlo è mettere delimitatori di commento attorno alla regola per `.carrot`:

    ```css
    /*
    .carrot {
      color: orange;
    }
    */
    ```

## Stili del testo

### Lettere iniziali grandi

- Sfida
  - : Senza cambiare nient'altro, faccia in modo che tutte e sei le lettere iniziali siano due volte più grandi nel font serif predefinito del browser.
- Soluzione

  - : Aggiunga la seguente dichiarazione di stile alla regola `strong`:

    ```css
    font: 200% serif;
    ```

    Se utilizza dichiarazioni separate per `font-size` e `font-family`, allora l'impostazione `font-style` sul primo paragrafo non viene sovrascritta.

## Colore

### Codici colore a tre cifre

- Sfida
  - : Nel suo file CSS, cambi tutti i nomi dei colori in codici colore a 3 cifre senza alterare il risultato.
- Soluzione

  - : I seguenti valori sono approssimazioni ragionevoli dei colori denominati:

    ```css
    strong {
      color: #f00; /* red */
      background-color: #ddf; /* pale blue */
      font: 200% serif;
    }

    .carrot {
      color: #fa0; /* orange */
    }

    .spinach {
      color: #080; /* dark green */
    }

    p {
      color: #00f; /* blue */
    }
    ```

## Contenuto

Le sfide nella pagina sono:

### Aggiungi un'immagine

- Sfida
  - : Aggiunga una regola al suo foglio di stile in modo che mostri l'immagine all'inizio di ogni riga.
- Soluzione

  - : Aggiunga questa regola al suo foglio di stile:

    ```css
    p::before {
      content: url("yellow-pin.png");
    }
    ```

## Liste

Le sfide nella pagina [Stile delle liste](/it/docs/Learn_web_development/Core/Text_styling/Styling_lists) sono:

### Numeri romani minuscoli

- Sfida
  - : Aggiunga una regola al suo foglio di stile, per numerare gli oceani usando i numeri romani da i a v.
- Soluzione

  - : Definisca una regola per gli elementi della lista utilizzando lo stile lista `lower-roman`:

    ```css
    li {
      list-style: lower-roman;
    }
    ```

### Lettere maiuscole

- Sfida
  - : Modifichi il suo foglio di stile per identificare i titoli con lettere maiuscole tra parentesi.
- Soluzione

  - : Aggiunga una regola per l'elemento body (genitore dei titoli) per resettare un nuovo contatore, e una per visualizzare e incrementare il contatore sui titoli:

    ```css
    /* numbered headings */
    body {
      counter-reset: head-num;
    }
    h3::before {
      content: "(" counter(head-num, upper-latin) ") ";
      counter-increment: head-num;
    }
    ```

## Box

Le sfide nella pagina [Box](/it/docs/Learn_web_development/Core/CSS_layout) sono:

### Bordo oceano

- Sfida
  - : Aggiunga una regola al suo foglio di stile, mettendo un bordo largo intorno agli oceani con un colore che le ricorda il mare.
- Soluzione

  - : La seguente regola ottiene questo effetto:

    ```css
    ul {
      border: 10px solid lightblue;
      width: 100px;
    }
    ```

## Layout

Le sfide nella pagina [Layout](/it/docs/Learn_web_development/Core/CSS_layout) sono:

### Posizione predefinita dell'immagine

### Posizione fissa dell'immagine

- Sfida
  - : Modifichi il suo documento di esempio, `doc2.html`, aggiungendo questo tag verso la fine, appena prima di `</BODY>`: `<IMG id="fixed-pin" src="Yellow-pin.png" alt="Yellow map pin">`. Preveda dove apparirà l'immagine nel suo documento. Poi aggiorni il suo browser per verificare se aveva ragione.
- Soluzione
  - : L'immagine appare alla destra della seconda lista.
    ![Una lista di cinque testi segnaposto è intitolata Paragrafi numerati. Un pin giallo è posizionato a destra di un riquadro blu contenente la lista.](pin_placement.png)
- Sfida
  - : Aggiunga una regola al suo foglio di stile che posizioni l'immagine in alto a destra del suo documento.
- Soluzione

  - : La seguente regola ottiene il risultato desiderato:

    ```css
    #fixed-pin {
      position: fixed;
      top: 3px;
      right: 3px;
    }
    ```

## Tabelle

Le sfide nella pagina [Tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables) sono:

### Bordi solo sulle celle di dati

- Sfida
  - : Modifichi il foglio di stile per fare in modo che la tabella abbia un bordo verde solo attorno alle celle di dati.
- Soluzione

  - : La seguente regola mette bordi solo attorno agli elementi {{ HTMLElement("td") }} che sono all'interno dell'elemento {{ HTMLElement("tbody") }} della tabella con `id=demo-table`:

    ```css
    #demo-table tbody td {
      border: 1px solid #7a7;
    }
    ```

## Media

Le sfide nella pagina [Media](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) sono:

### File di stile separato per la stampa

- Sfida
  - : Sposti le regole di stile specifiche per la stampa in un file CSS separato e le importi nel suo foglio di stile `style4.css`.
- Soluzione

  - : Tagli e incolli le righe tra `/* print only */` e `/* end print only */` in un file chiamato `style4_print.css`. In style4.css, aggiunga la seguente riga all'inizio del file:

    ```css
    @import url("style4_print.css") print;
    ```

### Colore Hover dei titoli

- Sfida
  - : Faccia in modo che i titoli diventino blu quando il puntatore del mouse è su di essi.
- Soluzione

  - : La seguente regola ottiene il risultato desiderato:

    ```css
    h1:hover {
      color: blue;
    }
    ```

## JavaScript

### Sposti il box a destra

- Sfida
  - : Modifichi lo script in modo che il quadrato salti a destra di 20 em quando cambia colore, e salti indietro successivamente.
- Soluzione

  - : Aggiunga delle righe per modificare la proprietà `margin-left`. Si assicuri di specificarla come `marginLeft` in JavaScript. Il seguente script ottiene il risultato desiderato:

    ```js
    // JavaScript demonstration
    function doDemo(button) {
      const square = document.getElementById("square");
      square.style.backgroundColor = "#fa4";
      square.style.marginLeft = "20em";
      button.setAttribute("disabled", "true");
      setTimeout(clearDemo, 2000, button);
    }

    function clearDemo(button) {
      const square = document.getElementById("square");
      square.style.backgroundColor = "transparent";
      square.style.marginLeft = "0em";
      button.removeAttribute("disabled");
    }
    ```

## SVG e CSS

### Cambia colore dei petali interni

- Sfida
  - : Modifichi il foglio di stile in modo che i petali interni diventino tutti rosa quando il puntatore del mouse è su uno di essi, senza modificare il funzionamento dei petali esterni.
- Soluzione

  - : Sposti la posizione della pseudo-classe :hover da un petalo specifico a tutti i petali

    ```css
    #inner-petals {
      --segment-fill-fill-hover: pink;
    }

    /* Non-standard way for some older browsers */
    #inner-petals:hover .segment-fill {
      fill: pink;
      stroke: none;
    }
    ```
