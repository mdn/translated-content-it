---
title: Cosa contengono i layout web comuni?
slug: Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Quando si progettano pagine per il tuo sito web, è utile avere un'idea dei layout più comuni.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Assicurati di aver già riflettuto su
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/Thinking_before_coding"
          >cosa vuoi realizzare</a>
        con il tuo progetto web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Impara dove posizionare gli elementi sulle tue pagine web e come farlo.
      </td>
    </tr>
  </tbody>
</table>

## Riassunto

C'è una ragione per cui parliamo di design web. Si parte con una pagina vuota, e puoi portarla in molte direzioni. E se non hai molta esperienza, iniziare con una pagina vuota potrebbe essere un po' spaventoso. Abbiamo oltre 25 anni di esperienza e ti daremo alcune regole pratiche comuni per aiutarti a progettare il tuo sito.

Anche ora, con la nuova attenzione al web mobile, quasi tutte le pagine web principali sono costruite da queste parti:

- Header
  - : Visibile nella parte superiore di ogni pagina del sito. Contiene informazioni rilevanti per tutte le pagine (come il nome o il logo del sito) e un sistema di navigazione facile da usare.
- Contenuto principale
  - : La regione più grande, contenente contenuti unici per la pagina corrente.
- Elementi laterali
  - : 1) Informazioni che completano il contenuto principale; 2) informazioni condivise tra un sottoinsieme di pagine; 3) sistema di navigazione alternativo. In sintesi, tutto ciò che non è assolutamente richiesto dal contenuto principale della pagina.
- Footer
  - : Visibile nella parte inferiore di ogni pagina del sito. Come l'header, contiene informazioni globali meno prominenti come avvisi legali o informazioni di contatto.

Questi elementi sono piuttosto comuni in tutti i formati, ma possono essere disposti in modi diversi. Ecco alcuni esempi (**1** rappresenta l'header, **2** il footer; **A** il contenuto principale; **B1, B2** elementi laterali):

**Layout a 1 colonna**. Particolarmente importante per i browser mobili, affinché non si sovraccarichi il piccolo schermo.

![Esempio di layout a 1 colonna: Main in alto e elementi laterali impilati sotto di esso.](1-col-layout.png)

**Layout a 2 colonne**. Spesso usato per targettizzare i tablet, poiché hanno schermi di dimensione media.

![Esempio di un layout base a 2 colonne: Un elemento laterale nella colonna sinistra e main nella colonna destra.](2-col-layout-right.png) ![Esempio di un layout base a 2 colonne: Un elemento laterale nella colonna destra e main nella colonna sinistra.](2-col-layout-left.png)

**Layout a 3 colonne**. Adatto solo per desktop con schermi grandi. (Anche molti utenti desktop preferiscono visualizzare le cose in finestre piccole piuttosto che a schermo intero.)

![Esempio di un layout base a 3 colonne: Elemento laterale nelle colonne sinistra e destra, Main nella colonna centrale.](3-col-layout.png) ![Altro esempio di un layout a 3 colonne: Elementi laterali affiancati nella sinistra, Main nella colonna destra.](3-col-layout-alt.png) ![Altro esempio di un layout a 3 colonne: Elementi laterali affiancati nella destra, Main nella colonna sinistra.](3-col-layout-alt2.png)

Il vero divertimento inizia quando inizi a mescolare tutto insieme:

![Esempio di layout misto: Main in alto e elementi laterali sotto di esso affiancati.](1-col-layout-alt.png) ![Esempio di un layout misto: Main nella colonna sinistra e elementi laterali impilati uno sopra l'altro nella colonna destra](2-col-layout-left-alt.png) ![Esempio di un layout misto: un elemento laterale nella colonna sinistra e main nella colonna destra con un elemento laterale sotto il main.](2-col-layout-mix.png) ![Esempio di un layout misto: Main a sinistra della prima riga e un elemento laterale a destra della stessa riga, un secondo elemento laterale che copre tutta la seconda riga.](2-col-layout-mix-alt.png)...

Questi sono solo esempi e sei abbastanza libero di disporre gli elementi come desideri. Potresti notare che, mentre il contenuto può muoversi sullo schermo, manteniamo sempre l'header (1) in alto e il footer (2) in basso. Inoltre, il contenuto principale (A) è quello che conta di più, quindi dagliene la maggior parte dello spazio.

Queste sono delle regole pratiche da cui puoi trarre ispirazione. Ovviamente ci sono design complessi ed eccezioni. In altri articoli discuteremo di come progettare siti responsive (siti che cambiano a seconda della dimensione dello schermo) e siti i cui layout variano tra le pagine. Per ora, è meglio mantenere il layout coerente in tutto il sito.

## Apprendimento attivo

_Non è ancora disponibile un apprendimento attivo. [Per favore, considera di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimento

Studiamo alcuni esempi più concreti presi da siti ben noti.

### Layout a una colonna

Un tipico layout a una colonna che fornisce tutte le informazioni in modo lineare su una sola pagina.

![Esempio di un layout a 1 colonna in uso](screenshot-product.jpg) ![Layout a 1 colonna con header, contenuto principale, una pila di contenuti laterali e un footer](screenshot-product-overlay.jpg)

Abbastanza semplice. Ricorda solo che molte persone continueranno a navigare sul tuo sito dai desktop, quindi rendi il tuo contenuto utilizzabile/leggibile anche lì.

### Layout a due colonne

I blog di solito hanno due colonne, una larga per il contenuto principale e una più stretta per gli elementi laterali (come widget, livelli di navigazione secondari e annunci).

![Esempio di layout a 2 colonne per un blog](screenshot-blog.jpg) ![Un layout a 2 colonne con il contenuto principale nella colonna sinistra](screenshot-blog-overlay.jpg)

In questo esempio, guarda l'immagine (B1) subito sotto l'header. È correlata al contenuto principale, ma il contenuto principale ha senso anche senza di essa, quindi potresti considerare l'immagine sia come contenuto principale sia come contenuto laterale. Non importa veramente. Ciò che conta è che, se metti qualcosa subito sotto l'header, dovrebbe essere il contenuto principale o _direttamente correlato_ al contenuto principale.

### È una trappola

**[MICA](https://www.mica.edu/about-mica/)**. Questo è un po' più complicato. Sembra un layout a tre colonne:

![Esempio di un falso layout a 3 colonne](screenshot-education.jpg) ![Sembra un layout a 3 colonne ma in realtà, il contenuto laterale fluttua intorno.](screenshot-education-overlay.jpg)

Ma non lo è! B1 e B2 fluttuano intorno al contenuto principale. Ricorda quella parola "float"—ti tornerà in mente quando inizierai a imparare su {{Glossary("CSS", "CSS")}}.

Perché pensi che sia un layout a tre colonne? Perché l'immagine in alto a destra è a forma di L, perché B1 sembra una colonna che supporta il contenuto principale spostato, e perché la "M" e la "I" del logo MICA creano una linea verticale di forza.

Questo è un buon esempio di un layout classico che supporta una certa creatività nel design. I layout semplici sono più facili da implementare, ma concediti spazio per esprimere la tua creatività in quest'area.

### Un layout molto più impegnativo

**L'Opera di Parigi**.

![Un esempio di un layout complesso.](screenshot-opera.jpg) ![Questo è un layout a 2 colonne ma l'header si sovrappone al contenuto principale.](screenshot-opera-overlay.jpg)

Fondamentalmente un layout a due colonne, ma noterai molte modifiche qua e là che spezzano visivamente il layout. In particolare, l'header si sovrappone all'immagine del contenuto principale. Il modo in cui la curva del menu dell'header si lega con la curva nella parte inferiore dell'immagine fa sembrare l'header e il contenuto principale un tutt'uno, anche se tecnicamente sono completamente diversi. L'esempio dell'Opera sembra più complesso dell'esempio MICA, ma in realtà è più facile da implementare (va bene, "facile" è un concetto relativo).

Come vedi, puoi creare siti web sbalorditivi anche con layout di base. Dai un'occhiata ai tuoi siti web preferiti e chiediti, dov'è l'header, il footer, il contenuto principale e quello laterale? Questo ti ispirerà per il tuo design e ti darà buoni suggerimenti su quali design funzionano e quali no.
