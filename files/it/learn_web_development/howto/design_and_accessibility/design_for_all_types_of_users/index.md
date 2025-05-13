---
title: Come possiamo progettare per tutti i tipi di utenti?
slug: Learn_web_development/Howto/Design_and_accessibility/Design_for_all_types_of_users
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo fornisce suggerimenti di base per aiutarla a progettare siti web per qualsiasi tipo di utente.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovrebbe innanzitutto leggere
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility"
          >Che cos'è l'accessibilità?</a
        >, poiché non copriamo l'accessibilità in dettaglio qui.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Il design universale significa progettare per tutti, indipendentemente dalle disabilità o dai vincoli tecnici. Questo articolo elenca i più importanti guadagni rapidi per il design universale.
      </td>
    </tr>
  </tbody>
</table>

## Riassunto

Quando sta costruendo un sito web, una delle principali questioni da considerare è il [Design Universale](https://en.wikipedia.org/wiki/Universal_design): accogliere tutti gli utenti indipendentemente dalla disabilità, dai vincoli tecnici, dalla cultura, dalla posizione, e così via.

## Apprendimento attivo

_Non c'è ancora apprendimento attivo disponibile. [Per favore, consideri di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimenti

### Contrasto dei colori

Per mantenere il testo leggibile, utilizzi un colore di testo che contrasti bene con il colore di sfondo. Renda il testo facilmente leggibile, per aiutare le persone con problemi visivi e le persone che utilizzano i loro telefoni per strada.

Il {{Glossary("W3C", "W3C")}} definisce un buon mix di colori con un algoritmo che calcola il rapporto di luminosità tra primo piano e sfondo. Il calcolo può sembrare piuttosto complicato, ma possiamo affidarci a strumenti per svolgere il lavoro al posto nostro.

Scarichiamo e installiamo il [Color Contrast Analyser](https://www.tpgi.com/color-contrast-checker/) del Paciello Group.

> [!NOTE]
> In alternativa, può trovare numerosi strumenti di verifica del contrasto online, come il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM. Suggeriamo un verificatore locale perché viene fornito con un selettore di colori sullo schermo per trovare un valore di colore.

Per esempio, esaminiamo i colori su questa pagina e vediamo come ci comportiamo nel Color Contrast Analyser:

![Contrasto colore su questa pagina: eccellente!](color-contrast.png)

Il rapporto di contrasto di luminosità tra testo e sfondo è 8.30:1, che supera lo standard minimo (4.5:1) e dovrebbe permettere a molte persone con problemi visivi di leggere questa pagina.

### Dimensione del carattere

Può specificare la dimensione del carattere su un sito web sia tramite unità relative che assolute.

#### Unità assolute

Le unità assolute non vengono calcolate proporzionalmente ma si riferiscono a una dimensione impostata in modo fisso, per così dire, e sono espresse la maggior parte delle volte in pixel (`px`). Per esempio, se nel suo CSS dichiara questo:

```css
body {
  font-size: 16px;
}
```

… sta dicendo al browser che qualunque cosa accada, la dimensione del carattere deve essere di 16 pixel. I browser moderni aggirano questa regola fingendo che le stia chiedendo "16 pixel quando l'utente imposta un fattore zoom del 100%".

#### Unità relative

Chiamate anche _unità proporzionali,_ le unità relative vengono calcolate rispetto a un elemento padre. Le unità relative sono più favorevoli all'accessibilità perché rispettano le impostazioni sul sistema dell'utente.

Le unità relative sono espresse in `em`, `%` e `rem`:

- Dimensioni basate su percentuali: `%`
  - : Questa unità dice al suo browser che la dimensione del carattere di un elemento deve essere N% rispetto all'elemento precedente la cui dimensione del carattere era espressa. Se non si trova alcun padre, la dimensione predefinita del carattere all'interno del browser viene considerata come dimensione base per il calcolo (di solito l'equivalente di 16 pixel).
- Dimensioni basate su Em: `em`
  - : Questa unità si calcola nello stesso modo delle percentuali, tranne che si computa in porzioni di 1 e non in porzioni di 100. Si dice che "em" sia la larghezza di una "M" maiuscola dell'alfabeto (grosso modo, una "M" entra in un quadrato).
- Dimensioni basate su Rem: `rem`
  - : Questa unità è proporzionale alla dimensione del carattere dell'elemento radice ed è espressa come porzioni di 1, come `em`.

Supponiamo di voler una dimensione base del carattere di 16px e un h1 (intestazione principale) pari all'equivalente di 32px, e che se all'interno dell'h1 troviamo uno `span` con la classe `subheading`, questo deve essere visualizzato alla dimensione del carattere predefinita (di solito 16px).

Qui c'è l'HTML che stiamo utilizzando:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Font size experiment</title>
  </head>
  <body>
    <h1>
      This is our main heading
      <span class="subheading">This is our subheading</span>
    </h1>
  </body>
</html>
```

Un CSS basato su percentuali sembrerà così:

```css
body {
  /* 100% of the browser's base font size, so in most cases this will render as 16 pixels */
  font-size: 100%;
}
h1 {
  /* twice the size of the body, thus 32 pixels */
  font-size: 200%;
}
span.subheading {
  /* half the size of the h1, thus 16 pixels to come back to the original size */
  font-size: 50%;
}
```

Lo stesso problema espresso con ems:

```css
body {
  /* 1em = 100% of the browser's base font size, so in most cases this will render as 16 pixels */
  font-size: 1em;
}
h1 {
  /* twice the size of the body, thus 32 pixels */
  font-size: 2em;
}
span.subheading {
  /* half the size of the h1, thus 16 pixels to come back to the original size */
  font-size: 0.5em;
}
```

Come può vedere, la matematica diventa rapidamente scoraggiante quando deve tenere traccia del padre, del padre del padre, del padre del padre del padre, e così via. (La maggior parte dei progetti vengono effettuati in software basati su pixel, quindi la matematica deve essere fatta dalla persona che codifica il CSS).

Introduciamo `rem`. Questa unità è relativa alla dimensione dell'elemento radice e non a nessun altro genitore. Il CSS può essere riscritto così:

```css
body {
  /* 1em = 100% of the browser's base font size, so in most cases this will render as 16 pixels */
  font-size: 1em;
}
h1 {
  /* twice the size of the body, thus 32 pixels */
  font-size: 2rem;
}
span.subheading {
  /* original size */
  font-size: 1rem;
}
```

Più semplice, vero? Questo funziona su [ogni browser attuale](https://caniuse.com/#search=rem), dunque si senta libero di utilizzare questa unità.

> [!NOTE]
> Potrebbe notare che Opera Mini non supporta la dimensione dei caratteri in rem. Finirà per impostare la sua dimensione del carattere, quindi non si preoccupi di fornire unità di carattere.

#### Perché dovrei voler utilizzare unità proporzionali?

Perché non sa quando un browser arriverà e si rifiuterà di ingrandire il testo la cui dimensione è espressa in pixel. Inoltre, controlli le statistiche del suo sito web: potrebbe ricevere visite da browser più vecchi.

Consiglieremmo quanto segue:

- Descriva i font in unità `rem`, la maggior parte dei browser sarà molto contenta di esse;
- Lasci che i browser più vecchi visualizzino i font con il loro motore interno. I motori dei browser ignoreranno qualsiasi proprietà o valore nel CSS se non riescono a gestirli, quindi il suo sito rimane utilizzabile se non fedele alla visione del designer. In ogni caso, i browser più vecchi sono in fase di estinzione.

> [!NOTE]
> La sua esperienza potrebbe variare. Se deve indirizzarsi a browser più vecchi, dovrà usare `em` e fare un po' più di matematica.

### Larghezza della linea

C'è un dibattito in corso sulla lunghezza delle righe sul web, ma ecco il punto. Quando avevamo i giornali, i tipografi si accorsero che gli occhi del lettore avevano difficoltà a passare da una riga all'altra se le righe erano troppo lunghe. La soluzione? Le colonne.

Ovviamente il problema non scompare quando passiamo al Web. Gli occhi del lettore funzionano come una navetta che passa da una linea all'altra. Per agevolare la lettura, si limiti la larghezza delle linee a circa 60 o 70 caratteri.

Per ottenere questo, può specificare una dimensione per il contenitore del testo. Consideriamo questo HTML:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Font size experiment</title>
  </head>
  <body>
    <div class="container">
      <h1>
        This is our main heading
        <span class="subheading">This is our subheading</span>
      </h1>

      <p>[lengthy text that spans many lines]</p>
    </div>
  </body>
</html>
```

Abbiamo un `div` con la classe `container`. Possiamo stilizzare il `div` sia per impostare la sua larghezza (utilizzando la proprietà `width`) sia per impostare la sua larghezza massima in modo che non diventi troppo grande (utilizzando la proprietà `max-width`). Se desidera un sito web elastico/responsivo, e non sa quale sia la larghezza predefinita del browser, può utilizzare la proprietà `max-width` per consentire fino a 70 caratteri per riga e non di più:

```css
div.container {
  max-width: 70em;
}
```

### Contenuto alternativo per immagini, audio e video

I siti web spesso includono contenuti diversi dal semplice testo.

#### Immagini

Le immagini possono essere decorative o informative, ma non c'è garanzia che i suoi utenti possano vederle. Per esempio,

- Gli utenti con problemi visivi si affidano a un lettore di schermo, che può gestire solo il testo.
- I suoi lettori potrebbero utilizzare una intranet molto rigorosa che blocca le immagini provenienti da un {{Glossary("CDN", "CDN")}}.
- I suoi lettori potrebbero aver disabilitato le immagini per risparmiare larghezza di banda, specialmente sui dispositivi mobili (vedere sotto).

<!---->

- Immagini decorative
  - : Sono solo decorative e non trasmettono informazioni reali. Potrebbero spesso essere sostituite da un'immagine di sfondo. Si assicuri che presentino un attributo `alt` vuoto: `<img src="deco.gif" alt="">` per evitare che ostacolino il testo.
- Immagini informative
  - : Vengono utilizzate per trasmettere informazioni, da qui il nome. Possono, ad esempio, presentare un grafico, mostrare il gesto di una persona o qualsiasi altra informazione. Al minimo, si deve fornire un attributo `alt` pertinente.

Se l'immagine può essere descritta sinteticamente, può fornire un attributo `alt` e nient'altro. Se l'immagine non può essere descritta sinteticamente, dovrà fornire lo stesso contenuto in un'altra forma nella stessa pagina (ad esempio, complementare un grafico a torta con una tabella che fornisce gli stessi dati), o ricorrere a un attributo `longdesc`. Il valore di questo attributo è un URL che punta verso una risorsa che descrive esplicitamente in dettaglio il contenuto dell'immagine.

> [!NOTE]
> L'uso e persino l'esistenza di `longdesc` è stato oggetto di dibattito per un certo tempo. La preghiamo di fare riferimento alla [Image Description Extension (longdesc)](https://www.w3.org/TR/html-longdesc/) del W3C per una spiegazione completa ed esempi dettagliati.

#### Audio/video

Deve inoltre fornire alternative ai contenuti multimediali.

- Sottotitolazione/sottotitoli
  - : Dovrebbe includere sottotitoli nel suo video per soddisfare i visitatori che non possono sentire l'audio. Alcuni utenti hanno problemi di udito, mancanza di altoparlanti funzionanti o lavorano in un ambiente rumoroso (come sul treno).
- Trascrizione
  - : I sottotitoli funzionano solo se qualcuno guarda il video. Molti utenti non hanno tempo, o mancano del plugin o del codec appropriato. Inoltre, i motori di ricerca si affidano principalmente al testo per indicizzare i suoi contenuti. Per tutte queste ragioni, fornisca una trascrizione testuale del file video/audio.

### Compressione delle immagini

Alcuni utenti potrebbero scegliere di visualizzare le immagini, ma avere comunque una larghezza di banda limitata disponibile, specialmente nei paesi in via di sviluppo e sui dispositivi mobili. Se desidera un sito web di successo, per favore comprima le sue immagini. Ci sono vari strumenti per aiutarla, sia online che locali. In generale, gli strumenti locali sono preferiti perché possono essere più integrati con il suo flusso di lavoro di sviluppo; questi strumenti includono [ImageOptim](https://imageoptim.com/api) (Mac), [OptiPNG](https://optipng.sourceforge.net/) (tutte le piattaforme), e [PNGcrush](https://pmt.sourceforge.io/pngcrush/) (DOS, Unix/Linux).
