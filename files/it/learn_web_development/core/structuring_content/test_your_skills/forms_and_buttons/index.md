---
title: "Metti alla prova le tue competenze: moduli e pulsanti"
short-title: "Test: moduli e pulsanti"
slug: Learn_web_development/Core/Structuring_content/Test_your_skills/Forms_and_buttons
l10n:
  sourceCommit: 3cd2de993df83bd5e738abe0a28bd5702af78c11
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content/Forms_challenge", "Learn_web_development/Core/Structuring_content")}}

Lo scopo di questo test di competenze è aiutare a valutare se si comprende il funzionamento di [moduli e pulsanti HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_forms).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida sull'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci usando uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Moduli e pulsanti 1

Questo esercizio inizia in modo semplice, chiedendo di creare due elementi `<input>`, per l'ID e la password di un utente, insieme a un pulsante di invio.

Per completare l'esercizio:

1. Creare input appropriati per l'ID e la password dell'utente.
2. Associarli semanticamente anche alle rispettive etichette di testo.
3. Creare un pulsante di invio nell'elemento di elenco rimanente, con il testo del pulsante "Log in".

<!-- Code shared across examples -->

```css hidden live-sample___forms-buttons-1 live-sample___forms-buttons-2 live-sample___forms-buttons-3 live-sample___forms-buttons-4 live-sample___forms-buttons-5 live-sample___forms-buttons-6 live-sample___forms-buttons-1-finished live-sample___forms-buttons-2-finished live-sample___forms-buttons-3-finished live-sample___forms-buttons-4-finished live-sample___forms-buttons-5-finished live-sample___forms-buttons-6-finished
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

<!-- Example-specific code -->

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("forms-buttons-1", "100%", 150) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___forms-buttons-1
<form>
  <ul>
    <li>User ID</li>
    <li>Password</li>
    <li></li>
  </ul>
</form>
```

Il modulo aggiornato dovrebbe avere un aspetto simile a questo:

{{ EmbedLiveSample("forms-buttons-1-finished", "100%", 150) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html live-sample___forms-buttons-1-finished
<form>
  <ul>
    <li>
      <label for="uid">User ID</label>
      <input type="text" id="uid" name="uid" />
    </li>
    <li>
      <label for="pwd">Password</label>
      <input type="password" id="pwd" name="pwd" />
    </li>
    <li>
      <button>Log in</button>
    </li>
  </ul>
</form>
```

</details>

## Moduli e pulsanti 2

L'esercizio successivo richiede di creare insiemi funzionanti di checkbox e radio button, a partire dalle etichette di testo fornite.

Per completare l'esercizio:

1. Trasformare il contenuto del primo `<fieldset>` in un insieme di radio button: dovrebbe essere possibile selezionare un solo personaggio pony alla volta.
2. Fare in modo che il primo radio button sia selezionato al caricamento della pagina.
3. Trasformare il contenuto del secondo `<fieldset>` in un insieme di checkbox.
4. Aggiungere un paio di ulteriori scelte di hot dog.

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("forms-buttons-2", "100%", 350) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___forms-buttons-2
<form>
  <fieldset>
    <legend>Who is your favorite pony?</legend>
    <ul>
      <li>
        <label for="pinkie">Pinkie Pie</label>
      </li>
      <li>
        <label for="rainbow">Rainbow Dash</label>
      </li>
      <li>
        <label for="twilight">Twilight Sparkle</label>
      </li>
    </ul>
  </fieldset>
  <fieldset>
    <legend>Hotdog preferences</legend>
    <ul>
      <li>
        <label for="vegan">Vegan</label>
      </li>
      <li>
        <label for="onions">Onions</label>
      </li>
    </ul>
  </fieldset>
  <button>Submit</button>
</form>
```

Il modulo aggiornato dovrebbe avere un aspetto simile a questo:

{{ EmbedLiveSample("forms-buttons-2-finished", "100%", 360) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html live-sample___forms-buttons-2-finished
<form>
  <fieldset>
    <legend>Who is your favorite pony?</legend>
    <ul>
      <li>
        <label for="pinkie">Pinkie Pie</label>
        <input type="radio" id="pinkie" name="pony" value="pinkie" checked />
      </li>
      <li>
        <label for="rainbow">Rainbow Dash</label>
        <input type="radio" id="rainbow" name="pony" value="rainbow" />
      </li>
      <li>
        <label for="twilight">Twilight Sparkle</label>
        <input type="radio" id="twilight" name="pony" value="twilight" />
      </li>
    </ul>
  </fieldset>
  <fieldset>
    <legend>Hotdog preferences</legend>
    <ul>
      <li>
        <label for="vegan">Vegan</label>
        <input type="checkbox" id="vegan" name="hotdog_vegan" />
      </li>
      <li>
        <label for="onions">Onions</label>
        <input type="checkbox" id="onions" name="hotdog_onions" />
      </li>
      <li>
        <label for="mustard">Mustard</label>
        <input type="checkbox" id="mustard" name="hotdog_mustard" />
      </li>

      <li>
        <label for="ketchup">Ketchup</label>
        <input type="checkbox" id="ketchup" name="hotdog_ketchup" />
      </li>
    </ul>
  </fieldset>
  <button>Submit</button>
</form>
```

</details>

## Moduli e pulsanti 3

In questo esercizio verranno esplorati alcuni tipi di input più specifici. Occorre creare input appropriati affinché un utente possa aggiornare i propri dati:

1. Email
2. Sito web
3. Numero di telefono
4. Colore preferito

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("forms-buttons-3", "100%", 250) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___forms-buttons-3
<form>
  <h2>Edit your preferences</h2>
  <ul>
    <li>
      <label for="email">Email</label>
    </li>
    <li>
      <label for="website">Website</label>
    </li>
    <li>
      <label for="phone">Phone number</label>
    </li>
    <li>
      <label for="fave-color">Favorite color</label>
    </li>
    <li>
      <button>Update preferences</button>
    </li>
  </ul>
</form>
```

Il modulo aggiornato dovrebbe avere un aspetto simile a questo:

{{ EmbedLiveSample("forms-buttons-3-finished", "100%", 250) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html live-sample___forms-buttons-3-finished
<form>
  <h2>Edit your preferences</h2>
  <ul>
    <li>
      <label for="email">Email</label>
      <input type="email" id="email" name="email" />
    </li>
    <li>
      <label for="website">Website</label>
      <input type="url" id="website" name="website" />
    </li>
    <li>
      <label for="phone">Phone number</label>
      <input type="tel" id="phone" name="phone" />
    </li>
    <li>
      <label for="fave-color">Favorite color</label>
      <input type="color" id="fave-color" name="fave-color" />
    </li>
    <li>
      <button>Update preferences</button>
    </li>
  </ul>
</form>
```

</details>

## Moduli e pulsanti 4

Ora è il momento di provare a implementare un menu a selezione a discesa, per consentire a un utente di scegliere il proprio cibo preferito tra le opzioni fornite.

Per completare l'esercizio:

1. Creare una struttura di base per una casella di selezione.
2. Associarla semanticamente all'etichetta "food" fornita.
3. All'interno dell'elenco, suddividere le scelte in 2 sottogruppi: "mains" e "snacks".

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("forms-buttons-4", "100%", 120) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___forms-buttons-4
<form>
  <ul>
    <li>
      <label for="food">Pick your favorite food:</label>

      Salad Curry Pizza Fajitas Biscuits Crisps Fruit Breadsticks
    </li>
    <li>
      <button>Submit choice</button>
    </li>
  </ul>
</form>
```

Il modulo aggiornato dovrebbe avere un aspetto simile a questo:

{{ EmbedLiveSample("forms-buttons-4-finished", "100%", 120) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html live-sample___forms-buttons-4-finished
<form>
  <ul>
    <li>
      <label for="food">Pick your favorite food:</label>
      <select name="food" id="food">
        <optgroup label="mains">
          <option>Salad</option>
          <option>Curry</option>
          <option>Pizza</option>
          <option>Fajitas</option>
        </optgroup>
        <optgroup label="snacks">
          <option>Biscuits</option>
          <option>Crisps</option>
          <option>Fruit</option>
          <option>Breadsticks</option>
        </optgroup>
      </select>
    </li>
    <li>
      <button>Submit choice</button>
    </li>
  </ul>
</form>
```

</details>

## Moduli e pulsanti 5

In questo esercizio occorre strutturare le funzionalità del modulo fornite.

Per completare l'esercizio:

1. Separare i primi due e i secondi due campi del modulo in due contenitori distinti, ciascuno con una legenda descrittiva (usare "Personal details" per i primi due e "Comment information" per i secondi due).
2. Contrassegnare ogni etichetta di testo con un elemento appropriato affinché sia semanticamente associata al rispettivo campo del modulo.
3. Aggiungere un insieme appropriato di elementi strutturali attorno alle coppie etichetta/campo per separarle.

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("forms-buttons-5", "100%", 120) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___forms-buttons-5
<form>
  Name:
  <input type="text" id="name" name="name" />

  Age:
  <input type="number" id="age" name="age" />

  Comment:
  <input type="text" id="comment" name="comment" />

  Email:
  <input type="email" id="email" name="email" />
</form>
```

Il modulo aggiornato dovrebbe avere un aspetto simile a questo:

{{ EmbedLiveSample("forms-buttons-5-finished", "100%", 300) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html live-sample___forms-buttons-5-finished
<form>
  <fieldset>
    <legend>Personal details</legend>
    <ul>
      <li>
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" />
      </li>
      <li>
        <label for="age">Age:</label>
        <input type="number" id="age" name="age" />
      </li>
    </ul>
  </fieldset>
  <fieldset>
    <legend>Comment information</legend>
    <ul>
      <li>
        <label for="comment">Comment:</label>
        <input type="text" id="comment" name="comment" />
      </li>
      <li>
        <label for="email">Email (include if you want a reply):</label>
        <input type="email" id="email" name="email" />
      </li>
    </ul>
  </fieldset>
</form>
```

</details>

## Moduli e pulsanti 6

In questo esercizio viene fornito un semplice modulo per richieste di assistenza, al quale occorre aggiungere alcune funzionalità di convalida. Questo esercizio richiede alcune conoscenze che non vengono insegnate nell'articolo "Forms and buttons in HTML", quindi potrebbe essere necessario svolgere alcune ricerche altrove.

Per completare l'esercizio:

1. Rendere obbligatori tutti i campi di input prima che il modulo possa essere inviato.
2. Modificare il tipo dei campi "Email address" e "Phone number" affinché il browser applichi una convalida più specifica e adatta ai dati richiesti.
3. Assegnare al campo "User name" una lunghezza obbligatoria compresa tra 5 e 20 caratteri, al campo "Phone number" una lunghezza massima di 15 caratteri e al campo "Comment" una lunghezza massima di 200 caratteri.

Provare a inviare il modulo: dovrebbe rifiutare l'invio finché non vengono rispettati i vincoli sopra indicati e mostrare messaggi di errore appropriati.

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("forms-buttons-6", "100%", 300) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___forms-buttons-6
<form>
  <h2>Enter your support query</h2>
  <ul>
    <li>
      <label for="uname">User name:</label>
      <input type="text" name="uname" id="uname" />
    </li>
    <li>
      <label for="email">Email address:</label>
      <input type="text" name="email" id="email" />
    </li>
    <li>
      <label for="phone">Phone number:</label>
      <input type="text" name="phone" id="phone" />
    </li>
    <li>
      <label for="comment">Comment:</label>
      <textarea name="comment" id="comment"> </textarea>
    </li>
    <li>
      <button>Submit comment</button>
    </li>
  </ul>
</form>
```

Non sono stati forniti contenuti completati per questo esercizio, poiché ha lo stesso aspetto del punto di partenza.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html live-sample___forms-buttons-6-finished
<form>
  <h2>Enter your support query</h2>
  <ul>
    <li>
      <label for="uname">User name:</label>
      <input
        type="text"
        name="uname"
        id="uname"
        required
        minlength="5"
        maxlength="20" />
    </li>
    <li>
      <label for="email">Email address:</label>
      <input type="email" name="email" id="email" required />
    </li>
    <li>
      <label for="phone">Phone number:</label>
      <input type="tel" name="phone" id="phone" required maxlength="15" />
    </li>
    <li>
      <label for="comment">Comment:</label>
      <textarea name="comment" id="comment" required maxlength="200"></textarea>
    </li>
    <li>
      <button>Submit comment</button>
    </li>
  </ul>
</form>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content/Forms_challenge", "Learn_web_development/Core/Structuring_content")}}
