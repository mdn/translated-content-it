---
title: "Metti alla prova le tue competenze: i link"
short-title: "Test: link"
slug: Learn_web_development/Core/Structuring_content/Test_your_skills/Links
l10n:
  sourceCommit: 1cf3cb0fb22bf89c780fefe74c3db7f1b9e8ca09
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content")}}

L'obiettivo di questo test di competenze è aiutare a valutare se si è compreso come [implementare i link in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links).

> [!NOTE]
> Per ricevere aiuto, leggere la nostra guida sull'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

> [!NOTE]
> Alcuni dei link nel codice iniziale di queste attività hanno impostato l'attributo `target="_blank"`, pertanto, quando vengono selezionati, proveranno ad aprire la pagina collegata in una nuova scheda anziché nella stessa scheda. Questa non è strettamente una best practice, ma è stato fatto qui affinché le pagine non si aprano nell'`<iframe>` di output di MDN Playground, eliminando così il codice di esempio!

## Link 1

In questa attività, è necessario completare i link nella pagina informativa sulle balene.

Per completare l'attività, aggiornare i link come segue:

1. Il primo link deve essere collegato a una pagina denominata `whales.html`, che si trova nella stessa directory della pagina corrente.
2. Aggiungere un tooltip che, al passaggio del mouse, informi l'utente che la pagina include informazioni sulle balene azzurre e sui capodogli.
3. Il secondo link deve diventare un link selezionabile per aprire un'email nell'applicazione di posta predefinita dell'utente, con il destinatario impostato su "whales\@example.com".
4. Punti bonus se viene impostato anche il riempimento automatico dell'oggetto dell'email con "Question about Whales".

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample('links-1', "100%", 170) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___links-1
<h1>Information on Whales</h1>

<p>
  For more information on our conservation activities and which Whales we study,
  see our <a target="_blank">Whales page</a>.
</p>

<p>
  If you want to ask our team more questions, feel free to
  <a target="_blank">email us</a>.
</p>
```

```css hidden live-sample___links-1 live-sample___links-1-finished
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

h1 {
  font-size: 2rem;
  margin: 0;
  color: purple;
}

p {
  color: gray;
  margin: 0.5em 0;
}

* {
  box-sizing: border-box;
}
```

Il contenuto aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample('links-1-finished', "100%", 170) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html-nolint live-sample___links-1-finished
<h1>Information on Whales</h1>

<p>
  For more information on our conservation activities and which Whales we study,
  see our <a target="_blank" href="whales.html" title="Includes information on Blue Whales and Sperm Whales">
  Whales page</a>.
</p>

<p>
  If you want to ask our team more questions, feel free to
  <a target="_blank" href="mailto:whales@example.com?subject=Question%20about%20Whales">
  email us</a>.
</p>
```

</details>

## Link 2

In questa attività, è necessario completare i quattro link affinché puntino alle destinazioni appropriate.

Per completare l'attività, aggiornare i link come segue:

1. Il primo link deve puntare a un'immagine denominata `blue-whale.jpg`, che si trova in una directory denominata `blue` all'interno della directory corrente.
2. Il secondo link deve puntare a un'immagine denominata `narwhal.jpg`, che si trova nella directory `narwhal`, situata un livello di directory sopra quella corrente.
3. Il terzo link deve puntare alla ricerca immagini di Google nel Regno Unito. L'URL di base è `https://www.google.co.uk` e la ricerca immagini si trova in una sottodirectory denominata `imghp`.
4. Il quarto link deve puntare al paragrafo in fondo alla pagina corrente. Ha un ID `bottom`.

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample('links-2', "100%", 200) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___links-2
<h1>List path tests</h1>

<ul>
  <li><a target="_blank">Link me to the blue whale image</a></li>
  <li><a target="_blank">Link me to the narwhal image</a></li>
  <li><a target="_blank">Link me to Google image search</a></li>
  <li><a>Link me to the paragraph at the bottom of the page</a></li>
</ul>

<div></div>

<p id="bottom">The bottom of the page!</p>
```

```css hidden live-sample___links-2 live-sample___links-2-finished
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

h1 {
  font-size: 2rem;
  margin: 0;
  color: purple;
}

li {
  color: gray;
  margin: 0.5em 0;
}

div {
  height: 600px;
}
```

Il contenuto aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample('links-2-finished', "100%", 200) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html-nolint live-sample___links-2-finished
<h1>List path tests</h1>

<ul>
  <li><a target="_blank" href="blue/blue-whale.jpg">
    Link me to the blue whale image
  </a></li>
  <li><a target="_blank" href="../narwhal/narwhal.jpg">
    Link me to the narwhal image
  </a></li>
  <li><a target="_blank" href="https://www.google.co.uk/imghp">
    Link me to Google image search
  </a></li>
  <li><a href="#bottom">
    Link me to the paragraph at the bottom of the page
  </a></li>
</ul>

<div></div>

<p id="bottom">The bottom of the page!</p>
```

</details>

## Link 3

I seguenti link rimandano a una pagina informativa sui narvali, a un indirizzo email di supporto e a una scheda informativa in PDF di 4 MB.

Per completare l'attività:

1. Prendere i paragrafi esistenti con testo del link scritto in modo inadeguato e riscriverli in modo che abbiano un buon testo del link.
2. Aggiungere un avviso a tutti i link che ne richiedono uno.

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample('links-3', "100%", 200) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___links-3
<p>
  We do lots of work with Narwhals. To find out more about this work,
  <a href="narwhals.html" target="_blank">click here</a>.
</p>

<p>
  You can email our support team if you have any more questions —
  <a href="mailto:whales@example.com">click here</a> to do so.
</p>

<p>
  You can also <a href="factfile.pdf" target="_blank">click here</a> to download
  our factfile, which contains lots more information, including an FAQ.
</p>
```

```css hidden live-sample___links-3
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

p {
  color: gray;
  margin: 0.5em 0;
}

* {
  box-sizing: border-box;
}
```

Non è stato fornito il contenuto completato per questa attività, poiché rivelerebbe la soluzione.

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html-nolint
<p>
  We do lots of work with Narwhals. <a href="narwhals.html" target="_blank">Find out more about this work</a>.
</p>

<p>
  You can <a href="mailto:whales@example.com">email our support team</a> if you have any more questions.
</p>

<p>
  You can also <a href="factfile.pdf" target="_blank">download
  our factfile</a> (PDF, 4MB), which contains lots more information, including an FAQ.
</p>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content")}}
