---
title: Come funziona Internet?
slug: Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work
l10n:
  sourceCommit: 3a5d88d0377791fea0700a772ca047f6c2463083
---

Questo articolo descrive che cos'è Internet e come funziona.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nessuno, ma si consiglia di leggere prima
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/Thinking_before_coding"
          >l'articolo sulla definizione degli obiettivi del progetto</a
        >
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Verranno apprese le basi dell'infrastruttura tecnica del Web e
        la differenza tra Internet e il Web.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

**Internet** è la spina dorsale del Web, l'infrastruttura tecnica che rende possibile il Web. Nella sua forma più semplice, Internet è una grande rete di computer che comunicano tra loro.

[La storia di Internet è in parte poco chiara](https://en.wikipedia.org/wiki/Internet#History). È iniziata negli anni Sessanta come progetto di ricerca finanziato dall'esercito statunitense, per poi evolvere in un'infrastruttura pubblica negli anni Ottanta con il sostegno di molte università pubbliche e aziende private. Le varie tecnologie che supportano Internet si sono evolute nel tempo, ma il suo funzionamento non è cambiato molto: Internet è un modo per connettere tra loro i computer e garantire che, qualunque cosa accada, trovino un modo per rimanere connessi.

## Video su Internet

- [Come funziona Internet in 5 minuti](https://www.youtube.com/watch?v=7_LPdttKXPc): un video di 5 minuti per comprendere le basi di Internet, di Aaron Titus.
- [Come funziona Internet?](https://www.youtube.com/watch?v=x3c1ih2NJEg) Video dettagliato di 9 minuti, ben visualizzato.

## Approfondimento

### Una rete semplice

Quando due computer devono comunicare, occorre collegarli, fisicamente (di solito con un [cavo Ethernet](https://en.wikipedia.org/wiki/Ethernet_crossover_cable)) oppure in modalità wireless (per esempio con sistemi [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) o [Bluetooth](https://en.wikipedia.org/wiki/Bluetooth)). Tutti i computer moderni possono supportare uno qualsiasi di questi collegamenti.

> [!NOTE]
> Nel resto dell'articolo si parlerà soltanto di cavi fisici, ma le reti wireless funzionano allo stesso modo.

![Due computer collegati tra loro](internet-schema-1.png)

Una rete di questo tipo non è limitata a due computer. È possibile connettere tutti i computer desiderati. Tuttavia, la situazione si complica rapidamente. Per connettere, ad esempio, dieci computer, sono necessari 45 cavi, con nove connettori per computer!

![Dieci computer tutti insieme](internet-schema-2.png)

Per risolvere questo problema, ogni computer di una rete viene connesso a uno speciale piccolo computer chiamato _network switch_ (o semplicemente _switch_). Questo switch ha un solo compito: come un segnalatore in una stazione ferroviaria, inoltra i messaggi verso i destinatari previsti. Per inviare un messaggio al computer B, il computer A invia il messaggio allo switch, che a sua volta lo inoltra al computer B.

Una volta aggiunto uno switch al sistema, la rete di 10 computer richiede soltanto 10 cavi: un singolo connettore per ciascun computer e uno switch con 10 connettori.

![Dieci computer con uno switch](internet-schema-3.png)

Per distinguere i computer, lo switch utilizza gli _indirizzi MAC_, che identificano le interfacce di rete per la consegna all'interno della rete locale. Gli indirizzi MAC sono come impronte digitali: in genere vengono assegnati dal produttore, ma possono anche essere assegnati o modificati dal software, una pratica oggi comune per ragioni di privacy. Ogni messaggio contiene gli indirizzi MAC del mittente e del destinatario. Lo switch legge l'indirizzo del mittente e ricorda da quale connessione è arrivato il messaggio, così sa dove inoltrare i futuri messaggi indirizzati a quel mittente. Se non ha ancora appreso dove si trova un destinatario, inoltra il messaggio attraverso tutte le altre connessioni. Quando il destinatario invia un messaggio di risposta, anche lo switch apprende la sua posizione.

### Una rete di reti

Fin qui tutto bene. Ma come si possono connettere centinaia, migliaia o miliardi di computer? Naturalmente un singolo switch non può scalare fino a questo punto, ma, leggendo attentamente, è stato detto che uno switch è un computer come qualsiasi altro: cosa impedisce di connettere due switch tra loro? Nulla, quindi facciamolo.

![Due switch collegati tra loro](internet-schema-4.png)

Si può immaginare di connettere tra loro gli switch all'infinito, formando una rete come questa:

![Switch collegati ad altri switch](internet-schema-5.png)

Collegare gli switch in questo modo estende una singola rete locale. Ogni switch dispone di un'ampia mappa che indica quale connessione usare per ciascun indirizzo MAC nella propria rete locale. Se si connettessero dieci miliardi di computer in questa rete, ogni switch dovrebbe ricordare fino a dieci miliardi di indirizzi MAC. Ogni volta che l'indirizzo del destinatario è sconosciuto, oppure è stato eliminato per inattività, gli switch devono trasmettere il messaggio a tutti i computer della rete locale. Man mano che la rete cresce, diventa sempre più costoso tenere traccia dei singoli dispositivi e trovare i destinatari sconosciuti.

Il problema principale è che gli indirizzi non hanno una gerarchia e non corrispondono alla struttura della rete: è come cercare di capire a chi consegnare la posta confrontando l'impronta digitale di ogni persona. Per risolvere questo problema, i computer vengono divisi in reti locali separate e queste reti vengono connesse usando un dispositivo chiamato _router_. Esso usa un tipo diverso di indirizzo, un _{{Glossary("IP_address", "indirizzo IP")}}_, che è una sequenza di 4 numeri come `142.250.190.78`. A differenza degli indirizzi MAC, che sono "impronte digitali", gli indirizzi IP sono "indirizzi stradali" e vengono assegnati quando un computer si connette a una rete, identificata nell'indirizzo IP da un _prefisso_ condiviso. Un router può quindi memorizzare istruzioni di inoltro per un intero gruppo di indirizzi, ad esempio "inoltra a questo router ogni volta che l'indirizzo IP inizia con `142.250`", senza apprendere la posizione di ogni singolo computer di quel gruppo.

> [!NOTE]
> Potrebbe sorgere la domanda sul perché siano necessari indirizzi MAC e switch, se gli indirizzi IP e i router possono realizzare una rete end-to-end. Gli switch offrono molti vantaggi pratici. Uno di questi è che una rete locale commutata consente a un dispositivo di mantenere lo stesso indirizzo IP mentre si sposta tra connessioni all'interno della rete, ad esempio tra due punti di accesso Wi-Fi: lo switch apprende nuovamente su quale connessione si trova l'indirizzo MAC, quindi l'indirizzo IP — e tutte le connessioni che lo stanno già utilizzando — continua a funzionare. Un altro vantaggio è che i router stessi necessitano di indirizzi MAC: per passare un pacchetto al router successivo lungo il percorso, un router deve comunque identificare quale dispositivo sulla rete condivisa debba riceverlo.

Una rete di questo tipo si avvicina molto a ciò che viene chiamato Internet. Serve soltanto il mezzo fisico, ovvero i cavi, per connettere tutti questi router. Fortunatamente, un'infrastruttura di questo genere esisteva già prima di Internet: la rete telefonica. Per connettere la rete all'infrastruttura telefonica, è necessaria un'apparecchiatura speciale chiamata _modem_. Questo _modem_ trasforma le informazioni della rete in informazioni gestibili dall'infrastruttura telefonica e viceversa.

![Un router collegato a un modem](internet-schema-6.png)

Si noti che il router commerciale presente in casa probabilmente combina uno switch, un router e un modem in un unico dispositivo.

A questo punto la rete è connessa all'infrastruttura telefonica. Il passaggio successivo consiste nell'inviare i messaggi dalla propria rete alla rete che si desidera raggiungere. Per farlo, viene connessa a un Internet Service Provider (ISP). Un ISP è un'azienda che gestisce alcuni _router_ speciali, tutti collegati tra loro e in grado di accedere anche ai router di altri ISP. Il messaggio proveniente dalla rete viene quindi trasportato attraverso la rete di reti degli ISP fino alla rete di destinazione. Internet è costituita da tutta questa infrastruttura di reti.

![Stack Internet completo](internet-schema-7.png)

### Nomi di dominio

Gli indirizzi IP sono perfettamente adatti ai computer, ma per gli esseri umani è difficile ricordare questo tipo di indirizzo. Per semplificare le cose, è possibile associare a un indirizzo IP un nome leggibile dall'uomo chiamato _nome di dominio_. Per esempio, al momento della stesura; gli indirizzi IP possono cambiare, `google.com` è il nome di dominio usato per l'indirizzo IP `142.250.190.78`. Pertanto, usare il nome di dominio è il modo più semplice per raggiungere un computer tramite Internet.

![Mostra come un nome di dominio può essere associato a un indirizzo IP](dns-ip.png)

### Internet e il Web

Come si può notare, quando si naviga sul Web con un browser web, solitamente si usa il nome di dominio per raggiungere un sito web. Significa che Internet e il Web sono la stessa cosa? Non è così semplice. Come visto, Internet è un'infrastruttura tecnica che consente di connettere tra loro miliardi di computer. Tra questi computer, alcuni, chiamati _Web server_, possono inviare messaggi comprensibili ai browser web. Internet è un'infrastruttura, mentre il Web è un servizio costruito sopra l'infrastruttura. Vale la pena notare che esistono molti altri servizi costruiti sopra Internet, come l'email e {{Glossary("IRC", "IRC")}}.

### Intranet ed Extranet

Le intranet sono reti _private_ limitate ai membri di una determinata organizzazione.
Sono comunemente usate per fornire un portale attraverso cui i membri possono accedere in modo sicuro alle risorse condivise, collaborare e comunicare.
Per esempio, l'intranet di un'organizzazione può ospitare pagine web per condividere informazioni di reparti o team, unità condivise per gestire documenti e file importanti,
portali per svolgere attività di amministrazione aziendale e strumenti di collaborazione come wiki, forum di discussione e sistemi di messaggistica.

Le extranet sono molto simili alle intranet, tranne per il fatto che aprono tutta o parte di una rete privata per consentire la condivisione e la collaborazione con altre organizzazioni.
Sono tipicamente usate per condividere informazioni in modo sicuro con clienti e parti interessate che collaborano strettamente con un'azienda.
Spesso le loro funzioni sono simili a quelle fornite da un'intranet: condivisione di informazioni e file, strumenti di collaborazione, forum di discussione e così via.

Sia le intranet sia le extranet funzionano sullo stesso tipo di infrastruttura di Internet e utilizzano gli stessi protocolli.
Possono quindi essere accessibili ai membri autorizzati da diverse ubicazioni fisiche.

![Rappresentazione grafica del funzionamento di Extranet e Intranet](internet-schema-8.png)

## Passaggi successivi

- [Come funziona il Web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)
- [Comprendere la differenza tra una pagina web, un sito web, un web server e un motore di ricerca](/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web)
- [Comprendere i nomi di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name)
