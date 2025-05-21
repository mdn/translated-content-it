---
title: "Metti alla prova le tue competenze: Tabelle"
short-title: Tables
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Tables
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se hai compreso come [stilizzare le tabelle HTML in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Tables).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice sottostanti per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se hai difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito

Nella lezione su [come stilizzare le tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables), abbiamo stilizzato una tabella in modo piuttosto vistoso. In questo compito, stileremo la stessa tabella, ma utilizzando alcune buone pratiche per il design delle tabelle come delineato nell'articolo esterno [Web Typography: designing tables to be read not looked at](https://alistapart.com/article/web-typography-tables/).

La nostra tabella finita apparirà come nell'immagine sotto. Ci sono diversi modi per ottenere questo risultato, ma suggeriamo di seguire modelli simili a quelli utilizzati nel tutorial per fare le seguenti operazioni:

- Aggiungere un padding di `0.3em` alle intestazioni della tabella e ai dati, e allinearli in alto nelle loro celle.
- Allineare a destra le intestazioni e i dati delle colonne contenenti numeri.
- Allineare a sinistra le intestazioni e i dati delle colonne contenenti testo.
- Allineare a destra l'intestazione del footer della tabella.
- Allineare a sinistra i dati del footer della tabella.
- Aggiungere un bordo solido di 1px in alto e in basso con il colore esadecimale `#999` alla tabella.
- Aggiungere un bordo solido di 1px in alto con il colore esadecimale `#999` al footer.
- Rimuovere la spaziatura predefinita tra i bordi degli elementi della tabella per ottenere il risultato atteso.
- Strisciare ogni riga dispari della tabella principale con il colore esadecimale `#eee`.

![Una tabella con righe a strisce.](mdn-table-bands.png)

**Domanda bonus:** Cosa puoi fare per far sì che il layout della tabella si comporti in modo un po' più prevedibile? Pensa a come le colonne della tabella vengono dimensionate di default e come possiamo cambiare questo comportamento per dimensionare le colonne in base alla larghezza delle loro intestazioni.

Prova ad aggiornare il codice sottostante per ricreare l'esempio finito:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Di seguito un esempio su come il risultato finale potrebbe essere raggiunto, utilizzando tecniche simili alla lezione. Tuttavia ci sono diversi modi che sarebbero perfettamente corretti, forse leggermente più verbosi.

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

Per la domanda bonus, puoi rendere il layout della tabella più prevedibile aggiungendo {{cssxref("table-layout")}} con un valore di [`fixed`](/it/docs/Web/CSS/table-layout#fixed) e una `width` esplicita:

```css
table {
  table-layout: fixed;
  width: 100%;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo styling in CSS](/it/docs/Learn_web_development/Core/Styling_basics)
- [Web Typography: Designing Tables to be Read, Not Looked At](https://alistapart.com/article/web-typography-tables/) su alistapart.com (2017)
