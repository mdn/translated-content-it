---
title: Stile delle tabelle
slug: Learn_web_development/Core/Styling_basics/Tables
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics")}}

Stilare una tabella HTML non è il compito più glamour al mondo, ma a volte dobbiamo farlo. Questo articolo spiega come far apparire le tabelle HTML al meglio, evidenziando alcune tecniche specifiche di stilizzazione delle tabelle.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics"
          >Tabelle HTML</a
        >, CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Gestire gli spazi nelle tabelle, inclusa la fusione dei bordi.</li>
          <li>Mettere chiaramente in evidenza le diverse regioni della tabella, inclusi intestazioni, didascalie, header, corpo e footer.</li>
          <li>Come implementare l'alternanza a strisce (zebra striping) e perché è utile.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Una tipica tabella HTML

Iniziamo esaminando una tipica tabella HTML. Beh, dico tipica — la maggior parte degli esempi di tabelle HTML riguardano scarpe, il tempo atmosferico o dipendenti; abbiamo deciso di rendere le cose più interessanti parlandovi di famose band punk britanniche. Il markup è il seguente:

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

La tabella è ben marcata, facilmente stilizzabile e accessibile, grazie a caratteristiche come [`scope`](/it/docs/Web/HTML/Reference/Elements/th#scope), {{htmlelement("caption")}}, {{htmlelement("thead")}}, {{htmlelement("tbody")}}, ecc. Sfortunatamente, non appare bene quando viene resa sullo schermo (vedi dal vivo su [punk-bands-unstyled.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-unstyled.html)):

![una tabella non stilizzata che mostra un riassunto delle famose band punk britanniche](table-unstyled.png)

Con solo lo stile predefinito del browser, appare compatta, difficile da leggere e noiosa. Dobbiamo usare un po' di CSS per sistemare la situazione.

## Iniziare con lo stile della nostra tabella

Lavoriamo insieme per stilizzare l'esempio della nostra tabella.

1. Per iniziare, crea una copia locale del [markup di esempio](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/punk-bands-unstyled.html), scarica entrambe le immagini ([noise](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/noise.png) e [leopardskin](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/leopardskin.jpg)), e metti i tre file risultanti in una directory di lavoro sul tuo computer locale.
2. Successivamente, crea un nuovo file chiamato `style.css` e salvalo nella stessa directory degli altri file.
3. Collega il CSS all'HTML inserendo la seguente linea di HTML all'interno del tuo {{htmlelement("head")}}:

   ```html
   <link href="style.css" rel="stylesheet" />
   ```

## Spaziatura e layout

La prima cosa da fare è sistemare la spaziatura/il layout — lo stile predefinito delle tabelle è così compatto! Per farlo, aggiungi il seguente CSS al tuo file `style.css`:

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

- Un valore di {{cssxref("table-layout")}} `fixed` è generalmente una buona idea da impostare sulla tua tabella, poiché fa comportare la tabella in modo un po' più prevedibile per default. Normalmente, le colonne delle tabelle tendono a essere dimensionate in base alla quantità di contenuto che contengono, il che produce alcuni risultati strani. Con `table-layout: fixed`, puoi dimensionare le colonne in base alla larghezza delle loro intestazioni, e poi gestire il loro contenuto in modo appropriato. Questo è il motivo per cui abbiamo selezionato le quattro diverse intestazioni con il selettore `thead th:nth-child(n)` ({{cssxref(":nth-child")}}) ("Seleziona l'n-esimo figlio che è un elemento {{htmlelement("th")}} in una sequenza, all'interno di un elemento {{htmlelement("thead")}}") e assegnato loro larghezze percentuali impostate. L'intera larghezza della colonna segue la larghezza della sua intestazione, rendendo gradevole il dimensionamento delle colonne della tabella. Chris Coyier discute questa tecnica in modo più dettagliato in [Fixed Table Layouts](https://css-tricks.com/fixing-tables-long-strings/).

  Abbiamo accoppiato questo con una {{cssxref("width")}} del 100%, il che significa che la tabella riempirà qualsiasi contenitore in cui viene inserita, risultando ben responsiva (sebbene sia necessario ancora del lavoro per ottenere un bell'aspetto su larghezze di schermo strette).

- Un valore di {{cssxref("border-collapse")}} `collapse` è una prassi standard per qualsiasi sforzo di stilizzazione delle tabelle. Per default, quando imposti i bordi sugli elementi della tabella, questi avranno tutti uno spazio tra di loro, come illustra l'immagine seguente: ![una tabella 2x2 con la spaziatura predefinita tra i bordi che mostra nessun collasso dei bordi](no-border-collapse.png) Questo non sembra molto bello (anche se potrebbe essere l'aspetto che desideri, chissà?). Con `border-collapse: collapse;` impostato, i bordi si uniranno in uno solo, che appare molto meglio: ![una tabella 2x2 con la proprietà border-collapse impostata su collapse che mostra i bordi uniti in uno solo](border-collapse.png)
- Abbiamo messo un {{cssxref("border")}} intorno all'intera tabella, che è necessario perché metteremo alcuni bordi intorno all'intestazione e al footer della tabella più avanti — sembra davvero strano e disgiunto quando non hai un bordo intorno all'intero esterno della tabella e finisci con degli spazi vuoti.
- Abbiamo impostato del {{cssxref("padding")}} sugli elementi {{htmlelement("th")}} e {{htmlelement("td")}} — questo dà agli elementi di dati un po' di spazio per respirare, rendendo la tabella molto più leggibile.

A questo punto, la nostra tabella già sembra molto più bella:

![una tabella semi-stilizzata con spaziatura tale da rendere i dati più leggibili e che mostra un riassunto delle famose band punk britanniche](table-with-spacing.png)

## Alcuna tipografia semplice

Ora sistemeremo un po' il nostro testo.

Prima di tutto, abbiamo trovato un font su [Google Fonts](https://fonts.google.com/) che è adatto per una tabella su band punk. Puoi andare lì e trovarne uno diverso se vuoi; dovrai solo sostituire l'elemento {{htmlelement("link")}} che forniamo e la dichiarazione personalizzata {{cssxref("font-family")}} con quelli che Google Fonts ti dà.

Per prima cosa, aggiungi il seguente elemento {{htmlelement("link")}} nella sezione head del tuo HTML, appena sopra il tuo `<link>` esistente:

```html
<link
  href="https://fonts.googleapis.com/css?family=Rock+Salt"
  rel="stylesheet"
  type="text/css" />
```

Ora aggiungi il seguente CSS nel tuo file `style.css`, sotto la precedente aggiunta:

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

Non c'è nulla di specifico per le tabelle qui; stiamo generalmente sistemando la stilizzazione dei font per migliorare la leggibilità:

- Abbiamo impostato una pila globale di font sans-serif; questa è puramente una scelta stilistica. Abbiamo anche impostato il nostro font personalizzato sulle intestazioni all'interno degli elementi {{htmlelement("thead")}} e {{htmlelement("tfoot")}}, per un aspetto grunge e punk.
- Abbiamo impostato un po' di {{cssxref("letter-spacing")}} sulle intestazioni e sulle celle, poiché crediamo che aiuti la leggibilità. Anche in questo caso, è per lo più una scelta stilistica.
- Abbiamo allineato al centro il testo nelle celle della tabella all'interno del {{htmlelement("tbody")}} in modo che si allinei con le intestazioni. Per default, le celle hanno un valore di {{cssxref("text-align")}} `left`, e le intestazioni hanno un valore di `center`, ma generalmente è meglio che gli allineamenti siano impostati allo stesso modo per entrambi. Il peso del font grassetto predefinito sulle intestazioni è sufficiente per differenziare il loro aspetto.
- Abbiamo allineato a destra l'intestazione all'interno del {{htmlelement("tfoot")}} in modo che sia visivamente associato meglio con il suo dato.

Il risultato appare un po' più ordinato:

![una tabella stilizzata con una pila globale di font sans-serif e buona spaziatura per rendere i dati più leggibili e che mostra un riassunto delle famose band punk britanniche](table-with-typography.png)

## Grafica e colori

Ora passiamo a grafica e colori! Poiché la tabella è piena di punk e atteggiamento, dobbiamo darle uno stile luminoso e imponente adatto. Non preoccuparti, non devi fare le tue tabelle così rumorose — puoi optare per qualcosa di più sottile e di buon gusto.

Inizia aggiungendo il seguente CSS al tuo file `style.css`, di nuovo in fondo:

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

Ancora una volta non c'è nulla di specifico per le tabelle qui, ma è utile notare alcune cose.

Abbiamo aggiunto una {{cssxref("background-image")}} ai {{htmlelement("thead")}} e {{htmlelement("tfoot")}}, e cambiato il {{cssxref("color")}} di tutto il testo all'interno dell'intestazione e del footer in bianco (e gli abbiamo dato un {{cssxref("text-shadow")}}) in modo che sia leggibile. Dovresti sempre assicurarti che il tuo testo contrasti bene con il tuo sfondo, così che sia leggibile.

Abbiamo anche aggiunto un gradiente lineare agli elementi {{htmlelement("th")}} e {{htmlelement("td")}} all'interno dell'intestazione e del footer per un bel tocco di texture, oltre a dare a quegli elementi un bordo viola brillante. È utile avere più elementi nidificati disponibili in modo da poter sovrapporre stili l'uno sull'altro. Sì, avremmo potuto mettere sia l'immagine di sfondo che il gradiente lineare sugli elementi {{htmlelement("thead")}} e {{htmlelement("tfoot")}} utilizzando più immagini di sfondo, ma abbiamo deciso di farlo separatamente per il vantaggio dei browser più vecchi che non supportano più immagini di sfondo o gradienti lineari.

### Zebra striping

Volevamo dedicare una sezione a parte per mostrarti come implementare le **strisce zebra** — righe alternate di colore che rendono più facile interpretare e leggere le diverse righe di dati nella tua tabella. Aggiungi il seguente CSS al fondo del tuo file `style.css`:

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

- In precedenza hai visto il selettore {{cssxref(":nth-child")}} usato per selezionare elementi figli specifici. Può anche essere dato una formula come parametro, quindi selezionerà una sequenza di elementi. La formula `2n+1` selezionerebbe tutti i figli numerati dispari (1, 3, 5, ecc.) e la formula `2n` selezionerebbe tutti i figli numerati pari (2, 4, 6, ecc.) Abbiamo usato le parole chiave `odd` e `even` nel nostro codice, che fanno esattamente le stesse cose delle suddette formule. In questo caso stiamo dando alle righe dispari e pari colori diversi (appariscenti).
- Abbiamo anche aggiunto una tile di sfondo ripetuta a tutte le righe del corpo, che è solo un po' di rumore (un `.png` semitrasparente con un po' di distorsione visiva su di esso) per fornire un po' di texture.
- Infine, abbiamo dato alla tabella intera un colore di sfondo solido in modo che i browser che non supportano il selettore `:nth-child` abbiano comunque uno sfondo per le loro righe del corpo.

Questa esplosione di colori risulta nell'aspetto seguente:

![una tabella ben stilizzata con uno sfondo ripetuto nelle righe del corpo e l'intera tabella con uno sfondo solido per rendere i dati che mostrano un riassunto delle famose band punk britanniche più attraenti](table-with-color.png)

Ora, questo potrebbe essere un po' esagerato e non di tuo gusto, ma il punto che stiamo cercando di fare qui è che le tabelle non devono essere noiose e accademiche.

## Stilizzare la didascalia

C'è un'ultima cosa da fare con la nostra tabella — stilizzare la didascalia. Per farlo, aggiungi quanto segue al fondo del tuo file `style.css`:

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

Non c'è nulla di notevole qui, eccetto la proprietà {{cssxref("caption-side")}}, che è stata impostata con un valore di `bottom`. Questo fa sì che la didascalia venga posizionata sul fondo della tabella, che insieme alle altre dichiarazioni ci dà questo aspetto finale (vedi dal vivo su [punk-bands-complete.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html)):

![un fondo bianco sotto la tabella stilizzata che contiene una didascalia di ciò di cui parla la tabella. "un riassunto delle famose band punk britanniche" in questo caso](table-with-caption.png)

## Consigli rapidi per la stilizzazione delle tabelle

Prima di passare oltre, abbiamo pensato di fornire un rapido elenco dei punti più utili illustrati sopra:

- Rendete il markup della vostra tabella il più semplice possibile e mantenete le cose flessibili, ad esempio utilizzando percentuali, così che il design sia più responsivo.
- Utilizzate {{cssxref("table-layout", "table-layout: fixed")}} per creare un layout di tabella più prevedibile che vi permetta di impostare facilmente le larghezze delle colonne impostando la {{cssxref("width")}} sulle loro intestazioni ({{htmlelement("th")}}).
- Utilizzate {{cssxref("border-collapse", "border-collapse: collapse")}} per far collassare i bordi degli elementi della tabella l'uno nell'altro, producendo un aspetto più ordinato e facile da controllare.
- Utilizzate {{htmlelement("thead")}}, {{htmlelement("tbody")}}, e {{htmlelement("tfoot")}} per suddividere la tabella in blocchi logici e fornire ulteriori luoghi per applicare CSS, in modo che sia più facile sovrapporre stili l'uno sull'altro se necessario.
- Utilizzate strisce zebra per rendere le righe alternative più facili da leggere.
- Utilizzate {{cssxref("text-align")}} per allineare il testo di {{htmlelement("th")}} e {{htmlelement("td")}}, per rendere le cose più ordinate e facili da seguire.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare che tu abbia memorizzato queste informazioni prima di proseguire — consulta [Metti alla prova le tue abilità: Tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Tables).

## Sommario

Con la stilizzazione delle tabelle ormai alle spalle, abbiamo bisogno di qualcos'altro per occupare il nostro tempo. Il prossimo articolo esplora il debugging del CSS — come risolvere problemi come layout che non appaiono come dovrebbero, o proprietà che non si applicano quando pensi che dovrebbero. Questo include informazioni sull'uso degli strumenti di sviluppo del browser per trovare soluzioni ai tuoi problemi.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics")}}
