---
title: Immagini HTML
short-title: Images
slug: Learn_web_development/Core/Structuring_content/HTML_images
l10n:
  sourceCommit: c99b4f2d0ea81c0e8822a749d218015c75995b5b
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}

All'inizio, il web era solo testo, ed era davvero piuttosto noioso. Fortunatamente, non ci volle molto tempo prima che venisse aggiunta la possibilità di incorporare immagini (e altri tipi di contenuto più interessanti) all'interno delle pagine web. In questo articolo esamineremo in dettaglio come utilizzare l'elemento {{htmlelement("img")}}, inclusi i fondamenti, l'annotazione con didascalie utilizzando {{htmlelement("figure")}}, e come esso si relaziona alle {{Glossary("CSS", "immagini di sfondo CSS")}}.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come coperto in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
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
          <li>Sintassi di base del tag <code>&lt;img&gt;</code></li>
          <li>Utilizzo di <code>src</code> per puntare a una risorsa.</li>
          <li>Utilizzo di <code>width</code> e <code>height</code>, ad esempio, per evitare aggiornamenti poco gradevoli all'interfaccia utente una volta che un'immagine è stata caricata e viene visualizzata.</li>
          <li>Ottimizzazione delle risorse multimediali per il web — mantenere le dimensioni dei file ridotte.</li>
          <li>Comprendere le licenze delle risorse multimediali — diversi tipi di licenza, come conformarsi ad esse, e come cercare file multimediali correttamente licenziati da utilizzare nei progetti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Come inseriamo un'immagine in una pagina web?

Per inserire un'immagine in una pagina web, utilizziamo l'elemento {{htmlelement("img")}}. Questo è un {{Glossary("void_element", "elemento void")}} (cioè, non può avere alcun contenuto figlio e non può avere un tag di chiusura) che richiede due attributi per essere utile: `src` e `alt`. L'attributo `src` contiene un URL che punta all'immagine che si desidera incorporare nella pagina. Come con l'attributo `href` per gli elementi {{htmlelement("a")}}, l'attributo `src` può essere un URL relativo o un URL assoluto. Senza un attributo `src`, un elemento `img` non ha un'immagine da caricare.

L'[attributo `alt` è descritto più avanti](#testo_alternativo).

> [!NOTE]
> È opportuno leggere [Primer rapido su URL e percorsi](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#a_quick_primer_on_urls_and_paths) per rinfrescarsi la memoria sugli URL relativi e assoluti prima di continuare.

Quindi, ad esempio, se la vostra immagine si chiama `dinosaur.jpg` e si trova nella stessa directory della vostra pagina HTML, potreste incorporare l'immagine in questo modo:

```html
<img src="dinosaur.jpg" alt="Dinosaur" />
```

Se l'immagine si trovasse in una sottodirectory `images`, che si trova all'interno della stessa directory della pagina HTML, allora la incorporereste così:

```html
<img src="images/dinosaur.jpg" alt="Dinosaur" />
```

E così via.

> [!NOTE]
> Anche i motori di ricerca leggono i nomi dei file delle immagini e li contano per la SEO. Pertanto, si dovrebbe dare all'immagine un nome file descrittivo; `dinosaur.jpg` è meglio di `img835.png`.

Potreste anche incorporare l'immagine usando il suo URL assoluto, ad esempio:

```html
<img src="https://www.example.com/images/dinosaur.jpg" alt="Dinosaur" />
```

Tuttavia, non è raccomandato collegarsi tramite URL assoluti. Dovreste ospitare le immagini che volete usare sul vostro sito, il che in configurazioni semplici significa mantenere le immagini per il vostro sito web sullo stesso server del vostro HTML. Inoltre, è più efficiente utilizzare URL relativi che URL assoluti in termini di manutenzione (quando si sposta il sito su un dominio diverso, non sarà necessario aggiornare tutti gli URL per includere il nuovo dominio). In configurazioni più avanzate, potreste utilizzare una {{Glossary("CDN", "CDN (Rete di Consegna Contenuti)")}} per distribuire le vostre immagini.

Se non avete creato le immagini, dovreste assicurarvi di avere il permesso di usarle secondo le condizioni della licenza sotto cui sono pubblicate (vedi [Risorse multimediali e licenze](#risorse_multimediali_e_licenze) qui sotto per ulteriori informazioni).

> **Warning:** _Mai_ puntare l'attributo `src` a un'immagine ospitata su un sito web di qualcun altro _senza permesso_. Questo è chiamato "hotlinking". È considerato non etico, poiché qualcun altro dovrebbe pagare i costi di banda per la consegna dell'immagine quando qualcuno visita la vostra pagina. Vi lascia anche senza controllo sull'immagine che può essere rimossa o sostituita con qualcosa di imbarazzante.

Il precedente snippet di codice, sia con l'URL assoluto che relativo, ci darà il seguente risultato:

![Un'immagine base di un dinosauro, incorporata in un browser, con sopra scritto "Immagini in HTML"](basic-image.png)

> [!NOTE]
> Elementi come {{htmlelement("img")}} e {{htmlelement("video")}} sono talvolta indicati come **elementi sostituiti**. Questo perché il contenuto e la dimensione dell'elemento sono definiti da una risorsa esterna (come un file immagine o video), non dal contenuto dell'elemento stesso. Potete leggere di più su di loro in {{Glossary("replaced_elements", "elementi sostituiti")}}.

> [!NOTE]
> Potete trovare l'esempio completo di questa sezione [in esecuzione su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/images-in-html/index.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/images-in-html/index.html)).

### Testo alternativo

Il prossimo attributo che esamineremo è `alt`. Il suo valore deve essere una descrizione testuale dell'immagine, da utilizzare in situazioni in cui l'immagine non può essere vista/visualizzata o richiede troppo tempo per essere resa a causa di una connessione internet lenta. Ad esempio, il nostro codice sopra potrebbe essere modificato in questo modo:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth" />
```

Il modo più semplice per testare il vostro testo `alt` è di fare intenzionalmente uno sbaglio nel nome del file. Se, ad esempio, il nostro nome immagine fosse scritto `dinosooooor.jpg`, il browser non mostrerebbe l'immagine, e mostrerebbe il testo `alt` al suo posto:

![Il titolo Immagini in HTML, ma questa volta l'immagine del dinosauro non è visualizzata, e al suo posto c'è il testo alt.](alt-text.png)

Quindi, perché mai dovreste vedere o avere bisogno di testo `alt`? Può essere utile per diversi motivi:

- L'utente è ipovedente e utilizza uno [screen reader](https://en.wikipedia.org/wiki/Screen_reader) per leggere il web. In effetti, avere del testo `alt` disponibile per descrivere le immagini è utile per la maggior parte degli utenti.
- Come descritto sopra, l'ortografia del nome del file o del percorso potrebbe essere errata.
- Il browser non supporta il tipo di immagine. Alcune persone utilizzano ancora browser solo testo, come [Lynx](https://en.wikipedia.org/wiki/Lynx_%28web_browser%29), che mostrano il testo `alt` delle immagini.
- Potreste voler fornire testo ai motori di ricerca da utilizzare; ad esempio, i motori di ricerca possono confrontare il testo `alt` con le query di ricerca.
- Gli utenti hanno disattivato le immagini per ridurre il volume di trasferimento dei dati e le distrazioni. Questo è particolarmente comune sui telefoni cellulari e in paesi dove la larghezza di banda è limitata o costosa.

Cosa esattamente dovreste scrivere all'interno del vostro attributo `alt`? Dipende da _perché_ l'immagine è lì in primo luogo. In altre parole, cosa si perde se l'immagine non viene visualizzata:

- **Decorazione.** Si dovrebbero usare [immagini di sfondo CSS](#immagini_di_sfondo_css) per immagini decorative, ma se si deve usare l'HTML, aggiungere un `alt=""` vuoto. Se l'immagine non fa parte del contenuto, uno screen reader non dovrebbe perdere tempo a leggerla.
- **Contenuto.** Se la vostra immagine fornisce informazioni significative, fornite le stesse informazioni in un testo `alt` _breve_ — o ancora meglio, nel testo principale che tutti possono vedere. Non scrivete un testo `alt` ridondante. Quanto potrebbe essere fastidioso per un utente vedente se tutti i paragrafi fossero scritti due volte nel contenuto principale? Se l'immagine è descritta adeguatamente dal corpo del testo principale, potete semplicemente usare `alt=""`.
- **Link.** Se si inserisce un'immagine all'interno di un tag {{htmlelement("a")}}, per trasformare un'immagine in un link, si deve comunque fornire [testo link accessibile](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#use_clear_link_wording). In tali casi, potete scriverlo all'interno dello stesso elemento `<a>`, o all'interno dell'attributo `alt` dell'immagine, a seconda di ciò che funziona meglio nel vostro caso.
- **Testo.** Non dovreste inserire il vostro testo nelle immagini. Se il vostro titolo principale ha bisogno di un'ombra, per esempio, [usate CSS](/it/docs/Web/CSS/text-shadow) per questo piuttosto che inserire il testo in un'immagine. Tuttavia, se _davvero non potete evitare di farlo_, dovreste fornire il testo all'interno dell'attributo `alt`.

Fondamentalmente, il punto chiave è fornire un'esperienza utilizzabile, anche quando le immagini non possono essere viste. Questo assicura che tutti gli utenti non perdano nessuno dei contenuti. Provate a disattivare le immagini nel vostro browser e guardate come appare tutto. Ci si renderà presto conto di quanto sia utile il testo `alt` quando l'immagine non può essere vista.

> [!NOTE]
> Vedere la nostra guida ai [Testi alternativi](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) e [An alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) per imparare come usare un attributo `alt` per le immagini in varie situazioni.

### Larghezza e altezza

Si possono usare gli attributi [`width`](/it/docs/Web/HTML/Reference/Elements/img#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/img#height) per specificare la larghezza e l'altezza della propria immagine. Sono dati come interi senza unità e rappresentano la larghezza e l'altezza dell'immagine in pixel.

Potete trovare la larghezza e l'altezza della vostra immagine in diversi modi. Per esempio, su Mac potete usare <kbd>Cmd</kbd> + <kbd>I</kbd> per ottenere le informazioni di visualizzazione per il file immagine. Tornando al nostro esempio, potremmo fare così:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341" />
```

C'è un buon motivo per farlo. L'HTML per la vostra pagina e l'immagine sono risorse separate, recuperate dal browser come richieste HTTP(S) separate. Non appena il browser ha ricevuto l'HTML, inizierà a visualizzarlo all'utente. Se le immagini non sono ancora state ricevute (e spesso sarà il caso, dato che le dimensioni dei file delle immagini sono spesso molto più grandi dei file HTML), allora il browser renderà solo l'HTML e aggiornerà la pagina con l'immagine non appena questa sarà ricevuta.

Per esempio, supponiamo di avere del testo dopo l'immagine:

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

Non appena il browser scaricherà l'HTML, inizierà a visualizzare la pagina.

Una volta che l'immagine è stata caricata, il browser aggiunge l'immagine alla pagina. Poiché l'immagine occupa spazio, il browser deve spostare il testo verso il basso nella pagina, per far posto all'immagine sopra di esso:

![Confronto del layout della pagina mentre il browser carica una pagina e quando ha finito, quando non viene specificata alcuna dimensione per l'immagine.](no-size.png)

Spostare il testo in questo modo è estremamente distraente per gli utenti, soprattutto se hanno già iniziato a leggerlo.

Se si specifica la dimensione effettiva dell'immagine nel proprio HTML, utilizzando gli attributi `width` e `height`, il browser saprà, prima di aver scaricato l'immagine, quanto spazio deve consentire per essa.

Questo significa che quando l'immagine è stata scaricata, il browser non deve spostare il contenuto circostante.

![Confronto del layout della pagina mentre il browser carica una pagina e quando ha finito, quando viene specificata la dimensione dell'immagine.](size.png)

Per un eccellente articolo sulla storia di questa funzione, vedere [Impostare altezza e larghezza sulle immagini è di nuovo importante](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/).

> [!NOTE]
> Anche se, come abbiamo detto, è buona pratica specificare la dimensione _effettiva_ delle vostre immagini utilizzando gli attributi HTML, non dovreste usarli per _ridimensionare_ le immagini.
>
> Se impostate l'immagine troppo grande, finirete con immagini che appaiono sgranate, sfocate, oppure sprecate banda scaricando un'immagine che non soddisfa le esigenze dell'utente. L'immagine potrebbe anche apparire distorta, se non si mantiene il corretto {{Glossary("aspect_ratio", "rapporto di aspetto")}}. Dovreste utilizzare un editor di immagini per mettere la vostra immagine alla dimensione corretta prima di inserirla nella vostra pagina web.
>
> Se dovete alterare la dimensione di un'immagine, dovreste usare [CSS](/it/docs/Learn_web_development/Core/Styling_basics) invece.

### Titoli delle immagini

Come [con i link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#adding_supporting_information_with_the_title_attribute), potete anche aggiungere attributi `title` alle immagini per fornire ulteriori informazioni di supporto se necessario. Nel nostro esempio, potremmo fare così:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341"
  title="A T-Rex on display in the Manchester University Museum" />
```

Questo ci dà un tooltip quando il mouse passa sopra, proprio come i titoli dei link:

![L'immagine del dinosauro con un tooltip title sopra di essa che recita Un T-Rex in mostra al Museo dell'Università di Manchester](image-with-title.png)

Tuttavia, questo non è raccomandato — `title` ha una serie di problemi di accessibilità, principalmente legati al fatto che il supporto degli screen reader è molto imprevedibile e la maggior parte dei browser non lo mostrerà a meno che non si stia passando con il mouse (quindi, ad esempio, nessun accesso per gli utenti della tastiera). Se siete interessati a maggiori informazioni su questo, leggete [The Trials and Tribulations of the Title Attribute](https://www.24a11y.com/2017/the-trials-and-tribulations-of-the-title-attribute/) di Scott O'Hara.

È meglio includere tali informazioni di supporto nel testo principale dell'articolo piuttosto che allegato all'immagine.

### Apprendimento attivo: incorporare un'immagine

Ora tocca a voi giocare! Questa sezione di apprendimento attivo vi farà iniziare con un esercizio di incorporamento. Vi viene fornito un tag {{htmlelement("img")}} di base; ci piacerebbe che incorporaste l'immagine situata al seguente URL:

```url
https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg
```

In precedenza abbiamo detto di non collegarsi mai a immagini su altri server, ma questo è solo a scopo di apprendimento, quindi faremo un'eccezione per una volta.

Vorremmo anche che voi:

- Aggiungeste del testo `alt`, e verificaste che funzioni sbagliando l'URL dell'immagine.
- Impostaste la `width` e `height` corrette dell'immagine (suggerimento: è larga 200px e alta 171px), quindi sperimentate con altri valori per vedere quale è l'effetto.
- Impostaste un `title` sull'immagine.

Se commettete un errore, potete sempre resettare usando il pulsante _Reset_. Se siete veramente bloccati, premete il pulsante _Show solution_ per vedere una risposta:

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

## Risorse multimediali e licenze

Le immagini (e altri tipi di risorse multimediali) che trovate sul web sono rilasciate sotto vari tipi di licenze. Prima di usare un'immagine su un sito che state costruendo, assicuratevi di possederla, di avere il permesso di usarla o di rispettare le condizioni di licenza del proprietario.

### Comprendere i tipi di licenza

Esaminiamo alcune categorie comuni di licenze che è probabile trovare sul web.

#### Tutti i diritti riservati

I creatori di opere originali come canzoni, libri o software rilasciano spesso il loro lavoro sotto la protezione del diritto d'autore chiuso. Questo significa che, di default, essi (o il loro editore) hanno diritti esclusivi di utilizzare (ad esempio, visualizzare o distribuire) il loro lavoro. Se volete usare immagini protette da copyright con una licenza _tutti i diritti riservati_, dovete fare una delle seguenti cose:

- Ottenere permesso esplicito e scritto dal detentore del copyright.
- Pagare una tassa di licenza per usarle. Questa può essere una tassa una tantum per un uso illimitato ("royalty-free"), oppure potrebbe essere "rights-managed", nel qual caso potrebbe essere necessario pagare commissioni specifiche per uso da tempo slot, regione geografica, settore o tipo di media, ecc.
- Limitare i vostri usi a quelli che sarebbero considerati [uso corretto](https://fairuse.stanford.edu/overview/fair-use/what-is-fair-use/) o [fair dealing](https://copyrightservice.co.uk/copyright/p27_work_of_others) nella vostra giurisdizione.

Gli autori non sono tenuti a includere un avviso sul copyright o termini di licenza con il loro lavoro. Il copyright esiste automaticamente in un'opera originale di autorialità una volta che è creata in un mezzo tangibile. Quindi, se trovate un'immagine online e non ci sono avvisi sul copyright o termini di licenza, il corso più sicuro è supporre che sia protetta da copyright con tutti i diritti riservati.

#### Permissivo

Se l'immagine è rilasciata sotto una licenza permissiva, come [MIT](https://mit-license.org/), [BSD](https://opensource.org/license/BSD-3-clause) o una licenza [Creative Commons (CC) adatta](https://creativecommons.org/chooser/), non è necessario pagare una tassa di licenza o cercare il permesso di usarla. Tuttavia, ci sono varie condizioni di licenza che si dovranno soddisfare, le quali variano a seconda della licenza.

Ad esempio, potrebbe essere necessario:

- Fornire un link alla fonte originale dell'immagine e accreditare il suo creatore.
- Indicare se sono state apportate modifiche.
- Condividere eventuali opere derivate create utilizzando l'immagine sotto la stessa licenza dell'originale.
- Non condividere alcuna opera derivata.
- Non utilizzare l'immagine in alcun lavoro commerciale.
- Includere una copia della licenza insieme a qualsiasi rilascio che utilizzi l'immagine.

Dovreste consultare la licenza applicabile per i termini specifici che dovrete seguire.

> [!NOTE]
> Potreste incontrare il termine "copyleft" nel contesto delle licenze permissive. Le licenze copyleft (come la [GNU General Public License (GPL)](https://www.gnu.org/licenses/gpl-3.0.en.html) o le licenze Creative Commons "Share Alike") stabiliscono che le opere derivate devono essere rilasciate sotto la stessa licenza dell'originale.

Le licenze copyleft sono prominenti nel mondo del software. L'idea di base è che un nuovo progetto costruito con il codice di un progetto licenziato in copyleft (questo è noto come "fork" del software originale) dovrà essere anch'esso licenziato sotto la stessa licenza copyleft. Questo assicura che anche il codice sorgente del nuovo progetto sia reso disponibile per altri per studiare e modificare. Si noti che, in generale, le licenze che sono state redatte per il software, come la GPL, non sono considerate buone licenze per opere non software poiché non sono state redatte pensando alle opere non software.

Esplorate i link forniti in precedenza in questa sezione per leggere sui diversi tipi di licenza e i tipi di condizioni che specificano.

#### Pubblico dominio/CC0

Le opere rilasciate nel pubblico dominio sono talvolta indicate come "nessun diritto riservato" — non si applica alcun copyright e possono essere utilizzate senza permesso e senza dover soddisfare alcuna condizione di licenza. Le opere possono finire nel pubblico dominio mediante vari mezzi, come l'espirazione del copyright, o lo specifico rinuncio dei diritti.

Uno dei modi più efficaci per posizionare il lavoro nel pubblico dominio è licenziarlo sotto [CC0](https://creativecommons.org/public-domain/cc0/), una specifica licenza creative commons che fornisce uno strumento legale chiaro e inequivocabile per questo scopo.

Quando si utilizzano immagini nel pubblico dominio, ottenete prove che l'immagine è nel pubblico dominio e conservate le prove per i vostri record. Ad esempio, fate uno screenshot della fonte originale con lo stato della licenza chiaramente visualizzato e considerate l'aggiunta di una pagina al vostro sito web con un elenco delle immagini acquisite insieme ai loro requisiti di licenza.

### Ricerca di immagini con licenza permissiva

Potete trovare immagini con licenza permissiva per i vostri progetti utilizzando un motore di ricerca immagini o direttamente da repository di immagini.

Cercate immagini utilizzando una descrizione dell'immagine che state cercando insieme ai termini di licenza pertinenti. Ad esempio, quando cercate "dinosauro giallo" aggiungete "immagini di pubblico dominio", "libreria di immagini di pubblico dominio", "immagini con licenza aperta", o termini simili alla query di ricerca.

Alcuni motori di ricerca hanno strumenti che vi aiutano a trovare immagini con licenze permissive. Ad esempio, utilizzando Google, andate alla scheda "Immagini" per cercare immagini, quindi cliccate su "Strumenti". C'è un menu a discesa "Diritti di utilizzo" nella barra degli strumenti risultante in cui potete scegliere di cercare specificamente immagini sotto licenze creative commons.

Siti di repository di immagini, come [Flickr](https://flickr.com/), [ShutterStock](https://www.shutterstock.com/), e [Pixabay](https://pixabay.com/), hanno opzioni di ricerca che vi permettono di cercare solo immagini con licenza permissiva. Alcuni siti distribuiscono esclusivamente immagini e icone con licenza permissiva, come [Picryl](https://picryl.com/) e [The Noun Project](https://thenounproject.com/).

Rispettare la licenza sotto cui l'immagine è stata rilasciata è una questione di trovare i dettagli della licenza, leggere la pagina di licenza o le istruzioni fornite dalla fonte, e seguire quelle istruzioni. I repository di immagini affidabili rendono chiari e facili da trovare le condizioni della loro licenza.

## Annotazione delle immagini con figure e didascalie

Parlando di didascalie, ci sono diversi modi in cui potreste aggiungere una didascalia da accompagnare alla vostra immagine. Ad esempio, non ci sarebbe nulla che vi impedisca di fare questo:

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

Questo va bene. Contiene il contenuto di cui avete bisogno ed è facilmente stilizzabile utilizzando CSS. Ma c'è un problema qui: non c'è nulla che semanticamente colleghi l'immagine alla sua didascalia, il che può causare problemi per gli screen reader. Ad esempio, quando avete 50 immagini e didascalie, quale didascalia va con quale immagine?

Una soluzione migliore è utilizzare gli elementi HTML {{htmlelement("figure")}} e {{htmlelement("figcaption")}}. Questi sono stati creati proprio per questo scopo: fornire un contenitore semantico per le figure e collegare chiaramente la figura alla didascalia. Il nostro esempio precedente potrebbe essere riscritto in questo modo:

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

L'elemento {{htmlelement("figcaption")}} dice ai browser e alla tecnologia assistiva che la didascalia descrive l'altro contenuto dell'elemento {{htmlelement("figure")}}.

> [!NOTE]
> Dal punto di vista dell'accessibilità, le didascalie e il testo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) hanno ruoli distinti. Le didascalie beneficiano anche le persone che possono vedere l'immagine, mentre il testo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) fornisce la stessa funzionalità di un'immagine assente. Pertanto, le didascalie e il testo `alt` non dovrebbero dire la stessa cosa, perché appaiono entrambi quando l'immagine è assente. Provate a disattivare le immagini nel vostro browser e vedete come appare.

Una figura non deve essere un'immagine. È un'unità indipendente di contenuto che:

- Esprime il vostro significato in un modo compatto e facile da afferrare.
- Potrebbe posizionarsi in diversi punti nel flusso lineare della pagina.
- Fornisce informazioni essenziali che supportano il testo principale.

Una figura potrebbe essere diverse immagini, uno snippet di codice, audio, video, equazioni, una tabella o qualcos'altro.

### Apprendimento attivo: creare una figura

In questa sezione di apprendimento attivo, vorremmo che prendeste il codice finito dalla precedente sezione di apprendimento attivo e lo trasformaste in una figura:

1. Avvolgetelo in un elemento {{htmlelement("figure")}}.
2. Copiate il testo dall'attributo `title`, rimuovete l'attributo `title`, e mettete il testo all'interno di un elemento {{htmlelement("figcaption")}} sotto l'immagine.

Se commettete un errore, potete sempre resettare usando il pulsante _Reset_. Se siete veramente bloccati, premete il pulsante _Show solution_ per vedere una risposta:

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

Potete anche usare CSS per incorporare immagini nelle pagine web (e JavaScript, ma questa è un'altra storia del tutto). La proprietà CSS {{cssxref("background-image")}}, e le altre proprietà `background-*`, sono utilizzate per controllare il posizionamento delle immagini di sfondo. Ad esempio, per posizionare un'immagine di sfondo su ogni paragrafo di una pagina, potreste fare così:

```css
p {
  background-image: url("images/dinosaur.jpg");
}
```

L'immagine incorporata risultante è probabilmente più facile da posizionare e controllare rispetto alle immagini HTML. Quindi, perché preoccuparsi delle immagini HTML? Come accennato sopra, le immagini di sfondo CSS sono solo per decorazione. Se volete solo aggiungere qualcosa di carino alla vostra pagina per migliorare l'aspetto visivo, va bene. Tuttavia, tali immagini non hanno alcun significato semantico. Non possono avere equivalenti testuali, sono invisibili agli screen reader, e così via. Qui le immagini HTML brillano!

Riassumendo: se un'immagine ha significato, in termini di vostro contenuto, dovreste usare un'immagine HTML. Se un'immagine è puramente decorativa, dovreste usare immagini di sfondo CSS (le copriremo in dettaglio più avanti nei moduli Core).

## Testa le tue competenze!

Sei giunto alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che tu abbia trattenuto queste informazioni prima di procedere — vedi [Testa le tue competenze: Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Images).

## Riepilogo

Questo è tutto per ora. Abbiamo trattato in dettaglio le immagini e le didascalie. Nel prossimo articolo, passeremo a un livello superiore, esaminando come usare l'HTML per incorporare contenuti video e audio nelle pagine web.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}
