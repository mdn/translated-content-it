---
title: FAQ su CSS
short-title: FAQ
slug: Learn_web_development/Howto/Solve_CSS_problems/CSS_FAQ
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

In questo articolo sono disponibili alcune domande frequenti (FAQ) su CSS, accompagnate da risposte che possono aiutare nel percorso per diventare sviluppatori web.

## Perché il mio CSS, pur essendo valido, non viene visualizzato correttamente?

I browser usano la dichiarazione `doctype` per scegliere se mostrare il documento usando una modalità più compatibile con gli standard Web o con i bug dei vecchi browser. Usare una dichiarazione `doctype` corretta e moderna all'inizio dell'HTML migliorerà la conformità agli standard del browser.

I browser moderni dispongono di due modalità di rendering principali:

- _Quirks Mode_: detta anche modalità di compatibilità con le versioni precedenti, consente il rendering delle pagine web legacy nel modo previsto dai loro autori, seguendo le regole di rendering non standard utilizzate dai browser meno recenti. I documenti con una dichiarazione `doctype` incompleta, errata o assente, oppure una dichiarazione `doctype` nota e comunemente usata prima del 2001, verranno visualizzati in Quirks Mode.
- _Standards Mode_: il browser tenta di seguire rigorosamente gli standard W3C. Ci si aspetta che le nuove pagine HTML siano progettate per browser conformi agli standard e, di conseguenza, le pagine con una dichiarazione `doctype` moderna verranno visualizzate in Standards Mode.

I browser basati su Gecko dispongono di una terza [modalità quirks limitata](https://en.wikipedia.org/wiki/Quirks_mode#Limited_quirks_mode), che presenta solo alcune anomalie minori.

La dichiarazione `doctype` standard che attiverà la modalità standards è:

```html
<!doctype html>
```

Quando possibile, è consigliabile usare semplicemente il doctype riportato sopra. Esistono altri doctypes legacy validi che attiveranno la modalità Standards o Almost Standards:

```html
<!doctype html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

```html
<!doctype html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

```html
<!doctype html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

```html
<!doctype html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

## Perché il mio CSS, pur essendo valido, non viene visualizzato affatto?

Ecco alcune possibili cause:

- Il percorso al file CSS non è corretto.
- Per essere applicato, un foglio di stile CSS deve essere servito con un tipo MIME `text/css`. Se il server Web non lo serve con questo tipo, non verrà applicato.

## Qual è la differenza tra `id` e `class`?

Gli elementi HTML possono avere un attributo `id` e/o `class`. L'attributo `id` assegna un nome all'elemento a cui è applicato e, per un markup valido, può esistere un solo elemento con quel nome. L'attributo `class` assegna un nome di classe all'elemento e tale nome può essere usato su molti elementi nella pagina. CSS consente di applicare stili a particolari nomi `id` e/o `class`.

- Usare uno stile specifico per una classe quando si desidera applicare le regole di stile a molti blocchi ed elementi nella pagina, oppure quando al momento esiste un solo elemento da stilizzare con quello stile ma potrebbe essere necessario aggiungerne altri in seguito.
- Usare uno stile specifico per un id quando è necessario limitare le regole di stile applicate a un blocco o elemento specifico. Questo stile verrà usato solo dall'elemento con quel particolare id.

In generale, è consigliabile usare le classi il più possibile e usare gli id solo quando strettamente necessario per impieghi specifici, ad esempio per collegare elementi label e form o per stilizzare elementi che devono essere semanticamente univoci:

- L'uso delle classi rende gli stili estensibili: anche se al momento esiste un solo elemento da stilizzare con un determinato insieme di regole, potrebbe essere necessario aggiungerne altri in seguito.
- Le classi consentono di stilizzare più elementi e possono quindi portare a fogli di stile più brevi, invece di dover scrivere le stesse informazioni di stile in più regole che usano selettori id. I fogli di stile più brevi offrono prestazioni migliori.
- I selettori di classe hanno una [specificità](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity) inferiore rispetto ai selettori id, quindi sono più facili da sovrascrivere se necessario.

> [!NOTE]
> Per ulteriori informazioni, consultare [Selettori](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors).

## Come ripristinare il valore predefinito di una proprietà?

Inizialmente CSS non forniva una parola chiave "default" e l'unico modo per ripristinare il valore predefinito di una proprietà era dichiarare nuovamente in modo esplicito quella proprietà. Ad esempio:

```css
/* Heading default color is black */
h1 {
  color: red;
}
h1 {
  color: black;
}
```

Questo è cambiato con CSS 2; la parola chiave {{cssxref("initial")}} è ora un valore valido per una proprietà CSS. La reimposta al suo valore predefinito, definito nella specifica CSS della proprietà indicata.

```css
/* Heading default color is black */
h1 {
  color: red;
}
h1 {
  color: initial;
}
```

## Come derivare uno stile da un altro?

CSS non consente esattamente di definire uno stile in termini di un altro. Tuttavia, assegnare più classi a un singolo elemento può produrre lo stesso effetto e le [variabili CSS](/it/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties) forniscono ora un modo per definire informazioni di stile in un unico punto e riutilizzarle in più punti.

## Come assegnare più classi a un elemento?

Agli elementi HTML possono essere assegnate più classi elencandole nell'attributo `class`, con uno spazio vuoto per separarle.

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

Se la stessa proprietà viene dichiarata in entrambe le regole, il conflitto viene risolto prima attraverso la specificità, quindi in base all'ordine delle dichiarazioni CSS. L'ordine delle classi nell'attributo `class` non è rilevante.

## Perché le mie regole di stile non funzionano correttamente?

Le regole di stile sintatticamente corrette potrebbero non essere applicate in determinate situazioni. È possibile usare la [vista Rules](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html) del _riquadro CSS_ dell'Inspector per eseguire il debug di problemi di questo tipo, ma di seguito sono elencati i casi più frequenti di regole di stile ignorate.

### Gerarchia degli elementi HTML

Il modo in cui gli stili CSS vengono applicati agli elementi HTML dipende anche dalla gerarchia degli elementi. È importante ricordare che una regola applicata a un discendente sovrascrive lo stile del genitore, indipendentemente da qualsiasi specificità o priorità delle regole CSS.

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

In caso di gerarchie HTML complesse, se una regola sembra essere ignorata, verificare se l'elemento si trova all'interno di un altro elemento con uno stile diverso.

### Regola di stile ridefinita esplicitamente

Nei fogli di stile CSS, l'ordine **è** importante. Se viene definita una regola e poi viene ridefinita la stessa regola, viene usata l'ultima definizione.

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

Per evitare questo tipo di errore, cercare di definire le regole una sola volta per un determinato selettore e raggruppare tutte le regole appartenenti a quel selettore.

### Uso di una proprietà shorthand

L'uso delle proprietà shorthand per definire le regole di stile è utile perché impiega una sintassi molto compatta. Usare una shorthand con solo alcuni attributi è possibile e corretto, ma occorre ricordare che gli attributi non dichiarati vengono automaticamente reimpostati ai loro valori predefiniti. Ciò significa che una regola precedente per un singolo attributo potrebbe essere sovrascritta implicitamente.

```css
#stockTicker {
  font-size: 12px;
  font-family: "Verdana";
  font-weight: bold;
}
.stockSymbol {
  font: 14px "Arial";
  color: red;
}
```

```html
<div id="stockTicker">NYS: <span class="stockSymbol">GE</span> +1.0…</div>
```

Nell'esempio precedente il problema si è verificato in regole appartenenti a elementi diversi, ma potrebbe verificarsi anche per lo stesso elemento, poiché l'ordine delle regole **è** importante.

```css
#stockTicker {
  font-weight: bold;
  font: 12px "Verdana"; /* font-weight is now set to normal */
}
```

### Uso del selettore `*`

Il selettore wildcard `*` si riferisce a qualsiasi elemento e deve essere usato con particolare attenzione.

```css
body * {
  font-weight: normal;
}
#stockTicker {
  font: 12px "Verdana";
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

In questo esempio il selettore `body *` applica la regola a tutti gli elementi all'interno di body, a qualsiasi livello della gerarchia, compresa la classe `.stockUp`. Pertanto, `font-weight: bold;` applicato alla classe `.corpName` viene sovrascritto da `font-weight: normal;` applicato a tutti gli elementi nel body.

L'uso del selettore \* dovrebbe essere ridotto al minimo, poiché è un selettore lento, soprattutto quando non viene usato come primo elemento di un selettore. Il suo uso dovrebbe essere evitato quanto più possibile.

### Specificità in CSS

Quando più regole si applicano a un determinato elemento, la regola scelta dipende dalla sua [specificità](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity) di stile. Lo stile inline, negli attributi HTML `style`, ha la specificità più elevata e sovrascrive qualsiasi selettore, seguito dai selettori ID, quindi dai selettori di classe e infine dai selettori di elemento. Il colore del testo del {{htmlelement("div")}} seguente sarà quindi rosso.

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

Le regole sono più complesse quando il selettore ha più parti. Una spiegazione più dettagliata di come viene calcolata la specificità del selettore è disponibile nella [documentazione sulla specificità CSS](/it/docs/Web/CSS/Guides/Cascade/Specificity).

## Cosa fanno le proprietà -moz-\*, -ms-\*, -webkit-\*, -o-\* e -khtml-\*?

Queste proprietà, chiamate _proprietà con prefisso_, sono estensioni dello standard CSS. Un tempo venivano usate per consentire l'uso di funzionalità sperimentali e non standard nei browser senza inquinare lo spazio dei nomi regolare, evitando l'insorgere di future incompatibilità quando lo standard veniva esteso.

L'uso di tali proprietà nei siti web di produzione non è consigliato: hanno già creato un enorme problema di compatibilità web. Ad esempio, molti sviluppatori usano solo la versione con prefisso `-webkit-` di una proprietà quando la versione senza prefisso è pienamente supportata da tutti i browser. Ciò significa che un design basato su quella proprietà non funzionerebbe nei browser non basati su webkit, quando potrebbe farlo. Il problema è diventato sufficientemente grande da spingere altri browser a implementare alias con prefisso `-webkit-` per migliorare la compatibilità web, come specificato nel [Compatibility Living Standard](https://compat.spec.whatwg.org/).

I browser non usano più prefissi CSS quando implementano nuove funzionalità sperimentali. Testano invece le nuove funzionalità dietro flag sperimentali configurabili oppure solo nelle versioni Nightly dei browser o versioni simili.

Se è necessario usare prefissi nel proprio lavoro, scrivere prima le versioni con prefisso, seguite dalla versione standard senza prefisso. In questo modo la versione standard sovrascriverà automaticamente le versioni con prefisso quando supportata. Ad esempio:

```css
-webkit-border-after-color: navy;
border-block-end-color: navy;
```

> [!NOTE]
> Consultare [Mozilla CSS Extensions](/it/docs/Web/CSS/Reference/Mozilla_extensions) e [WebKit CSS Extensions](/it/docs/Web/CSS/Reference/Webkit_extensions) per gli elenchi delle proprietà CSS con prefisso del browser.

## In che modo z-index è correlato al posizionamento?

La proprietà `z-index` specifica l'ordine di sovrapposizione degli elementi.

Un elemento con un ordine z-index/di sovrapposizione superiore viene sempre visualizzato davanti a un elemento con un ordine z-index/di sovrapposizione inferiore sullo schermo. Z-index funziona solo sugli elementi che hanno una posizione specificata (`position:absolute`, `position:relative` o `position:fixed`).

> [!NOTE]
> Per ulteriori informazioni, consultare il nostro articolo di apprendimento sul [posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Positioning), in particolare la sezione [Introduzione a z-index](/it/docs/Learn_web_development/Core/CSS_layout/Positioning#introducing_z-index).
