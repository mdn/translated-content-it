---
title: "Metti alla prova le tue competenze: accessibilità HTML"
short-title: "Test: a11y HTML"
slug: Learn_web_development/Core/Accessibility/Test_your_skills/HTML
l10n:
  sourceCommit: 2bda943b59604eb44f5d759708845c5f56970635
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML","Learn_web_development/Core/Accessibility/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}

Lo scopo di questo test di competenze è aiutare a valutare se è stato compreso l'articolo [HTML: una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML).

> [!NOTE]
> Per ottenere aiuto, leggere la guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Accessibilità HTML 1

In questa attività verrà testata la comprensione dell'HTML semantico e del motivo per cui è utile per l'accessibilità. Il testo fornito è un pannello informativo con pulsanti di azione, ma l'HTML è davvero scadente.

Per completare l'attività, aggiornare il markup affinché utilizzi HTML semantico appropriato. Non è necessario preoccuparsi troppo di ricreare l'aspetto e le dimensioni del testo _esattamente_ uguali, purché la semantica sia corretta.

<!-- Codice condiviso tra gli esempi -->

```css hidden live-sample___html-ally-1 live-sample___html-ally-2 live-sample___html-ally-3 live-sample___html-ally-4 live-sample___html-ally-2-finish
body {
  background-color: white;
  color: #333333;
  font:
    1em / 1.4 "Helvetica Neue",
    "Helvetica",
    "Arial",
    sans-serif;
  padding: 1em;
  margin: 0;
}

* {
  box-sizing: border-box;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'attività ha questo aspetto:

{{ EmbedLiveSample("html-ally-1", "100%", 630) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___html-ally-1
<font size="7">Need help?</font> <br /><br />
If you have any problems with our products, our support center can offer you all
the help you need, whether you want:
<br /><br />
1. Advice choosing a new product
<br />
2. Tech support on an existing product
<br />
3. Refund and cancellation assistance
<br /><br />
<font size="5">Contact us now</font>
<br /><br />
Our help center contains live chat, email addresses, and phone numbers.
<br /><br />
<div class="button">Find Contact Details</div>
<br />
<font size="5">Find out answers</font>
<br /><br />
Our Forums section contains a large knowledge base of searchable previously
asked questions, and you can always ask a new question if you can't find the
answer you're looking for.
<br /><br />
<div class="button">Access Forums</div>
```

```css live-sample___html-ally-1
.button {
  color: white;
  background-color: blue;
  border-radius: 10px;
  width: 170px;
  padding: 10px;
  text-align: center;
}
```

Non abbiamo fornito il contenuto finale per questa attività, poiché non appare significativamente diverso dallo stato iniziale.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile al seguente:

```html
<h2>Need help?</h2>

<p>
  If you have any problems with our products, our support center can offer you
  all the help you need, whether you want:
</p>

<ul>
  <li>Advice choosing a new product</li>
  <li>Tech support on an existing product</li>
  <li>Refund and cancellation assistance</li>
</ul>

<h3>Contact us now</h3>

<p>Our help center contains live chat, email addresses, and phone numbers.</p>

<button>Find Contact Details</button>

<h3>Find out answers</h3>

<p>
  Our Forums section contains a large knowledge base of searchable previously
  asked questions, and you can always ask a new question if you can't find the
  answer you're looking for.
</p>

<button>Access forums</button>
```

Punti bonus per:

- Usare semplicemente `<button>`, non `<button class="button">` (ripetere la semantica non è necessario), e aggiornare il selettore CSS per assicurarsi che il pulsante riceva ancora gli stili.
- Usare un elenco non ordinato, non un elenco ordinato: l'elenco degli elementi non deve realmente seguire un ordine.

</details>

## Accessibilità HTML 2

Nella seconda attività è presente un modulo contenente tre campi di input.

Per completare l'attività:

1. Associare semanticamente gli input alle rispettive label.
2. Presumere che questi input facciano parte di un modulo più ampio e racchiuderli in un elemento che li associ tutti insieme come un singolo gruppo correlato.
3. Assegnare al gruppo una descrizione/titolo che riassuma tutte le informazioni come dati personali.

Il punto di partenza dell'attività ha questo aspetto:

{{ EmbedLiveSample("html-ally-2", "100%", 200) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___html-ally-2
<form>
  <ul>
    <li>
      Name
      <input type="text" name="name" />
    </li>
    <li>
      Age
      <input type="number" name="age" />
    </li>
    <li>
      Email address
      <input type="email" name="email" />
    </li>
  </ul>
</form>
```

```css live-sample___html-ally-2 live-sample___html-ally-2-finish
form {
  width: 400px;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
```

Il modulo aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample("html-ally-2-finish", "100%", 220) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile al seguente:

```html live-sample___html-ally-2-finish
<form>
  <fieldset>
    <legend>Personal data</legend>
    <ul>
      <li>
        <label for="name">Name</label>
        <input type="text" name="name" id="name" />
      </li>
      <li>
        <label for="age">Age</label>
        <input type="number" name="age" id="age" />
      </li>
      <li>
        <label for="email">Email address</label>
        <input type="email" name="email" id="email" />
      </li>
    </ul>
  </fieldset>
</form>
```

</details>

## Accessibilità HTML 3

In questa attività è necessario trasformare tutti i link informativi nel paragrafo in link validi e accessibili.

- I primi due link rimandano a normali pagine web.
- Il terzo link rimanda a un PDF di grandi dimensioni, 8 MB.
- Il quarto link rimanda a un documento Word, quindi l'utente dovrà avere installata qualche applicazione in grado di gestirlo.

Per completare l'attività, aggiornare opportunamente i link in base alle descrizioni precedenti.

Il punto di partenza dell'attività ha questo aspetto:

{{ EmbedLiveSample("html-ally-3", "100%", 140) }}

Ecco il codice sottostante per questo punto di partenza:

```html-nolint live-sample___html-ally-3
<p>
  For more information about our activities, check out our fundraising page
  (<a href="/fundraising" target="_blank">click here</a>), education page
  (<a href="/education" target="_blank">click here</a>), sponsorship pack
  (<a href="/resources/sponsorship.pdf" target="_blank">click here</a>),
   and assessment sheets
  (<a href="/resources/assessments.docx" target="_blank">click here</a>).
</p>
```

> [!NOTE]
> I link nel codice iniziale hanno impostato l'attributo `target="_blank"`, in modo che, quando vengono selezionati, tentino di aprire le pagine collegate in una nuova scheda anziché nella stessa scheda. Questa non è strettamente una buona pratica, ma è stata adottata qui per evitare che le pagine si aprano nell'`<iframe>` di output del Playground MDN, eliminando di conseguenza il codice di esempio.

Non abbiamo fornito il contenuto finale per questa attività, poiché rivelerebbe la soluzione.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile al seguente:

```html
<p>
  For more information about our activities, check out our
  <a href="/fundraising" target="_blank">fundraising page</a>,
  <a href="/education" target="_blank">education page</a>,
  <a href="/resources/sponsorship.pdf" target="_blank"
    >sponsorship pack (PDF, 8MB)</a
  >, and
  <a href="/resources/assessments.docx" target="_blank"
    >assessment sheets (Word document)</a
  >.
</p>
```

</details>

## Accessibilità HTML 4

Nell'ultima attività sull'accessibilità HTML viene fornita una galleria di immagini che presenta alcuni problemi di accessibilità. È possibile risolverli?

- L'immagine dell'intestazione presenta un problema di accessibilità, così come le immagini della galleria.
- Si potrebbe migliorare ulteriormente l'immagine dell'intestazione implementandola con CSS, per un'accessibilità probabilmente migliore. Come si potrebbe creare una soluzione di questo tipo?

Aggiornare il codice per risolvere i problemi descritti sopra.

Il punto di partenza dell'attività ha questo aspetto:

{{ EmbedLiveSample("html-ally-4", "100%", 400) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___html-ally-4
<header>
  <img
    src="https://mdn.github.io/shared-assets/images/examples/star-pink_32x32.png"
    alt="A star that I use to decorate my page" />
  <h1>Groovy images</h1>
</header>
<main>
  <img
    src="https://mdn.github.io/shared-assets/images/examples/ballon-portrait.jpg" />
  <img
    src="https://mdn.github.io/shared-assets/images/examples/grapefruit-slice.jpg" />
</main>
```

```css live-sample___html-ally-4
body {
  width: 400px;
  margin: 0 auto;
}

main img {
  display: block;
  width: 250px;
  margin: 20px auto;
  box-shadow: 5px 5px 0 black;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}
```

Non abbiamo fornito il contenuto finale per questa attività, poiché appare uguale al punto di partenza.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

I problemi di accessibilità sono:

1. L'immagine dell'intestazione è decorativa, quindi non necessita di testo alternativo. La soluzione migliore, se si intende utilizzare immagini HTML decorative, consiste nell'inserire `alt=""`, in modo che uno screen reader non legga nulla, anziché una descrizione o il nome del file dell'immagine. Non fa parte del contenuto.
2. Le immagini della galleria necessitano di testo alternativo e fanno parte del contenuto.

L'HTML aggiornato potrebbe avere un aspetto simile al seguente:

```html
<header>
  <img
    src="https://mdn.github.io/shared-assets/images/examples/star-pink_32x32.png"
    alt="" />
  <h1>Groovy images</h1>
</header>
<main>
  <img
    src="https://mdn.github.io/shared-assets/images/examples/ballon-portrait.jpg"
    alt="a hot air balloon covered in a blue and while checked pattern" />
  <img
    src="https://mdn.github.io/shared-assets/images/examples/grapefruit-slice.jpg"
    alt="A cross-section of the middle of a pink grapefruit" />
</main>
```

Sarebbe probabilmente meglio implementare l'immagine di sfondo dell'intestazione utilizzando immagini di sfondo CSS. Per farlo, rimuovere il primo elemento `<img>` dal markup e aggiungere una regola al CSS come questa:

```css
h1 {
  background: url("https://mdn.github.io/shared-assets/images/examples/star-pink_32x32.png")
    no-repeat left;
  padding-left: 50px;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML","Learn_web_development/Core/Accessibility/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}
