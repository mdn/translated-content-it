---
title: FAQ CSS
short-title: FAQ
slug: Learn_web_development/Howto/Solve_CSS_problems/CSS_FAQ
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo troverai alcune domande frequenti (FAQ) sul CSS, insieme a risposte che possono aiutarti nel tuo percorso per diventare uno sviluppatore web.

## Perché il mio CSS, pur essendo valido, non viene reso correttamente?

I browser utilizzano la dichiarazione `doctype` per scegliere se mostrare il documento utilizzando una modalità più compatibile con gli standard Web o con i bug dei vecchi browser. Utilizzare una dichiarazione `doctype` corretta e moderna all'inizio del tuo HTML migliorerà la conformità agli standard del browser.

I browser moderni hanno due principali modalità di rendering:

- _Modalità Quirks_: chiamata anche modalità di retrocompatibilità, consente ai pagine legacy di essere mostrate come inteso dai loro autori, seguendo le regole di rendering non standard utilizzate dai browser più vecchi. I documenti con una dichiarazione `doctype` incompleta, errata o mancante, o una dichiarazione `doctype` conosciuta e utilizzata comunemente prima del 2001, verranno visualizzati in Modalità Quirks.
- _Modalità Standard_: il browser tenta di seguire strettamente gli standard W3C. Si prevede che le nuove pagine HTML siano progettate per browser conformi agli standard e, di conseguenza, le pagine con una dichiarazione `doctype` moderna verranno visualizzate in Modalità Standard.

I browser basati su Gecko hanno una terza [modalità quirks limitata](https://en.wikipedia.org/wiki/Quirks_mode#Limited_quirks_mode) che presenta solo alcune piccole eccezioni.

La dichiarazione `doctype` standard che attiverà la modalità standard è:

```html
<!doctype html>
```

Quando possibile, dovresti semplicemente utilizzare la `doctype` sopra. Ci sono altri `doctype` legacy validi che attiveranno la modalità Standard o Quasi Standard:

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

## Perché il mio CSS, pur essendo valido, non viene reso affatto?

Ecco alcune cause possibili:

- Hai sbagliato il percorso al file CSS.
- Per essere applicato, un foglio di stile CSS deve essere servito con un tipo MIME `text/css`. Se il server Web non lo serve con questo tipo, non verrà applicato.

## Qual è la differenza tra `id` e `class`?

Gli elementi HTML possono avere un attributo `id` e/o `class`. L'attributo `id` assegna un nome all'elemento a cui viene applicato e, per markup valido, può esserci solo un elemento con quel nome. L'attributo `class` assegna un nome di classe all'elemento e quel nome può essere utilizzato su molti elementi all'interno della pagina. CSS ti permette di applicare stili a nomi di `id` e/o `class` particolari.

- Usa uno stile specifico per classi quando vuoi applicare le regole di stile a molti blocchi ed elementi all'interno della pagina, o quando attualmente hai solo un elemento da stilizzare con quello stile, ma potrebbe essere necessario aggiungerne altri in seguito.
- Usa uno stile specifico per id quando hai bisogno di limitare le regole di stile applicate a un blocco specifico o elemento. Questo stile sarà utilizzato solo dall'elemento con quel particolare id.

Generalmente è consigliato usare le classi il più possibile e usare gli id solo quando assolutamente necessario per usi specifici (come connettere etichette e elementi di form o per stilizzare elementi che devono essere semanticamente unici):

- Usare classi rende il tuo stile estensibile — anche se hai solo un elemento da stilizzare con un particolare insieme di regole adesso, potresti voler aggiungerne altri in futuro.
- Le classi permettono di stilizzare più elementi, quindi possono portare a fogli di stile più brevi, piuttosto che scrivere le stesse informazioni di stile in più regole che utilizzano selettori di id. I fogli di stile più brevi sono più performanti.
- I selettori di classi hanno una [specificità](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity) inferiore rispetto ai selettori di id, quindi sono più facili da sovrascrivere se necessario.

> [!NOTE]
> Consulta [Selettori](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) per ulteriori informazioni.

## Come faccio a ripristinare il valore di default di una proprietà?

Inizialmente il CSS non forniva una parola chiave "default" e l'unico modo per ripristinare il valore di default di una proprietà era dichiarare nuovamente quella proprietà esplicitamente. Ad esempio:

```css
/* Heading default color is black */
h1 {
  color: red;
}
h1 {
  color: black;
}
```

Questo è cambiato con CSS 2; la parola chiave [initial](/it/docs/Web/CSS/initial) è ora un valore valido per una proprietà CSS. La resetta al suo valore di default, che è definito nella specifica CSS della data proprietà.

```css
/* Heading default color is black */
h1 {
  color: red;
}
h1 {
  color: initial;
}
```

## Come faccio a derivare uno stile da un altro?

Il CSS non permette esattamente di definire uno stile in termini di un altro. Tuttavia, assegnare più classi a un singolo elemento può fornire lo stesso effetto e [le variabili CSS](/it/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties) ora forniscono un modo per definire informazioni di stile in un unico luogo che possono essere riutilizzate in più posti.

## Come faccio ad assegnare più classi a un elemento?

Gli elementi HTML possono essere assegnati a più classi elencandole nell'attributo `class`, con uno spazio vuoto per separarle.

```html
<style>
  .news {
    background: black;
    color: white;
  }
  .today {
    font-weight: bold;
  }
</style>

<div class="news today">Content of today's news goes here.</div>
```

Se la stessa proprietà è dichiarata in entrambe le regole, il conflitto è risolto prima tramite specificità, poi secondo l'ordine delle dichiarazioni CSS. L'ordine delle classi nell'attributo `class` non è rilevante.

## Perché le mie regole di stile non funzionano correttamente?

Le regole di stile che sono sintatticamente corrette potrebbero non applicarsi in determinate situazioni. Puoi utilizzare [la vista delle regole](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html) di _CSS Pane_ dell'Inspector per debugare problemi di questo tipo, ma i casi più frequenti di regole di stile ignorate sono elencati di seguito.

### Gerarchia degli elementi HTML

Il modo in cui gli stili CSS sono applicati agli elementi HTML dipende anche dalla gerarchia degli elementi. È importante ricordare che una regola applicata a un discendente sovrascrive lo stile del genitore, nonostante qualsiasi specificità o priorità delle regole CSS.

```css
.news {
  color: black;
}
.corpName {
  font-weight: bold;
  color: red;
}
```

```html
<!-- news item text is black, but corporate name is red and in bold -->
<div class="news">
  (Reuters) <span class="corpName">General Electric</span> (GE.NYS) announced on
  Thursday…
</div>
```

In caso di gerarchie HTML complesse, se una regola sembra essere ignorata, controlla se l'elemento è all'interno di un altro elemento con uno stile diverso.

### Regola di stile ridefinita esplicitamente

Nei fogli di stile CSS, l'ordine **è** importante. Se definisci una regola e poi la ridefinisci, l'ultima definizione viene usata.

```css
#stockTicker {
  font-weight: bold;
}
.stockSymbol {
  color: red;
}
/*  other rules             */
/*  other rules             */
/*  other rules             */
.stockSymbol {
  font-weight: normal;
}
```

```html
<!-- most text is in bold, except "GE", which is red and not bold -->
<div id="stockTicker">NYS: <span class="stockSymbol">GE</span> +1.0…</div>
```

Per evitare questo tipo di errore, prova a definire le regole solo una volta per un certo selettore e raggruppa tutte le regole appartenenti a quel selettore.

### Uso di una proprietà abbreviata

Usare le proprietà abbreviate per definire le regole di stile è buono perché utilizza una sintassi molto compatta. Usare le abbreviazioni con solo alcuni attributi è possibile e corretto, ma occorre ricordare che gli attributi non dichiarati vengono automaticamente riportati ai loro valori di default. Questo significa che una precedente regola per un singolo attributo potrebbe essere implicitamente sovrascritta.

```css
#stockTicker {
  font-size: 12px;
  font-family: Verdana;
  font-weight: bold;
}
.stockSymbol {
  font: 14px Arial;
  color: red;
}
```

```html
<div id="stockTicker">NYS: <span class="stockSymbol">GE</span> +1.0…</div>
```

Nell'esempio precedente il problema si verificava per regole appartenenti a elementi diversi, ma potrebbe succedere anche per lo stesso elemento, perché l'ordine delle regole **è** importante.

```css
#stockTicker {
  font-weight: bold;
  font: 12px Verdana; /* font-weight is now set to normal */
}
```

### Uso del selettore `*`

Il selettore jolly `*` si riferisce a qualsiasi elemento e deve essere utilizzato con particolare attenzione.

```css
body * {
  font-weight: normal;
}
#stockTicker {
  font: 12px Verdana;
}
.corpName {
  font-weight: bold;
}
.stockUp {
  color: red;
}
```

```html
<div id="section">
  NYS: <span class="corpName"><span class="stockUp">GE</span></span> +1.0…
</div>
```

In questo esempio il selettore `body *` applica la regola a tutti gli elementi all'interno del corpo, a qualsiasi livello gerarchico, inclusa la classe `.stockUp`. Quindi `font-weight: bold;` applicato alla classe `.corpName` è sovrascritto da `font-weight: normal;` applicato a tutti gli elementi nel corpo.

L'uso del selettore \* dovrebbe essere minimizzato poiché è un selettore lento, specialmente quando non è utilizzato come primo elemento di un selettore. Il suo utilizzo dovrebbe essere evitato il più possibile.

### Specificità nel CSS

Quando più regole si applicano a un certo elemento, la regola scelta dipende dalla sua [specificità](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity). Lo stile inline (negli attributi `style` HTML) ha la specificità più alta e sovrascriverà qualsiasi selettore, seguito dai selettori ID, poi dai selettori di classe, e infine dai selettori di elemento. Il colore del testo del seguente {{htmlelement("div")}} sarà quindi rosso.

```css
div {
  color: black;
}
#orange {
  color: orange;
}
.green {
  color: green;
}
```

```html
<div id="orange" class="green" style="color: red;">This is red</div>
```

Le regole sono più complicate quando il selettore ha più parti. Una spiegazione più dettagliata su come viene calcolata la specificità del selettore può essere trovata nella [documentazione sulla specificità CSS](/it/docs/Web/CSS/CSS_cascade/Specificity).

## Cosa fanno le proprietà -moz-\*, -ms-\*, -webkit-\*, -o-\* e -khtml-\*?

Queste proprietà, chiamate _proprietà con prefisso_, sono estensioni allo standard CSS. Un tempo erano utilizzate per consentire l'uso di funzionalità sperimentali e non standard nei browser senza inquinare il namespace regolare, prevenendo l'insorgere di future incompatibilità quando lo standard viene esteso.

L'uso di tali proprietà su siti web di produzione non è raccomandato — hanno già creato un grande disordine di compatibilità web. Ad esempio, molti sviluppatori usano solo la versione con prefisso `-webkit-` di una proprietà quando la versione senza prefisso è completamente supportata su tutti i browser. Questo significa che un design che si basa su quella proprietà non funzionerebbe nei browser non basati su webkit, quando potrebbe. Questo è diventato un problema sufficientemente grande da spingere altri browser a implementare alias con prefisso `-webkit-` per migliorare la compatibilità web, come specificato nel [Compatibilità Living Standard](https://compat.spec.whatwg.org/).

I browser non usano più i prefissi CSS quando implementano nuove funzionalità sperimentali. Piuttosto, testano le nuove funzionalità dietro configurazioni sperimentali configurabili o solo nelle versioni Nightly dei browser o simili.

Se devi usare i prefissi nel tuo lavoro, scrivi le versioni con prefisso prima seguite dalla versione standard senza prefisso. In questo modo la versione standard sovrascriverà automaticamente le versioni con prefisso quando supportata. Ad esempio:

```css
-webkit-text-stroke: 4px navy;
text-stroke: 4px navy;
```

> [!NOTE]
> Consulta le [Estensioni CSS di Mozilla](/it/docs/Web/CSS/Mozilla_Extensions) e le [Estensioni CSS di WebKit](/it/docs/Web/CSS/WebKit_Extensions) per elenchi di proprietà CSS con prefisso browser.

## Come si relaziona z-index al posizionamento?

La proprietà `z-index` specifica l'ordine di impilamento degli elementi.

Un elemento con un ordine di impilamento/z-index più alto viene sempre mostrato davanti a un elemento con un ordine di impilamento/z-index più basso sullo schermo. Z-index funzionerà solo su elementi che hanno una posizione specificata (`position:absolute`, `position:relative`, o `position:fixed`).

> [!NOTE]
> Per ulteriori informazioni, consulta il nostro articolo formativo [Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Positioning), e in particolare la sezione [Introduzione a z-index](/it/docs/Learn_web_development/Core/CSS_layout/Positioning#introducing_z-index).
