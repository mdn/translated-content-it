---
title: Invio di moduli tramite JavaScript
slug: Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Quando un utente invia un modulo HTML, ad esempio cliccando sul {{Glossary("Submit_button", "pulsante di invio")}}, il browser effettua una richiesta [HTTP](/it/docs/Web/HTTP) per inviare i dati del modulo. Tuttavia, al posto di questo approccio dichiarativo, le app web a volte utilizzano API JavaScript come [`fetch()`](/it/docs/Web/API/Window/fetch) per inviare dati in modo programmatico a un endpoint che si aspetta una sottomissione del modulo. Questo articolo spiega perché questo è un caso d'uso importante e come farlo.

## Perché usare JavaScript per inviare dati del modulo?

L'invio standard del modulo HTML, come descritto nel nostro articolo su [invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data), carica l'URL dove i dati sono stati inviati, il che significa che la finestra del browser naviga caricando l'intera pagina.

Tuttavia, molte app web, specialmente le {{Glossary("progressive_web_apps", "progressive web apps")}} e le {{Glossary("SPA", "single-page apps")}}, utilizzano API JavaScript per richiedere dati dal server e aggiornare le parti rilevanti della pagina, evitando il sovraccarico di un caricamento completo della pagina.

Per questo motivo, quando queste app web vogliono inviare dati del modulo, utilizzano i moduli HTML solo per raccogliere input dall'utente, ma non per l'invio dei dati. Quando l'utente prova a inviare i dati, l'applicazione prende il controllo e invia i dati usando un'API JavaScript come [`fetch()`](/it/docs/Web/API/Window/fetch).

## Il problema con l'invio del modulo tramite JavaScript

Se l'endpoint del server a cui l'app web invia i dati del modulo è sotto il controllo dello sviluppatore dell'app web, allora possono inviare i dati del modulo in qualsiasi modo scelgano: ad esempio, come un oggetto JSON.

Tuttavia, se l'endpoint del server si aspetta un'invio del modulo, l'app web deve codificare i dati in un modo particolare. Ad esempio, se i dati sono solo testuali, sono composti da elenchi di coppie chiave/valore codificati URL e inviati con un {{httpheader("Content-Type")}} di `application/x-www-form-urlencoded`. Se il modulo include dati binari, deve essere inviato utilizzando il tipo di contenuto `multipart/form-data`.

L'interfaccia [`FormData`](/it/docs/Web/API/FormData) si occupa del processo di codifica dei dati in questo modo, e nel resto di questo articolo forniremo una rapida introduzione a `FormData`. Per ulteriori dettagli, vedere la nostra guida su [Utilizzo degli oggetti FormData](/it/docs/Web/API/XMLHttpRequest_API/Using_FormData_Objects).

## Costruire un oggetto `FormData` manualmente

Lei può popolare un oggetto `FormData` chiamando il metodo [`append()`](/it/docs/Web/API/FormData/append) dell'oggetto per ogni campo che desidera aggiungere, passando il nome e il valore del campo. Il valore può essere una stringa, per i campi di testo, o un [`Blob`](/it/docs/Web/API/Blob), per i campi binari, inclusi gli oggetti [`File`](/it/docs/Web/API/File).

Nel seguente esempio inviamo dati come una sottomissione del modulo quando l'utente clicca un pulsante:

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

1. Costruiamo prima un nuovo oggetto `FormData` vuoto.

2. Poi chiamiamo `append()` due volte, per aggiungere due elementi all'oggetto `FormData`: un campo di testo e un file.

3. Infine, facciamo una richiesta {{httpmethod("POST")}} usando l'API `fetch()`, impostando l'oggetto `FormData` come corpo della richiesta.

Noti che non è necessario impostare l'intestazione {{httpheader("Content-Type")}}: l'intestazione corretta viene impostata automaticamente quando viene passato un oggetto `FormData` in `fetch()`.

## Associare un oggetto `FormData` e un `<form>`

Se i dati che sta inviando provengono veramente da un {{htmlelement("form")}}, può popolare l'istanza `FormData` passando il modulo nel costruttore `FormData`.

Supponga che il nostro HTML dichiari un elemento `<form>`:

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

Aggiungiamo un gestore eventi di invio per l'elemento del modulo. Questo chiama prima [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) per prevenire l'invio incorporato del modulo del browser, in modo che possiamo prendere il controllo. Poi chiamiamo `sendData()`, che recupera l'elemento del modulo e lo passa nel costruttore `FormData`.

Dopo, inviamo l'istanza `FormData` come una richiesta `POST` HTTP, usando `fetch()`.
