---
title: Il primo modulo
slug: Learn_web_development/Extensions/Forms/Your_first_form
l10n:
  sourceCommit: f33de00c56ac53878eb2cb7cb5849df1f9ab8db7
---

{{NextMenu("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms")}}

Il primo articolo della nostra serie offre la prima esperienza nella creazione di un modulo web, inclusi la progettazione di un semplice modulo, la sua implementazione usando i controlli per moduli HTML e altri elementi HTML appropriati, l'aggiunta di uno stile molto semplice tramite CSS e la descrizione di come i dati vengono inviati a un server.
Ciascuno di questi sottoargomenti verrà approfondito più avanti nel modulo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con i moduli web, i loro utilizzi, come pensarne la
        progettazione e gli elementi HTML di base necessari per i casi semplici.
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono i moduli web?

I **moduli web** sono uno dei principali punti di interazione tra un utente e un sito web o un'applicazione.
I moduli consentono agli utenti di immettere dati, che generalmente vengono inviati a un server web per l'elaborazione e l'archiviazione (vedere [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) più avanti nel modulo), oppure vengono usati lato client per aggiornare immediatamente l'interfaccia in qualche modo (ad esempio, aggiungere un altro elemento a un elenco o mostrare o nascondere una funzionalità dell'interfaccia utente).

L'HTML di un modulo web è composto da uno o più **controlli del modulo** (talvolta chiamati **widget**), oltre ad alcuni elementi aggiuntivi che aiutano a strutturare il modulo nel suo insieme: vengono spesso indicati come **moduli HTML**.
I controlli possono essere campi di testo a riga singola o multilinea, caselle a discesa, pulsanti, caselle di controllo o pulsanti di opzione e sono creati principalmente usando l'elemento {{htmlelement("input")}}, sebbene vi siano anche altri elementi da conoscere.

I controlli del modulo possono anche essere programmati per imporre l'immissione di formati o valori specifici (**convalida del modulo**) e associati a etichette di testo che ne descrivono lo scopo sia agli utenti vedenti sia a quelli con disabilità visive.

## Progettare il modulo

Prima di iniziare a scrivere codice, è sempre meglio fare un passo indietro e dedicare tempo a riflettere sul modulo. Progettare rapidamente un mockup aiuterà a definire il giusto insieme di dati da chiedere all'utente. Dal punto di vista dell'esperienza utente (UX), è importante ricordare che più grande è il modulo, maggiore è il rischio di frustrare le persone e perdere utenti. Mantenerlo semplice e focalizzato: chiedere solo i dati strettamente necessari.

La progettazione dei moduli è un passaggio importante nella creazione di un sito o di un'applicazione.
Trattare l'esperienza utente dei moduli va oltre lo scopo di questo articolo, ma per approfondire l'argomento è consigliabile leggere i seguenti articoli:

- Smashing Magazine contiene alcuni [buoni articoli sulla UX dei moduli](https://www.smashingmagazine.com/2018/08/ux-html5-mobile-form-part-1/), incluso il meno recente ma ancora rilevante articolo [Extensive Guide To Web Form Usability](https://www.smashingmagazine.com/2011/11/extensive-guide-web-form-usability/).
- Anche UXMatters è una risorsa molto accurata, con buoni consigli dalle [migliori pratiche di base](https://www.uxmatters.com/mt/archives/2012/05/7-basic-best-practices-for-buttons.php) a questioni complesse come i [moduli multipagina](https://www.uxmatters.com/mt/archives/2010/03/pagination-in-web-forms-evaluating-the-effectiveness-of-web-forms.php).

In questo articolo verrà creato un semplice modulo di contatto. Iniziamo con uno schizzo approssimativo.

![Il modulo da creare, schizzo approssimativo](form-sketch-low.jpg)

Il modulo conterrà tre campi di testo e un pulsante. Verranno richiesti all'utente il nome, l'email e il messaggio che desidera inviare. Premendo il pulsante, i dati verranno inviati a un server web.

## Implementare l'HTML del modulo

Bene, proviamo a creare l'HTML del modulo. Verranno usati i seguenti elementi HTML: {{HTMLelement("form")}}, {{HTMLelement("label")}}, {{HTMLelement("input")}}, {{HTMLelement("textarea")}} e {{HTMLelement("button")}}.

Prima di procedere, creare una copia locale del nostro [semplice template HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html): qui verrà inserito l'HTML del modulo.

### L'elemento `<form>`

Tutti i moduli iniziano con un elemento {{HTMLelement("form")}}, come questo:

```html
<form action="/my-handling-form-page" method="post">…</form>
```

Questo elemento definisce formalmente un modulo. È un elemento contenitore come un elemento {{HTMLelement("section")}} o {{HTMLelement("footer")}}, ma specificamente destinato a contenere moduli; supporta inoltre alcuni attributi specifici per configurare il comportamento del modulo. Tutti i suoi attributi sono facoltativi, ma la pratica standard consiste nell'impostare sempre almeno gli attributi [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method):

- L'attributo `action` definisce la posizione (URL) a cui devono essere inviati i dati raccolti dal modulo quando viene inviato.
- L'attributo `method` definisce il metodo HTTP con cui inviare i dati (solitamente `get` o `post`).

> [!NOTE]
> Il funzionamento di questi attributi verrà esaminato più avanti nel nostro articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data).

Per ora, aggiungere l'elemento {{htmlelement("form")}} precedente nel proprio {{htmlelement("body")}} HTML.

### Gli elementi `<label>`, `<input>` e `<textarea>`

Il modulo di contatto non è complesso: la parte di immissione dei dati contiene tre campi di testo, ciascuno con un corrispondente {{HTMLelement("label")}}:

- Il campo di input per il nome è un {{HTMLelement("input/text", "campo di testo a riga singola")}}.
- Il campo di input per l'email è un {{HTMLelement("input/email", "input di tipo email")}}: un campo di testo a riga singola che accetta solo indirizzi email.
- Il campo di input per il messaggio è un {{HTMLelement("textarea")}}; un campo di testo multilinea.

In termini di codice HTML, per implementare questi widget del modulo serve qualcosa di simile al seguente:

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

Aggiornare il codice del modulo affinché corrisponda a quello precedente.

Gli elementi {{HTMLelement("p")}} servono a strutturare comodamente il codice e a semplificare lo stile (vedere più avanti nell'articolo).
Per usabilità e accessibilità, viene inclusa un'etichetta esplicita per ogni controllo del modulo.
Notare l'uso dell'attributo [`for`](/it/docs/Web/HTML/Reference/Attributes/for) su tutti gli elementi {{HTMLelement("label")}}, il cui valore corrisponde all'[`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) del controllo del modulo a cui è associato: è così che si associa un controllo del modulo alla sua etichetta.

Questo offre un grande vantaggio: associa l'etichetta al controllo del modulo, consentendo agli utenti di mouse, trackpad e dispositivi touch di fare clic sull'etichetta per attivare il controllo corrispondente e fornisce inoltre un nome accessibile che gli screen reader possono leggere ai propri utenti.
Ulteriori dettagli sulle etichette dei moduli sono disponibili in [Come strutturare un modulo web](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form).

Nell'elemento {{HTMLelement("input")}}, l'attributo più importante è `type`.
Questo attributo è estremamente importante perché definisce il modo in cui l'elemento {{HTMLelement("input")}} appare e si comporta.
Maggiori informazioni sono disponibili nell'articolo [Controlli nativi di base per moduli](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls), più avanti.

- Nel nostro semplice esempio, viene usato il valore {{HTMLelement("input/text", "text")}} per il primo input, ovvero il valore predefinito di questo attributo.
  Rappresenta un campo di testo a riga singola di base che accetta qualsiasi tipo di input testuale.
- Per il secondo input, viene usato il valore {{HTMLelement("input/email", "email")}}, che definisce un campo di testo a riga singola che accetta solo un indirizzo email ben formato.
  Questo trasforma un campo di testo di base in una sorta di campo "intelligente" che esegue alcuni controlli di convalida sui dati digitati dall'utente.
  Inoltre, sui dispositivi con tastiere dinamiche, come gli smartphone, fa apparire un layout di tastiera più appropriato per l'immissione di indirizzi email (ad esempio, con il simbolo @ disponibile per impostazione predefinita).
  Maggiori informazioni sulla convalida dei moduli sono disponibili più avanti nell'articolo [convalida dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

Infine, notare la sintassi di `<input>` rispetto a `<textarea></textarea>`.
Questa è una delle particolarità di HTML.
Il tag `<input>` è un {{Glossary("void_element", "elemento void")}}, il che significa che non necessita di un tag di chiusura.
{{HTMLElement("textarea")}} non è un elemento void, il che significa che deve essere chiuso con il tag finale appropriato.
Ciò ha un impatto su una funzionalità specifica dei moduli: il modo in cui viene definito il valore predefinito.
Per definire il valore predefinito di un elemento {{HTMLElement("input")}}, occorre usare l'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/input#value) in questo modo:

```html
<input type="text" value="by default this element is filled with this text" />
```

Al contrario, per definire un valore predefinito per un {{HTMLElement("textarea")}}, inserirlo tra il tag di apertura e quello di chiusura dell'elemento {{HTMLElement("textarea")}}, in questo modo:

```html
<textarea>
by default this element is filled with this text
</textarea>
```

### L'elemento `<button>`

Il markup del modulo è quasi completo; occorre solo aggiungere un pulsante che consenta all'utente di inviare, o "sottomettere", i dati dopo aver compilato il modulo.
Ciò avviene usando l'elemento {{HTMLelement("button")}}; aggiungere quanto segue appena sopra il tag di chiusura `</form>`:

```html
<p class="button">
  <button type="submit">Send your message</button>
</p>
```

L'elemento {{htmlelement("button")}} accetta anche un attributo `type`, che può assumere uno di tre valori: `submit`, `reset` o `button`.

- Un clic su un pulsante `submit` (il valore predefinito) invia i dati del modulo alla pagina web definita dall'attributo `action` dell'elemento {{HTMLelement("form")}}.
- Un clic su un pulsante `reset` ripristina immediatamente tutti i widget del modulo al loro valore predefinito. Dal punto di vista UX, questa è considerata una cattiva pratica, pertanto è consigliabile evitare di usare questo tipo di pulsante a meno che non vi sia davvero una buona ragione per includerlo.
- Un clic su un pulsante `button` non fa _nulla_! Può sembrare sciocco, ma è sorprendentemente utile per creare pulsanti personalizzati: è possibile definirne la funzionalità scelta con JavaScript.

> [!NOTE]
> È possibile usare anche l'elemento {{HTMLElement("input")}} con il `type` corrispondente per produrre un pulsante, ad esempio `<input type="submit">`. Il vantaggio principale dell'elemento {{HTMLelement("button")}} è che l'elemento {{HTMLelement("input")}} consente solo testo semplice nella propria etichetta, mentre l'elemento {{HTMLelement("button")}} consente contenuto HTML completo, permettendo contenuti dei pulsanti più complessi e creativi.

## Stile di base del modulo

Dopo aver terminato di scrivere il codice HTML del modulo, provare a salvarlo e visualizzarlo in un browser. Al momento, l'aspetto sarà piuttosto brutto.

> [!NOTE]
> Se il codice HTML non sembra corretto, provare a confrontarlo con il nostro esempio completato: vedere [first-form.html](https://github.com/mdn/learning-area/blob/main/html/forms/your-first-HTML-form/first-form.html) ([visualizzarlo anche in esecuzione](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form.html)).

I moduli sono notoriamente difficili da stilizzare in modo gradevole. Insegnare dettagliatamente lo stile dei moduli va oltre lo scopo di questo articolo, quindi per il momento verrà aggiunto solo un po' di CSS per renderne l'aspetto accettabile.

Prima di tutto, aggiungere un elemento {{htmlelement("style")}} alla pagina, all'interno dell'head HTML. Dovrebbe apparire così:

```html
<style>
  /* CSS goes here */
</style>
```

All'interno dei tag `style`, aggiungere il seguente CSS:

```css
body {
  /* Center the form on the page */
  text-align: center;
}

form {
  display: inline-block;
  /* Form outline */
  padding: 1em;
  border: 1px solid #cccccc;
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
  border: 1px solid #999999;
}

input:focus,
textarea:focus {
  /* Set the outline width and style */
  outline-style: solid;
  /* To give a little highlight on active elements */
  outline-color: black;
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

Salvare e ricaricare: il modulo dovrebbe apparire molto meno brutto.

> [!NOTE]
> La nostra versione è disponibile su GitHub in [first-form-styled.html](https://github.com/mdn/learning-area/blob/main/html/forms/your-first-HTML-form/first-form-styled.html) ([visualizzarla anche in esecuzione](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form-styled.html)).

## Inviare i dati del modulo al server web

L'ultima parte, e forse la più complessa, è gestire i dati del modulo lato server.
L'elemento {{HTMLelement("form")}} definisce dove e come inviare i dati grazie agli attributi [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method).

Viene fornito un attributo `name` per ogni controllo del modulo.
I nomi sono importanti sia lato client sia lato server: indicano al browser quale nome assegnare a ciascun dato e, lato server, consentono al server di gestire ciascun dato in base al nome.
I dati del modulo vengono inviati al server come coppie nome/valore.

Per assegnare un nome ai dati in un modulo, occorre usare l'attributo `name` su ogni widget del modulo che raccoglierà uno specifico dato.
Esaminiamo di nuovo parte del codice del modulo:

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

Nel nostro esempio, il modulo invierà 3 dati denominati `user_name`, `user_email` e `user_message`.
Questi dati saranno inviati all'URL `/my-handling-form-page` usando il metodo [HTTP `POST`](/it/docs/Web/HTTP/Reference/Methods/POST).

Lato server, lo script all'URL `/my-handling-form-page` riceverà i dati come un elenco di 3 elementi chiave/valore contenuti nella richiesta HTTP.
Il modo in cui questo script gestirà tali dati dipende dallo sviluppatore.
Ogni linguaggio lato server (PHP, Python, Ruby, Java, C# e così via) dispone di un proprio meccanismo per gestire i dati dei moduli.
Approfondire questo argomento va oltre lo scopo di questa esercitazione, ma alcuni esempi sono disponibili più avanti nel nostro articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data).

## Riepilogo

Congratulazioni, è stato creato il primo modulo web. In esecuzione appare così:

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
  border: 1px solid #cccccc;
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
  border: 1px solid #999999;
}

input:focus,
textarea:focus {
  /* To give a little highlight on active elements */
  border-color: black;
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

Tuttavia, questo è solo l'inizio: ora è il momento di approfondire. I moduli sono molto più potenti di quanto visto qui e gli altri articoli di questo modulo aiuteranno a padroneggiare il resto.

{{NextMenu("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms")}}
