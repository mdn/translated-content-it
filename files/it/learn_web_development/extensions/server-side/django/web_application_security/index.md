---
title: Sicurezza dell'applicazione web Django
short-title: Sicurezza di Django
slug: Learn_web_development/Extensions/Server-side/Django/web_application_security
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django/django_assessment_blog", "Learn_web_development/Extensions/Server-side/Django")}}

Proteggere i dati degli utenti è una parte essenziale di qualsiasi progetto di sito web. In precedenza abbiamo spiegato alcune delle minacce alla sicurezza più comuni nell'articolo [Sicurezza Web](/it/docs/Web/Security) — questo articolo fornisce una dimostrazione pratica di come le protezioni integrate di Django gestiscono tali minacce.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'argomento della programmazione lato server "<a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security">Sicurezza del sito web</a>".
        Completare gli argomenti del tutorial di Django fino a (e incluso) almeno <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms">Django Tutorial Parte 9: Lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le principali cose che bisogna fare (o non fare) per proteggere la sua applicazione web Django.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

L'argomento [Sicurezza del sito web](/it/docs/Web/Security) offre una panoramica su cosa significhi sicurezza del sito web per la progettazione lato server e alcune delle minacce più comuni contro le quali dovrebbe proteggere. Uno dei messaggi chiave in quell'articolo è che quasi tutti gli attacchi hanno successo quando l'applicazione web si fida dei dati provenienti dal browser.

> [!WARNING]
> La lezione più importante che può imparare sulla sicurezza del sito web è di **non fidarsi mai dei dati provenienti dal browser**. Questo include dati di richieste `GET` nei parametri URL, dati `POST`, intestazioni HTTP e cookie, file caricati dall'utente, ecc. Controlli e sani sempre tutti i dati in entrata. Supponga sempre il peggio.

La buona notizia per gli utenti di Django è che molte delle minacce più comuni sono gestite dal framework! L'articolo [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) (documentazione Django) spiega le funzionalità di sicurezza di Django e come proteggere un sito web alimentato da Django.

## Minacce/protezioni comuni

Piuttosto che duplicare la documentazione di Django qui, in questo articolo mostreremo solo alcune delle funzionalità di sicurezza nel contesto del nostro tutorial Django [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

### Cross site scripting (XSS)

XSS è un termine usato per descrivere una classe di attacchi che permettono a un attaccante di iniettare script lato client _attraverso_ il sito web nei browser di altri utenti. Questo viene di solito realizzato memorizzando script dannosi nel database dove possono essere recuperati e visualizzati ad altri utenti, o inducendo gli utenti a fare clic su un link che farà eseguire il JavaScript dell'attaccante dal browser dell'utente.

Il sistema di template di Django la protegge dalla maggior parte degli attacchi XSS [evitando specifici caratteri](https://docs.djangoproject.com/en/5.0/ref/templates/language/#automatic-html-escaping) che sono "pericolosi" in HTML. Possiamo dimostrare questo cercando di iniettare del JavaScript nel nostro sito LocalLibrary utilizzando il modulo Crea-autore che abbiamo impostato in [Django Tutorial Parte 9: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms).

1. Avvii il sito utilizzando il server di sviluppo (`python3 manage.py runserver`).
2. Apra il sito nel suo browser locale ed acceda con il suo account superutente.
3. Navighi alla pagina di creazione dell'autore (che dovrebbe essere all'URL: `http://127.0.0.1:8000/catalog/author/create/`).
4. Inserisca nomi e dettagli di data per un nuovo utente, quindi aggiunga il testo seguente al campo Cognome:
   `<script>alert('Test alert');</script>`.
   ![Test XSS Form Autore](author_create_form_alert_xss.png)

   > [!NOTE]
   > Questo è uno script innocuo che, se eseguito, visualizzerà una finestra di avviso nel suo browser. Se l'avviso è visualizzato quando lei invia il record, il sito è vulnerabile alle minacce XSS.

5. Premi **Invia** per salvare il record.
6. Quando salva l'autore, verrà visualizzato come mostrato di seguito. A causa delle protezioni XSS, l'`alert()` non dovrebbe essere eseguito. Invece, lo script viene visualizzato come testo normale.
   ![Vista dettaglio autore test XSS](author_detail_alert_xss.png)

Se visualizza il codice sorgente HTML della pagina, può vedere che i caratteri pericolosi per i tag script sono stati trasformati nei loro equivalenti di escape code innocui (per esempio, `>` è ora `&gt;`).

```html
<h1>
  Author: Boon&lt;script&gt;alert(&#39;Test alert&#39;);&lt;/script&gt;, David
  (Boonie)
</h1>
```

Usare i template di Django la protegge dalla maggior parte degli attacchi XSS. Tuttavia, è possibile disattivare questa protezione, e la protezione non è applicata automaticamente a tutti i tag che normalmente non sarebbero popolati da input dell'utente (per esempio, il `help_text` in un campo modulo di solito non è fornito dall'utente, quindi Django non esegue l'escape di quei valori).

È anche possibile che attacchi XSS abbiano origine da altre fonti di dati non attendibili, come cookie, servizi Web o file caricati (ogni volta che i dati non sono sufficientemente sanitizzati prima di essere inclusi in una pagina). Se sta visualizzando dati da queste fonti, potrebbe dover aggiungere il proprio codice di sanificazione.

### Protezione contro la forgery di richiesta cross site (CSRF)

Gli attacchi CSRF permettono a un utente malintenzionato di eseguire azioni usando le credenziali di un altro utente senza la conoscenza o il consenso di quest'ultimo. Per esempio, consideri il caso in cui un hacker voglia creare autori aggiuntivi per il nostro LocalLibrary.

> [!NOTE]
> Ovviamente il nostro hacker non è in questo per soldi! Un hacker più ambizioso potrebbe usare lo stesso approccio su altri siti per eseguire compiti molto più dannosi (come trasferire denaro nei propri conti, e così via.)

Per fare ciò, potrebbe creare un file HTML come quello sottostante, che contiene un modulo di creazione autore (come quello che abbiamo usato nella sezione precedente) che viene inviato non appena il file viene caricato. Invierà quindi il file a tutti i bibliotecari suggerendo loro di aprire il file (contiene alcune informazioni innocue, onestamente!). Se il file è aperto da un bibliotecario connesso, il modulo sarà inviato con le loro credenziali e un nuovo autore sarà creato.

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

Corra il server web di sviluppo, e acceda con il suo account superutente. Copi il testo sopra in un file e poi lo apra nel browser. Dovrebbe ottenere un errore CSRF, perché Django offre protezione contro questo tipo di eventi!

Il modo in cui la protezione è attivata è incluso il tag template `{% csrf_token %}` nella sua definizione di modulo. Questo token viene quindi reso nel suo HTML come mostrato di seguito, con un valore che è specifico per l'utente nel browser corrente.

```html
<input
  type="hidden"
  name="csrfmiddlewaretoken"
  value="0QRWHnYVg776y2l66mcvZqp8alrv4lb8S8lZ4ZJUWGZFA5VHrVfL2mpH29YZ39PW" />
```

Django genera una chiave specifica per utente/browser e rifiuterà i moduli che non contengono il campo o che contengono un valore di campo errato per l'utente/browser.

Per utilizzare questo tipo di attacco, l'hacker deve ora scoprire e includere la chiave CSRF per l'utente specifico preso di mira. Inoltre, non può utilizzare l'approccio "scattergun" di inviare un file dannoso a tutti i bibliotecari sperando che uno di loro lo apra, poiché la chiave CSRF è specifica del browser.

La protezione CSRF di Django è attiva per default. Lei dovrebbe sempre utilizzare il tag template `{% csrf_token %}` nei suoi moduli e usare `POST` per le richieste che potrebbero cambiare o aggiungere dati al database.

### Altre protezioni

Django fornisce anche altre forme di protezione (la maggior parte delle quali sarebbe difficile o non particolarmente utile dimostrare):

- Protezione dall'iniezione SQL
  - : Le vulnerabilità di iniezione SQL permettono agli utenti malintenzionati di eseguire codice SQL arbitrario su un database, permettendo l'accesso, la modifica o l'eliminazione dei dati indipendentemente dai permessi dell'utente. In quasi tutti i casi accederà al database utilizzando i queryset/modelli di Django, quindi l'SQL risultante sarà opportunamente escapato dal driver del database sottostante. Se ha bisogno di scrivere query raw o SQL personalizzato, dovrà pensare esplicitamente a prevenire l'iniezione SQL.
- Protezione da clickjacking
  - : In questo attacco un utente malintenzionato dirotta i clic destinati a un sito di livello superiore visibile e li reindirizza a una pagina nascosta sotto. Questa tecnica potrebbe essere utilizzata, per esempio, per visualizzare un sito bancario legittimo ma catturare le credenziali di accesso in un [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe) invisibile controllato dall'attaccante. Django contiene protezione dal [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) sotto forma del [`X-Frame-Options` middleware](https://docs.djangoproject.com/en/4.0/ref/middleware/#django.middleware.clickjacking.XFrameOptionsMiddleware) che, in un browser supportato, può impedire che un sito venga reso all'interno di un frame.
- Applicazione di TLS/HTTPS
  - : TLS/HTTPS può essere abilitato sul server web per criptare tutto il traffico tra il sito e il browser, comprese le credenziali di autenticazione che altrimenti verrebbero inviate in testo semplice (abilitare HTTPS è altamente raccomandato). Se HTTPS è abilitato, Django fornisce una serie di altre protezioni che può utilizzare:
    - [`SECURE_PROXY_SSL_HEADER`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_PROXY_SSL_HEADER) può essere usato per controllare se il contenuto è sicuro, anche se proviene da un proxy non HTTP.
    - [`SECURE_SSL_REDIRECT`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_SSL_REDIRECT) viene usato per reindirizzare tutte le richieste HTTP a HTTPS.
    - Usare [HTTP Strict Transport Security](https://docs.djangoproject.com/en/5.0/ref/middleware/#http-strict-transport-security) (HSTS). Questo è un'intestazione HTTP che informa un browser che tutte le connessioni future a un particolare sito devono sempre utilizzare HTTPS. Combinato con il reindirizzamento delle richieste HTTP a HTTPS, questa impostazione garantisce che HTTPS sia sempre utilizzato dopo che si è verificata una connessione di successo. L'HSTS può essere configurato con [`SECURE_HSTS_SECONDS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_HSTS_SECONDS) and [`SECURE_HSTS_INCLUDE_SUBDOMAINS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_HSTS_INCLUDE_SUBDOMAINS) o sul server Web.
    - Usare cookie "sicuri" impostando [`SESSION_COOKIE_SECURE`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SESSION_COOKIE_SECURE) e [`CSRF_COOKIE_SECURE`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-CSRF_COOKIE_SECURE) a `True`. Questo garantirà che i cookie siano inviati solo tramite HTTPS.
- Validazione dell'intestazione dell'host
  - : Utilizzare [`ALLOWED_HOSTS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-ALLOWED_HOSTS) per accettare solo richieste da host attendibili.

Esistono molte altre protezioni e avvertenze all'uso dei meccanismi sopra elencati. Sebbene speriamo che questo le abbia fornito una panoramica di ciò che Django offre, dovrebbe comunque leggere la documentazione sulla sicurezza di Django.

## Riepilogo

Django ha protezioni efficaci contro numerose minacce comuni, tra cui attacchi XSS e CSRF. In questo articolo abbiamo dimostrato come quelle particolari minacce sono gestite da Django nel nostro sito _LocalLibrary_. Abbiamo anche fornito una breve panoramica di alcune delle altre protezioni.

Questa è stata una incursione molto breve nella sicurezza web. Consigliamo vivamente di leggere [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) per ottenere una comprensione più profonda.

Il passo successivo e finale in questo modulo su Django è completare la [attività di valutazione](/it/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog).

## Vedere anche

- [Sicurezza sul web](/it/docs/Web/Security)
- [Guide pratiche all'implementazione della sicurezza](/it/docs/Web/Security/Practical_implementation_guides)
- [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django/django_assessment_blog", "Learn_web_development/Extensions/Server-side/Django")}}
