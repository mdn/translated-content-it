---
title: Il tuo primo modulo
slug: Learn_web_development/Extensions/Forms/Your_first_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms")}}

Il primo articolo della nostra serie ti offre la tua prima esperienza nella creazione di un modulo web, compreso il design di un modulo semplice, l'implementazione utilizzando i controlli modulo HTML corretti e altri elementi HTML, l'aggiunta di uno stile molto semplice tramite CSS, e descrivendo come i dati vengono inviati a un server. Amplieremo ciascuno di questi sottotemi in modo più dettagliato successivamente nel modulo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con cosa sono i moduli web, a cosa servono, come
        pensare a progettarli, e gli elementi HTML di base di cui avrai bisogno
        per i casi semplici.
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono i moduli web?

I **moduli web** sono uno dei principali punti di interazione tra un utente e un sito web o un'applicazione. Permettono agli utenti di inserire dati, che generalmente vengono inviati a un server web per l'elaborazione e l'archiviazione (vedi [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) più avanti nel modulo) o utilizzati lato client per aggiornare immediatamente l'interfaccia in qualche modo (ad esempio, aggiungere un altro elemento a una lista, o mostrare o nascondere una funzionalità dell'interfaccia utente).

L'HTML di un modulo web è composto da uno o più **controlli modulo** (talvolta chiamati **widget**), più alcuni elementi aggiuntivi per aiutare a strutturare il modulo generale — sono spesso indicati come **moduli HTML**. I controlli possono essere campi di testo a una o più linee, caselle a discesa, pulsanti, checkbox o pulsanti di opzione, e sono principalmente creati utilizzando l'elemento {{htmlelement("input")}}, anche se ci sono altri elementi da conoscere.

I controlli modulo possono anche essere programmati per imporre formati o valori specifici da inserire (**validazione del modulo**), e abbinati a etichette di testo che descrivono il loro scopo agli utenti sia vedenti che non vedenti.

## Progettare il tuo modulo

Prima di iniziare a codificare, è sempre meglio fare un passo indietro e prendere il tempo necessario per pensare al tuo modulo. Progettare un mockup rapido ti aiuterà a definire il giusto insieme di dati che desideri richiedere al tuo utente di inserire. Dal punto di vista dell'esperienza utente (UX), è importante ricordare che più grande è il tuo modulo, più rischi di frustrare le persone e perdere utenti. Mantienilo semplice e rimani focalizzato: chiedi solo i dati di cui hai assolutamente bisogno.

Progettare moduli è un passo importante quando stai costruendo un sito o un'applicazione. È al di là dell'ambito di questo articolo coprire l'esperienza utente dei moduli, ma se vuoi approfondire quell'argomento dovresti leggere i seguenti articoli:

- Smashing Magazine ha alcuni [buoni articoli su UX dei moduli](https://www.smashingmagazine.com/2018/08/ux-html5-mobile-form-part-1/), incluso un articolo più vecchio ma ancora rilevante [Guida Estesa all'Usabilità dei Moduli Web](https://www.smashingmagazine.com/2011/11/extensive-guide-web-form-usability/).
- UXMatters è anche una risorsa molto ponderata con buoni consigli dalle [migliori pratiche di base](https://www.uxmatters.com/mt/archives/2012/05/7-basic-best-practices-for-buttons.php) a preoccupazioni complesse come [moduli multipagina](https://www.uxmatters.com/mt/archives/2010/03/pagination-in-web-forms-evaluating-the-effectiveness-of-web-forms.php).

In questo articolo, costruiremo un semplice modulo di contatto. Facciamo uno schizzo approssimativo.

![Il modulo da costruire, schizzo approssimativo](form-sketch-low.jpg)

Il nostro modulo conterrà tre campi di testo e un pulsante. Stiamo chiedendo all'utente il loro nome, la loro email e il messaggio che vogliono inviare. Premendo il pulsante, i loro dati verranno inviati a un server web.

## Apprendimento attivo: Implementare il nostro HTML del modulo

Ok, proviamo a creare l'HTML per il nostro modulo. Useremo i seguenti elementi HTML: {{HTMLelement("form")}}, {{HTMLelement("label")}}, {{HTMLelement("input")}}, {{HTMLelement("textarea")}}, e {{HTMLelement("button")}}.

Prima di proseguire, fai una copia locale del nostro [template HTML semplice](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) — inserirai qui il tuo HTML del modulo.

### Elemento `<form>`

Tutti i moduli iniziano con un elemento {{HTMLelement("form")}}, come questo:

```html
<form action="/my-handling-form-page" method="post">…</form>
```

Questo elemento definisce formalmente un modulo. È un elemento contenitore come un elemento {{HTMLelement("section")}} o {{HTMLelement("footer")}}, ma specificamente per contenere moduli; supporta anche alcuni attributi specifici per configurare il modo in cui il modulo si comporta. Tutti i suoi attributi sono opzionali, ma è prassi standard impostare sempre almeno gli attributi [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method):

- L'attributo `action` definisce la posizione (URL) in cui i dati raccolti del modulo devono essere inviati quando viene inviato.
- L'attributo `method` definisce quale metodo HTTP utilizzare per inviare i dati (solitamente `get` o `post`).

> [!NOTE]
> Vedremo come funzionano questi attributi nel nostro articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) più avanti.

Per ora, aggiungi l'elemento {{htmlelement("form")}} sopra nel tuo {{htmlelement("body")}} HTML.

### Gli elementi `<label>`, `<input>`, e `<textarea>`

Il nostro modulo di contatto non è complesso: la parte di inserimento dati contiene tre campi di testo, ognuno con una corrispondente {{HTMLelement("label")}}:

- Il campo di input per il nome è un {{HTMLelement("input/text", "campo di testo a riga singola")}}.
- Il campo di input per l'email è un {{HTMLelement("input/email", "input di tipo email")}}: un campo di testo a riga singola che accetta solo indirizzi email.
- Il campo di input per il messaggio è un {{HTMLelement("textarea")}}; un campo di testo multilinea.

In termini di codice HTML, abbiamo bisogno di qualcosa di simile al seguente per implementare questi widget del modulo:

```html
<form action="/my-handling-form-page" method="post">
  <p>
    <label for="name">Name:</label>
    <input type="text" id="name" name="user_name" />
  </p>
  <p>
    <label for="mail">Email:</label>
    <input type="email" id="mail" name="user_email" />
  </p>
  <p>
    <label for="msg">Message:</label>
    <textarea id="msg" name="user_message"></textarea>
  </p>
</form>
```

Aggiorna il codice del tuo modulo per assomigliare a quanto sopra.

Gli elementi {{HTMLelement("p")}} sono lì per strutturare comodamente il nostro codice e rendere più facile lo styling (vedi più avanti nell'articolo). Per usabilità e accessibilità, includiamo un'etichetta esplicita per ogni controllo modulo. Nota l'uso dell'attributo [`for`](/it/docs/Web/HTML/Reference/Attributes/for) su tutti gli elementi {{HTMLelement("label")}}, che prende come valore l'attributo [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) del controllo modulo con il quale è associato — questo è come associ un controllo modulo con la sua etichetta.

Ci sono grandi benefici nel farlo — associa l'etichetta con il controllo modulo, consentendo agli utenti di mouse, trackpad e dispositivi touch di cliccare sull'etichetta per attivare il controllo corrispondente, e fornisce anche un nome accessibile che i lettori di schermo leggeranno ai loro utenti. Troverai ulteriori dettagli sulle etichette dei moduli in [Come strutturare un modulo web](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form).

Sull'elemento {{HTMLelement("input")}}, l'attributo più importante è l'attributo `type`. Questo attributo è estremamente importante perché definisce il modo in cui l'elemento {{HTMLelement("input")}} appare e si comporta. Troverai maggiori dettagli in merito nell'articolo [Controlli modulo nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls) più avanti.

- Nel nostro esempio semplice, utilizziamo il valore {{HTMLelement("input/text", "text")}} per il primo input — il valore predefinito per questo attributo. Rappresenta un campo di testo a riga singola di base che accetta qualsiasi tipo di input di testo.
- Per il secondo input, utilizziamo il valore {{HTMLelement("input/email", "email")}}, che definisce un campo di testo a riga singola che accetta solo un indirizzo email ben formato. Questo trasforma un campo di testo di base in una sorta di campo "intelligente" che effettuerà alcuni controlli di validazione sui dati digitati dall'utente. Fa anche apparire un layout di tastiera più appropriato per inserire indirizzi email (ad esempio, con un simbolo @ di default) su dispositivi con tastiere dinamiche, come gli smartphone. Scoprirai di più sulla validazione dei moduli nell'articolo [Validazione modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) più avanti.

Ultimo ma non meno importante, nota la sintassi di `<input>` vs. `<textarea></textarea>`. Questa è una delle stranezze dell'HTML. Il tag `<input>` è un {{Glossary("void_element", "elemento vuoto")}}, il che significa che non ha bisogno di un tag di chiusura. {{HTMLElement("textarea")}} non è un elemento vuoto, il che significa che dovrebbe essere chiuso con il tag di chiusura appropriato. Questo ha un impatto su una caratteristica specifica dei moduli: il modo in cui definisci il valore predefinito. Per definire il valore predefinito di un elemento {{HTMLElement("input")}}, devi usare l'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/input#value) in questo modo:

```html
<input type="text" value="by default this element is filled with this text" />
```

D'altra parte, se vuoi definire un valore predefinito per un {{HTMLElement("textarea")}}, lo metti tra i tag di apertura e chiusura dell'elemento {{HTMLElement("textarea")}}, in questo modo:

```html
<textarea>
by default this element is filled with this text
</textarea>
```

### Elemento `<button>`

Il markup per il nostro modulo è quasi completo; dobbiamo solo aggiungere un pulsante per consentire all'utente di inviare, o "sottoporre", i suoi dati una volta compilato il modulo. Questo viene fatto tramite l'elemento {{HTMLelement("button")}}; aggiungi quanto segue appena sopra il tag di chiusura `</form>`:

```html
<p class="button">
  <button type="submit">Send your message</button>
</p>
```

L'elemento {{htmlelement("button")}} accetta anche un attributo `type` — questo accetta uno dei tre valori: `submit`, `reset`, o `button`.

- Un clic su un pulsante `submit` (il valore predefinito) invia i dati del modulo alla pagina web definita dall'attributo `action` dell'elemento {{HTMLelement("form")}}.
- Un clic su un pulsante `reset` reimposta immediatamente tutti i widget del modulo al loro valore predefinito. Da un punto di vista UX, questo è considerato una cattiva pratica, quindi dovresti evitare di usare questo tipo di pulsante a meno che tu non abbia una buona ragione per includerne uno.
- Un clic su un pulsante `button` non fa _niente_! Questo può sembrare stupido, ma è incredibilmente utile per costruire pulsanti personalizzati — puoi definire la loro funzionalità scelta con JavaScript.

> [!NOTE]
> Puoi anche usare l'elemento {{HTMLElement("input")}} con il corrispondente `type` per produrre un pulsante, ad esempio `<input type="submit">`. Il principale vantaggio dell'elemento {{HTMLelement("button")}} è che l'elemento {{HTMLelement("input")}} permette solo testo semplice nella sua etichetta, mentre l'elemento {{HTMLelement("button")}} consente contenuto HTML completo, permettendo contenuti di pulsanti più complessi e creativi.

## Stile di base del modulo

Ora che hai finito di scrivere il codice HTML del tuo modulo, prova a salvarlo e guardarlo in un browser. Al momento, vedrai che sembra piuttosto brutto.

> [!NOTE]
> Se non pensi di aver scritto correttamente il codice HTML, prova a confrontarlo con il nostro esempio finito — vedi [first-form.html](https://github.com/mdn/learning-area/blob/main/html/forms/your-first-HTML-form/first-form.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form.html)).

I moduli sono notoriamente difficili da stilizzare bene. Va oltre l'ambito di questo articolo insegnarti lo stile per i moduli nel dettaglio, quindi per il momento ti faremo solo aggiungere del CSS per farlo sembrare a posto.

Prima di tutto, aggiungi un elemento {{htmlelement("style")}} alla tua pagina, all'interno della testa del tuo HTML. Dovrebbe assomigliare a questo modo:

```html
<style>
  …
</style>
```

All'interno dei tag `style`, aggiungi il seguente CSS:

```css
body {
  /* Center the form on the page */
  text-align: center;
}

form {
  display: inline-block;
  /* Form outline */
  padding: 1em;
  border: 1px solid #ccc;
  border-radius: 1em;
}

p + p {
  margin-top: 1em;
}

label {
  /* Uniform size & alignment */
  display: inline-block;
  min-width: 90px;
  text-align: right;
}

input,
textarea {
  /* To make sure that all text fields have the same font settings
     By default, text areas have a monospace font */
  font: 1em sans-serif;
  /* Uniform text field size */
  width: 300px;
  box-sizing: border-box;
  /* Match form field borders */
  border: 1px solid #999;
}

input:focus,
textarea:focus {
  /* Set the outline width and style */
  outline-style: solid;
  /* To give a little highlight on active elements */
  outline-color: #000;
}

textarea {
  /* Align multiline text fields with their labels */
  vertical-align: top;
  /* Provide space to type some text */
  height: 5em;
}

.button {
  /* Align buttons with the text fields */
  padding-left: 90px; /* same size as the label elements */
}

button {
  /* This extra margin represent roughly the same space as the space
     between the labels and their text fields */
  margin-left: 0.5em;
}
```

Salva e ricarica, e vedrai che il tuo modulo dovrebbe sembrare molto meno brutto.

> [!NOTE]
> Puoi trovare la nostra versione su GitHub su [first-form-styled.html](https://github.com/mdn/learning-area/blob/main/html/forms/your-first-HTML-form/first-form-styled.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form-styled.html)).

## Invio dei dati del modulo al tuo server web

L'ultima parte, e forse la più complicata, è gestire i dati del modulo lato server. L'elemento {{HTMLelement("form")}} definisce dove e come inviare i dati grazie agli attributi [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method).

Forniamo un attributo `name` per ciascun controllo modulo. I nomi sono importanti sia lato client che server; dicono al browser quale nome dare a ciascun pezzo di dati e, lato server, permettono al server di gestire ciascun pezzo di dati per nome. I dati del modulo vengono inviati al server come coppie nome/valore.

Per nominare i dati in un modulo, devi usare l'attributo `name` su ogni widget del modulo che raccoglierà un specifico pezzo di dati. Diamo un'altra occhiata a parte del nostro codice del modulo:

```html
<form action="/my-handling-form-page" method="post">
  <p>
    <label for="name">Name:</label>
    <input type="text" id="name" name="user_name" />
  </p>
  <p>
    <label for="mail">Email:</label>
    <input type="email" id="mail" name="user_email" />
  </p>
  <p>
    <label for="msg">Message:</label>
    <textarea id="msg" name="user_message"></textarea>
  </p>

  …
</form>
```

Nel nostro esempio, il modulo invierà 3 pezzi di dati denominati `user_name`, `user_email`, e `user_message`. Questi dati verranno inviati all'URL `/my-handling-form-page` utilizzando il metodo [HTTP `POST`](/it/docs/Web/HTTP/Reference/Methods/POST).

Lato server, lo script all'URL `/my-handling-form-page` riceverà i dati come un elenco di 3 elementi chiave/valore contenuti nella richiesta HTTP. Il modo in cui questo script gestirà quei dati dipende da te. Ogni linguaggio lato server (PHP, Python, Ruby, Java, C#, ecc.) ha il proprio meccanismo di gestione dei dati del modulo. È al di là dello scopo di questo tutorial approfondire quell'argomento, ma se vuoi sapere di più, abbiamo fornito alcuni esempi nel nostro articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) più avanti.

## Riepilogo

Congratulazioni, hai costruito il tuo primo modulo web. Questo appare così dal vivo:

```html hidden
<form action="/my-handling-form-page" method="post">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" name="user_name" />
  </div>

  <div>
    <label for="mail">Email:</label>
    <input type="email" id="mail" name="user_email" />
  </div>

  <div>
    <label for="msg">Message:</label>
    <textarea id="msg" name="user_message"></textarea>
  </div>

  <div class="button">
    <button type="submit">Send your message</button>
  </div>
</form>
```

```css hidden
form {
  /* Just to center the form on the page */
  margin: 0 auto;
  width: 400px;

  /* To see the limits of the form */
  padding: 1em;
  border: 1px solid #ccc;
  border-radius: 1em;
}

div + div {
  margin-top: 1em;
}

label {
  /* To make sure that all label have the same size and are properly align */
  display: inline-block;
  width: 90px;
  text-align: right;
}

input,
textarea {
  /* To make sure that all text field have the same font settings
     By default, textarea are set with a monospace font */
  font: 1em sans-serif;

  /* To give the same size to all text field */
  width: 300px;

  -moz-box-sizing: border-box;
  box-sizing: border-box;

  /* To harmonize the look & feel of text field border */
  border: 1px solid #999;
}

input:focus,
textarea:focus {
  /* To give a little highlight on active elements */
  border-color: #000;
}

textarea {
  /* To properly align multiline text field with their label */
  vertical-align: top;

  /* To give enough room to type some text */
  height: 5em;

  /* To allow users to resize any textarea vertically
     It works only on Chrome, Firefox and Safari */
  resize: vertical;
}

.button {
  /* To position the buttons to the same position of the text fields */
  padding-left: 90px; /* same size as the label elements */
}

button {
  /* This extra margin represent the same space as the space between
     the labels and their text fields */
  margin-left: 0.5em;
}
```

{{ EmbedLiveSample('Summary', '', '300') }}

Questo è solo l'inizio, tuttavia — ora è il momento di dare un'occhiata più approfondita. I moduli hanno molto più potere di quello che abbiamo visto qui e gli altri articoli in questo modulo ti aiuteranno a padroneggiare il resto.

{{NextMenu("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms")}}
