---
title: Soluzioni delle sfide
slug: Learn_web_development/Core/Challenges
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questa pagina fornisce soluzioni alle sfide presentate nel modulo [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics). Queste non sono le uniche soluzioni possibili. Le sezioni seguenti corrispondono ai titoli delle sezioni del tutorial.

## Cascata ed eredità

Le sfide nella pagina [Gestione dei conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts) sono:

### Stili ereditati

- Sfida
  - : Modificare il foglio di stile in modo che solo le lettere rosse siano sottolineate.
- Soluzione

  - : Spostare la dichiarazione per il sottolineato dalla regola per {{ HTMLElement("p") }} a quella per {{ HTMLElement("strong") }}. Il file risultante appare così:

    ```css
    p {
      color: blue;
    }
    strong {
      color: orange;
      text-decoration: underline;
    }
    ```

Le sezioni successive di questo tutorial descrivono le regole di stile e le dichiarazioni in modo più dettagliato.

## Selettori

Le sfide nella pagina [Selettori di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) sono:

### Secondo paragrafo blu

- Sfida
  - : Senza modificare il file HTML, aggiungere una singola regola al file CSS che mantenga tutte le lettere iniziali dello stesso colore, ma renda blu tutto il resto del testo nel secondo paragrafo.
- Soluzione

  - : Aggiungere una regola con un selettore ID `#second` e una dichiarazione `color: blue;`, come mostrato di seguito:

    ```css
    #second {
      color: blue;
    }
    ```

    Un selettore più specifico, `p#second`, funziona anche.

### Entrambi i paragrafi blu

- Sfida
  - : Ora modificare la regola appena aggiunta (senza cambiare nient'altro) per rendere blu anche il primo paragrafo.
- Soluzione

  - : Cambiare il selettore della nuova regola per usare un selettore di tag con `p`:

    ```css
    p {
      color: blue;
    }
    ```

Le regole per gli altri colori hanno tutte selettori più specifici, quindi sovrascrivono il blu del paragrafo.

## CSS leggibile

### Commentare una regola

- Sfida
  - : Commentare una parte del foglio di stile, senza cambiare nient'altro, per rendere rossa la primissima lettera del documento.
- Soluzione

  - : Un modo per farlo è mettere i delimitatori di commento attorno alla regola per `.carrot`:

    ```css
    /*
    .carrot {
      color: orange;
    }
    */
    ```

## Stili di testo

### Lettere iniziali grandi

- Sfida
  - : Senza modificare nient'altro, rendere tutte e sei le lettere iniziali il doppio della dimensione nel font serif predefinito del browser.
- Soluzione

  - : Aggiungere la seguente dichiarazione di stile alla regola `strong`:

    ```css
    font: 200% serif;
    ```

    Se si utilizzano dichiarazioni separate per `font-size` e `font-family`, l'impostazione `font-style` sul primo paragrafo _non_ viene sovrascritta.

## Colore

### Codici colore a tre cifre

- Sfida
  - : Nel file CSS, cambiare tutti i nomi dei colori in codici colore a 3 cifre senza modificare il risultato.
- Soluzione

  - : I seguenti valori sono approssimazioni ragionevoli dei colori nominati:

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

Le sfide sulla pagina sono:

### Aggiungere un'immagine

- Sfida
  - : Aggiungere una regola al foglio di stile per far apparire l'immagine all'inizio di ogni riga.
- Soluzione

  - : Aggiungere questa regola al foglio di stile:

    ```css
    p::before {
      content: url("yellow-pin.png");
    }
    ```

## Liste

Le sfide nella pagina [Stilizzare le liste](/it/docs/Learn_web_development/Core/Text_styling/Styling_lists) sono:

### Numeri romani minuscoli

- Sfida
  - : Aggiungere una regola al foglio di stile, per numerare gli oceani usando numeri romani da i a v.
- Soluzione

  - : Definire una regola per gli elementi di lista per utilizzare lo stile `lower-roman`:

    ```css
    li {
      list-style: lower-roman;
    }
    ```

### Lettere maiuscole

- Sfida
  - : Modificare il foglio di stile per identificare le intestazioni con lettere maiuscole tra parentesi.
- Soluzione

  - : Aggiungere una regola all'elemento body (genitore delle intestazioni) per resettare un nuovo contatore e una per visualizzare e incrementare il contatore sulle intestazioni:

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

### Bordo dell'oceano

- Sfida
  - : Aggiungere una regola al foglio di stile, creando un ampio bordo attorno agli oceani in un colore che ricorda il mare.
- Soluzione

  - : La seguente regola raggiunge questo effetto:

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
  - : Modificare il documento di esempio, `doc2.html`, aggiungendo questo tag verso la fine, appena prima di `</BODY>`: `<IMG id="fixed-pin" src="Yellow-pin.png" alt="Yellow map pin">` Predire dove apparirà l'immagine nel documento. Poi aggiornare il browser per vedere se la previsione era corretta.
- Soluzione
  - : L'immagine appare a destra del secondo elenco.
    ![Un elenco di cinque testi placeholder è intitolato Paragrafi Numerati. Una spilla gialla è posizionata a destra di un riquadro blu contenente l'elenco.](pin_placement.png)
- Sfida
  - : Aggiungere una regola al foglio di stile che posiziona l'immagine in alto a destra del documento.
- Soluzione

  - : La seguente regola raggiunge il risultato desiderato:

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
  - : Modificare il foglio di stile per far sì che la tabella abbia un bordo verde solo attorno alle celle di dati.
- Soluzione

  - : La seguente regola mette i bordi solo intorno agli elementi {{ HTMLElement("td") }} che si trovano all'interno dell'elemento {{ HTMLElement("tbody") }} della tabella con `id=demo-table`:

    ```css
    #demo-table tbody td {
      border: 1px solid #7a7;
    }
    ```

## Media

Le sfide nella pagina [Media](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) sono:

### File stile di stampa separato

- Sfida
  - : Spostare le regole di stile specifiche per la stampa in un file CSS separato e importarle nel foglio di stile `style4.css`.
- Soluzione

  - : Tagliare e incollare le righe tra `/* print only */` e `/* end print only */` in un file denominato `style4_print.css`. In style4.css, aggiungere la seguente riga all'inizio del file:

    ```css
    @import url("style4_print.css") print;
    ```

### Colore al passaggio del mouse sulle intestazioni

- Sfida
  - : Far sì che le intestazioni diventino blu quando il puntatore del mouse è sopra di esse.
- Soluzione

  - : La seguente regola raggiunge il risultato desiderato:

    ```css
    h1:hover {
      color: blue;
    }
    ```

## JavaScript

### Spostare la casella a destra

- Sfida
  - : Modificare lo script in modo che il quadrato salta a destra di 20 em quando il suo colore cambia, e torna indietro successivamente.
- Soluzione

  - : Aggiungere righe per modificare la proprietà `margin-left`. Assicurarsi di specificare `marginLeft` in JavaScript. Il seguente script raggiunge il risultato desiderato:

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

### Cambiare il colore dei petali interni

- Sfida
  - : Modificare il foglio di stile in modo che i petali interni diventino tutti rosa quando il puntatore del mouse è sopra uno di essi, senza cambiare il modo in cui funzionano i petali esterni.
- Soluzione

  - : Spostare la posizione della pseudo-classe :hover da un petalo specifico a tutti i petali

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
