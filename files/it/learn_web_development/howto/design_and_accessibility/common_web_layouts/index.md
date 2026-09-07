---
title: Cosa contengono i layout web comuni?
slug: Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts
l10n:
  sourceCommit: f33de00c56ac53878eb2cb7cb5849df1f9ab8db7
---

Quando si progettano le pagine del proprio sito web, è utile avere un'idea dei layout più comuni.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Assicurarsi di aver già riflettuto su
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/Thinking_before_coding"
          >ciò che si desidera realizzare</a
        >
        con il proprio progetto web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove collocare gli elementi nelle proprie pagine web e come inserirli.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

C'è un motivo per cui si parla di web design. Si parte da una pagina vuota e la si può sviluppare in moltissime direzioni. Se non si ha molta esperienza, inoltre, iniziare con una pagina vuota può risultare un po' intimidatorio. Abbiamo oltre 25 anni di esperienza e forniremo alcune regole pratiche comuni per aiutare a progettare il proprio sito.

Anche oggi, con la nuova attenzione rivolta al Web mobile, quasi tutte le pagine web principali sono composte da queste parti:

- Intestazione
  - : Visibile nella parte superiore di ogni pagina del sito. Contiene informazioni rilevanti per tutte le pagine, come il nome o il logo del sito, e un sistema di navigazione facile da usare.
- Contenuto principale
  - : L'area più grande, che contiene i contenuti unici della pagina corrente.
- Elementi laterali
  - : 1) Informazioni che completano il contenuto principale; 2) informazioni condivise da un sottoinsieme di pagine; 3) sistema di navigazione alternativo. In effetti, tutto ciò che non è assolutamente necessario al contenuto principale della pagina.
- Piè di pagina
  - : Visibile nella parte inferiore di ogni pagina del sito. Come l'intestazione, contiene informazioni globali meno evidenti, come note legali o informazioni di contatto.

Questi elementi sono piuttosto comuni in tutti i fattori di forma, ma possono essere disposti in modi diversi. Ecco alcuni esempi (**1** rappresenta l'intestazione, **2** il piè di pagina; **A** il contenuto principale; **B1, B2** gli elementi laterali):

**Layout a 1 colonna**. Particolarmente importante per i browser mobili, per non sovraccaricare lo schermo piccolo.

![Esempio di un layout a 1 colonna: contenuto principale in alto ed elementi laterali impilati sotto.](1-col-layout.png)

**Layout a 2 colonne**. Spesso usato per i tablet, poiché dispongono di schermi di medie dimensioni.

![Esempio di un layout di base a 2 colonne: un elemento laterale nella colonna sinistra e il contenuto principale nella colonna destra.](2-col-layout-right.png) ![Esempio di un layout di base a 2 colonne: un elemento laterale nella colonna destra e il contenuto principale nella colonna sinistra.](2-col-layout-left.png)

**Layout a 3 colonne**. Adatti solo a desktop con schermi grandi. Anche molti utenti desktop preferiscono visualizzare i contenuti in finestre piccole anziché a schermo intero.

![Esempio di un layout di base a 3 colonne: elementi laterali nelle colonne sinistra e destra, contenuto principale nella colonna centrale.](3-col-layout.png) ![Un altro esempio di layout a 3 colonne: elementi laterali affiancati a sinistra, contenuto principale nella colonna destra.](3-col-layout-alt.png) ![Un altro esempio di layout a 3 colonne: elementi laterali affiancati a destra, contenuto principale nella colonna sinistra.](3-col-layout-alt2.png)

Il vero divertimento inizia quando si comincia a combinarli:

![Esempio di layout misto: contenuto principale in alto ed elementi laterali affiancati sotto.](1-col-layout-alt.png) ![Esempio di layout misto: contenuto principale nella colonna sinistra ed elementi laterali impilati uno sopra l'altro nella colonna destra.](2-col-layout-left-alt.png) ![Esempio di layout misto: un elemento laterale nella colonna sinistra e contenuto principale nella colonna destra con un elemento laterale sotto il contenuto principale.](2-col-layout-mix.png) ![Esempio di layout misto: contenuto principale a sinistra nella prima riga e un elemento laterale a destra nella stessa riga, un secondo elemento laterale che copre l'intera seconda riga.](2-col-layout-mix-alt.png)…

Questi sono solo esempi ed è possibile disporre gli elementi come si desidera. Si può notare che, mentre il contenuto può spostarsi sullo schermo, l'intestazione (1) rimane sempre in alto e il piè di pagina (2) in basso. Inoltre, il contenuto principale (A) è il più importante, quindi dovrebbe ricevere la maggior parte dello spazio.

Queste sono regole pratiche a cui fare riferimento. Naturalmente esistono design complessi ed eccezioni. In altri articoli verrà illustrato come progettare siti responsive, ossia siti che cambiano in base alle dimensioni dello schermo, e siti i cui layout variano tra le pagine. Per ora, è preferibile mantenere il layout coerente in tutto il sito.

## Approfondimento

Esaminiamo alcuni esempi più concreti tratti da siti web noti.

### Layout a una colonna

Un tipico layout a una colonna che fornisce tutte le informazioni in modo lineare su una pagina.

![Esempio reale di layout a 1 colonna](screenshot-product.jpg) ![Layout a 1 colonna con intestazione, contenuto principale, una pila di contenuti laterali e piè di pagina](screenshot-product-overlay.jpg)

Piuttosto semplice. È sufficiente ricordare che molte persone visiteranno comunque il sito da desktop, quindi occorre rendere i contenuti utilizzabili e leggibili anche in quel contesto.

### Layout a due colonne

I blog hanno generalmente due colonne: una larga per il contenuto principale e una stretta per gli elementi laterali, come widget, livelli di navigazione secondari e pubblicità.

![Esempio di layout a 2 colonne per un blog](screenshot-blog.jpg) ![Un layout a 2 colonne con il contenuto principale nella colonna sinistra](screenshot-blog-overlay.jpg)

In questo esempio, si osservi l'immagine (B1) subito sotto l'intestazione. È correlata al contenuto principale, ma il contenuto principale ha senso anche senza di essa, quindi l'immagine può essere considerata sia contenuto principale sia contenuto laterale. Non è particolarmente importante. Ciò che conta è che, se si inserisce qualcosa immediatamente sotto l'intestazione, dovrebbe trattarsi di contenuto principale oppure essere _direttamente correlato_ al contenuto principale.

### È una trappola

**[MICA](https://www.mica.edu/about-mica/)**. Questo caso è un po' più complesso. Sembra un layout a tre colonne:

![Esempio di un falso layout a 3 colonne](screenshot-education.jpg) ![Sembra un layout a 3 colonne, ma in realtà il contenuto laterale fluttua attorno al contenuto principale.](screenshot-education-overlay.jpg)

Ma non lo è! B1 e B2 fluttuano attorno al contenuto principale. Ricordare la parola "float": risulterà familiare quando si inizierà a imparare {{Glossary("CSS", "CSS")}}.

Perché potrebbe sembrare un layout a tre colonne? Perché l'immagine in alto a destra è a forma di L, perché B1 sembra una colonna che sostiene il contenuto principale spostato e perché la "M" e la "I" del logo MICA creano una linea di forza verticale.

Questo è un buon esempio di layout classico che supporta una certa creatività nel design. I layout semplici sono più facili da implementare, ma è bene lasciare spazio all'espressione della propria creatività in quest'area.

### Un layout molto più complesso

**L'Opera di Parigi**.

![Un esempio di layout complesso.](screenshot-opera.jpg) ![Si tratta di un layout a 2 colonne, ma l'intestazione si sovrappone al contenuto principale.](screenshot-opera-overlay.jpg)

Fondamentalmente è un layout a due colonne, ma si noteranno molte modifiche qua e là che interrompono visivamente il layout. In particolare, l'intestazione si sovrappone all'immagine del contenuto principale. Grazie al modo in cui la curva del menu dell'intestazione si collega alla curva nella parte inferiore dell'immagine, l'intestazione e il contenuto principale sembrano un unico elemento, anche se tecnicamente sono completamente diversi. L'esempio dell'Opera appare più complesso dell'esempio MICA, ma in realtà è più facile da implementare, anche se "facile" _è_ un concetto relativo.

Come si può vedere, è possibile creare siti web straordinari anche usando soltanto layout di base. Osservare i propri siti web preferiti e chiedersi: dov'è l'intestazione, il piè di pagina, il contenuto principale e il contenuto laterale? Questo può ispirare il proprio design e fornire buoni indizi su quali design funzionano e quali no.
