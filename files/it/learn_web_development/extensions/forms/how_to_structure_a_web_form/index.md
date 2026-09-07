---
title: Come strutturare un modulo web
slug: Learn_web_development/Extensions/Forms/How_to_structure_a_web_form
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms")}}

Dopo aver trattato le basi, verranno ora esaminati più nel dettaglio gli elementi utilizzati per fornire struttura e significato alle diverse parti di un modulo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come strutturare i moduli HTML e attribuire loro semantica affinché siano utilizzabili e accessibili.
      </td>
    </tr>
  </tbody>
</table>

La flessibilità dei moduli li rende una delle strutture più complesse in [HTML](/it/docs/Learn_web_development/Core/Structuring_content); è possibile creare qualsiasi tipo di modulo di base utilizzando elementi e attributi dedicati ai moduli. Utilizzare la struttura corretta durante la creazione di un modulo HTML contribuirà a garantire che il modulo sia sia utilizzabile sia [accessibile](/it/docs/Learn_web_development/Core/Accessibility).

## L'elemento \<form>

L'elemento {{HTMLElement("form")}} definisce formalmente un modulo e gli attributi che ne determinano il comportamento. Ogni volta che si desidera creare un modulo HTML, occorre iniziare utilizzando questo elemento, annidando al suo interno tutto il contenuto. Molte tecnologie assistive e plugin del browser possono rilevare gli elementi {{HTMLElement("form")}} e implementare hook speciali per renderli più semplici da usare.

Questo elemento è già stato introdotto nell'articolo precedente.

> [!WARNING]
> È severamente vietato annidare un modulo all'interno di un altro modulo. L'annidamento può causare comportamenti imprevedibili dei moduli, pertanto è una cattiva idea.

È sempre possibile utilizzare un controllo del modulo al di fuori di un elemento {{HTMLElement("form")}}. In tal caso, per impostazione predefinita quel controllo non è associato ad alcun modulo, a meno che non venga associato a un modulo mediante il relativo attributo [`form`](/it/docs/Web/HTML/Reference/Attributes/form). Questa funzionalità è stata introdotta per consentire di associare esplicitamente un controllo a un modulo anche se non è annidato al suo interno.

Si prosegue ora con gli elementi strutturali che possono essere annidati in un modulo.

## Gli elementi `<fieldset>` e `<legend>`

L'elemento {{HTMLElement("fieldset")}} è un modo pratico per creare gruppi di widget con lo stesso scopo, a fini di stile e semantica. È possibile etichettare un {{HTMLElement("fieldset")}} includendo un elemento {{HTMLElement("legend")}} subito dopo il tag di apertura {{HTMLElement("fieldset")}}. Il contenuto testuale dell'elemento {{HTMLElement("legend")}} descrive formalmente lo scopo del {{HTMLElement("fieldset")}} al cui interno è incluso.

Molte tecnologie assistive utilizzano l'elemento {{HTMLElement("legend")}} come se facesse parte dell'etichetta di ciascun controllo all'interno del corrispondente elemento {{HTMLElement("fieldset")}}. Ad esempio, alcuni screen reader come [Jaws](https://vispero.com/jaws-screen-reader-software/) e [NVDA](https://www.nvaccess.org/) pronunciano il contenuto della legenda prima di pronunciare l'etichetta di ciascun controllo.

Ecco un esempio:

```html live-sample___fieldset-legend
<form>
  <fieldset>
    <legend>Fruit juice size</legend>
    <p>
      <input type="radio" name="size" id="size_1" value="small" />
      <label for="size_1">Small</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_2" value="medium" />
      <label for="size_2">Medium</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_3" value="large" />
      <label for="size_3">Large</label>
    </p>
  </fieldset>
</form>
```

Il risultato del rendering è il seguente:

{{embedlivesample("fieldset-legend", "100%", 200)}}

Durante la lettura del modulo precedente, uno screen reader pronuncerà "Dimensione del succo di frutta piccolo" per l'etichetta del primo pulsante radio, "Dimensione del succo di frutta medio" per il secondo e "Dimensione del succo di frutta grande" per il terzo.

Ogni volta che è presente un insieme di pulsanti radio, occorre annidarli all'interno di un elemento {{HTMLElement("fieldset")}}. Esistono anche altri casi d'uso e, in generale, l'elemento {{HTMLElement("fieldset")}} può essere usato anche per suddividere un modulo in sezioni. Idealmente, i moduli lunghi dovrebbero essere distribuiti su più pagine, ma se un modulo diventa lungo e deve trovarsi in una sola pagina, inserire le diverse sezioni correlate in fieldset distinti migliora l'usabilità.

A causa della sua influenza sulle tecnologie assistive, l'elemento {{HTMLElement("fieldset")}} è uno degli elementi fondamentali per creare moduli accessibili; tuttavia, è responsabilità dello sviluppatore non abusarne. Se possibile, ogni volta che si crea un modulo, è opportuno [ascoltare come uno screen reader](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) lo interpreta. Se il risultato sembra insolito, provare a migliorare la struttura del modulo.

## L'elemento \<label>

Come visto nell'articolo precedente, l'elemento {{HTMLElement("label")}} è il modo formale per definire un'etichetta per un widget di un modulo HTML. Questo è l'elemento più importante per creare moduli accessibili: se implementato correttamente, gli screen reader pronunceranno l'etichetta di un elemento del modulo insieme a tutte le istruzioni correlate, oltre a essere utile per gli utenti vedenti. Si consideri questo esempio, già visto nell'articolo precedente:

```html
<label for="name">Name:</label> <input type="text" id="name" name="user_name" />
```

Con `<label>` associato correttamente a `<input>` mediante il relativo attributo `for` (che contiene l'attributo `id` dell'elemento `<input>`), uno screen reader leggerà qualcosa come "Nome, modifica testo".

Esiste un altro modo per associare un controllo del modulo a un'etichetta: annidare il controllo del modulo all'interno di `<label>`, associandolo implicitamente.

```html
<label for="name">
  Name: <input type="text" id="name" name="user_name" />
</label>
```

Tuttavia, anche in questi casi è considerata una buona pratica impostare l'attributo `for`, per garantire che tutte le tecnologie assistive comprendano la relazione tra etichetta e widget.

Se non è presente un'etichetta, oppure se il controllo del modulo non è associato implicitamente né esplicitamente a un'etichetta, uno screen reader leggerà qualcosa come "Modifica testo vuoto", che non è affatto molto utile.

### Anche le etichette sono selezionabili

Un altro vantaggio delle etichette configurate correttamente è che è possibile fare clic o toccare l'etichetta per attivare il widget corrispondente. Questo è utile per controlli come gli input di testo, per i quali è possibile fare clic sia sull'etichetta sia sull'input per ricevere il focus, ma è particolarmente utile per pulsanti radio e checkbox: l'area attiva di questi controlli può essere molto piccola, quindi è utile renderne l'attivazione il più semplice possibile.

Ad esempio, facendo clic sul testo dell'etichetta "I like cherry" nell'esempio seguente verrà attivato o disattivato lo stato selezionato della checkbox _taste_cherry_:

```html live-sample___checkbox-label
<form>
  <p>
    <input type="checkbox" id="taste_1" name="taste_cherry" value="cherry" />
    <label for="taste_1">I like cherry</label>
  </p>
  <p>
    <input type="checkbox" id="taste_2" name="taste_banana" value="banana" />
    <label for="taste_2">I like banana</label>
  </p>
</form>
```

Provare:

{{embedlivesample("checkbox-label", "100%", 100)}}

### Etichette multiple

In senso stretto, è possibile inserire più etichette su un singolo widget, ma non è una buona idea poiché alcune tecnologie assistive possono avere difficoltà a gestirle. Nel caso di etichette multiple, occorre annidare un widget e le relative etichette all'interno di un singolo elemento {{htmlelement("label")}}.

Si consideri questo esempio:

```html
<p>Please complete all required (*) fields.</p>

<!-- So this: -->
<!--<div>
  <label for="username">Name:</label>
  <input id="username" type="text" name="username" required />
  <label for="username">*</label>
</div>-->

<!-- would be better done like this: -->
<!--<div>
  <label for="username">
    <span>Name:</span>
    <input id="username" type="text" name="username" required />
    <span>*</span>
  </label>
</div>-->

<!-- But this is probably best: -->
<div>
  <label for="username">Name *:</label>
  <input id="username" type="text" name="username" required />
</div>
```

{{EmbedLiveSample("Multiple_labels", 120, 120)}}

Il paragrafo in alto indica una regola per gli elementi obbligatori. La regola deve essere inclusa _prima_ di essere utilizzata, affinché gli utenti vedenti e gli utenti di tecnologie assistive (AT), come gli screen reader, possano comprenderne il significato prima di incontrare un elemento obbligatorio.

## Strutture HTML comuni utilizzate con i moduli

Oltre alle strutture specifiche dei moduli web, è bene ricordare che il markup dei moduli è semplicemente HTML. Ciò significa che è possibile utilizzare tutta la potenza di HTML per strutturare un modulo web.

Come mostrato negli esempi, è pratica comune racchiudere un'etichetta e il relativo widget in un elemento {{HTMLElement("li")}} all'interno di un elenco {{HTMLElement("ul")}} o {{HTMLElement("ol")}}. Vengono inoltre comunemente utilizzati gli elementi {{HTMLElement("p")}} e {{HTMLElement("div")}}. Gli elenchi sono consigliati per strutturare più checkbox o pulsanti radio.

Oltre all'elemento {{HTMLElement("fieldset")}}, è anche pratica comune utilizzare titoli HTML, ad esempio {{htmlelement("Heading_Elements", "h1")}} e {{htmlelement("Heading_Elements", "h2")}}, e sezioni, ad esempio {{htmlelement("section")}}, per strutturare moduli complessi.

Soprattutto, spetta allo sviluppatore trovare uno stile di codifica comodo che produca moduli accessibili e utilizzabili. Ogni sezione distinta di funzionalità dovrebbe essere contenuta in un elemento {{htmlelement("section")}} separato, con elementi {{htmlelement("fieldset")}} per contenere i pulsanti radio.

### Creare la struttura di un modulo

Mettiamo in pratica queste idee e creiamo un modulo leggermente più articolato: un modulo di pagamento. Questo modulo conterrà diversi tipi di controllo che potrebbero non essere ancora chiari. Non occorre preoccuparsene per ora; il loro funzionamento verrà illustrato nel prossimo articolo ([Controlli dei moduli nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls)). Per il momento, leggere attentamente le descrizioni mentre si seguono le istruzioni riportate di seguito e iniziare a comprendere quali elementi contenitore vengono utilizzati per strutturare il modulo e perché.

1. Per iniziare, creare una copia locale del [file modello vuoto](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) in una nuova directory sul computer.

2. Creare quindi il modulo aggiungendo un elemento {{htmlelement("form")}}:

   ```html-nolint
   <form>
   ```

3. All'interno dell'elemento `<form>`, aggiungere un'intestazione e un paragrafo per informare gli utenti su come vengono contrassegnati i campi obbligatori:

   ```html-nolint
   <h1>Payment form</h1>
   <p>Please complete all required (*) fields.</p>
   ```

4. Successivamente, verrà aggiunta al modulo una sezione di codice più ampia, sotto l'elemento precedente. Qui è possibile osservare che i campi delle informazioni di contatto vengono racchiusi in un elemento {{htmlelement("section")}} distinto. Inoltre, è presente un insieme di tre pulsanti radio, ognuno dei quali viene inserito nel proprio elemento di elenco ({{htmlelement("li")}}). Sono inoltre presenti due {{htmlelement("input")}} di testo standard con i relativi elementi {{htmlelement("label")}}, ciascuno contenuto in un {{htmlelement("p")}}, e un input password per inserire una password. Aggiungere questo codice al modulo:

   ```html
   <section>
     <h2>Contact information</h2>
     <fieldset>
       <legend>Title</legend>
       <ul>
         <li>
           <label for="title_1">
             <input type="radio" id="title_1" name="title" value="A" />
             Ace
           </label>
         </li>
         <li>
           <label for="title_2">
             <input type="radio" id="title_2" name="title" value="K" />
             King
           </label>
         </li>
         <li>
           <label for="title_3">
             <input type="radio" id="title_3" name="title" value="Q" />
             Queen
           </label>
         </li>
       </ul>
     </fieldset>
     <p>
       <label for="name">Name *:</label>
       <input type="text" id="name" name="username" required />
     </p>
     <p>
       <label for="mail">Email *:</label>
       <input type="email" id="mail" name="user-mail" required />
     </p>
     <p>
       <label for="pwd">Password *:</label>
       <input type="password" id="pwd" name="password" required />
     </p>
   </section>
   ```

5. La seconda `<section>` del modulo contiene le informazioni di pagamento.
   Sono presenti tre controlli distinti con le rispettive etichette, ciascuno contenuto in un `<p>`.
   Il primo è un menu a discesa ({{htmlelement("select")}}) per selezionare il tipo di carta di credito.
   Il secondo è un elemento `<input>` di tipo `tel`, per inserire il numero della carta di credito; si sarebbe potuto usare il tipo `number`, ma non si desidera l'interfaccia con spinner del numero.
   L'ultimo è un elemento `<input>` di tipo `text`, per inserire la data di scadenza della carta; include un attributo _placeholder_ che indica il formato corretto e un _pattern_ che verifica che la data immessa abbia il formato corretto.
   Questi tipi di input più recenti vengono reintrodotti in [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

   Inserire quanto segue sotto la sezione precedente:

   ```html
   <section>
     <h2>Payment information</h2>
     <p>
       <label for="card">
         <span>Card type:</span>
       </label>
       <select id="card" name="user-card">
         <option value="visa">Visa</option>
         <option value="mc">Mastercard</option>
         <option value="amex">American Express</option>
       </select>
     </p>
     <p>
       <label for="number">Card number *:</label>
       <input type="tel" id="number" name="card-number" required />
     </p>
     <p>
       <label for="expiration">Expiration date *:</label>
       <input
         type="text"
         id="expiration"
         name="expiration"
         required
         placeholder="MM/YY"
         pattern="^(0[1-9]|1[0-2])\/([0-9]{2})$" />
     </p>
   </section>
   ```

6. L'ultima sezione da aggiungere è molto più semplice e contiene solo un {{htmlelement("button")}} di tipo `submit`, per inviare i dati del modulo. Aggiungere ora questo elemento in fondo al modulo:

   ```html
   <section>
     <p>
       <button type="submit">Validate the payment</button>
     </p>
   </section>
   ```

7. Infine, completare il modulo aggiungendo il tag di chiusura esterno {{htmlelement("form")}}:

   ```html
   </form>
   ```

   ```css hidden
   h1 {
     margin-top: 0;
   }

   ul {
     margin: 0;
     padding: 0;
     list-style: none;
   }

   form {
     margin: 0 auto;
     width: 400px;
     padding: 1em;
     border: 1px solid #cccccc;
     border-radius: 1em;
   }

   div + div {
     margin-top: 1em;
   }

   label span {
     display: inline-block;
     text-align: right;
   }

   input,
   textarea {
     font: 1em sans-serif;
     width: 250px;
     box-sizing: border-box;
     border: 1px solid #999999;
   }

   input[type="checkbox"],
   input[type="radio"] {
     width: auto;
     border: none;
   }

   input:focus,
   textarea:focus {
     border-color: black;
   }

   textarea {
     vertical-align: top;
     height: 5em;
     resize: vertical;
   }

   fieldset {
     width: 250px;
     box-sizing: border-box;
     border: 1px solid #999999;
   }

   button {
     margin-top: 20px;
   }

   label {
     display: inline-block;
   }

   p label {
     width: 100%;
   }
   ```

Al modulo completato riportato di seguito è stato applicato del CSS aggiuntivo. Per apportare modifiche all'aspetto del modulo, è possibile copiare gli stili dall'[esempio](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form/Example) oppure visitare [Applicare stili ai moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

{{EmbedLiveSample("building_a_form_structure","100%",620)}}

## Riepilogo

Ora sono disponibili tutte le conoscenze necessarie per strutturare correttamente i moduli web. Molte delle funzionalità introdotte qui verranno trattate nei prossimi articoli; il prossimo articolo esaminerà più nel dettaglio l'uso di tutti i diversi tipi di widget dei moduli necessari per raccogliere informazioni dagli utenti.

## Vedere anche

- [A List Apart: _Sensible Forms: A Form Usability Checklist_](https://alistapart.com/article/sensibleforms/)

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms")}}
