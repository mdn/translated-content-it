---
title: "Sfida: mini blog Django fai da te"
short-title: "Sfida: blog Django"
slug: Learn_web_development/Extensions/Server-side/Django/django_assessment_blog
l10n:
  sourceCommit: 2530db14de9ac226cf06f84540fa0101e804ca9b
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}

In questa sfida verranno usate le conoscenze di Django acquisite nel modulo [Django Web Framework (Python)](/it/docs/Learn_web_development/Extensions/Server-side/Django) per creare un blog molto semplice.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima di affrontare questa sfida, è necessario aver già completato tutti gli articoli di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Verificare la comprensione dei fondamenti di Django, incluse configurazioni URL, modelli, viste, moduli e template.
      </td>
    </tr>
  </tbody>
</table>

## Descrizione del progetto

Di seguito sono elencate le pagine da visualizzare, i relativi URL e altri requisiti:

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
      <td>Pagina iniziale</td>
      <td><code>/</code> e <code>/blog/</code></td>
      <td>Una pagina indice che descrive il sito.</td>
    </tr>
    <tr>
      <td>Elenco di tutti gli articoli del blog</td>
      <td><code>/blog/blogs/</code></td>
      <td>
        <p>Elenco di tutti gli articoli del blog:</p>
        <ul>
          <li>Accessibile a tutti gli utenti tramite un collegamento nella barra laterale.</li>
          <li>Elenco ordinato per data di pubblicazione (dal più recente al meno recente).</li>
          <li>Elenco paginato in gruppi di 5 articoli.</li>
          <li>Gli elementi dell'elenco mostrano il titolo del blog, la data di pubblicazione e l'autore.</li>
          <li>I nomi degli articoli del blog sono collegati alle pagine di dettaglio del blog.</li>
          <li>
            I blogger (nomi degli autori) sono collegati alle pagine di dettaglio dell'autore del blog.
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
          Informazioni su un autore specificato (tramite id) ed elenco dei suoi articoli del blog:
        </p>
        <ul>
          <li>Accessibile a tutti gli utenti tramite i collegamenti degli autori negli articoli del blog ecc.</li>
          <li>
            Contiene alcune informazioni biografiche sul blogger/autore.
          </li>
          <li>Elenco ordinato per data di pubblicazione (dal più recente al meno recente).</li>
          <li>Non paginato.</li>
          <li>Gli elementi dell'elenco mostrano solo il nome dell'articolo del blog e la data di pubblicazione.</li>
          <li>I nomi degli articoli del blog sono collegati alle pagine di dettaglio del blog.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina di dettaglio dell'articolo del blog</td>
      <td>
        <code>/blog/<em>&#x3C;blog-id></em></code>
      </td>
      <td>
        <p>Dettagli dell'articolo del blog.</p>
        <ul>
          <li>Accessibile a tutti gli utenti dagli elenchi degli articoli del blog.</li>
          <li>
            La pagina contiene il nome, l'autore, la data di pubblicazione e il contenuto dell'articolo del blog.
          </li>
          <li>I commenti dell'articolo del blog devono essere visualizzati in fondo.</li>
          <li>I commenti devono essere ordinati dal più vecchio al più recente.</li>
          <li>
            Contiene un collegamento per aggiungere commenti alla fine per gli utenti che hanno effettuato l'accesso (vedere la pagina del modulo Comment).
          </li>
          <li>
            Gli articoli del blog e i commenti devono visualizzare solo testo semplice.
            Non è necessario supportare alcun tipo di markup HTML (ad esempio, collegamenti, immagini, grassetto/corsivo ecc.).
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
          <li>I nomi dei blogger sono collegati alle pagine di dettaglio dell'autore del blog.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Pagina del modulo per i commenti</td>
      <td><code>/blog/<em>&#x3C;blog-id></em>/create</code></td>
      <td>
        <p>Crea un commento per un articolo del blog:</p>
        <ul>
          <li>
            Accessibile agli utenti che hanno effettuato l'accesso (solo) tramite il collegamento in fondo alle pagine di dettaglio degli articoli del blog.
          </li>
          <li>
            Visualizza un modulo con una descrizione per inserire commenti (la data di pubblicazione e il blog non sono modificabili).
          </li>
          <li>
            Dopo che è stato pubblicato un commento, la pagina reindirizzerà alla pagina dell'articolo del blog associato.
          </li>
          <li>Gli utenti non possono modificare o eliminare i propri articoli.</li>
          <li>
            Gli utenti che non hanno effettuato l'accesso verranno indirizzati alla pagina di login per accedere,
            prima di poter aggiungere commenti. Dopo aver effettuato l'accesso, verranno
            reindirizzati alla pagina del blog su cui volevano commentare.
          </li>
          <li>
            Le pagine dei commenti devono includere il nome/collegamento all'articolo del blog commentato.
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
          Pagine standard di autenticazione Django per effettuare l'accesso, disconnettersi e impostare la password:
        </p>
        <ul>
          <li>Login/logout devono essere accessibili tramite collegamenti nella barra laterale.</li>
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
          Il sito di amministrazione deve essere abilitato per consentire la creazione/modifica/eliminazione di articoli del blog,
          autori del blog e commenti del blog (questo è il meccanismo con cui
          i blogger creano nuovi articoli del blog):
        </p>
        <ul>
          <li>
            I record degli articoli del blog nel sito di amministrazione devono visualizzare in linea l'elenco dei commenti associati (sotto ogni articolo del blog).
          </li>
          <li>
            I nomi dei commenti nel sito di amministrazione vengono creati troncando la descrizione del commento a 75 caratteri.
          </li>
          <li>Gli altri tipi di record possono usare una registrazione di base.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Inoltre, è necessario scrivere alcuni test di base per verificare che:

- Tutti i campi del modello abbiano l'etichetta e la lunghezza corrette.
- Tutti i modelli abbiano il nome dell'oggetto previsto (ad esempio, `__str__()` restituisce il valore previsto).
- I modelli abbiano l'URL previsto per i singoli record Blog e Comment (ad esempio, `get_absolute_url()` restituisce l'URL previsto).
- La BlogListView (pagina di tutti i blog) sia accessibile nella posizione prevista (ad esempio, /blog/blogs)
- La BlogListView (pagina di tutti i blog) sia accessibile all'URL denominato previsto (ad esempio, 'blogs')
- La BlogListView (pagina di tutti i blog) utilizzi il template previsto (ad esempio, quello predefinito)
- La BlogListView pagini i record in gruppi di 5 (almeno nella prima pagina)

> [!NOTE]
> Naturalmente esistono molti altri test che è possibile eseguire. Usare il proprio giudizio, ma ci si aspetta almeno i test elencati sopra.

La sezione seguente mostra [schermate](#schermate) di un sito che implementa i requisiti precedenti.

## Schermate

Le schermate seguenti forniscono un esempio di ciò che dovrebbe produrre il programma completato.

### Elenco di tutti gli articoli del blog

Viene visualizzato l'elenco di tutti gli articoli del blog (accessibile dal collegamento "All blogs" nella barra laterale). Aspetti da notare:

- La barra laterale elenca anche l'utente che ha effettuato l'accesso.
- I singoli articoli del blog e i blogger sono accessibili come collegamenti nella pagina.
- La paginazione è abilitata (in gruppi di 5).
- L'ordinamento va dal più recente al meno recente.

![Elenco di tutti i blog](diyblog_allblogs.png)

### Elenco di tutti i blogger

Fornisce collegamenti a tutti i blogger, tramite il collegamento "All bloggers" nella barra laterale. In questo caso, dalla barra laterale è possibile vedere che nessun utente ha effettuato l'accesso.

![Elenco di tutti i blogger](diyblog_blog_allbloggers.png)

### Pagina di dettaglio del blog

Mostra la pagina di dettaglio di un blog specifico.

![Dettaglio del blog con collegamento per aggiungere un commento](diyblog_blog_detail_add_comment.png)

Si noti che i commenti hanno una data _e_ un'ora e sono ordinati dal più vecchio al più recente (l'opposto dell'ordinamento dei blog). Alla fine è presente un collegamento per accedere al modulo che permette di aggiungere un nuovo commento. Se un utente non ha effettuato l'accesso, verrà invece visualizzato un suggerimento per accedere.

![Collegamento ai commenti quando l'accesso non è stato effettuato](diyblog_blog_detail_not_logged_in.png)

### Modulo per aggiungere un commento

Questo è il modulo per aggiungere commenti. Si noti che l'utente ha effettuato l'accesso. Quando l'operazione ha successo, si dovrebbe tornare alla pagina dell'articolo del blog associato.

![Modulo per aggiungere un commento](diyblog_comment_form.png)

### Biografia dell'autore

Visualizza le informazioni biografiche di un blogger insieme all'elenco dei suoi articoli del blog.

![Pagina di dettaglio del blogger](diyblog_blogger_detail.png)

## Passaggi da completare

Le sezioni seguenti descrivono ciò che occorre fare.

1. Creare un progetto scheletro e un'applicazione web per il sito (come descritto in [Tutorial Django Parte 2: creazione di un sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website)). Si potrebbero usare 'diyblog' come nome del progetto e 'blog' come nome dell'applicazione.
2. Creare modelli per gli articoli Blog, i Commenti e qualsiasi altro oggetto necessario. Durante la progettazione, ricordare:
   - Ogni commento avrà un solo blog, ma un blog può avere molti commenti.
   - Gli articoli del blog e i commenti devono essere ordinati per data di pubblicazione.
   - Non tutti gli utenti saranno necessariamente autori del blog, anche se qualunque utente può commentare.
   - Gli autori del blog devono includere anche informazioni biografiche.

3. Eseguire le migrazioni per i nuovi modelli e creare un superuser.
4. Usare il sito di amministrazione per creare alcuni articoli del blog e commenti del blog di esempio.
5. Creare viste, template e configurazioni URL per le pagine di elenco degli articoli del blog e dei blogger.
6. Creare viste, template e configurazioni URL per le pagine di dettaglio degli articoli del blog e dei blogger.
7. Creare una pagina con un modulo per aggiungere nuovi commenti (ricordando di renderla disponibile solo agli utenti che hanno effettuato l'accesso).

## Suggerimenti e consigli

Questo progetto è molto simile al tutorial [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website). Sarà possibile configurare lo scheletro, il comportamento di login/logout dell'utente, il supporto per file statici, viste, URL, moduli, template di base e configurazione del sito di amministrazione usando quasi tutti gli stessi approcci.

Alcuni suggerimenti generali:

1. La pagina indice può essere implementata come una vista funzione e un template di base (proprio come per locallibrary).
2. La vista elenco per gli articoli del blog e i blogger, e la vista di dettaglio per gli articoli del blog possono essere create usando le [viste generiche elenco e dettaglio](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views).
3. L'elenco degli articoli del blog per un determinato autore può essere creato usando una vista elenco blog generica e filtrando gli oggetti blog che corrispondono all'autore specificato.
   - Sarà necessario implementare `get_queryset(self)` per eseguire il filtro (in modo molto simile a quanto fatto nella classe della libreria `LoanedBooksAllListView`) e ottenere le informazioni sull'autore dall'URL.
   - Sarà inoltre necessario passare il nome dell'autore alla pagina nel contesto. Per farlo in una vista basata su classi, è necessario implementare `get_context_data()` (illustrato di seguito).

4. Il modulo per _aggiungere un commento_ può essere creato usando una vista basata su funzione (e il modello e il modulo associati) oppure usando un `CreateView` generico. Se si usa un `CreateView` (consigliato):
   - Sarà inoltre necessario passare il nome dell'articolo del blog alla pagina dei commenti nel contesto (implementando `get_context_data()` come illustrato di seguito).
   - Il modulo deve visualizzare solo la "description" del commento per l'immissione da parte dell'utente (la data e l'articolo del blog associato non devono essere modificabili). Poiché non saranno presenti nel modulo stesso, il codice dovrà impostare l'autore del commento nella funzione `form_valid()` affinché possa essere salvato nel modello ([come descritto qui](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-editing/#models-and-request-user) — documentazione di Django). Nella stessa funzione viene impostato il blog associato. Di seguito è mostrata una possibile implementazione (`pk` è un id del blog passato dall'URL/configurazione URL).

     ```python
         def form_valid(self, form):
             """
             Add author and associated blog to form data before setting it as valid (so it is saved to model)
             """
             # Add logged-in user as author of comment
             form.instance.author = self.request.user
             #Associate comment with blog based on passed id
             form.instance.blog=get_object_or_404(Blog, pk = self.kwargs['pk'])
             # Call super-class form validation behavior
             return super(BlogCommentCreate, self).form_valid(form)
     ```

   - Sarà necessario fornire un URL di successo a cui reindirizzare dopo la convalida del modulo; dovrebbe essere il blog originale. A questo scopo sarà necessario eseguire l'override di `get_success_url()` e applicare il "reverse" dell'URL per il blog originale. È possibile ottenere l'ID del blog richiesto usando l'attributo `self.kwargs`, come mostrato nel metodo `form_valid()` precedente.

Abbiamo brevemente parlato del passaggio di un contesto al template in una vista basata su classi nell'argomento [Tutorial Django Parte 6: viste generiche elenco e dettaglio](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views#overriding_methods_in_class-based_views). Per farlo è necessario eseguire l'override di `get_context_data()` (prima ottenendo il contesto esistente, aggiornandolo con eventuali variabili aggiuntive da passare al template e quindi restituendo il contesto aggiornato). Ad esempio, il frammento di codice seguente mostra come aggiungere un oggetto blogger al contesto in base al relativo id `BlogAuthor`.

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

La valutazione per questa sfida è [disponibile qui su GitHub](https://github.com/mdn/django-diy-blog/blob/main/MarkingGuide.md). Questa valutazione si basa principalmente su quanto l'applicazione soddisfa i requisiti elencati in precedenza, sebbene alcune parti verifichino che il codice usi modelli appropriati e che sia stato scritto almeno del codice di test.
Al termine, è possibile consultare [l'esempio completato](https://github.com/mdn/django-diy-blog), che corrisponde a un progetto con il massimo dei voti.

Dopo aver completato questo modulo, sarà stato completato anche tutto il contenuto MDN per apprendere la programmazione di base di siti web lato server con Django. Ci auguriamo che questo modulo sia stato utile e che abbia fornito una buona comprensione delle basi.

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}
