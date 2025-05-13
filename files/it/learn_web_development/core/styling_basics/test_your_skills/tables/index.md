---
title: "Metti alla prova le sue abilità: Tabelle"
short-title: Tables
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Tables
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test è valutare se lei comprende come [stilizzare le tabelle HTML con CSS](/it/docs/Learn_web_development/Core/Styling_basics/Tables).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nell'MDN Playground.
> Può anche copiare il codice (cliccando sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se trova delle difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito

Nella lezione su [come stilizzare le tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables), abbiamo stilizzato una tabella in modo piuttosto appariscente. In questo compito, andremo a stilizzare la stessa tabella, ma utilizzando alcune buone pratiche di design delle tabelle come descritto nell'articolo esterno [Web Typography: designing tables to be read not looked at](https://alistapart.com/article/web-typography-tables/).

La nostra tabella finale apparirà come nell'immagine qui sotto. Ci sono diversi modi per ottenere questo risultato, ma suggeriamo di seguire modelli simili a quelli utilizzati nel tutorial per fare le seguenti cose:

- Aggiungere un padding di `0.3em` alle intestazioni e ai dati della tabella e allinearli in alto nelle loro celle.
- Allineare le intestazioni e i dati delle colonne contenenti numeri a destra.
- Allineare le intestazioni e i dati delle colonne contenenti testo a sinistra.
- Allineare l'intestazione del footer della tabella a destra.
- Allineare i dati del footer della tabella a sinistra.
- Aggiungere un bordo solido di 1px sopra e sotto con il colore esadecimale `#999` alla tabella.
- Aggiungere un bordo solido di 1px sopra con il colore esadecimale `#999` al footer.
- Rimuovere la spaziatura predefinita tra i bordi degli elementi della tabella per ottenere il risultato previsto.
- Strisciare ogni riga dispari della tabella principale con il colore esadecimale `#eee`.

![Una tabella con righe a strisce.](mdn-table-bands.png)

**Domanda bonus:** Cosa può fare per rendere il layout della tabella un po' più prevedibile? Pensi a come le colonne della tabella sono dimensionate di default e a come possiamo cambiare questo comportamento per dimensionare le colonne in base alla larghezza delle loro intestazioni.

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___table
<table>
  <caption>
    A summary of the UK's most famous punk bands
  </caption>
  <thead>
    <tr>
      <th scope="col">Band</th>
      <th scope="col">Year formed</th>
      <th scope="col">No. of Albums</th>
      <th scope="col">Most famous song</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Buzzcocks</th>
      <td>1976</td>
      <td>9</td>
      <td>Ever fallen in love (with someone you shouldn't've)</td>
    </tr>
    <tr>
      <th scope="row">The Clash</th>
      <td>1976</td>
      <td>6</td>
      <td>London Calling</td>
    </tr>
    <tr>
      <th scope="row">The Damned</th>
      <td>1976</td>
      <td>10</td>
      <td>Smash it up</td>
    </tr>
    <tr>
      <th scope="row">Sex Pistols</th>
      <td>1975</td>
      <td>1</td>
      <td>Anarchy in the UK</td>
    </tr>
    <tr>
      <th scope="row">Sham 69</th>
      <td>1976</td>
      <td>13</td>
      <td>If the kids are united</td>
    </tr>
    <tr>
      <th scope="row">Siouxsie and the Banshees</th>
      <td>1976</td>
      <td>11</td>
      <td>Hong Kong Garden</td>
    </tr>
    <tr>
      <th scope="row">Stiff Little Fingers</th>
      <td>1977</td>
      <td>10</td>
      <td>Suspect Device</td>
    </tr>
    <tr>
      <th scope="row">The Stranglers</th>
      <td>1974</td>
      <td>17</td>
      <td>No More Heroes</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th colspan="2" scope="row">Total albums</th>
      <td colspan="2">77</td>
    </tr>
  </tfoot>
</table>
```

```css hidden live-sample___table
body {
  padding: 1em;
  font: 1.2em / 1.5 sans-serif;
  font-size: 80%;
}
```

```css live-sample___table
/* Add styles here */
```

{{EmbedLiveSample("table", "", "400px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Qui sotto c'è un esempio di come il risultato finale potrebbe essere ottenuto, utilizzando tecniche simili a quelle della lezione. Tuttavia, ci sono diversi modi che sarebbero perfettamente corretti, forse leggermente più verbosi.

```css
table {
  border-top: 1px solid #999;
  border-bottom: 1px solid #999;
  border-collapse: collapse;
}

th,
td {
  vertical-align: top;
  padding: 0.3em;
}

tr :nth-child(2),
tr :nth-child(3) {
  text-align: right;
}

tr :nth-child(1),
tr :nth-child(4) {
  text-align: left;
}

tbody tr:nth-child(odd) {
  background-color: #eee;
}

tfoot {
  border-top: 1px solid #999;
}

tfoot tr :nth-child(1) {
  text-align: right;
}

tfoot tr :nth-child(2) {
  text-align: left;
}
```

Per la domanda bonus, può rendere il layout della tabella più prevedibile aggiungendo {{cssxref("table-layout")}} con un valore di [`fixed`](/it/docs/Web/CSS/table-layout#fixed) e una `width` esplicita:

```css
table {
  table-layout: fixed;
  width: 100%;
}
```

</details>

## Veda anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
- [Web Typography: Designing Tables to be Read, Not Looked At](https://alistapart.com/article/web-typography-tables/) su alistapart.com (2017)
