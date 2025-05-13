---
title: Stilizzare le tabelle
slug: Learn_web_development/Core/Styling_basics/Tables
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics")}}

Stilizzare una tabella HTML non è l'attività più affascinante del mondo, ma a volte tutti dobbiamo farlo. Questo articolo spiega come far apparire belle le tabelle HTML, mettendo in evidenza alcune tecniche specifiche di styling delle tabelle.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics"
          >Tabelle HTML</a
        >, CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Gestire la spaziatura nelle tabelle, inclusa la riduzione degli spazi fra i bordi.</li>
          <li>Evidenziare chiaramente diverse aree della tabella, inclusi intestazioni, didascalia, intestazione, corpo e piè di pagina.</li>
          <li>Come implementare le strisce zebra e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Una tipica tabella HTML

Iniziamo osservando una tipica tabella HTML. Beh, dico tipica — la maggior parte degli esempi di tabelle HTML riguarda scarpe, il meteo o dipendenti; abbiamo deciso di rendere il tutto più interessante rendendolo riguardante famose punk band britanniche. Il markup appare così:

```html
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

    <!-- several other great bands -->

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

La tabella è ben marcata, facilmente stilizzabile e accessibile, grazie a funzionalità come [`scope`](/it/docs/Web/HTML/Reference/Elements/th#scope), {{htmlelement("caption")}}, {{htmlelement("thead")}}, {{htmlelement("tbody")}}, ecc. Sfortunatamente, non appare bene quando viene visualizzata sullo schermo (vedere in diretta su [punk-bands-unstyled.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-unstyled.html)):

![una tabella non stilizzata che mostra un riassunto delle famose punk band del Regno Unito](table-unstyled.png)

Con solo lo stile predefinito del browser appare angusta, difficile da leggere e noiosa. È necessario utilizzare del CSS per sistemarla.

## Iniziare a stilizzare la nostra tabella

Lavoriamo insieme sullo stile del nostro esempio di tabella.

1. Per iniziare, faccia una copia locale del [markup di esempio](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/punk-bands-unstyled.html), scarichi entrambe le immagini ([noise](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/noise.png) e [leopardskin](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/leopardskin.jpg)), e metta i tre file risultanti in una directory di lavoro sul computer locale.
2. Successivamente, crei un nuovo file denominato `style.css` e lo salvi nella stessa directory degli altri file.
3. Colleghi il CSS all'HTML inserendo la seguente linea di HTML all'interno del suo {{htmlelement("head")}}:

   ```html
   <link href="style.css" rel="stylesheet" />
   ```

## Spaziatura e layout

La prima cosa da fare è sistemare la spaziatura/layout — lo stile di default delle tabelle è così angusto! Per farlo, aggiunga il seguente CSS al suo file `style.css`:

```css
/* spacing */

table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
  border: 3px solid purple;
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
```

Le parti più importanti da notare sono le seguenti:

- Un valore {{cssxref("table-layout")}} di `fixed` è generalmente una buona idea da impostare sulla sua tabella, poiché rende il comportamento della tabella un po' più prevedibile di default. Normalmente, le colonne della tabella tendono a essere dimensionate in base alla quantità di contenuto che contengono, il che produce risultati strani. Con `table-layout: fixed`, è possibile dimensionare le colonne in base alla larghezza delle intestazioni e quindi gestire i contenuti di conseguenza. Questo è il motivo per cui abbiamo selezionato le quattro diverse intestazioni con il selettore `thead th:nth-child(n)` ({{cssxref(":nth-child")}}) ("Seleziona il n-esimo figlio che è un elemento {{htmlelement("th")}} in una sequenza, all'interno di un elemento {{htmlelement("thead")}}") e le abbiamo assegnato larghezze percentuali impostate. La larghezza dell'intera colonna segue la larghezza della sua intestazione, creando un modo comodo per dimensionare le colonne della tabella. Chris Coyier discute questa tecnica in maggiore dettaglio in [Fixed Table Layouts](https://css-tricks.com/fixing-tables-long-strings/).

  Abbiamo abbinato questo a una {{cssxref("width")}} del 100%, il che significa che la tabella riempirà qualsiasi contenitore in cui è inserita, risultando piacevolmente reattiva (anche se servirà ancora un po' di lavoro per farla apparire bene su larghezze di schermo ristrette).

- Un valore {{cssxref("border-collapse")}} di `collapse` è una pratica standard per qualsiasi sforzo di styling delle tabelle. Di default, quando si impostano bordi sugli elementi della tabella, avranno tutti uno spazio tra di loro, come l'immagine sottostante illustra: ![una tabella 2x2 con spaziatura standard tra i bordi che mostra nessun collasso del bordo](no-border-collapse.png) Questo non appare molto bello (anche se potrebbe essere l'aspetto che vuole, chi lo sa?). Con `border-collapse: collapse;` impostato, i bordi si comprimono in uno solo, il che sembra molto meglio: ![una tabella 2x2 con la proprietà border-collapse impostata su collapse che mostra i bordi che si comprimono in uno](border-collapse.png)
- Abbiamo messo un {{cssxref("border")}} attorno all'intera tabella, che è necessario perché metteremo dei bordi attorno all'intestazione e al piè di pagina della tabella in seguito — appare davvero strano e disgiunto quando non si ha alcun bordo attorno all'intero esterno della tabella e si finiscono con dei vuoti.
- Abbiamo impostato una certa {{cssxref("padding")}} sugli elementi {{htmlelement("th")}} e {{htmlelement("td")}} — questo dà agli elementi dati un po' di spazio per respirare, rendendo la tabella molto più leggibile.

A questo punto, la nostra tabella appare già molto meglio:

![una tabella semi-stilizzata con spaziatura per rendere i dati più leggibili e che mostra un riassunto delle famose punk band del Regno Unito](table-with-spacing.png)

## Alcune semplici tipografie

Ora sistemeremo un po' il nostro testo.

Prima di tutto, abbiamo trovato un font su [Google Fonts](https://fonts.google.com/) che è adatto per una tabella sulle punk band. Può andare là e trovare uno diverso, se lo desidera; dovrà solo sostituire il nostro elemento {{htmlelement("link")}} fornito e la dichiarazione personalizzata {{cssxref("font-family")}} con quelli che le fornisce Google Fonts.

Prima, aggiunga il seguente elemento {{htmlelement("link")}} nel suo head HTML, appena sopra il suo elemento `<link>` esistente:

```html
<link
  href="https://fonts.googleapis.com/css?family=Rock+Salt"
  rel="stylesheet"
  type="text/css" />
```

Ora aggiunga il seguente CSS al suo file `style.css`, sotto l'aggiunta precedente:

```css
/* typography */

html {
  font-family: "helvetica neue", helvetica, arial, sans-serif;
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
```

Non c'è niente di veramente specifico per le tabelle qui; stiamo generalmente modificando lo stile del font per rendere le cose più facili da leggere:

- Abbiamo impostato uno stack di font sans-serif globale; questa è puramente una scelta stilistica. Abbiamo anche impostato il nostro font personalizzato sulle intestazioni all'interno degli elementi {{htmlelement("thead")}} e {{htmlelement("tfoot")}}, per un aspetto punk, grunge.
- Abbiamo impostato un po' di {{cssxref("letter-spacing")}} sulle intestazioni e sulle celle, poiché riteniamo che favorisca la leggibilità. Anche in questo caso, principalmente una scelta stilistica.
- Abbiamo allineato il testo al centro nelle celle della tabella all'interno del {{htmlelement("tbody")}} in modo che si allini con le intestazioni. Di default, alle celle viene dato un valore di {{cssxref("text-align")}} `left`, e alle intestazioni viene dato un valore `center`, ma in generale sembra meglio avere gli allineamenti impostati allo stesso modo per entrambi. Il peso grassetto predefinito sui font delle intestazioni è sufficiente per differenziare il loro aspetto.
- Abbiamo allineato a destra l'intestazione all'interno del {{htmlelement("tfoot")}} in modo che sia visivamente associata meglio al suo dato.

Il risultato appare un po' più ordinato:

![una tabella stilizzata con uno stack di font sans-serif globale e buona spaziatura per rendere i dati più leggibili e che mostra un riassunto delle famose punk band del Regno Unito](table-with-typography.png)

## Grafica e colori

Ora passiamo a grafiche e colori! Poiché la tabella è piena di punk e atteggiamento, dobbiamo darle un po' di stile luminoso e imponente per adattarla. Non si preoccupi, non deve rendere le sue tabelle così vistose — può optare per qualcosa di più sobrio ed elegante.

Inizi con l'aggiungere il seguente CSS al suo file `style.css`, ancora una volta alla fine:

```css
/* graphics and colors */

thead,
tfoot {
  background: url(leopardskin.jpg);
  color: white;
  text-shadow: 1px 1px 1px black;
}

thead th,
tfoot th,
tfoot td {
  background: linear-gradient(to bottom, rgb(0 0 0 / 10%), rgb(0 0 0 / 50%));
  border: 3px solid purple;
}
```

Ancora, non c'è nulla di specifico per le tabelle qui, ma vale la pena notare alcune cose.

Abbiamo aggiunto un {{cssxref("background-image")}} al {{htmlelement("thead")}} e al {{htmlelement("tfoot")}}, e cambiato il {{cssxref("color")}} di tutto il testo all'interno dell'intestazione e del piè di pagina in bianco (e dato un {{cssxref("text-shadow")}}) in modo che sia leggibile. Dovrebbe sempre assicurarsi che il suo testo contrasti bene con il suo sfondo, in modo che sia leggibile.

Abbiamo anche aggiunto un gradiente lineare agli elementi {{htmlelement("th")}} e {{htmlelement("td")}} all'interno dell'intestazione e del piè di pagina per avere una bella consistenza, oltre ad aver dato a quegli elementi un bordo viola acceso. È utile avere più elementi annidati disponibili in modo da poter sovrapporre gli stili uno sopra l'altro. Sì, avremmo potuto posizionare sia l'immagine di sfondo che il gradiente lineare sugli elementi {{htmlelement("thead")}} e {{htmlelement("tfoot")}} usando più immagini di sfondo, ma abbiamo deciso di farlo separatamente per il vantaggio dei browser meno recenti che non supportano più immagini di sfondo né gradienti lineari.

### Strisce "zebra"

Abbiamo voluto dedicare una sezione separata per mostrarle come implementare le **strisce zebra** — righe alternate di colore che rendono più facili da leggere e interpretare le diverse righe di dati nella sua tabella. Aggiunga il seguente CSS alla fine del suo file `style.css`:

```css
/* zebra striping */

tbody tr:nth-child(odd) {
  background-color: #ff33cc;
}

tbody tr:nth-child(even) {
  background-color: #e495e4;
}

tbody tr {
  background-image: url(noise.png);
}

table {
  background-color: #ff33cc;
}
```

- In precedenza ha visto il selettore {{cssxref(":nth-child")}} usato per selezionare specifici elementi figlio. Può anche essere fornito di una formula come parametro, quindi selezionerà una sequenza di elementi. La formula `2n+1` selezionerebbe tutti i figli numerati dispari (1, 3, 5, ecc.) e la formula `2n` selezionerebbe tutti i figli numerati pari (2, 4, 6, ecc.) Abbiamo utilizzato le parole chiave `odd` e `even` nel nostro codice, che fanno esattamente le stesse cose delle suddette formule. In questo caso, stiamo dando righe dispari e pari colori differenti (vivaci).
- Abbiamo anche aggiunto una tessera di sottofondo ripetuta a tutte le righe del corpo, che è solo un po' di rumore (un `.png` semi-trasparente con un po' di distorsione visiva su di esso) per fornire consistenza.
- Infine, abbiamo dato a tutta la tabella un colore di sfondo solido in modo che i browser che non supportano il selettore `:nth-child` abbiano comunque uno sfondo per le righe del corpo.

Questa esplosione di colori si traduce nel seguente aspetto:

![una tabella ben stilizzata con uno sfondo ripetuto nelle righe del corpo e con tutta la tabella un colore di sfondo solido per rendere i dati che mostrano un riassunto delle famose punk band del Regno Unito più accattivanti](table-with-color.png)

Ora, questo potrebbe essere un po' esagerato e non di suo gusto, ma il punto che stiamo cercando di fare è che le tabelle non devono essere noiose e accademiche.

## Stilizzare la didascalia

Rimane solo una cosa da fare con la nostra tabella — stilizzare la didascalia. Per farlo, aggiunga il seguente stile alla fine del suo file `style.css`:

```css
/* caption */

caption {
  font-family: "Rock Salt", cursive;
  padding: 20px;
  font-style: italic;
  caption-side: bottom;
  color: #666;
  text-align: right;
  letter-spacing: 1px;
}
```

Non c'è nulla di notevole qui, tranne per la proprietà {{cssxref("caption-side")}}, a cui è stato dato un valore di `bottom`. Ciò fa sì che la didascalia sia posizionata sul fondo della tabella, il che insieme alle altre dichiarazioni ci dà questo aspetto finale (vedere in diretta su [punk-bands-complete.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html)):

![uno sfondo bianco sotto la tabella stilizzata contenente una didascalia di ciò di cui tratta la tabella. "un riassunto delle famose punk band del Regno Unito" in questo caso](table-with-caption.png)

## Consigli rapidi per stilizzare le tabelle

Prima di andare avanti, abbiamo pensato di fornire un elenco rapido dei punti più utili illustrati sopra:

- Renda il suo markup tabella il più semplice possibile e mantenga le cose flessibili, ad esempio utilizzando percentuali, in modo che il design sia più reattivo.
- Usi {{cssxref("table-layout", "table-layout: fixed")}} per creare un layout tabella più prevedibile che le consenta di impostare facilmente le larghezze delle colonne impostando {{cssxref("width")}} sulle loro intestazioni ({{htmlelement("th")}}).
- Usi {{cssxref("border-collapse", "border-collapse: collapse")}} per far sì che i bordi degli elementi della tabella si comprimano l'uno nell'altro, producendo un aspetto più ordinato e facile da controllare.
- Usi {{htmlelement("thead")}}, {{htmlelement("tbody")}}, e {{htmlelement("tfoot")}} per suddividere la sua tabella in blocchi logici e fornire ulteriori punti per applicare CSS, in modo che sia più facile stratificare gli stili uno sopra l'altro se richiesto.
- Usi le strisce zebra per rendere più facili da leggere le righe alternative.
- Usi {{cssxref("text-align")}} per allineare il testo delle sue {{htmlelement("th")}} e {{htmlelement("td")}}, per rendere le cose più ordinate e facili da seguire.

## Verifichi le sue competenze!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di andare avanti — veda [Verifichi le sue competenze: Tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Tables).

## Sommario

Avendo ora stilizzato le tabelle, abbiamo bisogno di qualcos'altro per occupare il nostro tempo. L'articolo successivo esplora il debug del CSS — come risolvere problemi come layout che non appaiono come dovrebbero o proprietà che non si applicano quando lei pensa che dovrebbero. Questo include informazioni su come usare i DevTools del browser per trovare soluzioni ai suoi problemi.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics")}}
