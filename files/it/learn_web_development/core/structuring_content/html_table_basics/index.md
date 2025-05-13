---
title: Nozioni di base sulle tabelle HTML
short-title: Nozioni di base sulle tabelle
slug: Learn_web_development/Core/Structuring_content/HTML_table_basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content")}}

Questo articolo ti introduce alle tabelle HTML, coprendo le basi come righe, celle, intestazioni, come far estendere le celle su più colonne e righe, e come raggruppare tutte le celle in una colonna ai fini della formattazione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come coperto in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>A cosa servono le tabelle: strutturare i dati in forma tabellare.</li>
          <li>A cosa non servono le tabelle: layout o <em>qualsiasi altra cosa</em>.</li>
          <li>Sintassi di base delle tabelle: <code>&lt;table&gt;</code>, <code>&lt;tr&gt;</code> e <code>&lt;td&gt;</code>.</li>
          <li>Definire le intestazioni delle tabelle con <code>&lt;th&gt;</code>.</li>
          <li>Estendere più colonne e righe con <code>colspan</code> e <code>rowspan</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è una tabella?

Una tabella è un insieme strutturato di dati composto da righe e colonne (**dati tabellari**). Una tabella consente di cercare rapidamente e facilmente valori che indicano qualche tipo di connessione tra diversi tipi di dati, ad esempio una persona e la sua età, un giorno della settimana, o l'orario di una piscina locale.

![Un esempio di tabella che mostra nomi ed età di alcune persone - Chris 38, Dennis 45, Sarah 29, Karen 47.](numbers-table.png)

![L'orario di una piscina che mostra una tabella di dati di esempio](swimming-timetable.png)

Le tabelle sono molto comunemente utilizzate nella società umana, e lo sono da molto tempo, come dimostrato da questo documento del censimento statunitense del 1800:

![Un vecchissimo documento in pergamena; i dati non sono facilmente leggibili, ma mostra chiaramente l'uso di una tabella di dati.](1800-census.jpg)

Non sorprende quindi che i creatori di HTML abbiano fornito un mezzo per strutturare e presentare dati tabellari sul web.

### Come funziona una tabella?

Il punto di una tabella è che è rigida. Le informazioni sono facilmente interpretate facendo associazioni visive tra le intestazioni delle righe e delle colonne. Ad esempio, guarda la tabella qui sotto e trova un gigante gassoso gioviano con 62 lune. È possibile trovare la risposta associando le intestazioni di riga e colonna pertinenti.

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

Quando implementate correttamente, le tabelle HTML sono gestite bene da strumenti di accessibilità come i lettori di schermo, quindi una tabella HTML ben fatta dovrebbe migliorare l'esperienza sia per gli utenti vedenti che per quelli non vedenti.

### Formattazione delle tabelle

Può anche dare un'occhiata all'[esempio dal vivo](https://mdn.github.io/learning-area/html/tables/assessment-finished/planets-data.html) su GitHub! Una cosa che noterà è che la tabella lì appare un po' più leggibile — questo perché la tabella che vede sopra su questa pagina ha uno stile minimo, mentre la versione su GitHub ha un CSS più significativo applicato.

Non si illuda; per rendere le tabelle efficaci sul web, è necessario fornire un po' di informazioni di stile con [CSS](/it/docs/Learn_web_development/Core/Styling_basics), oltre a una buona solida struttura con HTML. In questa lezione ci concentriamo sulla parte HTML; verrà a conoscenza della formattazione delle tabelle più avanti, nella nostra lezione [Formattazione delle tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables).

Non ci concentreremo sul CSS in questo modulo, ma abbiamo fornito un foglio di stile CSS minimo che potrà utilizzare per rendere le sue tabelle più leggibili rispetto al predefinito che ottiene senza alcuna formattazione. Può trovare il [foglio di stile qui](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css), e può anche trovare un [modello HTML](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) che applica il foglio di stile — insieme forniranno un buon punto di partenza per sperimentare con le tabelle HTML.

### Quando evitare le tabelle HTML?

Le tabelle HTML devono essere utilizzate per i dati tabellari (informazioni che sono facili da gestire in righe e colonne) — è a questo che sono destinate. Purtroppo, molte persone hanno usato le tabelle HTML per strutturare pagine web, ad esempio una riga per contenere l'intestazione della pagina, una riga per contenere ogni colonna di contenuto, una riga per contenere il piè di pagina, ecc. Questa tecnica è stata utilizzata perché il supporto CSS tra browser era molto più limitato. I browser moderni hanno un solido supporto CSS, quindi i layout basati su tabelle sono ora estremamente rari, ma potrebbe ancora vederli in alcuni angoli del web.

In breve, utilizzare le tabelle per il layout anziché [le tecniche di layout CSS](/it/docs/Learn_web_development/Core/CSS_layout) è una cattiva idea. Le principali ragioni sono le seguenti:

1. **I layout delle tabelle riducono l'accessibilità per gli utenti con disabilità visive**: i [lettori di schermo](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers), utilizzati dai non vedenti, interpretano i tag presenti in una pagina HTML e leggono i contenuti all'utente. Poiché le tabelle non sono lo strumento giusto per il layout, e il markup è più complesso rispetto alle tecniche di layout CSS, l'output dei lettori di schermo risulterà confuso per gli utenti.
2. **Le tabelle producono "tag soup"**: Come detto in precedenza, i layout delle tabelle generalmente coinvolgono strutture di markup più complesse rispetto alle tecniche di layout corrette. Questo può rendere il codice più difficile da scrivere, mantenere e debugare.
3. **Le tabelle non sono automaticamente responsive**: Quando si utilizzano corretti contenitori di layout (come {{htmlelement("header")}}, {{htmlelement("section")}}, {{htmlelement("article")}}, o {{htmlelement("div")}}), la loro larghezza si adatta automaticamente al 100% dell'elemento padre. Le tabelle, d'altro canto, sono dimensionate in base al loro contenuto per impostazione predefinita, quindi sono necessarie misure aggiuntive per ottenere uno stile di layout della tabella efficace su una varietà di dispositivi.

## Apprendimento attivo: creare la sua prima tabella

Abbiamo parlato abbastanza della teoria delle tabelle, quindi, immergiamoci in un esempio pratico e costruiamo una semplice tabella.

1. Innanzitutto, faccia una copia locale di [blank-template.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sulla sua macchina locale.
2. Il contenuto di ogni tabella è racchiuso da questi due tag: **[`<table></table>`](/it/docs/Web/HTML/Reference/Elements/table)**. Aggiunga questi all'interno del corpo del suo HTML.
3. Il contenitore più piccolo all'interno di una tabella è una cella di tabella, che è creata da un elemento **[`<td>`](/it/docs/Web/HTML/Reference/Elements/td)** ('td' sta per 'table data'). Aggiunga quanto segue all'interno dei tag della sua tabella:

   ```html
   <td>Hi, I'm your first cell.</td>
   ```

4. Se vogliamo una riga di quattro celle, dobbiamo copiare questi tag tre volte. Aggiorni il contenuto della sua tabella per apparire così:

   ```html
   <td>Hi, I'm your first cell.</td>
   <td>I'm your second cell.</td>
   <td>I'm your third cell.</td>
   <td>I'm your fourth cell.</td>
   ```

Come vedrà, le celle non sono posizionate l'una sotto l'altra, piuttosto sono automaticamente allineate insieme sulla stessa riga. Ogni elemento `<td>` crea una singola cella e insieme formano la prima riga. Ogni cella che aggiungiamo fa crescere di lunghezza la riga.

Per fermare la crescita di questa riga e iniziare a posizionare le celle successive su una seconda riga, dobbiamo usare l'elemento **[`<tr>`](/it/docs/Web/HTML/Reference/Elements/tr)** ('tr' sta per 'table row'). Esaminiamo ora questo.

1. Inserisca le quattro celle già create all'interno dei tag `<tr>`, come segue:

   ```html
   <tr>
     <td>Hi, I'm your first cell.</td>
     <td>I'm your second cell.</td>
     <td>I'm your third cell.</td>
     <td>I'm your fourth cell.</td>
   </tr>
   ```

2. Ora che ha creato una riga, provi a crearne una o due in più — ogni riga deve essere incapsulata in un ulteriore elemento `<tr>`, con ciascuna cella contenuta in un `<td>`.

### Risultato

Questo dovrebbe portare a una tabella che appare qualcosa di simile al seguente:

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
> Può anche trovare questo su GitHub come [simple-table.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/simple-table.html) ([vedi anche la versione dal vivo](https://mdn.github.io/learning-area/html/tables/basic/simple-table.html)).

## Aggiungere intestazioni con elementi \<th>

Ora concentriamoci sulle intestazioni delle tabelle — celle speciali che vanno all'inizio di una riga o di una colonna e definiscono il tipo di dati che quella riga o colonna contiene (come esempio, vedere le celle "Persona" e "Età" nel primo esempio mostrato in questo articolo). Per illustrare perché sono utili, dia un'occhiata al seguente esempio di tabella. Prima il codice sorgente:

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

Ora la tabella effettivamente visualizzata:

{{EmbedLiveSample("Adding_headers_with_th_elements", "", "250")}}

Il problema qui è che, mentre si può in qualche modo capire cosa sta succedendo, non è così facile incrociare i dati come potrebbe essere. Se le intestazioni di colonna e riga si distinguessero in qualche modo, sarebbe di gran lunga migliore.

### Apprendimento attivo: intestazioni di tabella

Proviamo a migliorare questa tabella.

1. Innanzitutto, faccia una copia locale del nostro file [dogs-table.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/dogs-table.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sulla sua macchina locale. L'HTML contiene lo stesso esempio sui Cani che ha visto sopra.
2. Per riconoscere le intestazioni della tabella come intestazioni, sia visivamente che semanticamente, lei può usare l'elemento **[`<th>`](/it/docs/Web/HTML/Reference/Elements/th)** ('th' sta per 'table header'). Questo funziona esattamente come un `<td>`, tranne che denota un'intestazione, non una cella normale. Vada nel suo HTML, e cambi tutti gli elementi `<td>` che circondano le intestazioni delle tabelle in elementi `<th>`.
3. Salvi il suo HTML e lo carichi in un browser, e dovrebbe vedere che le intestazioni ora appaiono come intestazioni.

> [!NOTE]
> Può trovare il nostro esempio finito su [dogs-table-fixed.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/dogs-table-fixed.html) su GitHub ([vedi anche la versione dal vivo](https://mdn.github.io/learning-area/html/tables/basic/dogs-table-fixed.html)).

### Perché le intestazioni sono utili?

Abbiamo già parzialmente risposto a questa domanda — è più facile trovare i dati che si sta cercando quando le intestazioni si distinguono chiaramente, e il design generalmente appare migliore.

> [!NOTE]
> Le intestazioni di tabella vengono con uno stile predefinito — sono in grassetto e centrate anche se non aggiunge il proprio stile alla tabella, per aiutarle a risaltare.

Le intestazioni delle tabelle hanno anche un ulteriore vantaggio — insieme all'attributo `scope` (che apprenderemo nel prossimo articolo), consentono di rendere le tabelle più accessibili associando ciascuna intestazione a tutti i dati nella stessa riga o colonna. I lettori di schermo sono quindi in grado di leggere un'intera riga o colonna di dati in una volta, il che è piuttosto utile.

## Permettere alle celle di estendersi su più righe e colonne

A volte vogliamo che le celle si estendano su più righe o colonne. Consideri il seguente semplice esempio, che mostra i nomi di animali comuni. In alcuni casi, vogliamo mostrare i nomi dei maschi e delle femmine accanto al nome dell'animale. Qualche volta non lo facciamo, e in tali casi vogliamo semplicemente che il nome dell'animale si estenda sull'intera tabella.

Il markup iniziale sembra così:

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

Ma l'output non ci dà esattamente ciò che vogliamo:

{{EmbedLiveSample("Allowing_cells_to_span_multiple_rows_and_columns", "", "350")}}

Abbiamo bisogno di un modo per far sì che "Animals", "Hippopotamus", e "Crocodile" si estendano su due colonne, e "Horse" e "Chicken" si estendano verso il basso su due righe. Fortunatamente, le intestazioni e le celle delle tabelle hanno gli attributi `colspan` e `rowspan`, che ci permettono di fare proprio queste cose. Entrambi accettano un valore numerico senza unità, che equivale al numero di righe o di colonne che si desidera siano estese. Ad esempio, `colspan="2"` fa estendere una cella su due colonne.

Usiamo `colspan` e `rowspan` per migliorare questa tabella.

1. Innanzitutto, faccia una copia locale del nostro file [animals-table.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/animals-table.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sulla sua macchina locale. L'HTML contiene lo stesso esempio sugli animali che ha visto sopra.
2. Successivamente, usi `colspan` per far sì che "Animals", "Hippopotamus", e "Crocodile" si estendano su due colonne.
3. Infine, usi `rowspan` per far sì che "Horse" e "Chicken" si estendano su due righe.
4. Salvi e apra il suo codice in un browser per vedere il miglioramento.

> [!NOTE]
> Può trovare il nostro esempio finito su [animals-table-fixed.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/animals-table-fixed.html) su GitHub ([vedi anche la versione dal vivo](https://mdn.github.io/learning-area/html/tables/basic/animals-table-fixed.html)).

## Sommario

Questo conclude le basi delle tabelle HTML. Nel prossimo articolo, esamineremo alcune funzionalità ulteriori che possono essere utilizzate per rendere le tabelle HTML più accessibili alle persone con disabilità visive.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content")}}
