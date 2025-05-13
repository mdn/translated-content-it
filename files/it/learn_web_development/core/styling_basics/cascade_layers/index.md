---
title: Cascade layers
slug: Learn_web_development/Core/Styling_basics/Cascade_layers
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questa lezione mira a introdurle [i livelli di cascata](/it/docs/Web/CSS/@layer), una funzionalità più avanzata che si basa sui concetti fondamentali della [cascata CSS](/it/docs/Web/CSS/CSS_cascade/Cascade) e della [specificità CSS](/it/docs/Web/CSS/CSS_cascade/Specificity).

Se è nuovo al CSS, il lavoro attraverso questa lezione potrebbe sembrare meno rilevante immediatamente e un po' più accademico rispetto ad altre parti del corso. Tuttavia, è utile sapere i fondamenti su cosa siano i livelli di cascata nel caso li incontri nei suoi progetti. Più lavora con il CSS, la comprensione dei livelli di cascata e la conoscenza di come sfruttarne la potenza la risparmieranno da molti problemi nella gestione di una base di codice con CSS proveniente da diverse parti, plugin, e team di sviluppo.

I livelli di cascata sono particolarmente rilevanti quando sta lavorando con CSS da più fonti, quando ci sono selettori CSS in conflitto e specificità concorrenti, o quando sta considerando l'uso di [`!important`](/it/docs/Web/CSS/important).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Un'idea di come funziona il CSS, inclusa la cascata e la specificità (studi
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi del CSS Styling</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts">Gestione dei conflitti</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come funzionano i livelli di cascata.
      </td>
    </tr>
  </tbody>
</table>

Per ogni proprietà CSS applicata a un elemento, può esserci solo un valore. È possibile visualizzare tutti i valori delle proprietà applicati a un elemento ispezionando l'elemento negli strumenti per sviluppatori del suo browser. Il pannello "Stili" dello strumento mostra tutti i valori delle proprietà applicati all'elemento ispezionato, insieme al selettore corrispondente e al file sorgente CSS. Il selettore dall'origine con precedenza ha i suoi valori applicati all'elemento corrispondente.

Oltre agli stili applicati, il pannello Stili visualizza valori barrati che corrispondevano all'elemento selezionato ma non sono stati applicati a causa della cascata, della specificità o dell'ordine delle fonti. Gli stili barrati possono provenire dalla stessa origine con precedenza ma con specificità inferiore o con origine e specificità corrispondenti, ma sono stati trovati prima nella base di codice. Per qualsiasi valore di proprietà applicato, ci possono essere diverse dichiarazioni barrate da molte fonti diverse. Se vede uno stile barrato che ha un selettore con maggiore specificità, significa che al valore manca origine o importanza.

Spesso, man mano che la complessità di un sito aumenta, il numero di fogli di stile aumenta, il che rende l'ordine delle fonti dei fogli di stile sia più importante che più complesso. I livelli di cascata semplificano il mantenimento dei fogli di stile in tali basi di codice. I livelli di cascata sono contenitori di specificità esplicita che forniscono un controllo più semplice e maggiore sulle dichiarazioni CSS che in definitiva vengono applicate, consentendo agli sviluppatori web di dare priorità alle sezioni del CSS senza dover combattere la specificità.

Per comprendere i livelli di cascata, deve comprendere bene la cascata CSS. Le sezioni seguenti forniscono un rapido riepilogo dei concetti importanti della cascata.

## Revisione del concetto di cascata

La 'C' in CSS sta per "Cascading". È il metodo con cui gli stili si uniscono. Il user agent esegue diversi passaggi chiaramente definiti per determinare i valori assegnati a ogni proprietà per ogni elemento. Elencheremo brevemente questi passaggi qui e poi approfondiremo il passo 4, **Livelli di cascata**, che è ciò che è venuto qui per imparare:

1. **Rilevanza:** Trovi tutti i blocchi di dichiarazione con un selettore che corrisponde a ciascun elemento.
2. **Importanza:** Ordina le regole in base al fatto che siano normali o importanti. Gli stili importanti sono quelli in cui è impostato il flag [`!important`](/it/docs/Web/CSS/important).
3. **Origine:** All'interno di ciascuno dei due raggruppamenti di importanza, ordina le regole per autore, utente o origine dell'utente-agent.
4. **Livelli di cascata:** All'interno di ciascuno dei sei raggruppamenti di importanza dell'origine, ordina per livello di cascata. L'ordine dei livelli per le dichiarazioni normali va dal primo livello creato all'ultimo, seguito dagli stili normali non stratificati. Questo ordine è invertito per gli stili importanti, con stili importanti non stratificati che hanno la priorità più bassa.
5. **Specificità:** Per gli stili in competizione nel livello di origine con precedenza, ordina le dichiarazioni per [specificità](/it/docs/Web/CSS/CSS_cascade/Specificity).
6. **Prossimità di scoping:** Quando due selettori nel livello di origine con precedenza hanno la stessa specificità, il valore della proprietà all'interno delle regole con scoping con il minor numero di passaggi nella gerarchia DOM fino alla radice dello scoping vince. Veda [Come vengono risolti i conflitti di `@scope`](/it/docs/Web/CSS/@scope#how_scope_conflicts_are_resolved) per maggiori dettagli e un esempio.
7. **Ordine di apparizione:** Quando due selettori nel livello di origine con precedenza hanno la stessa specificità e prossimità di scoping, il valore della proprietà dal selettore dichiarato per ultimo con la specificità più alta vince.

Per ogni passaggio, solo le dichiarazioni "ancora in corsa" passano a "competere" nel passaggio successivo. Se solo una dichiarazione è in corsa, "vince", e i passaggi successivi sono irrilevanti.

### Origine e cascata

Ci sono tre [tipi di origine della cascata](/it/docs/Web/CSS/CSS_cascade/Cascade#origin_types): fogli di stile del user-agent, fogli di stile dell'utente e fogli di stile dell'autore. Il browser ordina ogni dichiarazione in sei categorie di origine per origine e importanza. Ci sono otto livelli di precedenza: le sei categorie di origine, le proprietà in transizione e le proprietà in animazione. L'ordine di precedenza va dagli stili normali del user-agent, che hanno la precedenza più bassa, agli stili che vengono attualmente animati, agli stili importanti del user-agent, e quindi agli stili in transizione, che hanno la precedenza più alta:

1. stili normali del user-agent
2. stili normali dell'utente
3. stili normali dell'autore
4. stili in animazione
5. stili importanti dell'autore
6. stili importanti dell'utente
7. stili importanti del user-agent
8. stili in transizione

Il "user-agent" è il browser. L'"utente" è il visitatore del sito. L'"autore" è lei, lo sviluppatore. Gli stili dichiarati direttamente su un elemento con l'elemento {{HTMLElement('style')}} sono stili dell'autore. Non includendo stili in animazione e in transizione, gli stili normali del user-agent hanno la precedenza più bassa; gli stili importanti del user-agent hanno la precedenza più alta.

### Origine e specificità

Per ciascuna proprietà, la dichiarazione che "vince" è quella dell'origine con precedenza basata sul peso (normale o importante). Ignorando i livelli per il momento, il valore dall'origine con la precedenza più alta viene applicato. Se l'origine vincente ha più di una dichiarazione di proprietà per un elemento, la [specificità](/it/docs/Web/CSS/CSS_cascade/Specificity) dei selettori per quei valori di proprietà in competizione viene confrontata. La specificità non è mai confrontata tra selettori di origini diverse.

Nell'esempio seguente, ci sono due collegamenti. Il primo non ha stili dell'autore applicati, quindi vengono applicati solo gli stili del user-agent (e i suoi stili personali, se presenti). Il secondo ha [`text-decoration`](/it/docs/Web/CSS/text-decoration) e [`color`](/it/docs/Web/CSS/color) impostati dagli stili dell'autore, anche se il selettore nel foglio di stile dell'autore ha una specificità di [`0-0-0`](/it/docs/Web/CSS/CSS_cascade/Specificity#selector_weight_categories). Il motivo per cui gli stili dell'autore "vincono" è perché quando ci sono stili in conflitto di origini diverse, le regole dell'origine con precedenza vengono applicate, indipendentemente dalla specificità nell'origine che non ha precedenza.

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

Il selettore "in competizione" nel foglio di stile del user-agent al momento della stesura è `a:any-link`, che ha un peso di specificità di `0-1-1`. Sebbene questo sia maggiore del selettore `0-0-0` nel foglio di stile dell'autore, anche se il selettore nel suo user agent corrente è diverso, non importa: i pesi di specificità delle origini dell'autore e del user-agent non sono mai confrontati. Scopra di più su [come viene calcolato il peso di specificità](/it/docs/Web/CSS/CSS_cascade/Specificity#how_is_specificity_calculated).

L'ordine di precedenza delle origini vince sempre sulla specificità del selettore. Se una proprietà dell'elemento viene stilizzata con una dichiarazione di stile normale in più origini, il foglio di stile dell'autore sovrascriverà sempre le proprietà normali ridondanti dichiarate in un foglio di stile dell'utente o del user-agent. Se lo stile è importante, il foglio di stile del user-agent vincerà sempre sugli stili dell'autore e dell'utente. L'ordine di precedenza dell'origine della cascata assicura che i conflitti di specificità tra origini non si verifichino mai.

Un'ultima cosa da notare prima di procedere: l'ordine di apparizione diventa rilevante solo quando le dichiarazioni in competizione nell'origine di precedenza hanno la stessa specificità.

## Panoramica dei livelli di cascata

Abbiamo ora compreso "l'ordine di precedenza dell'origine della cascata", ma cos'è "l'ordine di precedenza dei livelli di cascata"? Risponderemo a questa domanda affrontando cosa sono i livelli di cascata, come sono ordinati e come gli stili vengono assegnati ai livelli di cascata. Tratteremo [livelli regolari](#creazione_dei_livelli_di_cascata), [livelli annidati](#panoramica_dei_livelli_di_cascata_annidati), e livelli anonimi. Discutiamo prima di cosa siano i livelli di cascata e quali problemi risolvono.

### Ordine di precedenza dei livelli di cascata

Similmente a come abbiamo sei livelli di priorità basati su origine e importanza, i livelli di cascata ci permettono di creare un livello sub-origine di priorità all'interno di ciascuna di quelle origini.

All'interno di ciascuna delle sei categorie di origine, ci possono essere più livelli di cascata. L'[ordine di creazione del livello](/it/docs/Web/CSS/@layer) è molto importante. È l'ordine di creazione che stabilisce l'ordine di precedenza tra i livelli all'interno di un'origine.

Nei raggruppamenti normali di origine, i livelli sono ordinati nell'ordine di creazione di ciascun livello. L'ordine di precedenza va dal primo livello creato all'ultimo, seguito dagli stili normali non stratificati.

Questo ordine è invertito per gli stili importanti. Tutti gli stili importanti non stratificati cascano insieme in un livello implicito che ha precedenza su tutti gli stili normali non in transizione. Gli stili importanti non stratificati hanno meno precedenza rispetto a qualsiasi stile importante stratificato. Gli stili importanti nei livelli dichiarati precedentemente hanno precedenza su quelli nei livelli dichiarati successivamente all'interno della stessa origine.

Per il resto di questo tutorial, limiteremo la nostra discussione agli stili degli autori, ma tenga a mente che i livelli possono anche esistere nei fogli di stile degli utenti e dei user-agent.

### Problemi che i livelli di cascata possono risolvere

Grandi basi di codice possono avere stili provenienti da più team, librerie di componenti, framework e terze parti. Non importa quanti fogli di stile siano inclusi, tutti questi stili cascano insieme in un'unica origine: il foglio di stile dell'autore.

Avere stili da molte fonti che cascano insieme, soprattutto da team che non lavorano insieme, può creare problemi. Team diversi potrebbero avere metodologie diverse; uno potrebbe avere la best practice di ridurre la specificità, mentre un altro potrebbe avere uno standard di includere un `id` in ciascun selettore.

I conflitti di specificità possono aumentare rapidamente. Uno sviluppatore web potrebbe creare una "soluzione rapida" aggiungendo un flag `!important`. Sebbene questo possa sembrare una soluzione facile, spesso sposta soltanto la guerra di specificità dalle dichiarazioni normali a quelle importanti.

Allo stesso modo in cui le origini della cascata forniscono un equilibrio di potere tra stili dell'utente, user-agent e autore, i livelli di cascata forniscono un modo strutturato per organizzare e bilanciare le preoccupazioni all'interno di un'unica origine come se ciascun livello in un'origine fosse un sub-origine. Un livello può essere creato per ciascun team, componente e terza parte, con la precedenza degli stili basata sull'ordine del livello.

Le regole all'interno di un livello cascano insieme, senza competere con le regole di stile al di fuori del livello. I livelli di cascata consentono di dare priorità a interi fogli di stile su altri fogli di stile, senza doversi preoccupare della specificità tra questi sub-origini.

La precedenza dei livelli vince sempre sulla specificità del selettore. Gli stili nei livelli con precedenza "vincono" su quelli con meno precedenza. La specificità di un selettore in un livello perdente è irrilevante. La specificità è ancora importante per i valori delle proprietà in competizione all'interno di un livello, ma non ci sono preoccupazioni di specificità tra i livelli poiché viene considerato solo il livello di priorità più alto per ciascuna proprietà.

### Problemi che i livelli di cascata annidati possono risolvere

I livelli di cascata permettono la creazione di livelli annidati. Ciascun livello di cascata può contenere livelli annidati.

Ad esempio, una libreria di componenti può essere importata in un livello `components`. Un normale livello di cascata aggiungerà la libreria di componenti all'origine dell'autore, rimuovendo eventuali conflitti di specificità con altri stili dell'autore. All'interno del livello `components`, uno sviluppatore può scegliere di definire vari temi, ciascuno come un livello annidato separato. L'ordine di questi livelli tematici annidati può essere definito in base a [media queries (vedi sezione alla creazione di livelli e media query)](#creazione_di_livelli_e_media_query) come la dimensione della finestra o l'[orientamento](/it/docs/Web/CSS/@media/orientation). Questi livelli annidati forniscono un modo per creare temi che non si scontrano basandosi sulla specificità.

La capacità di annidare i livelli è molto utile per chiunque lavori su librerie di componenti, framework, widget di terze parti e temi.

La capacità di creare livelli annidati rimuove anche la preoccupazione di avere nomi di livelli in conflitto. Tratteremo questo nella sezione [livello annidato](#panoramica_dei_livelli_di_cascata_annidati).

> "Gli autori possono creare livelli per rappresentare i default degli elementi, librerie di terze parti, temi, componenti, sovrapposizioni e altri problemi di stile - e sono in grado di riordinare la cascata dei livelli in modo esplicito, senza alterare i selettori o la specificità all'interno di ciascun livello, o fare affidamento sull'ordine di apparizione per risolvere i conflitti tra i livelli."
>
> —Specifiche Cascading e Inheritance

## Creazione dei livelli di cascata

I livelli possono essere creati utilizzando uno dei seguenti metodi:

- La regola at-rule [`@layer`](/it/docs/Web/CSS/@layer), dichiara livelli utilizzando `@layer` seguito dai nomi di uno o più livelli. Questo crea livelli nominati senza assegnare loro stili.
- La regola at-rule `@layer` block, in cui tutti gli stili all'interno di un blocco sono aggiunti a un livello nominato o non nominato.
- La regola [`@import`](/it/docs/Web/CSS/@import) con la parola chiave `layer` o la funzione `layer()`, che assegna il contenuto del file importato a quel livello.

Tutti e tre i metodi creano un livello se un livello con quel nome non è già stato inizializzato. Se non viene fornito alcun nome di livello nella regola at-rule `@layer` o `@import` con `layer()`, viene creato un nuovo livello anonimo (non nominato).

> [!NOTE]
> L'ordine di precedenza dei livelli è l'ordine in cui vengono creati. Gli stili non in un livello, o "stili non stratificati", cascano insieme in un'etichetta finale implicita.

Copriamo i tre modi di creare un livello in un po' più di dettaglio prima di discutere i livelli annidati.

### La regola at-rule @layer statement per livelli nominati

L'ordine dei livelli è impostato dall'ordine in cui i livelli appaiono nel suo CSS. Dichiarare i livelli utilizzando `@layer` seguito dai nomi di uno o più livelli senza assegnare alcuno stile è un modo per definire l'[ordine dei livelli](#determinazione_della_precedenza_basata_sull'ordine_dei_livelli).

La regola CSS [`@layer`](/it/docs/Web/CSS/@layer) at-rule è utilizzata per dichiarare un livello di cascata e definire l'ordine di precedenza quando ci sono più livelli di cascata. La seguente at-rule dichiara tre livelli, nell'ordine elencato:

```css
@layer theme, layout, utilities;
```

Spesso vorrà avere la prima linea del suo CSS come questa dichiarazione `@layer` (ovviamente con i nomi dei livelli che hanno senso per il suo sito) per avere il pieno controllo sull'ordinamento dei livelli.

Se la dichiarazione sopra è la prima linea del CSS di un sito, l'ordine dei livelli sarà `theme`, `layout`, e `utilities`. Se alcuni livelli sono stati creati prima della dichiarazione sopra, fintanto che i livelli con questi nomi non esistono già, questi tre livelli saranno creati e aggiunti alla fine dell'elenco dei livelli esistenti. Tuttavia, se un livello con lo stesso nome esiste già, la dichiarazione sopra creerà solo due nuovi livelli. Ad esempio, se `layout` esiste già, solo `theme` e `utilities` saranno creati, ma l'ordine dei livelli, in questo caso, sarà `layout`, `theme` e `utilities`.

### La regola at-rule @layer block per livelli nominati e anonimi

I livelli possono essere creati utilizzando la regola at-rule `@layer` block. Se una regola at-rule `@layer` è seguita da un identificatore e un blocco di stili, l'identificatore viene utilizzato per nominare il livello e gli stili in questa regola at-rule vengono aggiunti agli stili del livello. Se un livello con il nome specificato non esiste già, verrà creato un nuovo livello. Se un livello con lo stesso nome esiste già, gli stili sono aggiunti al livello precedentemente esistente. Se nessun nome è specificato durante la creazione di un blocco di stili usando `@layer`, gli stili nella regola at-rule verranno aggiunti a un nuovo livello anonimo.

Nell'esempio seguente, abbiamo usato quattro regole at-rule `@layer` block e una regola at-rule `@layer` statement. Questo CSS fa quanto segue nell'ordine elencato:

1. Crea un livello nominato `layout`
2. Crea un livello anonimo
3. Dichiara una lista di tre livelli e crea solo due nuovi livelli, `theme` e `utilities`, perché `layout` esiste già
4. Aggiunge stili aggiuntivi al livello `layout` già esistente
5. Crea un secondo livello anonimo

```css
/* file: layers1.css */

/* unlayered styles */
body {
  color: #333;
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
    color: #000;
  }
}

/* creates the fifth layer: an unnamed, anonymous layer */
@layer {
  body {
    margin: 1vw;
  }
}
```

Nel codice CSS sopra, abbiamo creato cinque livelli: `layout`, `<anonimo(01)>`, `theme`, `utilities` e `<anonimo(02)>` – in quell'ordine – con un sesto livello implicito di stili non stratificati contenuti nel blocco di stile `body`. L'ordine dei livelli è l'ordine in cui i livelli vengono creati, con il livello implicito di stili non stratificati sempre ultimo. Non c'è modo di cambiare l'ordine dei livelli una volta creati.

Abbiamo assegnato alcuni stili al livello nominato `layout`. Se un livello nominato non esiste già, specificando il nome in una regola at-rule `@layer`, con o senza assegnare stili al livello, crea il livello; questo aggiunge il livello alla fine della serie di nomi di livelli esistenti. Se il livello nominato esiste già, tutti gli stili all'interno del blocco nominato vengono aggiunti agli stili nel livello precedentemente esistente – specificare stili in un blocco riutilizzando un nome di livello esistente non crea un nuovo livello.

I livelli anonimi sono creati assegnando stili a un livello senza nominare il livello. Gli stili possono essere aggiunti a un livello non nominato solo al momento della sua creazione.

> [!NOTE]
> L'uso successivo di `@layer` senza nome di livello crea livelli anonimi aggiuntivi; non aggiunge stili a un livello anonimo precedentemente esistente.

La regola at-rule `@layer` crea un livello, nominato o no, o aggiunge stili a un livello se il livello nominato esiste già. Abbiamo chiamato il primo livello anonimo `<anonimo(01)>` e il secondo `<anonimo(02)>`, questo solo per poterli spiegare. Questi sono in realtà livelli non nominati. Non c'è modo di fare riferimento a loro o aggiungere stili aggiuntivi a loro.

Tutti gli stili dichiarati al di fuori di un livello sono uniti insieme in un livello implicito. Nel codice di esempio sopra, la prima dichiarazione ha impostato la proprietà `color: #333` su `body`. Questo è stato dichiarato al di fuori di qualsiasi livello. Le dichiarazioni normali non stratificate hanno precedenza sulle dichiarazioni normali stratificate anche se gli stili non stratificati hanno una specificità inferiore e vengono prima nell'ordine di apparizione. Questo spiega perché anche se il CSS non stratificato è stato dichiarato per primo nel blocco di codice, il livello implicito contenente questi stili non stratificati ha precedenza come se fosse l'ultimo livello dichiarato.

Nella linea `@layer theme, layout, utilities;`, in cui è stata dichiarata una serie di livelli, sono stati creati solo i livelli `theme` e `utilities`; `layout` era già stato creato nella prima linea. Si noti che questa dichiarazione non cambia l'ordine dei livelli già creati. Al momento non c'è modo di riordinare i livelli una volta dichiarati.

Nel seguente esempio, assegnamo stili a due livelli, creandoli e assegnando loro un nome nel processo. Poiché esistono già, essendo stati creati quando utilizzati per la prima volta, dichiararli nell'ultima linea non fa niente.

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

Provi a spostare l'ultima linea, `@layer site, page;`, per renderla la prima linea. Cosa succede?

#### Creazione di livelli e media query

Se definisce un livello utilizzando [query dei media](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) o [query di caratteristiche](/it/docs/Web/CSS/CSS_conditional_rules/Using_feature_queries), e il media non è una corrispondenza o la funzione non è supportata, il livello non viene creato. L'esempio sotto mostra come cambiare la dimensione del suo dispositivo o browser potrebbe cambiare l'ordine del livello. In questo esempio, creiamo il livello `site` solo nei browser più larghi. Assegnamo quindi stili ai livelli `page` e `site`, in quest'ordine.

```html live-sample___media-order
<h1>Is this heading underlined?</h1>
```

```css live-sample___media-order
@media (min-width: 50em) {
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

Nei schermi larghi, il livello `site` è dichiarato nella prima linea, il che significa che `site` ha meno precedenza rispetto a `page`. Altrimenti, `site` ha precedenza su `page` perché è dichiarato più tardi nei schermi stretti. Se non funziona, provi a cambiare il `50em` nella media query con `10em` o `100em`.

### Importazione di fogli di stile in livelli nominati e anonimi con @import

La regola [`@import`](/it/docs/Web/CSS/@import) permette agli utenti di importare regole di stile da altri fogli di stile direttamente in un file CSS o in un elemento {{htmlelement('style')}}.

Quando importa fogli di stile, la dichiarazione `@import` deve essere definita prima di qualsiasi stile CSS all'interno del foglio di stile o blocco `<style>`. La dichiarazione `@import` deve venire per prima, prima di qualsiasi stile, ma può essere preceduta da una regola at-rule `@layer` che crea uno o più livelli senza assegnare stili ai livelli. (`@import` può anche essere preceduta da una regola [`@charset`](/it/docs/Web/CSS/@charset).)

Può importare un foglio di stile in un livello nominato, un livello annidato nominato, o un livello anonimo. Il seguente esempio importa i fogli di stile in un livello `components`, un livello annidato `dialog` all'interno del livello `components`, e un livello non nominato, rispettivamente:

```css
@import url("components-lib.css") layer(components);
@import url("dialog.css") layer(components.dialog);
@import url("marketing.css") layer();
```

Può importare più di un file CSS in un singolo livello. La seguente dichiarazione importa due file separati in un unico livello `social`:

```css
@import url(comments.css) layer(social);
@import url(sm-icons.css) layer(social);
```

Può importare stili e creare livelli basati su condizioni specifiche usando [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) e [query di caratteristiche](/it/docs/Web/CSS/CSS_conditional_rules/Using_feature_queries). Il seguente importa un foglio di stile in un livello `international` solo se il browser supporta `display: ruby`, e il file importato è dipendente dalla larghezza dello schermo.

```css
@import url("ruby-narrow.css") layer(international) supports(display: ruby)
  (width < 32rem);
@import url("ruby-wide.css") layer(international) supports(display: ruby)
  (width >= 32rem);
```

> [!NOTE]
> Non esiste un equivalente del metodo {{HTMLElement('link')}} per collegare i fogli di stile. Utilizzi `@import` per importare un foglio di stile in un livello quando non può utilizzare `@layer` all'interno del foglio di stile.

## Panoramica dei livelli di cascata annidati

I livelli annidati sono livelli all'interno di un livello nominato o anonimo. Ciascun livello di cascata, anche uno anonimo, può contenere livelli annidati. I livelli importati in un altro livello diventano livelli annidati all'interno di quel livello.

### Vantaggi di annidare i livelli

La capacità di annidare i livelli permette ai team di creare livelli di cascata senza preoccuparsi se altri team li importeranno in un livello. Allo stesso modo, l'annidamento le consente di importare fogli di stile di terze parti in un livello senza preoccuparsi se quel foglio di stile stesso ha livelli. Poiché i livelli possono essere annidati, non deve preoccuparsi di avere nomi di livelli in conflitto tra fogli di stile esterni e interni.

### Creazione di livelli di cascata annidati

I livelli annidati possono essere creati usando gli stessi metodi descritti per i livelli regolari. Ad esempio, possono essere creati usando la regola at-rule `@layer` seguita dai nomi di uno o più livelli, usando una notazione a punti. Più punti e nomi di livelli indicano multipli annidamenti.

Se annida una regola at-rule `@layer` block all'interno di un'altra regola at-rule `@layer` block, con o senza nome, il blocco annidato diventa un livello annidato. Allo stesso modo, quando un foglio di stile è importato con una dichiarazione `@import` contenente la parola chiave `layer` o la funzione `layer()`, gli stili vengono assegnati a quel livello nominato o anonimo. Se la dichiarazione `@import` contiene livelli, quei livelli diventano livelli annidati all'interno di quel livello anonimo o nominato.

Osserviamo il seguente esempio:

```css
@import url("components-lib.css") layer(components);
@import url("narrow-theme.css") layer(components.narrow);
```

Nella prima linea, importiamo `components-lib.css` nel livello `components`. Se quel file contiene livelli, nominati o no, quei livelli diventano livelli annidati all'interno del livello `components`.

La seconda linea importa `narrow-theme.css` nel livello `narrow`, che è un sotto-livello del `components`. Il livello annidato `components.narrow` viene creato come ultimo livello all'interno del livello `components`, a meno che `components-lib.css` non contenga già un livello `narrow`, nel qual caso, il contenuto di `narrow-theme.css` verrebbe aggiunto al livello annidato `components.narrow`. Ulteriori livelli annidati nominati possono essere aggiunti al livello `components` usando il pattern `components.<layerName>`. Come detto prima, i livelli non nominati possono essere creati ma non possono essere acceduti successivamente.

Osserviamo un altro esempio, in cui [importiamo `layers1.css` in un livello nominato](#the_layer_block_at_rule_for_named_and_anonymous_layers) usando la seguente dichiarazione:

```css
@import url(layers1.css) layer(example);
```

Questo creerà un singolo livello chiamato `example` contenente alcune dichiarazioni e cinque livelli annidati - `example.layout`, `example.<anonimo(01)>`, `example.theme`, `example.utilities`, e `example.<anonimo(02)>`.

Per aggiungere stili a un livello annidato nominato, usi la notazione a punti:

```css
@layer example.layout {
  main {
    width: 50vw;
  }
}
```

## Determinazione della precedenza basata sull'ordine dei livelli

L'ordine dei livelli determina il loro ordine di precedenza. Pertanto, l'ordine dei livelli è molto importante. Allo stesso modo in cui la cascata ordina per origine e importanza, la cascata ordina ciascuna dichiarazione CSS per origine livello e importanza.

### Ordine di precedenza dei livelli di cascata regolari

```css
@import url(A.css) layer(firstLayer);
@import url(B.css) layer(secondLayer);
@import url(C.css);
```

Il codice sopra crea due livelli nominati (gli stili di C.css vengono aggiunti al livello implicito di stili non stratificati). Supponiamo che i tre file (`A.css`, `B.css`, e `C.css`) non contengano ulteriori livelli all'interno di essi. L'elenco seguente mostra dove gli stili dichiarati dentro e fuori da questi file saranno ordinati dal meno (1) alla massima (10) precedenza.

1. Stili normali di `firstLayer` (`A.css`)
2. Stili normali di `secondLayer` (`B.css`)
3. Stili normali non stratificati (`C.css`)
4. Stili normali inline
5. Stili in animazione
6. Stili importanti non stratificati (`C.css`)
7. Stili importanti di `secondLayer` (`B.css`)
8. Stili importanti di `firstLayer` (`A.css`)
9. Stili importanti inline
10. Stili in transizione

Gli stili normali dichiarati all'interno dei livelli ricevono la priorità più bassa e sono ordinati in base all'ordine in cui i livelli sono stati creati. Gli stili normali nel primo livello creato hanno la precedenza più bassa, e gli stili normali nel livello creato per ultimo hanno la precedenza più alta tra i livelli. In altre parole, gli stili normali dichiarati all'interno di `firstLayer` saranno sovrascritti da eventuali stilizzazioni successive sull'elenco se esistono conflitti.

Prossimi sono gli stili dichiarati al di fuori dei livelli. Gli stili in `C.css` non sono stati importati in un livello e sovrascriveranno qualsiasi stile in conflitto da `firstLayer` e `secondLayer`. Gli stili non dichiarati in un livello hanno sempre precedenza sugli stili che _sono_ dichiarati all'interno di un livello (ad eccezione degli stili importanti).

Gli stili inline sono dichiarati utilizzando l'[attributo `style`](/it/docs/Web/HTML/Reference/Global_attributes/style). Gli stili normali dichiarati in questo modo avranno precedenza su stili normali trovati nei fogli non stratificati e stratificati (`firstLayer – A.css`, `secondLayer – B.css`, e `C.css`).

Gli stili in animazione hanno precedenza più alta rispetto a tutti gli stili normali, inclusi gli stili normali inline.

Gli stili importanti, ovvero i valori delle proprietà che includono il flag `!important`, hanno precedenza su qualsiasi stile precedentemente menzionato nel nostro elenco. Sono ordinati in ordine inverso rispetto agli stili normali. Gli stili importanti dichiarati al di fuori di un livello hanno meno precedenza di quelli dichiarati all'interno di un livello. Gli stili importanti trovati nei livelli sono anche ordinati in base all'ordine di creazione del livello. Per gli stili importanti, l'ultimo livello creato ha la precedenza più bassa, e il primo livello creato ha la precedenza più alta tra i livelli dichiarati.

Gli stili importanti inline di nuovo hanno precedenza su tutti gli stili importanti dichiarati altrove.

Gli stili in transizione hanno la precedenza più alta. Quando un valore di proprietà normale è in transizione, ha precedenza su tutte le altre dichiarazioni di valore di proprietà, anche sugli stili importanti inline; ma solo mentre è in transizione.

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

In questo esempio, due livelli (`A` e `B`) sono inizialmente definiti utilizzando una regola at-rule `@layer` statement senza alcuno stile. Gli stili del livello sono definiti in due regole at-rule `@layer` block che appaiono dopo la regola CSS `h1` dichiarata al di fuori di qualsiasi livello.

Gli stili inline aggiunti sull'elemento `h1` utilizzando l'attributo `style`, impostano un normale `color` e un importante `background-color`. Gli stili normali inline sovrascrivono tutti gli stili stratificati e non stratificati normali. Gli stili importanti inline sovrascrivono tutti gli stili stratificati e non stratificati normali e importanti dell'autore. Non c'è modo per gli stili dell'autore di sovrascrivere stili importanti inline.

I normali `text-decoration` e importanti `box-shadow` non fanno parte degli stili inline `style` e possono quindi essere sovrascritti. Per gli stili normali non inline, gli stili non stratificati hanno precedenza. Per gli stili importanti, importa anche l'ordine del livello. Mentre gli stili normali non stratificati sovrascrivono tutti gli stili normali impostati in un livello, con gli stili importanti, l'ordine di precedenza è invertito; gli stili importanti non stratificati hanno meno precedenza degli stili stratificati.

I due stili dichiarati solo all'interno dei livelli sono `font-style`, con importanza normale, e `font-weight` con un flag `!important`. Per gli stili normali, il livello `B`, dichiarato per ultimo, sovrascrive gli stili nel livello dichiarato precedentemente `A`. Per gli stili normali, i livelli successivi hanno precedenza sui livelli precedenti. L'ordine di precedenza è invertito per gli stili importanti. Per le dichiarazioni di `font-weight` importanti, il livello `A`, essendo dichiarato per primo, ha precedenza sul livello dichiarato per ultimo `B`.

Può invertire l'ordine dei livelli cambiando la prima linea da `@layer A, B;` a `@layer B, A;`. Provi a farlo. Quali stili vengono cambiati da questo e quali rimangono gli stessi? Perché?

L'ordine dei livelli è impostato dall'ordine in cui i livelli appaiono nel suo CSS. Nella nostra prima linea, abbiamo dichiarato i livelli senza assegnare alcuno stile usando `@layer` seguito dai nomi dei nostri livelli, terminando con un punto e virgola. Se avessimo omesso questa linea, i risultati sarebbero stati gli stessi. Perché? Abbiamo assegnato regole di stile nei blocchi nominati `@layer` nell'ordine A poi B. I due livelli sono stati creati in quella prima linea. Se non lo fossero stati, questi blocchi di regole li avrebbero creati, in quell'ordine.

Abbiamo incluso quella prima linea per due motivi: primo, in modo che potesse facilmente modificare la linea e cambiare l'ordine, e secondo perché spesso troverà che dichiarare in anticipo l'ordine del livello di solito è a lungo termine la migliore pratica per la gestione dell'ordine dei livelli.

Per riassumere:

- L'ordine di precedenza dei livelli è l'ordine in cui i livelli vengono creati.
- Una volta creati, non c'è modo di cambiare l'ordine dei livelli.
- La precedenza dei livelli per gli stili normali è l'ordine in cui i livelli vengono creati.
- Gli stili normali non stratificati hanno precedenza sugli stili normali stratificati.
- La precedenza dei livelli per gli stili importanti è invertita, con i livelli creati prima avendo precedenza.
- Tutti gli stili importanti stratificati hanno precedenza sugli stili importanti (e normali) non stratificati.
- Gli stili normali inline hanno precedenza su tutti gli stili normali, stratificati o no.
- Gli stili importanti inline hanno precedenza su tutti gli altri stili, ad eccezione degli stili in transizione.
- Non c'è modo per gli stili dell'autore di sovrascrivere gli stili importanti inline (se non transitandoli, il che è temporaneo).

### Ordine di precedenza dei livelli di cascata annidati

L'ordine di precedenza della cascata per i livelli annidati è simile a quello dei livelli regolari, ma contenuto all'interno del livello. L'ordine di precedenza è basato sull'ordine di creazione del livello annidato. Gli stili non annidati all'interno di un livello hanno precedenza sugli stili normali annidati, con l'ordine di precedenza invertito per gli stili importanti. Il peso della specificità tra i livelli annidati non importa, sebbene importa per gli stili in conflitto all'interno di un livello annidato.

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

Ecco un riepilogo delle proprietà utilizzate e del perché ciascuna dichiarazione è applicata:

- `background-color`: Poiché gli stili normali non stratificati hanno precedenza sugli stili normali stratificati, il colore `wheat` vince.
- `border`: Poiché all'interno di un livello gli stili non annidati hanno precedenza sugli stili normali annidati, il colore `red` vince.
- `color`: Con gli stili importanti, gli stili stratificati hanno precedenza sugli stili non stratificati, con importanza sugli stili dei livelli dichiarati precedentemente avendo precedenza sui livelli dichiarati successivamente. In questo esempio, l'ordine di creazione del livello annidato è `components.narrow`, poi `components.wide`, quindi gli stili importanti in `components.narrow` hanno precedenza sugli stili importanti in `components.wide`, il che significa che il colore `purple` vince.
- `border-radius`: La proprietà è stata impostata solo nei livelli annidati quindi per ordine di dichiarazione il raggio `20%` vince.

## Testi le sue abilità!

Ha raggiunto la fine di questo articolo, ma ricorda le informazioni più importanti? Può trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — veda [Testare le sue abilità: La Cascata, Task 2](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade#task_2).

## Sommario

Se ha compreso la maggior parte di questo articolo, ben fatto — ora è familiare con i meccanismi fondamentali dei livelli di cascata CSS.
