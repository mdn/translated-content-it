---
title: Come strutturare un modulo web
slug: Learn_web_development/Extensions/Forms/How_to_structure_a_web_form
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms")}}

Con le basi fuori dai piedi, ora esamineremo più dettagliatamente gli elementi utilizzati per fornire struttura e significato alle diverse parti di un modulo.

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
        Comprendere come strutturare i moduli HTML e dare loro semantica affinché siano utilizzabili e accessibili.
      </td>
    </tr>
  </tbody>
</table>

La flessibilità dei moduli li rende una delle strutture più complesse in [HTML](/it/docs/Learn_web_development/Core/Structuring_content); si può costruire qualsiasi tipo di modulo di base utilizzando elementi e attributi dedicati. Utilizzare la corretta struttura quando si crea un modulo HTML aiuta a garantire che il modulo sia sia utilizzabile sia [accessibile](/it/docs/Learn_web_development/Core/Accessibility).

## L'elemento \<form>

L'elemento {{HTMLElement("form")}} definisce formalmente un modulo e gli attributi che ne determinano il comportamento. Ogni volta che si desidera creare un modulo HTML, è necessario iniziarlo utilizzando questo elemento, annidando tutti i contenuti al suo interno. Molte tecnologie assistive e plugin del browser possono scoprire gli elementi {{HTMLElement("form")}} e implementare collegamenti speciali per renderli più facili da usare.

Abbiamo già incontrato questo nell'articolo precedente. 

> [!WARNING]
> È strettamente proibito annidare un modulo all'interno di un altro modulo. L'annidamento può causare un comportamento imprevedibile nei moduli, quindi è una cattiva idea.

È sempre possibile utilizzare un controllo modulo al di fuori di un elemento {{HTMLElement("form")}}. Se lo si fa, per impostazione predefinita, quel controllo non ha nulla a che fare con alcun modulo a meno che non sia associato a un modulo tramite il suo attributo [`form`](/it/docs/Web/HTML/Reference/Elements/input#form). Questo è stato introdotto per consentire di collegare esplicitamente un controllo con un modulo anche se non è annidato al suo interno.

Procediamo a coprire gli elementi strutturali che troverà annidati in un modulo.

## Gli elementi \<fieldset> e \<legend>

L'elemento {{HTMLElement("fieldset")}} è un modo conveniente per creare gruppi di widget che condividono lo stesso scopo, per scopi di stile e semantici. Si può etichettare un {{HTMLElement("fieldset")}} includendo un elemento {{HTMLElement("legend")}} subito sotto il tag di apertura {{HTMLElement("fieldset")}}. Il contenuto testuale del {{HTMLElement("legend")}} descrive formalmente lo scopo del {{HTMLElement("fieldset")}} in cui è incluso.

Molte tecnologie assistive utilizzeranno l'elemento {{HTMLElement("legend")}} come se fosse una parte dell'etichetta di ciascun controllo all'interno dell'elemento {{HTMLElement("fieldset")}} corrispondente. Ad esempio, alcuni screen reader come [Jaws](https://www.freedomscientific.com/products/software/jaws/) e [NVDA](https://www.nvaccess.org/) pronunceranno il contenuto della legenda prima di pronunciare l'etichetta di ciascun controllo.

Ecco un piccolo esempio:

```html
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

> [!NOTE]
> Può trovare questo esempio in [fieldset-legend.html](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/fieldset-legend.html) ([vivi anche l'esperienza](https://mdn.github.io/learning-area/html/forms/html-form-structure/fieldset-legend.html)).

Quando si legge il modulo sopra, uno screen reader pronuncerà "Fruit juice size small" per il primo widget, "Fruit juice size medium" per il secondo, e "Fruit juice size large" per il terzo.

Il caso d'uso in questo esempio è uno dei più importanti. Ogni volta che si ha un set di pulsanti radio, si dovrebbe annidarli all'interno di un elemento {{HTMLElement("fieldset")}}. Ci sono altri casi d'uso, e in generale si può anche usare l'elemento {{HTMLElement("fieldset")}} per sezionare un modulo. Idealmente, moduli lunghi dovrebbero essere distribuiti su più pagine, ma se un modulo sta diventando lungo e deve restare su una singola pagina, mettere le diverse sezioni correlate all'interno di diversi fieldset migliora l'usabilità.

A causa della sua influenza sulle tecnologie assistive, l'elemento {{HTMLElement("fieldset")}} è uno degli elementi chiave per costruire moduli accessibili; tuttavia, è sua responsabilità non abusarne. Se possibile, ogni volta che si costruisce un modulo provi a [ascoltare come uno screen reader](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) lo interpreta. Se suona strano, provi a migliorare la struttura del modulo.

## L'elemento \<label>

Come abbiamo visto nell'articolo precedente, l'elemento {{HTMLElement("label")}} è il modo formale per definire un'etichetta per un widget di modulo HTML. Questo è l'elemento più importante se desidera costruire moduli accessibili — quando viene implementato correttamente, gli screen reader pronunceranno l'etichetta di un elemento di modulo insieme a eventuali istruzioni correlate, oltre ad essere utile per gli utenti vedenti. Prenda questo esempio, che abbiamo visto nell'articolo precedente:

```html
<label for="name">Name:</label> <input type="text" id="name" name="user_name" />
```

Con il `<label>` associato correttamente con l'`<input>` tramite il suo attributo `for` (che contiene l'attributo `id` dell'elemento `<input>`), uno screen reader leggerà qualcosa come "Name, edit text".

C'è un altro modo per associare un controllo modulo con un'etichetta — annidare il controllo modulo all'interno del `<label>`, associandolo implicitamente.

```html
<label for="name">
  Name: <input type="text" id="name" name="user_name" />
</label>
```

Anche in tali casi, tuttavia, è considerata una buona pratica impostare l'attributo `for` per garantire che tutte le tecnologie assistive comprendano la relazione tra etichetta e widget.

Se non c'è etichetta, oppure se il controllo modulo non è né implicitamente né esplicitamente associato a un'etichetta, uno screen reader leggerà qualcosa come "Edit text blank", che non è assolutamente d'aiuto.

### Le etichette sono cliccabili, anche!

Un altro vantaggio delle etichette impostate correttamente è che si può cliccare o toccare l'etichetta per attivare il widget corrispondente. Questo è utile per controlli come i campi di testo, dove si può cliccare l'etichetta oltre che l'input per metterlo a fuoco, ma è particolarmente utile per i pulsanti radio e le caselle di controllo — l'area di un tale controllo può essere molto piccola, quindi è utile renderla il più facile da attivare possibile.

Ad esempio, cliccando sul testo dell'etichetta "Mi piace cherry" nell'esempio qui sotto si attiverà lo stato selezionato della casella di controllo _taste_cherry_:

```html
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

> [!NOTE]
> Può trovare questo esempio in [checkbox-label.html](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/checkbox-label.html) ([vivi anche l'esperienza](https://mdn.github.io/learning-area/html/forms/html-form-structure/checkbox-label.html)).

### Etichette multiple

Strettamente parlando, si possono mettere più etichette su un singolo widget, ma non è una buona idea poiché alcune tecnologie assistive possono avere difficoltà a gestirle. Nel caso di etichette multiple, dovrebbe annidare un widget e le sue etichette all'interno di un singolo elemento {{htmlelement("label")}}.

Consideriamo questo esempio:

```html
<p>Required fields are followed by <span aria-label="required">*</span>.</p>

<!-- So this: -->
<!--div>
  <label for="username">Name:</label>
  <input id="username" type="text" name="username" required>
  <label for="username"><span aria-label="required">*</label>
</div-->

<!-- would be better done like this: -->
<!--div>
  <label for="username">
    <span>Name:</span>
    <input id="username" type="text" name="username" required>
    <span aria-label="required">*</span>
  </label>
</div-->

<!-- But this is probably best: -->
<div>
  <label for="username">Name: <span aria-label="required">*</span></label>
  <input id="username" type="text" name="username" required />
</div>
```

{{EmbedLiveSample("Multiple_labels", 120, 120)}}

Il paragrafo in alto dichiara una regola per gli elementi richiesti. La regola deve essere inclusa _prima_ che venga utilizzata in modo che gli utenti vedenti e gli utenti di tecnologie assistive come gli screen reader possano imparare cosa significa prima di incontrare un elemento richiesto. Anche se questo aiuta a informare gli utenti su cosa significhi un asterisco, non ci si può fare affidamento. Uno screen reader pronuncerà un asterisco come "_star_" quando incontrato. Quando viene sorvolato da un mouse vedente, dovrebbe apparire "_required_", ciò è ottenuto mediante l'uso dell'attributo `title`. La lettura dei titoli ad alta voce dipende dalle impostazioni dello screen reader, quindi è più affidabile includere anche l'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label), che è sempre letto dagli screen reader.

Le varianti sopra aumentano in efficacia man mano che si procede:

- Nel primo esempio, l'etichetta non è letta affatto con l'input — si ottiene solo "edit text blank", più le etichette effettive sono lette separatamente. Le etichette multiple `<label>` confondono lo screen reader.
- Nel secondo esempio, le cose sono un po' più chiare — l'etichetta letta con l'input è "name star name edit text required", e le etichette sono ancora lette separatamente. Le cose sono ancora un po' confuse, ma è un po' meglio questa volta perché l'`<input>` ha un'etichetta associata.
- Il terzo esempio è il migliore — l'etichetta effettiva è letta tutta insieme, e l'etichetta letta con l'input è "name required edit text".

> [!NOTE]
> Potrebbe ottenere risultati leggermente diversi, a seconda del suo screen reader. Questo è stato testato in VoiceOver (e NVDA si comporta in modo simile). Ci piacerebbe sentire anche le sue esperienze.

> [!NOTE]
> Può trovare questo esempio su GitHub come [required-labels.html](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/required-labels.html) ([vivi anche l'esperienza](https://mdn.github.io/learning-area/html/forms/html-form-structure/required-labels.html)). Non testare l'esempio con 2 o 3 delle versioni commentate — gli screen reader si confonderanno sicuramente se ha più etichette E più input con lo stesso ID!

## Strutture HTML comuni usate con i moduli

Oltre alle strutture specifiche per i moduli web, è bene ricordare che il markup dei moduli è solo HTML. Ciò significa che si può usare tutto il potere dell'HTML per strutturare un modulo web.

Come può vedere negli esempi, è pratica comune racchiudere un'etichetta e il suo widget con un elemento {{HTMLElement("li")}} all'interno di una lista {{HTMLElement("ul")}} o {{HTMLElement("ol")}}. Anche gli elementi {{HTMLElement("p")}} e {{HTMLElement("div")}} sono comunemente usati. Le liste sono consigliate per strutturare più checkbox o pulsanti radio.

Oltre all'elemento {{HTMLElement("fieldset")}}, è anche pratica comune usare i titoli HTML (ad esempio, {{htmlelement("Heading_Elements", "h1")}}, {{htmlelement("Heading_Elements", "h2")}}) e sezioni (ad esempio, {{htmlelement("section")}}) per strutturare moduli complessi.

Soprattutto, spetta a lei trovare uno stile di codifica confortevole che produca moduli accessibili e utilizzabili. Ogni sezione separata di funzionalità dovrebbe essere contenuta in un elemento {{htmlelement("section")}} separato, con elementi {{htmlelement("fieldset")}} per contenere i pulsanti radio.

### Apprendimento attivo: costruzione della struttura di un modulo

Mettiamo in pratica queste idee e costruiamo un modulo leggermente più coinvolto — un modulo di pagamento. Questo modulo conterrà un numero di tipi di controllo che potrebbe non comprendere ancora. Non si preoccupi di questo per ora; scoprirà come funzionano nel prossimo articolo ([Controlli di modulo nativi di base](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls)). Per ora, legga attentamente le descrizioni mentre segue le istruzioni qui sotto, e inizia a formarsi un'idea di quali elementi di contenitore stiamo usando per strutturare il modulo, e perché.

1. Per iniziare, faccia una copia locale del nostro [file modello vuoto](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) in una nuova directory sul suo computer.

2. Successivamente, crei il suo modulo aggiungendo un elemento {{htmlelement("form")}}:

   ```html-nolint
   <form>
   ```

3. All'interno dell'elemento `<form>`, aggiunga un'intestazione e un paragrafo per informare gli utenti su come sono contrassegnati i campi obbligatori:

   ```html-nolint
   <h1>Payment form</h1>
   <p>
     Required fields are followed by
     <strong><span aria-label="required">*</span></strong>.
   </p>
   ```

4. Successivamente, aggiungeremo una sezione di codice più grande nel modulo, sotto il nostro inserimento precedente. Qui vedrà che stiamo racchiudendo i campi delle informazioni di contatto all'interno di un elemento {{htmlelement("section")}} distinto. Inoltre, abbiamo un insieme di tre pulsanti radio, ciascuno dei quali stiamo mettendo all'interno del proprio elemento di lista ({{htmlelement("li")}}). Abbiamo anche due {{htmlelement("input")}} testuali standard e i loro elementi {{htmlelement("label")}} associati, ciascuno contenuto all'interno di un {{htmlelement("p")}}, e un input per password per inserire una password. Aggiunga questo codice al suo modulo:

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
       <label for="name">
         <span>Name: </span>
         <strong><span aria-label="required">*</span></strong>
       </label>
       <input type="text" id="name" name="username" required />
     </p>
     <p>
       <label for="mail">
         <span>Email: </span>
         <strong><span aria-label="required">*</span></strong>
       </label>
       <input type="email" id="mail" name="user-mail" required />
     </p>
     <p>
       <label for="pwd">
         <span>Password: </span>
         <strong><span aria-label="required">*</span></strong>
       </label>
       <input type="password" id="pwd" name="password" required />
     </p>
   </section>
   ```

5. La seconda `<section>` del nostro modulo è l'informazione di pagamento.
   Abbiamo tre controlli distinti insieme alle loro etichette, ciascuno contenuto all'interno di un `<p>`.
   Il primo è un menu a discesa ({{htmlelement("select")}}) per selezionare il tipo di carta di credito.
   Il secondo è un elemento `<input>` di tipo `tel`, per inserire un numero di carta di credito; mentre avremmo potuto utilizzare il tipo `number`, non vogliamo l'UI dello spinner del numero.
   L'ultimo è un elemento `<input>` di tipo `text`, per inserire la data di scadenza della carta; questo include un attributo _placeholder_ che indica il formato corretto, e un _pattern_ che verifica che la data inserita abbia il formato corretto.
   Questi nuovi tipi di input sono reintrodotti in [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

   Inserisca quanto segue sotto la sezione precedente:

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
       <label for="number">
         <span>Card number:</span>
         <strong><span aria-label="required">*</span></strong>
       </label>
       <input type="tel" id="number" name="card-number" required />
     </p>
     <p>
       <label for="expiration">
         <span>Expiration date:</span>
         <strong><span aria-label="required">*</span></strong>
       </label>
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

6. L'ultima sezione che aggiungeremo è molto più semplice, contenendo solo un {{htmlelement("button")}} di tipo `submit`, per inviare i dati del modulo. Aggiunga questo ora alla fine del suo modulo:

   ```html
   <section>
     <p>
       <button type="submit">Validate the payment</button>
     </p>
   </section>
   ```

7. Infine, completi il suo modulo aggiungendo il tag di chiusura esterno {{htmlelement("form")}}:

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
     border: 1px solid #ccc;
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
     border: 1px solid #999;
   }

   input[type="checkbox"],
   input[type="radio"] {
     width: auto;
     border: none;
   }

   input:focus,
   textarea:focus {
     border-color: #000;
   }

   textarea {
     vertical-align: top;
     height: 5em;
     resize: vertical;
   }

   fieldset {
     width: 250px;
     box-sizing: border-box;
     border: 1px solid #999;
   }

   button {
     margin: 20px 0 0 0;
   }

   label {
     display: inline-block;
   }

   p label {
     width: 100%;
   }
   ```

Abbiamo applicato alcuni CSS extra al modulo finale qui sotto. Se desidera apportare modifiche all'aspetto del suo modulo, può copiare stili dall'[esempio](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form/Example) o visitare [Stilizzare i moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

{{EmbedLiveSample("active_learning_building_a_form_structure","100%",620)}}

## Testi le sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare un ulteriore test per verificare che abbia mantenuto queste informazioni prima di procedere — veda [Verifica le tue abilità: Struttura del modulo](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Form_structure).

## Sommario

Ora ha tutte le conoscenze di cui ha bisogno per strutturare correttamente i suoi moduli web. Copriremo molte delle funzionalità introdotte qui nei prossimi articoli, con il prossimo articolo che esaminerà in modo più dettagliato l'uso di tutti i diversi tipi di widget di modulo che vorrà utilizzare per raccogliere informazioni dagli utenti.

## Veda anche

- [A List Apart: _Sensible Forms: A Form Usability Checklist_](https://alistapart.com/article/sensibleforms/)

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Your_first_form", "Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms")}}
