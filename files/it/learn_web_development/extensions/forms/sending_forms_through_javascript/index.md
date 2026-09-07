---
title: Invio di moduli tramite JavaScript
short-title: Invio di moduli con JS
slug: Learn_web_development/Extensions/Forms/Sending_forms_through_JavaScript
l10n:
  sourceCommit: f85d2e26b062decf7a2bb9179c3a93003f4067a9
---

Quando un utente invia un modulo HTML, ad esempio facendo clic sul {{Glossary("Submit_button", "pulsante di invio")}}, il browser effettua una richiesta [HTTP](/it/docs/Web/HTTP) per inviare i dati nel modulo. Tuttavia, invece di questo approccio dichiarativo, le web app talvolta usano API JavaScript come [`fetch()`](/it/docs/Web/API/Window/fetch) per inviare programmaticamente i dati a un endpoint che prevede l'invio di un modulo. Questo articolo spiega perché si tratta di un importante caso d'uso e come farlo.

## Perché usare JavaScript per inviare dati di moduli?

L'invio standard di moduli HTML, come descritto nel nostro articolo sull'[invio di dati dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data), carica l'URL a cui sono stati inviati i dati, il che significa che la finestra del browser esegue una navigazione con un caricamento completo della pagina.

Tuttavia, molte web app, in particolare le {{Glossary("progressive_web_apps", "progressive web app")}} e le {{Glossary("SPA", "single-page app")}}, usano API JavaScript per richiedere dati al server e aggiornare le parti pertinenti della pagina, evitando il sovraccarico di un caricamento completo della pagina.

Per questo motivo, quando queste web app vogliono inviare dati di moduli, usano i moduli HTML solo per raccogliere l'input dell'utente, ma non per l'invio dei dati. Quando l'utente tenta di inviare i dati, l'applicazione assume il controllo e invia i dati usando un'API JavaScript come [`fetch()`](/it/docs/Web/API/Window/fetch).

## Il problema dell'invio di moduli tramite JavaScript

Se l'endpoint del server a cui la web app invia i dati del modulo è sotto il controllo dello sviluppatore della web app, questi può inviare i dati del modulo in qualsiasi modo scelga: ad esempio, come oggetto JSON.

Tuttavia, se l'endpoint del server prevede l'invio di un modulo, la web app deve codificare i dati in un modo particolare. Ad esempio, se i dati sono solo testuali, sono costituiti da elenchi di coppie chiave/valore codificati in URL e vengono inviati con un {{httpheader("Content-Type")}} di `application/x-www-form-urlencoded`. Se il modulo include dati binari, questi devono essere inviati usando il tipo di contenuto `multipart/form-data`.

L'interfaccia [`FormData`](/it/docs/Web/API/FormData) si occupa del processo di codifica dei dati in questo modo e nel resto di questo articolo verrà fornita una breve introduzione a `FormData`. Per maggiori dettagli, consultare la nostra guida su [Uso di oggetti FormData](/it/docs/Web/API/XMLHttpRequest_API/Using_FormData_Objects).

## Creazione manuale di un oggetto `FormData`

È possibile popolare un oggetto `FormData` chiamando il metodo [`append()`](/it/docs/Web/API/FormData/append) dell'oggetto per ogni campo che si desidera aggiungere, passando il nome e il valore del campo. Il valore può essere una stringa, per i campi di testo, oppure un [`Blob`](/it/docs/Web/API/Blob), per i campi binari, inclusi gli oggetti [`File`](/it/docs/Web/API/File).

Nell'esempio seguente inviamo i dati come invio di un modulo quando l'utente fa clic su un pulsante:

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

1. Per prima cosa viene creato un nuovo oggetto `FormData` vuoto.

2. Successivamente, `append()` viene chiamato due volte per aggiungere due elementi all'oggetto `FormData`: un campo di testo e un file.

3. Infine, viene effettuata una richiesta {{httpmethod("POST")}} usando l'API `fetch()`, impostando l'oggetto `FormData` come corpo della richiesta.

Non è necessario impostare l'header {{httpheader("Content-Type")}}: l'header corretto viene impostato automaticamente quando un oggetto `FormData` viene passato a `fetch()`.

## Associazione di un oggetto `FormData` e di un `<form>`

Se i dati inviati provengono effettivamente da un {{htmlelement("form")}}, è possibile popolare l'istanza `FormData` passando il modulo al costruttore `FormData`.

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

Il modulo include un input di testo, un input per file e un pulsante di invio.

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

Viene aggiunto un gestore dell'evento di invio per l'elemento del modulo. Questo chiama innanzitutto [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) per impedire l'invio del modulo integrato nel browser, così da poter assumere il controllo. Quindi viene chiamato `sendData()`, che recupera l'elemento del modulo e lo passa al costruttore `FormData`.

Dopodiché, l'istanza `FormData` viene inviata come richiesta HTTP `POST`, usando `fetch()`.
