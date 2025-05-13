---
title: Utilizzare il contenuto generato con CSS
short-title: Uso del contenuto generato
slug: Learn_web_development/Howto/Solve_CSS_problems/Generated_content
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo descrive alcuni modi in cui può utilizzare CSS per aggiungere contenuto quando un documento viene visualizzato. Modifichi il suo foglio di stile per aggiungere contenuto testuale o immagini.

Uno dei vantaggi importanti del CSS è che aiuta a separare lo stile di un documento dal suo contenuto. Tuttavia, ci sono situazioni in cui ha senso specificare un certo contenuto come parte del foglio di stile, non come parte del documento. È possibile specificare contenuto testuale o immagini all'interno di un foglio di stile quando quel contenuto è strettamente collegato alla struttura del documento.

> [!NOTE]
> Il contenuto specificato in un foglio di stile non diventa parte del DOM.

Specificare contenuti in un foglio di stile può causare complicazioni. Ad esempio, potrebbe avere diverse versioni linguistiche del suo documento che condividono un foglio di stile. Se specifica contenuto nel suo foglio di stile che richiede traduzione, deve mettere quelle parti del suo foglio di stile in file diversi e fare in modo che siano collegati con le versioni linguistiche appropriate del suo documento.

Questo problema non si presenta se il contenuto specificato consiste in simboli o immagini che si applicano a tutte le lingue e culture.

## Esempi

### Contenuto testuale

Il CSS può inserire contenuto testuale prima o dopo un elemento, o modificare il contenuto di un indicatore di elemento della lista (come un simbolo a pallino o un numero) prima di un {{HTMLElement('li')}} o un altro elemento con {{ cssxref("display", "display: list-item;") }}. Per specificare questo, crei una regola e aggiunga {{ cssxref("::before") }}, {{ cssxref("::after") }}, o {{cssxref("::marker")}} al selettore. Nella dichiarazione, specifichi la proprietà {{ cssxref("content") }} con il contenuto testuale come valore.

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

Il set di caratteri di un foglio di stile è UTF-8 di default, ma può anche essere specificato nel link, nel foglio di stile stesso, o in altri modi. Per i dettagli, vedere [4.4 Rappresentazione del foglio di stile CSS](https://www.w3.org/TR/CSS21/syndata.html#q23) nella Specifica CSS.

Anche i singoli caratteri possono essere specificati tramite un meccanismo di escape che utilizza il backslash come carattere di escape. Ad esempio, "\265B" è il simbolo degli scacchi per una regina nera ♛. Per i dettagli, vedere [Fare riferimento ai caratteri non rappresentati in una codifica di caratteri](https://www.w3.org/TR/CSS21/syndata.html#q24) e [Caratteri e maiuscole/minuscole](https://www.w3.org/TR/CSS21/syndata.html#q6) nella Specifica CSS.

### Contenuto delle immagini

Per aggiungere un'immagine prima o dopo un elemento, può specificare l'URL di un file immagine nel valore della proprietà {{ cssxref("content") }}.

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
