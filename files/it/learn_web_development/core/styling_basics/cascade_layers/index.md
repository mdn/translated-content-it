---
title: Livelli della cascata
slug: Learn_web_development/Core/Styling_basics/Cascade_layers
l10n:
  sourceCommit: d805adc1604ad27fbee00598dc81bcdfe5635bd3
---

Questa lezione mira a introdurre i [livelli della cascata](/it/docs/Web/CSS/Reference/At-rules/@layer), una funzionalità più avanzata che si basa sui concetti fondamentali della [cascata CSS](/it/docs/Web/CSS/Guides/Cascade/Introduction) e della [specificità CSS](/it/docs/Web/CSS/Guides/Cascade/Specificity).

Per chi è nuovo a CSS, affrontare questa lezione può sembrare nell'immediato meno rilevante e un po' più accademico rispetto ad altre parti del corso. Tuttavia, è utile conoscere le basi dei livelli della cascata nel caso si incontrino nei propri progetti. Con l'aumentare dell'esperienza con CSS, comprendere i livelli della cascata e sapere come sfruttarne le potenzialità eviterà molti problemi nella gestione di una base di codice CSS proveniente da soggetti, plugin e team di sviluppo differenti.

I livelli della cascata sono particolarmente rilevanti quando si lavora con CSS proveniente da più fonti, in presenza di selettori CSS in conflitto e specificità concorrenti, oppure quando si sta valutando l'uso di {{cssxref("important", "!important")}}.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una conoscenza del funzionamento di CSS, inclusi cascata e specificità (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts">Gestione dei conflitti</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come funzionano i livelli della cascata.
      </td>
    </tr>
  </tbody>
</table>

Per ogni proprietà CSS applicata a un elemento può esistere un solo valore. È possibile visualizzare tutti i valori delle proprietà applicati a un elemento ispezionandolo negli strumenti per sviluppatori del browser. Il pannello "Styles" dello strumento mostra tutti i valori delle proprietà applicati all'elemento ispezionato, insieme al selettore corrispondente e al file sorgente CSS. Il selettore proveniente dall'origine con precedenza applica i propri valori all'elemento corrispondente.

Oltre agli stili applicati, il pannello Styles mostra valori barrati che corrispondevano all'elemento selezionato ma non sono stati applicati a causa della cascata, della specificità o dell'ordine di origine. Gli stili barrati possono provenire dalla stessa origine con precedenza, ma avere una specificità inferiore, oppure avere origine e specificità corrispondenti ma essere stati trovati prima nella base di codice. Per qualsiasi valore di proprietà applicato, possono esistere diverse dichiarazioni barrate provenienti da molte fonti diverse. Se uno stile barrato ha un selettore con specificità maggiore, significa che al valore manca l'origine o l'importanza necessaria.

Spesso, con l'aumentare della complessità di un sito, aumenta il numero di fogli di stile, rendendo l'ordine di origine dei fogli di stile sia più importante sia più complesso. I livelli della cascata semplificano la manutenzione dei fogli di stile in queste basi di codice. I livelli della cascata sono contenitori di specificità espliciti che offrono un controllo più semplice e maggiore sulle dichiarazioni CSS che vengono infine applicate, consentendo agli sviluppatori web di dare priorità a sezioni di CSS senza dover combattere con la specificità.

Per comprendere i livelli della cascata, occorre comprendere bene la cascata CSS. Le sezioni seguenti forniscono un rapido riepilogo degli importanti concetti della cascata.

## Riepilogo del concetto di cascata

La "C" in CSS significa "Cascading" (a cascata). È il metodo con cui gli stili si concatenano. Lo user agent esegue diversi passaggi chiaramente definiti per determinare i valori assegnati a ogni proprietà di ogni elemento. Di seguito elencheremo brevemente questi passaggi, quindi approfondiremo il passaggio 4, **Livelli della cascata**, ovvero ciò che si è venuti qui a imparare:

1. **Rilevanza:** trovare tutti i blocchi di dichiarazione con un selettore corrispondente per ogni elemento.
2. **Importanza:** ordinare le regole in base al fatto che siano normali o importanti. Gli stili importanti sono quelli con il flag {{cssxref("important", "!important")}} impostato.
3. **Origine:** all'interno di ciascuno dei due gruppi di importanza, ordinare le regole in base all'origine dell'autore, dell'utente o dello user agent.
4. **Livelli della cascata:** all'interno di ciascuno dei sei gruppi di origine e importanza, ordinare in base al livello della cascata. L'ordine dei livelli per le dichiarazioni normali va dal primo livello creato all'ultimo, seguito dagli stili normali non inseriti in un livello. Questo ordine è invertito per gli stili importanti, per i quali gli stili importanti non inseriti in un livello hanno la precedenza più bassa.
5. **Specificità:** per gli stili concorrenti nel livello di origine con precedenza, ordinare le dichiarazioni in base alla [specificità](/it/docs/Web/CSS/Guides/Cascade/Specificity).
6. **Prossimità dell'ambito**: quando due selettori nel livello di origine con precedenza hanno la stessa specificità, vince il valore della proprietà all'interno delle regole con ambito che ha il minor numero di passaggi nella gerarchia DOM fino alla radice dell'ambito. Per maggiori dettagli e un esempio, vedere [Come vengono risolti i conflitti di `@scope`](/it/docs/Web/CSS/Reference/At-rules/@scope#how_scope_conflicts_are_resolved).
7. **Ordine di apparizione:** quando due selettori nel livello di origine con precedenza hanno la stessa specificità e prossimità dell'ambito, vince il valore della proprietà dell'ultimo selettore dichiarato con la massima specificità.

A ogni passaggio, soltanto le dichiarazioni ancora "in gara" passano a "competere" nel passaggio successivo. Se rimane in gara una sola dichiarazione, questa "vince" e i passaggi successivi diventano irrilevanti.

### Origine e cascata

Esistono tre [tipi di origine della cascata](/it/docs/Web/CSS/Guides/Cascade/Introduction#origin_types): fogli di stile dello user agent, fogli di stile dell'utente e fogli di stile dell'autore. Il browser ordina ogni dichiarazione in sei gruppi di origine in base all'origine e all'importanza. Esistono otto livelli di precedenza: i sei gruppi di origine, le proprietà in transizione e le proprietà in animazione. L'ordine di precedenza va dagli stili normali dello user agent, che hanno la precedenza più bassa, agli stili nelle animazioni attualmente applicate, agli stili importanti dello user agent e infine agli stili in transizione, che hanno la precedenza più alta:

1. stili normali dello user agent
2. stili normali dell'utente
3. stili normali dell'autore
4. stili in animazione
5. stili importanti dell'autore
6. stili importanti dell'utente
7. stili importanti dello user agent
8. stili in transizione

Lo "user agent" è il browser. L'"utente" è il visitatore del sito. L'"autore" è lo sviluppatore. Gli stili dichiarati direttamente su un elemento con l'elemento {{HTMLElement('style')}} sono stili dell'autore. Escludendo gli stili in animazione e in transizione, gli stili normali dello user agent hanno la precedenza più bassa; gli stili importanti dello user agent hanno la precedenza più alta.

### Origine e specificità

Per ogni proprietà, la dichiarazione che "vince" è quella proveniente dall'origine con precedenza in base al peso, normale o importante. Ignorando momentaneamente i livelli, viene applicato il valore dell'origine con la precedenza più alta. Se l'origine vincente ha più di una dichiarazione di proprietà per un elemento, viene confrontata la [specificità](/it/docs/Web/CSS/Guides/Cascade/Specificity) dei selettori per questi valori di proprietà concorrenti. La specificità non viene mai confrontata tra selettori provenienti da origini diverse.

Nell'esempio seguente sono presenti due link. Al primo non vengono applicati stili dell'autore, quindi vengono applicati solo gli stili dello user agent, oltre agli eventuali stili personali dell'utente. Al secondo vengono impostati {{cssxref("text-decoration")}} e {{cssxref("color")}} dagli stili dell'autore, anche se il selettore nel foglio di stile dell'autore ha una specificità di [`0-0-0`](/it/docs/Web/CSS/Guides/Cascade/Specificity#selector_weight_categories). Gli stili dell'autore "vincono" perché, quando esistono stili in conflitto provenienti da origini diverse, vengono applicate le regole dell'origine con precedenza, indipendentemente dalla specificità nell'origine che non ha precedenza.

```html live-sample___basic-cascade
<p><a href="https://example.org">User agent styles</a></p>
<p><a class="author" href="https://example.org">Author styles</a></p>
```

```css live-sample___basic-cascade
:where(a.author) {
  text-decoration: overline;
  color: red;
}
```

{{EmbedLiveSample("basic-cascade")}}

Il selettore "concorrente" nel foglio di stile dello user agent al momento della stesura è `a:any-link`, che ha un peso di specificità di `0-1-1`. Sebbene questo sia maggiore del selettore `0-0-0` nel foglio di stile dell'autore, anche se il selettore nello user agent attuale è diverso, non importa: i pesi di specificità delle origini dell'autore e dello user agent non vengono mai confrontati. Per ulteriori informazioni, vedere [come viene calcolato il peso di specificità](/it/docs/Web/CSS/Guides/Cascade/Specificity#how_is_specificity_calculated).

La precedenza dell'origine vince sempre sulla specificità del selettore. Se una proprietà di un elemento viene definita con una dichiarazione di stile normale in più origini, il foglio di stile dell'autore sovrascriverà sempre le proprietà normali ridondanti dichiarate in un foglio di stile dell'utente o dello user agent. Se lo stile è importante, il foglio di stile dello user agent vincerà sempre sugli stili dell'autore e dell'utente. La precedenza dell'origine della cascata garantisce che i conflitti di specificità tra origini non si verifichino mai.

Un'ultima cosa da notare prima di proseguire: l'ordine di apparizione diventa rilevante soltanto quando le dichiarazioni concorrenti nell'origine con precedenza hanno la stessa specificità.

## Panoramica dei livelli della cascata

Ora si comprende la "precedenza dell'origine della cascata", ma che cos'è la "precedenza del livello della cascata"? Risponderemo a questa domanda affrontando cosa sono i livelli della cascata, come vengono ordinati e come gli stili vengono assegnati ai livelli della cascata. Verranno trattati i [livelli regolari](#creazione_di_livelli_della_cascata), i [livelli annidati](#panoramica_dei_livelli_della_cascata_annidati) e i livelli anonimi. Iniziamo discutendo cosa sono i livelli della cascata e quali problemi risolvono.

### Ordine di precedenza dei livelli della cascata

Analogamente ai sei livelli di priorità basati su origine e importanza, i livelli della cascata consentono di creare un sottolivello di priorità dell'origine all'interno di una qualsiasi di tali origini.

All'interno di ciascuno dei sei gruppi di origine possono esistere più livelli della cascata. L'[ordine di creazione dei livelli](/it/docs/Web/CSS/Reference/At-rules/@layer) è molto importante. È l'ordine di creazione a stabilire l'ordine di precedenza tra i livelli all'interno di un'origine.

Nei gruppi di origine normali, i livelli vengono ordinati nell'ordine di creazione di ciascun livello. L'ordine di precedenza va dal primo livello creato all'ultimo, seguito dagli stili normali non inseriti in un livello.

Questo ordine è invertito per gli stili importanti. Tutti gli stili importanti non inseriti in un livello confluiscono insieme in un livello implicito che ha precedenza su tutti gli stili normali non in transizione. Gli stili importanti non inseriti in un livello hanno precedenza inferiore rispetto a qualsiasi stile importante inserito in un livello. Gli stili importanti nei livelli dichiarati prima hanno precedenza sugli stili importanti nei livelli dichiarati successivamente all'interno della stessa origine.

Per il resto di questo tutorial, la discussione sarà limitata agli stili dell'autore, ma occorre tenere presente che i livelli possono esistere anche nei fogli di stile dell'utente e dello user agent.

### Problemi che i livelli della cascata possono risolvere

Le grandi basi di codice possono contenere stili provenienti da più team, librerie di componenti, framework e terze parti. Indipendentemente dal numero di fogli di stile inclusi, tutti questi stili confluiscono in un'unica origine: il foglio di stile dell'_autore_.

Avere stili provenienti da molte fonti che confluiscono insieme, specialmente da team che non lavorano insieme, può creare problemi. Team diversi possono adottare metodologie diverse; uno può considerare buona pratica ridurre la specificità, mentre un altro può avere come standard l'inclusione di un `id` in ogni selettore.

I conflitti di specificità possono intensificarsi rapidamente. Uno sviluppatore web può creare una "soluzione rapida" aggiungendo un flag `!important`. Sebbene possa sembrare una soluzione facile, spesso sposta soltanto la guerra della specificità dalle dichiarazioni normali a quelle importanti.

Allo stesso modo in cui le origini della cascata offrono un equilibrio di potere tra stili dell'utente, dello user agent e dell'autore, i livelli della cascata forniscono un modo strutturato per organizzare e bilanciare le esigenze all'interno di una singola origine, come se ogni livello di un'origine fosse una sotto-origine. È possibile creare un livello per ogni team, componente e terza parte, con la precedenza degli stili basata sull'ordine dei livelli.

Le regole all'interno di un livello confluiscono insieme senza competere con le regole di stile esterne al livello. I livelli della cascata consentono di dare priorità a interi fogli di stile rispetto ad altri fogli di stile, senza doversi preoccupare della specificità tra queste sotto-origini.

La precedenza del livello vince sempre sulla specificità del selettore. Gli stili nei livelli con precedenza "vincono" sui livelli con minore precedenza. La specificità di un selettore in un livello perdente è irrilevante. La specificità è ancora importante per i valori di proprietà concorrenti all'interno di un livello, ma non esistono problemi di specificità tra livelli poiché viene considerato soltanto il livello con la priorità più alta per ciascuna proprietà.

### Problemi che i livelli della cascata annidati possono risolvere

I livelli della cascata consentono la creazione di livelli annidati. Ogni livello della cascata può contenere livelli annidati.

Ad esempio, una libreria di componenti può essere importata in un livello `components`. Un livello della cascata regolare aggiungerà la libreria di componenti all'origine dell'autore, eliminando eventuali conflitti di specificità con altri stili dell'autore. All'interno del livello `components`, uno sviluppatore può scegliere di definire vari temi, ciascuno come livello annidato separato. L'ordine di questi livelli di tema annidati può essere definito in base alle media query (vedere la sezione [Creazione dei livelli e media query](#creazione_dei_livelli_e_media_query) più avanti), come la dimensione del viewport o l'[orientamento](/it/docs/Web/CSS/Reference/At-rules/@media/orientation). Questi livelli annidati offrono un modo per creare temi che non entrano in conflitto in base alla specificità.

La possibilità di annidare i livelli è molto utile per chiunque lavori allo sviluppo di librerie di componenti, framework, widget di terze parti e temi.

La possibilità di creare livelli annidati elimina inoltre il problema di avere nomi di livelli in conflitto. Questo aspetto verrà trattato nella sezione sui [livelli annidati](#panoramica_dei_livelli_della_cascata_annidati).

> "Gli autori possono creare livelli per rappresentare valori predefiniti degli elementi, librerie di terze parti, temi, componenti, sovrascritture e altre esigenze di stile, e sono in grado di riordinare la cascata dei livelli in modo esplicito, senza modificare selettori o specificità all'interno di ciascun livello, né fare affidamento sull'ordine di apparizione per risolvere i conflitti tra livelli."
>
> —[Specifica Cascading and Inheritance](https://drafts.csswg.org/css-cascade-5/#layering).

## Creazione di livelli della cascata

I livelli possono essere creati usando uno dei seguenti metodi:

- L'at-rule statement {{cssxref("@layer")}}, che dichiara livelli usando `@layer` seguito dai nomi di uno o più livelli. Questo crea livelli con nome senza assegnarvi stili.
- L'at-rule block `@layer`, nel quale tutti gli stili all'interno di un blocco vengono aggiunti a un livello con nome o senza nome.
- La regola {{cssxref("@import")}} con la parola chiave `layer` o la funzione `layer()`, che assegna il contenuto del file importato a quel livello.

Tutti e tre i metodi creano un livello se un livello con quel nome non è stato già inizializzato. Se non viene fornito alcun nome di livello nell'at-rule `@layer` o in `@import` con `layer()`, viene creato un nuovo livello anonimo, senza nome.

> [!NOTE]
> L'ordine di precedenza dei livelli corrisponde all'ordine in cui vengono creati. Gli stili non appartenenti a un livello, ovvero gli "stili non inseriti in un livello", confluiscono insieme in un livello implicito finale.

Analizziamo più nel dettaglio i tre modi per creare un livello prima di discutere dei livelli annidati.

### L'at-rule statement @layer per livelli con nome

L'ordine dei livelli viene stabilito dall'ordine in cui i livelli appaiono nel CSS. Dichiarare livelli usando `@layer` seguito dai nomi di uno o più livelli senza assegnare loro alcuno stile è un modo per definire l'[ordine dei livelli](#determinazione_della_precedenza_in_base_all'ordine_dei_livelli).

L'at-rule CSS {{cssxref("@layer")}} viene usata per dichiarare un livello della cascata e per definire l'ordine di precedenza quando sono presenti più livelli della cascata. La seguente at-rule dichiara tre livelli nell'ordine elencato:

```css
@layer theme, layout, utilities;
```

Spesso sarà opportuno che la prima riga del CSS sia questa dichiarazione `@layer`, naturalmente con nomi di livello appropriati per il sito, per avere il pieno controllo dell'ordinamento dei livelli.

Se l'istruzione precedente è la prima riga del CSS di un sito, l'ordine dei livelli sarà `theme`, `layout` e `utilities`. Se alcuni livelli sono stati creati prima dell'istruzione precedente, purché non esistano già livelli con questi nomi, i tre livelli verranno creati e aggiunti alla fine dell'elenco dei livelli esistenti. Tuttavia, se esiste già un livello con lo stesso nome, l'istruzione precedente creerà soltanto due nuovi livelli. Ad esempio, se `layout` esiste già, verranno creati soltanto `theme` e `utilities`, ma l'ordine dei livelli, in questo caso, sarà `layout`, `theme` e `utilities`.

### L'at-rule block @layer per livelli con nome e anonimi

I livelli possono essere creati usando l'at-rule block `@layer`. Se un'at-rule `@layer` è seguita da un identificatore e da un blocco di stili, l'identificatore viene usato per assegnare un nome al livello e gli stili nell'at-rule vengono aggiunti agli stili del livello. Se non esiste già un livello con il nome specificato, viene creato un nuovo livello. Se esiste già un livello con il nome specificato, gli stili vengono aggiunti al livello esistente in precedenza. Se non viene specificato alcun nome durante la creazione di un blocco di stili usando `@layer`, gli stili nell'at-rule vengono aggiunti a un nuovo livello anonimo.

Nell'esempio seguente vengono usate quattro at-rule block `@layer` e una at-rule statement `@layer`. Questo CSS esegue le seguenti operazioni nell'ordine elencato:

1. Crea un livello `layout` con nome
2. Crea un livello anonimo senza nome
3. Dichiara un elenco di tre livelli e crea soltanto due nuovi livelli, `theme` e `utilities`, poiché `layout` esiste già
4. Aggiunge ulteriori stili al livello `layout` già esistente
5. Crea un secondo livello anonimo senza nome

```css
/* file: layers1.css */

/* unlayered styles */
body {
  color: #333333;
}

/* creates the first layer: `layout` */
@layer layout {
  main {
    display: grid;
  }
}

/* creates the second layer: an unnamed, anonymous layer */
@layer {
  body {
    margin: 0;
  }
}

/* creates the third and fourth layers: `theme` and `utilities` */
@layer theme, layout, utilities;

/* adds styles to the already existing `layout` layer */
@layer layout {
  main {
    color: black;
  }
}

/* creates the fifth layer: an unnamed, anonymous layer */
@layer {
  body {
    margin: 1vw;
  }
}
```

Nel CSS precedente sono stati creati cinque livelli: `layout`, `<anonymous(01)>`, `theme`, `utilities` e `<anonymous(02)>`, in questo ordine, con un sesto livello implicito di stili non inseriti in un livello contenuto nel blocco di stile `body`. L'ordine dei livelli corrisponde all'ordine in cui vengono creati, con il livello implicito degli stili non inseriti in un livello sempre per ultimo. Non è possibile modificare l'ordine dei livelli dopo la loro creazione.

Alcuni stili sono stati assegnati al livello denominato `layout`. Se un livello con nome non esiste già, specificarne il nome in un'at-rule `@layer`, assegnando o meno stili al livello, crea il livello; ciò aggiunge il livello alla fine della serie di nomi di livello esistenti. Se il livello con nome esiste già, tutti gli stili all'interno del blocco con nome vengono aggiunti agli stili nel livello esistente in precedenza: specificare stili in un blocco riutilizzando un nome di livello esistente non crea un nuovo livello.

I livelli anonimi vengono creati assegnando stili a un livello senza assegnare un nome al livello. Gli stili possono essere aggiunti a un livello senza nome soltanto al momento della sua creazione.

> [!NOTE]
> Gli utilizzi successivi di `@layer` senza nome di livello creano ulteriori livelli senza nome; non aggiungono stili a un livello senza nome già esistente.

L'at-rule `@layer` crea un livello, con nome o meno, oppure aggiunge stili a un livello se il livello con nome esiste già. Il primo livello anonimo è stato chiamato `<anonymous(01)>` e il secondo `<anonymous(02)>` soltanto per poterli spiegare. In realtà sono livelli senza nome. Non esiste alcun modo per farvi riferimento o aggiungere ulteriori stili.

Tutti gli stili dichiarati al di fuori di un livello vengono riuniti in un livello implicito. Nel codice di esempio precedente, la prima dichiarazione ha impostato la proprietà `color: #333333` su `body`. Questa dichiarazione era esterna a qualsiasi livello. Le dichiarazioni normali non inserite in un livello hanno precedenza sulle dichiarazioni normali inserite in un livello, anche se gli stili non inseriti in un livello hanno una specificità inferiore e appaiono per primi nell'ordine di apparizione. Ciò spiega perché, anche se il CSS non inserito in un livello è stato dichiarato per primo nel blocco di codice, il livello implicito contenente questi stili non inseriti in un livello ha precedenza come se fosse l'ultimo livello dichiarato.

Nella riga `@layer theme, layout, utilities;`, in cui è stata dichiarata una serie di livelli, sono stati creati soltanto i livelli `theme` e `utilities`; `layout` era già stato creato nella prima riga. Questa dichiarazione non modifica l'ordine dei livelli già creati. Attualmente non esiste alcun modo per riordinare i livelli dopo la dichiarazione.

Nell'esempio seguente, gli stili vengono assegnati a due livelli, creandoli e assegnando loro un nome nel processo. Poiché esistono già, essendo stati creati al primo utilizzo, dichiararli nell'ultima riga non ha alcun effetto.

```html live-sample___layer-order
<h1>Is this heading underlined?</h1>
```

```css live-sample___layer-order
@layer page {
  h1 {
    text-decoration: overline;
    color: red;
  }
}

@layer site {
  h1 {
    text-decoration: underline;
    color: green;
  }
}

/* this does nothing */
@layer site, page;
```

{{EmbedLiveSample("layer-order")}}

Provare a spostare l'ultima riga, `@layer site, page;`, in modo che diventi la prima riga. Cosa accade?

#### Creazione dei livelli e media query

Se viene definito un livello usando query [media](/it/docs/Web/CSS/Guides/Media_queries/Using) o [feature](/it/docs/Web/CSS/Guides/Conditional_rules/Using_feature_queries), e il media non corrisponde oppure la funzionalità non è supportata, il livello non viene creato. L'esempio seguente mostra come modificare la dimensione del dispositivo o del browser possa modificare l'ordine dei livelli. In questo esempio, il livello `site` viene creato soltanto nei browser più larghi. Gli stili vengono quindi assegnati ai livelli `page` e `site`, in questo ordine.

```html live-sample___media-order
<h1>Is this heading underlined?</h1>
```

```css live-sample___media-order
@media (width >= 50em) {
  @layer site;
}

@layer page {
  h1 {
    text-decoration: overline;
    color: red;
  }
}

@layer site {
  h1 {
    text-decoration: underline;
    color: green;
  }
}
```

{{EmbedLiveSample("media-order")}}

Sugli schermi larghi, il livello `site` viene dichiarato nella prima riga, il che significa che `site` ha una precedenza inferiore rispetto a `page`. Altrimenti, `site` ha precedenza su `page` perché viene dichiarato più tardi sugli schermi stretti. Se non funziona, provare a modificare `50em` nella media query in `10em` o `100em`.

### Importazione di fogli di stile in livelli con nome e anonimi con @import

La regola {{cssxref("@import")}} consente agli utenti di importare regole di stile da altri fogli di stile direttamente in un file CSS o in un elemento {{htmlelement('style')}}.

Quando si importano fogli di stile, l'istruzione `@import` deve essere definita prima di qualsiasi stile CSS all'interno del foglio di stile o del blocco `<style>`. L'istruzione `@import` deve comparire per prima, prima di qualsiasi stile, ma può essere preceduta da un'at-rule `@layer` che crea uno o più livelli senza assegnare stili ai livelli. (`@import` può anche essere preceduta da una regola {{cssxref("@charset")}}.)

È possibile importare un foglio di stile in un livello con nome, in un livello con nome annidato o in un livello anonimo. Il seguente livello importa i fogli di stile rispettivamente in un livello `components`, in un livello `dialog` annidato all'interno del livello `components` e in un livello senza nome:

```css
@import "components-lib.css" layer(components);
@import "dialog.css" layer(components.dialog);
@import "marketing.css" layer();
```

È possibile importare più di un file CSS in un singolo livello. La dichiarazione seguente importa due file separati in un unico livello `social`:

```css
@import "comments.css" layer(social);
@import "sm-icons.css" layer(social);
```

È possibile importare stili e creare livelli in base a condizioni specifiche usando [media query](/it/docs/Web/CSS/Guides/Media_queries/Using) e [feature query](/it/docs/Web/CSS/Guides/Conditional_rules/Using_feature_queries). Il seguente esempio importa un foglio di stile in un livello `international` soltanto se il browser supporta `display: ruby` e il file importato dipende dalla larghezza dello schermo.

```css
@import "ruby-narrow.css" layer(international) supports(display: ruby)
  (width < 32rem);
@import "ruby-wide.css" layer(international) supports(display: ruby)
  (width >= 32rem);
```

> [!NOTE]
> Non esiste un equivalente del metodo {{HTMLElement('link')}} per collegare i fogli di stile. Usare `@import` per importare un foglio di stile in un livello quando non è possibile usare `@layer` all'interno del foglio di stile.

## Panoramica dei livelli della cascata annidati

I livelli annidati sono livelli all'interno di un livello con nome o anonimo. Ogni livello della cascata, anche uno anonimo, può contenere livelli annidati. I livelli importati in un altro livello diventano livelli annidati all'interno di quel livello.

### Vantaggi dell'annidamento dei livelli

La possibilità di annidare i livelli consente ai team di creare livelli della cascata senza preoccuparsi che altri team li importino in un livello. Analogamente, l'annidamento consente di importare fogli di stile di terze parti in un livello senza preoccuparsi se quel foglio di stile contiene a sua volta livelli. Poiché i livelli possono essere annidati, non è necessario preoccuparsi di avere nomi di livello in conflitto tra fogli di stile esterni e interni.

### Creazione di livelli della cascata annidati

I livelli annidati possono essere creati usando gli stessi metodi descritti per i livelli regolari. Ad esempio, possono essere creati usando l'at-rule `@layer` seguita dai nomi di uno o più livelli, usando una notazione con punto. Più punti e nomi di livelli indicano più livelli di annidamento.

Se un'at-rule block `@layer` viene annidata all'interno di un'altra at-rule block `@layer`, con o senza nome, il blocco annidato diventa un livello annidato. Analogamente, quando un foglio di stile viene importato con una dichiarazione `@import` contenente la parola chiave `layer` o la funzione `layer()`, gli stili vengono assegnati a quel livello con nome o anonimo. Se l'istruzione `@import` contiene livelli, tali livelli diventano livelli annidati all'interno del livello anonimo o con nome.

Consideriamo il seguente esempio:

```css
@import "components-lib.css" layer(components);
@import "narrow-theme.css" layer(components.narrow);
```

Nella prima riga, `components-lib.css` viene importato nel livello `components`. Se tale file contiene livelli, con nome o meno, quei livelli diventano livelli annidati all'interno del livello `components`.

La seconda riga importa `narrow-theme.css` nel livello `narrow`, che è un sottolivello di `components`. Il livello annidato `components.narrow` viene creato come ultimo livello all'interno del livello `components`, a meno che `components-lib.css` contenga già un livello `narrow`; in tal caso, il contenuto di `narrow-theme.css` verrebbe aggiunto al livello annidato `components.narrow`. È possibile aggiungere ulteriori livelli annidati con nome al livello `components` usando il modello `components.<layerName>`. Come indicato in precedenza, i livelli senza nome possono essere creati ma non vi si può accedere successivamente.

Consideriamo un altro esempio, in cui [viene importato `layers1.css` in un livello con nome](#the_layer_block_at-rule_for_named_and_anonymous_layers) usando la seguente istruzione:

```css
@import "layers1.css" layer(example);
```

Questo creerà un singolo livello denominato `example` contenente alcune dichiarazioni e cinque livelli annidati: `example.layout`, `example.<anonymous(01)>`, `example.theme`, `example.utilities` e `example.<anonymous(02)>`.

Per aggiungere stili a un livello annidato con nome, usare la notazione con punto:

```css
@layer example.layout {
  main {
    width: 50vw;
  }
}
```

## Determinazione della precedenza in base all'ordine dei livelli

L'ordine dei livelli determina il loro ordine di precedenza. Pertanto, l'ordine dei livelli è molto importante. Allo stesso modo in cui la cascata ordina in base a origine e importanza, la cascata ordina ciascuna dichiarazione CSS in base al livello di origine e all'importanza.

### Ordine di precedenza dei livelli della cascata regolari

```css
@import "A.css" layer(firstLayer);
@import "B.css" layer(secondLayer);
@import "C.css";
```

Il codice precedente crea due livelli con nome, mentre gli stili di C.css vengono aggiunti al livello implicito degli stili non inseriti in un livello. Si supponga che i tre file, `A.css`, `B.css` e `C.css`, non contengano ulteriori livelli. L'elenco seguente mostra dove vengono ordinati gli stili dichiarati all'interno e all'esterno di questi file, dalla precedenza più bassa (1) alla più alta (10).

1. stili normali di `firstLayer` (`A.css`)
2. stili normali di `secondLayer` (`B.css`)
3. stili normali non inseriti in un livello (`C.css`)
4. stili normali inline
5. stili in animazione
6. stili importanti non inseriti in un livello (`C.css`)
7. stili importanti di `secondLayer` (`B.css`)
8. stili importanti di `firstLayer` (`A.css`)
9. stili importanti inline
10. stili in transizione

Gli stili normali dichiarati all'interno dei livelli ricevono la priorità più bassa e vengono ordinati in base all'ordine in cui i livelli sono stati creati. Gli stili normali nel primo livello creato hanno la precedenza più bassa, mentre gli stili normali nel livello creato per ultimo hanno la precedenza più alta tra i livelli. In altre parole, gli stili normali dichiarati all'interno di `firstLayer` verranno sovrascritti da qualsiasi stile successivo nell'elenco in caso di conflitto.

Seguono tutti gli stili dichiarati al di fuori dei livelli. Gli stili in `C.css` non sono stati importati in un livello e sovrascriveranno qualsiasi stile in conflitto da `firstLayer` e `secondLayer`. Gli stili non dichiarati in un livello hanno sempre precedenza più alta rispetto agli stili che _sono_ dichiarati all'interno di un livello, a eccezione degli stili importanti.

Gli stili inline vengono dichiarati usando l'[attributo `style`](/it/docs/Web/HTML/Reference/Global_attributes/style). Gli stili normali dichiarati in questo modo hanno precedenza sugli stili normali presenti nei fogli di stile non inseriti in un livello e in quelli inseriti in un livello, ovvero `firstLayer – A.css`, `secondLayer – B.css` e `C.css`.

Gli stili in animazione hanno precedenza più alta di tutti gli stili normali, inclusi gli stili normali inline.

Gli stili importanti, ovvero i valori di proprietà che includono il flag `!important`, hanno precedenza rispetto a tutti gli stili menzionati precedentemente nell'elenco. Vengono ordinati nell'ordine inverso rispetto agli stili normali. Gli stili importanti dichiarati al di fuori di un livello hanno una precedenza inferiore rispetto a quelli dichiarati all'interno di un livello. Anche gli stili importanti presenti nei livelli vengono ordinati in base all'ordine di creazione dei livelli. Per gli stili importanti, l'ultimo livello creato ha la precedenza più bassa e il primo livello creato ha la precedenza più alta tra i livelli dichiarati.

Gli stili importanti inline hanno nuovamente precedenza più alta rispetto agli stili importanti dichiarati altrove.

Gli stili in transizione hanno la precedenza più alta. Quando un valore di proprietà normale è in transizione, ha precedenza su tutte le altre dichiarazioni di valore della proprietà, compresi gli stili importanti inline, ma soltanto durante la transizione.

```html live-sample___layer-precedence
<div>
  <h1 style="color: yellow; background-color: maroon !important;">
    Inline styles
  </h1>
</div>
```

```css live-sample___layer-precedence
@layer A, B;

h1 {
  font-family: sans-serif;
  margin: 1em;
  padding: 0.2em;
  color: orange;
  background-color: green;
  text-decoration: overline pink !important;
  box-shadow: 5px 5px lightgreen !important;
}

@layer A {
  h1 {
    color: grey;
    background-color: black !important;
    text-decoration: line-through grey;
    box-shadow: -5px -5px lightblue !important;
    font-style: normal;
    font-weight: normal !important;
  }
}

@layer B {
  h1 {
    color: aqua;
    background: yellow !important;
    text-decoration: underline aqua;
    box-shadow: -5px 5px magenta !important;
    font-style: italic;
    font-weight: bold !important;
  }
}
```

{{EmbedLiveSample("layer-precedence")}}

In questo esempio, due livelli, `A` e `B`, vengono inizialmente definiti usando un'at-rule statement `@layer` senza stili. Gli stili dei livelli vengono definiti in due at-rule block `@layer` che appaiono dopo la regola CSS `h1` dichiarata al di fuori di qualsiasi livello.

Gli stili inline aggiunti sull'elemento `h1` usando l'attributo `style` impostano un `color` normale e un `background-color` importante. Gli stili normali inline sovrascrivono tutti gli stili normali inseriti o meno in un livello. Gli stili importanti inline sovrascrivono tutti gli stili dell'autore normali e importanti inseriti o meno in un livello. Non esiste alcun modo per gli stili dell'autore di sovrascrivere gli stili importanti inline.

`text-decoration` normale e `box-shadow` importante non fanno parte degli stili inline `style` e possono quindi essere sovrascritti. Per gli stili normali non inline, gli stili non inseriti in un livello hanno precedenza. Per gli stili importanti, è importante anche l'ordine dei livelli. Sebbene gli stili normali non inseriti in un livello sovrascrivano tutti gli stili normali impostati in un livello, con gli stili importanti l'ordine di precedenza è invertito: gli stili importanti non inseriti in un livello hanno una precedenza inferiore rispetto agli stili inseriti in un livello.

I due stili dichiarati soltanto all'interno dei livelli sono `font-style`, con importanza normale, e `font-weight`, con un flag `!important`. Per gli stili normali, il livello `B`, dichiarato per ultimo, sovrascrive gli stili nel livello `A` dichiarato prima. Per gli stili normali, i livelli successivi hanno precedenza sui livelli precedenti. L'ordine di precedenza è invertito per gli stili importanti. Per le dichiarazioni `font-weight` importanti, il livello `A`, dichiarato per primo, ha precedenza sul livello `B` dichiarato per ultimo.

È possibile invertire l'ordine dei livelli modificando la prima riga da `@layer A, B;` a `@layer B, A;`. Provare a farlo. Quali stili vengono modificati e quali restano uguali? Perché?

L'ordine dei livelli viene stabilito dall'ordine in cui i livelli appaiono nel CSS. Nella prima riga, i livelli sono stati dichiarati senza assegnare alcuno stile usando `@layer` seguito dai nomi dei livelli e terminando con un punto e virgola. Se questa riga fosse stata omessa, i risultati sarebbero stati gli stessi. Perché? Le regole di stile sono state assegnate in blocchi `@layer` con nome nell'ordine A e poi B. I due livelli sono stati creati in quella prima riga. Se non fossero stati creati allora, questi blocchi di regole li avrebbero creati in quell'ordine.

Quella prima riga è stata inclusa per due motivi: il primo, per poter modificare facilmente la riga e invertire l'ordine; il secondo, perché spesso dichiarare in anticipo l'ordine dei livelli è considerata una buona pratica per la gestione dell'ordine dei livelli.

In sintesi:

- L'ordine di precedenza dei livelli corrisponde all'ordine in cui vengono creati.
- Una volta creati, non è possibile modificare l'ordine dei livelli.
- La precedenza dei livelli per gli stili normali segue l'ordine in cui i livelli vengono creati.
- Gli stili normali non inseriti in un livello hanno precedenza sugli stili normali inseriti in un livello.
- La precedenza dei livelli per gli stili importanti è invertita, con i livelli creati prima che hanno precedenza.
- Tutti gli stili importanti inseriti in un livello hanno precedenza sugli stili importanti, e normali, non inseriti in un livello.
- Gli stili normali inline hanno precedenza su tutti gli stili normali, inseriti o meno in un livello.
- Gli stili importanti inline hanno precedenza su tutti gli altri stili, a eccezione degli stili in transizione.
- Non esiste alcun modo per gli stili dell'autore di sovrascrivere gli stili importanti inline, se non applicando una transizione, che è temporanea.

### Ordine di precedenza dei livelli della cascata annidati

L'ordine di precedenza della cascata per i livelli annidati è simile a quello dei livelli regolari, ma è contenuto all'interno del livello. L'ordine di precedenza si basa sull'ordine di creazione dei livelli annidati. Gli stili non annidati in un livello hanno precedenza sugli stili normali annidati, con l'ordine di precedenza invertito per gli stili importanti. Il peso di specificità tra livelli annidati non è rilevante, sebbene lo sia per gli stili in conflitto all'interno di un livello annidato.

Il seguente esempio crea e aggiunge stili al livello `components`, al livello annidato `components.narrow` e al livello annidato `components.wide`:

```html hidden
<div>Text</div>
```

```css hidden
div {
  height: 150px;
  width: 150px;
  margin: 1rem;
  padding: 1rem;
  font-size: 3rem;
}
```

```css
div {
  background-color: wheat;
  color: pink !important;
}

@layer components {
  div {
    background-color: yellow;
    border: 1rem dashed red;
    color: orange !important;
  }
}

@layer components.narrow {
  div {
    background-color: skyblue;
    border: 1rem dashed blue;
    color: purple !important;
    border-radius: 50%;
  }
}

@layer components.wide {
  div {
    background-color: limegreen;
    border: 1rem dashed green;
    color: seagreen !important;
    border-radius: 20%;
  }
}
```

{{EmbedLiveSample("Precedence order of nested cascade layers", "100%", "250")}}

Ecco un riepilogo delle proprietà utilizzate e del motivo per cui viene applicata ciascuna dichiarazione:

- `background-color`: poiché gli stili normali non inseriti in un livello hanno precedenza sugli stili normali inseriti in un livello, vince il colore `wheat`.
- `border`: poiché all'interno di un livello gli stili non annidati hanno precedenza sugli stili normali annidati, vince il colore `red`.
- `color`: con gli stili importanti, gli stili inseriti in un livello hanno precedenza sugli stili non inseriti in un livello, con gli stili importanti nei livelli dichiarati prima che hanno precedenza su quelli nei livelli dichiarati dopo. In questo esempio, l'ordine di creazione dei livelli annidati è `components.narrow`, poi `components.wide`, quindi gli stili importanti in `components.narrow` hanno precedenza sugli stili importanti in `components.wide`, il che significa che vince il colore `purple`.
- `border-radius`: la proprietà è stata impostata soltanto nei livelli annidati, quindi in base all'ordine delle dichiarazioni vince il raggio `20%`.

## Riepilogo

Se è stata compresa la maggior parte di questo articolo, ottimo lavoro: ora si conoscono i meccanismi fondamentali dei livelli della cascata CSS.
