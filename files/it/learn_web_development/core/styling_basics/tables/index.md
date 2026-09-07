---
title: Assegnare stili alle tabelle
slug: Learn_web_development/Core/Styling_basics/Tables
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Images", "Learn_web_development/Core/Styling_basics/Home_color_scheme_search", "Learn_web_development/Core/Styling_basics")}}

Assegnare stili a una tabella HTML non è il lavoro più entusiasmante del mondo, ma a volte è necessario farlo. Questo articolo spiega come rendere gradevoli le tabelle HTML, evidenziando alcune tecniche specifiche per l'assegnazione di stili alle tabelle.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics"
          >tabelle HTML</a
        >, CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Gestione della spaziatura nelle tabelle, incluso il collasso dei bordi.</li>
          <li>Evidenziare chiaramente le diverse regioni della tabella, inclusi intestazioni, didascalia, header, corpo e footer.</li>
          <li>Come implementare le strisce zebrata e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Una tipica tabella HTML

Iniziamo osservando una tipica tabella HTML. Beh, dico tipica: la maggior parte degli esempi di tabelle HTML riguarda scarpe, meteo o dipendenti; abbiamo deciso di rendere le cose più interessanti parlando di famose band punk del Regno Unito. Il markup è il seguente:

```html live-sample___unstyled live-sample___punk-style live-sample___best-practice-style
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
      <td>If The Kids Are United</td>
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
      <th scope="row" colspan="2">Total albums</th>
      <td colspan="2">77</td>
    </tr>
  </tfoot>
</table>
```

La tabella ha un markup ben strutturato, è facile da stilizzare ed è accessibile, grazie a funzionalità quali [`scope`](/it/docs/Web/HTML/Reference/Elements/th#scope), {{htmlelement("caption")}}, {{htmlelement("thead")}}, {{htmlelement("tbody")}}, ecc. Sfortunatamente, non ha un aspetto eccezionale. Con il solo stile predefinito del browser appare troppo compatta, difficile da leggere e un po' noiosa:

{{embedlivesample("unstyled", "", "200")}}

È necessario usare un po' di CSS per sistemarla. È possibile stilizzare una tabella in qualunque modo usando CSS. Per esempio, è stato creato questo design dall'aspetto piuttosto "punk":

```css hidden live-sample___punk-style
/* font import */
@import "https://fonts.googleapis.com/css2?family=Rock+Salt&display=swap";

/* spacing */
table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
  border: 3px solid purple;
}

thead th {
  line-height: 1.5;
}

thead th:nth-child(1) {
  width: 30%;
}

thead th:nth-child(2) {
  width: 20%;
}

thead th:nth-child(3) {
  width: 15%;
}

thead th:nth-child(4) {
  width: 35%;
}

th,
td {
  padding: 20px;
}

/* typography */
html {
  font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif;
}

thead th,
tfoot th {
  font-family: "Rock Salt", cursive;
}

th {
  letter-spacing: 2px;
}

td {
  letter-spacing: 1px;
}

tbody td {
  text-align: center;
}

tfoot th {
  text-align: right;
}

/* graphics */
thead,
tfoot {
  background: url("https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/leopardskin.jpg");
  color: white;
}

thead th,
tfoot th,
tfoot td {
  background: linear-gradient(to bottom, rgb(0 0 0 / 0.1), rgb(0 0 0 / 0.5));
  border: 3px solid purple;
  text-shadow: 1px 1px 1px black;
}

tbody tr:nth-child(odd) {
  background-color: #ff33cc;
}

tbody tr:nth-child(even) {
  background-color: #e495e4;
}

tbody tr {
  background-image: url("https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/noise.png");
}

table {
  background-color: #ff33cc;
}

/* caption */
caption {
  font-family: "Rock Salt", cursive;
  padding: 20px;
  font-style: italic;
  caption-side: bottom;
  color: #666666;
  text-align: right;
  letter-spacing: 1px;
}
```

{{embedlivesample("punk-style", "", "500")}}

Tuttavia, questo design è piuttosto appariscente. In questo articolo verrà mostrato come applicare alcune buone pratiche per il design delle tabelle, come illustrato in [Web Typography: designing tables to be read not looked at](https://alistapart.com/article/web-typography-tables/).

## Iniziare a stilizzare la tabella

Vediamo insieme come stilizzare l'esempio di tabella.

1. Per iniziare, creare una copia locale del markup di esempio [mostrato in precedenza](#una_tipica_tabella_html) e salvarla in una directory di lavoro nel computer locale.
2. Creare quindi un nuovo file denominato `style.css` e salvarlo nella stessa directory degli altri file.
3. Collegare il CSS all'HTML inserendo la seguente riga HTML all'interno di {{htmlelement("head")}}:

   ```html
   <link href="style.css" rel="stylesheet" />
   ```

Caricare l'HTML in un browser per vedere il suo aspetto predefinito.

## Aggiornare il font

Iniziare il CSS aggiungendo la seguente regola:

```css
html {
  font-family: "Helvetica", "Arial", sans-serif;
}
```

## Spaziatura

La prima cosa da fare alla tabella è sistemare la spaziatura: lo stile predefinito delle tabelle è troppo compatto. Per farlo, aggiungere il seguente CSS alla fine del file `style.css`:

```css
table {
  table-layout: fixed;
  width: 90%;
  margin: 10px auto;
  border-collapse: collapse;
}

th,
td {
  padding: 0.6em;
}
```

Le parti più importanti da notare sono le seguenti:

- Un valore {{cssxref("table-layout")}} pari a `fixed` è generalmente una buona scelta per la tabella, poiché fa sì che la tabella si comporti in modo un po' più prevedibile per impostazione predefinita. Normalmente, le colonne di una tabella tendono a essere dimensionate in base alla quantità di contenuto che contengono, producendo risultati insoliti. Con `table-layout: fixed`, è possibile dimensionare le colonne in base alla larghezza delle loro intestazioni, quindi gestire il contenuto in modo appropriato. Chris Coyier illustra questa tecnica più dettagliatamente in [Fixed Table Layouts](https://css-tricks.com/fixing-tables-long-strings/).

- Il layout fisso è stato abbinato a una {{cssxref("width")}} di `90%` e a una {{cssxref("margin")}} di `10px auto`. Queste impostazioni indicano che la tabella riempirà quasi completamente il viewport e sarà centrata orizzontalmente.

- Un valore {{cssxref("border-collapse")}} pari a `collapse` è una buona pratica standard per qualsiasi tentativo di stilizzare una tabella. Per impostazione predefinita, quando si impostano bordi sugli elementi della tabella, questi presentano tutti dello spazio tra loro, come illustrato nell'immagine seguente: ![una tabella 2 per 2 con spaziatura predefinita tra i bordi che mostra l'assenza di collasso dei bordi](no-border-collapse.png) Questo non ha un aspetto molto gradevole, anche se potrebbe essere l'aspetto desiderato. Con `border-collapse: collapse;`, i bordi vengono uniti in uno solo, con un risultato molto migliore: ![una tabella 2 per 2 con la proprietà border-collapse impostata su collapse che mostra i bordi uniti in uno solo](border-collapse.png)
- Sono stati impostati alcuni {{cssxref("padding")}} sugli elementi {{htmlelement("th")}} e {{htmlelement("td")}}: questo dà agli elementi di dati un po' di spazio, rendendo la tabella molto più leggibile.

Salvare il codice e aggiornare il browser per vedere i risultati.

## Allineamento

Successivamente, verrà gestito l'allineamento dei diversi tipi di dati all'interno delle celle. Le buone pratiche indicano di allineare il testo a sinistra e i numeri a destra; il seguente CSS permette di farlo, quindi aggiungerlo ora alla fine del file CSS.

```css
tr :nth-child(2),
tr :nth-child(3) {
  text-align: right;
  width: 15%;
}

tr :nth-child(1),
tr :nth-child(4) {
  text-align: left;
  width: 35%;
}

tfoot tr :nth-child(1) {
  text-align: right;
}

tfoot tr :nth-child(2) {
  text-align: left;
}
```

Qui è stata usata la pseudo-classe {{cssxref(":nth-child")}}; un selettore utile che consente di selezionare uno specifico figlio numerato di un elemento oppure una sequenza specifica. Qui viene usata per selezionare specifici elementi `<td>` all'interno degli elementi <th>.

Notare che sono state impostate anche larghezze specifiche sulle righe della tabella, con le righe contenenti testo impostate molto più larghe rispetto alle righe contenenti numeri. Questa è una buona idea: le righe con più contenuto necessitano di più spazio per offrire al contenuto la maggiore possibilità possibile di restare su una sola riga. Le righe con meno contenuto non necessitano di tanto spazio per visualizzare i dati e, in effetti, se viene assegnato molto spazio, i dati risultano un po' dispersi e quindi più difficili da leggere.

Occorre inoltre assicurarsi che gli elementi di dati siano allineati alla parte superiore delle celle anziché al centro. Per ottenere questo risultato, è possibile usare la proprietà {{cssxref("vertical-align")}}. Aggiornare la regola `th, td` esistente come segue:

```css
th,
td {
  vertical-align: top;
  padding: 0.3em;
}
```

Ancora una volta, salvare e aggiornare per vedere l'effetto degli ultimi aggiornamenti CSS.

## Aggiungere bordi

La tabella ha già un aspetto molto migliore, ma è opportuno aggiungere alcuni bordi per fornire una separazione visiva tra la `<caption>` della tabella, i dati e la riga dei totali in fondo. Per farlo, aggiungere le seguenti regole al CSS:

```css
tfoot {
  border-top: 1px solid #999999;
}
```

Successivamente, aggiornare la regola `table` esistente come segue:

```css
table {
  table-layout: fixed;
  width: 90%;
  margin: 10px auto;
  border-collapse: collapse;
  border-top: 1px solid #999999;
  border-bottom: 1px solid #999999;
}
```

Salvare e aggiornare; la tabella dovrebbe iniziare ad apparire piuttosto leggibile.

## Strisce zebrata

È stata dedicata una sezione separata per mostrare come implementare le **strisce zebrata**: righe di colori alternati che rendono le diverse righe di dati della tabella più facili da analizzare e leggere. Aggiungere il seguente CSS alla fine del file `style.css`:

```css
tbody tr:nth-child(odd) {
  background-color: #dddddd;
}
```

In precedenza è stato mostrato il selettore {{cssxref(":nth-child")}} usato per selezionare specifici elementi figli. Può anche ricevere una formula come parametro, in modo da selezionare una sequenza di elementi. La formula `2n+1` selezionerebbe tutti i figli con numero dispari (1, 3, 5, ecc.), mentre la formula `2n` selezionerebbe tutti i figli con numero pari (2, 4, 6, ecc.). Nel codice è stata usata la parola chiave `odd`, che è un'abbreviazione della formula `2n+1` (`even` è l'abbreviazione di `2n`).

Ancora una volta, non dimenticare di salvare e aggiornare per vedere il risultato.

## Stilizzare la didascalia

Resta un'ultima cosa da fare con la tabella: stilizzare la didascalia. Per farlo, aggiungere quanto segue alla fine del file `style.css`:

```css
caption {
  padding: 1em;
  font-style: italic;
  caption-side: bottom;
  letter-spacing: 1px;
}
```

Non c'è nulla di particolare qui, tranne la proprietà {{cssxref("caption-side")}}, a cui è stato assegnato il valore `bottom`. Questo fa sì che la didascalia sia posizionata in fondo alla tabella.

## Tabella completata

Il design finale della tabella dovrebbe apparire così:

```css hidden live-sample___best-practice-style
html {
  font-family: "Helvetica", "Arial", sans-serif;
}

table {
  table-layout: fixed;
  width: 90%;
  margin: 10px auto;
  border-collapse: collapse;
  border-top: 1px solid #999999;
  border-bottom: 1px solid #999999;
}

th,
td {
  vertical-align: top;
  padding: 0.6em;
}

tr :nth-child(2),
tr :nth-child(3) {
  text-align: right;
  width: 15%;
}

tr :nth-child(1),
tr :nth-child(4) {
  text-align: left;
  width: 35%;
}

tfoot tr :nth-child(1) {
  text-align: right;
}

tfoot tr :nth-child(2) {
  text-align: left;
}

tfoot {
  border-top: 1px solid #999999;
}

tbody tr:nth-child(odd) {
  background-color: #dddddd;
}

caption {
  padding: 1em;
  font-style: italic;
  caption-side: bottom;
  letter-spacing: 1px;
}
```

{{embedlivesample("best-practice-style", "", "520")}}

## Suggerimenti rapidi per stilizzare le tabelle

Prima di proseguire, ecco un rapido elenco dei punti più utili illustrati sopra:

- Rendere il markup della tabella il più semplice possibile e mantenere la flessibilità.
- Usare {{cssxref("table-layout", "table-layout: fixed")}} per creare un layout della tabella più prevedibile, che consente di impostare facilmente le larghezze delle colonne impostando {{cssxref("width")}} sulle relative intestazioni ({{htmlelement("th")}}).
- Usare {{cssxref("border-collapse", "border-collapse: collapse")}} per fare in modo che i bordi degli elementi della tabella collassino gli uni negli altri, producendo un aspetto più ordinato e più facile da controllare.
- Usare {{htmlelement("thead")}}, {{htmlelement("tbody")}} e {{htmlelement("tfoot")}} per suddividere la tabella in sezioni logiche e fornire ulteriori punti a cui applicare CSS, così da rendere più semplice sovrapporre gli stili, se necessario.
- Usare le strisce zebrata per rendere più facili da leggere le righe alternate.
- Usare {{cssxref("text-align")}} per allineare il testo di {{htmlelement("th")}} e {{htmlelement("td")}}, rendendo il tutto più ordinato e facile da seguire.

## Riepilogo

Ora che l'assegnazione di stili alle tabelle è conclusa, serve qualcos'altro su cui dedicare il tempo. Il prossimo articolo esplora il debugging CSS: come risolvere problemi quali layout che non hanno l'aspetto previsto oppure proprietà che non vengono applicate quando si ritiene che dovrebbero esserlo. Include informazioni sull'uso dei DevTools del browser per trovare soluzioni ai problemi.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Images", "Learn_web_development/Core/Styling_basics/Home_color_scheme_search", "Learn_web_development/Core/Styling_basics")}}
