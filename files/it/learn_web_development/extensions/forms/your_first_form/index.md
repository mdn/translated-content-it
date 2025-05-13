---
title: Il suo primo modulo
slug: Learn_web_development/Extensions/Forms/Your_first_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms")}}

Il primo articolo della nostra serie le offre la sua prima esperienza nella creazione di un modulo web, che include la progettazione di un modulo semplice, la sua implementazione utilizzando i controlli del modulo HTML appropriati e altri elementi HTML, l'aggiunta di un po' di stile mediante CSS, e la descrizione di come i dati vengono inviati a un server. Approfondiremo ciascuno di questi sottotemi più avanti nel modulo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >dell'HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con cosa sono i moduli web, a cosa servono, come progettare i moduli e gli elementi HTML di base che le serviranno per casi semplici.
      </td>
    </tr>
  </tbody>
</table>

## Che cosa sono i moduli web?

I **moduli web** sono uno dei principali punti di interazione tra un utente e un sito web o un'applicazione. I moduli permettono agli utenti di inserire dati, che generalmente vengono inviati a un server web per l'elaborazione e l'archiviazione (vedi [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) più avanti nel modulo), o utilizzati lato client per aggiornare immediatamente l'interfaccia in qualche modo (ad esempio, aggiungere un altro elemento a una lista, o mostrare o nascondere una funzionalità dell'interfaccia utente).

L'HTML di un modulo web è costituito da uno o più **controlli del modulo** (talvolta chiamati **widget**), più alcuni elementi aggiuntivi per aiutare a strutturare il modulo globale — spesso sono chiamati **moduli HTML**. I controlli possono essere campi di testo a singola o multipla linea, caselle a discesa, pulsanti, caselle di controllo o pulsanti radio, e sono per lo più creati utilizzando l'elemento {{htmlelement("input")}}, anche se ci sono altri elementi da imparare a conoscere.

I controlli del modulo possono anche essere programmati per imporre formati o valori specifici da inserire (**validazione del modulo**), e associati a etichette di testo che descrivono il loro scopo sia per gli utenti vedenti che per quelli non vedenti.

## Progettare il suo modulo

Prima di iniziare a codificare, è sempre meglio fare un passo indietro e prendersi il tempo necessario per pensare al suo modulo. Progettare un rapido schizzo l'aiuterà a definire il giusto insieme di dati che vuole chiedere all'utente di inserire. Dal punto di vista dell'esperienza utente (UX), è importante ricordare che quanto più grande è il suo modulo, tanto più rischia di frustrare le persone e perdere utenti. Mantenga la semplicità e si concentri: chieda solo i dati di cui ha assolutamente bisogno.

Progettare moduli è un passo importante quando si costruisce un sito o un'applicazione. È oltre l'ambito di questo articolo trattare l'esperienza utente dei moduli, ma se vuole approfondire l'argomento dovrebbe leggere i seguenti articoli:

- Smashing Magazine ha alcuni [buoni articoli sull'UX dei moduli](https://www.smashingmagazine.com/2018/08/ux-html5-mobile-form-part-1/), inclusi un articolo più vecchio ma ancora rilevante [Guida Esterna Alla Usabilità dei Moduli Web](https://www.smashingmagazine.com/2011/11/extensive-guide-web-form-usability/).
- UXMatters è anche una risorsa molto riflessiva con buoni consigli da [pratiche migliori di base](https://www.uxmatters.com/mt/archives/2012/05/7-basic-best-practices-for-buttons.php) a problemi complessi come [moduli multipagina](https://www.uxmatters.com/mt/archives/2010/03/pagination-in-web-forms-evaluating-the-effectiveness-of-web-forms.php).

In questo articolo, costruiremo un semplice modulo di contatto. Creiamo uno schizzo approssimativo.

![Il modulo da costruire, schizzo approssimativo](form-sketch-low.jpg)

Il nostro modulo conterrà tre campi di testo e un pulsante. Chiediamo all'utente il suo nome, la sua email e il messaggio che vuole inviare. Premendo il pulsante, i suoi dati verranno inviati a un server web.

## Apprendimento attivo: Implementare l'HTML del nostro modulo

Ok, proviamo a creare l'HTML per il nostro modulo. Useremo i seguenti elementi HTML: {{HTMLelement("form")}}, {{HTMLelement("label")}}, {{HTMLelement("input")}}, {{HTMLelement("textarea")}}, e {{HTMLelement("button")}}.

Prima di proseguire, faccia una copia locale del nostro [semplice modello HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) — inserirà qui il suo HTML del modulo.

### L'elemento `<form>`

Tutti i moduli iniziano con un elemento {{HTMLelement("form")}}, come questo:

```html
<form action="/my-handling-form-page" method="post">…</form>
```

Questo elemento definisce formalmente un modulo. È un elemento contenitore come un elemento {{HTMLelement("section")}} o {{HTMLelement("footer")}}, ma specifico per contenere moduli; supporta anche alcuni attributi specifici per configurare il modo in cui il modulo si comporta. Tutti i suoi attributi sono opzionali, ma è una pratica standard impostare sempre almeno gli attributi [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method):

- L'attributo `action` definisce la posizione (URL) in cui i dati raccolti dal modulo dovrebbero essere inviati quando viene inviato.
- L'attributo `method` definisce quale metodo HTTP utilizzare per inviare i dati (solitamente `get` o `post`).

> [!NOTE]
> Vedremo come funzionano questi attributi nel nostro articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) più avanti.

Per ora, aggiunga l'elemento {{htmlelement("form")}} sopra nel suo HTML {{htmlelement("body")}}.

### Gli elementi `<label>`, `<input>`, e `<textarea>`

Il nostro modulo di contatto non è complesso: la parte di inserimento dei dati contiene tre campi di testo, ciascuno con un corrispondente {{HTMLelement("label")}}:

- Il campo di input per il nome è un {{HTMLelement("input/text", "campo di testo a singola linea")}}.
- Il campo di input per l'email è un {{HTMLelement("input/email", "input di tipo email")}}: un campo di testo a singola linea che accetta solo indirizzi email.
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

Aggiorni il suo codice del modulo in modo che corrisponda al codice sopra.

Gli elementi {{HTMLelement("p")}} sono lì per strutturare convenientemente il codice e facilitare la stilizzazione (vedi più avanti nell'articolo). Per usabilità e accessibilità, includiamo un'etichetta esplicita per ogni controllo del modulo. Noti l'utilizzo dell'attributo [`for`](/it/docs/Web/HTML/Reference/Attributes/for) su tutti gli elementi {{HTMLelement("label")}}, che prende come valore l'[`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) del controllo del modulo con cui è associato — questo è il modo in cui si associa un controllo del modulo con la sua etichetta.

Il vantaggio di farlo è grande — associa l'etichetta al controllo del modulo, consentendo agli utenti di mouse, trackpad e dispositivi touch di cliccare sull'etichetta per attivare il controllo corrispondente, e fornisce anche un nome accessibile che i lettori di schermo possono leggere ai loro utenti. Troverà ulteriori dettagli sulle etichette dei moduli in [Come strutturare un modulo web](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form).

Sull'elemento {{HTMLelement("input")}}, l'attributo più importante è l'attributo `type`. Questo attributo è estremamente importante perché definisce il modo in cui l'elemento {{HTMLelement("input")}} appare e si comporta. Troverà maggiori informazioni su questo nell'articolo [Controlli del modulo nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls) più avanti.

- Nel nostro esempio semplice, usiamo il valore {{HTMLelement("input/text", "text")}} per il primo input — il valore predefinito per questo attributo. Rappresenta un campo di testo a singola linea di base che accetta qualsiasi tipo di input di testo.
- Per il secondo input, usiamo il valore {{HTMLelement("input/email", "email")}}, che definisce un campo di testo a singola linea che accetta solo indirizzi email ben formati. Questo trasforma un campo di testo di base in un campo "intelligente" che eseguirà alcuni controlli di validazione sui dati digitati dall'utente. Inoltre, fa apparire una disposizione della tastiera più appropriata per l'inserimento degli indirizzi email (ad esempio, con un simbolo @ di default) su dispositivi con tastiere dinamiche, come gli smartphone. Troverà ulteriori informazioni sulla validazione dei moduli nell'articolo [validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) più avanti.

Infine, ma non meno importante, noti la sintassi di `<input>` rispetto a `<textarea></textarea>`. Questa è una delle stranezze dell'HTML. Il tag `<input>` è un {{Glossary("void_element", "elemento vuoto")}}, il che significa che non ha bisogno di un tag di chiusura. {{HTMLElement("textarea")}} non è un elemento vuoto, il che significa che dovrebbe essere chiuso con il tag di chiusura appropriato. Questo ha un impatto su una specifica caratteristica dei moduli: il modo in cui definisce il valore predefinito. Per definire il valore predefinito di un elemento {{HTMLElement("input")}} deve utilizzare l'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/input#value) in questo modo:

```html
<input type="text" value="by default this element is filled with this text" />
```

D'altra parte, se vuole definire un valore predefinito per un {{HTMLElement("textarea")}}, deve inserirlo tra i tag di apertura e di chiusura dell'elemento {{HTMLElement("textarea")}}, in questo modo:

```html
<textarea>
by default this element is filled with this text
</textarea>
```

### L'elemento `<button>`

Il markup per il nostro modulo è quasi completo; ci manca solo di aggiungere un pulsante per permettere all'utente di inviare, o "sottoporre", i suoi dati una volta che ha compilato il modulo. Questo viene fatto utilizzando l'elemento {{HTMLelement("button")}}; aggiunga quanto segue appena sopra il tag di chiusura `</form>`:

```html
<p class="button">
  <button type="submit">Send your message</button>
</p>
```

L'elemento {{htmlelement("button")}} accetta anche un attributo `type` — questo accetta uno di tre valori: `submit`, `reset` o `button`.

- Un clic su un pulsante `submit` (il valore di default) invia i dati del modulo alla pagina web definita dall'attributo `action` dell'elemento {{HTMLelement("form")}}.
- Un clic su un pulsante `reset` reimposta immediatamente tutti i widget del modulo al loro valore predefinito. Dal punto di vista dell'UX, questo è considerato una cattiva pratica, quindi dovrebbe evitare di usare questo tipo di pulsante a meno che non abbia davvero una buona ragione per includerne uno.
- Un clic su un pulsante `button` non fa nulla! Questo sembra sciocco, ma è straordinariamente utile per costruire pulsanti personalizzati — può definire la loro funzionalità prescelta con JavaScript.

> [!NOTE]
> Può anche usare l'elemento {{HTMLElement("input")}} con il corrispondente `type` per produrre un pulsante, per esempio `<input type="submit">`. Il principale vantaggio dell'elemento {{HTMLelement("button")}} è che l'elemento {{HTMLelement("input")}} permette solo testo semplice nella sua etichetta mentre l'elemento {{HTMLelement("button")}} permette contenuto HTML completo, permettendo contenuto del pulsante più complesso e creativo.

## Stile di base del modulo

Ora che ha finito di scrivere il codice HTML del suo modulo, provi a salvarlo e a guardarlo in un browser. Al momento, noterà che appare piuttosto brutto.

> [!NOTE]
> Se pensa di non aver inserito correttamente il codice HTML, provi a confrontarlo con il nostro esempio finale — veda [first-form.html](https://github.com/mdn/learning-area/blob/main/html/forms/your-first-HTML-form/first-form.html) ([vedi anche live](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form.html)).

I moduli sono notoriamente difficili da stilizzare in modo gradevole. È oltre l'ambito di questo articolo insegnarle a stilizzare i moduli in dettaglio, quindi per il momento le chiederemo di aggiungere un po' di CSS per farlo sembrare decente.

Per prima cosa, aggiunga un elemento {{htmlelement("style")}} alla sua pagina, all'interno del suo head HTML. Dovrebbe apparire così:

```html
<style>
  …
</style>
```

All'interno dei tag `style`, aggiunga il seguente CSS:

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

Salvi e ricarichi, e noterà che il suo modulo dovrebbe apparire molto meno brutto.

> [!NOTE]
> Può trovare la nostra versione su GitHub a [first-form-styled.html](https://github.com/mdn/learning-area/blob/main/html/forms/your-first-HTML-form/first-form-styled.html) ([vedi anche live](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form-styled.html)).

## Invio dei dati del modulo al suo server web

L'ultima parte, e forse la più complicata, è gestire i dati del modulo lato server. L'elemento {{HTMLelement("form")}} definisce dove e come inviare i dati grazie agli attributi [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method).

Forniamo un attributo `name` per ciascun controllo del modulo. I nomi sono importanti sia lato client che lato server; dicono al browser quale nome dare a ciascun pezzo di dati e, lato server, permettono al server di gestire ciascun pezzo di dati per nome. I dati del modulo vengono inviati al server come coppie nome/valore.

Per nominare i dati in un modulo, deve utilizzare l'attributo `name` su ciascun widget del modulo che raccoglierà un pezzo di dati specifico. Diamo un'occhiata ad alcuni dei nostri codici del modulo di nuovo:

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

Nel nostro esempio, il modulo invierà 3 pezzi di dati chiamati `user_name`, `user_email`, e `user_message`. Questi dati verranno inviati all'URL `/my-handling-form-page` utilizzando il metodo [HTTP `POST`](/it/docs/Web/HTTP/Reference/Methods/POST).

Lato server, lo script all'URL `/my-handling-form-page` riceverà i dati come un elenco di 3 elementi chiave/valore contenuti nella richiesta HTTP. Il modo in cui questo script gestirà quei dati dipende da lei. Ogni linguaggio lato server (PHP, Python, Ruby, Java, C#, ecc.) ha il proprio meccanismo per gestire i dati del modulo. È oltre l'ambito di questo tutorial approfondire tale argomento, ma se vuole sapere di più, le abbiamo fornito alcuni esempi nel nostro articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) più avanti.

## Riepilogo

Congratulazioni, ha costruito il suo primo modulo web. Assomiglia a questo live:

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

Questo è solo l'inizio, tuttavia — ora è il momento di approfondire. I moduli hanno molto più potenziale di quello che abbiamo visto qui e gli altri articoli in questo modulo la aiuteranno a padroneggiare il resto.

{{NextMenu("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms")}}
