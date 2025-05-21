---
title: Utilizzare il contenuto generato da CSS
short-title: Utilizzare il contenuto generato
slug: Learn_web_development/Howto/Solve_CSS_problems/Generated_content
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo descrive alcuni modi in cui utilizzare CSS per aggiungere contenuto quando un documento viene visualizzato. È possibile modificare il foglio di stile per aggiungere contenuti di testo o immagini.

Uno dei vantaggi importanti di CSS è che aiuta a separare lo stile di un documento dal suo contenuto. Tuttavia, ci sono situazioni in cui ha senso specificare determinati contenuti come parte del foglio di stile, e non come parte del documento. Si può specificare contenuto di testo o immagine all'interno di un foglio di stile quando quel contenuto è strettamente legato alla struttura del documento.

> [!NOTE]
> Il contenuto specificato in un foglio di stile non diventa parte del DOM.

Specificare contenuti in un foglio di stile può causare complicazioni. Ad esempio, si potrebbero avere diverse versioni linguistiche del documento che condividono un foglio di stile. Se si specifica un contenuto nel foglio di stile che richiede traduzione, è necessario mettere quelle parti del foglio di stile in file diversi e disporre che siano collegati con le versioni linguistiche appropriate del documento.

Questo problema non si presenta se il contenuto specificato è costituito da simboli o immagini applicabili in tutte le lingue e culture.

## Esempi

### Contenuto testuale

CSS può inserire contenuti testuali prima o dopo un elemento, o modificare il contenuto del marcatore di un elenco (come un simbolo di pallino o un numero) prima di un {{HTMLElement('li')}} o di un altro elemento con {{ cssxref("display", "display: list-item;") }}. Per specificare ciò, creare una regola e aggiungere {{ cssxref("::before") }}, {{ cssxref("::after") }}, o {{cssxref("::marker")}} al selettore. Nella dichiarazione, specificare la proprietà {{ cssxref("content") }} con il contenuto testuale come suo valore.

#### HTML

```html
A text where I need to <span class="ref">something</span>
```

#### CSS

```css
.ref::before {
  font-weight: bold;
  color: navy;
  content: "Reference ";
}
```

#### Output

{{ EmbedLiveSample('Text_content', 600, 30) }}

Il set di caratteri di un foglio di stile è di default UTF-8, ma può anche essere specificato nel link, nel foglio di stile stesso o in altri modi. Per i dettagli, vedere [4.4 Rappresentazione del foglio di stile CSS](https://www.w3.org/TR/CSS21/syndata.html#q23) nella Specifica CSS.

I singoli caratteri possono anche essere specificati tramite un meccanismo di escape che utilizza la barra obliqua inversa come carattere di escape. Ad esempio, "\265B" è il simbolo degli scacchi per una regina nera ♛. Per i dettagli, vedere [Riferimento a caratteri non rappresentati in una codifica di caratteri](https://www.w3.org/TR/CSS21/syndata.html#q24) e [Caratteri e case](https://www.w3.org/TR/CSS21/syndata.html#q6) nella Specifica CSS.

### Contenuto di immagine

Per aggiungere un'immagine prima o dopo un elemento, si può specificare l'URL di un file immagine nel valore della proprietà {{ cssxref("content") }}.

Questa regola aggiunge uno spazio e un'icona dopo ogni link che ha la classe `glossary`:

#### HTML

```html
<a href="developer.mozilla.org" class="glossary">developer.mozilla.org</a>
```

#### CSS

```css
a.glossary::after {
  content: " " url("glossary-icon.gif");
}
```

{{ EmbedLiveSample('Image_content', 600, 40) }}
