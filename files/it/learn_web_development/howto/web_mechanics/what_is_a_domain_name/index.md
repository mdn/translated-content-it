---
title: Cos'è un Nome di Dominio?
slug: Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima di tutto, è necessario sapere
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
        Imparare cosa sono i nomi di dominio, come funzionano e perché sono importanti.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

I nomi di dominio sono una parte fondamentale dell'infrastruttura di Internet. Forniscono un indirizzo leggibile per gli esseri umani per qualsiasi server web disponibile su Internet.

Qualsiasi computer connesso a Internet può essere raggiunto attraverso un {{Glossary("IP_Address", "indirizzo IP")}} pubblico, sia esso un indirizzo IPv4 (es., `192.0.2.172`) oppure un indirizzo IPv6 (es., `2001:db8:8b73:0000:0000:8a2e:0370:1337`).

I computer possono gestire facilmente tali indirizzi, ma le persone hanno difficoltà a capire chi gestisce il server o quale servizio offre il sito web. Gli indirizzi IP sono difficili da ricordare e potrebbero cambiare nel tempo.

Per risolvere tutti questi problemi utilizziamo indirizzi leggibili detti nomi di dominio.

## Approfondimento

### Struttura dei nomi di dominio

Un nome di dominio ha una struttura semplice composta da diverse parti (può essere una sola parte, due, tre...), separate da punti e **lette da destra a sinistra**:

![Anatomia del nome di dominio MDN](structure.png)

Ognuna di queste parti fornisce informazioni specifiche sul nome di dominio complessivo.

- {{Glossary("TLD", "TLD")}} (Top-Level Domain).

  - I TLD indicano agli utenti lo scopo generale del servizio dietro il nome di dominio. I TLD più generici (`.com`, `.org`, `.net`) non richiedono che i servizi web soddisfino criteri particolari, ma alcuni TLD impongono politiche più restrittive per chiarire qual è il loro scopo. Per esempio:

    - I TLD locali come `.us`, `.fr` o `.se` possono richiedere che il servizio sia fornito in una determinata lingua o ospitato in un certo paese, in quanto dovrebbero indicare una risorsa in una lingua o paese particolare.
    - I TLD contenenti `.gov` possono essere utilizzati solo da dipartimenti governativi.
    - Il TLD `.edu` è riservato all'uso da parte di istituzioni educative e accademiche.

    I TLD possono contenere caratteri speciali così come caratteri latini. La lunghezza massima di un TLD è di 63 caratteri, anche se la maggior parte varia tra 2 e 3.

    La lista completa dei TLD è [gestita da ICANN](https://www.icann.org/en/contracted-parties/registry-operators/resources/list-of-top-level-domains).

- Etichetta (o componente)

  - Le etichette seguono il TLD. Un'etichetta è una sequenza di caratteri non sensibile al maiuscolo/minuscolo da uno a sessantatré caratteri di lunghezza, contenente solo le lettere da `A` a `Z`, i numeri da `0` a `9`, e il carattere '-' (che non può essere il primo o l'ultimo carattere dell'etichetta). `a`, `97`, e `hello-strange-person-16-how-are-you` sono tutti esempi di etichette valide.

    L'etichetta situata subito prima del TLD è anche chiamata _Secondary Level Domain_ (SLD).

    Un nome di dominio può avere molte etichette (o componenti). Non è obbligatorio né necessario avere 3 etichette per formare un nome di dominio. Ad esempio, [informatics.ed.ac.uk](https://informatics.ed.ac.uk/) è un nome di dominio valido. Per qualsiasi dominio controllato (es., [mozilla.org](https://www.mozilla.org/en-US/)), è possibile creare "sottodomini" con contenuto differente posizionato in ciascuno, come [developer.mozilla.org](/), [support.mozilla.org](https://support.mozilla.org/), o [bugzilla.mozilla.org](https://bugzilla.mozilla.org/).

### Acquistare un nome di dominio

#### Chi possiede un nome di dominio?

Non si può "comprare un nome di dominio". Questo per garantire che i nomi di dominio inutilizzati diventino eventualmente disponibili per essere usati di nuovo da qualcun altro. Se ogni nome di dominio fosse comprato, il web si riempirebbe rapidamente di nomi di dominio inutilizzati, bloccati e non utilizzabili da nessuno.

Invece, si paga per il diritto di utilizzare un nome di dominio per uno o più anni. È possibile rinnovare il diritto, e il rinnovo ha la precedenza rispetto alle domande di altre persone. Ma non si possiede mai il nome di dominio.

Le aziende chiamate registrar utilizzano registri di nomi di dominio per tenere traccia delle informazioni tecniche e amministrative che collegano a Lei al suo nome di dominio.

> [!NOTE]
> Per alcuni nomi di dominio, potrebbe non essere un registrar incaricato di tenere traccia. Ad esempio, ogni nome di dominio sotto `.fire` è gestito da Amazon.

#### Trovare un nome di dominio disponibile

Per scoprire se un dato nome di dominio è disponibile,

- Vada sul sito web di un registrar di nomi di dominio. La maggior parte di essi offre un servizio "whois" che dice se un nome di dominio è disponibile.
- In alternativa, se utilizza un sistema con una shell integrata, digiti un comando `whois` in essa, come mostrato qui per `mozilla.org`:

  ```bash
  whois mozilla.org
  ```

  Questo restituirà il seguente output:

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

Come può vedere, non posso registrare `mozilla.org` perché la Mozilla Foundation lo ha già registrato.

D'altra parte, vediamo se potrei registrare `afunkydomainname.org`:

```bash
whois afunkydomainname.org
```

Questo restituirà il seguente output (al momento della scrittura):

```plain
NOT FOUND
```

Come può vedere, il dominio non esiste nel database `whois`, quindi potremmo chiedere di registrarlo. Buono a sapersi!

#### Ottenere un nome di dominio

Il processo è piuttosto semplice:

1. Vada sul sito web di un registrar.
2. Di solito c'è un "Ottieni un nome di dominio" ben visibile. Clicchi su di esso.
3. Compili il modulo con tutti i dettagli richiesti. Assicurati, soprattutto, di non avere scritto male il suo desiderato nome di dominio. Una volta pagato, è troppo tardi!
4. Il registrar la informerà quando il nome di dominio è correttamente registrato. Entro poche ore, tutti i server DNS avranno ricevuto le sue informazioni DNS.

> [!NOTE]
> In questo processo, il registrar le chiede il suo indirizzo reale. Si assicuri di compilarlo correttamente, poiché in alcuni paesi i registrar potrebbero essere costretti a chiudere il dominio se non possono fornire un indirizzo valido.

#### Aggiornamento del DNS

I database DNS sono memorizzati su ogni server DNS mondiale, e tutti questi server fanno riferimento a pochi server speciali chiamati "server di nomi autorevoli" o "server DNS di alto livello" — questi sono come i server capo che gestiscono il sistema.

Ogni volta che il suo registrar crea o aggiorna una qualsiasi informazione per un determinato dominio, le informazioni devono essere aggiornate in ogni database DNS. Ogni server DNS a conoscenza di un determinato dominio memorizza le informazioni per un certo tempo prima che vengano automaticamente invalidate e poi aggiornate (il server DNS interroga un server autorevole e riceve le informazioni aggiornate da esso). Pertanto, ci vuole del tempo affinché i server DNS che conoscono questo nome di dominio ottengano le informazioni aggiornate.

### Come funziona una richiesta DNS?

Come abbiamo già visto, quando vuole visualizzare una pagina web nel suo browser è più facile digitare un nome di dominio piuttosto che un indirizzo IP. Diamo un'occhiata al processo:

1. Digiti `mozilla.org` nella barra degli indirizzi del suo browser.
2. Il suo browser chiede al suo computer se riconosce già l'indirizzo IP identificato da questo nome di dominio (usando una cache DNS locale). Se lo riconosce, il nome viene tradotto nell'indirizzo IP e il browser negozia i contenuti con il server web. Fine della storia.
3. Se il suo computer non sa quale IP si nasconde dietro il nome `mozilla.org`, prosegue chiedendo a un server DNS, il cui compito è precisamente quello di dire al suo computer quale indirizzo IP corrisponde a ciascun nome di dominio registrato.
4. Ora che il computer conosce l'indirizzo IP richiesto, il suo browser può negoziare contenuti con il server web.

![Spiegazione dei passaggi necessari per ottenere il risultato di una richiesta DNS](2014-10-dns-request2.png)

## Prossimi passi

Bene, abbiamo parlato molto di processi e architettura. È tempo di andare avanti.

- Se desidera mettere in pratica, è un buon momento per iniziare a scavare nel design ed esplorare [l'anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
- Vale anche la pena notare che alcuni aspetti della costruzione di un sito web costano denaro. Si prega di consultare [quanto costa costruire un sito web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost).
- Oppure legga di più sui [Nomi di Dominio](https://en.wikipedia.org/wiki/Domain_name) su Wikipedia.
- Può anche trovare [qui](https://howdns.works/) una spiegazione divertente e colorata di come funziona il DNS.
