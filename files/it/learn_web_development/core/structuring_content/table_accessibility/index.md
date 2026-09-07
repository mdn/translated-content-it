---
title: Accessibilità delle tabelle HTML
short-title: Accessibilità delle tabelle
slug: Learn_web_development/Core/Structuring_content/Table_accessibility
l10n:
  sourceCommit: 754b68246f4e69e404309fee4a1699e047e43994
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content")}}

Nell'articolo precedente, è stata esaminata una delle funzionalità più importanti per rendere accessibili le tabelle HTML agli utenti con disabilità visive: l'elemento {{htmlelement("th")}}. In questo articolo, si prosegue su questa strada, esaminando altre funzionalità di accessibilità delle tabelle HTML, come didascalie/riepiloghi, il raggruppamento delle righe in sezioni di intestazione, corpo e piè di pagina della tabella e l'ambito di colonne e righe.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Le basi di HTML (vedere
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Comprensione dei problemi di accessibilità associati alle tabelle.</li>
          <li>Aggiungere didascalie alle tabelle.</li>
          <li>Una migliore strutturazione delle tabelle con intestazione, corpo e piè di pagina.</li>
          <li>Creare ulteriori associazioni tra intestazioni e celle con gli attributi <code>scope</code>, <code>id</code> e <code>headers</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo: tabelle per utenti con disabilità visive

Ricapitoliamo brevemente come si usano le tabelle di dati. Una tabella può essere uno strumento pratico per fornire accesso rapido ai dati e consentire di cercare valori diversi. Ad esempio, basta una rapida occhiata alla tabella seguente per scoprire quanti anelli sono stati venduti a Gand durante agosto 2016. Per comprenderne le informazioni, vengono create associazioni visive tra i dati nella tabella e le relative intestazioni di colonna e/o riga.

<table>
  <caption>Articoli venduti agosto 2016</caption>
  <thead>
    <tr>
      <td colspan="2" rowspan="2"></td>
      <th colspan="3" scope="colgroup">Abbigliamento</th>
      <th colspan="2" scope="colgroup">Accessori</th>
    </tr>
    <tr>
      <th scope="col">Pantaloni</th>
      <th scope="col">Gonne</th>
      <th scope="col">Vestiti</th>
      <th scope="col">Braccialetti</th>
      <th scope="col">Anelli</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" scope="rowgroup">Belgio</th>
      <th scope="row">Anversa</th>
      <td>56</td>
      <td>22</td>
      <td>43</td>
      <td>72</td>
      <td>23</td>
    </tr>
    <tr>
      <th scope="row">Gand</th>
      <td>46</td>
      <td>18</td>
      <td>50</td>
      <td>61</td>
      <td>15</td>
    </tr>
    <tr>
      <th scope="row">Bruxelles</th>
      <td>51</td>
      <td>27</td>
      <td>38</td>
      <td>69</td>
      <td>28</td>
    </tr>
    <tr>
      <th rowspan="2" scope="rowgroup">Paesi Bassi</th>
      <th scope="row">Amsterdam</th>
      <td>89</td>
      <td>34</td>
      <td>69</td>
      <td>85</td>
      <td>38</td>
    </tr>
    <tr>
      <th scope="row">Utrecht</th>
      <td>80</td>
      <td>12</td>
      <td>43</td>
      <td>36</td>
      <td>19</td>
    </tr>
  </tbody>
</table>

Ma cosa accade se non è possibile creare tali associazioni visive? Come si può leggere una tabella come quella sopra? Le persone con disabilità visive usano spesso un {{Glossary("Screen_reader", "lettore di schermo")}} che legge loro le informazioni presenti nelle pagine web. Questo non è un problema quando si legge testo semplice, ma interpretare una tabella può essere piuttosto difficile per una persona non vedente. Tuttavia, con il markup appropriato è possibile sostituire le associazioni visive con associazioni programmatiche.

> [!NOTE]
> Secondo i [dati dell'OMS del 2017](https://www.who.int/en/news-room/fact-sheets/detail/blindness-and-visual-impairment), nel mondo vivono circa 253 milioni di persone con disabilità visive.

### Uso delle intestazioni di colonna e riga

I lettori di schermo identificano tutte le intestazioni e le utilizzano per creare associazioni programmatiche tra tali intestazioni e le celle a cui si riferiscono. La combinazione di intestazioni di colonna e riga identifica e interpreta i dati in ogni cella, così che gli utenti di lettori di schermo possano interpretare la tabella in modo simile a un utente vedente.

Le intestazioni sono già state trattate nell'articolo precedente: vedere [Aggiunta di intestazioni con elementi \<th>](/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics#adding_headers_with_th_elements).

## Aggiungere una didascalia alla tabella con \<caption>

È possibile assegnare una didascalia alla tabella inserendola in un elemento {{htmlelement("caption")}} e annidandolo nell'elemento {{htmlelement("table")}}. Dovrebbe essere inserita subito sotto il tag di apertura `<table>`.

```html
<table>
  <caption>
    Dinosaurs in the Jurassic period
  </caption>
  <!-- … -->
</table>
```

Come si può dedurre dal breve esempio precedente, la didascalia è destinata a contenere una descrizione del contenuto della tabella. È utile a tutti i lettori che desiderano farsi rapidamente un'idea dell'utilità della tabella mentre esaminano la pagina, ma in particolare agli utenti non vedenti. Invece di far leggere a un lettore di schermo il contenuto di molte celle solo per capire di cosa tratta la tabella, l'utente può fare affidamento su una didascalia e decidere quindi se leggere o meno la tabella in maggiore dettaglio.

Una didascalia viene inserita direttamente sotto il tag `<table>`.

> [!NOTE]
> L'attributo [`summary`](/it/docs/Web/HTML/Reference/Elements/table#summary) può essere usato anche sull'elemento `<table>` per fornire una descrizione; anch'essa viene letta dai lettori di schermo. Tuttavia, è consigliabile usare invece l'elemento `<caption>`, poiché `summary` è deprecato e non può essere letto dagli utenti vedenti, dato che non appare nella pagina.

### Esercitazione sulle didascalie delle tabelle

A questo punto, è possibile provare ad aggiungere una didascalia a una tabella HTML, usando l'orario scolastico incontrato nell'articolo precedente.

1. Copiare il primo blocco HTML della sezione [Raggruppare colonne con `<colgroup>` e `<col>`](/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics#grouping_columns_with_colgroup_and_col) in un file HTML sul computer oppure in un editor online come [CodePen](https://codepen.io/) o [JSBin](https://jsbin.com/).
2. Aggiungere una didascalia adatta alla tabella.
3. Salvare il codice e osservare il risultato.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html
<table>
  <caption>
    St. Winnifred's weekly language lesson timetable
  </caption>
  <colgroup>
    <col span="2" />
    <col class="column-background" />
    <col class="column-fixed-width" />
    <col class="column-background" />
    <col class="column-background-border" />
    <col span="2" class="column-fixed-width" />
  </colgroup>

  <!-- Rest of code omitted for brevity -->
</table>
```

</details>

## Aggiungere struttura con \<thead>, \<tbody> e \<tfoot>

Quando le tabelle diventano un po' più complesse dal punto di vista strutturale, è utile fornire loro una maggiore definizione strutturale. Un modo chiaro per farlo consiste nell'usare {{htmlelement("thead")}}, {{htmlelement("tbody")}} e {{htmlelement("tfoot")}}, che consentono di definire una sezione di intestazione, corpo e piè di pagina per la tabella.

Questi elementi non rendono necessariamente la tabella più accessibile agli utenti di lettori di schermo. Non producono alcun miglioramento visivo da soli, tuttavia sono molto utili per applicare miglioramenti di stile e layout tramite CSS, che possono migliorare l'accessibilità. Per fornire alcuni esempi interessanti, nel caso di una tabella lunga si potrebbero fare ripetere l'intestazione e il piè di pagina della tabella su ogni pagina stampata, e fare visualizzare il corpo della tabella su una singola pagina rendendo il contenuto disponibile tramite lo scorrimento verso l'alto e verso il basso.

Per usarli, devono essere inclusi nel seguente ordine:

- L'elemento `<thead>` deve racchiudere la parte della tabella che costituisce l'intestazione; di solito è la prima riga contenente i titoli delle colonne, ma non è necessariamente sempre così. Se si usano elementi {{htmlelement("col")}}/{{htmlelement("colgroup")}}, l'intestazione della tabella dovrebbe trovarsi subito sotto di essi.
- L'elemento `<tbody>` deve racchiudere la parte principale del contenuto della tabella che non è l'intestazione o il piè di pagina della tabella e deve venire dopo `<thead>`.
- L'elemento `<tfoot>` deve racchiudere la parte della tabella che costituisce il piè di pagina; potrebbe trattarsi, ad esempio, di una riga finale con la somma degli elementi delle righe precedenti. `<tfoot>` deve venire dopo `<tbody>`.

> [!NOTE]
> `<tbody>` viene sempre incluso implicitamente in ogni tabella se non viene specificato nel codice. Per verificarlo, aprire uno degli esempi precedenti che non include `<tbody>` e osservare il codice HTML negli [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools): sarà visibile che il browser ha aggiunto questo tag. Ci si potrebbe chiedere perché sia necessario includerlo: è opportuno farlo perché offre maggiore controllo sulla struttura e sullo stile della tabella.

### Aggiungere struttura a una tabella di registro delle spese

Mettiamo in pratica questi nuovi elementi.

1. Prima di tutto, creare un nuovo file HTML chiamato `spending-record.html` e inserire il seguente HTML all'interno di `<body>`:

   ```html
   <h1>My spending record</h1>

   <table>
     <caption>
       How I chose to spend my money
     </caption>
     <tr>
       <th>Purchase</th>
       <th>Location</th>
       <th>Date</th>
       <th>Evaluation</th>
       <th>Cost (€)</th>
     </tr>
     <tr>
       <td>Haircut</td>
       <td>Hairdresser</td>
       <td>12/09</td>
       <td>Great idea</td>
       <td>30</td>
     </tr>
     <tr>
       <td>Lasagna</td>
       <td>Restaurant</td>
       <td>12/09</td>
       <td>Regrets</td>
       <td>18</td>
     </tr>
     <tr>
       <td>Shoes</td>
       <td>Shoe shop</td>
       <td>13/09</td>
       <td>Big regrets</td>
       <td>65</td>
     </tr>
     <tr>
       <td>Toothpaste</td>
       <td>Supermarket</td>
       <td>13/09</td>
       <td>Good</td>
       <td>5</td>
     </tr>
     <tr>
       <td>SUM</td>
       <td>118</td>
     </tr>
   </table>
   ```

2. Successivamente, creare un file CSS chiamato `minimal-table.css` nella stessa directory del file HTML e inserire il seguente contenuto:

   ```css live-sample___finished-table-structure
   html {
     font-family: sans-serif;
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

   th {
     background-color: rgb(235 235 235);
   }

   td {
     text-align: center;
   }

   tr:nth-child(even) td {
     background-color: rgb(250 250 250);
   }

   tr:nth-child(odd) td {
     background-color: rgb(245 245 245);
   }

   caption {
     padding: 10px;
   }
   ```

3. Aggiungere un elemento `<link>` nell'elemento `<head>` dell'HTML per applicare il CSS all'HTML (per assistenza, vedere [Applicare CSS e JavaScript a HTML](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#applying_css_and_javascript_to_html)).

4. Provare a inserire l'evidente riga delle intestazioni all'interno di un elemento `<thead>`, la riga "SUM" all'interno di un elemento `<tfoot>` e il resto del contenuto all'interno di un elemento `<tbody>`.
5. Successivamente, aggiungere un attributo [`colspan`](/it/docs/Web/HTML/Reference/Elements/td#colspan) per fare in modo che la cella "SUM" si estenda sulle prime quattro colonne, così che il numero effettivo appaia in fondo alla colonna "Cost".
6. Aggiungiamo un semplice stile aggiuntivo alla tabella, per dare un'idea dell'utilità di questi elementi nell'applicazione di CSS. Aggiungere quanto segue al file CSS:

   ```css live-sample___finished-table-structure
   tbody {
     font-size: 95%;
     font-style: italic;
   }

   tfoot {
     font-weight: bold;
   }
   ```

   > [!NOTE]
   > Non è necessario comprendere completamente il CSS in questo momento. L'argomento verrà approfondito nei moduli CSS, a partire da [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics), che include un articolo specifico sullo [stile delle tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables).

7. Salvare e aggiornare la pagina, quindi osservare il risultato. Se gli elementi `<tbody>` e `<tfoot>` non fossero presenti, sarebbe necessario scrivere selettori/regole molto più complessi per applicare lo stesso stile.

L'esempio completato dovrebbe apparire così:

{{embedlivesample("finished-table-structure", "100%", "300")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html live-sample___finished-table-structure
<table>
  <caption>
    How I chose to spend my money
  </caption>
  <thead>
    <tr>
      <th>Purchase</th>
      <th>Location</th>
      <th>Date</th>
      <th>Evaluation</th>
      <th>Cost (€)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Haircut</td>
      <td>Hairdresser</td>
      <td>12/09</td>
      <td>Great idea</td>
      <td>30</td>
    </tr>
    <tr>
      <td>Lasagna</td>
      <td>Restaurant</td>
      <td>12/09</td>
      <td>Regrets</td>
      <td>18</td>
    </tr>
    <tr>
      <td>Shoes</td>
      <td>Shoe shop</td>
      <td>13/09</td>
      <td>Big regrets</td>
      <td>65</td>
    </tr>
    <tr>
      <td>Toothpaste</td>
      <td>Supermarket</td>
      <td>13/09</td>
      <td>Good</td>
      <td>5</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="4">SUM</td>
      <td>118</td>
    </tr>
  </tfoot>
</table>
```

</details>

## L'attributo `scope`

L'attributo [`scope`](/it/docs/Web/HTML/Reference/Elements/th#scope) può essere aggiunto all'elemento `<th>` per indicare ai lettori di schermo esattamente per quali celle l'intestazione è un'intestazione: è un'intestazione per la riga in cui si trova oppure per la colonna, ad esempio? Tornando all'esempio precedente del registro delle spese, è possibile definire senza ambiguità le intestazioni delle colonne come intestazioni di colonna nel seguente modo:

```html
<thead>
  <tr>
    <th scope="col">Purchase</th>
    <th scope="col">Location</th>
    <th scope="col">Date</th>
    <th scope="col">Evaluation</th>
    <th scope="col">Cost (€)</th>
  </tr>
</thead>
```

E ogni riga potrebbe avere un'intestazione definita in questo modo, se si aggiungessero intestazioni di riga oltre alle intestazioni di colonna:

```html
<tr>
  <th scope="row">Haircut</th>
  <td>Hairdresser</td>
  <td>12/09</td>
  <td>Great idea</td>
  <td>30</td>
</tr>
```

I lettori di schermo riconosceranno un markup strutturato in questo modo e consentiranno ai loro utenti di leggere, ad esempio, l'intera colonna o riga in una sola volta.

`scope` ha altri due valori possibili: `colgroup` e `rowgroup`. Vengono usati per le intestazioni che si trovano sopra più colonne o righe. Se si torna alla tabella "Articoli venduti agosto 2016" all'inizio di questa sezione dell'articolo, si noterà che la cella "Abbigliamento" si trova sopra le celle "Pantaloni", "Gonne" e "Vestiti". Tutte queste celle devono essere contrassegnate come intestazioni (`<th>`), ma "Abbigliamento" è un'intestazione che si trova sopra e definisce le altre tre sottointestazioni. "Abbigliamento" dovrebbe quindi avere un attributo `scope="colgroup"`, mentre le altre dovrebbero avere un attributo `scope="col"`:

```html
<thead>
  <tr>
    <th colspan="3" scope="colgroup">Clothes</th>
  </tr>
  <tr>
    <th scope="col">Trousers</th>
    <th scope="col">Skirts</th>
    <th scope="col">Dresses</th>
  </tr>
</thead>
```

Lo stesso vale per le intestazioni di più righe raggruppate. Osservare nuovamente la tabella "Articoli venduti agosto 2016", questa volta concentrandosi sulle righe con le intestazioni "Amsterdam" e "Utrecht" (`<th>`). Si noterà che l'intestazione "Paesi Bassi", anch'essa contrassegnata come elemento `<th>`, si estende su entrambe le righe, essendo l'intestazione delle altre due sottointestazioni. Pertanto, su questa cella di intestazione dovrebbe essere specificato `scope="rowgroup"` per aiutare i lettori di schermo a creare le associazioni corrette:

```html
<tr>
  <th rowspan="2" scope="rowgroup">The Netherlands</th>
  <th scope="row">Amsterdam</th>
  <td>89</td>
  <td>34</td>
  <td>69</td>
</tr>
<tr>
  <th scope="row">Utrecht</th>
  <td>80</td>
  <td>12</td>
  <td>43</td>
</tr>
```

## Gli attributi `id` e `headers`

Un'alternativa all'uso dell'attributo `scope` consiste nell'usare gli attributi [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) e [`headers`](/it/docs/Web/HTML/Reference/Elements/td#headers) per creare associazioni tra celle di dati e celle di intestazione.

Un elemento `<th>` può fornire un'intestazione per una cella di dati (`<td>`) oppure, nelle tabelle più complesse, per un'altra cella di intestazione (`<th>`). Ciò consente di creare intestazioni stratificate o raggruppate, in cui un'intestazione ne descrive diverse altre.

L'attributo `headers` viene usato per collegare una cella, `<td>` o `<th>`, a una o più celle di intestazione. Accetta un elenco di {{Glossary("string", "stringhe")}} separate da spazi; l'ordine delle stringhe non è importante. Ogni stringa deve corrispondere all'`id` univoco di un elemento `<th>` a cui la cella è associata.

Questo metodo fornisce alla tabella HTML una definizione più esplicita della posizione di ogni cella, basata sulle intestazioni della colonna e della riga di cui fa parte, in modo simile a un foglio di calcolo. Affinché funzioni bene, la tabella dovrebbe includere sia intestazioni di colonna sia di riga.

Vediamo una parte dell'esempio "Articoli venduti agosto 2016" per capire come usare gli attributi `id` e `headers`:

1. Aggiungere un `id` univoco a ogni elemento `<th>` della tabella.
2. Per le celle di intestazione: aggiungere un attributo `headers` a ogni elemento `<th>` che agisce da sottointestazione, ovvero una cella di intestazione con un'altra intestazione sopra di essa. Il valore è l'`id` dell'intestazione di livello superiore. Nel nostro esempio, è `"clothes"` per le intestazioni di colonna e `"belgium"` per l'intestazione di riga.
3. Per le celle di dati: aggiungere un attributo `headers` a ogni elemento `<td>` e aggiungere gli `id` degli elementi `<th>` associati come elenco separato da spazi. Si può procedere come in un foglio di calcolo: trovare la cella di dati, quindi individuare le intestazioni di riga e colonna che la descrivono. L'ordine degli `id` specificati non è importante, ma mantenerlo coerente aiuta a mantenere il codice organizzato e ne migliora la leggibilità.

```html
<thead>
  <tr>
    <th></th>
    <th></th>
    <th id="clothes" colspan="3">Clothes</th>
  </tr>
  <tr>
    <th></th>
    <th></th>
    <th id="trousers" headers="clothes">Trousers</th>
    <th id="skirts" headers="clothes">Skirts</th>
    <th id="dresses" headers="clothes">Dresses</th>
  </tr>
</thead>
<tbody>
  <tr>
    <th id="belgium" rowspan="2">Belgium</th>
    <th id="antwerp" headers="belgium">Antwerp</th>
    <td headers="belgium antwerp clothes trousers">56</td>
    <td headers="belgium antwerp clothes skirts">22</td>
    <td headers="belgium antwerp clothes dresses">43</td>
  </tr>
  <tr>
    <th id="ghent" headers="belgium">Ghent</th>
    <td headers="belgium ghent clothes trousers">41</td>
    <td headers="belgium ghent clothes skirts">17</td>
    <td headers="belgium ghent clothes dresses">35</td>
  </tr>
</tbody>
```

In questo esempio:

- Il `<th>` per `"Belgium"` usa `rowspan="2"` per estendersi sia su `"Antwerp"` sia su `"Ghent"`.
- Le celle di intestazione delle città (`"Antwerp"` e `"Ghent"`) usano l'attributo `headers` per fare riferimento a `"belgium"` e mostrare che appartengono al gruppo Belgio.
- Ogni `<td>` include un attributo `headers` per il paese (`belgium`), la città (`antwerp` o `ghent`), il gruppo (`clothes`) e lo specifico capo di abbigliamento (`trousers`, `skirts` o `dresses`).

> [!NOTE]
> Questo metodo crea associazioni molto precise tra intestazioni e celle di dati, ma utilizza **molto** più markup e non lascia spazio a errori. L'approccio con `scope` è generalmente sufficiente per la maggior parte delle tabelle.

## Esercitarsi con scope e headers

Per questo esercizio finale, è possibile provare a usare scope e headers sulla tabella di esempio introdotta sopra.

1. Creare copie locali di [items-sold.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/minimal-table.css), in una nuova directory.
2. Provare ad aggiungere gli attributi `scope` appropriati per rendere questa tabella più accessibile.
3. Creare un'altra copia dei file iniziali in un'altra directory locale.
4. Questa volta, rendere la tabella più accessibile creando associazioni precise ed esplicite mediante gli attributi `id` e `headers`.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il primo esempio HTML completato dovrebbe essere simile a questo:

```html
<table>
  <caption>
    Items Sold August 2016
  </caption>
  <thead>
    <tr>
      <td colspan="2" rowspan="2"></td>
      <th colspan="3" scope="colgroup">Clothes</th>
      <th colspan="2" scope="colgroup">Accessories</th>
    </tr>
    <tr>
      <th scope="col">Trousers</th>
      <th scope="col">Skirts</th>
      <th scope="col">Dresses</th>
      <th scope="col">Bracelets</th>
      <th scope="col">Rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" scope="rowgroup">Belgium</th>
      <th scope="row">Antwerp</th>
      <td>56</td>
      <td>22</td>
      <td>43</td>
      <td>72</td>
      <td>23</td>
    </tr>
    <tr>
      <th scope="row">Ghent</th>
      <td>46</td>
      <td>18</td>
      <td>50</td>
      <td>61</td>
      <td>15</td>
    </tr>
    <tr>
      <th scope="row">Brussels</th>
      <td>51</td>
      <td>27</td>
      <td>38</td>
      <td>69</td>
      <td>28</td>
    </tr>
    <tr>
      <th rowspan="2" scope="rowgroup">The Netherlands</th>
      <th scope="row">Amsterdam</th>
      <td>89</td>
      <td>34</td>
      <td>69</td>
      <td>85</td>
      <td>38</td>
    </tr>
    <tr>
      <th scope="row">Utrecht</th>
      <td>80</td>
      <td>12</td>
      <td>43</td>
      <td>36</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
```

Il secondo dovrebbe invece apparire così:

```html
<table>
  <caption>
    Items Sold August 2016
  </caption>
  <thead>
    <tr>
      <td colspan="2" rowspan="2"></td>
      <th colspan="3" id="clothes">Clothes</th>
      <th colspan="2" id="accessories">Accessories</th>
    </tr>
    <tr>
      <th id="trousers" headers="clothes">Trousers</th>
      <th id="skirts" headers="clothes">Skirts</th>
      <th id="dresses" headers="clothes">Dresses</th>
      <th id="bracelets" headers="accessories">Bracelets</th>
      <th id="rings" headers="accessories">Rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" id="belgium">Belgium</th>
      <th id="antwerp" headers="belgium">Antwerp</th>
      <td headers="antwerp belgium clothes trousers">56</td>
      <td headers="antwerp belgium clothes skirts">22</td>
      <td headers="antwerp belgium clothes dresses">43</td>
      <td headers="antwerp belgium accessories bracelets">72</td>
      <td headers="antwerp belgium accessories rings">23</td>
    </tr>
    <tr>
      <th id="ghent" headers="belgium">Ghent</th>
      <td headers="ghent belgium clothes trousers">46</td>
      <td headers="ghent belgium clothes skirts">18</td>
      <td headers="ghent belgium clothes dresses">50</td>
      <td headers="ghent belgium accessories bracelets">61</td>
      <td headers="ghent belgium accessories rings">15</td>
    </tr>
    <tr>
      <th id="brussels" headers="belgium">Brussels</th>
      <td headers="brussels belgium clothes trousers">51</td>
      <td headers="brussels belgium clothes skirts">27</td>
      <td headers="brussels belgium clothes dresses">38</td>
      <td headers="brussels belgium accessories bracelets">69</td>
      <td headers="brussels belgium accessories rings">28</td>
    </tr>
    <tr>
      <th rowspan="2" id="netherlands">The Netherlands</th>
      <th id="amsterdam" headers="netherlands">Amsterdam</th>
      <td headers="amsterdam netherlands clothes trousers">89</td>
      <td headers="amsterdam netherlands clothes skirts">34</td>
      <td headers="amsterdam netherlands clothes dresses">69</td>
      <td headers="amsterdam netherlands accessories bracelets">85</td>
      <td headers="amsterdam netherlands accessories rings">38</td>
    </tr>
    <tr>
      <th id="utrecht" headers="netherlands">Utrecht</th>
      <td headers="utrecht netherlands clothes trousers">80</td>
      <td headers="utrecht netherlands clothes skirts">12</td>
      <td headers="utrecht netherlands clothes dresses">43</td>
      <td headers="utrecht netherlands accessories bracelets">36</td>
      <td headers="utrecht netherlands accessories rings">19</td>
    </tr>
  </tbody>
</table>
```

Gli esempi completati sono disponibili anche su GitHub:

- Per il primo esempio, vedere [items-sold-scope.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold-scope.html) ([vedere anche l'esempio in esecuzione](https://mdn.github.io/learning-area/html/tables/advanced/items-sold-scope.html)).
- Per il secondo esempio, vedere [items-sold-headers.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold-headers.html) ([vedere anche l'esempio in esecuzione](https://mdn.github.io/learning-area/html/tables/advanced/items-sold-headers.html)).

</details>

## Riepilogo

Ci sono alcuni altri aspetti da imparare sulle tabelle in HTML, ma per ora è tutto ciò che serve sapere. Successivamente, è possibile mettersi alla prova con la sfida sulle tabelle HTML. Buon divertimento!

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content")}}
