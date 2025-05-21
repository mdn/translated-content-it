---
title: Cosa sono i collegamenti ipertestuali?
slug: Learn_web_development/Howto/Web_mechanics/What_are_hyperlinks
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, vedremo cosa sono i collegamenti ipertestuali e perché sono importanti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovresti sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >
        ed essere familiare con <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
        >
          la differenza tra una pagina web, un sito web, un server web e un
          motore di ricerca</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a conoscere i link sul web e perché sono importanti.</td>
    </tr>
  </tbody>
</table>

## Riepilogo

I collegamenti ipertestuali, di solito chiamati link, sono un concetto fondamentale alla base del Web. Per spiegare cosa sono i link, dobbiamo tornare molto indietro alle basi dell'architettura Web.

Nel 1989, Tim Berners-Lee, l'inventore del Web, ha parlato dei tre pilastri su cui si basa il Web:

1. {{Glossary("URL", "URL")}}, un sistema di indirizzi che tiene traccia dei documenti Web
2. {{Glossary("HTTP", "HTTP")}}, un protocollo di trasferimento per trovare documenti dato il loro URL
3. {{Glossary("HTML", "HTML")}}, un formato di documento che consente l'inserimento di _collegamenti ipertestuali_

Come si può vedere dai tre pilastri, tutto sul Web ruota intorno ai documenti e a come accedervi. Lo scopo originale del Web era fornire un modo semplice per raggiungere, leggere e navigare attraverso documenti di testo. Da allora, il Web si è evoluto per fornire accesso a immagini, video e dati binari, ma questi miglioramenti hanno cambiato poco i tre pilastri.

Prima del Web, era piuttosto difficile accedere ai documenti e passare da uno all'altro. Essendo leggibili dall'uomo, gli URL hanno già reso le cose più semplici, ma è difficile digitare un URL lungo ogni volta che si desidera accedere a un documento. È qui che i collegamenti ipertestuali hanno rivoluzionato tutto. I collegamenti possono correlare qualsiasi stringa di testo con un URL, in modo che l'utente possa raggiungere immediatamente il documento di destinazione attivando il link.

I link si distinguono dal testo circostante grazie al fatto che sono sottolineati e in testo blu. Tocca o clicca un link per attivarlo, oppure, se utilizzi una tastiera, premi Tab finché il link è in evidenza e poi premi Invio o la barra spaziatrice.

![Esempio di visualizzazione di base ed effetto di un link in una pagina web](link-1.png)

I link sono l'innovazione che ha reso il Web così utile e di successo. Nel resto di questo articolo, discuteremo i vari tipi di link e la loro importanza nel design moderno del Web.

## Approfondimento

Come abbiamo detto, un link è una stringa di testo legata a un URL, e usiamo i link per consentire il passaggio facile da un documento all'altro. Detto ciò, ci sono alcune sfumature che vale la pena considerare:

### Tipi di link

- Link interno
  - : Un link tra due pagine web, dove entrambe le pagine appartengono allo stesso sito web, è chiamato link interno. Senza link interni, non esiste un sito web (a meno che, naturalmente, non sia un sito a pagina singola).
- Link esterno
  - : Un link dalla tua pagina web a quella di qualcun altro. Senza link esterni, non esiste il Web, poiché il Web è una rete di pagine web. Utilizza link esterni per fornire informazioni aggiuntive rispetto al contenuto disponibile attraverso la tua pagina web.
- Link in entrata
  - : Un link dalla pagina web di qualcun altro al tuo sito. È l'opposto di un link esterno. Nota che non è necessario collegarsi indietro quando qualcuno si collega al tuo sito.

Quando stai costruendo un sito web, concentrati sui link interni, poiché quelli rendono il tuo sito utilizzabile. Trova un buon equilibrio tra avere troppi link e troppo pochi. Parleremo di come progettare la navigazione del sito web in un altro articolo, ma come regola, ogni volta che aggiungi una nuova pagina web, assicurati che almeno una delle tue altre pagine si colleghi a quella nuova pagina. D'altra parte, se il tuo sito ha più di circa dieci pagine, è controproducente collegarsi ad ogni pagina da ogni altra pagina.

Quando stai iniziando, non devi preoccuparti tanto dei link esterni e in entrata, ma sono molto importanti se vuoi che i motori di ricerca trovino il tuo sito (vedi sotto per ulteriori dettagli).

### Ancore

La maggior parte dei link mette in relazione due pagine web. Le **ancore** mettono in relazione due sezioni di un documento. Quando segui un link che punta a un'ancora, il tuo browser salta a un'altra parte del documento corrente invece di caricare un nuovo documento. Tuttavia, crei e usi le ancore nello stesso modo in cui fai con altri link.

![Esempio di visualizzazione di base ed effetto di un'ancora in una pagina web](link-2.png)

### Link e motori di ricerca

I link sono importanti sia per gli utenti che per i motori di ricerca. Ogni volta che i motori di ricerca eseguono la scansione di una pagina web, indicizzano il sito web seguendo i link disponibili sulla pagina. I motori di ricerca non solo seguono i link per scoprire le varie pagine del sito web, ma utilizzano anche il testo visibile del link per determinare quali interrogazioni di ricerca sono appropriate per raggiungere la pagina web di destinazione.

I link influenzano quanto facilmente un motore di ricerca collegherà il tuo sito. Il problema è che è difficile misurare le attività dei motori di ricerca. Le aziende vogliono naturalmente che i loro siti si classificano in alto nei risultati di ricerca. Sappiamo quanto segue su come i motori di ricerca determinano il rango di un sito:

- Il _testo visibile_ di un link influenza quali interrogazioni di ricerca troveranno un dato URL.
- Più _link in entrata_ può vantare una pagina web, più alto sarà il suo posizionamento nei risultati di ricerca.
- I _link esterni_ influenzano il posizionamento di ricerca sia delle pagine web sorgenti che di quelle di destinazione, ma è poco chiaro di quanto.

La [SEO](https://en.wikipedia.org/wiki/Search_engine_optimization) (ottimizzazione per i motori di ricerca) è lo studio di come far classificare i siti web in alto nei risultati di ricerca. Migliorare l'uso dei link di un sito web è una tecnica SEO utile.

## Prossimi passi

Ora vorrà impostare alcune pagine web con link.

- Per ottenere un po' più di background teorico, impara a conoscere [gli URL e la loro struttura](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL), poiché ogni link punta a un URL.
- Cerchi qualcosa di un po' più pratico? Il nostro tutorial [Creare link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links) spiega come implementare i link in dettaglio.
