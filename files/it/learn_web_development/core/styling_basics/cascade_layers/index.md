---
title: Livelli di cascata
slug: Learn_web_development/Core/Styling_basics/Cascade_layers
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questa lezione mira a introdurti ai [livelli di cascata](/it/docs/Web/CSS/@layer), una funzionalità più avanzata che si basa sui concetti fondamentali della [cascata CSS](/it/docs/Web/CSS/CSS_cascade/Cascade) e della [specificità CSS](/it/docs/Web/CSS/CSS_cascade/Specificity).

Se sei nuovo nel CSS, lavorare attraverso questa lezione potrebbe sembrare meno immediatamente rilevante e un po' più accademico rispetto ad altre parti del corso. Tuttavia, conoscere le basi di cosa sono i livelli di cascata può essere utile se li incontri nei tuoi progetti. Più lavori con il CSS, comprendere i livelli di cascata e sapere come sfruttare la loro potenza ti risparmierà molto dolore nella gestione di una base di codice con CSS da diverse fonti, plugin e team di sviluppo.

I livelli di cascata sono particolarmente rilevanti quando lavori con CSS da più fonti, quando ci sono selettori CSS in conflitto e specificità concorrenti, o quando stai considerando l'uso di [`!important`](/it/docs/Web/CSS/important).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Un'idea di come funziona il CSS, inclusi cascata e specificità (studia
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">le basi dello stile CSS</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts">Gestione dei conflitti</a>).
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

Per ogni proprietà CSS applicata a un elemento, può esserci solo un valore. Puoi visualizzare tutti i valori delle proprietà applicate a un elemento ispezionandolo negli strumenti di sviluppo del browser. Il pannello "Stili" dello strumento mostra tutti i valori delle proprietà applicate all'elemento in esame, insieme al selettore corrispondente e al file sorgente CSS. Il selettore dell'origine con precedenza applica i propri valori all'elemento corrispondente.

Oltre agli stili applicati, il pannello Stili mostra i valori barrati che corrispondono all'elemento selezionato ma non sono stati applicati a causa della cascata, della specificità o dell'ordine della fonte. Gli stili barrati possono provenire dalla stessa origine con precedenza ma con specificità inferiore, o con origine e specificità corrispondenti ma trovati in precedenza nella base di codice. Per qualsiasi valore di proprietà applicato, potrebbero esserci diverse dichiarazioni barrate provenienti da molte fonti diverse. Se vedi uno stile barrato che ha un selettore con maggiore specificità, significa che il valore manca di origine o importanza.

Spesso, man mano che aumenta la complessità di un sito, aumenta anche il numero di fogli di stile, il che rende l'ordine delle fonti dei fogli di stile sia più importante che più complesso. I livelli di cascata semplificano la manutenzione dei fogli di stile attraverso tali basi di codice. I livelli di cascata sono contenitori di specificità esplicita che offrono un controllo più semplice e maggiore sulle dichiarazioni CSS che vengono applicate, consentendo agli sviluppatori web di dare priorità a sezioni del CSS senza dover lottare contro la specificità.

Per comprendere i livelli di cascata, devi capire bene la cascata CSS. Le sezioni seguenti forniscono un rapido riepilogo dei concetti importanti della cascata.

## Revisione del concetto di cascata

La 'C' in CSS sta per "Cascading". È il metodo con cui gli stili si uniscono in cascata. Il user agent attraversa diversi passaggi chiaramente definiti per determinare i valori assegnati a ogni proprietà per ogni elemento. Elencheremo brevemente questi passaggi qui e poi approfondiremo il passaggio 4, **Livelli di cascata**, che è ciò che sei venuto a imparare:

1. **Rilevanza:** Trova tutti i blocchi di dichiarazione con una corrispondenza di selettore per ogni elemento.
2. **Importanza:** Ordina le regole in base alla normalità o all'importanza. Gli stili importanti sono quelli che hanno impostato il flag [`!important`](/it/docs/Web/CSS/important).
3. **Origine:** All'interno di ciascun bucket di importanza, ordina le regole per origine autore, utente o user-agent.
4. **Livelli di cascata:** All'interno di ciascuno dei sei bucket di origine importanza, ordina per livello di cascata. L'ordine del livello per le dichiarazioni normali è dal primo livello creato all'ultimo, seguito dagli stili normali non stratificati. Questo ordine è invertito per gli stili importanti, con gli stili importanti non stratificati che hanno la minore precedenza.
5. **Specificità:** Per stili concorrenti nello strato di origine con precedenza, ordina le dichiarazioni per [specificità](/it/docs/Web/CSS/CSS_cascade/Specificity).
6. **Prossimità di ambito**: Quando due selettori nello strato di origine con precedenza hanno la stessa specificità, il valore della proprietà all'interno delle regole con il minor numero di salti nella gerarchia DOM verso la radice dell'ambito vince. Consulta [Come i conflitti di `@scope` sono risolti](/it/docs/Web/CSS/@scope#how_scope_conflicts_are_resolved) per ulteriori dettagli e un esempio.
7. **Ordine di apparizione:** Quando due selettori nello strato di origine con precedenza hanno la stessa specificità e prossimità di ambito, il valore della proprietà dall'ultimo selettore dichiarato con la specificità più alta vince.

Per ogni passaggio, solo le dichiarazioni "ancora in gioco" passano a "competere" nel passaggio successivo. Se solo una dichiarazione è in gioco, "vince", e i passaggi successivi sono inutili.

### Origine e cascata

Esistono tre [tipi di origine della cascata](/it/docs/Web/CSS/CSS_cascade/Cascade#origin_types): fogli di stile dell'user-agent, fogli di stile dell'utente e fogli di stile dell'autore. Il browser ordina ciascuna dichiarazione in sei bucket di origine per origine e importanza. Ci sono otto livelli di precedenza: i sei bucket di origine, proprietà in transizione e proprietà che stanno animando. L'ordine di precedenza va dagli stili normali dell'user-agent, che hanno la minore precedenza, agli stili nelle animazioni attualmente applicate, agli stili importanti dell'user-agent e poi agli stili in transizione, che hanno la massima precedenza:

1. stili normali dell'user-agent
2. stili normali dell'utente
3. stili normali dell'autore
4. stili in animazione
5. stili importanti dell'autore
6. stili importanti dell'utente
7. stili importanti dell'user-agent
8. stili in transizione

Il "user-agent" è il browser. L'"utente" è il visitatore del sito. L'"autore" sei tu, lo sviluppatore. Gli stili dichiarati direttamente su un elemento con l'elemento {{HTMLElement('style')}} sono stili dell'autore. Escludendo gli stili animati e in transizione, gli stili normali dell'user-agent hanno la minore precedenza; gli stili importanti dell'user-agent hanno la maggiore.

### Origine e specificità

Per ogni proprietà, la dichiarazione che "vince" è quella dell'origine con precedenza basata sul peso (normale o importante). Ignorando i livelli per il momento, il valore dell'origine con la massima precedenza viene applicato. Se l'origine vincente ha più di una dichiarazione di proprietà per un elemento, la [specificità](/it/docs/Web/CSS/CSS_cascade/Specificity) dei selettori per quei valori di proprietà concorrenti viene confrontata. La specificità non viene mai paragonata tra selettori di origini diverse.

Nell'esempio qui sotto, ci sono due link. Il primo non ha stili dell'autore applicati, quindi vengono applicati solo gli stili dell'user-agent (e i tuoi personali stili utente, se presenti). Il secondo ha [`text-decoration`](/it/docs/Web/CSS/text-decoration) e [`color`](/it/docs/Web/CSS/color) impostati dagli stili dell'autore anche se il selettore nel foglio di stile dell'autore ha una specificità di [`0-0-0`](/it/docs/Web/CSS/CSS_cascade/Specificity#selector_weight_categories). Il motivo per cui gli stili dell'autore "vincono" è perché quando ci sono stili in conflitto provenienti da origini diverse, le regole dell'origine con precedenza vengono applicate, indipendentemente dalla specificità nell'origine che non ha precedenza.

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

Il selettore "concorrente" nel foglio di stile dell'user-agent al momento della scrittura è `a:any-link`, che ha un peso di specificità di `0-1-1`. Anche se questo è maggiore del selettore `0-0-0` nel foglio di stile dell'autore, anche se il selettore nel tuo user-agent attuale è diverso, non importa: i pesi di specificità da origini autore e user-agent non vengono mai confrontati. Scopri di più su [come viene calcolato il peso di specificità](/it/docs/Web/CSS/CSS_cascade/Specificity#how_is_specificity_calculated).

L'ordine di precedenza dell'origine vince sempre sulla specificità del selettore. Se la proprietà di un elemento è stilizzata con una dichiarazione di stile normale in più origini, il foglio di stile dell'autore sovrascriverà sempre le ridondanti proprietà normali dichiarate in un foglio di stile dell'utente o user-agent. Se lo stile è importante, il foglio di stile dell'user-agent vincerà sempre sugli stili autore e utente. La precedenza dell'origine della cascata assicura che non ci siano mai conflitti di specificità tra le origini.

Un'ultima cosa da notare prima di procedere: l'ordine di apparizione diventa rilevante solo quando le dichiarazioni concorrenti nell'origine della precedenza hanno la stessa specificità.

## Panoramica dei livelli di cascata

Ora comprendiamo la "precedenza dell'origine della cascata", ma cos'è la "precedenza del livello di cascata"? Risponderemo a questa domanda affrontando cosa sono i livelli di cascata, come vengono ordinati e come gli stili vengono assegnati ai livelli di cascata. Tratteremo i [livelli regolari](#creare_livelli_di_cascata), i [livelli annidati](#panoramica_dei_livelli_di_cascata_annidati) e i livelli anonimi. Discutiamo innanzitutto cosa sono i livelli di cascata e quali problemi risolvono.

### Ordine di precedenza del livello di cascata

Analogamente a come abbiamo sei livelli di priorità basati su origine e importanza, i livelli di cascata ci consentono di creare un sottolivello di origine all'interno di ciascuna di quelle origini.

All'interno di ciascun bucket di origine, ci possono essere più livelli di cascata. L'[ordine di creazione dei livelli](/it/docs/Web/CSS/@layer) è molto importante. È l'ordine di creazione che determina l'ordine di precedenza tra i livelli all'interno di un'origine.

Nei bucket di origine normali, i livelli sono ordinati nell'ordine di creazione di ciascun livello. L'ordine di precedenza è dal primo livello creato all'ultimo, seguito dagli stili normali non stratificati.

Questo ordine è invertito per gli stili importanti. Tutti gli stili importanti non stratificati vengono uniti in un livello implicito che ha precedenza su tutti gli stili normali non in transizione. Gli stili importanti non stratificati hanno una precedenza inferiore rispetto a qualsiasi stile importante stratificato. Gli stili importanti nei livelli dichiarati prima hanno precedenza sugli stili importanti nei livelli dichiarati successivamente all'interno della stessa origine.

Per il resto di questo tutorial, ci limiteremo a discutere degli stili dell'autore, ma tieni presente che i livelli possono esistere anche nei fogli di stile utente e user-agent.

### Problemi che i livelli di cascata possono risolvere

Basi di codice di grandi dimensioni possono avere stili provenienti da più team, librerie di componenti, framework e terze parti. Non importa quanti fogli di stile siano inclusi, tutti questi stili si uniscono in una singola origine: il foglio di stile dell'_autore_.

Avere stili provenienti da molte fonti che si uniscono in cascata, specialmente da team che non lavorano insieme, può creare problemi. I diversi team possono avere differenti metodologie; uno può avere come pratica migliore quella di ridurre la specificità, mentre un altro può avere come standard includere un `id` in ciascun selettore.

I conflitti di specificità possono intensificarsi rapidamente. Uno sviluppatore web può creare una "soluzione rapida" aggiungendo un flag `!important`. Anche se questo può sembrare una soluzione facile, spesso sposta semplicemente la guerra della specificità dalle dichiarazioni normali a quelle importanti.

Nello stesso modo in cui le origini della cascata forniscono un equilibrio di potere tra stili dell'utente, user-agent e autore, i livelli di cascata forniscono un modo strutturato per organizzare e bilanciare le preoccupazioni all'interno di una singola origine come se ciascun livello in un'origine fosse un'ulteriore origine. Un livello può essere creato per ciascun team, componente e terza parte, con la precedenza degli stili basata sull'ordine dei livelli.

Le regole all'interno di un livello si uniscono in cascata insieme, senza competere con le regole di stile al di fuori del livello. I livelli di cascata consentono di dare priorità ai fogli di stile interi rispetto ad altri fogli di stile, senza dover preoccuparsi della specificità tra questi sottolivelli.

La precedenza dei livelli sempre batte la specificità del selettore. Gli stili nei livelli con precedenza "vincono" sui livelli con meno precedenza. La specificità di un selettore in un livello perdente è irrilevante. La specificità conta ancora per i valori di proprietà concorrenti all'interno di un livello, ma non ci sono preoccupazioni di specificità tra i livelli perché viene considerato solo il livello con la priorità più alta per ciascuna proprietà.

### Problemi che i livelli di cascata annidati possono risolvere

I livelli di cascata consentono la creazione di livelli annidati. Ciascun livello di cascata può contenere livelli annidati.

Per esempio, una libreria di componenti può essere importata in un livello `components`. Un livello di cascata regolare aggiungerà la libreria di componenti all'origine dell'autore, eliminando eventuali conflitti di specificità con altri stili dell'autore. All'interno del livello `components`, uno sviluppatore può scegliere di definire vari temi, ognuno come un livello annidato separato. L'ordine di questi livelli di temi annidati può essere definito in base a media queries (vedi la sezione [Creazione di livelli e media queries](#creazione_di_livelli_e_media_queries) sotto), come la dimensione del viewport o l'[orientamento](/it/docs/Web/CSS/@media/orientation). Questi livelli annidati forniscono un modo per creare temi che non entrano in conflitto basati sulla specificità.

La possibilità di annidare i livelli è molto utile per chi lavora allo sviluppo di librerie di componenti, framework, widget di terze parti e temi.

La possibilità di creare livelli annidati rimuove anche la preoccupazione di avere nomi di livelli in conflitto. Tratteremo questo nella sezione [livelli annidati](#panoramica_dei_livelli_di_cascata_annidati).

> "Gli autori possono creare livelli per rappresentare i valori predefiniti degli elementi, librerie di terze parti, temi, componenti, sovrascritture e altre possibili preoccupazioni di stile—e sono in grado di riordinare esplicitamente i livelli della cascata, senza alterare i selettori o la specificità all'interno di ciascun livello, o affidarsi all'ordine di apparizione per risolvere conflitti tra i livelli."
>
> —[Specifiche di Cascading e Inheritance](https://www.w3.org/TR/css-cascade-5/#layering).

## Creare livelli di cascata

I livelli possono essere creati utilizzando uno dei seguenti metodi:

- L'istruzione `@layer` con at-rule, dichiarando livelli usando `@layer` seguita dai nomi di uno o più livelli. Questo crea livelli nominati senza assegnare loro stili.
- Il blocco at-rule `@layer`, in cui tutti gli stili all'interno di un blocco sono aggiunti a un livello nominato o senza nome.
- La regola [`@import`](/it/docs/Web/CSS/@import) con la parola chiave `layer` o la funzione `layer()`, che assegna il contenuto del file importato a quel livello.

Tutti e tre i metodi creano un livello se un livello con quel nome non è già stato inizializzato. Se non viene fornito alcun nome di livello nella regola at-rule `@layer` o `@import` con `layer()`, viene creato un nuovo livello anonimo (senza nome).

> [!NOTE]
> L'ordine di precedenza dei livelli è l'ordine in cui vengono creati. Gli stili che non sono in un livello, o "stili non stratificati", si uniscono in un'etichetta implicita finale.

Copriamo i tre modi di creare un livello un po' più in dettaglio prima di discutere i livelli annidati.

### L'istruzione at-rule @layer per livelli nominati

L'ordine dei livelli è stabilito dall'ordine in cui i livelli appaiono nel tuo CSS. Dichiarare i livelli usando `@layer` seguito dai nomi di uno o più livelli senza assegnare alcun stile è un modo per definire l'[ordine dei livelli](#determinare_la_precedenza_basata_sull'ordine_dei_livelli).

La regola at-rule [`@layer`](/it/docs/Web/CSS/@layer) del CSS viene utilizzata per dichiarare un livello di cascata e per definire l'ordine di precedenza quando ci sono più livelli di cascata. Il seguente at-rule dichiara tre livelli, nell'ordine elencato:

```css
@layer theme, layout, utilities;
```

Spesso vuoi avere la tua prima riga di CSS come questa dichiarazione `@layer` (con nomi di livelli che abbiano senso per il tuo sito, ovviamente) per avere un controllo completo sull'ordinamento dei livelli.

Se l'istruzione sopra è la prima riga del CSS di un sito, l'ordine dei livelli sarà `theme`, `layout`, e `utilities`. Se alcuni livelli sono stati creati prima dell'istruzione sopra, finché i livelli con questi nomi non esistono già, questi tre livelli verranno creati e aggiunti alla fine dell'elenco dei livelli esistenti. Tuttavia, se esiste già un livello con lo stesso nome, l'istruzione sopra creerà solo due nuovi livelli. Per esempio, se `layout` già esisteva, verranno creati solo `theme` e `utilities`, ma l'ordine dei livelli, in questo caso, sarà `layout`, `theme`, e `utilities`.

### L'at-rule del blocco @layer per livelli nominati e anonimi

I livelli possono essere creati utilizzando l'at-rule del blocco `@layer`. Se un at-rule `@layer` è seguito da un identificatore e da un blocco di stili, l'identificatore viene utilizzato per nominare il livello, e gli stili in questo at-rule vengono aggiunti agli stili del livello. Se un livello con il nome specificato non esiste già, verrà creato un nuovo livello. Se un livello con il nome specificato esiste già, gli stili vengono aggiunti al livello precedentemente esistente. Se nessun nome è specificato durante la creazione di un blocco di stili usando `@layer`, gli stili nell'at-rule verranno aggiunti a un nuovo livello anonimo.

Nell'esempio qui sotto, abbiamo utilizzato quattro at-rule del blocco `@layer` e un at-rule dell'istruzione `@layer`. Questo CSS fa quanto segue nell'ordine elencato:

1. Crea un livello `layout` nominato
2. Crea un livello anonimo, senza nome
3. Dichiara un elenco di tre livelli e crea solo due nuovi livelli, `theme` e `utilities`, poiché `layout` esiste già
4. Aggiunge ulteriori stili al livello `layout` già esistente
5. Crea un secondo livello anonimo, senza nome

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

Nel CSS sopra, abbiamo creato cinque livelli: `layout`, `<anonymous(01)>`, `theme`, `utilities`, e `<anonymous(02)>` – in quell'ordine – con un sesto, livello implicito di stili non stratificati contenuti nel blocco di stile `body`. L'ordine dei livelli è l'ordine in cui vengono creati, con il livello implicito di stili non stratificati sempre ultimo. Non c'è modo di cambiare l'ordine del livello una volta creato.

Abbiamo assegnato alcuni stili al livello denominato `layout`. Se un livello nominato non esiste già, specificare il nome in un at-rule `@layer`, con o senza assegnare stili al livello, crea il livello; questo aggiunge il livello alla fine della serie di nomi di livelli esistenti. Se il livello nominato esiste già, tutti gli stili all'interno del blocco denominato vengono aggiunti agli stili nel livello precedentemente esistente – specificare gli stili in un blocco riutilizzando un nome di livello esistente non crea un nuovo livello.

I livelli anonimi vengono creati assegnando stili a un livello senza nominarlo. Gli stili possono essere aggiunti a un livello senza nome solo al momento della sua creazione.

> [!NOTE]
> L'uso successivo di `@layer` senza nome di livello crea ulteriori livelli anonimi; non appende stili a un livello anonimo precedentemente esistente.

L'at-rule `@layer` crea un livello, sia nominato che non, o appende stili a un livello se il livello nominato esiste già. Abbiamo chiamato il primo livello anonimo `<anonymous(01)>` e il secondo `<anonymous(02)>`, questo è solo per spiegare loro. Questi sono in realtà livelli senza nome. Non c'è modo di fare riferimento a loro o aggiungere ulteriori stili a loro.

Tutti gli stili dichiarati al di fuori di un livello vengono uniti insieme in un livello implicito. Nel codice di esempio sopra, la prima dichiarazione imposta la proprietà `color: #333` su `body`. Questo è stato dichiarato al di fuori di qualsiasi livello. Le dichiarazioni normali non stratificate hanno precedenza sulle dichiarazioni normali stratificate anche se gli stili non stratificati hanno una specificità inferiore e vengono prima nell'ordine apparente. Questo spiega perché anche se lo stile non stratificato è stato dichiarato per primo nel blocco di codice, il livello implicito contenente questi stili non stratificati ha precedenza come se fosse l'ultimo livello dichiarato.

Nella riga `@layer theme, layout, utilities;`, in cui una serie di livelli è stata dichiarata, solo i livelli `theme` e `utilities` sono stati creati; `layout` era già stato creato nella prima riga. Nota che questa dichiarazione non cambia l'ordine dei livelli già creati. Al momento non c'è modo di riordinare i livelli una volta dichiarati.

Nel seguente esempio, assegnamo stili a due livelli, creandoli e nominando loro nel processo. Poiché esistono già, essendo creati al primo utilizzo, dichiararli nell'ultima riga non produce alcun effetto.

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

Prova a spostare l'ultima riga, `@layer site, page;`, per renderla la prima riga. Cosa succede?

#### Creazione di livelli e media queries

Se definisci un livello utilizzando [media](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) o [feature](/it/docs/Web/CSS/CSS_conditional_rules/Using_feature_queries) queries, e il media non è corrispondente o la feature non è supportata, il livello non viene creato. L'esempio qui sotto mostra come cambiare la dimensione del tuo dispositivo o browser può cambiare l'ordine del livello. In questo esempio, creiamo il livello `site` solo nei browser più larghi. Quindi assegnamo stili ai livelli `page` e `site`, in quell'ordine.

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

Nei widescreen, il livello `site` viene dichiarato nella prima riga, il che significa che `site` ha una precedenza inferiore rispetto a `page`. Altrimenti, `site` ha precedenza su `page` perché viene dichiarato successivamente sugli schermi stretti. Se ciò non funziona, prova a cambiare il `50em` nella media query in `10em` o `100em`.

### Importazione di fogli di stile in livelli nominati e anonimi con @import

La regola [`@import`](/it/docs/Web/CSS/@import) consente agli utenti di importare regole di stile da altri fogli di stile direttamente in un file CSS o in un elemento {{htmlelement('style')}}.

Quando si importano fogli di stile, l'istruzione `@import` deve essere definita prima di qualsiasi stile CSS all'interno del foglio di stile o del blocco `<style>`. L'istruzione `@import` deve essere la prima, prima di qualsiasi stile, ma può essere preceduta da un at-rule `@layer` che crea uno o più livelli senza assegnare loro stili. (`@import` può anche essere preceduto da una regola [`@charset`](/it/docs/Web/CSS/@charset).)

Puoi importare un foglio di stile in un livello nominato, un livello annidato nominato o un livello anonimo. Il seguente livello importa i fogli di stile in un livello `components`, un livello `dialog` annidato all'interno del livello `components`, e un livello senza nome, rispettivamente:

```css
@import url("components-lib.css") layer(components);
@import url("dialog.css") layer(components.dialog);
@import url("marketing.css") layer();
```

Puoi importare più di un file CSS in un singolo livello. La seguente dichiarazione importa due file separati in un unico livello `social`:

```css
@import url(comments.css) layer(social);
@import url(sm-icons.css) layer(social);
```

Puoi importare stili e creare livelli in base a condizioni specifiche usando [media queries](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) e [feature queries](/it/docs/Web/CSS/CSS_conditional_rules/Using_feature_queries). Quanto segue importa un foglio di stile in un livello `international` solo se il browser supporta `display: ruby`, e il file importato dipende dalla larghezza dello schermo.

```css
@import url("ruby-narrow.css") layer(international) supports(display: ruby)
  (width < 32rem);
@import url("ruby-wide.css") layer(international) supports(display: ruby)
  (width >= 32rem);
```

> [!NOTE]
> Non c'è un equivalente del metodo {{HTMLElement('link')}} per collegare fogli di stile. Usa `@import` per importare un foglio di stile in un livello quando non puoi usare `@layer` all'interno del foglio di stile.

## Panoramica dei livelli di cascata annidati

I livelli annidati sono livelli all'interno di un livello nominato o anonimo. Ogni livello di cascata, anche uno anonimo, può contenere livelli annidati. I livelli importati in un altro livello diventano livelli annidati all'interno di quel livello.

### Vantaggi dell'annidamento dei livelli

La capacità di annidare i livelli consente ai team di creare livelli di cascata senza preoccuparsi se altri team li importeranno in un livello. Allo stesso modo, l'annidamento ti consente di importare fogli di stile di terze parti in un livello senza preoccuparti se quel foglio di stile stesso ha livelli. Poiché i livelli possono essere annidati, non devi preoccuparti di avere nomi di livelli in conflitto tra fogli di stile esterni e interni.

### Creare livelli di cascata annidati

I livelli annidati possono essere creati utilizzando gli stessi metodi descritti per i livelli regolari. Per esempio, possono essere creati usando l'at-rule `@layer` seguito dai nomi di uno o più livelli, usando una notazione a puntini. Punti multipli e nomi di livelli indicano annidamenti multipli.

Se annidi un blocco at-rule `@layer` all'interno di un altro blocco at-rule `@layer`, con o senza nome, il blocco annidato diventa un livello annidato. Allo stesso modo, quando un foglio di stile viene importato con una dichiarazione `@import` contenente la parola chiave `layer` o la funzione `layer()`, gli stili vengono assegnati a quel livello nominato o anonimo. Se la dichiarazione `@import` contiene livelli, quei livelli diventano livelli annidati all'interno di quel livello anonimo o nominato.

Esaminiamo il seguente esempio:

```css
@import url("components-lib.css") layer(components);
@import url("narrow-theme.css") layer(components.narrow);
```

Nella prima riga, importiamo `components-lib.css` nel livello `components`. Se quel file contiene livelli, nominati o no, quei livelli diventano livelli annidati all'interno del livello `components`.

La seconda riga importa `narrow-theme.css` nel livello `narrow`, che è un sottolivello di `components`. Il `components.narrow` annidato viene creato come ultimo livello all'interno del livello `components`, a meno che `components-lib.css` contenga già un livello `narrow`, nel qual caso, il contenuto di `narrow-theme.css` verrebbe aggiunto al `components.narrow` livello annidato. Ulteriori livelli annidati nominati possono essere aggiunti al livello `components` usando il modello `components.<layerName>`. Come già menzionato, i livelli senza nome possono essere creati ma non possono essere successivamente accessibili.

Esaminiamo un altro esempio, in cui [importiamo `layers1.css` in un livello nominato](#the_layer_block_at-rule_for_named_and_anonymous_layers) usando la seguente istruzione:

```css
@import url(layers1.css) layer(example);
```

Questo creerà un singolo livello denominato `example` contenente alcune dichiarazioni e cinque livelli annidati - `example.layout`, `example.<anonymous(01)>`, `example.theme`, `example.utilities`, e `example.<anonymous(02)>`.

Per aggiungere stili a un livello annidato nominato, usa la notazione a puntini:

```css
@layer example.layout {
  main {
    width: 50vw;
  }
}
```

## Determinare la precedenza basata sull'ordine dei livelli

L'ordine dei livelli determina il loro ordine di precedenza. Pertanto, l'ordine dei livelli è molto importante. Nello stesso modo in cui la cascata ordina per origine e importanza, la cascata ordina ogni dichiarazione CSS per origine, livello e importanza.

### Ordine di precedenza dei livelli di cascata regolari

```css
@import url(A.css) layer(firstLayer);
@import url(B.css) layer(secondLayer);
@import url(C.css);
```

Il codice sopra crea due livelli nominati (gli stili C.css vengono aggiunti al livello implicito degli stili non stratificati). Supponiamo che i tre file (`A.css`, `B.css`, e `C.css`) non contengano livelli aggiuntivi al loro interno. L'elenco seguente mostra dove gli stili dichiarati dentro e fuori questi file verranno ordinati da minore (1) a maggiore (10) precedenza.

1. Stili normali di `firstLayer` (`A.css`)
2. Stili normali di `secondLayer` (`B.css`)
3. Stili normali non stratificati (`C.css`)
4. Stili normali inline
5. Stili animati
6. Stili importanti non stratificati (`C.css`)
7. Stili importanti di `secondLayer` (`B.css`)
8. Stili importanti di `firstLayer` (`A.css`)
9. Stili importanti inline
10. Stili in transizione

Gli stili normali dichiarati all'interno dei livelli ricevono la priorità più bassa e sono ordinati in base all'ordine di creazione dei livelli. Gli stili normali nel primo livello creato hanno la precedenza più bassa, e gli stili normali nel livello creato per ultimo hanno la precedenza più alta tra i livelli. In altre parole, gli stili normali dichiarati all'interno di `firstLayer` verranno sostituiti da qualsiasi successivo styling nell'elenco se esiste un conflitto.

Successivamente ci sono gli stili dichiarati al di fuori dei livelli. Gli stili in `C.css` non sono stati importati in un livello e sovrascriveranno qualsiasi stile in conflitto proveniente da `firstLayer` e `secondLayer`. Gli stili non dichiarati in un livello hanno sempre una precedenza maggiore rispetto agli stili che _sono_ dichiarati all'interno di un livello (con l'eccezione degli stili importanti).

Gli stili inline sono dichiarati utilizzando l'[attributo `style`](/it/docs/Web/HTML/Reference/Global_attributes/style). Gli stili normali dichiarati in questo modo avranno precedenza sugli stili normali presenti nei fogli di stile non stratificati e stratificati (`firstLayer – A.css`, `secondLayer – B.css`, e `C.css`).

Gli stili animati hanno precedenza su tutti gli stili normali, compresi gli stili normali inline.

Gli stili importanti, cioè i valori delle proprietà che includono il flag `!important`, hanno precedenza su qualsiasi stile menzionato precedentemente nella nostra lista. Sono ordinati in ordine inverso rispetto agli stili normali. Qualsiasi stile importante dichiarato al di fuori di un livello ha meno precedenza rispetto a quelli dichiarati all'interno di un livello. Gli stili importanti trovati all'interno dei livelli sono anche ordinati in base all'ordine di creazione del livello. Per gli stili importanti, l'ultimo livello creato ha la precedenza più bassa e il primo creato ha la precedenza più alta tra i livelli dichiarati.

Gli stili importanti inline hanno di nuovo una precedenza più alta rispetto agli stili importanti dichiarati altrove.

Gli stili in transizione hanno la precedenza più alta. Quando un valore di proprietà normale è in transizione, ha precedenza su tutte le altre dichiarazioni di valore di proprietà, persino sugli stili importanti inline; ma solo mentre è in transizione.

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

In questo esempio, due livelli (`A` e `B`) sono inizialmente definiti utilizzando un at-rule `@layer` senza stili. Gli stili del livello sono definiti in due at-rule del blocco `@layer` che appaiono dopo la regola CSS `h1` dichiarata al di fuori di qualsiasi livello.

Gli stili inline aggiunti all'elemento `h1` utilizzando l'attributo `style`, impostano un `color` normale e un `background-color` importante. Gli stili normali inline sovrascrivono tutti gli stili normali stratificati e non stratificati. Gli stili importanti inline sovrascrivono tutti gli stili normali e importanti stratificati e non stratificati dell'autore. Non c'è modo per gli stili dell'autore di sovrascrivere gli stili importanti inline.

Il `text-decoration` normale e il `box-shadow` importante non fanno parte degli stili inline di `style` e possono quindi essere sovrascritti. Per gli stili normali non inline, gli stili non stratificati hanno precedenza. Per gli stili importanti, importa anche l'ordine del livello. Mentre gli stili normali non stratificati sovrascrivono tutti gli stili normali impostati in un livello, con gli stili importanti, l'ordine di precedenza è invertito; gli stili importanti non stratificati hanno una precedenza inferiore rispetto agli stili stratificati.

I due stili dichiarati solo all'interno dei livelli sono `font-style`, di importanza normale, e `font-weight` con un flag `!important`. Per gli stili normali, il livello `B`, dichiarato per ultimo, sovrascrive gli stili nel livello `A` dichiarato prima. Per gli stili normali, i livelli successivi hanno precedenza sui livelli precedenti. L'ordine di precedenza è invertito per gli stili importanti. Per le dichiarazioni importanti `font-weight`, il livello `A`, essendo dichiarato per primo, ha precedenza sul livello `B` dichiarato per ultimo.

Puoi invertire l'ordine dei livelli cambiando la prima linea da `@layer A, B;` a `@layer B, A;`. Prova a farlo. Quali stili cambiano in questo modo, e quali rimangono gli stessi? Perché?

L'ordine dei livelli è stabilito dall'ordine in cui i livelli appaiono nel tuo CSS. Nella nostra prima riga, abbiamo dichiarato livelli senza assegnare alcun stile usando `@layer` seguito dai nomi dei nostri livelli, terminando con un punto e virgola. Se avessimo omesso questa riga, i risultati sarebbero stati gli stessi. Perché? Abbiamo assegnato regole di stile in blocchi `@layer` nominati nell'ordine A poi B. I due livelli sono stati creati in quella prima riga. Se non lo fossero stati, questi blocchi di regole li avrebbero creati, in quell'ordine.

Abbiamo incluso quella prima riga per due motivi: prima così potresti modificare facilmente la linea e invertire l'ordine, e secondo perché spesso troverai dichiarare l'ordine del livello in anticipo come la migliore pratica per la gestione dell'ordine dei livelli.

Per riassumere:

- L'ordine di precedenza dei livelli è l'ordine in cui i livelli vengono creati.
- Una volta creati, non c'è modo di cambiare l'ordine dei livelli.
- La precedenza dei livelli per gli stili normali è l'ordine in cui i livelli vengono creati.
- Gli stili normali non stratificati hanno precedenza sugli stili normali stratificati.
- La precedenza dei livelli per gli stili importanti è invertita, con i livelli creati in precedenza che hanno precedenza.
- Tutti gli stili importanti stratificati hanno precedenza sugli stili importanti (e normali) non stratificati.
- Gli stili normali inline hanno precedenza su tutti gli stili normali, stratificati o no.
- Gli stili importanti inline hanno precedenza su tutti gli altri stili, a eccezione degli stili in transizione.
- Non c'è modo per gli stili dell'autore di sovrascrivere gli stili importanti inline (tranne che per transizionarli, il che è temporaneo).

### Ordine di precedenza dei livelli di cascata annidati

L'ordine di precedenza della cascata per i livelli annidati è simile a quello dei livelli regolari, ma contenuto all'interno del livello. L'ordine di precedenza è basato sull'ordine di creazione del livello annidato. Gli stili non annidati in un livello hanno precedenza sugli stili normali annidati, con l'ordine di precedenza inverso per gli stili importanti. Il peso di specificità tra livelli annidati non conta, anche se conta per gli stili in conflitto all'interno di un livello annidato.

Il seguente crea e aggiunge stili al livello `components`, al livello annidato `components.narrow` e al livello annidato `components.wide`:

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

Ecco un riassunto delle proprietà utilizzate e perché ciascuna dichiarazione viene applicata:

- `background-color`: Poiché gli stili normali non stratificati hanno precedenza sugli stili normali stratificati, vince il colore `wheat`.
- `border`: Poiché all'interno di un livello gli stili non annidati hanno precedenza sugli stili normali annidati, vince il colore `red`.
- `color`: Con gli stili importanti, gli stili stratificati hanno precedenza sugli stili non stratificati, con gli stili importanti nei livelli dichiarati in precedenza che hanno precedenza sui livelli dichiarati successivamente. In questo esempio, l'ordine di creazione dei livelli annidati è `components.narrow`, poi `components.wide`, quindi gli stili importanti in `components.narrow` hanno precedenza sugli stili importanti in `components.wide`, il che significa che vince il colore `purple`.
- `border-radius`: La proprietà è stata impostata solo nei livelli annidati quindi per ordine di dichiarazione vince un raggio del `20%`.

## Testa le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che tu abbia trattenuto queste informazioni prima di procedere — vedi [Testa le tue abilità: La Cascata, Compito 2](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade#task_2).

## Sommario

Se hai compreso la maggior parte di questo articolo, ottimo lavoro — sei ora familiare con le meccaniche fondamentali dei livelli di cascata CSS.
