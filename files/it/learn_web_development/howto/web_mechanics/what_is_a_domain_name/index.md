---
title: Cos'è un Nome di Dominio?
slug: Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima è necessario sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >
        e comprendere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL"
          >cosa sono gli URL</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Scoprire cosa sono i nomi di dominio, come funzionano e perché sono importanti.
      </td>
    </tr>
  </tbody>
</table>

## Sintesi

I nomi di dominio sono una parte fondamentale dell'infrastruttura di Internet. Forniscono un indirizzo leggibile per gli esseri umani per qualsiasi server web disponibile su Internet.

Qualsiasi computer connesso a Internet può essere raggiunto tramite un {{Glossary("IP_Address", "indirizzo IP")}} pubblico, sia un indirizzo IPv4 (ad es., `192.0.2.172`) sia un indirizzo IPv6 (ad es., `2001:db8:8b73:0000:0000:8a2e:0370:1337`).

I computer possono gestire facilmente tali indirizzi, ma le persone trovano difficile scoprire chi gestisce il server o quale servizio offre il sito web. Gli indirizzi IP sono difficili da ricordare e potrebbero cambiare nel tempo.

Per risolvere tutti questi problemi utilizziamo gli indirizzi leggibili per gli esseri umani chiamati nomi di dominio.

## Approfondimento

### Struttura dei nomi di dominio

Un nome di dominio ha una struttura semplice composta da diverse parti (potrebbe essere composta da una sola parte, due, tre…), separate da punti e **lette da destra a sinistra**:

![Anatomia del nome di dominio MDN](structure.png)

Ciascuna di queste parti fornisce informazioni specifiche sull'intero nome di dominio.

- {{Glossary("TLD", "TLD")}} (Top-Level Domain).

  - : I TLD indicano agli utenti lo scopo generale del servizio dietro il nome di dominio. I TLD più generici (`.com`, `.org`, `.net`) non richiedono che i servizi web soddisfino criteri particolari, ma alcuni TLD applicano politiche più rigorose in modo che il loro scopo sia più chiaro. Ad esempio:

    - I TLD locali come `.us`, `.fr`, o `.se` possono richiedere che il servizio sia fornito in una determinata lingua o ospitato in un certo paese — dovrebbero indicare una risorsa in una particolare lingua o paese.
    - I TLD contenenti `.gov` sono consentiti solo per essere utilizzati dai dipartimenti governativi.
    - Il TLD `.edu` è destinato esclusivamente all'uso da parte delle istituzioni educative e accademiche.

    I TLD possono contenere caratteri speciali oltre ai caratteri latini. La lunghezza massima di un TLD è di 63 caratteri, anche se la maggior parte si aggira tra 2–3.

    L'elenco completo dei TLD è [gestito da ICANN](https://www.icann.org/en/contracted-parties/registry-operators/resources/list-of-top-level-domains).

- Label (o componente)

  - : Le label sono ciò che segue il TLD. Una label è una sequenza di caratteri case-insensitive di lunghezza compresa tra uno e sessanta-tre caratteri, contenente solo le lettere da `A` a `Z`, le cifre da `0` a `9`, e il carattere '- ' (che non può essere il primo o l'ultimo carattere della label). `a`, `97`, e `hello-strange-person-16-how-are-you` sono tutti esempi di label valide.

    La label situata immediatamente prima del TLD è anche chiamata _Secondary Level Domain_ (SLD).

    Un nome di dominio può avere molte label (o componenti). Non è obbligatorio né necessario avere 3 label per formare un nome di dominio. Ad esempio, [informatics.ed.ac.uk](https://informatics.ed.ac.uk/) è un nome di dominio valido. Per qualsiasi dominio che controlla (ad esempio, [mozilla.org](https://www.mozilla.org/en-US/)), è possibile creare "sottodomini" con contenuti diversi situati in ciascuno di essi, come [developer.mozilla.org](/), [support.mozilla.org](https://support.mozilla.org/), o [bugzilla.mozilla.org](https://bugzilla.mozilla.org/).

### Acquistare un nome di dominio

#### Chi possiede un nome di dominio?

Non è possibile "acquistare un nome di dominio". Questo per fare in modo che i nomi di dominio non utilizzati diventino eventualmente disponibili per l'uso da parte di qualcun altro. Se ogni nome di dominio fosse acquistato, il web si riempirebbe rapidamente di nomi di dominio non utilizzati che sarebbero bloccati e non potrebbero essere usati da nessuno.

Invece, si paga per il diritto di usare un nome di dominio per uno o più anni. Può rinnovare il suo diritto, e il suo rinnovo ha la priorità rispetto alle applicazioni di altre persone. Ma non si possiede mai il nome di dominio.

Aziende chiamate registrar utilizzano registri di nomi di dominio per tenere traccia delle informazioni tecniche e amministrative che la collegano al suo nome di dominio.

> [!NOTE]
> Per alcuni nomi di dominio, potrebbe non essere un registrar a tenere traccia. Ad esempio, ogni nome di dominio sotto `.fire` è gestito da Amazon.

#### Trovare un nome di dominio disponibile

Per scoprire se un dato nome di dominio è disponibile,

- Visitare il sito web di un registrar di nomi di dominio. La maggior parte di loro fornisce un servizio "whois" che dice se un nome di dominio è disponibile.
- In alternativa, se usa un sistema con una shell integrata, inserisca un comando `whois` al suo interno, come mostrato qui per `mozilla.org`:

  ```bash
  whois mozilla.org
  ```

  Questo produrrà il seguente output:

  ```plain
  Domain Name:MOZILLA.ORG
  Domain ID: D1409563-LROR
  Creation Date: 1998-01-24T05:00:00Z
  Updated Date: 2013-12-08T01:16:57Z
  Registry Expiry Date: 2015-01-23T05:00:00Z
  Sponsoring Registrar:MarkMonitor Inc. (R37-LROR)
  Sponsoring Registrar IANA ID: 292
  WHOIS Server:
  Referral URL:
  Domain Status: clientDeleteProhibited
  Domain Status: clientTransferProhibited
  Domain Status: clientUpdateProhibited
  Registrant ID:mmr-33684
  Registrant Name:DNS Admin
  Registrant Organization:Mozilla Foundation
  Registrant Street: 650 Castro St Ste 300
  Registrant City:Mountain View
  Registrant State/Province:CA
  Registrant Postal Code:94041
  Registrant Country:US
  Registrant Phone:+1.6509030800
  ```

Come può vedere, non posso registrare `mozilla.org` perché la Fondazione Mozilla l'ha già registrato.

D'altra parte, vediamo se potrei registrare `afunkydomainname.org`:

```bash
whois afunkydomainname.org
```

Questo produrrà il seguente output (al momento della scrittura):

```plain
NOT FOUND
```

Come può vedere, il dominio non esiste nel database `whois`, quindi potremmo chiedere di registrarlo. Buono a sapersi!

#### Ottenere un nome di dominio

Il processo è piuttosto semplice:

1. Vada al sito web di un registrar.
2. Di solito c'è un prominente pulsante "Ottenere un nome di dominio". Clicchi su di esso.
3. Compili il modulo con tutti i dettagli richiesti. Si assicuri, soprattutto, di non aver scritto male il nome di dominio desiderato. Una volta pagato, è troppo tardi!
4. Il registrar le farà sapere quando il nome di dominio sarà correttamente registrato. Nel giro di poche ore, tutti i server DNS avranno ricevuto le sue informazioni DNS.

> [!NOTE]
> In questo processo il registrar le chiederà il suo indirizzo fisico reale. Si assicuri di compilarlo correttamente, poiché in alcuni paesi i registrar potrebbero essere costretti a chiudere il dominio se non possono fornire un indirizzo valido.

#### Aggiornamento del DNS

I database DNS sono memorizzati su ogni server DNS in tutto il mondo e tutti questi server fanno riferimento a pochi server speciali chiamati "server autoritativi" o "server DNS di livello superiore" — questi sono come i server principali che gestiscono il sistema.

Ogni volta che il suo registrar crea o aggiorna delle informazioni per un determinato dominio, le informazioni devono essere aggiornate in ogni database DNS. Ogni server DNS che conosce un dato dominio memorizza le informazioni per un certo tempo prima che vengano automaticamente invalidate e poi aggiornate (il server DNS interroga un server autoritativo e recupera da esso le informazioni aggiornate). Pertanto, ci vuole un po' di tempo prima che i server DNS che conoscono questo nome di dominio ottengano le informazioni aggiornate.

### Come funziona una richiesta DNS?

Come abbiamo già visto, quando vuole visualizzare una pagina web nel suo browser è più facile digitare un nome di dominio piuttosto che un indirizzo IP. Esaminiamo il processo:

1. Digiti `mozilla.org` nella barra degli indirizzi del suo browser.
2. Il suo browser chiede al suo computer se riconosce già l'indirizzo IP identificato da questo nome di dominio (utilizzando una cache DNS locale). Se lo fa, il nome viene tradotto nell'indirizzo IP e il browser negozia i contenuti con il server web. Fine della storia.
3. Se il suo computer non sa quale indirizzo IP è dietro il nome `mozilla.org`, chiede a un server DNS, il cui compito è proprio quello di dire al suo computer quale indirizzo IP corrisponde a ciascun nome di dominio registrato.
4. Ora che il computer conosce l'indirizzo IP richiesto, il suo browser può negoziare i contenuti con il server web.

![Spiegazione dei passaggi necessari per ottenere il risultato di una richiesta DNS](2014-10-dns-request2.png)

## Prossimi passi

Bene, abbiamo parlato a lungo di processi e architettura. È ora di andare avanti.

- Se desidera passare all'azione concreta, è un buon momento per iniziare a esplorare il design e scoprire [l'anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
- Vale anche la pena notare che alcuni aspetti della costruzione di un sito web costano denaro. Si prega di fare riferimento a [quanto costa costruire un sito web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost).
- Oppure legga di più sui [nomi di dominio](https://it.wikipedia.org/wiki/Domain_name) su Wikipedia.
- Il tutorial [How DNS works](https://howdns.works/) offre una spiegazione divertente e colorata.
