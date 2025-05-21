---
title: Le classi in JavaScript
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

Nell'ultimo articolo, abbiamo introdotto alcuni concetti di base della programmazione orientata agli oggetti (OOP) e discusso un esempio in cui abbiamo utilizzato i principi OOP per modellare professori e studenti in una scuola.

Abbiamo anche parlato di come sia possibile usare [prototipi](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes) e [costruttori](/it/docs/Learn_web_development/Core/Scripting/Object_basics#introducing_constructors) per implementare un modello di questo tipo, e che JavaScript fornisce anche funzionalità che si avvicinano maggiormente ai concetti classici della OOP.

In questo articolo, esamineremo queste funzionalità. Vale la pena tenere a mente che le funzionalità descritte qui non sono un nuovo modo di combinare oggetti: sotto il cofano, utilizzano ancora i prototipi. Sono solo un modo per facilitare la configurazione di una catena di prototipi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (in particolare
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Le basi degli oggetti</a>) e i concetti di JavaScript orientato agli oggetti trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Creazione di classi in JavaScript.</li>
          <li>Creazione di costruttori in JavaScript.</li>
          <li>Ereditarietà e incapsulamento in JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Classi e costruttori

È possibile dichiarare una classe utilizzando la parola chiave {{jsxref("Statements/class", "class")}}. Ecco una dichiarazione di classe per il nostro `Person` dall'articolo precedente:

```js
class Person {
  name;

  constructor(name) {
    this.name = name;
  }

  introduceSelf() {
    console.log(`Hi! I'm ${this.name}`);
  }
}
```

Questo dichiara una classe chiamata `Person`, con:

- una proprietà `name`.
- un costruttore che prende un parametro `name` che viene utilizzato per inizializzare la proprietà `name` del nuovo oggetto
- un metodo `introduceSelf()` che può fare riferimento alle proprietà dell'oggetto utilizzando `this`.

La dichiarazione `name;` è opzionale: si potrebbe ometterla, e la linea `this.name = name;` nel costruttore creerà la proprietà `name` prima di inizializzarla. Tuttavia, elencare le proprietà esplicitamente nella dichiarazione della classe potrebbe facilitare a chi legge il tuo codice vedere quali proprietà fanno parte di questa classe.

È inoltre possibile inizializzare la proprietà a un valore predefinito quando la si dichiara, con una riga come `name = '';`.

Il costruttore è definito utilizzando la parola chiave {{jsxref("Classes/constructor", "constructor")}}. Proprio come un [costruttore al di fuori di una definizione di classe](/it/docs/Learn_web_development/Core/Scripting/Object_basics#introducing_constructors), esso:

- crea un nuovo oggetto
- associa `this` al nuovo oggetto, così da poter riferirsi a `this` nel codice del costruttore
- esegue il codice nel costruttore
- restituisce il nuovo oggetto.

Data la dichiarazione di classe sopra, si può creare e utilizzare una nuova istanza di `Person` in questo modo:

```js
const giles = new Person("Giles");

giles.introduceSelf(); // Hi! I'm Giles
```

Si noti che chiamiamo il costruttore utilizzando il nome della classe, `Person` in questo esempio.

### Ommettere i costruttori

Se non è necessario eseguire alcuna specializzazione dell'inizializzazione, è possibile omettere il costruttore, e un costruttore predefinito verrà generato automaticamente:

```js
class Animal {
  sleep() {
    console.log("zzzzzzz");
  }
}

const spot = new Animal();

spot.sleep(); // 'zzzzzzz'
```

## Ereditarietà

Data la nostra classe `Person` sopra, definiamo la sottoclasse `Professor`.

```js
class Professor extends Person {
  teaches;

  constructor(name, teaches) {
    super(name);
    this.teaches = teaches;
  }

  introduceSelf() {
    console.log(
      `My name is ${this.name}, and I will be your ${this.teaches} professor.`,
    );
  }

  grade(paper) {
    const grade = Math.floor(Math.random() * (5 - 1) + 1);
    console.log(grade);
  }
}
```

Utilizziamo la parola chiave {{jsxref("Classes/extends", "extends")}} per dire che questa classe eredita da un'altra classe.

La classe `Professor` aggiunge una nuova proprietà `teaches`, quindi la dichiariamo.

Poiché vogliamo impostare `teaches` quando viene creato un nuovo `Professor`, definiamo un costruttore, che prende il `name` e `teaches` come argomenti. La prima cosa che questo costruttore fa è chiamare il costruttore della superclasse utilizzando {{jsxref("Operators/super", "super()")}}, passando il parametro `name`. Il costruttore della superclasse si occupa di impostare `name`. Dopodiché, il costruttore di `Professor` imposta la proprietà `teaches`.

> [!NOTE]
> Se una sottoclasse deve effettuare una propria inizializzazione, deve **prima** chiamare il costruttore della superclasse utilizzando `super()`, passando tutti i parametri che il costruttore della superclasse si aspetta.

Abbiamo anche sovrascritto il metodo `introduceSelf()` della superclasse, e aggiunto un nuovo metodo `grade()`, per valutare un compito (il nostro professore non è molto bravo, e assegna solo voti casuali agli elaborati).

Con questa dichiarazione possiamo ora creare e usare professori:

```js
const walsh = new Professor("Walsh", "Psychology");
walsh.introduceSelf(); // 'My name is Walsh, and I will be your Psychology professor'

walsh.grade("my paper"); // some random grade
```

## Incapsulamento

Infine, vediamo come implementare l'incapsulamento in JavaScript. Nell'ultimo articolo abbiamo discusso di come ci piacerebbe rendere privata la proprietà `year` di `Student`, in modo da poter cambiare le regole sulle classi di tiro con l'arco senza rompere alcun codice che utilizza la classe `Student`.

Ecco una dichiarazione della classe `Student` che fa proprio questo:

```js
class Student extends Person {
  #year;

  constructor(name, year) {
    super(name);
    this.#year = year;
  }

  introduceSelf() {
    console.log(`Hi! I'm ${this.name}, and I'm in year ${this.#year}.`);
  }

  canStudyArchery() {
    return this.#year > 1;
  }
}
```

In questa dichiarazione di classe, `#year` è una [proprietà dati privata](/it/docs/Web/JavaScript/Reference/Classes/Private_properties). Possiamo costruire un oggetto `Student`, e può usare `#year` internamente, ma se il codice al di fuori dell'oggetto tenta di accedere a `#year` il browser genera un errore:

```js
const summers = new Student("Summers", 2);

summers.introduceSelf(); // Hi! I'm Summers, and I'm in year 2.
summers.canStudyArchery(); // true

summers.#year; // SyntaxError
```

> [!NOTE]
> Il codice eseguito nella console di Chrome può accedere a proprietà private al di fuori della classe. Questa è una deroga DevTools solo alla restrizione della sintassi JavaScript.

Le proprietà dati private devono essere dichiarate nella dichiarazione di classe, e i loro nomi iniziano con `#`.

### Metodi privati

Puoi avere metodi privati oltre alle proprietà dati private. Proprio come le proprietà dati private, i loro nomi iniziano con `#`, e possono essere chiamati solo dai metodi dell'oggetto stesso:

```js
class Example {
  somePublicMethod() {
    this.#somePrivateMethod();
  }

  #somePrivateMethod() {
    console.log("You called me?");
  }
}

const myExample = new Example();

myExample.somePublicMethod(); // 'You called me?'

myExample.#somePrivateMethod(); // SyntaxError
```

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma puoi ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: JavaScript orientato agli oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript).

## Sommario

In questo articolo, abbiamo esaminato gli strumenti principali disponibili in JavaScript per scrivere programmi orientati agli oggetti. Non abbiamo coperto tutto qui, ma questo dovrebbe essere sufficiente per iniziare. Il nostro [articolo sulle classi](/it/docs/Web/JavaScript/Reference/Classes) è un buon punto di partenza per saperne di più.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
