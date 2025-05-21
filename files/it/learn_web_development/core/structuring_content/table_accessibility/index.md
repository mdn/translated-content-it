---
title: Accessibilità delle tabelle HTML
short-title: Accessibilità delle tabelle
slug: Learn_web_development/Core/Structuring_content/Table_accessibility
l10n:
  sourceCommit: 73c2e226085c7902dd200f7d6c3a6730beb6d941
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content")}}

Nell'articolo precedente, abbiamo esaminato una delle caratteristiche più importanti per rendere le tabelle HTML accessibili agli utenti con disabilità visive — l'elemento {{htmlelement("th")}}. In questo articolo, continueremo su questa strada, esaminando ulteriori caratteristiche di accessibilità delle tabelle HTML come didascalie/sommari, raggruppamento delle righe nelle sezioni di intestazione, corpo e piede della tabella, e definizione dell'ambito delle colonne e delle righe.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Le basi di HTML (vedi
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax">Sintassi di base di HTML</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere i problemi di accessibilità associati alle tabelle.</li>
          <li>Aggiungere didascalie alle tabelle.</li>
          <li>Strutturazione migliore della tabella con intestazione, corpo e piede.</li>
          <li>Creare ulteriori associazioni tra intestazioni e celle con gli attributi <code>scope</code>, <code>id</code> e <code>headers</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo: Tabelle per utenti con disabilità visiva

Recapitoliamo brevemente come utilizziamo le tabelle di dati. Una tabella può essere uno strumento utile per permetterci di accedere rapidamente ai dati e consentirci di cercare diversi valori. Ad esempio, è sufficiente un breve sguardo alla tabella qui sotto per scoprire quanti anelli sono stati venduti a Gent durante agosto 2016. Per comprendere le sue informazioni, facciamo associazioni visive tra i dati in questa tabella e le sue intestazioni di colonna e/o riga.

<table>
  <caption>Oggetti venduti ad agosto 2016</caption>
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
      <th scope="row">Gent</th>
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

Ma cosa succede se non puoi fare quelle associazioni visive? Come puoi leggere una tabella come quella sopra? Le persone con disabilità visive spesso utilizzano uno screen reader che legge ad alta voce le informazioni sulle pagine web. Questo non è un problema quando si legge un testo semplice, ma interpretare una tabella può essere piuttosto una sfida per una persona non vedente. Tuttavia, con il corretto markup possiamo sostituire le associazioni visive con associazioni programmatiche.

> [!NOTE]
> Ci sono circa 253 milioni di persone che vivono con disabilità visive secondo i [dati dell'OMS del 2017](https://www.who.int/en/news-room/fact-sheets/detail/blindness-and-visual-impairment).

Questa sezione dell'articolo fornisce ulteriori tecniche per rendere le tabelle il più accessibili possibile.

### Uso delle intestazioni di colonna e riga

Gli screen reader identificheranno tutte le intestazioni e le utilizzeranno per creare associazioni programmatiche tra quelle intestazioni e le celle a cui si riferiscono. La combinazione di intestazioni di colonna e riga identificherà e interpreterà i dati in ogni cella in modo che gli utenti di screen reader possano interpretare la tabella in modo simile a come farebbe un utente vedente.

Abbiamo già trattato le intestazioni nel nostro articolo precedente — vedere [Aggiungere intestazioni con elementi \<th>](/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics#adding_headers_with_th_elements).

## Aggiunta di una didascalia alla tabella con \<caption>

Puoi dare una didascalia alla tua tabella inserendola all'interno di un elemento {{htmlelement("caption")}} e annidandolo all'interno dell'elemento {{htmlelement("table")}}. Dovresti posizionarlo appena sotto il tag `<table>` di apertura.

```html
<table>
  <caption>
    Dinosaurs in the Jurassic period
  </caption>

  …
</table>
```

Come puoi dedurre dal breve esempio sopra, la didascalia è destinata a contenere una descrizione del contenuto della tabella. Questo è utile per tutti i lettori che desiderano avere un'idea rapida di se la tabella sia utile per loro mentre navigano la pagina, ma particolarmente per gli utenti non vedenti. Piuttosto che avere uno screen reader che legge il contenuto di molte celle solo per scoprire di cosa tratta la tabella, l'utente può fare affidamento su una didascalia e poi decidere se leggere o meno la tabella in modo più dettagliato.

Una didascalia è posizionata direttamente sotto il tag `<table>`.

> [!NOTE]
> L'attributo [`summary`](/it/docs/Web/HTML/Reference/Elements/table#summary) può anche essere usato sull'elemento `<table>` per fornire una descrizione — anche questo viene letto dagli screen reader. Tuttavia, consigliamo di usare l'elemento `<caption>` poiché `summary` è deprecato e non può essere letto dagli utenti vedenti (non appare sulla pagina).

### Apprendimento attivo: Aggiunta di una didascalia

Proviamo questo utilizzo, prendendo come esempio l'orario scolastico di un insegnante di lingue.

1. Fai una copia locale del nostro file [timetable-fixed.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/timetable-fixed.html).
2. Aggiungi una didascalia adatta per la tabella.
3. Salva il tuo codice e aprilo in un browser per vedere come appare.

> [!NOTE]
> Puoi trovare la nostra versione su GitHub — vedi [timetable-caption.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/timetable-caption.html) ([vedilo in diretta anche](https://mdn.github.io/learning-area/html/tables/advanced/timetable-caption.html)).

## Aggiunta di struttura con \<thead>, \<tbody>, e \<tfoot>

Man mano che le tue tabelle diventano un po' più complesse nella struttura, è utile fornire loro maggiore definizione strutturale. Un modo chiaro per farlo è utilizzare {{htmlelement("thead")}}, {{htmlelement("tbody")}}, e {{htmlelement("tfoot")}}, che ti consentono di contrassegnare una sezione di intestazione, corpo e piede per la tabella.

Questi elementi non rendono necessariamente la tabella più accessibile agli utenti di screen reader. Non portano a nessun miglioramento visivo di per sé, tuttavia sono molto utili per applicare miglioramenti di stile e layout tramite CSS, che possono migliorare l'accessibilità. Per darti alcuni esempi interessanti, nel caso di una tabella lunga potresti fare in modo che l'intestazione e il piede della tabella si ripetino su ogni pagina stampata, e potresti fare in modo che il corpo della tabella sia visualizzato su una singola pagina e che i contenuti siano disponibili scorrendo verso l'alto e verso il basso.

Per utilizzarli, devono essere inclusi nel seguente ordine:

- L'elemento `<thead>` deve avvolgere la parte della tabella che è l'intestazione — questo è solitamente la prima riga contenente le intestazioni delle colonne, ma non è necessariamente sempre il caso. Se stai utilizzando gli elementi {{htmlelement("col")}}/{{htmlelement("colgroup")}}, l'intestazione della tabella dovrebbe venire subito sotto questi.
- L'elemento `<tbody>` deve racchiudere la parte principale del contenuto della tabella che non è l'intestazione o il piede della tabella.
- L'elemento `<tfoot>` deve avvolgere la parte della tabella che è il piede — questo potrebbe essere una riga finale con elementi nelle righe precedenti sommati, ad esempio.

> **Nota:** `<tbody>` è sempre incluso in ogni tabella, implicitamente se non lo specifichi nel tuo codice. Per verificarlo, apri uno dei tuoi esempi precedenti che non include `<tbody>` e guarda il codice HTML nei tuoi [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) — vedrai che il browser ha aggiunto questo tag per te. Potresti chiederti perché dovresti preoccuparti di includerlo affatto — dovresti, perché ti dà maggiore controllo sulla struttura e lo stile della tua tabella.

### Apprendimento attivo: Aggiunta di struttura alla tabella

Mettiamo in pratica questi nuovi elementi.

1. Innanzitutto, fai una copia locale dei file [spending-record.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/spending-record.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/minimal-table.css) in una nuova cartella.
2. Prova a mettere la riga ovvia delle intestazioni all'interno di un elemento `<thead>`, la riga "SOMMA" all'interno di un elemento `<tfoot>`, e il resto del contenuto all'interno di un elemento `<tbody>`.
3. Successivamente, aggiungi un attributo [`colspan`](/it/docs/Web/HTML/Reference/Elements/td#colspan) per fare in modo che la cella "SOMMA" si estenda attraverso le prime quattro colonne, in modo che il numero effettivo appaia in fondo alla colonna "Costo".
4. Aggiungiamo un semplice styling extra alla tabella, per darti un'idea di quanto utili questi elementi siano per applicare CSS. All'interno del capo del tuo documento HTML, vedrai un elemento {{htmlelement("style")}} vuoto. All'interno di questo elemento, aggiungi le seguenti righe di codice CSS:

   ```css
   tbody {
     font-size: 95%;
     font-style: italic;
   }

   tfoot {
     font-weight: bold;
   }
   ```

5. Salva e aggiorna, e dai un'occhiata al risultato. Se gli elementi `<tbody>` e `<tfoot>` non fossero presenti, dovresti scrivere selettori/regole molto più complicati per applicare lo stesso stile.

> [!NOTE]
> Non ci aspettiamo che tu comprenda completamente il CSS in questo momento. Imparerai di più su questo quando affronterai i nostri moduli CSS ([Basi di styling CSS](/it/docs/Learn_web_development/Core/Styling_basics) è un buon punto di partenza; abbiamo anche un articolo specifico sul [styling delle tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables)).

La tua tabella finita dovrebbe apparire come segue:

{{ EmbedGHLiveSample('learning-area/html/tables/advanced/spending-record-finished.html', '100%', 400) }}

> [!NOTE]
> Puoi anche trovarlo su GitHub come [spending-record-finished.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/spending-record-finished.html) ([vedilo live qui](https://mdn.github.io/learning-area/html/tables/advanced/spending-record-finished.html)).

## L'attributo `scope`

L'attributo [`scope`](/it/docs/Web/HTML/Reference/Elements/th#scope) può essere aggiunto all'elemento `<th>` per indicare agli screen reader esattamente quali celle l'intestazione è destinata a coprire — è un'intestazione per la riga in cui si trova, o la colonna, ad esempio? Guardando al nostro esempio di spesa da prima, potresti definire chiaramente le intestazioni delle colonne come intestazioni di colonna in questo modo:

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

E ogni riga potrebbe avere un'intestazione definita in questo modo (se aggiungessimo intestazioni di riga oltre a quelle di colonna):

```html
<tr>
  <th scope="row">Haircut</th>
  <td>Hairdresser</td>
  <td>12/09</td>
  <td>Great idea</td>
  <td>30</td>
</tr>
```

Gli screen reader riconosceranno il markup strutturato in questo modo e consentiranno ai loro utenti di leggere l'intera colonna o riga in una volta sola, ad esempio.

`scope` ha altri due valori possibili — `colgroup` e `rowgroup`. Questi sono usati per intestazioni che si trovano sopra più colonne o righe. Se guardi di nuovo alla tabella "Oggetti venduti ad agosto 2016" all'inizio di questa sezione dell'articolo, vedrai che la cella "Abbigliamento" si trova sopra le celle "Pantaloni", "Gonne", e "Vestiti". Tutte queste celle dovrebbero essere contrassegnate come intestazioni (`<th>`), ma "Abbigliamento" è un'intestazione che si trova sopra e definisce le altre tre sottointestazioni. "Abbigliamento" quindi dovrebbe ricevere un attributo di `scope="colgroup"`, mentre le altre avrebbero un attributo di `scope="col"`:

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

Lo stesso si applica alle intestazioni per più righe raggruppate. Dai un'altra occhiata alla tabella "Oggetti venduti ad agosto 2016", questa volta concentrandoti sulle righe con le intestazioni "Amsterdam" e "Utrecht" (`<th>`). Noterai che l'intestazione "Paesi Bassi", anch'essa contrassegnata come elemento `<th>`, si estende su entrambe le righe, essendo l'intestazione per le altre due sottointestazioni. Pertanto, `scope="rowgroup"` dovrebbe essere specificato su questa cella di intestazione per aiutare gli screen reader a creare le associazioni corrette:

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

Un'alternativa all'utilizzo dell'attributo `scope` è utilizzare gli attributi [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) e [`headers`](/it/docs/Web/HTML/Reference/Elements/td#headers) per creare associazioni tra celle di dati e celle di intestazione.

Un elemento `<th>` può fornire un'intestazione sia per una cella di dati (`<td>`) sia, in tabelle più complesse, per un'altra cella di intestazione (`<th>`). Ciò consente di creare intestazioni stratificate o raggruppate, dove un'intestazione descrive diverse altre.

L'attributo `headers` è utilizzato per collegare una cella, `<td>` o `<th>`, a una o più celle di intestazione. Prende una lista di {{Glossary("string", "stringhe")}} separate da spazi; l'ordine delle stringhe non importa. Ogni stringa deve corrispondere all'`id` unico di un elemento `<th>` con cui la cella è associata.

Questo metodo conferisce alla tua tabella HTML una definizione più esplicita della posizione di ciascuna cella, basata sulle intestazioni per la colonna e la riga a cui appartiene, quasi come un foglio di calcolo. Per far funzionare bene, la tua tabella dovrebbe includere sia intestazioni di colonna che di riga.

Guardiamo a una parte dell'esempio "Oggetti venduti ad agosto 2016" per vedere come utilizzare gli attributi `id` e `headers`:

1. Aggiungi un `id` unico a ciascun elemento `<th>` nella tabella.
2. Per le celle di intestazione: Aggiungi un attributo `headers` a ciascun elemento `<th>` che funge da sottointestazione, cioè a una cella di intestazione con un'altra intestazione sopra di essa. Il valore è l'`id` dell'intestazione di alto livello. Nel nostro esempio, è `"abbigliamento"` per le intestazioni di colonna e `"belgio"` per l'intestazione di riga.
3. Per le celle di dati: Aggiungi un attributo `headers` a ciascun elemento `<td>`, ed aggiungi gli `id` degli elementi `<th>` associati come lista separata da spazi. Puoi procedere come faresti in un foglio di calcolo: Trova la cella di dati, quindi individua le intestazioni di riga e colonna che la descrivono. L'ordine degli `id` specificati non conta, ma mantenere una coerenza aiuta a mantenerlo organizzato e migliora la leggibilità del codice.

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

- Il `<th>` per `"Belgio"` utilizza `rowspan="2"` per coprire sia `"Anversa"` che `"Ghent"`.
- Le celle di intestazione delle città (`"Anversa"` e `"Ghent"`) utilizzano l'attributo `headers` per fare riferimento a `"belgio"` per mostrare che appartengono al gruppo Belgio.
- Ogni `<td>` include un attributo `headers` per paese (`belgio`), città (`anversa` o `ghent`), gruppo (`abbigliamento`) e l'oggetto di abbigliamento specifico (`pantaloni`, `gonne` o `vestiti`).

> [!NOTE]
> Questo metodo crea associazioni molto precise tra intestazioni e celle di dati ma utilizza **molto** più markup e non lascia spazio a errori. L'approccio `scope` è solitamente sufficiente per la maggior parte delle tabelle.

## Apprendimento attivo: giocare con scope e headers

1. Per questo esercizio finale, ti invitiamo prima a creare copie locali di [items-sold.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/minimal-table.css), in una nuova directory.
2. Ora prova ad aggiungere i giusti attributi `scope` per rendere questa tabella più accessibile.
3. Infine, prova a creare un'altra copia dei file iniziali, e questa volta rendi la tabella più accessibile creando associazioni precise ed esplicite utilizzando gli attributi `id` e `headers`.

> [!NOTE]
> Puoi confrontare il tuo lavoro con i nostri esempi finiti — vedi [items-sold-scope.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold-scope.html) ([vedi anche questo live](https://mdn.github.io/learning-area/html/tables/advanced/items-sold-scope.html)) e [items-sold-headers.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold-headers.html) ([vedi anche questo live](https://mdn.github.io/learning-area/html/tables/advanced/items-sold-headers.html)).

## Riepilogo

Ci sono alcune altre cose che potresti imparare sulle tabelle in HTML, ma questo è tutto ciò che devi sapere per ora. Successivamente, puoi metterti alla prova con la nostra sfida sulle tabelle HTML. Divertiti!

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content")}}
