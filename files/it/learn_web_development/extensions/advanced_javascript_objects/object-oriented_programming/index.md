---
title: Programmazione orientata agli oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

La programmazione orientata agli oggetti (OOP) è un paradigma di programmazione fondamentale per molti linguaggi di programmazione, tra cui Java e C++. In questo articolo, forniremo una panoramica dei concetti di base dell'OOP. Descriveremo tre concetti principali: **classi e istanze**, **ereditarietà** e **incapsulamento**. Per ora, descriveremo questi concetti senza riferimento specifico a JavaScript, quindi tutti gli esempi sono forniti in {{Glossary("Pseudocode", "pseudocodice")}}.

> [!NOTE]
> Per essere precisi, le caratteristiche descritte qui sono di uno stile particolare di OOP chiamato OOP **basata sulle classi** o "classica". Quando si parla di OOP, generalmente si intende questo tipo.

Successivamente, in JavaScript, esamineremo come i costruttori e la catena dei prototipi si riferiscono a questi concetti di OOP e come differiscono. Nel prossimo articolo, esamineremo alcune caratteristiche aggiuntive di JavaScript che rendono più facile implementare programmi orientati agli oggetti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (soprattutto
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Fondamenti sugli oggetti</a>) e concetti di JavaScript orientato agli oggetti trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Concetti di programmazione orientata agli oggetti (OOP): Classi, istanze, ereditarietà e incapsulamento.</li>
          <li>Come questi concetti di OOP si applicano a JavaScript e quali sono le differenze rispetto a un linguaggio come Java o C++.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

La programmazione orientata agli oggetti riguarda la modellazione di un sistema come una raccolta di oggetti, dove ogni oggetto rappresenta un particolare aspetto del sistema. Gli oggetti contengono sia funzioni (o metodi) sia dati. Un oggetto fornisce un'interfaccia pubblica ad altro codice che vuole usarlo, ma mantiene il proprio stato interno privato; le altre parti del sistema non devono preoccuparsi di ciò che accade all'interno dell'oggetto.

## Classi e istanze

Quando modelliamo un problema in termini di oggetti nell'OOP, creiamo definizioni astratte che rappresentano i tipi di oggetti che vogliamo avere nel nostro sistema. Ad esempio, se stessimo modellando una scuola, potremmo voler avere oggetti che rappresentano professori. Ogni professore condivide alcune proprietà: tutti hanno un nome e una materia che insegnano. Inoltre, ogni professore può fare certe cose: possono tutti correggere un compito e presentarsi ai propri studenti all'inizio dell'anno, per esempio.

Quindi `Professor` potrebbe essere una **classe** nel nostro sistema. La definizione della classe elenca i dati e i metodi che ogni professore ha.

In pseudocodice, una classe `Professor` potrebbe essere scritta così:

```plain
class Professor
    properties
        name
        teaches
    methods
        grade(paper)
        introduceSelf()
```

Questa definisce una classe `Professor` con:

- due proprietà di dati: `name` e `teaches`
- due metodi: `grade()` per correggere un compito e `introduceSelf()` per presentarsi.

Da sola, una classe non fa nulla: è una sorta di modello per creare oggetti concreti di quel tipo. Ogni professore concreto che creiamo è chiamato un'**istanza** della classe `Professor`. Il processo di creazione di un'istanza è eseguito da una funzione speciale chiamata **costruttore**. Passiamo valori al costruttore per qualsiasi stato interno che vogliamo inizializzare nella nuova istanza.

Generalmente, il costruttore è scritto come parte della definizione della classe e di solito ha lo stesso nome della classe stessa:

```plain
class Professor
    properties
        name
        teaches
    constructor
        Professor(name, teaches)
    methods
        grade(paper)
        introduceSelf()
```

Questo costruttore accetta due parametri, quindi possiamo inizializzare le proprietà `name` e `teaches` quando creiamo un nuovo professore concreto.

Ora che abbiamo un costruttore, possiamo creare alcuni professori. I linguaggi di programmazione spesso usano la parola chiave `new` per segnalare che un costruttore viene chiamato.

```js
walsh = new Professor("Walsh", "Psychology");
lillian = new Professor("Lillian", "Poetry");

walsh.teaches; // 'Psychology'
walsh.introduceSelf(); // 'My name is Professor Walsh and I will be your Psychology professor.'

lillian.teaches; // 'Poetry'
lillian.introduceSelf(); // 'My name is Professor Lillian and I will be your Poetry professor.'
```

Questo crea due oggetti, entrambe istanze della classe `Professor`.

## Ereditarietà

Supponiamo che nella nostra scuola vogliamo anche rappresentare gli studenti. A differenza dei professori, gli studenti non possono correggere i compiti, non insegnano una materia specifica e appartengono a un anno particolare.

Tuttavia, gli studenti hanno un nome e possono anche voler presentarsi, quindi potremmo scrivere la definizione di una classe studente così:

```plain
class Student
    properties
        name
        year
    constructor
        Student(name, year)
    methods
        introduceSelf()
```

Sarebbe utile se potessimo rappresentare il fatto che studenti e professori condividono alcune proprietà, o più accuratamente, il fatto che a un certo livello, essi sono lo _stesso tipo di cosa_. **L'ereditarietà** ci consente di fare questo.

Iniziamo osservando che studenti e professori sono entrambi persone, e le persone hanno nomi e desiderano presentarsi. Possiamo modellare questo definendo una nuova classe `Person`, in cui definiamo tutte le proprietà comuni delle persone. Poi, `Professor` e `Student` possono entrambi **derivare** da `Person`, aggiungendo le loro proprietà extra:

```plain
class Person
    properties
        name
    constructor
        Person(name)
    methods
        introduceSelf()

class Professor : extends Person
    properties
        teaches
    constructor
        Professor(name, teaches)
    methods
        grade(paper)
        introduceSelf()

class Student : extends Person
    properties
        year
    constructor
        Student(name, year)
    methods
        introduceSelf()
```

In questo caso, diremmo che `Person` è la **superclasse** o **classe genitore** sia di `Professor` sia di `Student`. Al contrario, `Professor` e `Student` sono **sottocategorie** o **classi figlio** di `Person`.

Potrebbe notare che `introduceSelf()` è definito in tutte e tre le classi. Il motivo è che mentre tutte le persone vogliono presentarsi, il modo in cui lo fanno è diverso:

```js
walsh = new Professor("Walsh", "Psychology");
walsh.introduceSelf(); // 'My name is Professor Walsh and I will be your Psychology professor.'

summers = new Student("Summers", 1);
summers.introduceSelf(); // 'My name is Summers and I'm in the first year.'
```

Potremmo avere un'implementazione predefinita di `introduceSelf()` per le persone che non sono studenti _o_ professori:

```js
pratt = new Person("Pratt");
pratt.introduceSelf(); // 'My name is Pratt.'
```

Questa caratteristica - quando un metodo ha lo stesso nome ma una diversa implementazione in classi differenti - è chiamata **polimorfismo**. Quando un metodo in una sottoclasse sostituisce l'implementazione della superclasse, diciamo che la sottoclasse **sovrascrive** la versione nella superclasse.

## Incapsulamento

Gli oggetti forniscono un'interfaccia ad altro codice che vuole usarli ma mantengono il proprio stato interno. Lo stato interno dell'oggetto è mantenuto **privato**, il che significa che può essere accessibile solo dai metodi dell'oggetto stesso, non da altri oggetti. Mantenere lo stato interno di un oggetto privato e generalmente fare una chiara distinzione tra la sua interfaccia pubblica e il suo stato interno privato, è chiamato **incapsulamento**.

Questa è una caratteristica utile perché consente al programmatore di modificare l'implementazione interna di un oggetto senza dover trovare e aggiornare tutto il codice che lo utilizza: crea una sorta di firewall tra questo oggetto e il resto del sistema.

Ad esempio, supponiamo che gli studenti possano studiare tiro con l'arco se sono al secondo anno o superiore. Potremmo implementare questo semplicemente esponendo la proprietà `year` dello studente, e altro codice potrebbe esaminare quella per decidere se lo studente può partecipare al corso:

```js
if (student.year > 1) {
  // allow the student into the class
}
```

Il problema è che, se decidiamo di cambiare i criteri per permettere agli studenti di studiare tiro con l'arco - per esempio richiedendo anche il permesso del genitore o tutore - dovremmo aggiornare ogni luogo nel nostro sistema che esegue questo test. Sarebbe meglio avere un metodo `canStudyArchery()` sugli oggetti `Student`, che implementi la logica in un solo posto:

```plain
class Student : extends Person
    properties
       year
    constructor
       Student(name, year)
    methods
       introduceSelf()
       canStudyArchery() { return this.year > 1 }
```

```js
if (student.canStudyArchery()) {
  // allow the student into the class
}
```

In questo modo, se vogliamo cambiare le regole sullo studio del tiro con l'arco, dobbiamo solo aggiornare la classe `Student`, e tutto il codice che la utilizza continuerà a funzionare.

In molti linguaggi OOP, possiamo impedire ad altro codice di accedere allo stato interno di un oggetto marcando alcune proprietà come `private`. Questo genererà un errore se del codice esterno all'oggetto tenta di accedervi:

```plain
class Student : extends Person
    properties
       private year
    constructor
        Student(name, year)
    methods
       introduceSelf()
       canStudyArchery() { return this.year > 1 }

student = new Student('Weber', 1)
student.year // error: 'year' is a private property of Student
```

Nei linguaggi che non applicano l'accesso in questo modo, i programmatori usano convenzioni di denominazione, come iniziare il nome con un underscore, per indicare che la proprietà dovrebbe essere considerata privata.

## OOP e JavaScript

In questo articolo, abbiamo descritto alcune delle caratteristiche di base della programmazione orientata agli oggetti basata sulle classi come implementata in linguaggi come Java e C++.

Negli articoli precedenti, abbiamo esaminato un paio di caratteristiche fondamentali di JavaScript: [costruttori](/it/docs/Learn_web_development/Core/Scripting/Object_basics) e [prototipi](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes). Queste caratteristiche hanno certamente qualche relazione con alcuni dei concetti di OOP descritti sopra.

- I **costruttori** in JavaScript ci forniscono qualcosa di simile a una definizione di classe, permettendoci di definire la "forma" di un oggetto, incluso qualsiasi metodo contenga, in un unico posto. Ma anche i prototipi possono essere usati qui. Ad esempio, se un metodo è definito sulla proprietà `prototype` di un costruttore, allora tutti gli oggetti creati usando quel costruttore ottengono quel metodo tramite il loro prototipo, e non è necessario definirlo nel costruttore.

- **la catena dei prototipi** sembra un modo naturale per implementare l'ereditarietà. Ad esempio, se possiamo avere un oggetto `Student` il cui prototipo è `Person`, allora può ereditare `name` e sovrascrivere `introduceSelf()`.

Ma vale la pena comprendere le differenze tra queste caratteristiche e i concetti di OOP "classici" descritti sopra. Evidenzieremo un paio di esse qui.

In primo luogo, nell'OOP basato sulle classi, classi e oggetti sono due costrutti separati, e gli oggetti sono sempre creati come istanze delle classi. Inoltre, c'è una distinzione tra la funzione utilizzata per definire una classe (la sintassi della classe stessa) e la funzione utilizzata per istanziare un oggetto (un costruttore). In JavaScript, possiamo e spesso creiamo oggetti senza alcuna definizione di classe separata, usando una funzione o un oggetto letterale. Questo può rendere il lavoro con gli oggetti molto più leggero rispetto a quanto avviene nell'OOP classico.

In secondo luogo, sebbene una catena di prototipi sembri una gerarchia di ereditarietà e si comporti in qualche modo come essa, è diversa in altri modi. Quando una sottoclasse viene istanziata, un singolo oggetto viene creato che combina le proprietà definite nella sottoclasse con le proprietà definite più in alto nella gerarchia. Con il prototipazione, ogni livello della gerarchia è rappresentato da un oggetto separato, e sono collegati insieme tramite la proprietà `__proto__`. Il comportamento della catena di prototipi è meno simile all'ereditarietà e più simile alla **delegazione**. La delegazione è un pattern di programmazione in cui un oggetto, quando viene chiesto di eseguire un compito, può eseguire il compito da solo o chiedere ad un altro oggetto (il suo **delegato**) di eseguire il compito per suo conto. In molti modi, la delegazione è un modo più flessibile per combinare oggetti rispetto all'ereditarietà (per una cosa, è possibile cambiare o sostituire completamente il delegato a tempo di esecuzione).

Detto ciò, i costruttori e i prototipi possono essere usati per implementare pattern di OOP basata sulle classi in JavaScript. Ma usarli direttamente per implementare caratteristiche come l'ereditarietà è complicato, quindi JavaScript fornisce caratteristiche extra, stratificate sul modello di prototipo, che mappano più direttamente ai concetti di OOP basata sulle classi. Queste caratteristiche extra sono l'argomento del prossimo articolo.

## Sommario

Questo articolo ha descritto le caratteristiche di base della programmazione orientata agli oggetti basata sulle classi e ha brevemente esaminato come i costruttori e i prototipi di JavaScript si confrontano con questi concetti.

Nel prossimo articolo, esamineremo le caratteristiche che JavaScript fornisce per supportare la programmazione orientata agli oggetti basata sulle classi.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
