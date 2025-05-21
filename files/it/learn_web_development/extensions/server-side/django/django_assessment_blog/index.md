---
title: "Sfida: Mini blog DIY con Django"
short-title: "Sfida: Blog Django"
slug: Learn_web_development/Extensions/Server-side/Django/django_assessment_blog
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}

In questa sfida, utilizzerai le conoscenze di Django acquisite nel modulo [Django Web Framework (Python)](/it/docs/Learn_web_development/Extensions/Server-side/Django) per creare un blog molto semplice.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima di affrontare questa sfida, dovresti aver già completato tutti gli articoli in questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Testare la comprensione dei fondamenti di Django, tra cui configurazioni URL, modelli, viste, form, e template.
      </td>
    </tr>
  </tbody>
</table>

## Progetto

Le pagine che devono essere visualizzate, i loro URL e altri requisiti sono elencati di seguito:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Pagina</th>
      <th scope="col">URL</th>
      <th scope="col">Requisiti</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home page</td>
      <td><code>/</code> e <code>/blog/</code></td>
      <td>Un indice che descrive il sito.</td>
    </tr>
    <tr>
      <td>Elenco di tutti i post del blog</td>
      <td><code>/blog/blogs/</code></td>
      <td>
        <p>Elenco di tutti i post del blog:</p>
        <ul>
          <li>Accessibile a tutti gli utenti da un link nella barra laterale.</li>
          <li>Elenco ordinato per data del post (dal più recente al più vecchio).</li>
          <li>Elenco paginato in gruppi di 5 articoli.</li>
          <li>Gli elementi dell'elenco mostrano il titolo del blog, la data del post e l'autore.</li>
          <li>I nomi dei post del blog sono collegati alle pagine di dettaglio del blog.</li>
          <li>
            I nomi dei blogger (autori) sono collegati alle pagine di dettaglio dell'autore del blog.
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina di dettaglio dell'autore del blog (blogger)</td>
      <td>
        <code>/blog/blogger/<em>&#x3C;author-id></em></code>
      </td>
      <td>
        <p>
          Informazioni per un autore specificato (per id) e elenco dei suoi post del blog:
        </p>
        <ul>
          <li>Accessibile a tutti gli utenti dai link degli autori nei post del blog, ecc.</li>
          <li>
            Contiene alcune informazioni biografiche sul blogger/autore.
          </li>
          <li>Elenco ordinato per data del post (dal più recente al più vecchio).</li>
          <li>Non paginato.</li>
          <li>Gli elementi dell'elenco mostrano solo il nome del post del blog e la data del post.</li>
          <li>I nomi dei post del blog sono collegati alle pagine di dettaglio del blog.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina di dettaglio del post del blog</td>
      <td>
        <code>/blog/<em>&#x3C;blog-id></em></code>
      </td>
      <td>
        <p>Dettagli del post del blog.</p>
        <ul>
          <li>Accessibile a tutti gli utenti dagli elenchi dei post del blog.</li>
          <li>
            La pagina contiene il post del blog: nome, autore, data del post e contenuto.
          </li>
          <li>I commenti per il post del blog dovrebbero essere mostrati in fondo.</li>
          <li>I commenti dovrebbero essere ordinati in ordine: dal più vecchio al più recente.</li>
          <li>
            Contiene un link per aggiungere commenti alla fine per utenti loggati (vedi pagina del modulo Commenti).
          </li>
          <li>
            I post del blog e i commenti devono visualizzare solo testo semplice.
            Non c'è bisogno di supportare alcun tipo di formattazione HTML (es. link, immagini, grassetto/italico, ecc.).
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Elenco di tutti i blogger</td>
      <td><code>/blog/bloggers/</code></td>
      <td>
        <p>Elenco dei blogger sul sistema:</p>
        <ul>
          <li>Accessibile a tutti gli utenti dalla barra laterale del sito</li>
          <li>I nomi dei blogger sono collegati alle pagine di dettaglio dell'autore del blog.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina del modulo Commenti</td>
      <td><code>/blog/<em>&#x3C;blog-id></em>/create</code></td>
      <td>
        <p>Crea commento per post del blog:</p>
        <ul>
          <li>
            Accessibile agli utenti loggati (solo) da un link in fondo alla pagina di dettaglio del post del blog.
          </li>
          <li>
            Visualizza il modulo con descrizione per inserire i commenti (data del post e blog non sono modificabili).
          </li>
          <li>
            Dopo che un commento è stato postato, la pagina si reindirizzerà alla pagina del post del blog associato.
          </li>
          <li>Gli utenti non possono modificare o eliminare i loro post.</li>
          <li>
            Gli utenti non loggati verranno indirizzati alla pagina di login per accedere,
            prima di poter aggiungere commenti. Dopo il login, verranno
            reindirizzati alla pagina del blog su cui volevano commentare.
          </li>
          <li>
            Le pagine dei commenti dovrebbero includere il nome/link al post del blog su cui si sta commentando.
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagine di autenticazione utente</td>
      <td>
        <code>/accounts/<em>&#x3C;standard urls></em></code>
      </td>
      <td>
        <p>
          Pagine standard di autenticazione Django per accesso, uscita e impostazione della password:
        </p>
        <ul>
          <li>L'accesso/uscita dovrebbe essere accessibile tramite link nella barra laterale.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Sito di amministrazione</td>
      <td>
        <code>/admin/<em>&#x3C;standard urls></em></code>
      </td>
      <td>
        <p>
          Il sito di amministrazione dovrebbe essere abilitato per consentire la creazione/modifica/eliminazione di post del blog, autori del blog e commenti al blog (questo è il meccanismo per consentire ai blogger di creare nuovi post del blog):
        </p>
        <ul>
          <li>
            I record dei post del blog nel sito di amministrazione dovrebbero mostrare l'elenco dei commenti associati in linea (sotto ogni post del blog).
          </li>
          <li>
            I nomi dei commenti nel sito di amministrazione sono creati troncando la descrizione del commento a 75 caratteri.
          </li>
          <li>Altri tipi di record possono utilizzare la registrazione di base.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

In aggiunta dovresti scrivere alcuni test di base per verificare:

- Tutti i campi del modello hanno l'etichetta e la lunghezza corretta.
- Tutti i modelli hanno il nome dell'oggetto atteso (ad es., `__str__()` restituisce il valore atteso).
- I modelli hanno l'URL previsto per i singoli record di Blog e Commento (ad es., `get_absolute_url()` restituisce l'URL atteso).
- La `BlogListView` (pagina di tutti i blog) è accessibile nella posizione prevista (ad es., /blog/blogs)
- La `BlogListView` (pagina di tutti i blog) è accessibile all'URL denominato previsto (ad es., 'blogs')
- La `BlogListView` (pagina di tutti i blog) utilizza il template previsto (ad es., il predefinito)
- La `BlogListView` pagina i record per 5 (almeno nella prima pagina)

> [!NOTE]
> Ci sono ovviamente molti altri test che puoi eseguire. Usa il tuo giudizio, ma ci aspettiamo che tu esegua almeno i test sopra.

La sezione seguente mostra [screenshot](#screenshot) di un sito che implementa i requisiti sopra.

## Screenshot

Gli screenshot seguenti forniscono un esempio di ciò che il programma finito dovrebbe produrre.

### Elenco di tutti i post del blog

Questo visualizza l'elenco di tutti i post del blog (accessibile dal link "Tutti i blog" nella barra laterale). Cose da notare:

- La barra laterale elenca anche l'utente loggato.
- I singoli post del blog e i blogger sono accessibili come link nella pagina.
- La paginazione è abilitata (in gruppi di 5)
- Ordinamento è dal più recente al più vecchio.

![Elenco di tutti i blog](diyblog_allblogs.png)

### Elenco di tutti i blogger

Questo fornisce link a tutti i blogger, come collegato dal link "Tutti i blogger" nella barra laterale. In questo caso, possiamo vedere dalla barra laterale che nessun utente è loggato.

![Elenco di tutti i blogger](diyblog_blog_allbloggers.png)

### Pagina di dettaglio del blog

Questo mostra la pagina di dettaglio per un particolare blog.

![Dettaglio del blog con link per aggiungere commento](diyblog_blog_detail_add_comment.png)

Nota che i commenti hanno una data _e_ un'ora, e sono ordinati dal più vecchio al più nuovo (opposto dell'ordinamento del blog). Alla fine abbiamo un link per accedere al modulo per aggiungere un nuovo commento. Se un utente non è loggato, vedremmo invece un suggerimento per accedere.

![Link per commenti quando non si è loggati](diyblog_blog_detail_not_logged_in.png)

### Modulo per aggiungere commento

Questo è il modulo per aggiungere commenti. Nota che siamo loggati. Quando questo avrà successo, dovremmo essere riportati alla pagina del post del blog associato.

![Modulo per aggiungere commento](diyblog_comment_form.png)

### Biografia dell'autore

Questo visualizza informazioni biografiche per un blogger insieme al loro elenco di post del blog.

![Pagina di dettaglio del blogger](diyblog_blogger_detail.png)

## Passaggi da completare

Le sezioni seguenti descrivono cosa devi fare.

1. Crea un progetto scheletro e un'applicazione web per il sito (come descritto in [Django Tutorial Parte 2: Creare un sito scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website)). Potresti usare 'diyblog' per il nome del progetto e 'blog' per il nome dell'applicazione.
2. Crea modelli per i post del blog, i commenti, e qualsiasi altro oggetto necessario. Quando pensi al tuo design, ricorda:

   - Ogni commento avrà solo un blog, ma un blog può avere molti commenti.
   - I post del blog e i commenti devono essere ordinati per data del post.
   - Non tutti gli utenti saranno necessariamente autori del blog anche se ogni utente può essere un commentatore.
   - Gli autori del blog devono includere anche informazioni biografiche.

3. Esegui le migrazioni per i tuoi nuovi modelli e crea un superutente.
4. Usa il sito di amministrazione per creare alcuni esempi di post del blog e commenti del blog.
5. Crea viste, template, e configurazioni URL per le pagine dell'elenco dei post del blog e dei blogger.
6. Crea viste, template, e configurazioni URL per le pagine di dettaglio del post del blog e dei blogger.
7. Crea una pagina con un modulo per aggiungere nuovi commenti (ricorda di renderla disponibile solo agli utenti loggati!)

## Suggerimenti e trucchi

Questo progetto è molto simile al tutorial [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website). Sarai in grado di configurare lo scheletro, il comportamento di login/logout utenti, il supporto per i file statici, le viste, gli URL, i form, i template di base e la configurazione del sito di amministrazione utilizzando praticamente gli stessi approcci.

Alcuni suggerimenti generali:

1. La pagina indice può essere implementata come una vista di funzione base e un template (proprio come per la biblioteca locale).
2. La vista elenco per i post del blog e i blogger, e la vista dei dettagli per i post del blog possono essere create utilizzando le [viste elenco e di dettaglio generiche](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views).
3. L'elenco dei post del blog per un autore particolare può essere creato utilizzando una vista elenco blog generica e filtrando per oggetti blog che corrispondono all'autore specificato.

   - Dovrai implementare `get_queryset(self)` per fare il filtraggio (proprio come nella nostra classe `LoanedBooksAllListView` della biblioteca) e ottenere le informazioni sull'autore dall'URL.
   - Dovrai anche passare il nome dell'autore alla pagina nel contesto. Per fare questo in una vista basata su classi devi implementare `get_context_data()` (discusso di seguito).

4. Il modulo _aggiungi commento_ può essere creato utilizzando una vista basata su funzione (e un modello e un form associati) o utilizzando una `CreateView` generica. Se usi una `CreateView` (raccomandato) allora:

   - Dovrai anche passare il nome del post del blog alla pagina dei commenti nel contesto (implementa `get_context_data()` come discusso di seguito).
   - Il modulo dovrebbe visualizzare solo la "descrizione" del commento per l'inserimento utente (la data e il post del blog associato non devono essere modificabili). Dato che non saranno nel modulo stesso, il tuo codice dovrà impostare l'autore del commento nella funzione `form_valid()` affinché possa essere salvato nel modello ([come descritto qui](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-editing/#models-and-request-user) — documenti Django). Nella stessa funzione impostiamo il blog associato. Un'implementazione possibile è mostrata di seguito (`pk` è un id del blog passato dall'URL/configurazione URL).

     ```python
         def form_valid(self, form):
             """
             Add author and associated blog to form data before setting it as valid (so it is saved to model)
             """
             #Add logged-in user as author of comment
             form.instance.author = self.request.user
             #Associate comment with blog based on passed id
             form.instance.blog=get_object_or_404(Blog, pk = self.kwargs['pk'])
             # Call super-class form validation behavior
             return super(BlogCommentCreate, self).form_valid(form)
     ```

   - Dovrai fornire un URL di successo a cui reindirizzare dopo che il modulo è validato; questo dovrebbe essere il blog originale. Per fare ciò dovrai sovrascrivere `get_success_url()` e "invertire" l'URL per il blog originale. Puoi ottenere l'ID del blog richiesto utilizzando l'attributo `self.kwargs`, come mostrato nel metodo `form_valid()` sopra.

Abbiamo parlato brevemente di passare un contesto al template in una vista basata su classi nel topic [Django Tutorial Parte 6: Viste elenco e di dettaglio generiche](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views#overriding_methods_in_class-based_views). Per fare questo devi sovrascrivere `get_context_data()` (prima ottenendo il contesto esistente, aggiornandolo con le variabili aggiuntive che vuoi passare al template, e poi restituendo il contesto aggiornato). Ad esempio, il frammento di codice di seguito mostra come puoi aggiungere un oggetto blogger al contesto basato sul loro id `BlogAuthor`.

```python
class SomeView(generic.ListView):
    # …

    def get_context_data(self, **kwargs):
        # Call the base implementation first to get a context
        context = super(SomeView, self).get_context_data(**kwargs)
        # Get the blogger object from the "pk" URL parameter and add it to the context
        context['blogger'] = get_object_or_404(BlogAuthor, pk = self.kwargs['pk'])
        return context
```

## Valutazione

La valutazione per questa sfida è [disponibile su GitHub qui](https://github.com/mdn/django-diy-blog/blob/main/MarkingGuide.md). Questa valutazione è basata principalmente su quanto bene la tua applicazione soddisfa i requisiti che abbiamo elencato sopra, anche se ci sono alcune parti che verificano che il tuo codice utilizzi modelli appropriati e che tu abbia scritto almeno una parte dei test.
Quando hai finito, puoi dare un'occhiata all'[esempio finale](https://github.com/mdn/django-diy-blog) che riflette un progetto "con il massimo dei voti".

Una volta completato questo modulo, hai anche terminato tutti i contenuti MDN per imparare la programmazione di siti web lato server con Django! Speriamo che tu abbia gradito questo modulo e abbia una buona padronanza dei concetti di base!

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}
