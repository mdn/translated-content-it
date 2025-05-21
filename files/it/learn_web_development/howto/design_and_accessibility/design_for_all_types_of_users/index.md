---
title: Come possiamo progettare per tutti i tipi di utenti?
slug: Learn_web_development/Howto/Design_and_accessibility/Design_for_all_types_of_users
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo fornisce suggerimenti di base per aiutare a progettare siti web per qualsiasi tipo di utente.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È consigliabile leggere prima
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility"
          >Che cos'è l'accessibilità?</a
        >, poiché qui non trattiamo l'accessibilità in dettaglio.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        La progettazione universale significa progettare per tutti, indipendentemente dalle disabilità
        o dai vincoli tecnici. Questo articolo elenca i guadagni rapidi più importanti per il design universale.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Quando si costruisce un sito web, una delle principali questioni da considerare è la [progettazione universale](https://en.wikipedia.org/wiki/Universal_design): accomodare tutti gli utenti indipendentemente da disabilità, vincoli tecnici, cultura, posizione e così via.

## Apprendimento attivo

_Non è ancora disponibile un apprendimento attivo. [Per favore, considera di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimenti

### Contrasto del colore

Per mantenere il testo leggibile, utilizza un colore del testo che contrasti bene con il colore di sfondo. Rendilo particolarmente facile da leggere per aiutare le persone ipovedenti e chi usa il telefono per strada.

Il {{Glossary("W3C", "W3C")}} definisce una buona combinazione di colori con un algoritmo che calcola il rapporto di luminosità tra primo piano e sfondo. Il calcolo può sembrare piuttosto complicato, ma possiamo affidarci a strumenti per il compito.

Scarichiamo e installiamo il [Color Contrast Analyser](https://www.tpgi.com/color-contrast-checker/) del Paciello Group.

> [!NOTE]
> In alternativa, puoi trovare numerosi controlli di contrasto online, come il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM. Suggeriamo un controllo locale perché è corredato di un selettore di colori su schermo per determinare un valore di colore.

Ad esempio, testiamo i colori su questa pagina e vediamo come ci comportiamo nel Color Contrast Analyser:

![Contrasto dei colori su questa pagina: eccellente!](color-contrast.png)

Il rapporto di contrasto di luminosità tra testo e sfondo è 8.30:1, che supera lo standard minimo (4.5:1) e dovrebbe permettere a molte persone ipovedenti di leggere questa pagina.

### Dimensione del carattere

È possibile specificare la dimensione del carattere su un sito web attraverso unità relative o unità assolute.

#### Unità assolute

Le unità assolute non sono calcolate proporzionalmente ma si riferiscono a una dimensione fissa e sono espresse per lo più in pixel (`px`). Ad esempio, se nel tuo CSS dichiari questo:

```css
body {
  font-size: 16px;
}
```

… stai dicendo al browser che qualunque cosa accada, la dimensione del carattere deve essere di 16 pixel. I browser moderni aggirano questa regola facendo finta che tu stia chiedendo "16 pixel quando l'utente imposta un fattore di zoom del 100%".

#### Unità relative

Chiamate anche _unità proporzionali_, le unità relative sono calcolate rispetto a un elemento genitore. Le unità relative sono più favorevoli all'accessibilità perché rispettano le impostazioni del sistema dell'utente.

Le unità relative sono espresse in `em`, `%` e `rem`:

- Dimensioni basate su percentuali: `%`
  - : Questa unità dice al tuo browser che la dimensione del carattere di un elemento deve essere N% dell'elemento precedente di cui è stata espressa la dimensione del carattere. Se non si trova un genitore, la dimensione del carattere predefinita nel browser è considerata la dimensione base per il calcolo (di solito equivalente a 16 pixel).
- Dimensioni basate su em: `em`
  - : Questa unità è calcolata nello stesso modo delle percentuali, tranne che si computano in porzioni di 1 e non di 100. Si dice che "em" sia la larghezza di una "M" maiuscola nell'alfabeto (grossolanamente parlando, una "M" entra in un quadrato).
- Dimensioni basate su rem: `rem`
  - : Questa unità è proporzionale alla dimensione del carattere dell'elemento radice ed è espressa come porzioni di 1, come `em`.

Supponiamo di voler una dimensione base del carattere di 16px e un h1 (intestazione principale) all'equivalente di 32px, e se all'interno dell'h1 troviamo uno `span` con la classe `subheading`, anch'esso deve essere reso alla dimensione predefinita del carattere (di solito 16px).

Ecco l'HTML che stiamo utilizzando:

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

Un CSS basato su percentuali apparirà così:

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

Come puoi vedere, la matematica si complica rapidamente quando devi tenere traccia del genitore, del genitore del genitore, del genitore del genitore del genitore, e così via. (La maggior parte dei design sono realizzati in software basati su pixel, quindi la matematica deve essere fatta dalla persona che codifica il CSS).

Entra in gioco `rem`. Questa unità è relativa alla dimensione dell'elemento radice e non a un altro genitore. Il CSS può essere riscritto in questo modo:

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

Più facile, non è vero? Questo funziona in [ogni browser attuale](https://caniuse.com/#search=rem), quindi sentiti libero di usare questa unità.

> [!NOTE]
> Potresti notare che Opera Mini non supporta il dimensionamento del carattere in rem. Imposterà alla fine una propria dimensione del carattere, quindi non preoccuparti di fornire unità di font.

#### Perché dovrei voler usare unità proporzionali?

Perché non sai quando un browser verrà fuori e rifiuterà di zoomare il testo la cui dimensione è espressa in pixel. Inoltre, controlla le statistiche del tuo sito web: potresti ricevere visite da browser più vecchi.

Consigliamo quanto segue:

- Descrivi i font con unità `rem`, la maggior parte dei browser sarà molto felice con esse;
- Lascia che i browser più vecchi visualizzino i font con il proprio motore interno. I motori dei browser ignoreranno qualsiasi proprietà o valore nel CSS se non possono gestirli, in modo che il tuo sito web sia ancora utilizzabile anche se non è fedele alla visione del tuo designer. In ogni caso, i browser più vecchi stanno scomparendo.

> [!NOTE]
> Il tuo chilometraggio può variare. Se devi soddisfare i browser più vecchi, dovrai usare gli `em` e fare un po' più di matematica.

### Larghezza della linea

Esiste un dibattito di lunga data sulla lunghezza delle linee sul web, ma ecco la storia. Quando avevamo i giornali, i tipografi si accorsero che gli occhi del lettore avrebbero avuto difficoltà a passare da una linea all'altra se le linee fossero state troppo lunghe. La soluzione? Colonne.

Ovviamente il problema non scompare quando passiamo al Web. Gli occhi del lettore si comportano come una navetta che va da linea a linea. Per rendere il compito più facile agli occhi delle persone, limita la larghezza della linea a circa 60 o 70 caratteri.

Per ottenere ciò, puoi specificare una dimensione per il contenitore del testo. Consideriamo questo HTML:

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

Abbiamo un `div` con classe `container`. Possiamo stilizzare il `div` impostando la sua larghezza (usando la proprietà `width`) o la sua larghezza massima in modo che non diventi mai troppo grande (usando la proprietà `max-width`). Se vuoi un sito web elastico/responsivo e non conosci la larghezza predefinita del browser, puoi utilizzare la proprietà `max-width` per consentire fino a 70 caratteri per linea e non di più:

```css
div.container {
  max-width: 70em;
}
```

### Contenuto alternativo per immagini, audio e video

I siti web spesso includono elementi oltre il semplice testo.

#### Immagini

Le immagini possono essere decorative o informative, ma non c'è alcuna garanzia che i tuoi utenti possano vederle. Ad esempio,

- Gli utenti ipovedenti si affidano a uno screen reader, che può gestire solo il testo.
- I tuoi lettori potrebbero utilizzare una intranet molto rigida che blocca le immagini provenienti da un {{Glossary("CDN", "CDN")}}.
- I tuoi lettori potrebbero aver disabilitato le immagini per risparmiare larghezza di banda, soprattutto su dispositivi mobili (vedi sotto).

<!---->

- Immagini decorative
  - : Sono solo decorative e non trasmettono alcuna informazione reale. Spesso potrebbero essere sostituite da un'immagine di sfondo. Assicurati che abbiano un attributo `alt` vuoto: `<img src="deco.gif" alt="">` in modo che non ingombrino il testo.
- Immagini informative
  - : Sono utilizzate per trasmettere informazioni, da qui il loro nome. Possono, ad esempio, rappresentare un grafico, mostrare il gesto di una persona, o qualsiasi altra informazione. Al minimo, devi fornire un attributo `alt` pertinente.

Se l'immagine può essere descritta brevemente, puoi fornire un attributo `alt` e nient'altro. Se l'immagine non può essere descritta brevemente, dovrai fornire lo stesso contenuto in un'altra forma nella stessa pagina (es., completare un grafico a torta con una tabella che fornisce gli stessi dati) o ricorrere a un attributo `longdesc`. Il valore di questo attributo è un URL che punta a una risorsa che descrive esplicitamente in dettaglio il contenuto dell'immagine.

> [!NOTE]
> L'uso e persino l'esistenza di `longdesc` sono stati oggetto di dibattito per un lungo periodo di tempo. Si prega di fare riferimento ai [Image Description Extension (longdesc)](https://www.w3.org/TR/html-longdesc/) del W3C per la spiegazione completa e esempi dettagliati.

#### Audio/video

Devi anche fornire alternative ai contenuti multimediali.

- Sottotitoli/trascrizioni
  - : Dovresti includere sottotitoli nel tuo video per soddisfare i visitatori che non possono sentire l'audio. Alcuni utenti hanno problemi di udito, non hanno altoparlanti funzionanti o lavorano in un ambiente rumoroso (come sul treno).
- Trascrizione
  - : I sottotitoli funzionano solo se qualcuno guarda il video. Molti utenti non hanno tempo, o non dispongono del plugin o codec adeguato. Inoltre, i motori di ricerca si basano principalmente sul testo per indicizzare i tuoi contenuti. Per tutte queste ragioni, per favore fornisci una trascrizione testuale del file video/audio.

### Compressione delle immagini

Alcuni utenti possono scegliere di visualizzare le immagini, ma avere comunque una larghezza di banda limitata, specialmente nei paesi in via di sviluppo e sui dispositivi mobili. Se vuoi avere un sito web di successo, per favore comprimi le tue immagini. Ci sono vari strumenti per aiutarti, disponibili sia online che localmente. In generale, gli strumenti locali sono preferiti perché possono essere più integrati nel flusso di lavoro di sviluppo; tra questi strumenti ci sono [ImageOptim](https://imageoptim.com/api) (Mac), [OptiPNG](https://optipng.sourceforge.net/) (tutte le piattaforme), e [PNGcrush](https://pmt.sourceforge.io/pngcrush/) (DOS, Unix/Linux).
