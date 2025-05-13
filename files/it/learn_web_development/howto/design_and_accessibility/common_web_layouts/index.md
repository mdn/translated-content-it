---
title: Cosa contengono i layout comuni dei siti web?
slug: Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Quando si progettano le pagine del proprio sito web, è utile avere un'idea dei layout più comuni.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Assicurarsi di aver già riflettuto su
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/Thinking_before_coding"
          >cosa si desidera ottenere</a
        >
        con il proprio progetto web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove posizionare gli elementi sulle pagine web e come posarli.
      </td>
    </tr>
  </tbody>
</table>

## Riassunto

C'è una ragione per cui parliamo di design web. Si parte da una pagina bianca e si può procedere in molte direzioni. E se non si ha molta esperienza, partire da una pagina bianca può essere un po' spaventoso. Abbiamo oltre 25 anni di esperienza e vi forniremo alcune regole pratiche comuni per aiutarvi a progettare il vostro sito.

Anche ora, con il nuovo focus sul Web mobile, quasi tutte le pagine web principali sono costruite da queste parti:

- Header
  - : Visibile in cima a ogni pagina del sito. Contiene informazioni rilevanti per tutte le pagine (come il nome del sito o il logo) e un sistema di navigazione facile da usare.
- Contenuto principale
  - : La regione più grande, che contiene contenuti unici per la pagina corrente.
- Contenuti laterali
  - : 1) Informazioni che completano il contenuto principale; 2) informazioni condivise tra un sottoinsieme di pagine; 3) sistema di navigazione alternativo. In effetti, tutto ciò che non è assolutamente richiesto dal contenuto principale della pagina.
- Footer
  - : Visibile in fondo a ogni pagina del sito. Come l'header, contiene informazioni globali meno prominenti come avvisi legali o informazioni di contatto.

Questi elementi sono piuttosto comuni in tutte le forme, ma possono essere disposti in modi diversi. Ecco alcuni esempi (**1** rappresenta l'header, **2** il footer; **A** il contenuto principale; **B1, B2** i contenuti laterali):

**Layout a 1 colonna**. Particolarmente importante per i browser mobili per non ingombrare lo schermo ridotto.

![Esempio di layout a 1 colonna: Contenuto principale in alto e contenuti laterali impilati sotto di esso.](1-col-layout.png)

**Layout a 2 colonne**. Spesso utilizzato per i tablet, poiché hanno schermi di medio formato.

![Esempio di layout base a 2 colonne: Un contenuto laterale nella colonna di sinistra e il contenuto principale nella colonna di destra.](2-col-layout-right.png) ![Esempio di layout base a 2 colonne: Un contenuto laterale nella colonna di destra e il contenuto principale nella colonna di sinistra.](2-col-layout-left.png)

**Layout a 3 colonne**. Adatto solo per desktop con schermi grandi. (Anche molti utenti desktop preferiscono visualizzare le cose in finestre piccole anziché a schermo intero.)

![Esempio di layout base a 3 colonne: Contenuto laterale nelle colonne di sinistra e destra, Contenuto principale nella colonna centrale.](3-col-layout.png) ![Un altro esempio di layout a 3 colonne: Contenuti laterali affiancati a sinistra, Contenuto principale nella colonna di destra.](3-col-layout-alt.png) ![Un altro esempio di layout a 3 colonne: Contenuti laterali affiancati a destra, Contenuto principale nella colonna di sinistra.](3-col-layout-alt2.png)

Il vero divertimento inizia quando si inizia a mescolare tutti questi layout:

![Esempio di layout misto: Contenuto principale in alto e contenuti laterali sotto di esso affiancati.](1-col-layout-alt.png) ![Esempio di un layout misto: Contenuto principale nella colonna di sinistra e contenuti laterali impilati uno sopra l'altro nella colonna di destra.](2-col-layout-left-alt.png) ![Esempio di un layout misto: un contenuto laterale nella colonna di sinistra e il contenuto principale nella colonna di destra con un contenuto laterale sotto il contenuto principale.](2-col-layout-mix.png) ![Esempio di un layout misto: Contenuto principale a sinistra della prima fila e un contenuto laterale a destra della stessa fila, un secondo contenuto laterale che copre l'intera seconda fila.](2-col-layout-mix-alt.png)…

Questi sono solo esempi e siete abbastanza liberi di disporre gli elementi come desiderate. Potreste notare che, mentre i contenuti possono muoversi sullo schermo, teniamo sempre l'header (1) in alto e il footer (2) in basso. Inoltre, il contenuto principale (A) è quello che conta di più, quindi dategli la maggior parte dello spazio.

Queste sono regole pratiche su cui potete basarvi. Esistono, naturalmente, progetti complessi ed eccezioni. In altri articoli discuteremo come progettare siti responsivi (siti che cambiano a seconda delle dimensioni dello schermo) e siti i cui layout variano tra le pagine. Per ora, è meglio mantenere il vostro layout coerente in tutto il vostro sito.

## Apprendimento attivo

_Non è ancora disponibile un apprendimento attivo. [Per favore, considerate di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimento

Studiamo alcuni esempi più concreti tratti da siti web ben noti.

### Layout a una colonna

Un tipico layout a una colonna che fornisce tutte le informazioni linearmente su una pagina.

![Esempio di layout a 1 colonna nel mondo reale](screenshot-product.jpg) ![Layout a 1 colonna con header, contenuto principale, una pila di contenuti laterali e un footer](screenshot-product-overlay.jpg)

Piuttosto diretto. Ricordate solo, molte persone continueranno a navigare il vostro sito dai desktop, quindi rendete i vostri contenuti usabili/leggibili anche lì.

### Layout a due colonne

I blog di solito hanno due colonne, una più ampia per il contenuto principale e una più sottile per i contenuti laterali (come widget, livelli di navigazione secondaria e pubblicità).

![Esempio di layout a 2 colonne per un blog](screenshot-blog.jpg) ![Un layout a 2 colonne con il contenuto principale nella colonna di sinistra](screenshot-blog-overlay.jpg)

In questo esempio, osservate l'immagine (B1) subito sotto l'header. È correlata al contenuto principale, ma il contenuto principale ha senso anche senza di essa, quindi si potrebbe considerare l'immagine sia come contenuto principale sia come contenuto laterale. Non ha molta importanza. Ciò che conta è, se si posiziona qualcosa subito sotto l'header, dovrebbe essere o contenuto principale o _direttamente correlato_ al contenuto principale.

### È una trappola

**[MICA](https://www.mica.edu/about-mica/)**. Questo è un po' più complicato. Sembra un layout a tre colonne:

![Esempio di un layout falso a 3 colonne](screenshot-education.jpg) ![Sembra un layout a 3 colonne ma in realtà, il contenuto laterale fluttua intorno.](screenshot-education-overlay.jpg)

Ma non lo è! B1 e B2 fluttuano intorno al contenuto principale. Ricordate quella parola "float"--suonerà familiare quando inizierete a imparare sui {{Glossary("CSS", "CSS")}}.

Perché si potrebbe pensare che sia un layout a tre colonne? Perché l'immagine in alto a destra ha una forma a L, perché B1 sembra una colonna che sostiene il contenuto principale spostato, e perché "M" e "I" del logo MICA creano una linea di forza verticale.

Questo è un buon esempio di un layout classico che supporta un po' di creatività nel design. I layout semplici sono più facili da implementare, ma concedetevi lo spazio per esprimere la vostra creatività in quest'area.

### Un layout molto più complicato

**L'Opéra di Parigi**.

![Un esempio di un layout complicato.](screenshot-opera.jpg) ![Questo è un layout a 2 colonne ma l'header si sovrappone al contenuto principale.](screenshot-opera-overlay.jpg)

Fondamentalmente un layout a due colonne, ma noterete molti accorgimenti qua e là che spezzano visivamente il layout. In particolare, l'header si sovrappone all'immagine del contenuto principale. Il modo in cui la curva del menu dell'header si integra con la curva nella parte inferiore dell'immagine fa sì che l'header e il contenuto principale appaiano come un'unica cosa anche se tecnicamente sono completamente diversi. L'esempio dell'Opera sembra più complesso di quello di MICA, ma è effettivamente più facile da implementare (va bene, "facile" è un concetto relativo).

Come vedete, potete creare siti web impressionanti anche con soli layout di base. Date un'occhiata ai vostri siti web preferiti e chiedetevi, dov'è l'header, il footer, il contenuto principale e il contenuto laterale? Questo vi ispirerà per il vostro design e vi darà buoni indizi su quali design funzionano e quali no.
