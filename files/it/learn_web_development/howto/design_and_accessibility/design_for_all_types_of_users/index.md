---
title: Come possiamo progettare per tutti i tipi di utenti?
slug: Learn_web_development/Howto/Design_and_accessibility/Design_for_all_types_of_users
l10n:
  sourceCommit: 5e815d522e796fb2209fa8470616b37e31c572b4
---

Questo articolo fornisce suggerimenti di base per aiutare a progettare siti web per qualsiasi tipo di utente.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere prima
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility"
          >Che cos'è l'accessibilità?</a
        >, poiché qui l'accessibilità non viene trattata in dettaglio.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Il design universale significa progettare per tutti, indipendentemente da disabilità
        o vincoli tecnici. Questo articolo elenca le soluzioni rapide più importanti
        per il design universale.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

Quando si crea un sito web, una delle principali questioni da considerare è il [design universale](https://en.wikipedia.org/wiki/Universal_design): soddisfare le esigenze di tutti gli utenti indipendentemente da disabilità, vincoli tecnici, cultura, posizione geografica e così via.

## Approfondimento

### Contrasto dei colori

Per mantenere il testo leggibile, usare un colore del testo che contrasti bene con il colore di sfondo. Rendere il testo particolarmente facile da leggere aiuta le persone con disabilità visive e le persone che usano il telefono per strada.

Il {{Glossary("W3C", "W3C")}} definisce una buona combinazione di colori con un algoritmo che calcola il rapporto di luminosità tra primo piano e sfondo. Il calcolo può sembrare piuttosto complicato, ma è possibile affidarsi a strumenti che svolgono il lavoro.

Scaricare e installare [Color Contrast Analyser](https://vispero.com/lp/color-contrast-checker/) di Vispero.

> [!NOTE]
> In alternativa, è possibile trovare diversi strumenti online per verificare il contrasto, come [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM. Si suggerisce uno strumento locale perché include un selettore di colori su schermo per individuare il valore di un colore.

Ad esempio, è possibile testare i colori di questa pagina e vedere il risultato in Color Contrast Analyser:

![Contrasto dei colori in questa pagina: eccellente!](color-contrast.png)

Il rapporto di contrasto della luminosità tra testo e sfondo è 8,30:1, che supera lo standard minimo (4,5:1) e dovrebbe consentire a molte persone con disabilità visive di leggere questa pagina.

### Dimensione del carattere

È possibile specificare la dimensione del carattere in un sito web usando unità relative oppure unità assolute.

#### Unità assolute

Le unità assolute non vengono calcolate proporzionalmente, ma fanno riferimento a una dimensione fissa e sono espresse nella maggior parte dei casi in pixel (`px`). Ad esempio, se nel CSS viene dichiarato quanto segue:

```css
body {
  font-size: 16px;
}
```

… si indica al browser che, qualunque cosa accada, la dimensione del carattere deve essere di 16 pixel. I browser moderni aggirano questa regola interpretandola come una richiesta di "16 pixel quando l'utente imposta un fattore di zoom del 100%".

#### Unità relative

Chiamate anche _unità proporzionali_, le unità relative vengono calcolate rispetto a un elemento padre. Le unità relative sono più adatte all'accessibilità perché rispettano le impostazioni del sistema dell'utente.

Le unità relative sono espresse in `em`, `%` e `rem`:

- Dimensioni basate sulla percentuale: `%`
  - : Questa unità indica al browser che la dimensione del carattere di un elemento deve essere N% di quella dell'elemento precedente la cui dimensione del carattere è stata espressa. Se non viene trovato alcun elemento padre, per il calcolo viene considerata come dimensione di base la dimensione predefinita del carattere nel browser (solitamente l'equivalente di 16 pixel).
- Dimensioni basate su em: `em`
  - : Questa unità viene calcolata nello stesso modo delle percentuali, tranne per il fatto che il calcolo avviene in frazioni di 1 anziché in frazioni di 100. Si dice che "em" sia la larghezza di una "M" maiuscola dell'alfabeto (approssimativamente, una "M" entra in un quadrato).
- Dimensioni basate su rem: `rem`
  - : Questa unità è proporzionale alla dimensione del carattere dell'elemento radice ed è espressa in frazioni di 1, come `em`.

Supponiamo di voler impostare una dimensione del carattere di base di 16px e un h1 (titolo principale) equivalente a 32px; tuttavia, se all'interno dell'h1 è presente uno `span` con la classe `subheading`, anche questo deve essere visualizzato con la dimensione predefinita del carattere (solitamente 16px).

Ecco l'HTML utilizzato:

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

Un CSS basato sulle percentuali sarà simile al seguente:

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

Lo stesso problema espresso con gli em:

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

Come si può vedere, i calcoli diventano rapidamente complessi quando occorre tenere traccia dell'elemento padre, del padre dell'elemento padre, del padre del padre dell'elemento padre e così via. La maggior parte dei design viene realizzata in software basati sui pixel, quindi i calcoli devono essere eseguiti dalla persona che scrive il CSS.

Ecco `rem`. Questa unità è relativa alla dimensione dell'elemento radice e non a quella di un altro elemento padre. Il CSS può essere riscritto in questo modo:

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

Più semplice, vero? Funziona in [tutti i browser attuali](https://caniuse.com/#search=rem), quindi è possibile usare liberamente questa unità.

> [!NOTE]
> Si può notare che Opera Mini non supporta la dimensione dei caratteri in rem. Finirà per impostare la propria dimensione del carattere, quindi non è necessario fornirgli unità di carattere.

#### Perché usare unità proporzionali?

Perché non si sa quando un browser potrebbe rifiutarsi di ingrandire il testo la cui dimensione è espressa in pixel. Inoltre, controllare le statistiche del sito web: potrebbero arrivare visite da browser meno recenti.

Si consiglia quanto segue:

- Descrivere i caratteri in unità `rem`; la maggior parte dei browser le gestisce correttamente.
- Lasciare che i browser meno recenti visualizzino i caratteri con il proprio motore interno. I motori dei browser ignorano qualsiasi proprietà o valore nel CSS che non sono in grado di gestire, quindi il sito web rimane utilizzabile anche se non è fedele alla visione del designer. I browser meno recenti sono comunque in fase di abbandono.

> [!NOTE]
> I risultati possono variare. Se è necessario supportare browser meno recenti, occorrerà usare `em` ed eseguire qualche calcolo in più.

### Larghezza delle righe

Esiste da tempo un dibattito sulla lunghezza delle righe sul Web, ma ecco il punto. Ai tempi dei giornali, i tipografi si resero conto che gli occhi del lettore avevano difficoltà a passare da una riga all'altra quando le righe erano troppo lunghe. La soluzione? Le colonne.

Naturalmente il problema non scompare passando al Web. Gli occhi del lettore si muovono come una navetta da una riga all'altra. Per rendere la lettura più agevole, limitare la larghezza delle righe a circa 60 o 70 caratteri.

Per ottenere questo risultato, è possibile specificare una dimensione per il contenitore del testo. Si consideri questo HTML:

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

È presente un `div` con classe `container`. È possibile applicare stile al `div` impostandone la larghezza, usando la proprietà `width`, oppure la larghezza massima, in modo che non diventi mai troppo grande, usando la proprietà `max-width`. Per un sito web elastico/responsive, quando non si conosce la larghezza predefinita del browser, è possibile usare la proprietà `max-width` per consentire fino a 70 caratteri per riga e non oltre:

```css
div.container {
  max-width: 70em;
}
```

### Contenuti alternativi per immagini, audio e video

I siti web spesso includono elementi oltre al semplice testo.

#### Immagini

Le immagini possono essere decorative o informative, ma non vi è alcuna garanzia che gli utenti possano vederle. Ad esempio:

- Gli utenti con disabilità visive si affidano a un lettore di schermo, che può gestire solo testo.
- I lettori potrebbero usare una intranet molto restrittiva che blocca le immagini provenienti da una {{Glossary("CDN", "CDN")}}.
- I lettori potrebbero aver disabilitato le immagini per risparmiare larghezza di banda, soprattutto sui dispositivi mobili (vedere di seguito).

<!---->

- Immagini decorative
  - : Servono solo come decorazione e non trasmettono informazioni effettive. Nella maggior parte dei casi potrebbero essere sostituite da un'immagine di sfondo. Assicurarsi che abbiano un attributo `alt` vuoto: `<img src="deco.gif" alt="">`, in modo che non ingombrino il testo.
- Immagini informative
  - : Vengono usate per trasmettere informazioni, da cui il nome. Possono ad esempio contenere un grafico, mostrare il gesto di una persona o fornire altre informazioni. Come minimo, occorre fornire un attributo `alt` pertinente.

Se l'immagine può essere descritta sinteticamente, è possibile fornire un attributo `alt` e nient'altro. Se l'immagine non può essere descritta sinteticamente, sarà necessario fornire lo stesso contenuto in un'altra forma nella stessa pagina, ad esempio completando un grafico a torta con una tabella che fornisca gli stessi dati, oppure ricorrere a un attributo `longdesc`. Il valore di questo attributo è un URL che punta a una risorsa che descrive esplicitamente e in dettaglio il contenuto dell'immagine.

> [!NOTE]
> L'uso e persino l'esistenza di `longdesc` sono stati discussi per molto tempo. Consultare [Image Description Extension (longdesc)](https://www.w3.org/TR/html-longdesc/) del W3C per la spiegazione completa ed esempi dettagliati.

#### Audio/video

Occorre anche fornire alternative ai contenuti multimediali.

- Sottotitolazione/didascalie
  - : Includere didascalie nel video per soddisfare le esigenze dei visitatori che non possono ascoltare l'audio. Alcuni utenti hanno difficoltà uditive, non dispongono di altoparlanti funzionanti o lavorano in un ambiente rumoroso, come in treno.
- Trascrizione
  - : I sottotitoli funzionano solo se qualcuno guarda il video. Molti utenti non hanno tempo oppure non dispongono del plugin o del codec appropriato. Inoltre, i motori di ricerca si basano principalmente sul testo per indicizzare i contenuti. Per tutti questi motivi, fornire una trascrizione testuale del file video/audio.

### Compressione delle immagini

Alcuni utenti possono scegliere di visualizzare le immagini, ma disporre comunque di larghezza di banda limitata, soprattutto nei paesi in via di sviluppo e sui dispositivi mobili. Per avere un sito web di successo, comprimere le immagini. Sono disponibili vari strumenti di aiuto, online o locali. In generale, sono preferibili gli strumenti locali perché possono integrarsi maggiormente nel flusso di lavoro di sviluppo; questi strumenti includono [ImageOptim](https://imageoptim.com/api) (Mac), [OptiPNG](https://optipng.sourceforge.net/) (tutte le piattaforme) e [PNGcrush](https://pmt.sourceforge.io/pngcrush/) (DOS, Unix/Linux).
