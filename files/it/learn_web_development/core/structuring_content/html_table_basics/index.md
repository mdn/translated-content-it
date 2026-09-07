---
title: Nozioni di base sulle tabelle HTML
short-title: Nozioni di base sulle tabelle
slug: Learn_web_development/Core/Structuring_content/HTML_table_basics
l10n:
  sourceCommit: ce12c10364f35c64184dec44be85537b7e10d91f
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Splash_page", "Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content")}}

Questo articolo introduce alle tabelle HTML, trattando le basi essenziali come righe, celle, intestazioni, la possibilità di estendere le celle su più colonne e righe e come raggruppare tutte le celle di una colonna a fini di styling.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>A cosa servono le tabelle: strutturare dati tabulari.</li>
          <li>A cosa non servono le tabelle: layout o <em>qualsiasi altra cosa</em>.</li>
          <li>Sintassi di base delle tabelle: <code>&lt;table&gt;</code>, <code>&lt;tr&gt;</code> e <code>&lt;td&gt;</code>.</li>
          <li>Definire intestazioni di tabella con <code>&lt;th&gt;</code>.</li>
          <li>Estendere celle su più colonne e righe con <code>colspan</code> e <code>rowspan</code>.</li>
          <li>Raggruppare colonne con <code>&lt;colgroup&gt;</code> e <code>&lt;col&gt;</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è una tabella?

Una tabella è un insieme strutturato di dati composto da righe e colonne (**dati tabulari**). Una tabella consente di individuare rapidamente e facilmente valori che indicano un qualche tipo di relazione tra diversi tipi di dati, ad esempio una persona e la sua età, un giorno della settimana o l'orario di una piscina locale.

![Una tabella di esempio che mostra nomi ed età di alcune persone: Chris 38, Dennis 45, Sarah 29, Karen 47.](numbers-table.png)

![Un orario di una piscina che mostra una tabella di dati di esempio](swimming-timetable.png)

Le tabelle sono usate molto comunemente nella società umana e lo sono da molto tempo, come dimostra questo documento del censimento statunitense del 1800:

![Un documento molto antico su pergamena; i dati non sono facilmente leggibili, ma mostra chiaramente l'uso di una tabella di dati.](1800-census.jpg)

Non sorprende quindi che i creatori di HTML abbiano fornito un mezzo per strutturare e presentare dati tabulari sul web.

### Come funziona una tabella?

Il punto fondamentale di una tabella è la sua rigidità. Le informazioni vengono interpretate facilmente creando associazioni visive tra intestazioni di riga e di colonna. Per esempio, osservare la tabella seguente e trovare un gigante gassoso gioviano con 62 lune. La risposta può essere trovata associando le intestazioni di riga e colonna pertinenti.

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

Se implementate correttamente, le tabelle HTML sono gestite bene dagli strumenti di accessibilità come gli screen reader, quindi una tabella HTML ben realizzata dovrebbe migliorare l'esperienza sia degli utenti vedenti sia di quelli con disabilità visive.

### Styling delle tabelle

È anche possibile [osservare l'esempio live dei dati sui pianeti](https://mdn.github.io/learning-area/html/tables/planets-data/) su GitHub. Si noterà che la tabella è un po' più leggibile: questo avviene perché la tabella mostrata qui sopra in questa pagina ha uno styling minimo, mentre alla versione su GitHub è applicato CSS più significativo.

Non bisogna farsi illusioni: affinché le tabelle siano efficaci sul web, è necessario fornire alcune informazioni di styling con [CSS](/it/docs/Learn_web_development/Core/Styling_basics), oltre a una struttura HTML buona e solida. In questa lezione l'attenzione è rivolta alla parte HTML; lo styling delle tabelle verrà trattato in seguito, nella lezione [Styling delle tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables).

In questo modulo non ci concentreremo sul CSS, ma viene fornito un foglio di stile CSS minimo che renderà le tabelle più leggibili rispetto all'aspetto predefinito ottenuto senza styling. Il [foglio di stile è disponibile qui](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css), ed è disponibile anche un [template HTML](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) che applica il foglio di stile: insieme costituiscono un buon punto di partenza per sperimentare con le tabelle HTML.

### Quando evitare le tabelle HTML?

Le tabelle HTML dovrebbero essere usate per i dati tabulari, ovvero informazioni facili da organizzare in righe e colonne: è per questo che sono state progettate. Purtroppo, molte persone usavano le tabelle HTML per realizzare il layout delle pagine web, ad esempio una riga per contenere l'intestazione della pagina, una riga per contenere ciascuna colonna di contenuto, una riga per contenere il piè di pagina e così via. Questa tecnica veniva usata in passato perché il supporto CSS tra i browser era molto più limitato. I browser moderni offrono un solido supporto CSS, quindi i layout basati su tabelle non sono più necessari. I layout con tabelle sono oggi estremamente rari, ma potrebbero ancora comparire in alcuni angoli del web.

In breve, usare le tabelle per il layout anziché le [tecniche di layout CSS](/it/docs/Learn_web_development/Core/CSS_layout) è una cattiva idea. I motivi principali sono i seguenti:

1. **Le tabelle di layout riducono l'accessibilità per gli utenti con disabilità visive**: gli [screen reader](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers), usati dalle persone non vedenti, interpretano i tag presenti in una pagina HTML e leggono il contenuto all'utente. Poiché le tabelle non sono lo strumento corretto per il layout e il markup è più complesso rispetto alle tecniche di layout CSS, l'output degli screen reader risulterà confuso per gli utenti.
2. **Le tabelle producono una zuppa di tag**: come già menzionato, i layout con tabelle coinvolgono generalmente strutture di markup più complesse rispetto alle tecniche di layout appropriate. Questo può rendere il codice più difficile da scrivere, mantenere e sottoporre a debug.
3. **Le tabelle non sono automaticamente responsive**: quando vengono usati contenitori di layout appropriati, come {{htmlelement("header")}}, {{htmlelement("section")}}, {{htmlelement("article")}} o {{htmlelement("div")}}, la loro larghezza predefinita è pari al 100% dell'elemento padre. Le tabelle, invece, vengono dimensionate in base al loro contenuto per impostazione predefinita, quindi sono necessarie misure aggiuntive affinché lo styling del layout basato su tabelle funzioni efficacemente su vari dispositivi.

## Creare la prima tabella

Abbiamo parlato abbastanza della teoria delle tabelle, quindi passiamo a un esempio pratico e costruiamo una semplice tabella.

1. Prima di tutto, creare una copia di [blank-template.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sul computer locale. Il template HTML contiene già un elemento `<link>` per applicare il CSS all'HTML, quindi non è necessario preoccuparsene.
2. Il contenuto di ogni tabella è racchiuso da questi due tag: **[`<table></table>`](/it/docs/Web/HTML/Reference/Elements/table)**. Aggiungerli all'interno del `body` dell'HTML.
3. Il contenitore più piccolo all'interno di una tabella è una cella, creata con un elemento **[`<td>`](/it/docs/Web/HTML/Reference/Elements/td)** ("td" significa "table data"). Aggiungere quanto segue all'interno dei tag della tabella:

   ```html
   <td>Hi, I'm your first cell.</td>
   ```

4. Per ottenere una riga di quattro celle, è necessario copiare questi tag altre tre volte. Aggiornare il contenuto della tabella affinché risulti così:

   ```html
   <td>Hi, I'm your first cell.</td>
   <td>I'm your second cell.</td>
   <td>I'm your third cell.</td>
   <td>I'm your fourth cell.</td>
   ```

Come si può vedere, le celle non vengono posizionate una sotto l'altra, bensì vengono automaticamente allineate sulla stessa riga. Ogni elemento `<td>` crea una singola cella e insieme costituiscono la prima riga. Ogni cella aggiunta allunga la riga.

Per impedire che questa riga continui ad allungarsi e iniziare a posizionare le celle successive su una seconda riga, è necessario usare l'elemento [`<tr>`](/it/docs/Web/HTML/Reference/Elements/tr) ("tr" significa "table row"). Vediamolo ora.

1. Inserire le quattro celle già create all'interno dei tag `<tr>`, in questo modo:

   ```html
   <tr>
     <td>Hi, I'm your first cell.</td>
     <td>I'm your second cell.</td>
     <td>I'm your third cell.</td>
     <td>I'm your fourth cell.</td>
   </tr>
   ```

2. Ora è stata creata una riga; provare a crearne una o due in più. Ogni riga deve essere racchiusa in un ulteriore elemento `<tr>`, con ciascuna cella contenuta in un `<td>`.

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html
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

</details>

## Aggiungere intestazioni con gli elementi \<th>

Ora concentriamoci sulle intestazioni della tabella: celle speciali poste all'inizio di una riga o colonna che definiscono il tipo di dati contenuti in quella riga o colonna (per esempio, vedere le celle "Person" e "Age" nel primo esempio mostrato in questo articolo). Per illustrare perché sono utili, osservare il seguente esempio di tabella. Prima il codice sorgente:

```html live-sample___table-headers
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

```css hidden live-sample___table-headers
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

{{EmbedLiveSample("table-headers", "", "250")}}

Il problema è che, sebbene sia possibile capire più o meno cosa sta succedendo, non è così facile confrontare i dati come potrebbe essere. Se le intestazioni di colonna e riga risaltassero in qualche modo, sarebbe molto meglio.

### Aggiungere intestazioni alla tabella dei cani

Ora provare a migliorare l'esempio della tabella dei cani aggiungendo alcune intestazioni.

1. Per prima cosa, creare un'altra copia dei file [blank-template.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sul computer locale.
2. Aggiungere il seguente codice all'interno del `<body>` dell'HTML:

   ```html
   <h1>Dogs Table</h1>
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

3. Per riconoscere le intestazioni della tabella come tali, sia visivamente sia semanticamente, è possibile usare l'elemento [`<th>`](/it/docs/Web/HTML/Reference/Elements/th) ("th" significa "table header"). Funziona esattamente come un `<td>`, tranne per il fatto che indica un'intestazione anziché una cella normale. Nell'HTML, cambiare tutti gli elementi `<td>` che circondano le intestazioni della tabella in elementi `<th>`.
4. Salvare l'HTML e caricarlo in un browser: le intestazioni dovrebbero ora avere l'aspetto di intestazioni.

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html
<table>
  <tr>
    <td>&nbsp;</td>
    <th>Knocky</th>
    <th>Flor</th>
    <th>Ella</th>
    <th>Juan</th>
  </tr>
  <tr>
    <th>Breed</th>
    <td>Jack Russell</td>
    <td>Poodle</td>
    <td>Streetdog</td>
    <td>Cocker Spaniel</td>
  </tr>
  <tr>
    <th>Age</th>
    <td>16</td>
    <td>9</td>
    <td>10</td>
    <td>5</td>
  </tr>
  <tr>
    <th>Owner</th>
    <td>Mother-in-law</td>
    <td>Me</td>
    <td>Me</td>
    <td>Sister-in-law</td>
  </tr>
  <tr>
    <th>Eating Habits</th>
    <td>Eats everyone's leftovers</td>
    <td>Nibbles at food</td>
    <td>Hearty eater</td>
    <td>Will eat till he explodes</td>
  </tr>
</table>
```

</details>

### Perché le intestazioni sono utili?

A questa domanda è già stata data una risposta parziale: è più facile trovare i dati cercati quando le intestazioni risaltano chiaramente e, in generale, il design appare migliore.

> [!NOTE]
> Le intestazioni di tabella dispongono di uno styling predefinito: sono in grassetto e centrate anche senza aggiungere styling alla tabella, per aiutarle a risaltare.

Le intestazioni delle tabelle offrono anche un ulteriore vantaggio: insieme all'attributo `scope` (che verrà trattato nel prossimo articolo), consentono di rendere le tabelle più accessibili associando ogni intestazione a tutti i dati presenti nella stessa riga o colonna. Gli screen reader possono quindi leggere un'intera riga o colonna di dati in una sola volta, il che è molto utile.

## Consentire alle celle di estendersi su più righe e colonne

A volte è necessario che le celle si estendano su più righe o colonne. Si consideri il seguente semplice esempio, che mostra i nomi di animali comuni. In alcuni casi, si vogliono mostrare i nomi dei maschi e delle femmine accanto al nome dell'animale. A volte non è necessario e, in questi casi, si vuole semplicemente che il nome dell'animale occupi l'intera tabella.

Il markup iniziale è simile a questo:

```html live-sample___multiple-rows-columns
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

```css hidden live-sample___multiple-rows-columns
table {
  border-collapse: collapse;
}
td,
th {
  border: 1px solid black;
  padding: 10px 20px;
}
```

Tuttavia, l'output non produce esattamente ciò che si desidera:

{{EmbedLiveSample("multiple-rows-columns", "", "350")}}

### Correggere il layout con `rowspan` e `colspan`

Serve un modo per fare in modo che "Animals", "Hippopotamus" e "Crocodile" si estendano su due colonne, mentre "Horse" e "Chicken" si estendano verticalmente su due righe. Fortunatamente, le intestazioni e le celle della tabella dispongono degli attributi `colspan` e `rowspan`, che consentono di fare proprio questo. Entrambi accettano un valore numerico senza unità, equivalente al numero di righe o colonne che si desidera coprire. Ad esempio, `colspan="2"` fa sì che una cella si estenda su due colonne.

Usiamo `colspan` e `rowspan` per migliorare questa tabella.

1. Creare un'altra copia locale dei file [blank-template.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/blank-template.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/basic/minimal-table.css) in una nuova directory sul computer locale.
2. Aggiungere quanto segue al `<body>` dell'HTML:

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

3. Quindi, usare `colspan` per fare in modo che "Animals", "Hippopotamus" e "Crocodile" si estendano su due colonne.
4. Infine, usare `rowspan` per fare in modo che "Horse" e "Chicken" si estendano su due righe.
5. Salvare e aprire il codice in un browser per osservare il miglioramento.

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html
<table>
  <tr>
    <th colspan="2">Animals</th>
  </tr>
  <tr>
    <th colspan="2">Hippopotamus</th>
  </tr>
  <tr>
    <th rowspan="2">Horse</th>
    <td>Mare</td>
  </tr>
  <tr>
    <td>Stallion</td>
  </tr>
  <tr>
    <th colspan="2">Crocodile</th>
  </tr>
  <tr>
    <th rowspan="2">Chicken</th>
    <td>Hen</td>
  </tr>
  <tr>
    <td>Rooster</td>
  </tr>
</table>
```

</details>

## Raggruppare colonne con `<colgroup>` e `<col>`

Esiste un modo per selezionare intere colonne della tabella come una singola entità, ad esempio quando si applicano stili a una tabella, argomento che verrà trattato più avanti nella [lezione Styling delle tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables). Acquisendo maggiore esperienza nella creazione di tabelle HTML, si scoprirà che applicare, per esempio, un colore di sfondo a ogni cella di una singola colonna è più difficile di quanto si possa pensare. Gli elementi {{htmlelement("colgroup")}} e {{htmlelement("col")}} forniscono una soluzione a questo problema.

L'elemento `<colgroup>` deve essere incluso come figlio della tabella, subito dopo l'elemento `<table>` di apertura. All'interno dell'elemento `<colgroup>` è possibile includere uno o più elementi `<col>`, che rappresentano gruppi di colonne. L'elemento `<col>` può includere un attributo `span` che indica il numero di colonne nel gruppo. Può anche includere attributi globali come `style`, se si desidera selezionare il gruppo con stili inline, oppure `class`, se si desidera selezionare quel gruppo con CSS o JavaScript usando un nome di classe. Gli elementi `<col>` rappresentano le colonne della tabella a partire dall'inizio delle colonne, ad esempio dal lato sinistro di una tabella scritta in una lingua da sinistra a destra come l'inglese.

Osserviamo un esempio per chiarire il concetto. La tabella seguente mostra un orario scolastico:

```html live-sample___colgroup-col
<h1>School language timetable</h1>

<table>
  <colgroup>
    <col span="2" />
    <col class="column-background" />
    <col class="column-fixed-width" />
    <col class="column-background" />
    <col class="column-background-border" />
    <col span="2" class="column-fixed-width" />
  </colgroup>
  <tr>
    <td>&nbsp;</td>
    <th>Mon</th>
    <th>Tues</th>
    <th>Wed</th>
    <th>Thurs</th>
    <th>Fri</th>
    <th>Sat</th>
    <th>Sun</th>
  </tr>
  <tr>
    <th>1st period</th>
    <td>English</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
    <td>German</td>
    <td>Dutch</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <th>2nd period</th>
    <td>English</td>
    <td>English</td>
    <td>&nbsp;</td>
    <td>German</td>
    <td>Dutch</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <th>3rd period</th>
    <td>&nbsp;</td>
    <td>German</td>
    <td>&nbsp;</td>
    <td>German</td>
    <td>Dutch</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <th>4th period</th>
    <td>&nbsp;</td>
    <td>English</td>
    <td>&nbsp;</td>
    <td>English</td>
    <td>Dutch</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
</table>
```

In questa tabella ci sono otto colonne. Osserviamo più da vicino la struttura `<colgroup>` e `<col>` per mostrare in che modo le influenza:

```html
<colgroup>
  <col span="2" />
  <col class="column-background" />
  <col class="column-fixed-width" />
  <col class="column-background" />
  <col class="column-background-border" />
  <col span="2" class="column-fixed-width" />
</colgroup>
```

Osservando gli elementi `<col>`:

- Il primo ha `span="2"` impostato, quindi rappresenta la prima _e_ la seconda colonna dal lato sinistro della tabella. Queste colonne non vengono selezionate con alcuno stile, ma devono essere incluse per poter selezionare le colonne successive.
- Il secondo e il quarto non hanno un attributo `span` impostato, quindi rappresentano una singola colonna: in questi casi, rispettivamente la terza e la quinta colonna. A essi viene applicata la `class` `column-background`.
- Il terzo non ha un attributo `span` impostato e ha applicata la `class` `column-fixed-width`. Rappresenta la quarta colonna.
- Il quinto non ha un attributo `span` impostato e ha applicata la `class` `column-background-border`. Rappresenta la sesta colonna.
- Il sesto ha `span="2"` impostato e ha applicata la `class` `column-fixed-width`. Rappresenta la settima e l'ottava colonna.

La maggior parte del CSS per questo esempio è nascosta, ma vengono mostrate le regole che applicano stili agli elementi `<col>` con le classi `column-background`, `column-fixed-width` e `column-background-border` impostate:

```css hidden live-sample___colgroup-col
html {
  font-family: sans-serif;
}

body {
  margin: 0 20px;
}

table {
  border-collapse: collapse;
  border: 2px solid rgb(200 200 200);
  letter-spacing: 1px;
  font-size: 0.8rem;
}

td,
th {
  border: 1px solid rgb(190 190 190);
  padding: 10px 20px;
}

td {
  text-align: center;
}
```

```css live-sample___colgroup-col
.column-background {
  background-color: #97db9a;
}

.column-fixed-width {
  width: 40px;
}

.column-background-border {
  background-color: #dcc48e;
  border: 4px solid #c1437a;
}
```

- Gli elementi `<col>` con una classe `column-background` hanno un colore di sfondo uniforme impostato.
- Gli elementi `<col>` con una classe `column-fixed-width` hanno una larghezza fissa ridotta impostata.
- L'elemento `<col>` con una classe `column-background-border` ha un colore di sfondo uniforme e un bordo spesso impostati.

Per ora non è necessario preoccuparsi di come funziona il CSS; verrà illustrato in dettaglio più avanti nel modulo [Nozioni di base sullo styling CSS](/it/docs/Learn_web_development/Core/Styling_basics).

Vediamo come viene renderizzato il codice precedente:

{{embedlivesample("colgroup-col", "100%", 400)}}

Notare come le diverse colonne ricevano gli stili specificati nelle classi.

> [!NOTE]
> Sebbene `<colgroup>` e `<col>` facilitino principalmente lo styling, sono una funzionalità HTML, quindi sono stati trattati qui anziché nei moduli CSS. È inoltre corretto dire che sono una funzionalità _limitata_: come mostrato nella [pagina di riferimento di `<colgroup>`](/it/docs/Web/HTML/Reference/Elements/colgroup#usage_notes), solo un sottoinsieme limitato di stili può essere applicato a un elemento `<col>` e la maggior parte delle altre impostazioni storicamente disponibili è stata deprecata, ovvero rimossa o contrassegnata per la rimozione.

## Riepilogo interattivo dei concetti sulle tabelle

Il seguente contenuto incorporato di Scrimba<sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> propone una lezione interattiva che riassume la maggior parte delle tecniche trattate in questo articolo. Consultarlo per un riepilogo dei punti chiave e ulteriore pratica.

<mdn-scrim-inline url="https://scrimba.com/frontend-path-c0j/~03s" scrimtitle="HTML tables"></mdn-scrim-inline>

## Riepilogo

Questo conclude le nozioni di base sulle tabelle HTML. Nel prossimo articolo verranno esaminate alcune ulteriori funzionalità utilizzabili per rendere le tabelle HTML più accessibili alle persone con disabilità visive.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Splash_page", "Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content")}}
