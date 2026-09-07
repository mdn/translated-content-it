---
title: Immagini HTML
short-title: Images
slug: Learn_web_development/Core/Structuring_content/HTML_images
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/Test_your_skills/Images", "Learn_web_development/Core/Structuring_content")}}

All'inizio, il web era costituito solo da testo ed era davvero piuttosto noioso. Fortunatamente, non passò molto tempo prima che venisse aggiunta la possibilità di incorporare immagini (e altri tipi di contenuto più interessanti) nelle pagine web. In questo articolo verrà esaminato in dettaglio come usare l'elemento {{htmlelement("img")}}, incluse le basi, l'aggiunta di didascalie tramite {{htmlelement("figure")}} e il rapporto con le immagini di sfondo in {{Glossary("CSS", "CSS")}}.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo, come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > ed <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Il termine "replaced element": che cosa significa?</li>
          <li>Sintassi di base del tag <code>&lt;img&gt;</code></li>
          <li>Uso di <code>src</code> per puntare a una risorsa.</li>
          <li>Uso di <code>width</code> e <code>height</code>, ad esempio per evitare spiacevoli aggiornamenti bruschi dell'interfaccia utente quando un'immagine ha terminato il caricamento ed è visualizzata.</li>
          <li>Ottimizzazione delle risorse multimediali per il web: mantenere ridotte le dimensioni dei file.</li>
          <li>Comprensione delle licenze delle risorse multimediali: diversi tipi di licenza, come rispettarli e come cercare file multimediali con licenza appropriata da usare nei progetti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Come inserire un'immagine in una pagina web?

Per inserire un'immagine in una pagina web, si usa l'elemento {{htmlelement("img")}}. Si tratta di un {{Glossary("void_element", "elemento vuoto")}} (ovvero, non può avere contenuto figlio e non può avere un tag di chiusura) che richiede due attributi per essere utile: `src` e `alt`. L'attributo `src` contiene un URL che punta all'immagine che si desidera incorporare nella pagina. Come per l'attributo `href` degli elementi {{htmlelement("a")}}, l'attributo `src` può essere un URL relativo o un URL assoluto. Senza un attributo `src`, un elemento `img` non ha alcuna immagine da caricare.

L'[attributo `alt` è descritto di seguito](#testo_alternativo).

> [!NOTE]
> Prima di proseguire, è consigliabile leggere [Una rapida introduzione a URL e percorsi](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#a_quick_primer_on_urls_and_paths) per ripassare gli URL relativi e assoluti.

Ad esempio, se l'immagine si chiama `dinosaur.jpg` e si trova nella stessa directory della pagina HTML, è possibile incorporarla in questo modo:

```html
<img src="dinosaur.jpg" alt="Dinosaur" />
```

Se l'immagine si trovasse in una sottodirectory `images`, all'interno della stessa directory della pagina HTML, verrebbe incorporata così:

```html
<img src="images/dinosaur.jpg" alt="Dinosaur" />
```

E così via.

> [!NOTE]
> Anche i motori di ricerca leggono i nomi dei file delle immagini e li considerano ai fini della SEO. È quindi opportuno assegnare all'immagine un nome file descrittivo; `dinosaur.jpg` è meglio di `img835.png`.

È anche possibile incorporare l'immagine usando il suo URL assoluto, ad esempio:

```html
<img src="https://www.example.com/images/dinosaur.jpg" alt="Dinosaur" />
```

Tuttavia, non è consigliabile creare collegamenti tramite URL assoluti. Le immagini da utilizzare dovrebbero essere ospitate sul proprio sito, il che, nelle configurazioni semplici, significa mantenere le immagini del sito web sullo stesso server dell'HTML. Inoltre, dal punto di vista della manutenzione, è più efficiente usare URL relativi anziché assoluti (quando il sito viene spostato in un dominio diverso, non sarà necessario aggiornare tutti gli URL includendo il nuovo dominio). Nelle configurazioni più avanzate, potrebbe essere usata una {{Glossary("CDN", "CDN (Content Delivery Network)")}} per distribuire le immagini.

Se le immagini non sono state create direttamente, occorre assicurarsi di avere il permesso di usarle secondo le condizioni della licenza con cui sono pubblicate (per maggiori informazioni, vedere [Risorse multimediali e licenze](#risorse_multimediali_e_licenze) di seguito).

> [!WARNING]
> Non impostare _mai_ l'attributo `src` su un'immagine ospitata sul sito web di qualcun altro _senza autorizzazione_. Questa pratica è chiamata "hotlinking". È considerata poco etica, poiché qualcun altro pagherebbe i costi di larghezza di banda per distribuire l'immagine quando qualcuno visita la pagina. Inoltre, non si avrebbe alcun controllo sull'eventuale rimozione dell'immagine o sulla sua sostituzione con qualcosa di imbarazzante.

Il precedente frammento di codice, sia con l'URL assoluto sia con quello relativo, produrrà il seguente risultato:

![Immagine di base di un dinosauro, incorporata in un browser, con la scritta "Images in HTML" sopra di essa](basic-image.png)

> [!NOTE]
> Elementi come {{htmlelement("img")}} e {{htmlelement("video")}} sono talvolta indicati come **replaced elements**. Questo perché il contenuto e le dimensioni dell'elemento sono definiti da una risorsa esterna, come un file immagine o video, e non dal contenuto dell'elemento stesso. Per ulteriori informazioni, vedere {{Glossary("replaced_elements", "replaced elements")}}.

### Testo alternativo

L'attributo successivo da esaminare è `alt`. Il suo valore dovrebbe essere una descrizione testuale dell'immagine, da usare nelle situazioni in cui l'immagine non può essere vista/visualizzata oppure richiede molto tempo per il rendering a causa di una connessione Internet lenta. Ad esempio, il codice precedente potrebbe essere modificato così:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth" />
```

Il modo più semplice per testare il testo `alt` consiste nello scrivere intenzionalmente in modo errato il nome del file. Se, ad esempio, il nome dell'immagine fosse scritto `dinosooooor.jpg`, il browser non visualizzerebbe l'immagine e mostrerebbe invece il testo alternativo:

![Il titolo Images in HTML, ma questa volta l'immagine del dinosauro non è visualizzata e al suo posto compare il testo alternativo.](alt-text.png)

Perché potrebbe essere necessario visualizzare o usare il testo alternativo? Può risultare utile per diversi motivi:

- L'utente ha una disabilità visiva e usa un [lettore di schermo](https://en.wikipedia.org/wiki/Screen_reader) per leggere il web. In effetti, disporre di testo alternativo per descrivere le immagini è utile alla maggior parte degli utenti.
- Come descritto sopra, l'ortografia del nome del file o del percorso potrebbe essere errata.
- Il browser non supporta il tipo di immagine. Alcune persone usano ancora browser di solo testo, come [Lynx](https://en.wikipedia.org/wiki/Lynx_%28web_browser%29), che visualizza il testo alternativo delle immagini.
- Potrebbe essere necessario fornire testo che i motori di ricerca possano utilizzare; ad esempio, i motori di ricerca possono confrontare il testo alternativo con le query di ricerca.
- Gli utenti hanno disattivato le immagini per ridurre il volume di trasferimento dati e le distrazioni. Questo è particolarmente comune sui telefoni cellulari e nei Paesi in cui la larghezza di banda è limitata o costosa.

Che cosa si dovrebbe scrivere esattamente nell'attributo `alt`? Dipende dal _motivo_ per cui l'immagine è presente. In altre parole, da ciò che andrebbe perso se l'immagine non venisse visualizzata:

- **Decorazione.** Per le immagini decorative dovrebbero essere usate le [immagini di sfondo CSS](#immagini_di_sfondo_css), ma se è necessario usare HTML, aggiungere un `alt=""` vuoto. Se l'immagine non fa parte del contenuto, un lettore di schermo non dovrebbe perdere tempo a leggerla.
- **Contenuto.** Se l'immagine fornisce informazioni significative, fornire le stesse informazioni in un testo `alt` _breve_ oppure, ancora meglio, nel testo principale che tutti possono vedere. Non scrivere testo `alt` ridondante. Quanto sarebbe fastidioso per un utente vedente se tutti i paragrafi fossero scritti due volte nel contenuto principale? Se l'immagine è descritta adeguatamente dal corpo del testo principale, è sufficiente usare `alt=""`.
- **Collegamento.** Se un'immagine viene inserita all'interno di tag {{htmlelement("a")}} per trasformarla in un collegamento, è comunque necessario fornire un [testo del collegamento accessibile](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#use_clear_link_wording). In questi casi, il testo può essere scritto nello stesso elemento `<a>` oppure nell'attributo `alt` dell'immagine, a seconda di quale soluzione funzioni meglio nel caso specifico.
- **Testo.** Il testo non dovrebbe essere inserito nelle immagini. Se il titolo principale necessita, ad esempio, di un'ombra esterna, è preferibile [usare CSS](/it/docs/Web/CSS/Reference/Properties/text-shadow) anziché inserire il testo in un'immagine. Tuttavia, se _davvero non è possibile evitarlo_, il testo dovrebbe essere fornito nell'attributo `alt`.

In sostanza, la chiave è offrire un'esperienza utilizzabile anche quando le immagini non possono essere viste. Questo garantisce che nessun utente perda parte del contenuto. Provare a disattivare le immagini nel browser e osservare il risultato. Diventerà subito chiaro quanto sia utile il testo alternativo quando l'immagine non può essere visualizzata.

> [!NOTE]
> Consultare la nostra guida alle [alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) e [Un albero decisionale per `alt`](https://www.w3.org/WAI/tutorials/images/decision-tree/) per imparare a usare un attributo `alt` per le immagini in varie situazioni.

> [!NOTE]
> [Tag HTML](https://scrimba.com/html-css-crash-course-c02l/~0d?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba è una lezione interattiva che fornisce informazioni sulle immagini e mini-sfide.

### Larghezza e altezza

È possibile usare gli attributi [`width`](/it/docs/Web/HTML/Reference/Elements/img#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/img#height) per specificare la larghezza e l'altezza dell'immagine. Vengono forniti come numeri interi senza unità e rappresentano la larghezza e l'altezza dell'immagine in pixel.

La larghezza e l'altezza dell'immagine possono essere individuate in vari modi. Ad esempio, su Mac è possibile usare <kbd>Cmd</kbd> + <kbd>I</kbd> per ottenere le informazioni di visualizzazione del file immagine. Tornando all'esempio, si potrebbe fare così:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341" />
```

C'è un ottimo motivo per farlo. L'HTML della pagina e l'immagine sono risorse separate, recuperate dal browser come richieste HTTP(S) separate. Non appena il browser riceve l'HTML, inizia a visualizzarlo per l'utente. Se le immagini non sono ancora state ricevute, situazione che spesso si verifica poiché le dimensioni dei file immagine sono spesso molto maggiori di quelle dei file HTML, il browser eseguirà il rendering solo dell'HTML e aggiornerà la pagina con l'immagine non appena questa verrà ricevuta.

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

Non appena il browser scarica l'HTML, inizierà a visualizzare la pagina.

Una volta caricata l'immagine, il browser la aggiunge alla pagina. Poiché l'immagine occupa spazio, il browser deve spostare il testo più in basso nella pagina per inserirvi l'immagine sopra di esso:

![Confronto del layout della pagina mentre il browser carica una pagina e quando ha terminato, senza dimensioni specificate per l'immagine.](no-size.png)

Spostare il testo in questo modo distrae moltissimo gli utenti, specialmente se hanno già iniziato a leggerlo, e causa inoltre un nuovo rendering della pagina da parte del browser, il che è negativo per le prestazioni.

Se si specificano le dimensioni effettive dell'immagine nell'HTML usando gli attributi `width` e `height`, il browser sa quanto spazio riservare all'immagine prima di averla scaricata.

Ciò significa che, quando l'immagine è stata scaricata, il browser non deve spostare il contenuto circostante.

![Confronto del layout della pagina mentre il browser carica una pagina e quando ha terminato, quando le dimensioni dell'immagine sono specificate.](size.png)

Per un eccellente articolo sulla storia di questa funzionalità, vedere [Setting height and width on images is important again](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/).

Tenere presente che se non c'è contenuto sotto l'immagine, il nuovo rendering non è un problema perché il ridimensionamento dell'immagine non causerà lo spostamento di altri elementi. In tal caso, è possibile impostare solo `width` dell'immagine. Se si imposta `width` ma non `height`, `height` assume per impostazione predefinita il valore `auto`, il che significa che viene impostato su un valore che mantiene le {{Glossary("Aspect_ratio", "proporzioni")}} dell'immagine.

#### Ridimensionare le immagini

Sebbene, come detto, sia buona pratica specificare la dimensione _effettiva_ delle immagini usando attributi HTML, non dovrebbero essere usati per _ridimensionare_ le immagini.

Se si imposta una dimensione troppo grande per l'immagine, si otterranno immagini dall'aspetto granuloso, sfocato o troppo piccolo, oltre a sprecare larghezza di banda scaricando un'immagine non adatta alle esigenze dell'utente. L'immagine potrebbe anche apparire distorta se non vengono mantenute le corrette {{Glossary("aspect_ratio", "proporzioni")}}. Prima di inserire un'immagine nella pagina web, occorre usare un editor di immagini per impostarla alla dimensione corretta.

Se è necessario modificare le dimensioni di un'immagine, è preferibile usare [CSS](/it/docs/Learn_web_development/Core/Styling_basics).

### Titoli delle immagini

Come [per i collegamenti](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#adding_supporting_information_with_the_title_attribute), è possibile aggiungere attributi `title` alle immagini per fornire ulteriori informazioni di supporto, se necessario. Nell'esempio, si potrebbe fare così:

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth"
  width="400"
  height="341"
  title="A T-Rex on display in the Manchester University Museum" />
```

Questo produce un tooltip al passaggio del mouse, proprio come i titoli dei collegamenti:

![L'immagine del dinosauro, con un tooltip sopra di essa che recita A T-Rex on display at the Manchester University Museum](image-with-title.png)

Tuttavia, questo non è consigliato: `title` presenta diversi problemi di accessibilità, dovuti principalmente al fatto che il supporto dei lettori di schermo è molto imprevedibile e che la maggior parte dei browser non lo mostra se non al passaggio del mouse, quindi, ad esempio, gli utenti della tastiera non vi hanno accesso. Per ulteriori informazioni al riguardo, leggere [The Trials and Tribulations of the Title Attribute](https://www.24a11y.com/2017/the-trials-and-tribulations-of-the-title-attribute/) di Scott O'Hara.

È preferibile includere tali informazioni di supporto nel testo principale dell'articolo anziché associarle all'immagine.

### Esercitazione sull'incorporamento di immagini

Ora è il momento di fare pratica. Questa attività richiede di incorporare un'immagine.

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nell'MDN Playground.
2. Modificare l'elemento {{htmlelement("img")}} esistente affinché incorpori l'immagine `dinosaur_small.jpg`.
3. Aggiungere un attributo `alt` all'immagine. È possibile verificare che il testo alternativo funzioni scrivendo temporaneamente in modo errato il nome del file dell'immagine.
4. Impostare `width` e `height` corretti dell'immagine (suggerimento: è larga `200px` e alta `171px`), quindi sperimentare altri valori per vedere quale effetto producono.
5. Impostare un `title` sull'immagine.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nell'MDN Playground. Se si è davvero bloccati, è possibile visualizzare la soluzione sotto il blocco di codice.

```html live-sample___images-1
<img />
```

{{ EmbedLiveSample('images-1', "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile al seguente:

```html
<img
  src="dinosaur_small.jpg"
  alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"
  width="200"
  height="171"
  title="A T-Rex on display in the Manchester University Museum" />
```

</details>

## Risorse multimediali e licenze

Le immagini, e altri tipi di risorse multimediali, trovate sul web vengono pubblicate con vari tipi di licenza. Prima di usare un'immagine in un sito in fase di sviluppo, assicurarsi di possederla, di avere il permesso di usarla o di rispettare le condizioni di licenza del proprietario.

### Comprendere i tipi di licenza

Esaminiamo alcune categorie comuni di licenze che probabilmente si incontreranno sul web.

#### Tutti i diritti riservati

I creatori di opere originali come canzoni, libri o software pubblicano spesso le loro opere con protezione del copyright chiusa. Ciò significa che, per impostazione predefinita, essi, o il loro editore, hanno diritti esclusivi sull'uso, ad esempio la visualizzazione o la distribuzione, delle loro opere. Per usare immagini protette da copyright con una licenza _tutti i diritti riservati_, è necessario effettuare una delle seguenti operazioni:

- Ottenere un'autorizzazione scritta esplicita dal titolare del copyright.
- Pagare una tariffa di licenza per usarle. Può trattarsi di una tariffa una tantum per un uso illimitato ("royalty-free") oppure di una licenza "rights-managed", nel qual caso potrebbe essere necessario pagare tariffe specifiche per ogni uso in base a fascia oraria, regione geografica, settore o tipo di mezzo, e così via.
- Limitare gli usi a quelli che sarebbero considerati [fair use](https://fairuse.stanford.edu/overview/fair-use/what-is-fair-use/) o [fair dealing](https://copyrightservice.co.uk/copyright/p27_work_of_others) nella propria giurisdizione.

Gli autori non sono obbligati a includere un avviso di copyright o i termini di licenza nelle loro opere. Il copyright esiste automaticamente per un'opera originale di autore una volta creata in un supporto tangibile. Pertanto, se si trova un'immagine online senza avvisi di copyright o termini di licenza, l'approccio più sicuro è presumere che sia protetta da copyright con tutti i diritti riservati.

#### Permissive

Se l'immagine viene pubblicata con una licenza permissiva, come [MIT](https://mit-license.org/), [BSD](https://opensource.org/license/BSD-3-clause) o una licenza [Creative Commons (CC)](https://creativecommons.org/chooser/) adatta, non è necessario pagare una tariffa di licenza né richiedere autorizzazione per usarla. Restano comunque varie condizioni di licenza da rispettare, che variano in base alla licenza.

Ad esempio, potrebbe essere necessario:

- Fornire un collegamento alla fonte originale dell'immagine e attribuirne la creazione al suo autore.
- Indicare se sono state apportate modifiche.
- Condividere le opere derivate create usando l'immagine con la stessa licenza dell'originale.
- Non condividere affatto opere derivate.
- Non usare l'immagine in opere commerciali.
- Includere una copia della licenza insieme a ogni pubblicazione che usa l'immagine.

Occorre consultare la licenza applicabile per conoscere i termini specifici da seguire.

> [!NOTE]
> Nel contesto delle licenze permissive può comparire il termine "copyleft". Le licenze copyleft, come la [GNU General Public License (GPL)](https://www.gnu.org/licenses/gpl-3.0.en.html) o le licenze Creative Commons "Share Alike", stabiliscono che le opere derivate devono essere pubblicate con la stessa licenza dell'originale.

Le licenze copyleft sono diffuse nel mondo del software. L'idea di base è che un nuovo progetto creato con il codice di un progetto con licenza copyleft, noto come "fork" del software originale, debba anch'esso essere concesso in licenza con la medesima licenza copyleft. Ciò garantisce che il codice sorgente del nuovo progetto sia reso disponibile anche ad altri per essere studiato e modificato. Si noti che, in generale, le licenze redatte per il software, come la GPL, non sono considerate buone licenze per opere non software poiché non sono state concepite tenendo conto delle opere non software.

Esplorare i collegamenti forniti in precedenza in questa sezione per leggere i diversi tipi di licenza e i tipi di condizioni che specificano.

#### Pubblico dominio/CC0

Le opere pubblicate nel pubblico dominio sono talvolta definite "nessun diritto riservato": non si applica alcun copyright e possono essere utilizzate senza autorizzazione e senza dover rispettare alcuna condizione di licenza. Un'opera può entrare nel pubblico dominio in vari modi, ad esempio per la scadenza del copyright o per una rinuncia specifica ai diritti.

Uno dei modi più efficaci per collocare un'opera nel pubblico dominio consiste nel concederla in licenza con [CC0](https://wiki.creativecommons.org/wiki/CC0), una specifica licenza Creative Commons che fornisce uno strumento giuridico chiaro e inequivocabile per questo scopo.

Quando si usano immagini di pubblico dominio, ottenere una prova che l'immagine appartenga al pubblico dominio e conservarla nei propri archivi. Ad esempio, acquisire una schermata della fonte originale con lo stato della licenza chiaramente visualizzato e considerare l'aggiunta al sito web di una pagina con un elenco delle immagini acquisite e dei relativi requisiti di licenza.

### Cercare immagini con licenza permissiva

È possibile trovare immagini con licenza permissiva per i progetti usando un motore di ricerca di immagini oppure direttamente nei repository di immagini.

Cercare immagini usando una descrizione dell'immagine desiderata insieme a termini di licenza pertinenti. Ad esempio, quando si cerca "yellow dinosaur", aggiungere alla query di ricerca "public domain images", "public domain image library", "open licensed images" o termini simili.

Alcuni motori di ricerca dispongono di strumenti per aiutare a trovare immagini con licenze permissive. Ad esempio, usando Google, accedere alla scheda "Immagini" per cercare immagini, quindi fare clic su "Strumenti". Nella barra degli strumenti risultante è presente un menu a discesa "Diritti di utilizzo", in cui è possibile scegliere di cercare specificamente immagini con licenze Creative Commons.

Siti repository di immagini come [Flickr](https://flickr.com/), [ShutterStock](https://www.shutterstock.com/) e [Pixabay](https://pixabay.com/) dispongono di opzioni di ricerca che permettono di cercare solo immagini con licenza permissiva. Alcuni siti distribuiscono esclusivamente immagini e icone con licenza permissiva, come [Picryl](https://picryl.com/) e [The Noun Project](https://thenounproject.com/).

Rispettare la licenza con cui è stata pubblicata un'immagine significa individuare i dettagli della licenza, leggere la licenza o la pagina di istruzioni fornita dalla fonte e quindi seguire tali istruzioni. I repository di immagini affidabili rendono le condizioni di licenza chiare e facili da trovare.

## Annotare immagini con figure e didascalie

A proposito di didascalie, esistono vari modi per aggiungere una didascalia a un'immagine. Ad esempio, nulla impedirebbe di fare quanto segue:

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

Va bene. Contiene il contenuto necessario ed è facilmente stilizzabile con CSS. Tuttavia, c'è un problema: non c'è nulla che colleghi semanticamente l'immagine alla sua didascalia, il che può causare problemi ai lettori di schermo. Ad esempio, quando ci sono 50 immagini e didascalie, quale didascalia appartiene a quale immagine?

Una soluzione migliore consiste nell'usare gli elementi HTML {{htmlelement("figure")}} e {{htmlelement("figcaption")}}. Sono stati creati esattamente per questo scopo: fornire un contenitore semantico per le figure e collegare chiaramente la figura alla didascalia. L'esempio precedente potrebbe essere riscritto così:

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

L'elemento {{htmlelement("figcaption")}} comunica ai browser e alle tecnologie assistive che la didascalia descrive l'altro contenuto dell'elemento {{htmlelement("figure")}}.

> [!NOTE]
> Dal punto di vista dell'accessibilità, le didascalie e il testo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) hanno ruoli distinti. Le didascalie sono utili anche per chi può vedere l'immagine, mentre il testo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt) fornisce la stessa funzionalità di un'immagine assente. Pertanto, le didascalie e il testo `alt` non dovrebbero dire semplicemente la stessa cosa, perché entrambi vengono visualizzati quando l'immagine non è presente. Provare a disattivare le immagini nel browser e osservare il risultato.

Una figura non deve necessariamente essere un'immagine. È un'unità di contenuto indipendente che:

- Esprime il significato in modo conciso e facilmente comprensibile.
- Potrebbe trovarsi in diversi punti del flusso lineare della pagina.
- Fornisce informazioni essenziali a supporto del testo principale.

Una figura potrebbe essere costituita da più immagini, un frammento di codice, audio, video, equazioni, una tabella o altro.

### Creare una figura

In questa attività, si deve usare come punto di partenza il codice completato nell'attività precedente e trasformarlo in una figura:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nell'MDN Playground.
2. Racchiudere l'elemento `<img>` in un elemento {{htmlelement("figure")}}.
3. Copiare il testo dall'attributo `title`, inserirlo in un elemento {{htmlelement("figcaption")}} sotto l'elemento `<img>`, quindi rimuovere l'attributo `title`.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nell'MDN Playground. Se si è davvero bloccati, è possibile visualizzare la soluzione sotto il blocco di codice.

```html live-sample___images-2
<img
  src="dinosaur_small.jpg"
  alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"
  width="200"
  height="171"
  title="A T-Rex on display in the Manchester University Museum" />
```

{{ EmbedLiveSample('images-2', "100%", 200) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html
<figure>
  <img
    src="dinosaur_small.jpg"
    alt="The head and torso of a dinosaur skeleton; it has a large head with long sharp teeth"
    width="200"
    height="171" />
  <figcaption>
    A T-Rex on display in the Manchester University Museum
  </figcaption>
</figure>
```

</details>

## Immagini di sfondo CSS

È possibile usare CSS per incorporare immagini nelle pagine web, e anche JavaScript, ma questa è tutta un'altra storia. La proprietà CSS {{cssxref("background-image")}} e le altre proprietà `background-*` vengono usate per controllare il posizionamento delle immagini di sfondo. Ad esempio, per inserire un'immagine di sfondo in ogni paragrafo di una pagina, si potrebbe fare così:

```css
p {
  background-image: url("images/dinosaur.jpg");
}
```

L'immagine incorporata risultante è probabilmente più facile da posizionare e controllare rispetto alle immagini HTML. Perché allora usare le immagini HTML? Come anticipato sopra, le immagini di sfondo CSS sono solo decorative. Se si desidera semplicemente aggiungere qualcosa di gradevole alla pagina per migliorarne l'aspetto visivo, va bene. Tuttavia, tali immagini non hanno alcun significato semantico. Non possono avere equivalenti testuali, sono invisibili ai lettori di schermo e così via. È qui che le immagini HTML eccellono.

In sintesi: se un'immagine ha significato in termini di contenuto, dovrebbe essere usata un'immagine HTML. Se un'immagine è puramente decorativa, dovrebbero essere usate immagini di sfondo CSS, che verranno trattate in dettaglio più avanti nei moduli Core.

## Riepilogo

Per ora è tutto. Sono state trattate in dettaglio immagini e didascalie.

Nel prossimo articolo verranno proposti alcuni test per verificare quanto bene sono state comprese e memorizzate le informazioni fornite sulle immagini HTML.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content/Test_your_skills/Images", "Learn_web_development/Core/Structuring_content")}}
