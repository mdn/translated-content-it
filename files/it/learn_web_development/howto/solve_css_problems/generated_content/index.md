---
title: Usare contenuto generato con CSS
short-title: Usare contenuto generato
slug: Learn_web_development/Howto/Solve_CSS_problems/Generated_content
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

Questo articolo descrive alcuni modi in cui è possibile usare CSS per aggiungere contenuto quando viene visualizzato un documento. Si modifica il foglio di stile per aggiungere contenuto testuale o immagini.

Uno degli importanti vantaggi di CSS è che aiuta a separare lo stile di un documento dal suo contenuto. Tuttavia, esistono situazioni in cui è sensato specificare determinati contenuti come parte del foglio di stile, anziché come parte del documento. È possibile specificare contenuto testuale o immagini all'interno di un foglio di stile quando tale contenuto è strettamente collegato alla struttura del documento.

> [!NOTE]
> Il contenuto specificato in un foglio di stile non entra a far parte del DOM.

Specificare contenuto in un foglio di stile può causare complicazioni. Ad esempio, un documento potrebbe avere versioni in lingue diverse che condividono un foglio di stile. Se viene specificato nel foglio di stile un contenuto che richiede traduzione, occorre inserire quelle parti del foglio di stile in file diversi e fare in modo che siano collegati alle versioni del documento nella lingua appropriata.

Questo problema non si verifica se il contenuto specificato è costituito da simboli o immagini applicabili in tutte le lingue e culture.

## Esempi

### Contenuto testuale

CSS può inserire contenuto testuale prima o dopo un elemento, oppure modificare il contenuto di un marcatore di elemento di elenco, come un simbolo di punto elenco o un numero, prima di un {{HTMLElement('li')}} o di un altro elemento con {{ cssxref("display", "display: list-item;") }}. Per specificarlo, creare una regola e aggiungere {{ cssxref("::before") }}, {{ cssxref("::after") }} o {{cssxref("::marker")}} al selettore. Nella dichiarazione, specificare la proprietà {{ cssxref("content") }} con il contenuto testuale come valore.

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

Il set di caratteri di un foglio di stile è UTF-8 per impostazione predefinita, ma può anche essere specificato nel collegamento, nel foglio di stile stesso o in altri modi. Per i dettagli, vedere il riferimento a {{cssxref("@charset")}}.

I singoli caratteri possono anche essere specificati tramite un meccanismo di escape che usa la barra rovesciata come carattere di escape. Ad esempio, "\265B" è il simbolo degli scacchi per una regina nera ♛.

### Contenuto immagine

Per aggiungere un'immagine prima o dopo un elemento, è possibile specificare l'URL di un file immagine nel valore della proprietà {{ cssxref("content") }}.

Questa regola aggiunge uno spazio e un'icona dopo ogni collegamento che ha la classe `glossary`:

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
