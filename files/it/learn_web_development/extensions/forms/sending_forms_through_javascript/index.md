---
title: Invio di moduli tramite JavaScript
slug: Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Quando un utente invia un modulo HTML, ad esempio facendo clic sul {{Glossary("Submit_button", "pulsante di invio")}}, il browser effettua una richiesta [HTTP](/it/docs/Web/HTTP) per inviare i dati contenuti nel modulo. Tuttavia, invece di questo approccio dichiarativo, le app web a volte utilizzano API JavaScript come [`fetch()`](/it/docs/Web/API/Window/fetch) per inviare dati in modo programmato a un endpoint che si aspetta l'invio di un modulo. Questo articolo spiega perché questo è un caso d'uso importante e come farlo.

## Perché usare JavaScript per inviare dati di un modulo?

L'invio standard di moduli HTML, come descritto nel nostro articolo su [invio di dati dai moduli](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data), carica l'URL a cui sono stati inviati i dati, il che significa che la finestra del browser si naviga con un caricamento completo della pagina.

Tuttavia, molte app web, specialmente le {{Glossary("progressive_web_apps", "progressive web apps")}} e le {{Glossary("SPA", "single-page apps")}}, utilizzano API JavaScript per richiedere dati dal server e aggiornare le parti rilevanti della pagina, evitando il sovraccarico di un caricamento completo della pagina.

Per questo motivo, quando queste app vogliono inviare dati di un modulo, usano i moduli HTML solo per raccogliere input dall'utente, ma non per l'invio di dati. Quando l'utente prova a inviare i dati, l'applicazione prende il controllo e invia i dati utilizzando un'API JavaScript come [`fetch()`](/it/docs/Web/API/Window/fetch).

## Il problema con l'invio dei moduli tramite JavaScript

Se l'endpoint del server a cui l'app web invia i dati del modulo è sotto il controllo dello sviluppatore dell'app, allora può inviare i dati in qualsiasi modo scelga: ad esempio, come oggetto JSON.

Tuttavia, se l'endpoint del server si aspetta un invio di modulo, l'app web deve codificare i dati in un modo particolare. Ad esempio, se i dati sono solo testuali, sono costituiti da elenchi di coppie chiave/valore codificate in URL e inviate con un {{httpheader("Content-Type")}} di `application/x-www-form-urlencoded`. Se il modulo include dati binari, devono essere inviati utilizzando il tipo di contenuto `multipart/form-data`.

L'interfaccia [`FormData`](/it/docs/Web/API/FormData) si occupa del processo di codifica dei dati in questo modo, e nel resto di questo articolo forniremo una rapida introduzione a `FormData`. Per ulteriori dettagli, vedi la nostra guida su [Utilizzo degli oggetti FormData](/it/docs/Web/API/XMLHttpRequest_API/Using_FormData_Objects).

## Creazione manuale di un oggetto `FormData`

Si può popolare un oggetto `FormData` chiamando il metodo [`append()`](/it/docs/Web/API/FormData/append) dell'oggetto per ciascun campo che si desidera aggiungere, passando il nome e il valore del campo. Il valore può essere una stringa, per i campi di testo, o un [`Blob`](/it/docs/Web/API/Blob), per i campi binari, inclusi gli oggetti [`File`](/it/docs/Web/API/File).

Nel seguente esempio inviamo dati come se fosse un invio di modulo quando l'utente clicca su un pulsante:

```js
async function sendData(data) {
  // Construct a FormData instance
  const formData = new FormData();

  // Add a text field
  formData.append("name", "Pomegranate");

  // Add a file
  const selection = await window.showOpenFilePicker();
  if (selection.length > 0) {
    const file = await selection[0].getFile();
    formData.append("file", file);
  }

  try {
    const response = await fetch("https://example.org/post", {
      method: "POST",
      // Set the FormData instance as the request body
      body: formData,
    });
    console.log(await response.json());
  } catch (e) {
    console.error(e);
  }
}

const send = document.querySelector("#send");
send.addEventListener("click", sendData);
```

1. Costruiamo dapprima un nuovo oggetto `FormData` vuoto.

2. Successivamente, chiamiamo `append()` due volte, per aggiungere due elementi all'oggetto `FormData`: un campo di testo e un file.

3. Infine, effettuiamo una richiesta {{httpmethod("POST")}} utilizzando l'API `fetch()`, impostando l'oggetto `FormData` come corpo della richiesta.

Si noti che non dobbiamo impostare l'intestazione {{httpheader("Content-Type")}}: l'intestazione corretta viene impostata automaticamente quando passiamo un oggetto `FormData` a `fetch()`.

## Associare un oggetto `FormData` e un `<form>`

Se i dati che si stanno inviando provengono effettivamente da un {{htmlelement("form")}}, è possibile popolare l'istanza `FormData` passando il modulo al costruttore `FormData`.

Supponiamo che il nostro HTML dichiari un elemento `<form>`:

```html
<form id="userinfo">
  <p>
    <label for="username">Enter your name:</label>
    <input type="text" id="username" name="username" value="Dominic" />
  </p>
  <p>
    <label for="avatar">Select an avatar</label>
    <input type="file" id="avatar" name="avatar" required />
  </p>
  <input type="submit" value="Submit" />
</form>
```

Il modulo include un input di testo, un input di file e un pulsante di invio.

Il JavaScript è il seguente:

```js
const form = document.querySelector("#userinfo");

async function sendData() {
  // Associate the FormData object with the form element
  const formData = new FormData(form);

  try {
    const response = await fetch("https://example.org/post", {
      method: "POST",
      // Set the FormData instance as the request body
      body: formData,
    });
    console.log(await response.json());
  } catch (e) {
    console.error(e);
  }
}

// Take over form submission
form.addEventListener("submit", (event) => {
  event.preventDefault();
  sendData();
});
```

Aggiungiamo un gestore dell'evento di invio per l'elemento del modulo. Questo chiama prima [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) per prevenire l'invio di moduli integrato nel browser, in modo che possiamo prendere il controllo. Successivamente, chiamiamo `sendData()`, che recupera l'elemento del modulo e lo passa al costruttore `FormData`.

Dopo di ciò, inviamo l'istanza `FormData` come richiesta HTTP `POST`, utilizzando `fetch()`.
