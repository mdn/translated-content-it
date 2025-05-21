---
title: Nozioni di base sulle tabelle HTML
short-title: Nozioni di base sulle tabelle
slug: Learn_web_development/Core/Structuring_content/HTML_table_basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content")}}

Questo articolo ti introduce alle tabelle HTML, coprendo gli aspetti di base come righe, celle, intestazioni, come far estendere le celle su più colonne e righe, e come raggruppare tutte le celle in una colonna per scopi di stile.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>A cosa servono le tabelle — strutturare dati tabulari.</li>
          <li>A cosa non servono le tabelle — layout, o <em>qualsiasi altra cosa</em>.</li>
          <li>Sintassi di base delle tabelle — <code>&lt;table&gt;</code>, <code>&lt;tr&gt;</code>, e <code>&lt;td&gt;</code>.</li>
          <li>Definire le intestazioni delle tabelle con <code>&lt;th&gt;</code>.</li>
          <li>Estendere celle su più colonne e righe con <code>colspan</code> e <code>rowspan</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è una tabella?

Una tabella è un insieme strutturato di dati composto da righe e colonne (**dati tabulari**). Una tabella consente di cercare rapidamente e facilmente i valori che indicano una sorta di connessione tra diversi tipi di dati, ad esempio una persona e la sua età, o un giorno della settimana, o l'orario di una piscina locale.

![Un esempio di tabella che mostra nomi ed età di alcune persone - Chris 38, Dennis 45, Sarah 29, Karen 47.](numbers-table.png)

![Un orario di nuoto che mostra una tabella di dati di esempio](swimming-timetable.png)

Le tabelle sono molto comunemente usate nella società umana, e lo sono da molto tempo, come dimostrato da questo documento del censimento degli Stati Uniti del 1800:

![Un documento molto antico su pergamena; i dati non sono facilmente leggibili, ma mostra chiaramente l'uso di una tabella di dati.](1800-census.jpg)

Non c'è quindi da meravigliarsi che i creatori di HTML abbiano fornito un mezzo per strutturare e presentare dati tabulari sul web.

### Come funziona una tabella?

Il punto di una tabella è che è rigida. L'informazione è facilmente interpretata facendo associazioni visive tra le intestazioni di righe e colonne. Guarda la tabella seguente, ad esempio, e trova un gigante gassoso gioviano con 62 lune. Puoi trovare la risposta associando le intestazioni di riga e colonna rilevanti.

```html hidden
<table>
  <caption>
    Data about the planets of our solar system (Source:
    <a href="https://nssdc.gsfc.nasa.gov/planetary/factsheet/"
      >Nasa's Planetary Fact Sheet - Metric</a
    >).
  </caption>
  <thead>
    <tr>
      <td colspan="2"></td>
      <th scope="col">Name</th>
      <th scope="col">Mass (10<sup>24</sup>kg)</th>
      <th scope="col">Diameter (km)</th>
      <th scope="col">Density (kg/m<sup>3</sup>)</th>
      <th scope="col">Gravity (m/s<sup>2</sup>)</th>
      <th scope="col">Length of day (hours)</th>
      <th scope="col">Distance from Sun (10<sup>6</sup>km)</th>
      <th scope="col">Mean temperature (°C)</th>
      <th scope="col">Number of moons</th>
      <th scope="col">Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th colspan="2" rowspan="4" scope="rowgroup">Terrestrial planets</th>
      <th scope="row">Mercury</th>
      <td>0.330</td>
      <td>4,879</td>
      <td>5427</td>
      <td>3.7</td>
      <td>4222.6</td>
      <td>57.9</td>
      <td>167</td>
      <td>0</td>
      <td>Closest to the Sun</td>
    </tr>
    <tr>
      <th scope="row">Venus</th>
      <td>4.87</td>
      <td>12,104</td>
      <td>5243</td>
      <td>8.9</td>
      <td>2802.0</td>
      <td>108.2</td>
      <td>464</td>
      <td>0</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">Earth</th>
      <td>5.97</td>
      <td>12,756</td>
      <td>5514</td>
      <td>9.8</td>
      <td>24.0</td>
      <td>149.6</td>
      <td>15</td>
      <td>1</td>
      <td>Our world</td>
    </tr>
    <tr>
      <th scope="row">Mars</th>
      <td>0.642</td>
      <td>6,792</td>
      <td>3933</td>
      <td>3.7</td>
      <td>24.7</td>
      <td>227.9</td>
      <td>-65</td>
      <td>2</td>
      <td>The red planet</td>
    </tr>
    <tr>
      <th rowspan="4" scope="rowgroup">Jovian planets</th>
      <th rowspan="2" scope="rowgroup">Gas giants</th>
      <th scope="row">Jupiter</th>
      <td>1898</td>
      <td>142,984</td>
      <td>1326</td>
      <td>23.1</td>
      <td>9.9</td>
      <td>778.6</td>
      <td>-110</td>
      <td>67</td>
      <td>The largest planet</td>
    </tr>
    <tr>
      <th scope="row">Saturn</th>
      <td>568</td>
      <td>120,536</td>
      <td>687</td>
      <td>9.0</td>
      <td>10.7</td>
      <td>1433.5</td>
      <td>-140</td>
      <td>62</td>
      <td></td>
    </tr>
    <tr>
      <th rowspan="2" scope="rowgroup">Ice giants</th>
      <th scope="row">Uranus</th>
      <td>86.8</td>
      <td>51,118</td>
      <td>1271</td>
      <td>8.7</td>
      <td>17.2</td>
      <td>2872.5</td>
      <td>-195</td>
      <td>27</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">Neptune</th>
      <td>102</td>
      <td>49,528</td>
      <td>1638</td>
      <td>11.0</td>
      <td>16.1</td>
      <td>4495.1</td>
      <td>-200</td>
      <td>14</td>
      <td></td>
    </tr>
    <tr>
      <th colspan="2" scope="rowgroup">Dwarf planets</th>
      <th scope="row">Pluto</th>
      <td>0.0146</td>
      <td>2,370</td>
      <td>2095</td>
      <td>0.7</td>
      <td>153.3</td>
      <td>5906.4</td>
      <td>-225</td>
      <td>5</td>
      <td>
        Declassified as a planet in 2006, but this
        <a
          href="https://www.usatoday.com/story/tech/2014/10/02/pluto-planet-solar-system/16578959/"
          >remains controversial</a
        >.
      </td>
    </tr>
  </tbody>
</table>
```

```css hidden
table {
  border-collapse: collapse;
  border: 2px solid black;
}

th,
td {
  padding: 5px;
  border: 1px solid black;
}
```

{{EmbedLiveSample("How_does_a_table_work", 100, 560)}}

Quando implementate correttamente, le tabelle HTML sono gestite bene dagli strumenti di accessibilità come i lettori di schermo, quindi una tabella HTML di successo dovrebbe migliorare l'esperienza sia degli utenti vedenti che di quelli ipovedenti.

### Stile delle tabelle

Puoi anche dare un'occhiata all'[esempio dal vivo](https://mdn.github.io/learning-area/html/tables/assessment-finished/planets-data.html) su GitHub! Una cosa che noterai è che la tabella lì appare un po' più leggibile — questo perché la tabella che vedi sopra su questa pagina ha uno stile minimo, mentre la versione su GitHub ha CSS più significativi applicati.

Non avere illusioni; affinché le tabelle siano efficaci sul web, devi fornire alcune informazioni di stile con [CSS](/it/docs/Learn_web_development/Core/Styling_basics), oltre a una buona solida struttura con HTML. In questa lezione ci concentriamo sulla parte HTML; scoprirai di più sullo stile delle tabelle più avanti, nella nostra lezione [Stilizzazione delle tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables).

Non ci concentreremo sui CSS in questo modulo, ma abbiamo fornito un foglio di stile CSS minimale che puoi utilizzare per rendere le tue tabelle più leggibili rispetto al default che ottieni senza alcuno stile. Puoi trovare il [foglio di stile qui](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css), e puoi trovare anche un [template HTML](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) che applica il foglio di stile — questi insieme ti daranno un buon punto di partenza per sperimentare con le tabelle HTML.

### Quando evitare le tabelle HTML?

Le tabelle HTML dovrebbero essere usate per dati tabulari (informazioni che è facile gestire in righe e colonne) — questo è ciò per cui sono state progettate. Purtroppo, molte persone usavano i tavoli HTML per strutturare le pagine web, ad esempio una riga per contenere un'intestazione di pagina, una riga per contenere ciascuna colonna di contenuto, una riga per contenere il footer, ecc. Questa tecnica era usata perché il supporto CSS tra browser era molto più limitato. I browser moderni hanno un buon supporto CSS, quindi i layout basati sulle tabelle sono ora estremamente rari, ma potresti ancora vederli in alcuni angoli del web.

In breve, usare le tabelle per i layout invece delle [tecniche di layout CSS](/it/docs/Learn_web_development/Core/CSS_layout) è una cattiva idea. I motivi principali sono i seguenti:

1. **I layout a tabelle riducono l'accessibilità per gli utenti ipovedenti**: i [lettori di schermo](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers), utilizzati da persone non vedenti, interpretano i tag che esistono in una pagina HTML e ne leggono il contenuto all'utente. Poiché le tabelle non sono lo strumento giusto per il layout, e il markup è più complesso rispetto alle tecniche di layout CSS, l'output dei lettori di schermo sarà confuso per i loro utenti.
2. **Le tabelle producono un "tag soup"**: Come menzionato sopra, i layout a tabelle generalmente coinvolgono strutture di markup più complesse rispetto alle tecniche di layout corrette. Questo può comportare che il codice sia più difficile da scrivere, mantenere e debugare.
3. **Le tabelle non sono automaticamente responsive**: Quando usi contenitori di layout adeguati (come {{htmlelement("header")}}, {{htmlelement("section")}}, {{htmlelement("article")}}, o {{htmlelement("div")}}), la loro larghezza predefinita è del 100% dell'elemento padre. Le tabelle, invece, sono dimensionate in base al loro contenuto per impostazione predefinita, quindi sono necessarie misure aggiuntive affinché lo stile del layout della tabella funzioni efficacemente su una varietà di dispositivi.

## Apprendimento attivo: Creare la tua prima tabella

Abbiamo parlato abbastanza della teoria delle tabelle, quindi, facciamo un esempio pratico e costruiamo una semplice tabella.

1. Prima di tutto, fai una copia locale di [blank-template.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sul tuo computer locale.
2. Il contenuto di ogni tabella è racchiuso tra questi due tag: **[`<table></table>`](/it/docs/Web/HTML/Reference/Elements/table)**. Aggiungi questi all'interno del corpo del tuo HTML.
3. Il contenitore più piccolo all'interno di una tabella è una cella della tabella, che viene creata da un elemento **[`<td>`](/it/docs/Web/HTML/Reference/Elements/td)** ('td' sta per 'dati della tabella'). Aggiungi il seguente codice all'interno dei tuoi tag della tabella:

   ```html
   <td>Hi, I'm your first cell.</td>
   ```

4. Se vogliamo una riga di quattro celle, dobbiamo copiare questi tag tre volte. Aggiorna il contenuto della tua tabella in modo che appaia così:

   ```html
   <td>Hi, I'm your first cell.</td>
   <td>I'm your second cell.</td>
   <td>I'm your third cell.</td>
   <td>I'm your fourth cell.</td>
   ```

Come vedrai, le celle non sono posizionate una sotto l'altra, piuttosto sono automaticamente allineate tra loro sulla stessa riga. Ogni elemento `<td>` crea una singola cella e insieme formano la prima riga. Ogni cella che aggiungiamo fa allungare la riga.

Per evitare che questa riga cresca e per iniziare a posizionare delle celle successive su una seconda riga, dobbiamo usare l'elemento **[`<tr>`](/it/docs/Web/HTML/Reference/Elements/tr)** ('tr' sta per 'riga della tabella'). Esaminiamolo ora.

1. Posiziona le quattro celle che hai già creato all'interno dei tag `<tr>`, in questo modo:

   ```html
   <tr>
     <td>Hi, I'm your first cell.</td>
     <td>I'm your second cell.</td>
     <td>I'm your third cell.</td>
     <td>I'm your fourth cell.</td>
   </tr>
   ```

2. Ora che hai creato una riga, prova a crearne una o due in più — ogni riga deve essere racchiusa in un ulteriore elemento `<tr>`, con ogni cella contenuta in un `<td>`.

### Risultato

Questo dovrebbe risultare in una tabella che somiglia a quanto segue:

```html hidden
<table>
  <tr>
    <td>Hi, I'm your first cell.</td>
    <td>I'm your second cell.</td>
    <td>I'm your third cell.</td>
    <td>I'm your fourth cell.</td>
  </tr>

  <tr>
    <td>Second row, first cell.</td>
    <td>Cell 2.</td>
    <td>Cell 3.</td>
    <td>Cell 4.</td>
  </tr>
</table>
```

```css hidden
table {
  border-collapse: collapse;
}
td,
th {
  border: 1px solid black;
  padding: 10px 20px;
}
```

{{EmbedLiveSample("Result")}}

> [!NOTE]
> Puoi trovare anche questo su GitHub come [simple-table.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/simple-table.html) ([vedi anche direttamente](https://mdn.github.io/learning-area/html/tables/basic/simple-table.html)).

## Aggiungere intestazioni con elementi \<th>

Ora concentriamoci sulle intestazioni delle tabelle — celle speciali che si trovano all'inizio di una riga o colonna e definiscono il tipo di dati che quella riga o colonna contiene (come esempio, vedi le celle "Persona" e "Età" nel primo esempio mostrato in questo articolo). Per illustrare perché sono utili, dai un'occhiata al seguente esempio di tabella. Prima il codice sorgente:

```html
<table>
  <tr>
    <td>&nbsp;</td>
    <td>Knocky</td>
    <td>Flor</td>
    <td>Ella</td>
    <td>Juan</td>
  </tr>
  <tr>
    <td>Breed</td>
    <td>Jack Russell</td>
    <td>Poodle</td>
    <td>Streetdog</td>
    <td>Cocker Spaniel</td>
  </tr>
  <tr>
    <td>Age</td>
    <td>16</td>
    <td>9</td>
    <td>10</td>
    <td>5</td>
  </tr>
  <tr>
    <td>Owner</td>
    <td>Mother-in-law</td>
    <td>Me</td>
    <td>Me</td>
    <td>Sister-in-law</td>
  </tr>
  <tr>
    <td>Eating Habits</td>
    <td>Eats everyone's leftovers</td>
    <td>Nibbles at food</td>
    <td>Hearty eater</td>
    <td>Will eat till he explodes</td>
  </tr>
</table>
```

```css hidden
table {
  border-collapse: collapse;
}
td,
th {
  border: 1px solid black;
  padding: 10px 20px;
}
```

Ora la tabella effettivamente renderizzata:

{{EmbedLiveSample("Adding_headers_with_th_elements", "", "250")}}

Il problema qui è che, mentre puoi in qualche modo intuire cosa sta succedendo, non è così facile mettere in relazione i dati come potrebbe essere. Se le intestazioni di colonna e di riga si distinguessero in qualche modo, sarebbe molto meglio.

### Apprendimento attivo: intestazioni delle tabelle

Proviamo a migliorare questa tabella.

1. Prima, fai una copia locale dei nostri file [dogs-table.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/dogs-table.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sul tuo computer locale. L'HTML contiene lo stesso esempio di cani che hai visto sopra.
2. Per riconoscere le intestazioni delle tabelle come intestazioni, sia visivamente che semanticamente, puoi utilizzare l'elemento **[`<th>`](/it/docs/Web/HTML/Reference/Elements/th)** ('th' sta per 'intestazione tabella'). Questo funziona esattamente allo stesso modo di un `<td>`, tranne per il fatto che denota un'intestazione, non una cella normale. Vai nel tuo HTML e cambia tutti gli elementi `<td>` che circondano le intestazioni delle tabelle in elementi `<th>`.
3. Salva il tuo HTML e caricalo in un browser, e dovresti vedere che le intestazioni ora sembrano intestazioni.

> [!NOTE]
> Puoi trovare il nostro esempio finito su [dogs-table-fixed.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/dogs-table-fixed.html) su GitHub ([vedi anche direttamente](https://mdn.github.io/learning-area/html/tables/basic/dogs-table-fixed.html)).

### Perché le intestazioni sono utili?

Abbiamo già parzialmente risposto a questa domanda — è più facile trovare i dati che stai cercando quando le intestazioni si distinguono chiaramente, e il design in generale sembra migliore.

> [!NOTE]
> Le intestazioni delle tabelle vengono fornite con uno stile di default — sono in grassetto e centrate anche se non aggiungi il tuo stile alla tabella, per aiutare a farle risaltare.

Le intestazioni delle tabelle hanno anche un ulteriore vantaggio — insieme all'attributo `scope` (di cui parleremo nel prossimo articolo), permettono di rendere le tabelle più accessibili associando ogni intestazione a tutti i dati nella stessa riga o colonna. I lettori di schermo sono in grado quindi di leggere un'intera riga o colonna di dati alla volta, il che è piuttosto utile.

## Consentire alle celle di estendersi su più righe e colonne

A volte vogliamo che le celle si estendano su più righe o colonne. Prendiamo il seguente semplice esempio, che mostra i nomi di animali comuni. In alcuni casi, vogliamo mostrare i nomi dei maschi e delle femmine accanto al nome dell'animale. A volte no, e in tali casi vogliamo solo che il nome dell'animale si estenda su tutta la tabella.

Il markup iniziale appare così:

```html
<table>
  <tr>
    <th>Animals</th>
  </tr>
  <tr>
    <th>Hippopotamus</th>
  </tr>
  <tr>
    <th>Horse</th>
    <td>Mare</td>
  </tr>
  <tr>
    <td>Stallion</td>
  </tr>
  <tr>
    <th>Crocodile</th>
  </tr>
  <tr>
    <th>Chicken</th>
    <td>Hen</td>
  </tr>
  <tr>
    <td>Rooster</td>
  </tr>
</table>
```

```css hidden
table {
  border-collapse: collapse;
}
td,
th {
  border: 1px solid black;
  padding: 10px 20px;
}
```

Ma l'output non ci dà esattamente quello che vogliamo:

{{EmbedLiveSample("Allowing_cells_to_span_multiple_rows_and_columns", "", "350")}}

Abbiamo bisogno di un modo per far sì che "Animals", "Hippopotamus", e "Crocodile" si estendano su due colonne, e che "Horse" e "Chicken" si estendano verso il basso su due righe. Fortunatamente, le intestazioni delle tabelle e le celle hanno gli attributi `colspan` e `rowspan`, che ci permettono di fare proprio queste cose. Entrambi accettano un valore numerico senza unità, che equivale al numero di righe o colonne che vuoi siano estese. Ad esempio, `colspan="2"` fa sì che una cella si estenda su due colonne.

Utilizziamo `colspan` e `rowspan` per migliorare questa tabella.

1. Prima, fai una copia locale dei nostri file [animals-table.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/animals-table.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sul tuo computer locale. L'HTML contiene lo stesso esempio di animali che hai visto sopra.
2. Successivamente, usa `colspan` per far sì che "Animals", "Hippopotamus", e "Crocodile" si estendano su due colonne.
3. Infine, usa `rowspan` per far sì che "Horse" e "Chicken" si estendano su due righe.
4. Salva e apri il tuo codice in un browser per vedere il miglioramento.

> [!NOTE]
> Puoi trovare il nostro esempio finito su [animals-table-fixed.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/animals-table-fixed.html) su GitHub ([vedi anche direttamente](https://mdn.github.io/learning-area/html/tables/basic/animals-table-fixed.html)).

## Sommario

Questo conclude le nozioni di base sulle tabelle HTML. Nel prossimo articolo, esamineremo alcune caratteristiche aggiuntive che possono essere utilizzate per rendere le tabelle HTML più accessibili alle persone ipovedenti.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content")}}
