---
title: HTML images
short-title: Images
slug: Learn_web_development/Core/Structuring_content/HTML_images
l10n:
  sourceCommit: c99b4f2d0ea81c0e8822a749d218015c75995b5b
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}

All'inizio, il web era solo testo, ed era davvero piuttosto noioso. Fortunatamente, non passò molto tempo prima che fosse aggiunta la possibilità di incorporare immagini (e altri tipi di contenuto più interessanti) all'interno delle pagine web. In questo articolo esamineremo in dettaglio come utilizzare l'elemento {{htmlelement("img")}}, inclusi i concetti di base, annotandolo con didascalie utilizzando {{htmlelement("figure")}} e dettagliando come si relaziona alle immagini di sfondo in {{Glossary("CSS", "CSS")}}.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come spiegato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Il termine "elemento sostituito" — cosa significa?</li>
          <li>Sintassi base del tag <code>&lt;img&gt;</code></li>
          <li>Utilizzo di <code>src</code> per puntare a una risorsa.</li>
          <li>Utilizzo di <code>width</code> e <code>height</code>, ad esempio, per evitare aggiornamenti bruschi sgradevoli all'interfaccia utente una volta che un'immagine è stata caricata e visualizzata.</li>
          <li>Ottimizzazione delle risorse multimediali per il web — mantenere le dimensioni dei file ridotte.</li>
          <li>Comprendere il licensing dei media — diversi tipi di licenze, come conformarsi a esse e come cercare file multimediali con licenze appropriate da utilizzare nei progetti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Come inseriamo un'immagine su una pagina web?

Per inserire un'immagine in una pagina web, utilizziamo l'elemento {{htmlelement("img")}}. Questo è un {{Glossary("void_element", "elemento vuoto")}} (cioè, non può avere contenuto figlio e non può avere un tag di chiusura) che richiede due attributi per essere utile: `src` e `alt`. L'attributo `src` contiene un URL che punta all'immagine che si desidera incorporare nella pagina. Come con l'attributo `href` per gli elementi {{htmlelement("a")}}, l'attributo `src` può essere un URL relativo o assoluto. Senza un attributo `src`, un elemento `img` non ha immagine da caricare.

L'attributo [`alt` è descritto di seguito](#testo_alternativo).

> [!NOTE]
> Dovreste leggere [Un rapido primer su URL e percorsi](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#a_quick_primer_on_urls_and_paths) per rinfrescare la memoria su URL relativi e assoluti prima di continuare.

Ad esempio, se la tua immagine si chiama `dinosaur.jpg` e si trova nella stessa directory della tua pagina HTML, potresti incorporare l'immagine in questo modo:

```html
<img src="dinosaur.jpg" alt="Dinosaur" />
```

Se l'immagine fosse in una sottodirectory `images`, che si trova all'interno della stessa directory della pagina HTML, allora la incorporeresti così:

```html
<img src="images/dinosaur.jpg" alt="Dinosaur" />
```

E così via.

> [!NOTE]
> I motori di ricerca leggono anche i nomi file delle immagini e li considerano per la SEO. Pertanto, dovresti dare alla tua immagine un nome file descrittivo; `dinosaur.jpg` è meglio di `img835.png`.

Potresti anche incorporare l'immagine utilizzando il suo URL assoluto, ad esempio:

```html
<img src="https://www.example.com/images/dinosaur.jpg" alt="Dinosaur" />
```

Tuttavia, non è consigliato collegarsi tramite URL assoluti. Dovresti ospitare le immagini che vuoi utilizzare sul tuo sito, il che in configurazioni semplici significa mantenere le immagini per il tuo sito web sullo stesso server del tuo HTML. Inoltre, è più efficiente usare URL relativi piuttosto che URL assoluti in termini di manutenzione (quando sposti il tuo sito su un dominio diverso, non dovrai aggiornare tutti i tuoi URL per includere il nuovo dominio). In configurazioni più avanzate, potresti utilizzare un {{Glossary("CDN", "CDN (Content Delivery Network)")}} per distribuire le tue immagini.

Se non hai creato le immagini, dovresti assicurarti di avere il permesso di usarle alle condizioni della licenza sotto la quale sono pubblicate (vedi [Risorse multimediali e licensing](#risorse_multimediali_e_licensing) di seguito per maggiori informazioni).

> **Warning:** _Mai_ puntare l'attributo `src` a un'immagine ospitata sul sito web di qualcun altro _senza permesso_. Questo è chiamato "hotlinking". È considerato non etico, poiché qualcun altro pagherebbe i costi della larghezza di banda per fornire l'immagine quando qualcuno visita la tua pagina. Inoltre, ti lascia senza controllo sull'immagine che viene rimossa o sostituita con qualcosa di imbarazzante.

Il precedente frammento di codice, sia con l'URL assoluto che relativo, ci darà il seguente risultato:

![Un'immagine base di un dinosauro, incorporata in un browser, con sopra scritto "Immagini in HTML"](basic-image.png)

> [!NOTE]
> Elementi come {{htmlelement("img")}} e {{htmlelement("video")}} sono talvolta chiamati **elementi sostituiti**. Questo perché il contenuto e la dimensione dell'elemento sono definiti da una risorsa esterna (come un file immagine o video), non dai contenuti dell'elemento stesso. Puoi leggere di più su di essi in {{Glossary("replaced_elements", "elementi sostituiti")}}.

> [!NOTE]
> Puoi trovare l'esempio completo di questa sezione [in esecuzione su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/images-in-html/index.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/images-in-html/index.html)).

### Testo alternativo

Il prossimo attributo che esamineremo è `alt`. Il suo valore dovrebbe essere una descrizione testuale dell'immagine, per l'uso in situazioni in cui l'immagine non può essere vista/visualizzata o impiega molto tempo a essere renderizzata a causa di una connessione internet lenta. Ad esempio, il nostro codice sopra potrebbe essere modificato così:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth" />
```

Il modo più semplice per testare il tuo testo `alt` è quello di scrivere intenzionalmente in modo errato il nome del tuo file. Se, ad esempio, il nome della nostra immagine fosse scritto `dinosooooor.jpg`, il browser non visualizzerebbe l'immagine e mostrerebbe invece il testo alternativo:

![Il titolo Immagini in HTML, ma questa volta l'immagine del dinosauro non è visualizzata e il testo alternativo è al suo posto.](alt-text.png)

Quindi, perché mai dovresti vedere o avere bisogno di testo alternativo? Può tornare utile per diversi motivi:

- L'utente è ipovedente e sta utilizzando un [lettore di schermo](https://en.wikipedia.org/wiki/Screen_reader) per leggere il web. Infatti, avere testo alternativo disponibile per descrivere le immagini è utile per quasi tutti gli utenti.
- Come descritto sopra, la scrittura del nome file o del percorso potrebbe essere sbagliata.
- Il browser non supporta il tipo di immagine. Alcune persone utilizzano ancora browser solo testo, come [Lynx](https://en.wikipedia.org/wiki/Lynx_%28web_browser%29), che visualizza il testo alternativo delle immagini.
- Potresti voler fornire testo ai motori di ricerca da utilizzare; ad esempio, i motori di ricerca possono abbinare il testo alternativo con le query di ricerca.
- Gli utenti hanno disattivato le immagini per ridurre il volume di trasferimento dati e le distrazioni. Questo è particolarmente comune sui telefoni cellulari e nei paesi in cui la larghezza di banda è limitata o costosa.

Cosa dovresti effettivamente scrivere dentro il tuo attributo `alt`? Dipende da _perché_ l'immagine è lì in primo luogo. In altre parole, cosa perdi se la tua immagine non viene visualizzata:

- **Decorazione.** Dovresti usare [immagini di sfondo CSS](#immagini_di_sfondo_css) per le immagini decorative, ma se devi usare HTML, aggiungi un `alt=""` vuoto. Se l'immagine non fa parte del contenuto, un lettore di schermo non dovrebbe perdere tempo a leggerla.
- **Contenuto.** Se la tua immagine fornisce informazioni significative, fornisci le stesse informazioni in un testo `alt` _breve_ – o ancora meglio, nel testo principale che tutti possono vedere. Non scrivere testo `alt` ridondante. Quanto sarebbe fastidioso per un utente che vede se tutti i paragrafi fossero scritti due volte nel contenuto principale? Se l'immagine è descritta adeguatamente dal corpo del testo principale, puoi semplicemente usare `alt=""`.
- **Link.** Se metti un'immagine all'interno di tag {{htmlelement("a")}}, per trasformare un'immagine in un collegamento, devi comunque fornire [testo di collegamento accessibile](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#use_clear_link_wording). In tali casi puoi, o scriverlo all'interno dello stesso elemento `<a>`, o all'interno dell'attributo `alt` dell'immagine – qualunque funzione meglio nel tuo caso.
- **Testo.** Non dovresti mettere il testo nelle immagini. Se il tuo titolo principale ha bisogno di un'ombra, ad esempio, usa [CSS](/it/docs/Web/CSS/text-shadow) invece di mettere il testo in un'immagine. Tuttavia, se _proprio non puoi evitare di farlo_, dovresti fornire il testo all'interno dell'attributo `alt`.

Essenzialmente, la chiave è fornire un'esperienza utilizzabile, anche quando le immagini non possono essere viste. Questo assicura che non manchi alcun contenuto a nessun utente. Prova a disattivare le immagini nel tuo browser e guarda come appaiono le cose. Ti renderai presto conto di quanto sia utile il testo alternativo se l'immagine non può essere vista.

> [!NOTE]
> Vedi la nostra guida ai [Testi Alternativi](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) e [Un albero decisionale per alt](https://www.w3.org/WAI/tutorials/images/decision-tree/) per imparare a usare un attributo `alt` per le immagini in varie situazioni.

### Larghezza e altezza

Puoi utilizzare gli attributi [`width`](/it/docs/Web/HTML/Reference/Elements/img#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/img#height) per specificare la larghezza e l'altezza della tua immagine. Sono dati come interi senza un'unità e rappresentano la larghezza e l'altezza dell'immagine in pixel.

Puoi trovare la larghezza e l'altezza della tua immagine in diversi modi. Ad esempio, su Mac puoi usare <kbd>Cmd</kbd> + <kbd>I</kbd> per ottenere le informazioni di visualizzazione per il file immagine. Tornando al nostro esempio, potremmo fare questo:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341" />
```

C'è un buon motivo per farlo. L'HTML per la tua pagina e l'immagine sono risorse separate, ottenute dal browser come richieste HTTP(S) separate. Non appena il browser ha ricevuto l'HTML, inizierà a visualizzarlo all'utente. Se le immagini non sono state ancora ricevute (e questo sarà spesso il caso, poiché le dimensioni dei file delle immagini sono spesso molto più grandi dei file HTML), il browser renderà solo l'HTML e aggiornerà la pagina con l'immagine non appena sarà ricevuta.

Ad esempio, supponiamo di avere del testo dopo l'immagine:

```html
<h1>Images in HTML</h1>

<img
  src="dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"
  title="A T-Rex on display in the Manchester University Museum" />
<blockquote>
  <p>
    But down there it would be dark now, and not the lovely lighted aquarium she
    imagined it to be during the daylight hours, eddying with schools of tiny,
    delicate animals floating and dancing slowly to their own serene currents
    and creating the look of a living painting. That was wrong, in any case. The
    ocean was different from an aquarium, which was an artificial environment.
    The ocean was a world. And a world is not art. Dorothy thought about the
    living things that moved in that world: large, ruthless and hungry. Like us
    up here.
  </p>
  <footer>- Rachel Ingalls, <cite>Mrs. Caliban</cite></footer>
</blockquote>
```

Non appena il browser scarica l'HTML, il browser inizierà a visualizzare la pagina.

Una volta caricata l'immagine, il browser aggiunge l'immagine alla pagina. Poiché l'immagine occupa spazio, il browser deve spostare il testo in basso nella pagina, per fare spazio all'immagine sopra:

![Confronto del layout della pagina mentre il browser sta caricando una pagina e quando ha finito, quando non è specificata alcuna dimensione per l'immagine.](no-size.png)

Spostare il testo in questo modo è estremamente distrattivo per gli utenti, soprattutto se hanno già iniziato a leggerlo.

Se specifichi la dimensione effettiva dell'immagine nel tuo HTML, utilizzando gli attributi `width` e `height`, allora il browser sa, prima di aver scaricato l'immagine, quanto spazio deve lasciare per essa.

Questo significa che quando l'immagine è stata scaricata, il browser non deve spostare il contenuto circostante.

![Confronto del layout della pagina mentre il browser sta caricando una pagina e quando ha finito, quando la dimensione dell'immagine è specificata.](size.png)

Per un ottimo articolo sulla storia di questa funzionalità, vedi [Impostare altezza e larghezza sulle immagini è di nuovo importante](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/).

> [!NOTE]
> Anche se, come abbiamo detto, è buona prassi specificare la dimensione _effettiva_ delle immagini utilizzando attributi HTML, non dovresti usarli per _ridimensionare_ le immagini.
>
> Se imposti una dimensione troppo grande per l'immagine, finirai per avere immagini che sembrano sgranate, sfocate o troppo piccole, e sprecare banda scaricando un'immagine che non soddisfa le esigenze dell'utente. L'immagine potrebbe anche finire per apparire distorta, se non mantieni il corretto {{Glossary("aspect_ratio", "rapporto d'aspetto")}}. Dovresti usare un editor di immagini per rendere la tua immagine della dimensione corretta prima di metterla sulla tua pagina web.
>
> Se hai bisogno di alterare la dimensione di un'immagine, dovresti usare [CSS](/it/docs/Learn_web_development/Core/Styling_basics) invece.

### Titoli delle immagini

Come [con i collegamenti](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#adding_supporting_information_with_the_title_attribute), puoi anche aggiungere attributi `title` alle immagini, per fornire ulteriori informazioni di supporto se necessario. Nel nostro esempio, potremmo fare questo:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341"
  title="A T-Rex on display in the Manchester University Museum" />
```

Questo ci dà un tooltip al passaggio del mouse, proprio come i titoli dei collegamenti:

![L'immagine del dinosauro, con un titolo del tooltip su di essa che legge Un T-Rex esposto al Manchester University Museum](image-with-title.png)

Tuttavia, non è consigliato — `title` ha una serie di problemi di accessibilità, principalmente basati sul fatto che il supporto del lettore di schermo è molto imprevedibile e la maggior parte dei browser non lo mostrerà a meno che non si passi con un mouse (quindi, ad esempio, nessun accesso per gli utenti della tastiera). Se sei interessato a maggiori informazioni su questo, leggi [Le prove e le tribolazioni dell'attributo Title](https://www.24a11y.com/2017/the-trials-and-tribulations-of-the-title-attribute/) di Scott O'Hara.

È meglio includere tali informazioni di supporto nel testo principale dell'articolo, piuttosto che attaccate all'immagine.

### Apprendimento attivo: incorporare un'immagine

Ora è il tuo turno di giocare! Questa sezione di apprendimento attivo ti farà passare a un esercizio di incorporamento. Ti viene fornito un tag di base {{htmlelement("img")}}; vorremmo che incorporassi l'immagine situata al seguente URL:

```url
https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg
```

In precedenza abbiamo detto di non mai fare hotlink alle immagini su altri server, ma questo è solo a scopo didattico, quindi ti perdoneremo questa volta.

Vorremmo anche che tu:

- Aggiungessi un po' di testo alternativo e verificassi che funzioni scrivendo male l'URL dell'immagine.
- Impostassi la `width` e `height` corrette dell'immagine (suggerimento: è larga 200px e alta 171px), quindi sperimentassi con altri valori per vedere quale effetto hanno.
- Impostassi un `title` sull'immagine.

Se fai un errore, puoi sempre reimpostarlo usando il pulsante _Reset_. Se sei davvero bloccato, premi il pulsante _Mostra soluzione_ per vedere una risposta:

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="min-height: 100px; width: 95%">
<img>
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
html {
  font-family: sans-serif;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

body {
  margin: 10px;
  background: #f5f9fa;
}
```

```js hidden
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const output = document.querySelector(".output");
const code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  output.innerHTML = textarea.value;
}

const htmlSolution =
  '<img src="https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg"\n alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"\n width="200"\n height="171"\n title="A T-Rex on display in the Manchester University Museum">';
let solutionEntry = htmlSolution;

reset.addEventListener("click", () => {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = htmlSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;

  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );
  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}

// Update the saved userCode every time the user updates the text area code

textarea.onkeyup = function () {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_embedding_an_image', 700, 350) }}

## Risorse multimediali e licensing

Le immagini (e altri tipi di risorse multimediali) che trovi sul web sono rilasciate sotto vari tipi di licenza. Prima di utilizzare un'immagine su un sito che stai costruendo, assicurati di possederla, avere il permesso di usarla o di rispettare le condizioni di licenza del proprietario.

### Compresione dei tipi di licenza

Diamo un'occhiata ad alcune comuni categorie di licenze che probabilmente troverai sul web.

#### Tutti i diritti riservati

I creatori di opere originali come canzoni, libri o software spesso rilasciano il loro lavoro sotto protezione di copyright chiuso. Ciò significa che, per impostazione predefinita, essi (o il loro editore) hanno diritti esclusivi di usare (ad esempio, mostrare o distribuire) il loro lavoro. Se vuoi usare immagini protette da copyright con una licenza _tutti i diritti riservati_, devi fare una delle seguenti cose:

- Ottenere un permesso esplicito e scritto dal detentore del copyright.
- Pagare una tassa di licenza per usarle. Questo può essere un pagamento una tantum per un uso illimitato ("royalty-free"), o potrebbe essere "gestito dai diritti", nel qual caso potresti dover pagare tariffe specifiche per utilizzo per intervallo di tempo, area geografica, industria o tipo di media, ecc.
- Limitare i tuoi usi a quelli che sarebbero considerati [fair use](https://fairuse.stanford.edu/overview/fair-use/what-is-fair-use/) o [fair dealing](https://copyrightservice.co.uk/copyright/p27_work_of_others) nella tua giurisdizione.

Gli autori non sono tenuti ad includere un avviso di copyright o termini di licenza con il loro lavoro. Il diritto d'autore esiste automaticamente in un'opera originale di paternità una volta che è stata creata in un mezzo tangibile. Quindi se trovi un'immagine online e non ci sono avvisi di copyright o termini di licenza, il corso più sicuro è presumere che sia protetta da copyright con tutti i diritti riservati.

#### Permissiva

Se l'immagine è rilasciata sotto una licenza permissiva, come [MIT](https://mit-license.org/), [BSD](https://opensource.org/license/BSD-3-clause), o una [licenza Creative Commons (CC) adatta](https://creativecommons.org/chooser/), non è necessario pagare una tassa di licenza o cercare il permesso di utilizzarla. Tuttavia, ci sono varie condizioni di licensing che dovrai soddisfare, che variano a seconda della licenza.

Ad esempio, potresti dover:

- Fornire un link alla fonte originale dell'immagine e attribuirla al suo creatore.
- Indicare se sono state apportate modifiche a essa.
- Condividere qualsiasi opera derivata creata utilizzando l'immagine sotto la stessa licenza dell'originale.
- Non condividere alcuna opera derivata.
- Non utilizzare l'immagine in alcun lavoro commerciale.
- Includere una copia della licenza insieme a qualsiasi pubblicazione che utilizzi l'immagine.

Dovresti consultare la licenza applicabile per i termini specifici che dovrai seguire.

> [!NOTE]
> Potresti incontrare il termine "copyleft" nel contesto di licenze permissive. Le licenze copyleft (come la [GNU General Public License (GPL)](https://www.gnu.org/licenses/gpl-3.0.en.html) o le licenze Creative Commons "Share Alike") stabiliscono che le opere derivate devono essere rilasciate sotto la stessa licenza dell'originale.

Le licenze copyleft sono prominenti nel mondo del software. L'idea di base è che un nuovo progetto costruito con il codice di un progetto con licenza copyleft (questo è noto come "fork" del software originale) dovrà anche essere concesso in licenza con la stessa licenza copyleft. Questo assicura che il codice sorgente del nuovo progetto sarà anche reso disponibile per altri da studiare e modificare. Nota che, in generale, le licenze che sono state redatte per il software, come la GPL, non sono considerate buone licenze per opere non software poiché non sono state redatte pensando a opere non software.

Esplora i link forniti in precedenza in questa sezione per leggere i diversi tipi di licenze e i tipi di condizioni che specificano.

#### Dominio pubblico/CC0

Un'opera rilasciata nel dominio pubblico viene talvolta definita "nessun diritto riservato" — non si applica alcun copyright e può essere utilizzata senza permesso e senza dover soddisfare alcune condizioni di licenza. Un'opera può finire nel dominio pubblico attraverso vari mezzi come la scadenza del diritto d'autore o la rinuncia specifica ai diritti.

Uno dei modi più efficaci per collocare un'opera nel dominio pubblico è licenziarla sotto [CC0](https://creativecommons.org/public-domain/cc0/), una licenza di creative commons specifica che fornisce uno strumento legale chiaro e inequivocabile a questo scopo.

Quando si utilizzano immagini di dominio pubblico, ottenere la prova che l'immagine è nel dominio pubblico e conservare la prova per i propri registri. Ad esempio, prendi uno screenshot della fonte originale con lo stato della licenza chiaramente visualizzato e considera l'aggiunta di una pagina al tuo sito web con un elenco delle immagini acquisite insieme ai loro requisiti di licenza.

### Ricerca di immagini con licenza permissiva

Puoi trovare immagini con licenza permissiva per i tuoi progetti usando un motore di ricerca di immagini o direttamente dai repository di immagini.

Cerca immagini utilizzando una descrizione dell'immagine che stai cercando insieme a termini di licenza pertinenti. Ad esempio, quando cerchi "dinosauro giallo" aggiungi "immagini di dominio pubblico", "libreria di immagini di dominio pubblico", "immagini con licenza aperta", o termini simili alla query di ricerca.

Alcuni motori di ricerca hanno strumenti per aiutarti a trovare immagini con licenze permissive. Ad esempio, quando usi Google, vai alla scheda "Immagini" per cercare immagini, quindi fai clic su "Strumenti". C'è un menu a discesa "Diritti di utilizzo" sulla barra degli strumenti risultante dove puoi scegliere di cercare specificamente immagini sotto le licenze creative commons.

Siti di repository di immagini, come [Flickr](https://flickr.com/), [ShutterStock](https://www.shutterstock.com/), e [Pixabay](https://pixabay.com/), hanno opzioni di ricerca per consentirti di cercare solo immagini con licenza permissiva. Alcuni siti distribuiscono esclusivamente immagini e icone con licenza permissiva, come [Picryl](https://picryl.com/) e [The Noun Project](https://thenounproject.com/).

Il rispetto della licenza sotto la quale l'immagine è stata rilasciata è una questione di trovare i dettagli della licenza, leggere la pagina della licenza o delle istruzioni fornite dalla fonte, e quindi seguire quelle istruzioni. I repository di immagini rispettabili chiariscono e rendono facili da trovare le condizioni della loro licenza.

## Annotare immagini con figure e didascalie

Parlando di didascalie, ci sono diversi modi per aggiungere una didascalia insieme alla tua immagine. Ad esempio, non ci sarebbe nulla che ti impedisca di fare questo:

```html
<div class="figure">
  <img
    src="images/dinosaur.jpg"
    alt="The head and torso of a dinosaur skeleton;
            it has a large head with long sharp teeth"
    width="400"
    height="341" />

  <p>A T-Rex on display in the Manchester University Museum.</p>
</div>
```

Questo va bene. Contiene il contenuto di cui hai bisogno ed è stilizzabile con CSS. Ma c'è un problema qui: non c'è nulla che colleghi semanticamente l'immagine alla sua didascalia, il che può creare problemi per i lettori di schermo. Ad esempio, quando hai 50 immagini e didascalie, quale didascalia va con quale immagine?

Una soluzione migliore è usare gli elementi HTML {{htmlelement("figure")}} e {{htmlelement("figcaption")}}. Sono stati creati esattamente per questo scopo: fornire un contenitore semantico per le figure e collegare chiaramente la figura alla didascalia. Il nostro esempio precedente potrebbe essere riscritto così:

```html
<figure>
  <img
    src="images/dinosaur.jpg"
    alt="The head and torso of a dinosaur skeleton;
            it has a large head with long sharp teeth"
    width="400"
    height="341" />

  <figcaption>
    A T-Rex on display in the Manchester University Museum.
  </figcaption>
</figure>
```

L'elemento {{htmlelement("figcaption")}} dice ai browser e alle tecnologie assistive che la didascalia descrive l'altro contenuto dell'elemento {{htmlelement("figure")}}.

> [!NOTE]
> Da un punto di vista dell'accessibilità, didascalie e il testo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) hanno ruoli distinti. Le didascalie beneficiano anche delle persone che possono vedere l'immagine, mentre il testo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) fornisce la stessa funzionalità come un'immagine assente. Pertanto, le didascalie e il testo `alt` non dovrebbero dire esattamente la stessa cosa, perché appaiono entrambi quando l'immagine manca. Prova a disattivare le immagini nel tuo browser e guarda come appare.

Una figura non deve essere un'immagine. È un'unità indipendente di contenuto che:

- Esprime il tuo significato in modo compatto e facile da afferrare.
- Potrebbe andare in diversi punti nel flusso lineare della pagina.
- Fornisce informazioni essenziali a supporto del testo principale.

Una figura potrebbe essere diverse immagini, un frammento di codice, audio, video, equazioni, una tabella o qualcos'altro.

### Apprendimento attivo: creare una figura

In questa sezione di apprendimento attivo, vorremmo che prendessi il codice finito dalla sezione di apprendimento attivo precedente e lo trasformassi in una figura:

1. Avvolgilo in un elemento {{htmlelement("figure")}}.
2. Copia il testo dall'attributo `title`, rimuovi l'attributo `title`, e metti il testo all'interno di un elemento {{htmlelement("figcaption")}} sotto l'immagine.

Se fai un errore, puoi sempre reimpostarlo usando il pulsante _Reset_. Se sei davvero bloccato, premi il pulsante _Mostra soluzione_ per vedere una risposta:

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea
  id="code"
  class="input"
  style="min-height: 100px; width: 95%"></textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
html {
  font-family: sans-serif;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

body {
  margin: 10px;
  background: #f5f9fa;
}
```

```js hidden
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const output = document.querySelector(".output");
const code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  output.innerHTML = textarea.value;
}

const htmlSolution =
  '<figure>\n <img src="https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg"\n alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"\n width="200"\n height="171">\n <figcaption>A T-Rex on display in the Manchester University Museum</figcaption>\n</figure>';
let solutionEntry = htmlSolution;

reset.addEventListener("click", () => {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = htmlSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;

  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );
  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}

// Update the saved userCode every time the user updates the text area code

textarea.onkeyup = () => {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_creating_a_figure', 700, 350) }}

## Immagini di sfondo CSS

Puoi anche utilizzare CSS per incorporare immagini nelle pagine web (e JavaScript, ma questa è un'altra storia completamente diversa). La proprietà CSS {{cssxref("background-image")}}, e le altre proprietà `background-*`, vengono utilizzate per controllare il posizionamento delle immagini di sfondo. Ad esempio, per posizionare un'immagine di sfondo su ogni paragrafo di una pagina, potresti fare questo:

```css
p {
  background-image: url("images/dinosaur.jpg");
}
```

L'immagine incorporata risultante è forse più facile da posizionare e controllare rispetto alle immagini HTML. Allora perché preoccuparsi delle immagini HTML? Come accennato sopra, le immagini di sfondo CSS sono solo per decorazione. Se vuoi solo aggiungere qualcosa di carino alla tua pagina per migliorare il visivo, va bene. Tuttavia, tali immagini non hanno alcun significato semantico. Non possono avere alcun testo equivalente, sono invisibili ai lettori di schermo, e così via. Qui risplendono le immagini HTML!

In sintesi: se un'immagine ha significato, in termini di contenuto, dovresti usare un'immagine HTML. Se un'immagine è puramente decorativa, dovresti usare immagini di sfondo CSS (copriremo queste in dettaglio più avanti nei moduli principali).

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma ricordi le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Images).

## Riepilogo

Questo è tutto per ora. Abbiamo coperto in dettaglio immagini e didascalie. Nel prossimo articolo, porteremo le cose a un livello superiore, guardando come usare HTML per incorporare contenuti video e audio nelle pagine web.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}
