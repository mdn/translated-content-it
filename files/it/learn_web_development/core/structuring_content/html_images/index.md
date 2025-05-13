---
title: Immagini HTML
short-title: Images
slug: Learn_web_development/Core/Structuring_content/HTML_images
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}

All’inizio, il web era solo testo, e risultava davvero piuttosto noioso. Fortunatamente, non ci volle molto prima di aggiungere la possibilità di incorporare immagini (e altri tipi di contenuti più interessanti) all’interno delle pagine web. In questo articolo esamineremo come utilizzare a fondo l'elemento {{htmlelement("img")}}, compresi i fondamenti, l’annotazione con didascalie usando {{htmlelement("figure")}}, e il dettaglio su come si relaziona alle immagini di sfondo {{Glossary("CSS", "CSS")}}.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Il termine "elemento sostituito" — cosa significa?</li>
          <li>Sintassi di base del tag <code>&lt;img&gt;</code></li>
          <li>Utilizzare <code>src</code> per puntare a una risorsa.</li>
          <li>Utilizzare <code>width</code> e <code>height</code>, ad esempio, per evitare aggiornamenti spiacevoli e sussultanti dell'interfaccia una volta che un'immagine è stata caricata e visualizzata.</li>
          <li>Ottimizzare i file multimediali per il web — mantenere le dimensioni dei file ridotte.</li>
          <li>Comprendere le licenze dei file multimediali — diversi tipi di licenza, come rispettarle e come cercare i file multimediali correttamente licenziati da utilizzare nei progetti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Come inserire un'immagine in una pagina web?

Per inserire un'immagine in una pagina web, utilizziamo l'elemento {{htmlelement("img")}}. Questo è un {{Glossary("void_element", "elemento vuoto")}} (il che significa che non può avere contenuto figlio e non può avere un tag di chiusura) che richiede due attributi per essere utile: `src` e `alt`. L'attributo `src` contiene un URL che punta all'immagine che vuole incorporare nella pagina. Come con l'attributo `href` per gli elementi {{htmlelement("a")}}, l'attributo `src` può essere un URL relativo o un URL assoluto. Senza un attributo `src`, un elemento `img` non ha immagini da caricare.

L'[attributo `alt` viene descritto di seguito](#testo_alternativo).

> [!NOTE]
> Si consiglia di leggere [Un rapido primer su URL e percorsi](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#a_quick_primer_on_urls_and_paths) per rinfrescare la memoria sugli URL relativi e assoluti prima di continuare.

Ad esempio, se la sua immagine si chiama `dinosaur.jpg` e si trova nella stessa directory della sua pagina HTML, potrebbe incorporare l'immagine in questo modo:

```html
<img src="dinosaur.jpg" alt="Dinosaur" />
```

Se l'immagine fosse in una sottodirectory `images`, che si trova dentro la stessa directory della pagina HTML, allora la incorporerebbe in questo modo:

```html
<img src="images/dinosaur.jpg" alt="Dinosaur" />
```

E così via.

> [!NOTE]
> I motori di ricerca leggono anche i nomi dei file delle immagini e li considerano per la SEO. Pertanto, dovrebbe dare alla sua immagine un nome descrittivo; `dinosaur.jpg` è meglio di `img835.png`.

Potrebbe anche incorporare l'immagine utilizzando il suo URL assoluto, ad esempio:

```html
<img src="https://www.example.com/images/dinosaur.jpg" alt="Dinosaur" />
```

Tuttavia, non è raccomandato collegarsi tramite URL assoluti. Dovrebbe ospitare le immagini che intende utilizzare sul proprio sito, il che in configurazioni semplici significa mantenere le immagini per il suo sito web sullo stesso server del suo HTML. Inoltre, è più efficiente utilizzare URL relativi piuttosto che URL assoluti in termini di manutenzione (quando sposta il suo sito su un dominio diverso, non dovrà aggiornare tutti i suoi URL per includere il nuovo dominio). In configurazioni più avanzate, potrebbe utilizzare una {{Glossary("CDN", "CDN (Content Delivery Network)")}} per distribuire le sue immagini.

Se non ha creato le immagini, dovrebbe assicurarsi di avere il permesso di utilizzarle nelle condizioni della licenza sotto cui sono pubblicate (vedi sotto [Asset multimediali e licenze](#asset_multimediali_e_licenze) per ulteriori informazioni).

> **Warning:** _Mai_ puntare l'attributo `src` a un'immagine ospitata sul sito web di qualcun altro _senza permesso_. Questo è chiamato "hotlinking". È considerato non etico, poiché qualcun altro pagherebbe i costi della larghezza di banda per fornire l'immagine quando qualcuno visita la sua pagina. Inoltre, la lascia senza controllo sull'immagine che potrebbe essere rimossa o sostituita con qualcosa di imbarazzante.

Il precedente snippet di codice, sia con l'URL assoluto sia relativo, ci darà il seguente risultato:

![Un'immagine di base di dinosauro, incorporata in un browser, con "Immagini in HTML" scritto sopra](basic-image.png)

> [!NOTE]
> Gli elementi come {{htmlelement("img")}} e {{htmlelement("video")}} sono talvolta indicati come **elementi sostituiti**. Questo perché il contenuto e le dimensioni dell'elemento sono definiti da una risorsa esterna (come un file immagine o video), e non dal contenuto dell'elemento stesso. Può leggere di più su di loro su {{Glossary("replaced_elements", "elementi sostituiti")}}.

> [!NOTE]
> Può trovare l'esempio finale di questa sezione [in esecuzione su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/images-in-html/index.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/images-in-html/index.html)).

### Testo alternativo

Il prossimo attributo che esamineremo è `alt`. Il suo valore dovrebbe essere una descrizione testuale dell'immagine, da utilizzare in situazioni in cui l'immagine non può essere vista/visualizzata o impiega molto tempo a essere renderizzata a causa di una connessione lenta a Internet. Ad esempio, il nostro codice sopra potrebbe essere modificato in questo modo:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth" />
```

Il modo più semplice per testare il suo testo `alt` è sbagliare volontariamente il nome del file. Se ad esempio il nome della nostra immagine fosse scritto `dinosooooor.jpg`, il browser non visualizzerebbe l'immagine e mostrerebbe invece il testo alternativo:

![Il titolo Immagini in HTML, ma questa volta l'immagine del dinosauro non viene visualizzata e al suo posto c'è il testo alternativo.](alt-text.png)

Allora, perché mai vedrebbe o avrebbe bisogno di un testo alternativo? Può essere utile per vari motivi:

- L'utente è ipovedente e sta usando un [lettore di schermo](https://en.wikipedia.org/wiki/Screen_reader) per leggere il web. Infatti, avere un testo alternativo disponibile per descrivere le immagini è utile per la maggior parte degli utenti.
- Come descritto sopra, l'ortografia del nome del file o del percorso potrebbe essere errata.
- Il browser non supporta il tipo di immagine. Alcune persone usano ancora browser solo testuali, come [Lynx](https://en.wikipedia.org/wiki/Lynx_%28web_browser%29), che visualizza il testo alternativo delle immagini.
- Potrebbe voler fornire del testo che i motori di ricerca possano utilizzare; ad esempio, i motori di ricerca possono abbinare il testo alternativo alle query di ricerca.
- Gli utenti hanno disattivato le immagini per ridurre il volume di trasferimento dati e le distrazioni. Questo è particolarmente comune sui telefoni cellulari e nei paesi in cui la larghezza di banda è limitata o costosa.

Cosa dovrebbe scrivere esattamente all'interno del suo attributo `alt`? Dipende dal _perché_ l'immagine è lì in primo luogo. In altre parole, cosa si perde se la sua immagine non compare:

- **Decorazione.** Dovrebbe utilizzare [immagini di sfondo CSS](#immagini_di_sfondo_css) per immagini decorative, ma se deve utilizzare HTML, aggiunga `alt=""` vuoto. Se l'immagine non fa parte del contenuto, un lettore di schermo non dovrebbe perdere tempo a leggerla.
- **Contenuto.** Se la sua immagine fornisce informazioni significative, fornisca le stesse informazioni in un testo _breve_ `alt` — o, ancor meglio, nel testo principale che tutti possono vedere. Non scriva testo alternativo ridondante. Quanto sarebbe fastidioso per un utente vedente se tutti i paragrafi fossero scritti due volte nel contenuto principale? Se l'immagine è adeguatamente descritta dal testo principale, può semplicemente utilizzare `alt=""`.
- **Link.** Se inserisce un'immagine all'interno dei tag {{htmlelement("a")}}, per trasformare un'immagine in un collegamento, deve comunque fornire [testo del collegamento accessibile](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#use_clear_link_wording). In tali casi può, o scrivere all'interno dello stesso elemento `<a>`, o all'interno dell'attributo `alt` dell'immagine — a seconda di cosa funziona meglio nel suo caso.
- **Testo.** Non dovrebbe inserire il suo testo nelle immagini. Se la sua intestazione principale necessita di un'ombra, ad esempio, [usi CSS](/it/docs/Web/CSS/text-shadow) per questo piuttosto che inserire il testo in un'immagine. Tuttavia, se _proprio non può evitarlo_, dovrebbe fornire il testo all'interno dell'attributo `alt`.

In sostanza, la chiave è offrire un'esperienza utilizzabile, anche quando le immagini non possono essere viste. Questo assicura che tutti gli utenti non perdano nessuno dei contenuti. Provi a disattivare le immagini nel suo browser e vedere come appaiono le cose. Si renderà presto conto di quanto sia utile il testo alternativo se l'immagine non può essere vista.

> [!NOTE]
> Vedere la nostra guida per [Testo Alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) e [Un albero decisionale per alt](https://www.w3.org/WAI/tutorials/images/decision-tree/) per imparare a utilizzare un attributo `alt` per le immagini in varie situazioni.

### Larghezza e altezza

Può utilizzare gli attributi [`width`](/it/docs/Web/HTML/Reference/Elements/img#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/img#height) per specificare la larghezza e l'altezza della sua immagine. Sono forniti come interi senza unità, e rappresentano la larghezza e l'altezza dell'immagine in pixel.

Può trovare la larghezza e l'altezza della sua immagine in vari modi. Ad esempio, sul Mac può utilizzare <kbd>Cmd</kbd> + <kbd>I</kbd> per ottenere le informazioni di visualizzazione per il file dell'immagine. Tornando al nostro esempio, potremmo fare questo:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341" />
```

C'è un ottimo motivo per farlo. L'HTML per la sua pagina e l'immagine sono risorse separate, recuperate dal browser come richieste HTTP(S) separate. Non appena il browser riceve l'HTML, inizierà a visualizzarlo all'utente. Se le immagini non sono state ancora ricevute (e spesso questo sarà il caso, poiché le dimensioni dei file immagine sono spesso molto più grandi dei file HTML), il browser renderizzerà solo l'HTML e aggiornerà la pagina con l'immagine non appena sarà ricevuta.

Ad esempio, supponiamo che ci sia del testo dopo l'immagine:

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

Appena il browser scarica l'HTML, il browser inizierà a visualizzare la pagina.

Una volta caricata l'immagine, il browser aggiunge l'immagine alla pagina. Poiché l'immagine occupa spazio, il browser deve spostare il testo giù nella pagina, per adattare l'immagine sopra di esso:

![Confronto del layout della pagina mentre il browser sta caricando una pagina e quando ha finito, quando nessuna dimensione è specificata per l'immagine.](no-size.png)

Spostare il testo in questo modo è estremamente distrattente per gli utenti, specialmente se hanno già iniziato a leggerlo.

Se specifica la dimensione effettiva dell'immagine nel suo HTML, usando gli attributi `width` e `height`, il browser saprà, prima ancora di aver scaricato l'immagine, quanto spazio deve lasciare per essa.

Questo significa che quando l'immagine è stata scaricata, il browser non deve spostare il contenuto circostante.

![Confronto del layout della pagina mentre il browser sta caricando una pagina e quando ha finito, quando la dimensione dell'immagine è specificata.](size.png)

Per un eccellente articolo sulla storia di questa caratteristica, veda [Impostare altezza e larghezza sulle immagini è importante di nuovo](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/).

> [!NOTE]
> Sebbene, come abbiamo detto, sia buona pratica specificare la dimensione _effettiva_ delle sue immagini usando attributi HTML, non dovrebbe utilizzarli per _ridimensionare_ le immagini.
>
> Se imposta la dimensione dell'immagine troppo grande, finirà con immagini che appaiono sgranate, sfocate o troppo piccole, e sprecherà banda scaricando un'immagine che non soddisfa le esigenze dell'utente. L'immagine potrebbe anche risultare distorta, se non mantiene il corretto {{Glossary("aspect_ratio", "rapporto d'aspetto")}}. Dovrebbe usare un editor di immagini per rendere la sua immagine nella dimensione corretta prima di metterla sulla sua pagina web.
>
> Se ha bisogno di alterare la dimensione di un'immagine, dovrebbe usare [CSS](/it/docs/Learn_web_development/Core/Styling_basics) al posto.

### Titoli delle immagini

Come [con i collegamenti](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#adding_supporting_information_with_the_title_attribute), può anche aggiungere attributi `title` alle immagini, per fornire ulteriori informazioni di supporto se necessario. Nel nostro esempio, potremmo fare questo:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341"
  title="A T-Rex on display in the Manchester University Museum" />
```

Questo ci fornisce un tooltip al passaggio del mouse, proprio come i titoli dei collegamenti:

![L'immagine del dinosauro, con un titolo di tooltip sopra di esso che recita Un T-Rex in esposizione al Museo dell'Università di Manchester](image-with-title.png)

Tuttavia, ciò non è raccomandato — `title` ha un certo numero di problemi di accessibilità, principalmente basati sul fatto che il supporto del lettore di schermo è molto imprevedibile e la maggior parte dei browser non lo mostrerà a meno che non si stia passando con il mouse (quindi ad esempio, nessun accesso per gli utenti da tastiera). Se è interessato a ulteriori informazioni su questo, legga [Le Prove e Tribolazioni dell'Attributo Title](https://www.24a11y.com/2017/the-trials-and-tribulations-of-the-title-attribute/) di Scott O'Hara.

È meglio includere tali informazioni di supporto nel testo principale dell'articolo, piuttosto che allegarle all'immagine.

### Apprendimento attivo: incorporare un'immagine

Ora è il suo turno di giocare! Questa sezione di apprendimento attivo la farà esercitare con un esercizio di incorporamento. Lei dispone di un tag {{htmlelement("img")}} di base; vorremmo che lei incorpori l'immagine situata all'URL seguente:

```url
https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg
```

Abbiamo detto in precedenza di non fare mai hotlinking alle immagini su altri server, ma questo è solo a scopo didattico, quindi le lasceremo passare questa volta.

Vorremmo anche che lei:

- Aggiunga del testo alternativo, e verifichi che funzioni sbagliando intenzionalmente l'URL dell'immagine.
- Imposti la corretta `width` e `height` dell'immagine (suggerimento: è larga 200px e alta 171px), quindi sperimenti con altri valori per vedere che effetto hanno.
- Imposti un `title` sull'immagine.

Se commette un errore, può sempre ripristinarlo usando il pulsante _Reset_. Se si blocca davvero, prema il pulsante _Mostra soluzione_ per vedere una risposta:

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

## Asset multimediali e licenze

Le immagini (e altri tipi di asset multimediali) che trova sul web sono pubblicate sotto vari tipi di licenza. Prima di utilizzare un'immagine su un sito che sta costruendo, assicurarsi di possederla, di avere il permesso di usarla o di rispettare le condizioni di licenza del proprietario.

### Comprendere i tipi di licenza

Diamo un'occhiata ad alcune categorie comuni di licenze che è probabile trovare sul web.

#### Tutti i diritti riservati

I creatori di opere originali, come canzoni, libri o software, spesso rilasciano il loro lavoro sotto protezione del copyright chiusa. Ciò significa che, per impostazione predefinita, essi (o il loro editore) hanno diritti esclusivi di utilizzo (ad esempio, visualizzazione o distribuzione) del loro lavoro. Se vuole utilizzare immagini protette da copyright con una licenza _tutti i diritti riservati_, deve fare una delle seguenti cose:

- Ottenere il permesso esplicito e scritto dal detentore dei diritti d'autore.
- Pagare una tariffa di licenza per usarle. Questo può essere un costo una tantum per uso illimitato ("royalty-free") oppure potrebbe essere "rights-managed", nel qual caso potrebbe dover pagare tariffe specifiche per uso per fascia oraria, regione geografica, settore o tipo di media, ecc.
- Limitare i suoi utilizzi a quelli che sarebbero considerati [uso corretto](https://fairuse.stanford.edu/overview/fair-use/what-is-fair-use/) o [equo trattamento](https://copyrightservice.co.uk/copyright/p27_work_of_others) nella sua giurisdizione.

Gli autori non sono tenuti a includere un avviso di copyright o termini di licenza con il loro lavoro. Il copyright esiste automaticamente in un'opera originale di paternità una volta creata in un mezzo tangibile. Quindi, se trova un'immagine online e non ci sono avvisi di copyright o termini di licenza, il percorso più sicuro è assumere che sia protetta da copyright con tutti i diritti riservati.

#### Permessiva

Se l'immagine è rilasciata con una licenza permissiva, come [MIT](https://mit-license.org/), [BSD](https://opensource.org/license/BSD-3-clause), o una adatta [Creative Commons (CC) license](https://chooser-beta.creativecommons.org/), non c'è bisogno di pagare una tassa di licenza o cercare il permesso di usarla. Tuttavia, ci sono varie condizioni di licenza che dovrà soddisfare, che variano in base alla licenza.

Ad esempio, potrebbe dover:

- Fornire un link alla fonte originale dell'immagine e accreditare il suo creatore.
- Indicare se sono state apportate modifiche.
- Condividere qualsiasi opera derivata creata utilizzando l'immagine sotto la stessa licenza dell'originale.
- Non condividere opere derivate.
- Non usare l'immagine in nessun lavoro commerciale.
- Includere una copia della licenza insieme a qualsiasi pubblicazione che utilizza l'immagine.

Dovrebbe consultare la licenza applicabile per i termini specifici che dovrà seguire.

> [!NOTE]
> Potrebbe imbattersi nel termine "copyleft" nel contesto delle licenze permissive. Le licenze di tipo copyleft (come la [GNU General Public License (GPL)](https://www.gnu.org/licenses/gpl-3.0.en.html) o le licenze Creative Commons "Share Alike") specificano che le opere derivate devono essere rilasciate sotto la stessa licenza dell'originale.

Le licenze di tipo copyleft sono prominenti nel mondo del software. L'idea di base è che un nuovo progetto costruito con il codice di un progetto con licenza copyleft (questo è noto come "fork" del software originale) dovrà anche essere licenziato sotto la stessa licenza copyleft. Questo assicura che il codice sorgente del nuovo progetto sarà disponibile per altri da studiare e modificare. Si noti che, in generale, le licenze che sono state redatte per il software, come il GPL, non sono considerate buone licenze per opere non software poiché non sono state redatte tenendo conto delle opere non software.

Esplori i link forniti all'inizio di questa sezione per leggere sui diversi tipi di licenza e sui tipi di condizioni che specificano.

#### Pubblico dominio/CC0

Il lavoro rilasciato nel pubblico dominio a volte è indicato come "nessun diritto riservato" — nessun copyright si applica ad esso, e può essere usato senza permesso e senza dover soddisfare alcuna condizione di licenza. Un'opera può finire nel pubblico dominio in vari modi, come la scadenza del copyright o la specifica rinuncia ai diritti.

Uno dei modi più efficaci per collocare un'opera nel pubblico dominio è licenziarla sotto [CC0](https://creativecommons.org/public-domain/cc0/), una licenza creative commons specifica che fornisce uno strumento legale chiaro e inequivocabile per questo scopo.

Quando si utilizzano immagini del pubblico dominio, ottenere la prova che l'immagine si trova nel pubblico dominio e conservare tale prova per i suoi registri. Ad esempio, faccia uno screenshot della fonte originale con lo stato di licenza chiaramente mostrato e consideri di aggiungere una pagina al suo sito web con un elenco delle immagini acquisite insieme ai loro requisiti di licenza.

### Ricerca di immagini con licenza permissiva

Può trovare immagini con licenza permissiva per i suoi progetti utilizzando un motore di ricerca di immagini o direttamente da repository di immagini.

Ricerchi immagini usando una descrizione dell'immagine che sta cercando insieme a termini di licenza rilevanti. Ad esempio, quando cerca "dinosauro giallo", aggiunga "immagini di pubblico dominio", "libreria di immagini di pubblico dominio", "immagini con licenza aperta" o termini simili nella query di ricerca.

Alcuni motori di ricerca hanno strumenti per aiutarla a trovare immagini con licenze permissive. Ad esempio, quando usa Google, vada alla scheda "Immagini" per cercare immagini, quindi clicchi su "Strumenti". C'è un menu a discesa "Diritti di utilizzo" nella barra degli strumenti risultante dove può scegliere di cercare specificamente immagini sotto licenze creative commons.

I siti di repository di immagini, come [Flickr](https://flickr.com/), [ShutterStock](https://www.shutterstock.com/), e [Pixabay](https://pixabay.com/), hanno opzioni di ricerca per consentirle di cercare solo immagini con licenza permissiva. Alcuni siti distribuiscono esclusivamente immagini e icone con licenza permissiva, come [Picryl](https://picryl.com/) e [The Noun Project](https://thenounproject.com/).

Conformarsi alla licenza sotto cui l'immagine è stata rilasciata è una questione di trovare i dettagli della licenza, leggere la pagina della licenza o le istruzioni fornite dalla fonte e quindi seguire quelle istruzioni. I repository di immagini affidabili rendono chiare e facili da trovare le loro condizioni di licenza.

## Annotare le immagini con figure e didascalie

Parlando di didascalie, ci sono diversi modi in cui potrebbe aggiungere una didascalia per accompagnare la sua immagine. Ad esempio, non ci sarebbe nulla che le impedisca di fare questo:

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

Questo va bene. Contiene il contenuto di cui ha bisogno, ed è facilmente stilizzabile usando CSS. Ma c'è un problema qui: non c'è nulla che colleghi semanticamente l'immagine alla sua didascalia, il che può causare problemi ai lettori di schermo. Ad esempio, quando ha 50 immagini e didascalie, quale didascalia si riferisce a quale immagine?

Una soluzione migliore è utilizzare gli elementi HTML {{htmlelement("figure")}} e {{htmlelement("figcaption")}}. Questi sono stati creati proprio per questo scopo: fornire un contenitore semantico per figure e collegare chiaramente la figura alla didascalia. Il nostro esempio sopra potrebbe essere riscritto così:

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

L'elemento {{htmlelement("figcaption")}} indica ai browser e alle tecnologie assistive che la didascalia descrive l'altro contenuto dell'elemento {{htmlelement("figure")}}.

> [!NOTE]
> Dal punto di vista dell'accessibilità, le didascalie e [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) hanno ruoli distinti. Le didascalie beneficiano anche le persone che possono vedere l'immagine, mentre [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) fornisce la stessa funzionalità come un'immagine assente. Pertanto, le didascalie e il testo `alt` non dovrebbero dire la stessa cosa, poiché appaiono entrambi quando l'immagine non è visibile. Provi a disattivare le immagini nel suo browser e veda come appare.

Una figura non deve essere un'immagine. È un'unità indipendente di contenuto che:

- Esprime il suo significato in un modo compatto e facile da comprendere.
- Potrebbe andare in diversi punti nel flusso lineare della pagina.
- Fornisce informazioni essenziali a sostegno del testo principale.

Una figura potrebbe essere diverse immagini, uno snippet di codice, audio, video, equazioni, una tabella o qualcos'altro.

### Apprendimento attivo: creare una figura

In questa sezione di apprendimento attivo, vorremmo che prendesse il codice finale dalla precedente sezione di apprendimento attivo e lo trasformasse in una figura:

1. Racchiuda il tutto in un elemento {{htmlelement("figure")}}.
2. Copi il testo dall'attributo `title`, rimuova l'attributo `title` e metta il testo all'interno di un elemento {{htmlelement("figcaption")}} sotto l'immagine.

Se commette un errore, può sempre ripristinarlo usando il pulsante _Reset_. Se si blocca davvero, prema il pulsante _Mostra soluzione_ per vedere una risposta:

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

Può anche utilizzare CSS per incorporare immagini nelle pagine web (e JavaScript, ma questa è un'altra storia completamente). La proprietà CSS {{cssxref("background-image")}}, e le altre proprietà `background-*`, sono utilizzate per controllare il posizionamento dell'immagine di sfondo. Ad esempio, per collocare un'immagine di sfondo su ogni paragrafo di una pagina, potrebbe fare questo:

```css
p {
  background-image: url("images/dinosaur.jpg");
}
```

L'immagine incorporata risultante è indubbiamente più facile da posizionare e controllare rispetto alle immagini HTML. Quindi perché preoccuparsi delle immagini HTML? Come accennato in precedenza, le immagini di sfondo CSS sono solo per decorazione. Se vuole semplicemente aggiungere qualcosa di bello alla sua pagina per migliorare l'aspetto visivo, va bene. Tuttavia, tali immagini non hanno alcun significato semantico. Non possono avere equivalenti testuali, sono invisibili ai lettori di schermo e così via. Questo è il punto in cui le immagini HTML brillano!

Riassumendo: se un'immagine ha un significato, in termini di suo contenuto, dovrebbe utilizzare un'immagine HTML. Se un'immagine è puramente decorativa, dovrebbe utilizzare immagini di sfondo CSS (tratteremo queste in dettaglio successivamente nei moduli Core).

## Testi le sue capacità!

Ha raggiunto la fine di questo articolo, ma può ricordarsi le informazioni più importanti? Può trovare alcuni ulteriori test per verificare che abbia mantenuto queste informazioni prima di procedere — vedere [Testi le sue capacità: Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Images).

## Sommario

Questo è tutto per ora. Abbiamo trattato immagini e didascalie in dettaglio. Nel prossimo articolo, andremo a un livello superiore, esaminando come utilizzare HTML per incorporare contenuti video e audio nelle pagine web.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}
