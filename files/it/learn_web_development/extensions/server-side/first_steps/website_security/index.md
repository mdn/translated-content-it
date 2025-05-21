---
title: Sicurezza del sito web
slug: Learn_web_development/Extensions/Server-side/First_steps/Website_security
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}

La sicurezza del sito web richiede vigilanza in tutti gli aspetti della progettazione e dell'uso del sito stesso. Questo articolo introduttivo non ti renderà un esperto di sicurezza del sito web, ma ti aiuterà a comprendere da dove provengono le minacce e cosa puoi fare per rafforzare la tua applicazione web contro gli attacchi più comuni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenze informatiche di base.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le minacce più comuni alla sicurezza delle applicazioni web e
        cosa puoi fare per ridurre il rischio che il tuo sito venga violato.
      </td>
    </tr>
  </tbody>
</table>

## Cos'è la sicurezza del sito web?

Internet è un posto pericoloso! Con grande regolarità, sentiamo parlare di siti web che diventano inattivi a causa di attacchi di denial of service, o che mostrano informazioni modificate (e spesso dannose) nelle loro home page. In altri casi di alto profilo, milioni di password, indirizzi email e dettagli delle carte di credito sono stati divulgati al pubblico dominio, esponendo gli utenti del sito a imbarazzi personali e rischi finanziari.

Lo scopo della sicurezza del sito web è prevenire questi (o qualsiasi altro) tipi di attacco. La definizione più formale di sicurezza del sito web _è l'atto/la pratica di proteggere i siti web da accessi, utilizzi, modifiche, distruzioni o interruzioni non autorizzate_.

Una sicurezza efficace del sito web richiede un impegno progettuale per l'intero sito web: nella tua applicazione web, nella configurazione del server web, nelle tue politiche per la creazione e il rinnovo delle password, e nel codice lato client. Sebbene tutto ciò possa sembrare molto minaccioso, la buona notizia è che se stai utilizzando un framework web lato server, quasi certamente questo abiliterà "di default" meccanismi di difesa robusti e ben pensati contro una serie di attacchi più comuni. Altri attacchi possono essere mitigati attraverso la tua configurazione del server web, ad esempio abilitando HTTPS. Infine, ci sono strumenti pubblicamente disponibili per la scansione delle vulnerabilità che possono aiutarti a scoprire se hai commesso errori evidenti.

Il resto di questo articolo ti fornirà maggiori dettagli su alcune minacce comuni e su alcuni semplici passaggi che puoi adottare per proteggere il tuo sito.

> [!NOTE]
> Questo è un argomento introduttivo, progettato per aiutarti a iniziare a pensare alla sicurezza del sito web, ma non è esaustivo.

## Minacce alla sicurezza del sito web

Questa sezione elenca solo alcune delle minacce più comuni ai siti web e come vengono mitigate. Mentre leggi, nota come le minacce abbiano più successo quando l'applicazione web o si fida, o non è _abbastanza paranoica_ riguardo ai dati provenienti dal browser.

### Cross-Site Scripting (XSS)

XSS è un termine usato per descrivere una classe di attacchi che permettono a un attaccante di iniettare script lato client _attraverso_ il sito web nei browser di altri utenti. Poiché il codice iniettato proviene dal sito, il codice è _attendibile_ e può fare cose come inviare il cookie di autorizzazione del sito dell'utente all'attaccante. Quando l'attaccante ha il cookie, può accedere al sito come se fosse l'utente e fare tutto ciò che l'utente può fare, come accedere ai dettagli della carta di credito, vedere dettagli di contatto o cambiare password.

> [!NOTE]
> Le vulnerabilità XSS sono state storicamente più comuni di qualsiasi altro tipo di minaccia alla sicurezza.

Le vulnerabilità XSS sono divise in _riflesse_ e _persistenti_, basate su come il sito restituisce gli script iniettati a un browser.

- Una vulnerabilità XSS _riflessa_ si verifica quando il contenuto dell'utente passato al server viene restituito _immediatamente_ e _non modificato_ per essere visualizzato nel browser. Qualsiasi script nel contenuto originale dell'utente verrà eseguito quando la nuova pagina viene caricata.
  Ad esempio, considera una funzione di ricerca del sito in cui i termini di ricerca sono codificati come parametri URL, e questi termini sono visualizzati insieme ai risultati. Un attaccante può costruire un collegamento di ricerca che contiene uno script dannoso come parametro (ad es., `https://developer.mozilla.org?q=beer<script%20src="http://example.com/tricky.js"></script>`) e inviarlo per email a un altro utente. Se l'utente di destinazione clicca su questo "collegamento interessante", lo script verrà eseguito quando i risultati della ricerca vengono visualizzati. Come discusso in precedenza, questo fornisce all'attaccante tutte le informazioni di cui ha bisogno per entrare nel sito come l'utente di destinazione, potenzialmente effettuando acquisti come l'utente o condividendone le informazioni di contatto.
- Una vulnerabilità XSS _persistente_ si verifica quando lo script dannoso è _memorizzato_ sul sito web e successivamente ripresentato non modificato affinché altri utenti lo eseguano inconsapevolmente.
  Ad esempio, una board di discussione che accetta commenti contenenti HTML non modificato potrebbe memorizzare uno script dannoso di un attaccante. Quando i commenti vengono visualizzati, lo script viene eseguito e può inviare all'attaccante le informazioni necessarie per accedere all'account dell'utente. Questo tipo di attacco è estremamente popolare e potente, perché l'attaccante potrebbe non avere nemmeno alcun contatto diretto con le vittime.

Sebbene i dati dalle richieste `POST` o `GET` siano la fonte più comune di vulnerabilità XSS, qualsiasi dato proveniente dal browser è potenzialmente vulnerabile, come i dati cookie renderizzati dal browser o i file utente che vengono caricati e visualizzati.

La migliore difesa contro le vulnerabilità XSS è rimuovere o disabilitare ogni markup che possa potenzialmente contenere istruzioni per eseguire codice. Per HTML ciò include elementi come `<script>`, `<object>`, `<embed>` e `<link>`.

Il processo di modifica dei dati dell'utente affinché non possano essere utilizzati per eseguire script o influenzare in altro modo l'esecuzione del codice del server è noto come sanitizzazione dell'input. Molti framework web sanificano automaticamente l'input dell'utente dai moduli HTML per impostazione predefinita.

### Iniezione SQL

Le vulnerabilità di iniezione SQL consentono agli utenti malintenzionati di eseguire codice SQL arbitrario su un database, permettendo l'accesso, la modifica o l'eliminazione dei dati indipendentemente dai permessi dell'utente. Un attacco di iniezione riuscito potrebbe mascherare identità, creare nuove identità con diritti di amministrazione, accedere a tutti i dati sul server o distruggere/modificare i dati per renderli inutilizzabili.

I tipi di iniezione SQL includono l'iniezione SQL basata sugli errori, l'iniezione SQL basata su errori booleani e l'iniezione SQL basata sul tempo.

Questa vulnerabilità è presente se l'input dell'utente che viene passato a una dichiarazione SQL sottostante può cambiare il significato della dichiarazione. Ad esempio, il seguente codice è inteso a elencare tutti gli utenti con un particolare nome (`userName`) fornito da un modulo HTML:

```python
statement = "SELECT * FROM users WHERE name = '" + userName + "';"
```

Se l'utente specifica un nome reale, la dichiarazione funzionerà come previsto. Tuttavia, un utente malintenzionato potrebbe cambiare completamente il comportamento di questa dichiarazione SQL nella nuova dichiarazione nell'esempio seguente, specificando `a';DROP TABLE users; SELECT * FROM userinfo WHERE 't' = 't` per `userName`.

```sql
SELECT * FROM users WHERE name = 'a';DROP TABLE users; SELECT * FROM userinfo WHERE 't' = 't';
```

La dichiarazione modificata crea una valida dichiarazione SQL che elimina la tabella `users` e seleziona tutti i dati dalla tabella `userinfo` (che rivela le informazioni di ogni utente). Questo funziona perché la prima parte del testo iniettato (`a';`) completa la dichiarazione originale.

Per evitare tali attacchi, la migliore pratica è utilizzare query parametrizzate (dichiarazioni preparate). Questo approccio assicura che l'input dell'utente venga trattato come una stringa di dati piuttosto che come SQL eseguibile, in modo che l'utente non possa abusare dei caratteri di sintassi speciali SQL per generare dichiarazioni SQL non desiderate. Il seguente è un esempio:

```sql
SELECT * FROM users WHERE name = ? AND password = ?;
```

Quando si esegue la query sopra, ad esempio, in Python, passiamo `name` e `password` come parametri, come mostrato di seguito.

```python
cursor.execute("SELECT * FROM users WHERE name = ? AND password = ?", (name, password))
```

Le librerie spesso forniscono API ben astratte che gestiscono la protezione da iniezioni SQL per lo sviluppatore, come i modelli di Django. È possibile evitare iniezioni SQL utilizzando API incapsulate piuttosto che scrivendo direttamente SQL grezzo.

### Cross-Site Request Forgery (CSRF)

Gli attacchi CSRF consentono a un utente malintenzionato di eseguire azioni utilizzando le credenziali di un altro utente senza che quest'ultimo ne sia a conoscenza o abbia dato il consenso.

Questo tipo di attacco è meglio spiegato con un esempio. Josh è un utente malintenzionato che sa che un particolare sito consente agli utenti connessi di inviare denaro a un account specificato tramite una richiesta HTTP `POST` che include il nome dell'account e un importo di denaro. Josh costruisce un modulo che include i suoi dati bancari e un importo di denaro come campi nascosti, e lo invia via email ad altri utenti del sito (con il pulsante _Invia_ mascherato come un link a un sito "per farsi ricchi velocemente").

Se un utente clicca sul pulsante invia, verrà inviata una richiesta HTTP `POST` al server contenente i dettagli della transazione e tutti i cookie lato client che il browser ha associato al sito (è un comportamento normale del browser aggiungere i cookie del sito associato alle richieste). Il server controllerà i cookie e li utilizzerà per determinare se l'utente è connesso e ha il permesso di effettuare la transazione.

Il risultato è che qualunque utente clicchi sul pulsante _Invia_ mentre è connesso al sito di trading effettuerà la transazione. Josh si arricchisce.

> [!NOTE]
> Il trucco qui è che Josh non ha bisogno di avere accesso ai cookie dell'utente (o alle credenziali di accesso). Il browser dell'utente memorizza queste informazioni e le include automaticamente in tutte le richieste al server associato.

Un modo per prevenire questo tipo di attacco è che il server richieda che le richieste `POST` includano un segreto generato dal sito specifico dell'utente. Il segreto sarebbe fornito dal server quando invia il modulo web usato per effettuare i trasferimenti. Questo approccio impedisce a Josh di creare il proprio modulo, perché dovrebbe conoscere il segreto che il server sta fornendo per l'utente. Anche se trovasse il segreto e creasse un modulo per un particolare utente, non sarebbe più in grado di utilizzare lo stesso modulo per attaccare ogni utente.

I framework web spesso includono meccanismi di prevenzione CSRF.

### Altre minacce

Altri attacchi/vulnerabilità comuni includono:

- [Clickjacking](/it/docs/Web/Security/Attacks/Clickjacking). In questo attacco, un utente malintenzionato intercetta clic destinati a un sito di primo livello visibile e li instrada a una pagina nascosta sottostante. Questa tecnica potrebbe essere utilizzata, ad esempio, per visualizzare un sito bancario legittimo ma catturare le credenziali di accesso in un {{htmlelement("iframe")}} invisibile controllato dall'attaccante. Il clickjacking potrebbe anche essere usato per far sì che l'utente clicchi su un pulsante su un sito visibile, ma in realtà clicchi inavvertitamente su un pulsante completamente diverso. Come difesa, il tuo sito può evitare di essere incorporato in un iframe in un altro sito impostando gli appropriati header HTTP.
- {{Glossary("Distributed_Denial_of_Service", "Denial of Service")}} (DoS). Il DoS è solitamente ottenuto inondando un sito di destinazione con richieste false in modo che l'accesso a un sito sia interrotto per gli utenti legittimi. Le richieste possono essere numerose o possono singolarmente consumare grandi quantità di risorse (ad es., letture lente o caricamenti di file grandi). Le difese DoS di solito lavorano identificando e bloccando il traffico "cattivo" mentre consentono il passaggio dei messaggi legittimi. Queste difese sono in genere situate prima o nel server web (non fanno parte dell'applicazione web stessa).
- [Directory Traversal](https://en.wikipedia.org/wiki/Directory_traversal_attack) (File and disclosure). In questo attacco, un utente malintenzionato tenta di accedere a parti del file system del server web a cui non dovrebbe poter accedere. Questa vulnerabilità si verifica quando l'utente è in grado di passare nomi di file che includono caratteri di navigazione del file system (ad esempio, `../../`). La soluzione è sanitizzare l'input prima di utilizzarlo.
- [File Inclusion](https://en.wikipedia.org/wiki/File_inclusion_vulnerability). In questo attacco, un utente è in grado di specificare un file "non previsto" per la visualizzazione o l'esecuzione nei dati passati al server. Quando viene caricato, questo file potrebbe essere eseguito sul server web o lato client (portando a un attacco XSS). La soluzione è sanitizzare l'input prima di utilizzarlo.
- [Command Injection](https://owasp.org/www-community/attacks/Command_Injection). Gli attacchi di iniezione di comandi consentono a un utente malintenzionato di eseguire comandi di sistema arbitrari sul sistema operativo host. La soluzione è sanitizzare l'input dell'utente prima che possa essere usato nelle chiamate di sistema.

Per un elenco completo delle minacce alla sicurezza dei siti web vedi [Categoria: Sfruttamenti di sicurezza web](https://en.wikipedia.org/wiki/Category:Web_security_exploits) (Wikipedia) e [Categoria: Attacco](https://owasp.org/www-community/attacks/) (Open Web Application Security Project).

## Alcuni messaggi chiave

Quasi tutti gli exploit di sicurezza nelle sezioni precedenti hanno successo quando l'applicazione web si fida dei dati provenienti dal browser. Qualunque cosa tu faccia per migliorare la sicurezza del tuo sito web, dovresti sanitizzare tutti i dati provenienti dall'utente prima che siano visualizzati nel browser, utilizzati in query SQL, o passati a una chiamata di sistema operativo o file system.

> [!WARNING]
> La lezione più importante che puoi imparare sulla sicurezza del sito web è di **non fidarti mai dei dati provenienti dal browser**. Questo include, ma non si limita a dati nei parametri URL delle richieste `GET`, richieste `POST`, intestazioni HTTP e cookie, e file caricati dall'utente. Controlla e sanitizza sempre tutti i dati in ingresso. Presumi sempre il peggio.

Alcuni altri passaggi concreti che puoi intraprendere sono:

- Utilizza una gestione più efficace delle password. Incoraggia password forti. Considera l'autenticazione a due fattori per il tuo sito, in modo che oltre a una password l'utente debba inserire un altro codice di autenticazione (di solito uno che viene fornito tramite un qualche hardware fisico che solo l'utente avrà, come un codice in un SMS inviato al loro telefono).
- Configura il tuo server web per utilizzare {{Glossary("HTTPS", "HTTPS")}} e [HTTP Strict Transport Security](/it/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security) (HSTS). HTTPS cripta i dati inviati tra il tuo client e server. Questo assicura che le credenziali di accesso, i cookie, i dati delle richieste `POST` e le informazioni delle intestazioni non siano facilmente disponibili per gli attaccanti.
- Tieni traccia delle minacce più popolari (l'[elenco OWASP attuale è qui](https://owasp.org/www-project-top-ten/)) e affronta prima le vulnerabilità più comuni.
- Usa [strumenti di scansione delle vulnerabilità](https://owasp.org/www-community/Vulnerability_Scanning_Tools) per eseguire test di sicurezza automatizzati sul tuo sito. In seguito, il tuo sito web molto di successo potrebbe anche trovare bug offrendo una ricompensa per bug [come fa Mozilla qui](https://www.mozilla.org/en-US/security/bug-bounty/faq-webapp/).
- Conserva e visualizza solo i dati di cui hai bisogno. Ad esempio, se i tuoi utenti devono memorizzare informazioni sensibili come i dettagli della carta di credito, visualizza solo una parte del numero di carta che possa essere identificata dall'utente, e non abbastanza che possa essere copiata da un attaccante e utilizzata su un altro sito. Il pattern più comune al momento è di visualizzare solo le ultime 4 cifre di un numero di carta di credito.
- Mantieni il software aggiornato.
  La maggior parte dei server ha aggiornamenti di sicurezza regolari che risolvono o mitigano le vulnerabilità note.
  Se possibile, programma aggiornamenti automatici regolari, e idealmente, programma gli aggiornamenti durante i momenti in cui il tuo sito web ha il minor traffico.
  È meglio eseguire backup dei tuoi dati prima di aggiornare e testare le nuove versioni del software per assicurarti che non ci siano problemi di compatibilità sul tuo server.

I framework web possono aiutare a mitigare molte delle vulnerabilità più comuni.

## Sommario

Questo articolo ha spiegato il concetto di sicurezza web e alcune delle minacce più comuni contro le quali il tuo sito web dovrebbe tentare di proteggersi. La cosa più importante, dovresti capire che un'applicazione web non può fidarsi di nessun dato proveniente dal browser web. Tutti i dati utente dovrebbero essere sanitizzati prima di essere visualizzati, o utilizzati in query SQL e chiamate al file system.

Con questo articolo, sei arrivato alla fine di [questo modulo](/it/docs/Learn_web_development/Extensions/Server-side/First_steps), coprendo i tuoi primi passi nella programmazione di siti web lato server. Speriamo che tu abbia apprezzato l'apprendimento di questi concetti fondamentali e che ora sia pronto a selezionare un Web Framework e iniziare a programmare.

{{PreviousMenu("Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}
