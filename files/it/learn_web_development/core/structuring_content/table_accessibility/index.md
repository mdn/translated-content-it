---
title: Accessibilità delle tabelle HTML
short-title: Accessibilità delle tabelle
slug: Learn_web_development/Core/Structuring_content/Table_accessibility
l10n:
  sourceCommit: 73c2e226085c7902dd200f7d6c3a6730beb6d941
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content")}}

Nel precedente articolo, abbiamo esaminato una delle caratteristiche più importanti per rendere le tabelle HTML accessibili agli utenti con disabilità visive — l'elemento {{htmlelement("th")}}. In questo articolo, continuiamo su questo percorso, esaminando ulteriori funzionalità per l'accessibilità delle tabelle HTML come didascalie/sommari, raggruppamento delle righe in sezioni di intestazione, corpo e piè di pagina, e l'ambito delle colonne e delle righe.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Le basi di HTML (vedi
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprensione delle problematiche di accessibilità associate alle tabelle.</li>
          <li>Aggiungere didascalie alle tabelle.</li>
          <li>Strutturare meglio la tabella con intestazione, corpo e piè di pagina.</li>
          <li>Creare ulteriori associazioni tra intestazioni e celle con gli attributi <code>scope</code>, <code>id</code>, e <code>headers</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo: Tabelle per utenti con disabilità visive

Riepiloghiamo brevemente come utilizziamo le tabelle di dati. Una tabella può essere uno strumento utile, permettendoci un accesso rapido ai dati e consentendoci di cercare diversi valori. Ad esempio, bastano pochi istanti per scoprire quanti anelli sono stati venduti a Gand durante agosto 2016. Per comprendere le informazioni, facciamo associazioni visive tra i dati in questa tabella e le relative intestazioni di colonna e/o riga.

<table>
  <caption>Articoli Venduti Agosto 2016</caption>
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

Ma cosa accade se non può fare quelle associazioni visive? Come può leggere una tabella come quella sopra? Le persone con disabilità visive spesso utilizzano un lettore di schermo che legge le informazioni sulle pagine web per loro. Questo non è un problema quando si legge testo semplice, ma interpretare una tabella può rappresentare una sfida per una persona cieca. Tuttavia, con il corretto markup possiamo sostituire le associazioni visive con quelle programmatiche.

> [!NOTE]
> Circa 253 milioni di persone vivono con disabilità visive secondo i [dati WHO del 2017](https://www.who.int/en/news-room/fact-sheets/detail/blindness-and-visual-impairment).

Questa sezione dell'articolo fornisce ulteriori tecniche per rendere le tabelle il più accessibili possibile.

### Utilizzo delle intestazioni di colonna e riga

I lettori di schermo identificheranno tutte le intestazioni e le utilizzeranno per creare associazioni programmatiche tra quelle intestazioni e le celle a cui si riferiscono. La combinazione di intestazioni di colonna e riga identificherà e interpreterà i dati in ciascuna cella in modo che gli utenti del lettore di schermo possano interpretare la tabella in modo simile a un utente vedente.

Abbiamo già trattato le intestazioni nel nostro precedente articolo — vedi [Aggiungere intestazioni con elementi \<th>](/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics#adding_headers_with_th_elements).

## Aggiungere una didascalia alla tabella con \<caption>

Può dare alla sua tabella una didascalia inserendola all'interno di un elemento {{htmlelement("caption")}} e nidificandolo all'interno dell'elemento {{htmlelement("table")}}. Dovrebbe metterla appena sotto il tag di apertura `<table>`.

```html
<table>
  <caption>
    Dinosaurs in the Jurassic period
  </caption>

  …
</table>
```

Come può dedurre dal breve esempio sopra, la didascalia è destinata a contenere una descrizione del contenuto della tabella. Questo è utile per tutti i lettori che desiderano avere un'idea rapida se la tabella è utile per loro mentre sfogliano la pagina, ma in particolare per gli utenti ciechi. Piuttosto che far leggere a un lettore di schermo il contenuto di molte celle solo per scoprire di cosa tratta la tabella, l'utente può affidarsi a una didascalia e poi decidere se leggere o meno la tabella in modo più dettagliato.

Una didascalia viene posizionata direttamente sotto il tag `<table>`.

> [!NOTE]
> L'attributo [`summary`](/it/docs/Web/HTML/Reference/Elements/table#summary) può anche essere utilizzato sull'elemento `<table>` per fornire una descrizione — questo viene anche letto dai lettori di schermo. Consigliamo tuttavia di utilizzare l'elemento `<caption>` poiché `summary` è deprecato e non può essere letto dagli utenti vedenti (non appare sulla pagina).

### Apprendimento attivo: Aggiungere una didascalia

Proviamo, utilizzando come esempio l'orario scolastico di un insegnante di lingua.

1. Faccia una copia locale del nostro file [timetable-fixed.html](https://github.com/mdn/learning-area/blob/main/html/tables/basic/timetable-fixed.html).
2. Aggiunga una didascalia adatta per la tabella.
3. Salvi il suo codice e lo apra in un browser per vedere come appare.

> [!NOTE]
> Può trovare la nostra versione su GitHub — vedi [timetable-caption.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/timetable-caption.html) ([vedilo anche in live](https://mdn.github.io/learning-area/html/tables/advanced/timetable-caption.html)).

## Aggiungere struttura con \<thead>, \<tbody>, e \<tfoot>

Man mano che le sue tabelle acquisiscono una struttura un po' più complessa, è utile fornire loro una definizione strutturale più chiara. Un modo chiaro per farlo è utilizzare {{htmlelement("thead")}}, {{htmlelement("tbody")}}, e {{htmlelement("tfoot")}}, che consentono di contrassegnare una sezione di intestazione, corpo e piè di pagina per la tabella.

Questi elementi non rendono necessariamente la tabella più accessibile agli utenti di screen reader. Non producono alcun miglioramento visivo da soli, tuttavia sono molto utili per applicare miglioramenti di stile e layout tramite CSS, che possono migliorare l'accessibilità. Per darvi alcuni esempi interessanti, nel caso di una tabella lunga, potresti fare in modo che l'intestazione e il piè di pagina della tabella si ripetano su ogni pagina stampata, e potresti fare in modo che il corpo della tabella venga visualizzato su una singola pagina con i contenuti disponibili tramite scorrimento su e giù.

Per utilizzarli, dovrebbero essere inclusi nel seguente ordine:

- L'elemento `<thead>` deve racchiudere la parte della tabella che è l'intestazione — questa è di solito la prima riga contenente le intestazioni delle colonne, ma non è sempre necessariamente così. Se si stanno utilizzando elementi {{htmlelement("col")}}/{{htmlelement("colgroup")}}, l'intestazione della tabella dovrebbe trovarsi appena sotto di essi.
- L'elemento `<tbody>` deve racchiudere la parte principale del contenuto della tabella che non è l'intestazione o il piè di pagina della tabella.
- L'elemento `<tfoot>` deve racchiudere la parte della tabella che è il piè di pagina — potrebbe essere ad esempio una riga finale con i totali delle righe precedenti.

> **Nota:** `<tbody>` è sempre incluso in ogni tabella, implicitamente se non lo specifica nel suo codice. Per verificare questo, apra uno dei suoi esempi precedenti che non include `<tbody>` e guardi il codice HTML nei suoi [strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) — vedrà che il browser ha aggiunto questo tag per lei. Potrebbe chiedersi perché dovrebbe preoccuparsi di includerlo del tutto — dovrebbe farlo perché le dà un maggiore controllo sulla struttura e sullo stile della tabella.

### Apprendimento attivo: Aggiungere struttura alla tabella

Mettiamo in pratica questi nuovi elementi.

1. Innanzitutto, faccia una copia locale di [spending-record.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/spending-record.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/minimal-table.css) in una nuova cartella.
2. Provi a mettere la riga evidente delle intestazioni all'interno di un elemento `<thead>`, la riga "SOMMA" all'interno di un elemento `<tfoot>`, e il resto del contenuto all'interno di un elemento `<tbody>`.
3. Quindi, aggiunga un attributo [`colspan`](/it/docs/Web/HTML/Reference/Elements/td#colspan) per fare in modo che la cella "SOMMA" si estenda su le prime quattro colonne, così che il numero effettivo appaia in fondo alla colonna "Costo".
4. Aggiungiamo quindi qualche semplice stile extra alla tabella, per darvi un'idea di quanto siano utili questi elementi per applicare il CSS. All'interno della sezione head del suo documento HTML, vedrà un elemento {{htmlelement("style")}} vuoto. All'interno di questo elemento, aggiungi le seguenti righe di codice CSS:

   ```css
   tbody {
     font-size: 95%;
     font-style: italic;
   }

   tfoot {
     font-weight: bold;
   }
   ```

5. Salvi e aggiorni, e dia un'occhiata al risultato. Se gli elementi `<tbody>` e `<tfoot>` non fossero presenti, dovrebbe scrivere selettori/regole molto più complessi per applicare lo stesso stile.

> [!NOTE]
> Non ci aspettiamo che comprenda completamente il CSS in questo momento. Imparerà di più quando affronterà i nostri moduli CSS ([CSS Stiling basics](/it/docs/Learn_web_development/Core/Styling_basics) è un buon punto di partenza; abbiamo anche un articolo specifico su [come stilizzare le tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables)).

La sua tabella finale dovrebbe apparire simile al seguente:

{{ EmbedGHLiveSample('learning-area/html/tables/advanced/spending-record-finished.html', '100%', 400) }}

> [!NOTE]
> Può anche trovarla su GitHub come [spending-record-finished.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/spending-record-finished.html) ([vedi questa versione dal vivo qui](https://mdn.github.io/learning-area/html/tables/advanced/spending-record-finished.html)).

## L'attributo `scope`

L'attributo [`scope`](/it/docs/Web/HTML/Reference/Elements/th#scope) può essere aggiunto all'elemento `<th>` per indicare ai lettori di schermo di quali celle l'intestazione è una intestazione — è una intestazione per la riga in cui si trova, o per la colonna, ad esempio? Tornando al nostro esempio del record di spesa, potrebbe definire in modo inequivocabile le intestazioni di colonna come intestazioni di colonna, in questo modo:

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

E ciascuna riga potrebbe avere una intestazione definita in questo modo (se aggiungessimo intestazioni di riga oltre a quelle di colonna):

```html
<tr>
  <th scope="row">Haircut</th>
  <td>Hairdresser</td>
  <td>12/09</td>
  <td>Great idea</td>
  <td>30</td>
</tr>
```

I lettori di schermo riconosceranno un markup strutturato in questo modo e consentiranno ai loro utenti di leggere l'intera colonna o riga in una volta sola.

`scope` ha altri due valori possibili — `colgroup` e `rowgroup`. Questi sono utilizzati per le intestazioni che si trovano sopra più colonne o righe. Se guarda la tabella "Articoli Venduti Agosto 2016" all'inizio di questa sezione dell'articolo, vedrà che la cella "Abbigliamento" si trova sopra le celle "Pantaloni", "Gonne" e "Vestiti". Tutte queste celle dovrebbero essere contrassegnate come intestazioni (`<th>`), ma "Abbigliamento" è una intestazione che si trova sopra e definisce le altre tre sotto-intestazioni. Pertanto, a "Abbigliamento" dovrebbe essere assegnato un attributo `scope="colgroup"`, mentre alle altre venire assegnato un attributo `scope="col"`:

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

Lo stesso vale per le intestazioni per più righe raggruppate. Guardi di nuovo la tabella "Articoli Venduti Agosto 2016", questa volta concentrandosi sulle righe con le intestazioni "Amsterdam" e "Utrecht" (`<th>`). Noterà che l'intestazione "Paesi Bassi", anch'essa contrassegnata come elemento `<th>`, si estende su entrambe le righe, essendo l'intestazione per le altre due sotto-intestazioni. Pertanto, `scope="rowgroup"` dovrebbe essere specificato su questa cella di intestazione per aiutare i lettori di schermo a creare le associazioni corrette:

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

Un'alternativa all'uso dell'attributo `scope` è utilizzare gli attributi [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) e [`headers`](/it/docs/Web/HTML/Reference/Elements/td#headers) per creare associazioni tra celle di dati e celle di intestazione.

Un elemento `<th>` può fornire un'intestazione per una cella di dati (`<td>`) o, in tabelle più complesse, per un'altra cella di intestazione (`<th>`). Questo le consente di creare intestazioni stratificate o raggruppate, dove un'intestazione descrive diverse altre.

L'attributo `headers` è utilizzato per collegare una cella, `<td>` o `<th>`, a una o più celle di intestazione. Prende un elenco separato da spazi di {{Glossary("string", "stringhe")}}; l'ordine delle stringhe non ha importanza. Ogni stringa deve corrispondere all'`id` univoco di un elemento `<th>` a cui la cella è associata.

Questo metodo fornisce alla sua tabella HTML una definizione più esplicita della posizione di ciascuna cella, basata sulle intestazioni della colonna e della riga a cui appartiene, un po' come un foglio di calcolo. Affinché ciò funzioni bene, la sua tabella dovrebbe includere sia intestazioni di colonna sia di riga.

Diamo uno sguardo a una porzione dell'esempio "Articoli Venduti Agosto 2016" per vedere come utilizzare gli attributi `id` e `headers`:

1. Aggiunga un `id` univoco a ciascun elemento `<th>` nella tabella.
2. Per le celle di intestazione: Aggiunga un attributo `headers` a ciascun elemento `<th>` che funge da sotto-intestazione, cioè una cella di intestazione con un'altra intestazione sopra. Il valore è l'`id` dell'intestazione di alto livello. Nel nostro esempio, è `"abbigliamento"` per le intestazioni di colonna e `"belgio"` per l'intestazione di riga.
3. Per le celle di dati: Aggiunga un attributo `headers` a ciascun elemento `<td>`, e aggiunga gli `id` degli elementi `<th>` associati come un elenco separato da spazi. Può procedere come farebbe in un foglio di calcolo: Trovi la cella di dati, quindi individui le intestazioni di riga e colonna che lo descrivono. L'ordine degli `id` specificati non ha importanza, ma mantenerlo coerente aiuta a tenere organizzato e migliora la leggibilità del codice.

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

- Il `<th>` per `"Belgio"` utilizza `rowspan="2"` per comprendere sia `"Anversa"` che `"Gand"`.
- Le celle di intestazione delle città (`"Anversa"` e `"Gand"`) utilizzano l'attributo `headers` per fare riferimento a `"belgio"` per mostrare che appartengono al gruppo Belgio.
- Ogni `<td>` include un attributo `headers` per il paese (`belgio`), la città (`anversa` o `gand`), il gruppo (`abbigliamento`) e l'articolo di abbigliamento specifico (`pantaloni`, `gonne` o `vestiti`).

> [!NOTE]
> Questo metodo crea associazioni molto precise tra intestazioni e celle di dati ma utilizza **molto** più markup e non lascia spazio per errori. L'approccio `scope` è di solito sufficiente per la maggior parte delle tabelle.

## Apprendimento attivo: giocare con scope e intestazioni

1. Per questo esercizio finale, prima di tutto faccia copie locali di [items-sold.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold.html) e [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/minimal-table.css), in una nuova directory.
2. Ora provi ad aggiungere gli attributi `scope` appropriati per rendere questa tabella più accessibile.
3. Infine, provi a fare un'altra copia dei file iniziali, e questa volta rendere la tabella più accessibile creando associazioni precise ed esplicite usando gli attributi `id` e `headers`.

> [!NOTE]
> Può confrontare il suo lavoro con i nostri esempi conclusi — vedi [items-sold-scope.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold-scope.html) ([vedi anche questo live](https://mdn.github.io/learning-area/html/tables/advanced/items-sold-scope.html)) e [items-sold-headers.html](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold-headers.html) ([vedi anche questo live](https://mdn.github.io/learning-area/html/tables/advanced/items-sold-headers.html)).

## Sommario

Ci sono alcune altre cose che potrebbe apprendere sulle tabelle in HTML, ma questo è quanto le serve sapere per ora. Successivamente, può mettersi alla prova con la nostra sfida sulle tabelle HTML. Buon divertimento!

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content")}}
