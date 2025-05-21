---
title: Sicurezza delle applicazioni web Django
short-title: Sicurezza Django
slug: Learn_web_development/Extensions/Server-side/Django/web_application_security
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django/django_assessment_blog", "Learn_web_development/Extensions/Server-side/Django")}}

Proteggere i dati degli utenti è una parte essenziale del design di qualsiasi sito web. Abbiamo precedentemente spiegato alcune delle minacce alla sicurezza più comuni nell'articolo [Sicurezza web](/it/docs/Web/Security) — questo articolo fornisce una dimostrazione pratica di come le protezioni integrate di Django gestiscono tali minacce.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'argomento della programmazione lato server "<a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security">Sicurezza del sito web</a>".
        Completare i capitoli del tutorial Django fino (e inclusivamente) almeno a <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms">Django Tutorial Parte 9: Lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le principali attività necessarie per proteggere la tua applicazione web Django.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

L'argomento [Sicurezza del sito web](/it/docs/Web/Security) fornisce una panoramica di cosa significhi la sicurezza del sito web per il design lato server e di alcune delle minacce più comuni contro le quali dovresti proteggerti. Uno dei messaggi chiave in quell'articolo è che quasi tutti gli attacchi hanno successo quando l'applicazione web si fida dei dati dal browser.

> [!WARNING]
> La lezione più importante che puoi imparare sulla sicurezza del sito web è di **non fidarti mai dei dati provenienti dal browser**. Questo include i dati della richiesta `GET` nei parametri dell'URL, i dati `POST`, le intestazioni HTTP e i cookie, i file caricati dall'utente, ecc. Verifica e sanitizza sempre tutti i dati in arrivo. Supponete sempre il peggio.

La buona notizia per gli utenti di Django è che molte delle minacce più comuni sono gestite dal framework! L'articolo [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) (documentazione Django) spiega le funzionalità di sicurezza di Django e come proteggere un sito alimentato da Django.

## Minacce/protezione comuni

Piuttosto che duplicare la documentazione Django qui, in questo articolo dimostreremo alcune delle funzionalità di sicurezza nel contesto del nostro tutorial [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) di Django.

### Cross site scripting (XSS)

XSS è un termine utilizzato per descrivere una classe di attacchi che consentono a un aggressore di iniettare script lato client _attraverso_ il sito web nei browser di altri utenti. Questo viene generalmente realizzato memorizzando script dannosi nel database dove possono essere recuperati e visualizzati ad altri utenti, o facendo cliccare agli utenti un link che farà eseguire al browser dell'utente il JavaScript dell'aggressore.

Il sistema di template di Django ti protegge dalla maggior parte degli attacchi XSS [escapando caratteri specifici](https://docs.djangoproject.com/en/5.0/ref/templates/language/#automatic-html-escaping) che sono "pericolosi" in HTML. Possiamo dimostrare questo tentando di iniettare del JavaScript nel nostro sito LocalLibrary usando il modulo Crea-autore che abbiamo impostato in [Django Tutorial Parte 9: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms).

1. Avvia il sito web utilizzando il server di sviluppo (`python3 manage.py runserver`).
2. Apri il sito nel tuo browser locale ed effettua il login nel tuo account superuser.
3. Vai alla pagina di creazione dell'autore (che dovrebbe essere all'URL: `http://127.0.0.1:8000/catalog/author/create/`).
4. Inserisci nomi e dettagli della data per un nuovo utente, e poi aggiungi alla fine del campo Cognome il seguente testo:
   `<script>alert('Test alert');</script>`.
   ![Test XSS del modulo Autore](author_create_form_alert_xss.png)

   > [!NOTE]
   > Questo è uno script inoffensivo che, se eseguito, mostrerà una finestra di avviso nel tuo browser. Se l'avviso viene visualizzato quando si invia il record, allora il sito è vulnerabile alle minacce XSS.

5. Premi **Invia** per salvare il record.
6. Quando salvi l'autore verrà visualizzato come mostrato di seguito. A causa delle protezioni XSS, l'`alert()` non dovrebbe essere eseguito. Invece, lo script viene visualizzato come testo semplice.
   ![Vista dettagli dell'autore, test XSS](author_detail_alert_xss.png)

Se visualizzi il codice sorgente HTML della pagina, puoi vedere che i caratteri pericolosi per i tag script sono stati trasformati nei loro equivalenti codici di escape innocui (ad esempio, `>` è ora `&gt;`).

```html
<h1>
  Author: Boon&lt;script&gt;alert(&#39;Test alert&#39;);&lt;/script&gt;, David
  (Boonie)
</h1>
```

Utilizzando i template di Django, sei protetto contro la maggior parte degli attacchi XSS. Tuttavia, è possibile disattivare questa protezione e la protezione non è automaticamente applicata a tutti i tag che normalmente non sarebbero popolati da input dell'utente (ad esempio, il `help_text` in un campo modulo di solito non è fornito dall'utente, quindi Django non effettua il escaping di quei valori).

È anche possibile che gli attacchi XSS provengano da altre fonti di dati non fidate, come cookie, servizi Web o file caricati (ogni volta che i dati non sono sufficientemente sanificati prima di essere inclusi in una pagina). Se stai visualizzando dati provenienti da queste fonti, potrebbe essere necessario aggiungere il proprio codice di sanificazione.

### Protezione contro il Cross site request forgery (CSRF)

Gli attacchi CSRF permettono a un utente malintenzionato di eseguire azioni utilizzando le credenziali di un altro utente senza che questi ne sia a conoscenza o consenta. Ad esempio, considera il caso in cui abbiamo un hacker che vuole creare autori aggiuntivi per il nostro LocalLibrary.

> [!NOTE]
> Ovviamente il nostro hacker non è in questo per il denaro! Un hacker più ambizioso potrebbe utilizzare lo stesso approccio su altri siti per compiere azioni molto più dannose (come trasferire denaro sui propri conti, e così via).

Per fare ciò, potrebbe creare un file HTML come quello qui sotto, che contiene un modulo di creazione di autori (simile a quello che abbiamo usato nella sezione precedente) che viene inviato non appena il file viene caricato.
Avrebbero poi inviato il file a tutti i Bibliotecari suggerendo loro di aprirlo (contiene alcune informazioni innocue, onestamente!). Se il file viene aperto da qualsiasi bibliotecario loggato, il modulo sarebbe inviato con le loro credenziali e un nuovo autore sarebbe creato.

```html
<html lang="en">
  <body onload="document.EvilForm.submit()">
    <form
      action="http://127.0.0.1:8000/catalog/author/create/"
      method="post"
      name="EvilForm">
      <table>
        <tr>
          <th><label for="id_first_name">First name:</label></th>
          <td>
            <input
              id="id_first_name"
              maxlength="100"
              name="first_name"
              type="text"
              value="Mad"
              required />
          </td>
        </tr>
        <tr>
          <th><label for="id_last_name">Last name:</label></th>
          <td>
            <input
              id="id_last_name"
              maxlength="100"
              name="last_name"
              type="text"
              value="Man"
              required />
          </td>
        </tr>
        <tr>
          <th><label for="id_date_of_birth">Date of birth:</label></th>
          <td>
            <input id="id_date_of_birth" name="date_of_birth" type="text" />
          </td>
        </tr>
        <tr>
          <th><label for="id_date_of_death">Died:</label></th>
          <td>
            <input
              id="id_date_of_death"
              name="date_of_death"
              type="text"
              value="12/10/2016" />
          </td>
        </tr>
      </table>
      <input type="submit" value="Submit" />
    </form>
  </body>
</html>
```

Esegui il server web di sviluppo e accedi con il tuo account superuser. Copia il testo sopra in un file e poi aprilo nel browser. Dovresti ottenere un errore CSRF, perché Django ha una protezione contro questo tipo di cose!

Il modo in cui la protezione è abilitata è che includi il tag template `{% csrf_token %}` nella definizione del tuo modulo. Questo token viene quindi reso nel tuo HTML come mostrato di seguito, con un valore specifico per l'utente sul browser corrente.

```html
<input
  type="hidden"
  name="csrfmiddlewaretoken"
  value="0QRWHnYVg776y2l66mcvZqp8alrv4lb8S8lZ4ZJUWGZFA5VHrVfL2mpH29YZ39PW" />
```

Django genera una chiave specifica per utente/browser e rifiuterà i moduli che non contengono il campo, o che contengono un valore di campo errato per l'utente/browser.

Per utilizzare questo tipo di attacco l'hacker deve ora scoprire e includere la chiave CSRF per l'utente target specifico. Inoltre, non può usare l'approccio "scattergun" di inviare un file dannoso a tutti i bibliotecari sperando che uno di loro lo apra, poiché la chiave CSRF è specifica del browser.

La protezione CSRF di Django è attivata per impostazione predefinita. Dovresti sempre usare il tag template `{% csrf_token %}` nei tuoi moduli e utilizzare `POST` per le richieste che potrebbero modificare o aggiungere dati al database.

### Altre protezioni

Django offre anche altre forme di protezione (la maggior parte delle quali sarebbe difficile o non particolarmente utile da dimostrare):

- Protezione contro l'iniezione SQL
  - : Le vulnerabilità di iniezione SQL consentono agli utenti malintenzionati di eseguire codice SQL arbitrario su un database, consentendo l'accesso, la modifica o l'eliminazione dei dati indipendentemente dai permessi dell'utente. Quasi in tutti i casi accedi al database utilizzando i queryset/modelli di Django, quindi l'SQL risultante verrà correttamente escapato dal driver del database sottostante. Se hai bisogno di scrivere query raw o SQL personalizzato, dovrai pensare esplicitamente a prevenire l'iniezione SQL.
- Protezione contro il clickjacking
  - : In questo attacco un utente malintenzionato dirotta i clic destinati a un sito di primo livello visibile e li instrada verso una pagina nascosta sottostante. Questa tecnica potrebbe essere utilizzata, ad esempio, per visualizzare un sito bancario legittimo ma catturare le credenziali di accesso in un [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe) invisibile controllato dall'aggressore. Django contiene protezione [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) sotto forma di [`X-Frame-Options` middleware](https://docs.djangoproject.com/en/4.0/ref/middleware/#django.middleware.clickjacking.XFrameOptionsMiddleware) che, in un browser supportato, può prevenire che un sito venga renderizzato all'interno di un frame.
- Forzare TLS/HTTPS
  - : TLS/HTTPS può essere abilitato sul server web per crittografare tutto il traffico tra il sito e il browser, comprese le credenziali di autenticazione che altrimenti verrebbero inviate in chiaro (abilitare HTTPS è altamente raccomandato). Se HTTPS è abilitato, Django offre una serie di altre protezioni che puoi utilizzare:
    - [`SECURE_PROXY_SSL_HEADER`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_PROXY_SSL_HEADER) può essere utilizzato per verificare se il contenuto è sicuro, anche se proviene da un proxy non HTTP.
    - [`SECURE_SSL_REDIRECT`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_SSL_REDIRECT) viene utilizzato per reindirizzare tutte le richieste HTTP a HTTPS.
    - Utilizzare [HTTP Strict Transport Security](https://docs.djangoproject.com/en/5.0/ref/middleware/#http-strict-transport-security) (HSTS). Questo è un'intestazione HTTP che informa un browser che tutte le connessioni future a un particolare sito devono sempre utilizzare HTTPS. Combinato con il reindirizzamento delle richieste HTTP a HTTPS, questa impostazione garantisce che HTTPS venga sempre utilizzato dopo che una connessione è stata stabilita con successo. HSTS può essere configurato con [`SECURE_HSTS_SECONDS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_HSTS_SECONDS) e [`SECURE_HSTS_INCLUDE_SUBDOMAINS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_HSTS_INCLUDE_SUBDOMAINS) o sul server web.
    - Utilizzare cookie "sicuri" impostando a `True` [`SESSION_COOKIE_SECURE`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SESSION_COOKIE_SECURE) e [`CSRF_COOKIE_SECURE`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-CSRF_COOKIE_SECURE). Questo assicurerà che i cookie vengano inviati solo su HTTPS.
- Validazione dell'intestazione host
  - : Utilizza [`ALLOWED_HOSTS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-ALLOWED_HOSTS) per accettare richieste solo da host fidati.

Ci sono molte altre protezioni e avvertenze sull'utilizzo dei meccanismi sopra indicati. Mentre speriamo che questo abbia fornito una panoramica di ciò che Django offre, dovresti comunque leggere la documentazione sulla sicurezza di Django.

## Riepilogo

Django offre protezioni efficaci contro numerose minacce comuni, compresi attacchi XSS e CSRF. In questo articolo abbiamo dimostrato come queste particolari minacce siano gestite da Django nel nostro sito _LocalLibrary_. Abbiamo anche fornito una breve panoramica di alcune delle altre protezioni.

Questa è stata una breve incursione nella sicurezza web. Raccomandiamo vivamente di leggere [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) per ottenere una comprensione più approfondita.

Il prossimo e ultimo passo in questo modulo su Django è completare il [compito di valutazione](/it/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog).

## Vedi anche

- [Sicurezza sul web](/it/docs/Web/Security)
- [Guide pratiche all'implementazione della sicurezza](/it/docs/Web/Security/Practical_implementation_guides)
- [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django/django_assessment_blog", "Learn_web_development/Extensions/Server-side/Django")}}
