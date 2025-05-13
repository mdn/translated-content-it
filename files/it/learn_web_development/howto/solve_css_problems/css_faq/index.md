---
title: CSS FAQ
short-title: FAQ
slug: Learn_web_development/Howto/Solve_CSS_problems/CSS_FAQ
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, troverà alcune delle domande frequenti (FAQ) su CSS, insieme a risposte che possono aiutarla nel suo percorso per diventare uno sviluppatore web.

## Perché il mio CSS, che è valido, non viene reso correttamente?

I browser utilizzano la dichiarazione `doctype` per decidere se mostrare il documento usando una modalità che è più compatibile con gli standard Web o con vecchi bug del browser. Utilizzare una dichiarazione `doctype` corretta e moderna all'inizio del suo HTML migliorerà la conformità agli standard del browser.

I browser moderni hanno due modalità principali di rendering:

- _Modalità Quirks_: chiamata anche modalità di compatibilità retrospettiva, consente di visualizzare pagine web legacy come inteso dai loro autori, seguendo le regole di rendering non standard usate dai browser più vecchi. I documenti con una dichiarazione `doctype` incompleta, errata o mancante, o con una dichiarazione `doctype` conosciuta di uso comune prima del 2001, verranno resi in Modalità Quirks.
- _Modalità Standard_: il browser tenta di seguire rigorosamente gli standard del W3C. Le nuove pagine HTML devono essere progettate per browser conformi agli standard e, di conseguenza, le pagine con una dichiarazione `doctype` moderna verranno rese in Modalità Standard.

I browser basati su Gecko hanno una terza modalità di [quirks limitata](https://en.wikipedia.org/wiki/Quirks_mode#Limited_quirks_mode) che presenta solo pochi difetti minori.

La dichiarazione standard `doctype` che attiverà la modalità standard è:

```html
<!doctype html>
```

Quando possibile, dovrebbe semplicemente usare il doctype sopra indicato. Ci sono altri doctype legacy validi che attiveranno la Modalità Standard o Quasi Standard:

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

## Perché il mio CSS, che è valido, non viene reso affatto?

Ecco alcune possibili cause:

- Ha sbagliato il percorso del file CSS.
- Per essere applicato, un foglio di stile CSS deve essere servito con un tipo MIME `text/css`. Se il server Web non lo serve con questo tipo, non verrà applicato.

## Qual è la differenza tra `id` e `class`?

Gli elementi HTML possono avere un attributo `id` e/o `class`. L'attributo `id` assegna un nome all'elemento a cui è applicato, e per un markup valido, può esserci solo un elemento con quel nome. L'attributo `class` assegna un nome di classe all'elemento, e quel nome può essere usato su molti elementi all'interno della pagina. CSS consente di applicare stili a nomi di `id` e/o `class` particolari.

- Usi uno stile specifico di classe quando desidera applicare le regole di stile a molti blocchi ed elementi all'interno della pagina, o quando attualmente ha solo un elemento da stilizzare con quello stile, ma potrebbe voler aggiungerne di più in seguito.
- Usi uno stile specifico di `id` quando ha bisogno di limitare le regole di stile applicate a un blocco o elemento specifico. Questo stile verrà utilizzato solo dall'elemento con quell'id particolare.

È generalmente consigliato usare le classi il più possibile e usare gli id solo quando assolutamente necessario per usi specifici (come connettere elementi etichetta e moduli o per stilizzare elementi che devono essere semanticamente unici):

- Usare le classi rende il suo stile estendibile — anche se attualmente ha solo un elemento da stilizzare con un determinato insieme di regole, potrebbe voler aggiungerne di più in seguito.
- Le classi le consentono di stilizzare più elementi, pertanto possono portare a fogli di stile più brevi, piuttosto che dover scrivere le stesse informazioni di stile in più regole che utilizzano selettori di `id`. Fogli di stile più brevi sono più performanti.
- I selettori di classi hanno una [specificità](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity) inferiore rispetto ai selettori di id, quindi sono più facili da sovrascrivere se necessario.

> [!NOTE]
> Vedere [Selettori](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) per ulteriori informazioni.

## Come ripristino il valore predefinito di una proprietà?

Inizialmente CSS non forniva una parola chiave "default" e l'unico modo per ripristinare il valore predefinito di una proprietà era ridefinire esplicitamente quella proprietà. Ad esempio:

```css
/* Heading default color is black */
h1 {
  color: red;
}
h1 {
  color: black;
}
```

Questo è cambiato con CSS 2; la parola chiave [initial](/it/docs/Web/CSS/initial) è ora un valore valido per una proprietà CSS. Ripristina il suo valore predefinito, che è definito nella specifica CSS della proprietà data.

```css
/* Heading default color is black */
h1 {
  color: red;
}
h1 {
  color: initial;
}
```

## Come derivo uno stile da un altro?

CSS non consente esattamente che uno stile sia definito in termini di un altro. Tuttavia, assegnare più classi a un singolo elemento può fornire lo stesso effetto, e [Variabili CSS](/it/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties) ora forniscono un modo per definire informazioni di stile in un'unica posizione che può essere riutilizzata in più punti.

## Come assegno più classi a un elemento?

Gli elementi HTML possono essere assegnati a più classi elencando le classi nell'attributo `class`, con uno spazio vuoto per separarle.

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

Se la stessa proprietà è dichiarata in entrambe le regole, il conflitto viene risolto prima attraverso la specificità, poi secondo l'ordine delle dichiarazioni CSS. L'ordine delle classi nell'attributo `class` non è rilevante.

## Perché le mie regole di stile non funzionano correttamente?

Regole di stile che sono sintatticamente corrette potrebbero non applicarsi in certe situazioni. Può usare la [vista delle regole](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html) del _Pannello CSS_ dell'Ispettore per debuggare problemi di questo tipo, ma i casi più frequenti di regole di stile ignorate sono elencati di seguito.

### Gerarchia degli elementi HTML

Il modo in cui gli stili CSS vengono applicati agli elementi HTML dipende anche dalla gerarchia degli elementi. È importante ricordare che una regola applicata a un discendente sovrascrive lo stile del genitore, a prescindere dalla specificità o priorità delle regole CSS.

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

In caso di gerarchie HTML complesse, se una regola sembra essere ignorata, verifichi se l'elemento si trova all'interno di un altro elemento con uno stile diverso.

### Regola di stile ridefinita esplicitamente

Nei fogli di stile CSS, l'ordine **è** importante. Se definisce una regola e poi ridefinisce la stessa regola, verrà utilizzata l'ultima definizione.

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

Per evitare questo tipo di errore, cerchi di definire le regole una sola volta per un certo selettore e raggruppi tutte le regole appartenenti a quel selettore.

### Uso di una proprietà abbreviata

Usare proprietà abbreviate per definire regole di stile è utile perché utilizza una sintassi molto compatta. Usare l'abbreviazione con solo alcuni attributi è possibile e corretto, ma deve ricordare che gli attributi non dichiarati vengono automaticamente ripristinati ai loro valori predefiniti. Ciò significa che una regola precedente per un singolo attributo potrebbe essere implicitamente sovrascritta.

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

Nell'esempio precedente il problema si è verificato su regole appartenenti a elementi diversi, ma potrebbe accadere anche per lo stesso elemento, poiché l'ordine delle regole **è** importante.

```css
#stockTicker {
  font-weight: bold;
  font: 12px Verdana; /* font-weight is now set to normal */
}
```

### Uso del selettore `*`

Il selettore wildcard `*` si riferisce a qualsiasi elemento e deve essere usato con particolare attenzione.

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

In questo esempio il selettore `body *` applica la regola a tutti gli elementi all'interno del body, a qualsiasi livello gerarchico, inclusa la classe `.stockUp`. Così `font-weight: bold;` applicato alla classe `.corpName` è sovrascritto da `font-weight: normal;` applicato a tutti gli elementi nel body.

L'uso del selettore \* dovrebbe essere minimizzato poiché è un selettore lento, specialmente quando non è usato come primo elemento di un selettore. Il suo uso dovrebbe essere evitato il più possibile.

### Specificità nel CSS

Quando più regole si applicano a un certo elemento, la regola scelta dipende dalla sua [specificità](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity) nello stile. Lo stile inline (negli attributi `style` HTML) ha la massima specificità e sovrascriverà qualsiasi selettore, seguito dai selettori di ID, poi dai selettori di classi, e infine dai selettori di elementi. Il colore del testo del seguente {{htmlelement("div")}} sarà quindi rosso.

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

Le regole sono più complicate quando il selettore ha più parti. Una spiegazione più dettagliata su come viene calcolata la specificità del selettore si trova nella [documentazione sulla specificità del CSS](/it/docs/Web/CSS/CSS_cascade/Specificity).

## Cosa fanno le proprietà -moz-\*, -ms-\*, -webkit-\*, -o-\* e -khtml-\*?

Queste proprietà, chiamate _proprietà con prefisso_, sono estensioni allo standard CSS. Una volta erano usate per permettere l'uso di funzionalità sperimentali e non standard nei browser senza inquinare il namespace regolare, prevenendo future incompatibilità quando lo standard veniva esteso.

L'uso di tali proprietà su siti web di produzione non è raccomandato — hanno già creato un enorme disordine di compatibilità web. Ad esempio, molti sviluppatori utilizzano solo la versione con prefisso `-webkit-` di una proprietà quando la versione senza prefisso è completamente supportata su tutti i browser. Ciò significa che un design che si basa su quella proprietà non funzionerebbe nei browser non basati su webkit, quando potrebbe. Questo è diventato un problema abbastanza grande da spingere altri browser a implementare alias con prefisso `-webkit-` per migliorare la compatibilità web, come specificato nel [Compatibility Living Standard](https://compat.spec.whatwg.org/).

I browser non utilizzano più i prefissi CSS quando implementano nuove funzionalità sperimentali. Piuttosto, testano nuove funzionalità dietro flag sperimentali configurabili o solo su versioni Nightly del browser o simili.

Se è necessario l'uso dei prefissi nel suo lavoro, scriva prima le versioni con prefisso seguite dalla versione standard senza prefisso. In questo modo la versione standard sovrascriverà automaticamente le versioni con prefisso quando supportata. Ad esempio:

```css
-webkit-text-stroke: 4px navy;
text-stroke: 4px navy;
```

> [!NOTE]
> Vedere le [Estensioni CSS di Mozilla](/it/docs/Web/CSS/Mozilla_Extensions) e le [Estensioni CSS di WebKit](/it/docs/Web/CSS/WebKit_Extensions) per elenchi di proprietà CSS con prefisso del browser.

## Come si relaziona lo z-index al posizionamento?

La proprietà `z-index` specifica l'ordine di sovrapposizione degli elementi.

Un elemento con un z-index/ordine di sovrapposizione più alto viene sempre reso davanti a un elemento con un z-index/ordine di sovrapposizione più basso sullo schermo. Z-index funzionerà solo su elementi che hanno una posizione specificata (`position:absolute`, `position:relative`, o `position:fixed`).

> [!NOTE]
> Per ulteriori informazioni, vedere il nostro articolo di apprendimento [Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Positioning), e in particolare la sezione [Introduzione allo z-index](/it/docs/Learn_web_development/Core/CSS_layout/Positioning#introducing_z-index).
