---
title: "Sfida: mini blog DIY con Django"
short-title: "Sfida: blog Django"
slug: Learn_web_development/Extensions/Server-side/Django/django_assessment_blog
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}

In questa sfida, utilizzerà le conoscenze acquisite nel modulo [Django Web Framework (Python)](/it/docs/Learn_web_development/Extensions/Server-side/Django) per creare un blog di base.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima di tentare questa sfida, dovrebbe aver già completato tutti gli articoli in questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Per testare la comprensione dei fondamenti di Django, inclusa la configurazione degli URL, modelli, viste, form e template.
      </td>
    </tr>
  </tbody>
</table>

## Brief del progetto

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
      <td>Una pagina indice che descrive il sito.</td>
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
          <li>Gli elementi della lista mostrano il titolo del blog, la data del post e l'autore.</li>
          <li>I nomi dei post del blog sono collegati alle pagine dei dettagli del blog.</li>
          <li>
            I blogger (i nomi degli autori) sono collegati alle pagine dei dettagli degli autori del blog.
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina dei dettagli dell'autore del blog (blogger)</td>
      <td>
        <code>/blog/blogger/<em>&#x3C;author-id></em></code>
      </td>
      <td>
        <p>
          Informazioni per un autore specificato (per id) e elenco dei suoi post del blog:
        </p>
        <ul>
          <li>Accessibile a tutti gli utenti dai link agli autori nei post del blog ecc.</li>
          <li>
            Contiene alcune informazioni biografiche sul blogger/autore.
          </li>
          <li>Elenco ordinato per data del post (dal più recente al più vecchio).</li>
          <li>Non paginato.</li>
          <li>Gli elementi della lista mostrano solo il nome del post del blog e la data del post.</li>
          <li>I nomi dei post del blog sono collegati alle pagine dei dettagli del blog.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina dei dettagli del post del blog</td>
      <td>
        <code>/blog/<em>&#x3C;blog-id></em></code>
      </td>
      <td>
        <p>Dettagli del post del blog.</p>
        <ul>
          <li>Accessibile a tutti gli utenti dagli elenchi dei post del blog.</li>
          <li>
            La pagina contiene il post: nome, autore, data del post e contenuto.
          </li>
          <li>I commenti per il post del blog dovrebbero essere visualizzati in fondo.</li>
          <li>I commenti dovrebbero essere ordinati: dal più vecchio al più recente.</li>
          <li>
            Contiene un link per aggiungere commenti alla fine per gli utenti autenticati (vedi pagina del form dei commenti)
          </li>
          <li>
            I post del blog e i commenti devono visualizzare solo testo semplice.
            Non è necessario supportare alcun tipo di markup HTML (ad esempio, link, immagini, grassetto/italico, ecc.).
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Elenco di tutti i blogger</td>
      <td><code>/blog/bloggers/</code></td>
      <td>
        <p>Elenco dei blogger nel sistema:</p>
        <ul>
          <li>Accessibile a tutti gli utenti dalla barra laterale del sito</li>
          <li>I nomi dei blogger sono collegati alle pagine dei dettagli degli autori del blog.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina del form dei commenti</td>
      <td><code>/blog/<em>&#x3C;blog-id></em>/create</code></td>
      <td>
        <p>Crea un commento per un post del blog:</p>
        <ul>
          <li>
            Accessibile agli utenti autenticati (solo) dal link in fondo alle pagine dei dettagli del post del blog.
          </li>
          <li>
            Visualizza un modulo con descrizione per l'inserimento di commenti (la data del post e del blog non è modificabile).
          </li>
          <li>
            Dopo che un commento è stato pubblicato, la pagina verrà reindirizzata alla pagina del post del blog associato.
          </li>
          <li>Gli utenti non possono modificare o eliminare i loro post.</li>
          <li>
            Gli utenti non autenticati saranno indirizzati alla pagina di login per effettuare l'accesso,
            prima di poter aggiungere commenti. Dopo aver effettuato l'accesso, saranno
            reindirizzati alla pagina del blog su cui volevano commentare.
          </li>
          <li>
            Le pagine dei commenti dovrebbero includere il nome/link al post del blog a cui si riferiscono.
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
          Pagine standard di autenticazione Django per accedere, uscire e impostare la password:
        </p>
        <ul>
          <li>Accesso/uscita dovrebbero essere accessibili tramite link nella barra laterale.</li>
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
          Il sito di amministrazione dovrebbe essere abilitato per consentire la creazione/modifica/eliminazione di blog
          post, autori del blog e commenti (questo è il meccanismo per
          i blogger per creare nuovi post del blog):
        </p>
        <ul>
          <li>
            I record dei post del blog del sito di amministrazione dovrebbero mostrare l'elenco dei commenti associati in linea (sotto ciascun post del blog).
          </li>
          <li>
            I nomi dei commenti nel sito di amministrazione sono creati troncando la descrizione del commento a 75 caratteri.
          </li>
          <li>Altri tipi di record possono utilizzare una registrazione di base.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Inoltre dovrebbe scrivere alcuni test di base per verificare:

- Tutti i campi del modello hanno l'etichetta e la lunghezza corretta.
- Tutti i modelli hanno il nome dell'oggetto previsto (ad esempio, `__str__()` restituisce il valore atteso).
- I modelli hanno l'URL previsto per i singoli record di Blog e Commento (ad esempio, `get_absolute_url()` restituisce l'URL atteso).
- La BlogListView (pagina di tutti i blog) è accessibile nella posizione prevista (ad esempio, /blog/blogs)
- La BlogListView (pagina di tutti i blog) è accessibile all'URL nominale previsto (ad esempio, 'blogs')
- La BlogListView (pagina di tutti i blog) utilizza il modello previsto (ad esempio, il predefinito)
- La BlogListView pagina i record di 5 (almeno sulla prima pagina)

> [!NOTE]
> Ovviamente ci sono molti altri test che si possono eseguire. Usi il suo discernimento, ma ci aspettiamo che faccia almeno i test sopra elencati.

La sezione seguente mostra gli [screenshot](#screenshot) di un sito che implementa i requisiti sopra.

## Screenshot

I seguenti screenshot forniscono un esempio di ciò che il programma finito dovrebbe produrre.

### Elenco di tutti i post del blog

Questo visualizza l'elenco di tutti i post del blog (accessibile dal link "Tutti i blog" nella barra laterale). Cose da notare:

- La barra laterale elenca anche l'utente autenticato.
- I singoli post del blog e i blogger sono accessibili come link nella pagina.
- La paginazione è abilitata (in gruppi di 5)
- L'ordinamento è dal più recente al più vecchio.

![Elenco di tutti i blog](diyblog_allblogs.png)

### Elenco di tutti i blogger

Questo fornisce link a tutti i blogger, come collegato dal link "Tutti i blogger" nella barra laterale. In questo caso possiamo vedere dalla barra laterale che nessun utente è autenticato.

![Elenco di tutti i blogger](diyblog_blog_allbloggers.png)

### Pagina dei dettagli del blog

Questo mostra la pagina dei dettagli per un particolare blog.

![Dettaglio del blog con link per aggiungere commento](diyblog_blog_detail_add_comment.png)

Si noti che i commenti hanno una data _e_ ora, e sono ordinati dal più vecchio al più recente (opposto all'ordinamento dei blog). Alla fine abbiamo un link per accedere al modulo per aggiungere un nuovo commento. Se un utente non è autenticato vedremmo invece un suggerimento per effettuare l'accesso.

![Link al commento quando non si è autenticati](diyblog_blog_detail_not_logged_in.png)

### Form di aggiunta commento

Questo è il modulo per aggiungere commenti. Si noti che siamo autenticati. Quando questo avrà successo dovremmo essere riportati alla pagina del post del blog associato.

![Form per aggiungere commento](diyblog_comment_form.png)

### Bio dell'autore

Questo visualizza informazioni biografiche per un blogger insieme al loro elenco di post del blog.

![Dettaglio del blogger](diyblog_blogger_detail.png)

## Passaggi da completare

Le sezioni seguenti descrivono cosa deve fare.

1. Crei un progetto scheletro e un'applicazione web per il sito (come descritto in [Django Tutorial Part 2: Creating a skeleton website](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website)). Potrebbe utilizzare 'diyblog' per il nome del progetto e 'blog' per il nome dell'applicazione.
2. Creare modelli per i post del blog, i commenti e qualsiasi altro oggetto necessario. Quando pensa al suo design, ricordi:

   - Ogni commento avrà solo un blog, ma un blog può avere molti commenti.
   - I post del blog e i commenti devono essere ordinati per data del post.
   - Non ogni utente sarà necessariamente un autore del blog anche se qualsiasi utente può essere un commentatore.
   - Gli autori del blog devono includere anche informazioni biografiche.

3. Esegua le migrazioni per i suoi nuovi modelli e crei un superutente.
4. Utilizzi il sito di amministrazione per creare alcuni post del blog di esempio e commenti sul blog.
5. Crei viste, modelli e configurazioni URL per le pagine di elenco dei post del blog e dei blogger.
6. Crei viste, modelli e configurazioni URL per le pagine dei dettagli dei post del blog e dei blogger.
7. Crei una pagina con un modulo per aggiungere nuovi commenti (si ricordi di renderla disponibile solo agli utenti autenticati!)

## Suggerimenti e consigli

Questo progetto è molto simile al tutorial [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website). Sarà in grado di impostare lo scheletro, il comportamento di login/logout dell'utente, il supporto per i file statici, le viste, gli URL, i form, i modelli base e la configurazione del sito di amministrazione utilizzando quasi gli stessi approcci.

Alcuni suggerimenti generali:

1. La pagina indice può essere implementata come una funzione di vista base e un modello (proprio come per la locallibrary).
2. La lista visualizzata per i post del blog e i blogger, e la vista dettagliata per i post del blog possono essere create utilizzando le [visualizzazioni generiche di lista e di dettaglio](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views).
3. L'elenco dei post del blog per un particolare autore può essere creato utilizzando una vista di elenco blog generica e filtrando per oggetti blog che corrispondono all'autore specificato.

   - Dovrà implementare `get_queryset(self)` per fare il filtro (molto simile alla nostra classe biblioteca `LoanedBooksAllListView`) e ottenere le informazioni sull'autore dall'URL.
   - Inoltre, dovrà passare il nome dell'autore alla pagina nel contesto. Per farlo in una vista basata su classe è necessario implementare `get_context_data()` (discusso di seguito).

4. Il modulo _aggiungi commento_ può essere creato utilizzando una vista basata su funzione (e associato modello e form) o utilizzando una `CreateView` generica. Se utilizza una `CreateView` (consigliato) allora:

   - Inoltre, dovrà passare il nome del post del blog alla pagina del commento nel contesto (implementare `get_context_data()` come discusso di seguito).
   - Il form dovrebbe visualizzare solo la "descrizione" del commento per l'inserimento da parte dell'utente (la data e il post del blog associato non devono essere modificabili). Poiché non saranno nel form stesso, il suo codice dovrà impostare l'autore del commento nella funzione `form_valid()` così che possa essere salvato nel modello ([come descritto qui](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-editing/#models-and-request-user) — documenti Django). Nella stessa funzione impostiamo il blog associato. Una possibile implementazione è mostrata di seguito (`pk` è un id di blog passato dall'URL/configurazione URL).

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

   - Dovrà fornire un URL di successo a cui reindirizzare dopo che il form è stato validato; questo dovrebbe essere il blog originale. Per fare questo dovrà sovrascrivere `get_success_url()` e "invertire" l'URL per il blog originale. Può ottenere l'ID del blog richiesto utilizzando l'attributo `self.kwargs`, come mostrato nel metodo `form_valid()` sopra.

Abbiamo parlato brevemente del passaggio di un contesto al modello in una vista basata su classe nell'argomento [Django Tutorial Part 6: Generic list and detail views](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views#overriding_methods_in_class-based_views). Per fare questo è necessario sovrascrivere `get_context_data()` (prima ottenere il contesto esistente, aggiornarlo con le variabili aggiuntive che si desidera passare al modello, e poi restituire il contesto aggiornato). Ad esempio, il frammento di codice di seguito mostra come è possibile aggiungere un oggetto blogger al contesto basato sul loro id `BlogAuthor`.

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

La valutazione per questa sfida è [disponibile su GitHub qui](https://github.com/mdn/django-diy-blog/blob/main/MarkingGuide.md). Questa valutazione si basa principalmente su quanto bene la sua applicazione soddisfa i requisiti che abbiamo elencato sopra, sebbene ci siano alcune parti che verificano che il codice utilizzi modelli appropriati e che abbia scritto almeno un po' di codice di test.
Quando avrà finito, può controllare [l'esempio finito](https://github.com/mdn/django-diy-blog) che riflette un progetto "a pieni voti".

Una volta completato questo modulo, avrà anche completato tutto il contenuto MDN per apprendere la programmazione di siti web lato server di base con Django! Speriamo che abbia apprezzato questo modulo e che abbia una buona comprensione delle basi!

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}
