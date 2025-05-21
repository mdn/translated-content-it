---
title: "Tutorial di Django Parte 8: Autenticazione e permessi utente"
short-title: "8: Autenticazione e permessi"
slug: Learn_web_development/Extensions/Server-side/Django/Authentication
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django")}}

In questo tutorial, mostreremo come consentire agli utenti di accedere al tuo sito con i propri account e come controllare ciò che possono fare e vedere in base al fatto che siano o meno connessi e in base ai loro _permessi_. Come parte di questa dimostrazione, estenderemo il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo pagine di login e logout, e pagine specifiche per utenti e staff per visualizzare i libri che sono stati presi in prestito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerrequisiti:</th>
      <td>
        Completa tutti i precedenti argomenti del tutorial, fino a includere <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions">Django Tutorial Parte 7: Framework delle sessioni</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come configurare e utilizzare l'autenticazione e i permessi utente.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Django fornisce un sistema di autenticazione e autorizzazione ("permessi"), costruito sopra il framework delle sessioni discusso nel [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions), che consente di verificare le credenziali degli utenti e di definire quali azioni ciascun utente è autorizzato a eseguire. Il framework include modelli integrati per `Users` e `Groups` (un modo generico per applicare permessi a più di un utente alla volta), permessi/flag che designano se un utente può eseguire un'attività, moduli e viste per il login degli utenti e strumenti per limitare i contenuti.

> [!NOTE]
> Secondo Django, il sistema di autenticazione mira a essere molto generico e quindi non fornisce alcune funzionalità offerte da altri sistemi di autenticazione web. Soluzioni per alcuni problemi comuni sono disponibili come pacchetti di terze parti. Ad esempio, il {{Glossary("throttle", "throttling")}} dei tentativi di login e l'autenticazione contro terze parti (ad esempio, OAuth).

In questo tutorial, ti mostreremo come abilitare l'autenticazione degli utenti nel sito LocalLibrary, creare le tue pagine di login e logout, aggiungere permessi ai tuoi modelli e controllare l'accesso alle pagine. Utilizzeremo l'autenticazione/permessi per visualizzare elenchi di libri che sono stati presi in prestito sia per gli utenti sia per i bibliotecari.

Il sistema di autenticazione è molto flessibile, e puoi costruire i tuoi URL, moduli, viste e template da zero se desideri, solo chiamando l'API fornita per accedere all'utente. Tuttavia, in questo articolo, utilizzeremo le viste e i moduli di autenticazione "standard" di Django per le nostre pagine di login e logout. Avremo comunque bisogno di creare alcuni template, ma ciò è abbastanza semplice.

Mostreremo anche come creare permessi e verificare lo stato del login e i permessi sia nelle viste che nei template.

## Abilitare l'autenticazione

L'autenticazione è stata abilitata automaticamente quando abbiamo [creato lo scheletro del sito web](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (nel tutorial 2), quindi non è necessario fare nulla in più a questo punto.

> [!NOTE]
> La configurazione necessaria è stata tutta effettuata per noi quando abbiamo creato l'app usando il comando `django-admin startproject`. Le tabelle del database per gli utenti e i permessi dei modelli sono state create quando abbiamo chiamato per la prima volta `python manage.py migrate`.

La configurazione è impostata nelle sezioni `INSTALLED_APPS` e `MIDDLEWARE` del file di progetto (**django-locallibrary-tutorial/locallibrary/settings.py**), come mostrato di seguito:

```python
INSTALLED_APPS = [
    # …
    'django.contrib.auth',  # Core authentication framework and its default models.
    'django.contrib.contenttypes',  # Django content type system (allows permissions to be associated with models).
    # …

MIDDLEWARE = [
    # …
    'django.contrib.sessions.middleware.SessionMiddleware',  # Manages sessions across requests
    # …
    'django.contrib.auth.middleware.AuthenticationMiddleware',  # Associates users with requests using sessions.
    # …
```

## Creazione di utenti e gruppi

Hai già creato il tuo primo utente quando abbiamo visto il [sito di amministrazione di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site) nel tutorial 4 (questo era un superutente, creato con il comando `python manage.py createsuperuser`). Il nostro superutente è già autenticato e ha tutti i permessi, quindi dovremo creare un utente di test per rappresentare un normale utente del sito. Utilizzeremo il sito di amministrazione per creare i nostri gruppi _locallibrary_ e i login al sito web, poiché è uno dei modi più rapidi per farlo.

> [!NOTE]
> Puoi anche creare utenti in modo programmatico come mostrato di seguito. Dovresti fare questo, ad esempio, se stai sviluppando un'interfaccia per consentire agli utenti "ordinari" di creare i propri login (non dovresti dare accesso alla maggior parte degli utenti al sito di amministrazione).
>
> ```python
> from django.contrib.auth.models import User
>
> # Crea l'utente e salva nel database
> user = User.objects.create_user('myusername', 'myemail@crazymail.com', 'mypassword')
>
> # Aggiorna i campi e salva di nuovo
> user.first_name = 'Tyrone'
> user.last_name = 'Citizen'
> user.save()
> ```
>
> Nota, però, che è altamente raccomandato configurare un _modello utente personalizzato_ quando si avvia un progetto, poiché sarà possibile personalizzarlo facilmente in futuro se necessario. Se si utilizza un modello utente personalizzato, il codice per creare lo stesso utente sarebbe simile a questo:
>
> ```python
> # Ottieni il modello utente corrente dalle impostazioni
> from django.contrib.auth import get_user_model
> User = get_user_model()
>
> # Crea l'utente dal modello e salva nel database
> user = User.objects.create_user('myusername', 'myemail@crazymail.com', 'mypassword')
>
> # Aggiorna i campi e salva di nuovo
> user.first_name = 'Tyrone'
> user.last_name = 'Citizen'
> user.save()
> ```
>
> Per ulteriori informazioni, vedi [Usare un modello utente personalizzato quando si inizia un progetto](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project) (documentazione di Django).

Di seguito creeremo prima un gruppo e poi un utente. Anche se non abbiamo ancora permessi da aggiungere per i nostri membri della biblioteca, se avremo bisogno di farlo in seguito, sarà molto più facile aggiungerli una volta al gruppo piuttosto che singolarmente a ciascun membro.

Avvia il server di sviluppo e naviga nel sito di amministrazione nel tuo browser web locale (`http://127.0.0.1:8000/admin/`). Accedi al sito utilizzando le credenziali del tuo account superutente. Il livello superiore del sito Admin visualizza tutti i tuoi modelli, ordinati per "applicazione di Django". Dalla sezione **Autenticazione e Autorizzazione**, puoi cliccare sui link **Users** o **Groups** per vedere i loro record esistenti.

![Sito Admin - aggiungi gruppi o utenti](admin_authentication_add.png)

Per iniziare, creiamo un nuovo gruppo per i nostri membri della biblioteca.

1. Clicca sul pulsante **Add** (accanto a Group) per creare un nuovo _Gruppo_; inserisci il **Nome** "Membri della Biblioteca" per il gruppo.
   ![Sito Admin - aggiungi gruppo](admin_authentication_add_group.png)
2. Non abbiamo bisogno di permessi per il gruppo, quindi premi semplicemente **SAVE** (sarai portato a un elenco di gruppi).

Ora creiamo un utente:

1. Torna alla home page del sito di amministrazione
2. Clicca sul pulsante **Add** accanto a _Users_ per aprire la finestra di dialogo _Add user_.
   ![Sito Admin - aggiungi utente pt1](admin_authentication_add_user_prt1.png)
3. Inserisci un **Username** appropriato e una **Password**/**Conferma Password** per il tuo utente di test
4. Premi **SAVE** per creare l'utente.

   Il sito di amministrazione creerà il nuovo utente e ti porterà immediatamente a una schermata _Change user_ dove potrai cambiare il tuo **username** e aggiungere informazioni per i campi opzionali del modello User. Questi campi includono nome, cognome, indirizzo email, stato e permessi dell'utente (deve essere impostato solo il flag **Attivo**). Più in basso puoi specificare gruppi e permessi dell'utente, e vedere date importanti relative all'utente (ad es., data di adesione e ultima data di accesso).
   ![Sito Admin - aggiungi utente pt2](admin_authentication_add_user_prt2.png)

5. Nella sezione _Groups_, seleziona il gruppo **Library Member** dall'elenco di _Available groups_ e poi premi la **freccia destra** tra le caselle per spostarlo nella casella _Chosen groups_.
   ![Sito Admin - aggiungi utente al gruppo](admin_authentication_user_add_group.png)
6. Non è necessario fare altro qui, quindi seleziona semplicemente **SAVE** di nuovo, per andare all'elenco degli utenti.

Questo è tutto! Ora hai un account "normale membro della biblioteca" che potrai utilizzare per il test (una volta che avremo implementato le pagine per consentire loro di accedere).

> [!NOTE]
> Dovresti provare a creare un altro utente membro della biblioteca. Inoltre, crea un gruppo per i Bibliotecari e aggiungi un utente anche lì!

## Configurazione delle viste di autenticazione

Django fornisce quasi tutto ciò di cui hai bisogno per creare pagine di autenticazione per gestire login, logout e gestione delle password "pronti all'uso". Questo include un mapper URL, viste e moduli, ma non include i template — dovremo creare i nostri!

In questa sezione, mostriamo come integrare il sistema predefinito nel sito _LocalLibrary_ e creare i template. Li metteremo negli URL principali del progetto.

> [!NOTE]
> Non devi utilizzare nessuno di questo codice, ma è probabile che tu voglia farlo perché rende le cose molto più facili.
> Sarà quasi certamente necessario modificare il codice di gestione del modulo se cambi il tuo modello utente, ma anche in tal caso, potresti comunque usare le funzioni di visualizzazione standard.

> [!NOTE]
> In questo caso, potremmo ragionevolmente mettere le pagine di autenticazione, inclusi gli URL e i template, all'interno della nostra applicazione catalog.
> Tuttavia, se avessimo più applicazioni, sarebbe meglio separare questo comportamento di login condiviso e averlo disponibile in tutto il sito, quindi questo è ciò che abbiamo mostrato qui!

### URL di progetto

Aggiungi il seguente codice alla fine del file urls.py del progetto (**django-locallibrary-tutorial/locallibrary/urls.py**):

```python
# Add Django site authentication urls (for login, logout, password management)

urlpatterns += [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

Naviga all'URL `http://127.0.0.1:8000/accounts/` (nota la barra in avanti finale!).
Django mostrerà un errore che indica che non è possibile trovare una mappatura per questo URL e elenca tutti gli URL che ha provato.
Da questo puoi vedere gli URL che funzioneranno una volta che avremo creato i template.

> [!NOTE]
> L'aggiunta del percorso `accounts/` come mostrato sopra aggiunge i seguenti URL, insieme ai nomi (indicati tra parentesi quadre) che possono essere utilizzati per invertire le mappature degli URL. Non devi implementare nient'altro — la mappatura degli URL sopra riportata mappa automaticamente gli URL menzionati di seguito.
>
> ```python
> accounts/ login/ [name='login']
> accounts/ logout/ [name='logout']
> accounts/ password_change/ [name='password_change']
> accounts/ password_change/done/ [name='password_change_done']
> accounts/ password_reset/ [name='password_reset']
> accounts/ password_reset/done/ [name='password_reset_done']
> accounts/ reset/<uidb64>/<token>/ [name='password_reset_confirm']
> accounts/ reset/done/ [name='password_reset_complete']
> ```

Ora prova a navigare all'URL di accesso (`http://127.0.0.1:8000/accounts/login/`). Anche questo fallirà, ma con un errore che ti dirà che ci manca il template richiesto (**registration/login.html**) nel percorso di ricerca del template.
Vedrai le seguenti righe elencate nella sezione gialla in alto:

```python
Exception Type:    TemplateDoesNotExist
Exception Value:    registration/login.html
```

Il prossimo passo è creare una directory per i template chiamata "registration" e poi aggiungere il file **login.html**.

### Directory dei template

Gli URL (e implicitamente, le viste) che abbiamo appena aggiunto si aspettano di trovare i loro template associati in una directory **/registration/** da qualche parte nel percorso di ricerca dei template.

Per questo sito, metteremo le nostre pagine HTML nella directory **templates/registration/**. Questa directory dovrebbe trovarsi nella directory radice del tuo progetto, cioè la stessa directory delle cartelle **catalog** e **locallibrary**. Crea queste cartelle ora.

> [!NOTE]
> La tua struttura delle cartelle dovrebbe essere simile a quella sotto:
>
> ```plain
> django-locallibrary-tutorial/   # Cartella del progetto di Django a livello superiore
>   catalog/
>   locallibrary/
>   templates/
>     registration/
> ```

Per rendere la directory **templates** visibile al caricatore di template, dobbiamo aggiungerla nel percorso di ricerca dei template.
Apri le impostazioni del progetto (**/django-locallibrary-tutorial/locallibrary/settings.py**).

Quindi importa il modulo `os` (aggiungi la seguente riga vicino alla parte superiore del file se non è già presente).

```python
import os # needed by code below
```

Aggiorna la riga `'DIRS'` nella sezione `TEMPLATES` come mostrato:

```python
    # …
    TEMPLATES = [
      {
       # …
       'DIRS': [os.path.join(BASE_DIR, 'templates')],
       'APP_DIRS': True,
       # …
```

### Template di login

> [!WARNING]
> I template di autenticazione forniti in questo articolo sono una versione molto basilare/leggermente modificata dei template di dimostrazione del login di Django. Potresti doverli personalizzare per il tuo uso!

Crea un nuovo file HTML denominato /**django-locallibrary-tutorial/templates/registration/login.html** e inserisci il seguente contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}

  {% if form.errors %}
    <p>Your username and password didn't match. Please try again.</p>
  {% endif %}

  {% if next %}
    {% if user.is_authenticated %}
      <p>Your account doesn't have access to this page. To proceed,
      please login with an account that has access.</p>
    {% else %}
      <p>Please login to see this page.</p>
    {% endif %}
  {% endif %}

  <form method="post" action="{% url 'login' %}">
    {% csrf_token %}
    <table>
      <tr>
        <td>\{{ form.username.label_tag }}</td>
        <td>\{{ form.username }}</td>
      </tr>
      <tr>
        <td>\{{ form.password.label_tag }}</td>
        <td>\{{ form.password }}</td>
      </tr>
    </table>
    <input type="submit" value="login">
    <input type="hidden" name="next" value="\{{ next }}">
  </form>

  {# Assumes you set up the password_reset view in your URLConf #}
  <p><a href="{% url 'password_reset' %}">Lost password?</a></p>

{% endblock %}
```

Questo template condivide alcune somiglianze con quelli che abbiamo visto prima — estende il nostro template base e sovrascrive il blocco `content`. Il resto del codice è abbastanza standard per la gestione dei moduli, che discuteremo in un tutorial successivo. Tutto quello che devi sapere per ora è che questo visualizzerà un modulo in cui puoi inserire il tuo nome utente e password, e che se inserisci valori non validi, ti verrà richiesto di inserire valori corretti quando la pagina si aggiorna.

Torna alla pagina di accesso (`http://127.0.0.1:8000/accounts/login/`) una volta che hai salvato il tuo template, e dovresti vedere qualcosa di simile a questo:

![Pagina di login della biblioteca v1](library_login.png)

Se accedi utilizzando credenziali valide, sarai reindirizzato a un'altra pagina (per impostazione predefinita, questa sarà `http://127.0.0.1:8000/accounts/profile/`). Il problema è che, per impostazione predefinita, Django si aspetta che, una volta effettuato l'accesso, tu venga portato a una pagina del profilo, il che potrebbe o meno essere il caso. Poiché non hai ancora definito questa pagina, riceverai un altro errore!

Apri le impostazioni del progetto (**/django-locallibrary-tutorial/locallibrary/settings.py**) e aggiungi il testo di seguito alla fine. Ora, quando effettui l'accesso, dovresti essere reindirizzato alla homepage del sito per impostazione predefinita.

```python
# Redirect to home URL after login (Default redirects to /accounts/profile/)
LOGIN_REDIRECT_URL = '/'
```

### Template di logout

Se navighi all'URL di logout (`http://127.0.0.1:8000/accounts/logout/`), otterrai un errore perché Django 5 non consente il logout utilizzando `GET`, ma solo `POST`.
Aggiungeremo un modulo che puoi usare per fare il logout in un minuto, ma prima creeremo la pagina a cui gli utenti vengono reindirizzati dopo il logout.

Crea e apri **/django-locallibrary-tutorial/templates/registration/logged_out.html**. Copia il testo di seguito:

```django
{% extends "base_generic.html" %}

{% block content %}
  <p>Logged out!</p>
  <a href="{% url 'login'%}">Click here to login again.</a>
{% endblock %}
```

Questo template è molto semplice. Mostra solo un messaggio che ti informa che ti sei disconnesso e fornisce un link che puoi premere per tornare alla schermata di login. La schermata viene resa come segue (dopo il logout):

![Pagina di logout della biblioteca v1](library_logout.png)

### Template per il reset della password

Il sistema di reset della password predefinito utilizza l'email per inviare all'utente un link per il reset. Devi creare moduli per ottenere l'indirizzo email dell'utente, inviare l'email, consentire loro di inserire una nuova password e notare quando il processo è completo.

I template seguenti possono essere utilizzati come punto di partenza.

#### Modulo di reset della password

Questo è il modulo utilizzato per ottenere l'indirizzo email dell'utente (per l'invio dell'email di reset della password). Crea **/django-locallibrary-tutorial/templates/registration/password_reset_form.html**, e dai questo contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}
  <form action="" method="post">
  {% csrf_token %}
  {% if form.email.errors %}
    \{{ form.email.errors }}
  {% endif %}
      <p>\{{ form.email }}</p>
    <input type="submit" class="btn btn-default btn-lg" value="Reset password">
  </form>
{% endblock %}
```

#### Reset della password completato

Questo modulo viene visualizzato dopo che l'indirizzo email è stato raccolto. Crea **/django-locallibrary-tutorial/templates/registration/password_reset_done.html**, e dai questo contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}
  <p>We've emailed you instructions for setting your password. If they haven't arrived in a few minutes, check your spam folder.</p>
{% endblock %}
```

#### Email di reset della password

Questo template fornisce il testo dell'email HTML che contiene il link per il reset da inviare agli utenti. Crea **/django-locallibrary-tutorial/templates/registration/password_reset_email.html**, e dai questo contenuto:

```django
Someone asked for password reset for email \{{ email }}. Follow the link below:
\{{ protocol }}://\{{ domain }}{% url 'password_reset_confirm' uidb64=uid token=token %}
```

#### Conferma del reset della password

Questa pagina è dove inserisci la tua nuova password dopo aver cliccato il link nell'email di reset della password. Crea **/django-locallibrary-tutorial/templates/registration/password_reset_confirm.html**, e dai questo contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}
    {% if validlink %}
        <p>Please enter (and confirm) your new password.</p>
        <form action="" method="post">
        {% csrf_token %}
            <table>
                <tr>
                    <td>\{{ form.new_password1.errors }}
                        <label for="id_new_password1">New password:</label></td>
                    <td>\{{ form.new_password1 }}</td>
                </tr>
                <tr>
                    <td>\{{ form.new_password2.errors }}
                        <label for="id_new_password2">Confirm password:</label></td>
                    <td>\{{ form.new_password2 }}</td>
                </tr>
                <tr>
                    <td></td>
                    <td><input type="submit" value="Change my password"></td>
                </tr>
            </table>
        </form>
    {% else %}
        <h1>Password reset failed</h1>
        <p>The password reset link was invalid, possibly because it has already been used. Please request a new password reset.</p>
    {% endif %}
{% endblock %}
```

#### Reset della password completato

Questo è l'ultimo template di reset della password, che viene visualizzato per notificare quando il reset della password ha avuto successo. Crea **/django-locallibrary-tutorial/templates/registration/password_reset_complete.html**, e dai questo contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>The password has been changed!</h1>
  <p><a href="{% url 'login' %}">log in again?</a></p>
{% endblock %}
```

### Test delle nuove pagine di autenticazione

Ora che hai aggiunto la configurazione degli URL e creato tutti questi template, le pagine di autenticazione (ad eccezione del logout) dovrebbero ora funzionare!

Puoi testare le nuove pagine di autenticazione tentando prima di accedere al tuo account superutente utilizzando l'URL `http://127.0.0.1:8000/accounts/login/`.
Potrai testare la funzionalità di reset della password dal link nella pagina di login. **Sii consapevole che Django invierà le email di reset solo agli indirizzi (utenti) già memorizzati nel suo database!**

Tieni presente che non potrai testare il logout dell'account ancora, perché le richieste di logout devono essere inviate come `POST` piuttosto che una richiesta `GET`.

> [!NOTE]
> Il sistema di reset della password richiede che il tuo sito web supporti l'email, il che è al di fuori dello scopo di questo articolo, quindi questa parte **non funzionerà ancora**. Per consentire il test, metti la seguente riga alla fine del tuo file settings.py. Questo registrerà tutte le email inviate nella console (così potrai copiare il link per il reset della password dalla console).
>
> ```python
> EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
> ```
>
> Per ulteriori informazioni, vedi [Invio di email](https://docs.djangoproject.com/en/5.0/topics/email/) (documentazione di Django).

## Test sui utenti autenticati

Questa sezione esamina cosa possiamo fare per controllare selettivamente i contenuti che l'utente vede in base al fatto che sia connesso o meno.

### Test nei template

Puoi ottenere informazioni sull'utente attualmente connesso nei template con la variabile di template `\{{ user }}` (questa viene aggiunta al contesto del template per impostazione predefinita quando imposti il progetto come abbiamo fatto nel nostro scheletro).

Tipicamente, testerai prima la variabile del template `\{{ user.is_authenticated }}` per determinare se l'utente è idoneo a vedere specifici contenuti. Per dimostrarlo, aggiorneremo il nostro sidebar per visualizzare un link "Login" se l'utente è disconnesso e un link "Logout" se è connesso.

Apri il template base (**/django-locallibrary-tutorial/catalog/templates/base_generic.html**) e copia il seguente testo nel blocco `sidebar`, immediatamente prima del tag di template `endblock`.

```django
  <ul class="sidebar-nav">
    …
   {% if user.is_authenticated %}
     <li>User: \{{ user.get_username }}</li>
     <li>
       <form id="logout-form" method="post" action="{% url 'logout' %}">
         {% csrf_token %}
         <button type="submit" class="btn btn-link">Logout</button>
       </form>
     </li>
   {% else %}
     <li><a href="{% url 'login' %}?next=\{{ request.path }}">Login</a></li>
   {% endif %}
    …
  </ul>
```

Come puoi vedere, usiamo i tag di template `if` / `else` / `endif` per visualizzare testualmente in modo condizionato in base al fatto che `\{{ user.is_authenticated }}` sia vero. Se l'utente è autenticato, sappiamo che abbiamo un utente valido, quindi chiamiamo `\{{ user.get_username }}` per visualizzare il suo nome.

Creiamo l'URL del link di accesso utilizzando il tag di template `url` e il nome della configurazione dell'URL `login`. Nota anche come abbiamo aggiunto `?next=\{{ request.path }}` alla fine dell'URL. Quello che fa è aggiungere un parametro URL `next` contenente l'indirizzo (URL) della pagina _corrente_, alla fine dell'URL collegato. Dopo che l'utente ha effettuato l'accesso con successo, la vista utilizzerà questo valore `next` per ridirigere l'utente alla pagina in cui ha cliccato per la prima volta il link di accesso.

Il codice del template di logout è diverso, perché da Django 5 per disconnettersi è necessario eseguire un `POST` all'URL `admin:logout`, utilizzando un modulo con un pulsante.
Per impostazione predefinita, questo verrebbe reso come un pulsante, ma puoi stilizzare il pulsante per visualizzarlo come un link.
Per questo esempio, stiamo usando _Bootstrap_, quindi facciamo apparire il pulsante come un link applicando `class="btn btn-link"`.
È inoltre necessario aggiungere i seguenti stili a **/django-locallibrary-tutorial/catalog/static/css/styles.css** per posizionare correttamente il link di logout accanto a tutti gli altri link della barra laterale:

```css
#logout-form {
  display: inline;
}
#logout-form button {
  padding: 0;
  margin: 0;
}
```

Provalo facendo clic sui link di Login/Logout nella barra laterale.
Dovresti essere portato alle pagine di logout/login che hai definito nella [Directory dei template](#directory_dei_template) sopra.

### Test nelle viste

Se stai usando viste basate su funzioni, il modo più semplice per limitare l'accesso alle tue funzioni è applicare il decoratore `login_required` alla tua funzione di vista, come mostrato di seguito. Se l'utente è connesso, il codice della tua vista verrà eseguito normalmente. Se l'utente non è connesso, sarà reindirizzato all'URL di accesso definito nelle impostazioni del progetto (`settings.LOGIN_URL`), passando il percorso assoluto corrente come parametro URL `next`. Se l'utente riesce a effettuare l'accesso, verrà restituito a questa pagina, ma questa volta autenticato.

```python
from django.contrib.auth.decorators import login_required

@login_required
def my_view(request):
    # …
```

> [!NOTE]
> Puoi fare la stessa cosa manualmente testando su `request.user.is_authenticated`, ma il decoratore è molto più conveniente!

Analogamente, il modo più semplice per limitare l'accesso agli utenti connessi nelle tue viste basate su classi è derivare da `LoginRequiredMixin`. È necessario dichiarare questo mixin per primo nell'elenco delle superclassi, prima della classe di vista principale.

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class MyView(LoginRequiredMixin, View):
    # …
```

Questo ha esattamente lo stesso comportamento di reindirizzamento del decoratore `login_required`. Puoi anche specificare una posizione alternativa per reindirizzare l'utente se non è autenticato (`login_url`) e un nome di parametro URL diverso da `next` per inserire il percorso assoluto corrente (`redirect_field_name`).

```python
class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
```

Per dettagli aggiuntivi, consulta la [documentazione di Django qui](https://docs.djangoproject.com/en/5.0/topics/auth/default/#limiting-access-to-logged-in-users).

## Esempio — elenco dei libri dell'utente corrente

Ora che sappiamo come limitare una pagina a un particolare utente, creiamo una vista dei libri che l'utente corrente ha preso in prestito.

Sfortunatamente, non abbiamo ancora alcun modo per far prendere in prestito i libri agli utenti! Quindi, prima di poter creare l'elenco dei libri, estenderemo il modello `BookInstance` per supportare il concetto di prestito e utilizzeremo l'applicazione Admin di Django per prestare un numero di libri al nostro utente di test.

### Modelli

Per prima cosa, dovremo rendere possibile per gli utenti avere un `BookInstance` in prestito (abbiamo già un `status` e una data `due_back`, ma non abbiamo ancora alcuna associazione tra questo modello e un particolare utente. Ne creeremo una utilizzando un campo `ForeignKey` (uno-a-molti). Abbiamo anche bisogno di un meccanismo semplice per testare se un libro prestato è in ritardo.

Apri **catalog/models.py**, e importa le `settings` da `django.conf` (aggiungi questo appena sotto la linea di importazione precedente nella parte superiore del file, in modo che le impostazioni siano disponibili per il codice successivo che ne fa uso):

```python
from django.conf import settings
```

Successivamente, aggiungi il campo `borrower` al modello `BookInstance`, impostando il modello utente per la chiave come valore dell'impostazione `AUTH_USER_MODEL`.
Poiché non abbiamo sovrascritto l'impostazione con un [modello utente personalizzato](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/), questo si mappa al modello predefinito `User` da `django.contrib.auth.models`.

```python
borrower = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.SET_NULL, null=True, blank=True)
```

> [!NOTE]
> Importare il modello in questo modo riduce il lavoro richiesto se in seguito scopri che hai bisogno di un modello utente personalizzato.
> Questo tutorial utilizza il modello predefinito, quindi potresti invece importare il modello `User` direttamente con le seguenti righe:
>
> ```python
> from django.contrib.auth.models import User
> ```
>
> ```python
> borrower = models.ForeignKey(User, on_delete=models.SET_NULL, null=True, blank=True)
> ```

Mentre siamo qui, aggiungiamo una proprietà che possiamo chiamare dai nostri template per capire se un particolare istanza di libro è in ritardo.
Sebbene potremmo calcolare questo nel template stesso, usare una [proprietà](https://docs.python.org/3/library/functions.html#property) come mostrato di seguito sarà molto più efficiente.

Aggiungi questo da qualche parte vicino alla parte superiore del file:

```python
from datetime import date
```

Ora aggiungi la seguente definizione di proprietà alla classe `BookInstance`:

> [!NOTE]
> Il seguente codice utilizza la funzione `bool()` di Python, che valuta un oggetto o l'oggetto risultante di un'espressione e restituisce `True` a meno che il risultato non sia "falso", nel qual caso restituisce `False`.
> In Python, un oggetto è _falso_ (si valuta come `False`) se è: vuoto (come `[]`, `()`, `{}`), `0`, `None` o se è `False`.

```python
@property
def is_overdue(self):
    """Determines if the book is overdue based on due date and current date."""
    return bool(self.due_back and date.today() > self.due_back)
```

> [!NOTE]
> Verifichiamo prima se `due_back` è vuoto prima di fare un confronto. Un campo `due_back` vuoto causerebbe un errore a Django invece di mostrare la pagina: i valori vuoti non sono comparabili. Questo non è qualcosa che vorremmo che i nostri utenti sperimentassero!

Ora che abbiamo aggiornato i nostri modelli, dovremo effettuare nuove migrazioni sul progetto e poi applicare quelle migrazioni:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

### Admin

Ora apri **catalog/admin.py**, e aggiungi il campo `borrower` alla classe `BookInstanceAdmin` sia nella `list_display` che nei `fieldsets` come mostrato di seguito.
Questo renderà il campo visibile nella sezione Admin, consentendoci di assegnare un `User` a un `BookInstance` quando necessario.

```python
@admin.register(BookInstance)
class BookInstanceAdmin(admin.ModelAdmin):
    list_display = ('book', 'status', 'borrower', 'due_back', 'id')
    list_filter = ('status', 'due_back')

    fieldsets = (
        (None, {
            'fields': ('book', 'imprint', 'id')
        }),
        ('Availability', {
            'fields': ('status', 'due_back', 'borrower')
        }),
    )
```

### Prestare alcuni libri

Ora che è possibile prestare libri a un utente specifico, vai e presta un certo numero di record `BookInstance`. Imposta il loro campo `borrowed` sul tuo utente di test, imposta lo `status` su "In prestito" e imposta date di scadenza sia nel futuro sia nel passato.

> [!NOTE]
> Non descriveremo il processo, poiché sai già come utilizzare il sito Admin!

### Vista sui libri in prestito

Ora aggiungeremo una vista per ottenere l'elenco di tutti i libri che sono stati prestati all'utente corrente. Useremo la stessa vista di elenco basata su classi generiche che conosciamo, ma questa volta importeremo e deriveremo da `LoginRequiredMixin`, in modo che solo un utente connesso possa chiamare questa vista. Inoltre, sceglieremo di dichiarare un `template_name`, piuttosto che usare il predefinito, perché potremmo finire con avere diversi elenchi di record BookInstance, con diverse viste e template.

Aggiungi il seguente codice a **catalog/views.py**:

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class LoanedBooksByUserListView(LoginRequiredMixin,generic.ListView):
    """Generic class-based view listing books on loan to current user."""
    model = BookInstance
    template_name = 'catalog/bookinstance_list_borrowed_user.html'
    paginate_by = 10

    def get_queryset(self):
        return (
            BookInstance.objects.filter(borrower=self.request.user)
            .filter(status__exact='o')
            .order_by('due_back')
        )
```

Per limitare la nostra query solo agli oggetti `BookInstance` per l'utente corrente, re-implementiamo `get_queryset()` come mostrato sopra. Nota che "o" è il codice memorizzato per "in prestito" e che ordiniamo per la data `due_back` in modo che gli articoli più vecchi vengano visualizzati per primi.

### Conf URL per libri in prestito

Ora apri **/catalog/urls.py** e aggiungi un `path()` che punti alla vista sopra (puoi semplicemente copiare il testo qui di seguito alla fine del file).

```python
urlpatterns += [
    path('mybooks/', views.LoanedBooksByUserListView.as_view(), name='my-borrowed'),
]
```

### Template per i libri in prestito

Ora, tutto ciò che dobbiamo fare per questa pagina è aggiungere un template. Innanzitutto, crea il file del template **/catalog/templates/catalog/bookinstance_list_borrowed_user.html** e dai questo contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}
    <h1>Borrowed books</h1>

    {% if bookinstance_list %}
    <ul>

      {% for bookinst in bookinstance_list %}
      <li class="{% if bookinst.is_overdue %}text-danger{% endif %}">
        <a href="{% url 'book-detail' bookinst.book.pk %}">\{{ bookinst.book.title }}</a> (\{{ bookinst.due_back }})
      </li>
      {% endfor %}
    </ul>

    {% else %}
      <p>There are no books borrowed.</p>
    {% endif %}
{% endblock %}
```

Questo template è molto simile a quelli che abbiamo creato in precedenza per gli oggetti `Book` e `Author`.
L'unica cosa "nuova" qui è che controlliamo il metodo che abbiamo aggiunto nel modello `(bookinst.is_overdue`) e lo usiamo per cambiare il colore degli articoli in ritardo.

Quando il server di sviluppo è in esecuzione, ora dovresti essere in grado di visualizzare l'elenco per un utente connesso nel tuo browser a `http://127.0.0.1:8000/catalog/mybooks/`. Prova questo con il tuo utente connesso e disconnesso (in quest'ultimo caso, dovresti essere reindirizzato alla pagina di login).

### Aggiungi l'elenco alla barra laterale

L'ultimissimo passaggio è aggiungere un link per questa nuova pagina nella barra laterale. Lo metteremo nella stessa sezione in cui visualizziamo altre informazioni per l'utente connesso.

Apri il template di base (**/django-locallibrary-tutorial/catalog/templates/base_generic.html**) e aggiungi la riga "My Borrowed" nella barra laterale nella posizione mostrata di seguito.

```django
 <ul class="sidebar-nav">
   {% if user.is_authenticated %}
   <li>User: \{{ user.get_username }}</li>

   <li><a href="{% url 'my-borrowed' %}">My Borrowed</a></li>

   <li>
     <form id="logout-form" method="post" action="{% url 'admin:logout' %}">
       {% csrf_token %}
       <button type="submit" class="btn btn-link">Logout</button>
     </form>
   </li>
   {% else %}
   <li><a href="{% url 'login' %}?next=\{{ request.path }}">Login</a></li>
   {% endif %}
 </ul>
```

### Che aspetto ha?

Quando un utente è connesso, vedrà il link _My Borrowed_ nella barra laterale e l'elenco dei libri visualizzato come di seguito (il primo libro non ha data di scadenza, il che è un bug che speriamo di risolvere in un tutorial successivo!).

![Biblioteca - libri in prestito dall'utente](library_borrowed_by_user.png)

## Permessi

I permessi sono associati ai modelli e definiscono le operazioni che possono essere eseguite su un'istanza di modello da un utente che ha il permesso. Per impostazione predefinita, Django dà automaticamente permessi di _aggiunta_, _modifica_ e _eliminazione_ a tutti i modelli, che consentono agli utenti con i permessi di eseguire le azioni associate tramite il sito di amministrazione. Puoi definire i tuoi permessi per i modelli e garantirli a utenti specifici. Puoi anche cambiare i permessi associati a differenti istanze dello stesso modello.

Il test sui permessi nelle viste e nei template è quindi molto simile al test sullo stato di autenticazione (e in effetti, testare un permesso verifica anche l'autenticazione).

### Modelli

La definizione dei permessi viene effettuata nella sezione `class Meta` del modello, utilizzando il campo `permissions`.
Puoi specificare quanti permessi necessari in una tuple, ogni permesso stesso definito in una sottotupla contenente il nome del permesso e il valore di visualizzazione del permesso.
Ad esempio, potremmo definire un permesso per consentire a un utente di segnare che un libro è stato restituito come mostrato:

```python
class BookInstance(models.Model):
    # …
    class Meta:
        # …
        permissions = (("can_mark_returned", "Set book as returned"),)
```

Potremmo quindi assegnare il permesso a un gruppo "Bibliotecario" nel sito di amministrazione.

Apri **catalog/models.py**, e aggiungi il permesso come mostrato sopra. Dovrai eseguire nuovamente le migrazioni (chiamare `python3 manage.py makemigrations` e `python3 manage.py migrate`) per aggiornare il database in modo appropriato.

### Template

I permessi dell'utente corrente sono memorizzati in una variabile del template chiamata `\{{ perms }}`. Puoi controllare se l'utente corrente ha un particolare permesso usando il nome della variabile specifica all'interno dell'app Django associata — ad esempio, `\{{ perms.catalog.can_mark_returned }}` sarà `True` se l'utente ha questo permesso, e `False` altrimenti. Tipicamente testi per il permesso usando i tag di template `{% if %}` come mostrato:

```django
{% if perms.catalog.can_mark_returned %}
    <!-- We can mark a BookInstance as returned. -->
    <!-- Perhaps add code to link to a "book return" view here. -->
{% endif %}
```

### Viste

I permessi possono essere testati in una vista di funzione utilizzando il decoratore `permission_required` o in una vista basata su classe utilizzando il `PermissionRequiredMixin`. I pattern sono gli stessi per l'autenticazione del login, sebbene, ovviamente, potresti dover aggiungere più permessi.

Decoratore di vista di funzione:

```python
from django.contrib.auth.decorators import permission_required

@permission_required('catalog.can_mark_returned')
@permission_required('catalog.can_edit')
def my_view(request):
    # …
```

Un mixin richiesto per i permessi per le viste basate su classi.

```python
from django.contrib.auth.mixins import PermissionRequiredMixin

class MyView(PermissionRequiredMixin, View):
    permission_required = 'catalog.can_mark_returned'
    # Or multiple permissions
    permission_required = ('catalog.can_mark_returned', 'catalog.change_book')
    # Note that 'catalog.change_book' is permission
    # Is created automatically for the book model, along with add_book, and delete_book
```

> [!NOTE]
> Vi è una piccola differenza predefinita nel comportamento sopra. Per **default** per un utente autenticato con una violazione del permesso:
>
> - `@permission_required` reindirizza alla schermata di login (HTTP Status 302).
> - `PermissionRequiredMixin` restituisce 403 (HTTP Status Forbidden).
>
> Normalmente, vorrai il comportamento di `PermissionRequiredMixin`: restituire 403 se un utente è connesso ma non ha il permesso corretto. Per fare questo per una vista di funzione utilizza `@login_required` e `@permission_required` con `raise_exception=True` come mostrato:
>
> ```python
> from django.contrib.auth.decorators import login_required, permission_required
>
> @login_required
> @permission_required('catalog.can_mark_returned', raise_exception=True)
> def my_view(request):
>     # …
> ```

### Esempio

Non aggiorneremo qui il _LocalLibrary_; forse nel prossimo tutorial!

## Metti alla prova te stesso

All'inizio di questo articolo, ti abbiamo mostrato come creare una pagina per l'utente corrente, elencando i libri che ha preso in prestito.
La sfida ora è creare una pagina simile che sia visibile solo per i bibliotecari, che mostri _tutti_ i libri che sono stati prestati e che includa il nome di ciascun prestatore.

Dovresti essere in grado di seguire lo stesso schema per l'altra vista. La principale differenza è che dovrai limitare la vista solo ai bibliotecari. Puoi farlo in base al fatto che l'utente sia un membro del personale (decoratore di funzione: `staff_member_required`, variabile di template: `user.is_staff`), ma ti consigliamo invece di utilizzare il permesso `can_mark_returned` e `PermissionRequiredMixin`, come descritto nella sezione precedente.

> [!WARNING]
> Ricorda di non utilizzare il tuo superutente per il test dei permessi (i controlli dei permessi restituiscono sempre vero per i superutenti, anche se un permesso non è ancora stato definito!). Invece, crea un utente bibliotecario e aggiungi la capacità richiesta.

Quando avrai finito, la tua pagina dovrebbe assomigliare a quella nello screenshot qui sotto.

![Tutti i libri in prestito, limitato ai bibliotecari](library_borrowed_all.png)

## Sintesi

Eccellente lavoro — ora hai creato un sito web in cui i membri della biblioteca possono accedere e visualizzare i loro contenuti e in cui i bibliotecari (con il permesso corretto) possono visualizzare tutti i libri presi in prestito e i loro prestatori. Al momento stiamo ancora solo visualizzando i contenuti, ma gli stessi principi e tecniche sono utilizzati quando vuoi iniziare a modificare e aggiungere dati.

Nel nostro prossimo articolo, vedremo come puoi utilizzare i moduli di Django per raccogliere input dagli utenti e quindi iniziare a modificare alcuni dei nostri dati memorizzati.

## Vedi anche

- [Autenticazione utente in Django](https://docs.djangoproject.com/en/5.0/topics/auth/) (documentazione di Django)
- [Utilizzare il sistema di autenticazione (predefinito) di Django](https://docs.djangoproject.com/en/5.0/topics/auth/default/) (documentazione di Django)
- [Introduzione alle viste basate su classi > Decorare le viste basate su classi](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/#decorating-class-based-views) (documentazione di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django")}}
