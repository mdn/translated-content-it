---
title: "Tutorial Django Parte 9: Lavorare con i moduli"
short-title: "9: Moduli"
slug: Learn_web_development/Extensions/Server-side/Django/Forms
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django")}}

In questo tutorial ti mostreremo come lavorare con i moduli HTML in Django, e in particolare, il modo più semplice per scrivere moduli per creare, aggiornare e cancellare istanze di modelli. Come parte di questa dimostrazione, estenderemo il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) in modo che i bibliotecari possano rinnovare i libri, e creare, aggiornare e cancellare autori usando i nostri moduli (anziché utilizzare l'applicazione di amministrazione).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti i precedenti argomenti del tutorial, inclusi
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication">Il Tutorial Django Parte 8: Autenticazione dell'utente e permessi</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come scrivere moduli per ottenere informazioni dagli utenti e aggiornare il database.
        Comprendere come le viste di modifica generiche basate su classi possono semplificare notevolmente la creazione di moduli per lavorare con un singolo modello.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Un [Modulo HTML](/it/docs/Learn_web_development/Extensions/Forms) è un gruppo di uno o più campi/widget su una pagina web, che possono essere utilizzati per raccogliere informazioni dagli utenti per l'invio a un server. I moduli sono un meccanismo flessibile per raccogliere input dagli utenti perché ci sono widget adatti per inserire molti diversi tipi di dati, inclusi caselle di testo, checkbox, pulsanti radio, selettori di date e così via. I moduli sono anche un modo relativamente sicuro per condividere dati con il server, poiché consentono di inviare dati in richieste `POST` con protezione contro la contraffazione delle richieste tra siti.

Sebbene non abbiamo creato alcun modulo in questo tutorial finora, li abbiamo già incontrati nel sito di amministrazione di Django — per esempio, lo screenshot qui sotto mostra un modulo per modificare uno dei nostri modelli [Book](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models), composto da una serie di liste di selezione e editor di testo.

![Admin Site - Book Add](admin_book_add.png)

Lavorare con i moduli può essere complicato! Gli sviluppatori devono scrivere HTML per il modulo, convalidare e sanificare correttamente i dati inseriti sul server (e possibilmente anche nel browser), ripostare il modulo con messaggi di errore per informare gli utenti di eventuali campi non validi, gestire i dati quando sono stati inviati con successo e infine rispondere all'utente in qualche modo per indicare il successo. _Django Forms_ elimina molto del lavoro da tutti questi passaggi, fornendo un framework che ti permette di definire i moduli e i loro campi programmaticamente, e poi usare questi oggetti sia per generare il codice HTML del modulo che per gestire gran parte della convalida e dell'interazione con l'utente.

In questo tutorial, ti mostreremo alcuni dei modi in cui puoi creare e lavorare con i moduli, e in particolare, come le viste di modifica generiche possono ridurre significativamente il lavoro che devi fare per creare moduli per manipolare i tuoi modelli. Lungo il percorso, estenderemo la nostra applicazione _LocalLibrary_ aggiungendo un modulo per permettere ai bibliotecari di rinnovare i libri della biblioteca, e creeremo pagine per creare, modificare e cancellare libri e autori (riproducendo una versione base del modulo mostrato sopra per modificare i libri).

## Moduli HTML

Prima, una breve panoramica dei [Moduli HTML](/it/docs/Learn_web_development/Extensions/Forms). Consideriamo un semplice modulo HTML, con un solo campo di testo per inserire il nome di una "squadra", e la sua etichetta associata:

![Simple name field example in HTML form](form_example_name_field.png)

Il modulo è definito in HTML come una raccolta di elementi all'interno dei tag `<form>…</form>`, contenente almeno un elemento `input` di `type="submit"`.

```html
<form action="/team_name_url/" method="post">
  <label for="team_name">Enter name: </label>
  <input
    id="team_name"
    type="text"
    name="name_field"
    value="Default name for team." />
  <input type="submit" value="OK" />
</form>
```

Mentre qui abbiamo solo un campo di testo per inserire il nome della squadra, un modulo _può_ avere un qualsiasi numero di altri elementi input e le loro etichette associate. L'attributo `type` del campo definisce quale tipo di widget verrà visualizzato. Il `name` e l'`id` del campo sono usati per identificare il campo in JavaScript/CSS/HTML, mentre `value` definisce il valore iniziale per il campo quando è visualizzato per la prima volta. L'etichetta della squadra corrispondente è specificata usando il tag `label` (vedi "Enter name" sopra), con un campo `for` che contiene il valore `id` del `input` associato.

L'input di tipo `submit` sarà visualizzato come un pulsante di default.
Questo può essere premuto per caricare i dati di tutti gli altri elementi input del modulo al server (in questo caso, solo il campo `team_name`).
Gli attributi del modulo definiscono il `method` HTTP usato per inviare i dati e la destinazione dei dati sul server (`action`):

- `action`: La risorsa/URL al quale i dati devono essere inviati per l'elaborazione quando il modulo viene inviato. Se non impostato (o impostato su una stringa vuota), il modulo sarà inviato di nuovo all'URL della pagina corrente.
- `method`: Il metodo HTTP utilizzato per inviare i dati: _post_ o _get_.

  - Il metodo `POST` dovrebbe sempre essere utilizzato se i dati causeranno un cambiamento nel database del server, perché può essere reso più resistente agli attacchi di richiesta di contraffazione tra siti.
  - Il metodo `GET` dovrebbe essere utilizzato solo per i moduli che non cambiano i dati dell'utente (per esempio, un modulo di ricerca). È consigliato quando vuoi poter aggiungere ai segnalibri o condividere l'URL.

Il ruolo del server è innanzitutto di rendere lo stato iniziale del modulo — contenente campi vuoti o pre-popolati con valori iniziali. Dopo che l'utente ha premuto il pulsante di invio, il server riceverà i dati del modulo con i valori dal browser web e dovrà validare l'informazione. Se il modulo contiene dati non validi, il server dovrebbe visualizzare di nuovo il modulo, questa volta con i dati inseriti dall'utente nei campi "validi" e messaggi per descrivere il problema per i campi non validi. Una volta che il server riceve una richiesta con tutti i dati del modulo validi, può eseguire un'azione appropriata (come: salvare i dati, restituire il risultato di una ricerca, caricare un file, ecc.) e poi notificare l'utente.

Come puoi immaginare, creare l'HTML, validare i dati restituiti, ridistribuire i dati inseriti con i report di errore se necessario, e eseguire l'operazione desiderata sui dati validi possono richiedere abbastanza sforzo per essere "fatti bene". Django rende questo molto più facile togliendo parte del lavoro pesante e del codice ripetitivo!

## Processo di gestione dei moduli in Django

La gestione dei moduli in Django usa tutte le stesse tecniche che abbiamo appreso nei tutorial precedenti (per visualizzare informazioni sui nostri modelli): la vista riceve una richiesta, esegue le azioni necessarie, incluso leggere dati dai modelli, quindi genera e restituisce una pagina HTML (da un template, nel quale passiamo un _context_ contenente i dati da visualizzare). Ciò che complica ulteriormente è che il server deve anche essere in grado di elaborare i dati forniti dall'utente e ridisporre la pagina se ci sono errori.

Un diagramma di flusso di processo su come Django gestisce le richieste di moduli è mostrato di seguito, iniziando con una richiesta per una pagina contenente un modulo (mostrata in verde).

![Updated form handling process doc.](form_handling_-_standard.png)

Basato sul diagramma sopra, le principali cose che la gestione dei moduli in Django fa sono:

1. Visualizza il modulo di default la prima volta che viene richiesta dall'utente.

   - Il modulo può contenere campi vuoti se stai creando un nuovo record, o può essere pre-popolato con valori iniziali (per esempio, se stai modificando un record, o hai valori iniziali predefiniti utili).
   - Il modulo è indicato come _unbound_ a questo punto, perché non è associato a nessun dato inserito dall'utente (sebbene possa avere valori iniziali).

2. Ricevi dati da una richiesta di invio e legali al modulo.

   - Legare i dati al modulo significa che i dati inseriti dall'utente e eventuali errori sono disponibili quando dobbiamo ridisporre il modulo.

3. Pulisci e valida i dati.

   - Pulire i dati esegue la sanificazione dei campi di input, come la rimozione di caratteri non validi che potrebbero essere usati per inviare contenuti dannosi al server, e li converte nei tipi Python consistenti.
   - La validazione controlla che i valori siano appropriati per il campo (per esempio, che siano nel giusto intervallo di date, non siano troppo corti o troppo lunghi, ecc.)

4. Se qualche dato è non valido, ridisponi il modulo, questa volta con eventuali valori popolati dall'utente e messaggi di errore per i campi con problemi.
5. Se tutti i dati sono validi, esegui le azioni richieste (come salvare i dati, inviare un'e-mail, restituire il risultato di una ricerca, caricare un file, e così via).
6. Una volta che tutte le azioni sono complete, reindirizza l'utente a un'altra pagina.

Django fornisce una serie di strumenti e approcci per aiutarti con i compiti sopra descritti. La più fondamentale è la classe `Form`, che semplifica sia la generazione dell'HTML del modulo sia la pulizia/validazione dei dati. Nella sezione successiva, descriviamo come funzionano i moduli usando l'esempio pratico di una pagina per permettere ai bibliotecari di rinnovare i libri.

> [!NOTE]
> Comprendere come viene utilizzata `Form` ti aiuterà quando discuteremo le classi del framework dei moduli di Django più "di alto livello".

## Modulo per il rinnovo dei libri utilizzando un modulo e una vista funzionale

Successivamente, aggiungeremo una pagina per permettere ai bibliotecari di rinnovare i libri presi in prestito. Per fare questo creeremo un modulo che permette agli utenti di inserire un valore di data. Inseriremo nel campo un valore iniziale di 3 settimane dalla data corrente (il periodo normale di prestito), e aggiungeremo alcune validazioni per garantire che il bibliotecario non possa inserire una data nel passato o una data troppo lontana nel futuro. Quando una data valida è stata inserita, la scriveremo nel campo `BookInstance.due_back` del record corrente.

L'esempio userà una vista basata su funzione e una classe `Form`. Le sezioni seguenti spiegano come funzionano i moduli, e le modifiche che devi fare al nostro progetto _LocalLibrary_ in corso.

### Form

La classe `Form` è il cuore del sistema di gestione dei moduli di Django. Specifica i campi nel modulo, il loro layout, i widget di visualizzazione, le etichette, i valori iniziali, i valori validi, e (una volta convalidati) i messaggi di errore associati ai campi non validi. La classe fornisce anche metodi per rendere sé stessa nei template usando formati predefiniti (tabelle, liste, ecc.) o per ottenere il valore di qualsiasi elemento (abilitando un rendering manuale a grana fine).

#### Dichiarare un modulo

La sintassi di dichiarazione di un `Form` è molto simile a quella per dichiarare un `Model`, e condivide gli stessi tipi di campo (e alcuni parametri simili). Questo ha senso perché in entrambi i casi dobbiamo garantire che ogni campo gestisca i giusti tipi di dati, sia limitato ai dati validi e abbia una descrizione per la visualizzazione/documentazione.

I dati del modulo sono memorizzati nel file forms.py di un'applicazione, all'interno della directory dell'applicazione. Crea e apri il file **django-locallibrary-tutorial/catalog/forms.py**. Per creare un `Form`, importiamo la libreria `forms`, deriviamo dalla classe `Form`, e dichiariamo i campi del modulo. Una classe di modulo di base molto semplice per il nostro modulo di rinnovo libri della biblioteca è mostrata di seguito — aggiungilo al tuo nuovo file:

```python
from django import forms

class RenewBookForm(forms.Form):
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")
```

#### Campi del modulo

In questo caso, abbiamo un singolo [`DateField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#datefield) per inserire la data di rinnovo che renderà in HTML con un valore vuoto, l'etichetta di default "_Renewal date:_", e un testo di utilizzo utile: "_Enter a date between now and 4 weeks (default 3 weeks)._". Poiché nessuno degli altri argomenti opzionali è specificato, il campo accetterà date usando i [input_formats](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#django.forms.DateField.input_formats): YYYY-MM-DD (2024-11-06), MM/DD/YYYY (02/26/2024), MM/DD/YY (10/25/24), e sarà reso utilizzando il [widget](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#widget) di default: [DateInput](https://docs.djangoproject.com/en/5.0/ref/forms/widgets/#django.forms.DateInput).

Ci sono molti altri tipi di campi modulo, che riconoscerai in gran parte per la loro somiglianza alle classi di campo modello equivalenti:

- [`BooleanField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#booleanfield)
- [`CharField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#charfield)
- [`ChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#choicefield)
- [`TypedChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#typedchoicefield)
- [`DateField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#datefield)
- [`DateTimeField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#datetimefield)
- [`DecimalField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#decimalfield)
- [`DurationField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#durationfield)
- [`EmailField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#emailfield)
- [`FileField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#filefield)
- [`FilePathField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#filepathfield)
- [`FloatField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#floatfield)
- [`ImageField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#imagefield)
- [`IntegerField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#integerfield)
- [`GenericIPAddressField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#genericipaddressfield)
- [`MultipleChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#multiplechoicefield)
- [`TypedMultipleChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#typedmultiplechoicefield)
- [`NullBooleanField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#nullbooleanfield)
- [`RegexField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#regexfield)
- [`SlugField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#slugfield)
- [`TimeField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#timefield)
- [`URLField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#urlfield)
- [`UUIDField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#uuidfield)
- [`ComboField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#combofield)
- [`MultiValueField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#multivaluefield)
- [`SplitDateTimeField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#splitdatetimefield)
- [`ModelMultipleChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#modelmultiplechoicefield)
- [`ModelChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#modelchoicefield)

Gli argomenti comuni alla maggior parte dei campi sono elencati di seguito (hanno valori predefiniti sensati):

- [`required`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#required): Se `True`, il campo non può essere lasciato vuoto o dato un valore `None`. I campi sono obbligatori per default, quindi imposteresti `required=False` per consentire valori vuoti nel modulo.
- [`label`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label): L'etichetta da utilizzare quando si renderizza il campo in HTML. Se una [label](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label) non è specificata, Django ne creerà una dal nome del campo, capitalizzando la prima lettera e sostituendo gli underscore con spazi (es., _Renewal date_).
- [`label_suffix`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label-suffix): Di default, viene visualizzato un due punti dopo l'etichetta (es., Renewal date&ZeroWidthSpace;**:**). Questo argomento ti permette di specificare un suffisso diverso contenente altri caratteri.
- [`initial`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#initial): Il valore iniziale per il campo quando il modulo è visualizzato.
- [`widget`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#widget): Il widget di visualizzazione da usare.
- [`help_text`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#help-text) (come visto nell'esempio sopra): Testo aggiuntivo che può essere visualizzato nei moduli per spiegare come usare il campo.
- [`error_messages`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#error-messages): Un elenco di messaggi di errore per il campo. Puoi sovrascriverli con i tuoi messaggi se necessario.
- [`validators`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#validators): Un elenco di funzioni che verranno chiamate sul campo quando è convalidato.
- [`localize`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#localize): Abilita la localizzazione degli input dei dati del modulo (vedi link per ulteriori informazioni).
- [`disabled`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#disabled): Il campo è visualizzato ma il suo valore non può essere modificato se questo è `True`. Il default è `False`.

#### Validazione

Django fornisce numerosi luoghi in cui puoi convalidare i tuoi dati. Il modo più semplice per convalidare un singolo campo è sovrascrivere il metodo `clean_<field_name>()` per il campo che vuoi controllare. Quindi, ad esempio, possiamo validare che i valori `renewal_date` inseriti siano tra oggi e 4 settimane implementando `clean_renewal_date()` come mostrato di seguito.

Aggiorna il tuo file forms.py in modo che assomigli a questo:

```python
import datetime

from django import forms

from django.core.exceptions import ValidationError
from django.utils.translation import gettext_lazy as _

class RenewBookForm(forms.Form):
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")

    def clean_renewal_date(self):
        data = self.cleaned_data['renewal_date']

        # Check if a date is not in the past.
        if data < datetime.date.today():
            raise ValidationError(_('Invalid date - renewal in past'))

        # Check if a date is in the allowed range (+4 weeks from today).
        if data > datetime.date.today() + datetime.timedelta(weeks=4):
            raise ValidationError(_('Invalid date - renewal more than 4 weeks ahead'))

        # Remember to always return the cleaned data.
        return data
```

Ci sono due cose importanti da notare. La prima è che otteniamo i nostri dati usando `self.cleaned_data['renewal_date']` e che restituiamo questi dati indipendentemente dal fatto che li cambiamo o meno alla fine della funzione.
Questo passaggio ci consente di ottenere i dati "puliti" e sanificati da input potenzialmente dannosi usando i validator di default, e convertiti nel tipo standard corretto per i dati (in questo caso un oggetto Python `datetime.datetime`).

Il secondo punto è che se un valore cade fuori dal nostro intervallo solleviamo un `ValidationError`, specificando il testo di errore che vogliamo visualizzare nel modulo se viene inserito un valore non valido.
L'esempio sopra avvolge anche questo testo in una delle funzioni di traduzione di Django [gettext_lazy()](https://docs.djangoproject.com/en/5.0/topics/i18n/translation/), (importata come `_()`), che è una buona pratica se vuoi tradurre il tuo sito in seguito.

> [!NOTE]
> Ci sono numerosi altri metodi ed esempi per convalidare i moduli in [Form and field validation](https://docs.djangoproject.com/en/5.0/ref/forms/validation/) (documenti Django). Ad esempio, nei casi in cui hai campi multipli che dipendono l'uno dall'altro, puoi sovrascrivere la funzione [Form.clean()](https://docs.djangoproject.com/en/5.0/ref/forms/api/#django.forms.Form.clean) e ancora sollevare un `ValidationError`.

Questo è tutto ciò di cui abbiamo bisogno per il modulo in questo esempio!

### Configurazione URL

Prima di creare la nostra vista, aggiungiamo una configurazione URL per la pagina _renew-books_. Copia la seguente configurazione in fondo a **django-locallibrary-tutorial/catalog/urls.py**:

```python
urlpatterns += [
    path('book/<uuid:pk>/renew/', views.renew_book_librarian, name='renew-book-librarian'),
]
```

La configurazione dell'URL reindirizzerà gli URL nel formato **/catalog/book/_\<bookinstance_id>_/renew/** alla funzione chiamata `renew_book_librarian()` in **views.py**, e invierà l'id di `BookInstance` come parametro chiamato `pk`. Il pattern corrisponde solo se `pk` è un `uuid` formattato correttamente.

> [!NOTE]
> Possiamo nominare i nostri dati URL catturati come vogliamo, perché abbiamo il completo controllo sulla funzione vista (non stiamo usando una classe vista di dettaglio generica che si aspetta parametri con un certo nome). Tuttavia, `pk`, abbreviazione di "primary key", è una convenzione ragionevole da usare!

### Vista

Come discusso nel [Processo di gestione dei moduli in Django](#processo_di_gestione_dei_moduli_in_django) sopra, la vista deve rendere il modulo di default quando viene chiamata per la prima volta e poi o ridisporlo con messaggi di errore se i dati sono non validi, o elaborare i dati e reindirizzarli a una nuova pagina se i dati sono validi. Per eseguire queste diverse azioni, la vista deve essere in grado di sapere se viene chiamata per la prima volta per visualizzare il modulo di default o per una successiva volta per validare i risultati.

Per i moduli che usano una richiesta `POST` per inviare informazioni al server, il pattern più comune è che la vista cerchi di questa richiesta `POST` (usando `if request.method == 'POST':`) per identificare le richieste di validazione del modulo e `GET` (usando una condizione `else`) per identificare la richiesta iniziale di creazione del modulo. Se desideri inviare i dati usando una richiesta `GET`, un approccio tipico per identificare se si tratta della prima o di una successiva invocazione della vista è leggere i dati del modulo (ad esempio, leggere un valore nascosto nel modulo).

Il processo di rinnovo del libro scriverà nel nostro database, quindi, per convenzione, usiamo l'approccio della richiesta `POST`.
Il frammento di codice qui sotto mostra il pattern (molto standard) per questo tipo di vista basata su funzione.

```python
import datetime

from django.shortcuts import render, get_object_or_404
from django.http import HttpResponseRedirect
from django.urls import reverse

from catalog.forms import RenewBookForm

def renew_book_librarian(request, pk):
    book_instance = get_object_or_404(BookInstance, pk=pk)

    # If this is a POST request then process the Form data
    if request.method == 'POST':

        # Create a form instance and populate it with data from the request (binding):
        form = RenewBookForm(request.POST)

        # Check if the form is valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
            book_instance.due_back = form.cleaned_data['renewal_date']
            book_instance.save()

            # redirect to a new URL:
            return HttpResponseRedirect(reverse('all-borrowed'))

    # If this is a GET (or any other method) create the default form.
    else:
        proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
        form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

    context = {
        'form': form,
        'book_instance': book_instance,
    }

    return render(request, 'catalog/book_renew_librarian.html', context)
```

Prima, importiamo il nostro modulo (`RenewBookForm`) e un numero di altri oggetti/metodi utili utilizzati nel corpo della funzione vista:

- [`get_object_or_404()`](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#get-object-or-404): Restituisce un oggetto specificato da un modello in base al suo valore di chiave primaria e solleva un'eccezione `Http404` (non trovato) se il record non esiste.
- [`HttpResponseRedirect`](https://docs.djangoproject.com/en/5.0/ref/request-response/#django.http.HttpResponseRedirect): Questo crea un reindirizzamento a un URL specificato (codice di stato HTTP 302).
- [`reverse()`](https://docs.djangoproject.com/en/5.0/ref/urlresolvers/#django.urls.reverse): Questo genera un URL da un nome di configurazione URL e un set di argomenti. È l'equivalente Python del tag `url` che abbiamo usato nei nostri template.
- [`datetime`](https://docs.python.org/3/library/datetime.html): Una libreria Python per manipolare le date e gli orari.

Nella vista, usiamo prima l'argomento `pk` in `get_object_or_404()` per ottenere l'attuale `BookInstance` (se questo non esiste, la vista uscirà immediatamente e la pagina mostrerà un errore "non trovato").
Se questo _non_ è una richiesta `POST` (gestita dalla clausola `else`) allora creiamo il modulo di default passando un valore `initial` per il campo `renewal_date`, 3 settimane dalla data corrente.

```python
book_instance = get_object_or_404(BookInstance, pk=pk)

# If this is a GET (or any other method) create the default form
else:
    proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
    form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

context = {
    'form': form,
    'book_instance': book_instance,
}

return render(request, 'catalog/book_renew_librarian.html', context)
```

Dopo aver creato il modulo, chiamiamo `render()` per creare la pagina HTML, specificando il template e un contesto che contiene il nostro modulo. In questo caso, il contesto contiene anche il nostro `BookInstance`, che useremo nel template per fornire informazioni sul libro che stiamo rinnovando.

Tuttavia, se si tratta di una richiesta `POST`, allora creiamo il nostro oggetto `form` e lo popoli con i dati dalla richiesta. Questo processo è chiamato "binding" e ci permette di validare il modulo.

Controlliamo quindi se il modulo è valido, il che esegue tutto il codice di validazione su tutti i campi, incluso sia il codice generico per controllare che il nostro campo data sia effettivamente una data valida sia la funzione `clean_renewal_date()` specifica del nostro modulo per controllare che la data sia nel giusto intervallo.

```python
book_instance = get_object_or_404(BookInstance, pk=pk)

# If this is a POST request then process the Form data
if request.method == 'POST':

    # Create a form instance and populate it with data from the request (binding):
    form = RenewBookForm(request.POST)

    # Check if the form is valid:
    if form.is_valid():
        # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
        book_instance.due_back = form.cleaned_data['renewal_date']
        book_instance.save()

        # redirect to a new URL:
        return HttpResponseRedirect(reverse('all-borrowed'))

context = {
    'form': form,
    'book_instance': book_instance,
}

return render(request, 'catalog/book_renew_librarian.html', context)
```

Se il modulo non è valido, chiamiamo `render()` di nuovo, ma questa volta il valore del modulo passa nel contesto includerà i messaggi di errore.

Se il modulo è valido, allora possiamo iniziare a usare i dati, accedendovi attraverso l'attributo `form.cleaned_data` (ad esempio, `data = form.cleaned_data['renewal_date']`). Qui, salviamo solo i dati nel valore `due_back` dell'oggetto `BookInstance` associato.

> [!WARNING]
> Mentre puoi anche accedere ai dati del modulo direttamente tramite la richiesta (ad esempio, `request.POST['renewal_date']` o `request.GET['renewal_date']` se si utilizza una richiesta GET), questo NON è raccomandato. I dati puliti sono sanificati, validati e convertiti in tipi compatibili con Python.

L'ultimo passaggio nella parte di gestione del modulo della vista è reindirizzare a un'altra pagina, solitamente una pagina di "successo". In questo caso, usiamo `HttpResponseRedirect` e `reverse()` per reindirizzare alla vista chiamata `'all-borrowed'` (questa è stata creata come "sfida" in [Il Tutorial Django Parte 8: Autenticazione dell'utente e permessi](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication#challenge_yourself)). Se non hai creato quella pagina considera di reindirizzare alla home page all'URL `/`).

Questo è tutto ciò che serve per la gestione del modulo stesso, ma dobbiamo ancora limitare l'accesso alla vista solo ai bibliotecari registrati che hanno il permesso di rinnovare i libri. Utilizziamo `@login_required` per richiedere che l'utente sia connesso, e il decoratore `@permission_required` con il nostro permesso esistente `can_mark_returned` per consentire l'accesso (i decoratori sono elaborati nell'ordine). Nota che probabilmente avremmo dovuto creare una nuova impostazione di permesso in `BookInstance` (`can_renew`), ma riutilizzeremo quella esistente per mantenere l'esempio semplice.

La vista finale è quindi come mostrato di seguito. Si prega di copiarlo in fondo a **django-locallibrary-tutorial/catalog/views.py**.

```python
import datetime

from django.contrib.auth.decorators import login_required, permission_required
from django.shortcuts import get_object_or_404
from django.http import HttpResponseRedirect
from django.urls import reverse

from catalog.forms import RenewBookForm

@login_required
@permission_required('catalog.can_mark_returned', raise_exception=True)
def renew_book_librarian(request, pk):
    """View function for renewing a specific BookInstance by librarian."""
    book_instance = get_object_or_404(BookInstance, pk=pk)

    # If this is a POST request then process the Form data
    if request.method == 'POST':

        # Create a form instance and populate it with data from the request (binding):
        form = RenewBookForm(request.POST)

        # Check if the form is valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
            book_instance.due_back = form.cleaned_data['renewal_date']
            book_instance.save()

            # redirect to a new URL:
            return HttpResponseRedirect(reverse('all-borrowed'))

    # If this is a GET (or any other method) create the default form.
    else:
        proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
        form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

    context = {
        'form': form,
        'book_instance': book_instance,
    }

    return render(request, 'catalog/book_renew_librarian.html', context)
```

### Il template

Crea il template a cui fa riferimento la vista (**/catalog/templates/catalog/book_renew_librarian.html**) e copia il codice qui sotto:

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>Renew: \{{ book_instance.book.title }}</h1>
  <p>Borrower: \{{ book_instance.borrower }}</p>
  <p {% if book_instance.is_overdue %} class="text-danger"{% endif %} >Due date: \{{ book_instance.due_back }}</p>

  <form action="" method="post">
    {% csrf_token %}
    <table>
    \{{ form.as_table }}
    </table>
    <input type="submit" value="Submit">
  </form>
{% endblock %}
```

La maggior parte di questo sarà completamente familiare dai tutorial precedenti.

Estendiamo il template base e quindi ridefiniamo il blocco di contenuti. Siamo in grado di fare riferimento a `\{{ book_instance }}` (e le sue variabili) perché è stato passato nell'oggetto contesto nella funzione `render()`, e li usiamo per elencare il titolo del libro, il prestatario e la data di scadenza originale.

Il codice del modulo è relativamente semplice. Per prima cosa, dichiariamo i tag `form`, specificando dove il modulo deve essere inviato (`action`) e il `method` per l'invio dei dati (in questo caso un `POST`) — se ricordi la panoramica sui [Moduli HTML](#moduli_html) in cima alla pagina, un'azione vuota come mostrato, significa che i dati del modulo verranno inviati di nuovo all'URL corrente della pagina (che è ciò che vogliamo). All'interno dei tag, definisci l'input `submit`, che un utente può premere per inviare i dati. Il `{% csrf_token %}` aggiunto proprio all'interno dei tag del modulo è parte della protezione contro la contraffazione dei siti incrociati di Django.

> [!NOTE]
> Aggiungi il `{% csrf_token %}` a ogni template Django che crei e che usa `POST` per inviare dati. Questo ridurrà le possibilità che i moduli siano dirottati da utenti malintenzionati.

Tutto ciò che rimane è la variabile del template `\{{ form }}`, che abbiamo passato al template nel dizionario del contesto.
Forse non sorprende, quando usato come mostrato questo fornisce il rendering predefinito di tutti i campi del modulo, comprese le loro etichette, widget e testo di aiuto — il rendering è come mostrato di seguito:

```html
<tr>
  <th><label for="id_renewal_date">Renewal date:</label></th>
  <td>
    <input
      id="id_renewal_date"
      name="renewal_date"
      type="text"
      value="2023-11-08"
      required />
    <br />
    <span class="helptext">
      Enter date between now and 4 weeks (default 3 weeks).
    </span>
  </td>
</tr>
```

> [!NOTE]
> Forse non è ovvio perché abbiamo solo un campo, ma, di default, ogni campo è definito nella propria riga di tabella. Questo stesso rendering viene fornito se fai riferimento alla variabile del template `\{{ form.as_table }}`.

Se dovessi inserire una data non valida, avresti anche un elenco degli errori visualizzati sulla pagina (vedi `error-list` di seguito).

```html
<tr>
  <th><label for="id_renewal_date">Renewal date:</label></th>
  <td>
    <ul class="error-list">
      <li>Invalid date - renewal in past</li>
    </ul>
    <input
      id="id_renewal_date"
      name="renewal_date"
      type="text"
      value="2023-11-08"
      required />
    <br />
    <span class="helptext">
      Enter date between now and 4 weeks (default 3 weeks).
    </span>
  </td>
</tr>
```

#### Altri modi di usare la variabile del modulo template

Usando `\{{ form.as_table }}` come mostrato sopra, ogni campo è reso come una riga della tabella. Puoi anche rendere ogni campo come un elemento di lista (usando `\{{ form.as_ul }}`) o come un paragrafo (usando `\{{ form.as_p }}`).

È anche possibile avere il pieno controllo sul rendering di ogni parte del modulo, indicando le sue proprietà usando la notazione a punti. Quindi, per esempio, possiamo accedere a un numero di elementi separati per il nostro campo `renewal_date`:

- `\{{ form.renewal_date }}:` Il campo intero.
- `\{{ form.renewal_date.errors }}`: L'elenco degli errori.
- `\{{ form.renewal_date.id_for_label }}`: L'id dell'etichetta.
- `\{{ form.renewal_date.help_text }}`: Il testo di aiuto del campo.

Per ulteriori esempi su come rendere manualmente i moduli nei template e fare cicli dinamici sui campi del template, vedi [Lavorare con i moduli > Rendering manuale dei campi](https://docs.djangoproject.com/en/5.0/topics/forms/#rendering-fields-manually) (documenti Django).

### Testare la pagina

Se hai accettato la "sfida" in [Il Tutorial Django Parte 8: Autenticazione dell'utente e permessi](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication#challenge_yourself) avrai una vista che mostra tutti i libri presi in prestito nella biblioteca, che è visibile solo al personale della biblioteca.
La vista potrebbe assomigliare a questa:

```django
{% extends "base_generic.html" %}

{% block content %}
    <h1>All Borrowed Books</h1>

    {% if bookinstance_list %}
    <ul>

      {% for bookinst in bookinstance_list %}
      <li class="{% if bookinst.is_overdue %}text-danger{% endif %}">
        <a href="{% url 'book-detail' bookinst.book.pk %}">\{{ bookinst.book.title }}</a> (\{{ bookinst.due_back }}) {% if user.is_staff %}- \{{ bookinst.borrower }}{% endif %}
      </li>
      {% endfor %}
    </ul>

    {% else %}
      <p>There are no books borrowed.</p>
    {% endif %}
{% endblock %}
```

Possiamo aggiungere un link alla pagina di rinnovo libro accanto a ciascun elemento aggiungendo il seguente codice del template al testo dell'elemento della lista sopra.
Nota che questo codice del template può funzionare solo all'interno del loop `{% for %}`, perché è lì che viene definito il valore `bookinst`.

```django
{% if perms.catalog.can_mark_returned %}- <a href="{% url 'renew-book-librarian' bookinst.id %}">Renew</a>{% endif %}
```

> [!NOTE]
> Ricorda che il tuo login di test dovrà avere il permesso `catalog.can_mark_returned` per vedere il nuovo link "Renew" aggiunto sopra, e per accedere alla pagina collegata (forse usa il tuo account superuser).

Puoi alternativamente costruire manualmente un URL di test come questo — `http://127.0.0.1:8000/catalog/book/<bookinstance_id>/renew/` (un valido `bookinstance_id` può essere ottenuto navigando a una pagina di dettaglio del libro nella tua biblioteca, e copiando il campo `id`).

### Come appare?

Se hai successo, il modulo predefinito apparirà così:

![Default form which displays the book details, due date, renewal date and a submit button appears in case the link works successfully](forms_example_renew_default.png)

Il modulo con un valore non valido inserito apparirà così:

![Same form as above with an error message: invalid date - renewal in the past](forms_example_renew_invalid.png)

L'elenco di tutti i libri con link di rinnovo apparirà così:

![Displays list of all renewed books along with their details. Past due is in red.](forms_example_renew_allbooks.png)

## ModelForms

Creare una classe `Form` usando l'approccio sopra descritto è molto flessibile, permettendoti di creare qualsiasi tipo di pagina di modulo tu voglia e associarla a qualsiasi modello o modelli.

Tuttavia, se ti serve solo un modulo per mappare i campi di un _singolo_ modello, il tuo modello definirà già la maggior parte delle informazioni di cui hai bisogno nel modulo: campi, etichette, testo di aiuto e così via. Piuttosto che ricreare le definizioni del modello nel tuo modulo, è più facile usare la classe di aiuto [ModelForm](https://docs.djangoproject.com/en/5.0/topics/forms/modelforms/) per creare il modulo dal tuo modello. Questo `ModelForm` può quindi essere usato all'interno delle tue viste esattamente allo stesso modo di un normale `Form`.

Un `ModelForm` di base contenente lo stesso campo del nostro `RenewBookForm` originale è mostrato di seguito. Tutto ciò che devi fare per creare il modulo è aggiungere `class Meta` con il `model` associato (`BookInstance`) e un elenco dei campi del modello da includere nel modulo.

```python
from django.forms import ModelForm

from catalog.models import BookInstance

class RenewBookModelForm(ModelForm):
    class Meta:
        model = BookInstance
        fields = ['due_back']
```

> [!NOTE]
> Puoi anche includere tutti i campi nel modulo usando `fields = '__all__'`, oppure puoi usare `exclude` (invece di `fields`) per specificare i campi _non_ da includere dal modello).
>
> Nessuno dei due approcci è raccomandato poiché i nuovi campi aggiunti al modello sono poi automaticamente inclusi nel modulo (senza che lo sviluppatore consideri necessariamente le possibili implicazioni di sicurezza).

> [!NOTE]
> Questo potrebbe non sembrare molto più semplice che usare un `Form` (e non lo è in questo caso, perché abbiamo solo un campo). Tuttavia, se hai molti campi, può ridurre considerevolmente la quantità di codice richiesta!

Il resto delle informazioni proviene dalle definizioni dei campi del modello (ad es., etichette, widget, testo di aiuto, messaggi di errore). Se questi non sono del tutto corretti, possiamo sovrascriverli nel nostro `class Meta`, specificando un dizionario contenente il campo da cambiare e il suo nuovo valore. Ad esempio, in questo modulo, potremmo volere un'etichetta per il nostro campo di "_Renewal date_" (anziché il default basato sul nome del campo: _Due Back_), e vogliamo anche che il nostro testo di aiuto sia specifico per questo caso d'uso.
Il `Meta` qui sotto mostra come sovrascrivere questi campi, e puoi allo stesso modo impostare `widgets` e `error_messages` se i default non sono sufficienti.

```python
class Meta:
    model = BookInstance
    fields = ['due_back']
    labels = {'due_back': _('New renewal date')}
    help_texts = {'due_back': _('Enter a date between now and 4 weeks (default 3).')}
```

Per aggiungere la validazione puoi usare lo stesso approccio usato per un normale `Form` — definisci una funzione chiamata `clean_<field_name>()` e solleva eccezioni di `ValidationError` per i valori non validi.
L'unica differenza rispetto al nostro modulo originale è che il campo del modello è chiamato `due_back` e non `renewal_date`.
Questo cambiamento è necessario poiché il campo corrispondente in `BookInstance` si chiama `due_back`.

```python
from django.forms import ModelForm

from catalog.models import BookInstance

class RenewBookModelForm(ModelForm):
    def clean_due_back(self):
       data = self.cleaned_data['due_back']

       # Check if a date is not in the past.
       if data < datetime.date.today():
           raise ValidationError(_('Invalid date - renewal in past'))

       # Check if a date is in the allowed range (+4 weeks from today).
       if data > datetime.date.today() + datetime.timedelta(weeks=4):
           raise ValidationError(_('Invalid date - renewal more than 4 weeks ahead'))

       # Remember to always return the cleaned data.
       return data

    class Meta:
        model = BookInstance
        fields = ['due_back']
        labels = {'due_back': _('Renewal date')}
        help_texts = {'due_back': _('Enter a date between now and 4 weeks (default 3).')}
```

La classe `RenewBookModelForm` sopra è ora funzionalmente equivalente al nostro `RenewBookForm` originale. Puoi importare e usarla ovunque stai attualmente usando `RenewBookForm` finché aggiorni anche il nome della variabile del modulo corrispondente da `renewal_date` a `due_back` come nella seconda dichiarazione del modulo: `RenewBookModelForm(initial={'due_back': proposed_renewal_date}`.

## Viste di modifica generiche

L'algoritmo di gestione dei moduli che abbiamo usato nel nostro esempio di vista basata su funzione sopra rappresenta un pattern estremamente comune nelle viste di modifica dei moduli. Django astratta molto di questo "boilerplate" per te, creando [viste di modifica generiche](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/) per creare, modificare e cancellare viste basate sui modelli. Non solo queste gestiscono il comportamento della "vista", ma creano automaticamente la classe del modulo (un `ModelForm`) per te dal modello.

> [!NOTE]
> Oltre alle viste di modifica descritte qui, c'è anche una classe [FormView](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/#formview), che si trova da qualche parte tra la nostra vista basata su funzione e le altre viste generiche in termini di "flessibilità" vs. "sforzo di codifica". Usando `FormView`, devi ancora creare il tuo `Form`, ma non devi implementare tutti i pattern standard di gestione dei moduli. Invece, devi solo fornire un'implementazione della funzione che sarà chiamata una volta che la sottomissione è nota per essere valida.

In questa sezione, useremo le viste di modifica generiche per creare pagine per aggiungere funzionalità per creare, modificare e cancellare record di `Author` dalla nostra biblioteca — fornendo effettivamente una reimplementazione di base di parti del sito di amministrazione (questo potrebbe essere utile se hai bisogno di offrire funzionalità di amministrazione in un modo più flessibile di quanto possa essere fornito dal sito di amministrazione).

### Viste

Apri il file delle viste (**django-locallibrary-tutorial/catalog/views.py**) e aggiungi il seguente blocco di codice in fondo ad esso:

```python
from django.views.generic.edit import CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from .models import Author

class AuthorCreate(PermissionRequiredMixin, CreateView):
    model = Author
    fields = ['first_name', 'last_name', 'date_of_birth', 'date_of_death']
    initial = {'date_of_death': '11/11/2023'}
    permission_required = 'catalog.add_author'

class AuthorUpdate(PermissionRequiredMixin, UpdateView):
    model = Author
    # Not recommended (potential security issue if more fields added)
    fields = '__all__'
    permission_required = 'catalog.change_author'

class AuthorDelete(PermissionRequiredMixin, DeleteView):
    model = Author
    success_url = reverse_lazy('authors')
    permission_required = 'catalog.delete_author'

    def form_valid(self, form):
        try:
            self.object.delete()
            return HttpResponseRedirect(self.success_url)
        except Exception as e:
            return HttpResponseRedirect(
                reverse("author-delete", kwargs={"pk": self.object.pk})
            )
```

Come puoi vedere, per creare, aggiornare o eliminare le viste devi derivare da `CreateView`, `UpdateView` e `DeleteView` (rispettivamente) e poi definire il modello associato.
Limitiamo anche la chiamata a queste viste solo agli utenti che sono loggati e che hanno i permessi `add_author`, `change_author` e `delete_author`, rispettivamente.

Per i casi di "creazione" e "aggiornamento" devi anche specificare i campi da visualizzare nel modulo (usando la stessa sintassi di `ModelForm`). In questo caso, mostriamo come elencarli singolarmente e la sintassi per elencare "tutti" i campi. Puoi anche specificare i valori iniziali per ciascuno dei campi usando un dizionario di coppie _field_name_/_value_ (qui impostiamo arbitrariamente la data di morte per dimostrazione — potresti voler rimuoverlo). Per impostazione predefinita, queste viste reindirizzeranno il successo a una pagina che visualizza l'elemento del modello appena creato/modificato, che nel nostro caso sarà la vista di dettaglio dell'autore che abbiamo creato in un tutorial precedente. Puoi specificare una posizione di reindirizzamento alternativa dichiarando esplicitamente il parametro `success_url`.

La classe `AuthorDelete` non ha bisogno di visualizzare alcun campo, quindi questi non devono essere specificati.
Impostiamo anche un `success_url` (come mostrato sopra), perché non c'è un URL predefinito ovvio verso il quale Django può navigare dopo aver cancellato con successo l'`Author`. Sopra utilizziamo la funzione [`reverse_lazy()`](https://docs.djangoproject.com/en/5.0/ref/urlresolvers/#reverse-lazy) per reindirizzare alla nostra lista degli autori dopo che un autore è stato eliminato — `reverse_lazy()` è una versione eseguita pigramente di `reverse()`, usata qui perché stiamo fornendo un URL a un attributo di vista basata su classe.

Se l'eliminazione degli autori dovrebbe sempre avere successo, ciò sarebbe sufficiente.
Purtroppo eliminare un `Author` causerà un'eccezione se l'autore ha un libro associato, perché il nostro [`modello Book`](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models#book_model) specifica `on_delete=models.RESTRICT` per il campo `ForeignKey` dell'autore.
Per gestire questo caso la vista sovrascrive il metodo [`form_valid()`](https://docs.djangoproject.com/en/5.0/ref/class-based-views/mixins-editing/#django.views.generic.edit.FormMixin.form_valid) in modo che se l'eliminazione dell'`Author` riesce, reindirizza all'`success_url`, ma se no, reindirizza semplicemente di nuovo allo stesso modulo.
Aggiorneremo il template qui sotto per chiarire che non puoi eliminare un'istanza di `Author` che è usata in un qualsiasi `Book`.

### Configurazioni URL

Apri il tuo file di configurazione dell'URL (**django-locallibrary-tutorial/catalog/urls.py**) e aggiungi la seguente configurazione in fondo al file:

```python
urlpatterns += [
    path('author/create/', views.AuthorCreate.as_view(), name='author-create'),
    path('author/<int:pk>/update/', views.AuthorUpdate.as_view(), name='author-update'),
    path('author/<int:pk>/delete/', views.AuthorDelete.as_view(), name='author-delete'),
]
```

Non c'è niente di particolarmente nuovo qui! Puoi vedere che le viste sono classi, e devono quindi essere chiamate tramite `.as_view()`, e dovresti essere in grado di riconoscere i pattern URL in ogni caso. Dobbiamo usare `pk` come nome per il nostro valore di chiave primaria catturato, poiché questo è il nome del parametro previsto dalle classi vista.

### Template

Le viste "crea" e "aggiorna" usano lo stesso template per impostazione predefinita, che sarà chiamato come il tuo modello: `model_name_form.html` (puoi cambiare il suffisso in qualcosa diverso da **\_form** usando il campo `template_name_suffix` nella tua vista, per esempio, `template_name_suffix = '_other_suffix'`)

Crea il file del template `django-locallibrary-tutorial/catalog/templates/catalog/author_form.html` e copia il testo qui sotto.

```django
{% extends "base_generic.html" %}

{% block content %}
<form action="" method="post">
  {% csrf_token %}
  <table>
    \{{ form.as_table }}
  </table>
  <input type="submit" value="Submit" />
</form>
{% endblock %}
```

Questo è simile ai nostri moduli precedenti e rende i campi usando una tabella. Nota anche come dichiariamo di nuovo il `{% csrf_token %}` per assicurare che i nostri moduli siano resistenti agli attacchi CSRF.

La vista "elimina" si aspetta di trovare un template denominato nel formato `[model_name]_confirm_delete.html` (di nuovo, puoi cambiare il suffisso usando `template_name_suffix` nella tua vista).
Crea il file del template `django-locallibrary-tutorial/catalog/templates/catalog/author_confirm_delete.html` e copia il testo qui sotto.

```django
{% extends "base_generic.html" %}

{% block content %}

<h1>Delete Author: \{{ author }}</h1>

{% if author.book_set.all %}

<p>You can't delete this author until all their books have been deleted:</p>
<ul>
  {% for book in author.book_set.all %}
    <li><a href="{% url 'book-detail' book.pk %}">\{{book}}</a> (\{{book.bookinstance_set.all.count}})</li>
  {% endfor %}
</ul>

{% else %}
<p>Are you sure you want to delete the author?</p>

<form action="" method="POST">
  {% csrf_token %}
  <input type="submit" action="" value="Yes, delete.">
</form>
{% endif %}

{% endblock %}
```

Il template dovrebbe essere familiare.
Controlla prima se l'autore è utilizzato in qualche libro e, in tal caso, visualizza l'elenco dei libri che devono essere eliminati prima di poter eliminare il record dell'autore.
In caso contrario, visualizza un modulo che chiede all'utente di confermare se vogliono eliminare il record dell'autore.

L'ultimo passaggio è collegare le pagine alla barra laterale.
Per prima cosa, aggiungeremo un link per creare l'autore nel _template base_, in modo che sia visibile in tutte le pagine per gli utenti che sono considerati "staff" e che hanno il permesso di creare autori (`catalog.add_author`).
Apri **/django-locallibrary-tutorial/catalog/templates/base_generic.html** e aggiungi le righe che permettono agli utenti con il permesso di creare l'autore (nello stesso blocco del link che mostra "Tutti i libri presi in prestito").
Ricorda di fare riferimento all'URL usando il suo nome `'author-create'` come mostrato di seguito.

```django
{% if user.is_staff %}
<hr>
<ul class="sidebar-nav">
<li>Staff</li>
   <li><a href="{% url 'all-borrowed' %}">All borrowed</a></li>
{% if perms.catalog.add_author %}
   <li><a href="{% url 'author-create' %}">Create author</a></li>
{% endif %}
</ul>
{% endif %}
```

Aggiungeremo i link per aggiornare e cancellare gli autori alla pagina dei dettagli dell'autore.
Apri **catalog/templates/catalog/author_detail.html** e aggiungi il seguente codice.

```django
{% block sidebar %}
  \{{ block.super }}

  {% if perms.catalog.change_author or perms.catalog.delete_author %}
  <hr>
  <ul class="sidebar-nav">
    {% if perms.catalog.change_author %}
      <li><a href="{% url 'author-update' author.id %}">Update author</a></li>
    {% endif %}
    {% if not author.book_set.all and perms.catalog.delete_author %}
      <li><a href="{% url 'author-delete' author.id %}">Delete author</a></li>
    {% endif %}
    </ul>
  {% endif %}

{% endblock %}
```

Questo blocco sovrascrive il blocco `sidebar` nel template base e poi inserisce il contenuto originale usando `\{{ block.super }}`.
Quindi aggiunge i link per aggiornare o cancellare l'autore, ma solo quando l'utente ha i permessi corretti e il record dell'autore non è associato a libri.

Le pagine sono pronte per essere testate!

### Testare la pagina

Prima, accedi al sito con un account che ha i permessi di aggiunta, modifica e cancellazione dell'autore.

Naviga a qualsiasi pagina e seleziona "Creare autore" nella barra laterale (con l'URL `http://127.0.0.1:8000/catalog/author/create/`).
La pagina dovrebbe apparire come nello screenshot qui sotto.

![Form Example: Create Author](forms_example_create_author.png)

Inserisci i valori per i campi e poi premi **Submit** per salvare il record dell'autore.
Dovresti ora essere portato a una vista dettagliata per il tuo nuovo autore, con un URL di qualcosa come `http://127.0.0.1:8000/catalog/author/10`.

![Form Example: Author Detail showing Update and Delete links](forms_example_detail_author_update.png)

Puoi testare la modifica del record selezionando il link "Aggiorna autore" (con URL qualcosa come `http://127.0.0.1:8000/catalog/author/10/update/`) — non mostriamo uno screenshot perché è uguale alla pagina "crea"!

Infine, possiamo eliminare la pagina selezionando "Elimina autore" dalla barra laterale nella pagina dettagli.
Django dovrebbe visualizzare la pagina di eliminazione mostrata sotto se il record dell'autore non è utilizzato in nessun libro.
Premi "**Sì, elimina.**" per rimuovere il record e essere portato all'elenco di tutti gli autori.

![Form with option to delete author](forms_example_delete_author.png)

## Sfida te stesso

Crea alcuni moduli per creare, modificare e cancellare i record `Book`. Puoi usare esattamente la stessa struttura per `Authors` (per l'eliminazione, ricorda che non puoi eliminare un `Book` finché tutti i suoi record associati `BookInstance` non sono cancellati) e devi usare i permessi corretti.
Se il tuo template **book_form.html** è solo una copia-rinominata del template **author_form.html**, allora la nuova pagina "crea libro" apparirà come nello screenshot di seguito:

![Screenshot displaying various fields in the form like title, author, summary, ISBN, genre and language](forms_example_create_book.png)

## Sommario

Creare e gestire i moduli può essere un processo complicato! Django lo rende molto più facile fornendo meccanismi programmatici per dichiarare, rendere e validare i moduli. Inoltre, Django fornisce viste di modifica generiche dei moduli che possono fare _quasi tutto_ il lavoro di definizione delle pagine che possono creare, modificare e cancellare record associati a un'istanza di modello singola.

C'è molto di più che può essere fatto con i moduli (dai un'occhiata al nostro [Vedi anche](#vedi_anche) elenco qui sotto), ma ora dovresti capire come aggiungere moduli di base e codice di gestione dei moduli ai tuoi siti web.

## Vedi anche

- [Lavorare con i moduli](https://docs.djangoproject.com/en/5.0/topics/forms/) (documenti Django)
- [Scrivere la tua prima app Django, parte 4 > Scrivere un modulo semplice](https://docs.djangoproject.com/en/5.0/intro/tutorial04/#write-a-simple-form) (documenti Django)
- [L'API dei moduli](https://docs.djangoproject.com/en/5.0/ref/forms/api/) (documenti Django)
- [Campi del modulo](https://docs.djangoproject.com/en/5.0/ref/forms/fields/) (documenti Django)
- [Validazione del modulo e dei campi](https://docs.djangoproject.com/en/5.0/ref/forms/validation/) (documenti Django)
- [Gestione dei moduli con le viste basate su classe](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-editing/) (documenti Django)
- [Creare moduli dai modelli](https://docs.djangoproject.com/en/5.0/topics/forms/modelforms/) (documenti Django)
- [Viste di modifica generiche](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/) (documenti Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/authentication_and_sessions", "Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django")}}
