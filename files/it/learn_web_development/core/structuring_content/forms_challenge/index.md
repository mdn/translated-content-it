---
title: "Sfida: strutturare un modulo di feedback"
short-title: "Sfida: modulo di feedback"
slug: Learn_web_development/Core/Structuring_content/Forms_challenge
l10n:
  sourceCommit: 8126a04c73f0c6821b2ac4e5571fa83320d3a65a
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Forms_and_buttons", "Learn_web_development/Core/Structuring_content/Debugging_HTML", "Learn_web_development/Core/Structuring_content")}}

In questa sfida verrà testata la capacità di creare e strutturare un modulo, oltre ad aggiungervi alcune altre funzionalità HTML.

## Punto di partenza

Per risolvere questa sfida, è necessario creare un progetto di sito web di base, all'interno di una cartella sul disco rigido del computer oppure usando un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/). Gran parte del codice necessario è già fornita in questa pagina.

1. Creare una nuova cartella in una posizione appropriata sul computer, denominata `forms-challenge` (oppure aprire un editor online ed eseguire i passaggi necessari per creare un nuovo progetto).
2. Salvare il seguente listato HTML in un file all'interno della cartella chiamato `index.html` (oppure incollarlo nel pannello HTML dell'editor online).

   ```html-nolint
   <!doctype html>
   <html lang="en">
     <head>
       <meta charset="utf-8" />
       <title>Forms challenge</title>
       <link href="style.css" rel="stylesheet" />
       <script defer src="index.js"></script>
     </head>
     <body>
       We want your feedback!

       We're very excited that you visited the little house in the woods,
       and we want to hear what you thought of it! Please fill in the below
       sections. You don't need to provide your name or contact details, but
       if you do, we'll enter you into a prize draw where you'll have a chance
       to win prizes.

       --

       Facilities

       Was the porridge
       Too hot?
       Too cold?
       Just right?

       Were the beds
       Too hard?
       Too soft?
       Just right?

       Describe the chairs (select all you agree with)
       Comfy
       Luxurious
       Hi-tech
       Pretty
       Majestic

       --

       About your hosts

       Who's your favorite bear?
       Papa bear
       Mama bear
       Junior
       Dozer

       Which greeting did you prefer?
       Wave
       Friendly greeting
       Growl
       Claw marks in the door

       --

       Any other feedback?

       Give us your comments

       --

       Your details

       Name
       Email
       Phone

       --

       Submit

       --
     </body>
   </html>
   ```

3. Salvare il seguente listato CSS in un file all'interno della cartella chiamato `style.css` (oppure incollarlo nel pannello CSS dell'editor online).

   ```css live-sample___form-finished
   /* Basic font styles */

   body {
     background-color: white;
     color: #333333;
     font: 1em / 1.4 system-ui;
     padding: 1em;
     max-width: 800px;
     margin: 0 auto;
   }

   h1 {
     font-size: 2rem;
   }

   h2 {
     font-size: 1.6rem;
   }

   h1,
   h2 {
     margin: 0 0 20px;
     color: purple;
   }

   * {
     box-sizing: border-box;
   }

   p {
     color: gray;
     margin: 0.5em 0;
   }

   /* Form structure */

   fieldset {
     border: 0;
     padding: 0;
   }

   legend {
     padding-bottom: 10px;
     font-weight: bold;
   }

   fieldset,
   .separator {
     margin-bottom: 20px;
   }

   .form-section {
     margin-bottom: 20px;
     padding: 20px;
   }

   img {
     max-width: 100%;
     height: 50px;
     margin: 20px 0;
   }

   /* Individual form items */

   fieldset input {
     margin: 0 10px 0 0;
   }

   label {
     margin-right: 40px;
   }

   textarea {
     margin-top: 10px;
     padding: 5px;
     width: 100%;
     height: 200px;
   }

   .separator {
     display: flex;
   }

   .separator label {
     flex: 2;
   }

   .separator input,
   .separator select {
     flex: 3;
     padding: 5px;
   }

   button {
     padding: 10px 20px;
     border-radius: 10px;
     border: 1px solid grey;
     background-color: #dddddd;
     width: 50%;
     margin: 0 auto;
     display: block;
   }

   button:hover,
   button:focus {
     background-color: #eeeeee;
     cursor: pointer;
   }
   ```

## Descrizione del progetto

Immaginiamo di aver appena soggiornato in un hotel chiamato la piccola casa nel bosco (o almeno, si pensava che fosse un hotel). L'obiettivo è creare un modulo di feedback fittizio per l'hotel. Oltre a marcare le funzionalità richieste e a strutturare il modulo, sono presenti alcune funzionalità HTML aggiuntive da implementare.

### Implementare i controlli del modulo

1. Nella sezione "Facilities", trasformare le prime due serie di righe in gruppi di pulsanti radio, più un'etichetta che descriva ciascuno di essi e una legenda che descriva l'intero gruppo. Aggiungere un attributo affinché il primo pulsante radio in ciascun caso sia selezionato per impostazione predefinita.
2. Nella sezione "Facilities", trasformare la terza serie di righe in un insieme di caselle di controllo, con un'etichetta che descriva ciascuna di esse e una legenda che descriva l'intero gruppo.
3. Nella sezione "About your hosts", trasformare entrambe le serie di righe in un menu a discesa di opzioni, con un'etichetta che descriva ciascuno.
4. Nella sezione "Any other feedback?", aggiungere una casella di immissione di testo su più righe e trasformare la riga esistente nella relativa etichetta descrittiva.
5. Nella sezione "Your details", aggiungere un tipo adatto di input di testo per raccogliere ciascuno dei tre valori elencati. Trasformare le righe esistenti nelle relative etichette.
6. Trasformare "Submit" in un pulsante di invio per il modulo.

### Strutturare il modulo

1. Racchiudere il modulo in un elemento contenitore appropriato per specificare l'intero elemento come modulo.
2. Aggiungere elementi strutturali ripetuti all'interno del modulo, per racchiudere ogni sezione del modulo. Assegnare a ogni elemento di sezione del modulo una `class` pari a `form-section`. Per semplificare il compito, ogni sezione del modulo è circondata da due serie di doppi trattini (`--`). È possibile rimuovere i doppi trattini dopo aver aggiunto gli elementi strutturali.
3. Sarà necessario includere elementi strutturali aggiuntivi attorno ad alcune coppie controllo/etichetta affinché si trovino su righe separate. Aggiungerli ora, assegnando a ciascuno una `class` pari a `separator`.
4. Aggiungere un elemento di interruzione di riga tra la casella di immissione di testo su più righe e la relativa etichetta, in modo che i due elementi si trovino su righe separate.

### Funzionalità HTML aggiuntive

1. Nel testo sono presenti diverse intestazioni che devono essere marcate con elementi appropriati:
   1. L'intestazione di primo livello: "We want your feedback!".
   2. Le intestazioni di secondo livello: "Facilities", "About your hosts", "Any other feedback?" e "Your details".
2. Il paragrafo iniziale sotto l'intestazione di primo livello deve essere marcato in modo appropriato.
3. Sempre nel paragrafo iniziale, trasformare il testo "little house in the woods" e "prize draw" in link. Non sono ancora presenti pagine a cui collegarsi, quindi per il momento impostare l'URL di destinazione su `#` come segnaposto.
4. Inserire un'immagine ampia e piatta sotto il paragrafo iniziale come decorazione. Il percorso dell'immagine è `https://mdn.github.io/shared-assets/images/examples/learn/woodland-strip.jpg` e il testo alternativo deve essere impostato su un valore vuoto, poiché è soltanto decorativa.
5. In seguito al punto precedente, come obiettivo aggiuntivo, ricercare un modo migliore per includere l'immagine decorativa nella pagina e provare a farlo (ciò coinvolge una tecnologia diversa dall'HTML, che non è stata trattata in questo modulo).

## Suggerimenti e consigli

- Usare il [validatore HTML del W3C](https://validator.w3.org/) per individuare errori involontari nell'HTML, in modo da poterli correggere.
- Se si rimane bloccati e non si riesce a immaginare quali elementi inserire in quali punti, disegnare un semplice diagramma a blocchi del layout della pagina e annotare gli elementi che dovrebbero racchiudere ogni blocco. È molto utile.

## Esempio

Il seguente esempio dal vivo mostra l'aspetto che il modulo potrebbe avere dopo essere stato marcato. Se non è chiaro come ottenere alcune di queste funzionalità, vedere la soluzione seguente.

{{embedlivesample("form-finished", "100%", 500)}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe essere simile al seguente:

```html-nolint live-sample___form-finished
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Forms challenge</title>
    <link href="style.css" rel="stylesheet" />
    <script defer src="index.js"></script>
  </head>
  <body>
    <h1>We want your feedback!</h1>

    <p>
      We're very excited that you visited the
      <a href="#">little house in the woods</a>, and we want to hear what you
      thought of it! Please fill in the below sections. You don't need to
      provide your name or contact details, but if you do, we'll enter you into
      a <a href="#">prize draw</a> where you'll have a chance to win prizes.
    </p>

    <img
      src="https://mdn.github.io/shared-assets/images/examples/learn/woodland-strip.jpg"
      alt="" />

    <form>
      <div class="form-section">
        <h2>Facilities</h2>

        <fieldset>
          <legend>Was the porridge</legend>

          <input type="radio" id="porridge-1" name="porridge" value="hot"
                 checked />
          <label for="porridge-1">Too hot?</label>

          <input type="radio" id="porridge-2" name="porridge" value="cold" />
          <label for="porridge-2">Too cold?</label>

          <input type="radio" id="porridge-3" name="porridge" value="right" />
          <label for="porridge-3">Just right?</label>
        </fieldset>

        <fieldset>
          <legend>Were the beds</legend>

          <input type="radio" id="beds-1" name="beds" value="hard" checked />
          <label for="beds-1">Too hard?</label>

          <input type="radio" id="beds-2" name="beds" value="soft" />
          <label for="beds-2">Too soft?</label>

          <input type="radio" id="beds-3" name="beds" value="right" />
          <label for="beds-3">Just right?</label>
        </fieldset>

        <fieldset>
          <legend>Describe the chairs (select all you agree with)</legend>

          <input type="checkbox" id="comfy" name="comfy" />
          <label for="comfy">Comfy</label>

          <input type="checkbox" id="luxurious" name="luxurious" />
          <label for="luxurious">Luxurious</label>

          <input type="checkbox" id="hi-tech" name="hi-tech" />
          <label for="hi-tech">Hi-tech</label>

          <input type="checkbox" id="pretty" name="pretty" />
          <label for="pretty">Pretty</label>

          <input type="checkbox" id="majestic" name="majestic" />
          <label for="majestic">Majestic</label>
        </fieldset>
      </div>

      <div class="form-section">
        <h2>About your hosts</h2>

        <div class="separator">
          <label for="favorite">Who's your favorite bear?</label>
          <select name="favorite" id="favorite">
            <option value="papa">Papa bear</option>
            <option value="mama">Mama bear</option>
            <option value="junior">Junior</option>
            <option value="dozer">Dozer</option>
          </select>
        </div>

        <div class="separator">
          <label for="greeting">Which greeting did you prefer?</label>
          <select name="greeting" id="greeting">
            <option value="wave">Wave</option>
            <option value="friendly">Friendly greeting</option>
            <option value="growl">Growl</option>
            <option value="claw">Claw marks in the door</option>
          </select>
        </div>
      </div>

      <div class="form-section">
        <h2>Any other feedback?</h2>

        <label for="comments">Give us your comments</label>
        <br />
        <textarea id="comments" name="comments"></textarea>
      </div>

      <div class="form-section">
        <h2>Your details</h2>

        <div class="separator">
          <label for="name">Name</label>
          <input type="text" id="name" name="name" />
        </div>

        <div class="separator">
          <label for="email">Email</label>
          <input type="email" id="email" name="email" />
        </div>

        <div class="separator">
          <label for="phone">Phone</label>
          <input type="tel" id="phone" name="phone" />
        </div>
      </div>

      <div class="form-section">
        <button>Submit</button>
      </div>
    </form>
  </body>
</html>
```

Per l'obiettivo aggiuntivo, un modo probabilmente migliore per aggiungere immagini decorative a una pagina web consiste nell'usare le [immagini di sfondo CSS](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders#background_images). Eliminare l'elemento `<img>` e usare la proprietà CSS {{cssxref("background")}} per posizionare invece l'immagine nella pagina. Un buon elemento su cui inserire l'immagine di sfondo sarebbe l'elemento `<form>`, ed è necessario indicare al browser di non ripetere l'immagine. È inoltre necessario fornire valori di {{cssxref("margin")}} e {{cssxref("padding")}} per distanziare l'immagine di sfondo, in modo che non si sovrapponga al testo.

```css
form {
  background: url("https://mdn.github.io/shared-assets/images/examples/learn/woodland-strip.jpg")
    no-repeat;
  margin-top: 20px;
  padding-top: 50px;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Forms_and_buttons", "Learn_web_development/Core/Structuring_content/Debugging_HTML", "Learn_web_development/Core/Structuring_content")}}
