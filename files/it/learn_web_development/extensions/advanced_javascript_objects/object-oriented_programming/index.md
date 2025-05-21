---
title: Programmazione orientata agli oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

La programmazione orientata agli oggetti (OOP) è un paradigma di programmazione fondamentale per molti linguaggi, tra cui Java e C++. In questo articolo, forniremo una panoramica dei concetti di base dell'OOP. Descriveremo tre concetti principali: **classi e istanze**, **ereditarietà** e **incapsulamento**. Per ora, descriveremo questi concetti senza fare riferimento specifico a JavaScript, quindi tutti gli esempi sono forniti in {{Glossary("Pseudocode", "pseudocodice")}}.

> [!NOTE]
> Per essere precisi, le funzionalità descritte qui appartengono a uno stile particolare di OOP chiamato OOP **basato su classi** o "classico". Quando si parla di OOP, generalmente si intende questo tipo.

Successivamente, in JavaScript, esamineremo come i costruttori e la catena dei prototipi si relazionano a questi concetti dell'OOP e come differiscono. Nell'articolo successivo, esamineremo alcune funzionalità aggiuntive di JavaScript che facilitano l'implementazione di programmi orientati agli oggetti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (soprattutto
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">concetti base sugli oggetti</a>) e i concetti di JavaScript orientato agli oggetti trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Concetti di programmazione orientata agli oggetti (OOP): Classi, istanze, ereditarietà e incapsulamento.</li>
          <li>Come questi concetti OOP si applicano a JavaScript e quali sono le differenze con un linguaggio come Java o C++.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

La programmazione orientata agli oggetti riguarda la modellazione di un sistema come una raccolta di oggetti, dove ogni oggetto rappresenta un aspetto particolare del sistema. Gli oggetti contengono sia funzioni (o metodi) che dati. Un oggetto fornisce un'interfaccia pubblica ad altro codice che desidera utilizzarlo, ma mantiene il proprio stato interno privato; le altre parti del sistema non devono preoccuparsi di cosa succede all'interno dell'oggetto.

## Classi e istanze

Quando modelliamo un problema in termini di oggetti nell'OOP, creiamo definizioni astratte che rappresentano i tipi di oggetti che vogliamo avere nel nostro sistema. Ad esempio, se stessimo modellando una scuola, potremmo voler avere oggetti che rappresentano i professori. Ogni professore ha alcune proprietà in comune: tutti hanno un nome e una materia che insegnano. Inoltre, ogni professore può fare certe cose: tutti possono valutare un compito e possono presentarsi agli studenti all'inizio dell'anno, per esempio.

Quindi `Professor` potrebbe essere una **classe** nel nostro sistema. La definizione della classe elenca i dati e i metodi che ogni professore ha.

In pseudocodice, una classe `Professor` potrebbe essere scritta in questo modo:

```plain
class Professor
    properties
        name
        teaches
    methods
        grade(paper)
        introduceSelf()
```

Questo definisce una classe `Professor` con:

- due proprietà di dati: `name` e `teaches`
- due metodi: `grade()` per valutare un compito e `introduceSelf()` per presentarsi.

Da sola, una classe non fa nulla: è una sorta di modello per la creazione di oggetti concreti di quel tipo. Ogni professore concreto che creiamo è chiamato una **istanza** della classe `Professor`. Il processo di creazione di un'istanza è eseguito da una funzione speciale chiamata **costruttore**. Passiamo valori al costruttore per qualsiasi stato interno che vogliamo inizializzare nella nuova istanza.

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

Questo costruttore prende due parametri, in modo che possiamo inizializzare le proprietà `name` e `teaches` quando creiamo un nuovo professore concreto.

Ora che abbiamo un costruttore, possiamo creare alcuni professori. I linguaggi di programmazione spesso utilizzano la parola chiave `new` per segnalare che un costruttore viene chiamato.

```js
walsh = new Professor("Walsh", "Psychology");
lillian = new Professor("Lillian", "Poetry");

walsh.teaches; // 'Psychology'
walsh.introduceSelf(); // 'My name is Professor Walsh and I will be your Psychology professor.'

lillian.teaches; // 'Poetry'
lillian.introduceSelf(); // 'My name is Professor Lillian and I will be your Poetry professor.'
```

Questo crea due oggetti, entrambi istanze della classe `Professor`.

## Ereditarietà

Supponiamo che nella nostra scuola vogliamo anche rappresentare gli studenti. A differenza dei professori, gli studenti non possono valutare compiti, non insegnano una particolare materia e appartengono a un particolare anno.

Tuttavia, gli studenti hanno un nome e possono anche voler presentarsi, quindi potremmo scrivere la definizione di una classe di studenti in questo modo:

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

Sarebbe utile poter rappresentare il fatto che studenti e professori condividono alcune proprietà, o più accuratamente, il fatto che a un certo livello, sono lo _stesso tipo di cosa_. L'**ereditarietà** ci permette di fare questo.

Iniziamo osservando che studenti e professori sono entrambi persone, e le persone hanno nomi e vogliono presentarsi. Possiamo modellarlo definendo una nuova classe `Person`, in cui definiamo tutte le proprietà comuni delle persone. Poi, `Professor` e `Student` possono entrambi **derivare** da `Person`, aggiungendo le loro proprietà extra:

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

In questo caso, diremmo che `Person` è la **superclasse** o **classe genitore** sia di `Professor` che di `Student`. Viceversa, `Professor` e `Student` sono **sottoclassi** o **classi figlio** di `Person`.

Potresti notare che `introduceSelf()` è definito in tutte e tre le classi. La ragione è che, mentre tutte le persone vogliono presentarsi, il modo in cui lo fanno è diverso:

```js
walsh = new Professor("Walsh", "Psychology");
walsh.introduceSelf(); // 'My name is Professor Walsh and I will be your Psychology professor.'

summers = new Student("Summers", 1);
summers.introduceSelf(); // 'My name is Summers and I'm in the first year.'
```

Potremmo avere un'implementazione predefinita di `introduceSelf()` per le persone che non sono né studenti né professori:

```js
pratt = new Person("Pratt");
pratt.introduceSelf(); // 'My name is Pratt.'
```

Questa caratteristica - quando un metodo ha lo stesso nome ma una diversa implementazione in diverse classi - si chiama **polimorfismo**. Quando un metodo in una sottoclasse sostituisce l'implementazione della superclasse, diciamo che la sottoclasse **sovrascrive** la versione nella superclasse.

## Incapsulamento

Gli oggetti forniscono un'interfaccia ad altri codici che vogliono utilizzarli ma mantengono il proprio stato interno. Lo stato interno dell'oggetto è mantenuto **privato**, il che significa che può essere accesso solo dai metodi dell'oggetto stesso, non da altri oggetti. Mantenere lo stato interno di un oggetto privato e generalmente creare una chiara divisione tra la sua interfaccia pubblica e il suo stato interno privato è chiamato **incapsulamento**.

Questa è una caratteristica utile perché consente al programmatore di modificare l'implementazione interna di un oggetto senza dover trovare e aggiornare tutto il codice che lo utilizza: crea una sorta di firewall tra questo oggetto e il resto del sistema.

Per esempio, supponiamo che gli studenti siano autorizzati a studiare tiro con l'arco se sono nel secondo anno o superiori. Potremmo implementare questo semplicemente esponendo la proprietà `year` dello studente, e altro codice potrebbe esaminarla per decidere se lo studente può seguire il corso:

```js
if (student.year > 1) {
  // allow the student into the class
}
```

Il problema è che, se decidiamo di cambiare i criteri per consentire agli studenti di studiare tiro con l'arco - per esempio richiedendo anche il permesso del genitore o tutore - dovremmo aggiornare ogni parte nel nostro sistema che esegue questo test. Sarebbe meglio avere un metodo `canStudyArchery()` sugli oggetti `Student`, che implementa la logica in un solo posto:

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

In molti linguaggi OOP, possiamo prevenire che altro codice acceda allo stato interno di un oggetto dichiarando alcune proprietà come `private`. Questo genererà un errore se un codice esterno all'oggetto tenta di accedervi:

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

Nei linguaggi che non impongono l'accesso in questo modo, i programmatori usano convenzioni di denominazione, come iniziare il nome con un underscore, per indicare che la proprietà dovrebbe essere considerata privata.

## OOP e JavaScript

In questo articolo, abbiamo descritto alcune delle caratteristiche di base della programmazione orientata agli oggetti basata su classi come implementata nei linguaggi come Java e C++.

Negli articoli precedenti, abbiamo esaminato un paio di funzionalità di base di JavaScript: [costruttori](/it/docs/Learn_web_development/Core/Scripting/Object_basics) e [prototipi](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes). Queste funzionalità hanno certamente qualche relazione con alcuni dei concetti OOP descritti sopra.

- **costruttori** in JavaScript ci forniscono qualcosa di simile a una definizione di classe, permettendoci di definire la "forma" di un oggetto, inclusi eventuali metodi che contiene, in un unico posto. Ma anche i prototipi possono essere utilizzati qui. Ad esempio, se un metodo è definito sulla proprietà `prototype` di un costruttore, allora tutti gli oggetti creati utilizzando quel costruttore ottengono quel metodo tramite il loro prototipo, e non dobbiamo definirlo nel costruttore.

- **la catena dei prototipi** sembra un modo naturale per implementare l'ereditarietà. Ad esempio, se possiamo avere un oggetto `Student` il cui prototipo è `Person`, allora può ereditare `name` e sovrascrivere `introduceSelf()`.

Ma vale la pena comprendere le differenze tra queste funzionalità e i concetti di OOP "classici" descritti sopra. Evidenzieremo alcune di esse qui.

Innanzitutto, nell'OOP basato su classi, le classi e gli oggetti sono due costrutti separati, e gli oggetti sono sempre creati come istanze di classi. Inoltre, vi è una distinzione tra la funzionalità utilizzata per definire una classe (la sintassi della classe stessa) e la funzionalità utilizzata per istanziare un oggetto (un costruttore). In JavaScript, possiamo e spesso creiamo oggetti senza alcuna separata definizione di classe, utilizzando una funzione o un object literal. Questo può rendere il lavoro con gli oggetti molto più leggero di quanto avvenga nell'OOP classico.

In secondo luogo, sebbene una catena di prototipi sembri una gerarchia di ereditarietà e si comporti in alcuni modi simile a essa, è diversa in altri modi. Quando una sottoclasse è istanziata, viene creato un singolo oggetto che combina le proprietà definite nella sottoclasse con le proprietà definite più in alto nella gerarchia. Con il prototyping, ogni livello della gerarchia è rappresentato da un oggetto separato, e sono collegati insieme tramite la proprietà `__proto__`. Il comportamento della catena dei prototipi è meno simile all'ereditarietà e più simile alla **delegazione**. La delegazione è un pattern di programmazione in cui un oggetto, quando gli viene chiesto di eseguire un compito, può svolgere il compito stesso o chiedere a un altro oggetto (il suo **delegato**) di svolgere il compito per suo conto. In molti modi, la delegazione è un modo più flessibile di combinare oggetti rispetto all'ereditarietà (per una cosa, è possibile cambiare o sostituire completamente il delegato in fase di esecuzione).

Detto questo, i costruttori e i prototipi possono essere utilizzati per implementare pattern OOP basati su classi in JavaScript. Ma utilizzarli direttamente per implementare caratteristiche come l'ereditarietà è complicato, quindi JavaScript fornisce funzionalità extra, stratificate sul modello di prototipo, che mappano più direttamente ai concetti di OOP basato su classi. Queste funzionalità extra sono l'argomento del prossimo articolo.

## Sommario

Questo articolo ha descritto le caratteristiche di base della programmazione orientata agli oggetti basata su classi e ha brevemente esaminato come i costruttori e i prototipi di JavaScript si confrontano con questi concetti.

Nel prossimo articolo, esamineremo le funzionalità che JavaScript fornisce per supportare la programmazione orientata agli oggetti basata su classi.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
