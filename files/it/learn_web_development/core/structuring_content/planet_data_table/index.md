---
title: "Sfida: strutturare una tabella di dati sui pianeti"
short-title: "Sfida: tabella di dati sui pianeti"
slug: Learn_web_development/Core/Structuring_content/Planet_data_table
l10n:
  sourceCommit: ee677b2c4d4a226fe4aedf05b2b156cae8a2bb95
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content")}}

In questa sfida, vengono forniti dati sui pianeti del sistema solare. Il compito consiste nello strutturarli in una tabella HTML accessibile.

## Punto di partenza

1. Creare una nuova cartella in una posizione appropriata sul computer denominata `planet-data-table` (oppure aprire un editor online e seguire i passaggi necessari per creare un nuovo progetto).
2. Salvare il seguente elenco HTML in un file nella cartella denominato `index.html` (oppure incollarlo nel pannello HTML dell'editor online).

   ```html
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />
       <title>Planet data table</title>
       <link href="style.css" rel="stylesheet" type="text/css" />
     </head>
     <body>
       <h1>Planet data table</h1>
     </body>
   </html>
   ```

3. Salvare il seguente elenco CSS in un file nella cartella denominato `style.css` (oppure incollarlo nel pannello CSS dell'editor online).

   ```css live-sample___planet-data-table
   html {
     font-family: sans-serif;
   }

   table {
     border-collapse: collapse;
     border: 2px solid rgb(200 200 200);
     letter-spacing: 1px;
     font-size: 0.8rem;
   }

   td,
   th {
     border: 1px solid rgb(190 190 190);
     padding: 10px 20px;
   }

   th {
     background-color: rgb(235 235 235);
   }

   td {
     text-align: center;
   }

   tr:nth-child(even) td {
     background-color: rgb(250 250 250);
   }

   tr:nth-child(odd) td {
     background-color: rgb(245 245 245);
   }

   caption {
     padding: 10px;
   }

   .column-border {
     border: 2px solid black;
   }
   ```

4. Tenere a portata di mano i seguenti dati; saranno necessari per trasformarli in una tabella HTML di dati all'interno del codice HTML.

   ```plain
   Rows

   Terrestrial planets

   Mercury 0.330 4,879 5427 3.7 4222.6 57.9 167 0 Closest to the Sun
   Venus 4.87 12,104 5243 8.9 2802.0 108.2 464 0
   Earth 5.97 12,756 5514 9.8 24.0 149.6 15 1 Our world
   Mars 0.642 6,792 3933 3.7 24.7 227.9 -65 2 The red planet

   Jovian planets

   Gas giants

   Jupiter 1898 142,984 1326 23.1 9.9 778.6 -110 67 The largest planet
   Saturn 568 120,536 687 9.0 10.7 1433.5 -140 62

   Ice giants

   Uranus 86.8 51,118 1271 8.7 17.2 2872.5 -195 27
   Neptune 102 49,528 1638 11.0 16.1 4495.1 -200 14

   Dwarf planets*

   Pluto 0.0146 2,370 2095 0.7 153.3 5906.4 -225 5 Declassified as a planet in 2006, but this <a href="http://www.usatoday.com/story/tech/2014/10/02/pluto-planet-solar-system/16578959/">remains controversial</a>.

   Columns

   Name
   Mass (10<sup>24</sup>kg)
   Diameter (km)
   Density (kg/m<sup>3</sup>)
   Gravity (m/s<sup>2</sup>)
   Length of day (hours)
   Distance from Sun (10<sup>6</sup>km)
   Mean temperature (°C)
   Number of moons
   Notes

   Caption

   Data about the planets of our solar system (Planetary facts taken from <a href="http://nssdc.gsfc.nasa.gov/planetary/factsheet/">Nasa's Planetary Fact Sheet - Metric</a>).
   ```

## Descrizione del progetto

Si sta lavorando in una scuola; gli studenti stanno attualmente studiando i pianeti del sistema solare e si desidera fornire loro un insieme di dati facile da consultare, per cercare fatti e cifre sui pianeti. Una tabella HTML di dati sarebbe ideale: è necessario prendere i dati grezzi disponibili e trasformarli in una tabella, seguendo i passaggi indicati di seguito.

Tutti i dati necessari sono contenuti nell'elenco di dati fornito sopra. Se risulta difficile visualizzare i dati, consultare l'esempio dal vivo seguente oppure provare a disegnare un diagramma.

1. Iniziare la tabella fornendole un contenitore esterno, un'intestazione della tabella e un corpo della tabella. Per questo esempio non è necessario un piè di pagina della tabella.
2. Aggiungere alla tabella la didascalia fornita.
3. Aggiungere una riga all'intestazione della tabella contenente tutte le intestazioni di colonna.
4. Creare tutte le righe di contenuto all'interno del corpo della tabella, ricordando di rendere semanticamente intestazioni tutte le intestazioni di riga.
5. Assicurarsi che tutti i contenuti siano inseriti nelle celle corrette: nei dati grezzi, ogni riga di dati relativi a un pianeta è mostrata accanto al pianeta corrispondente.
6. Aggiungere attributi per associare in modo non ambiguo le intestazioni di riga e di colonna alle righe, alle colonne o ai gruppi di righe per cui fungono da intestazioni.
7. Aggiungere un [bordo](/it/docs/Web/CSS/Reference/Properties/border) nero soltanto attorno alla colonna che contiene tutte le intestazioni di riga con i nomi dei pianeti. A questo scopo, utilizzare una struttura `<colgroup>`/`<col>` appropriata e lo stile della classe `.column-border` fornito nel CSS.

## Suggerimenti

- La prima cella della riga di intestazione deve essere vuota e occupare due colonne.
- Le intestazioni delle righe di gruppo (ad esempio, _Pianeti gioviani_) situate a sinistra delle intestazioni di riga con i nomi dei pianeti (ad esempio, _Saturno_) sono un po' difficili da organizzare: è necessario assicurarsi che ognuna occupi il numero corretto di righe e colonne.
- Un metodo per associare le intestazioni alle rispettive righe/colonne è molto più semplice dell'altro.

## Esempio

Dopo aver aggiunto il markup corretto, la tabella dovrebbe apparire come segue. In caso di difficoltà, consultare la soluzione sotto l'esempio dal vivo.

{{embedlivesample("planet-data-table", "100%", 650)}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il codice HTML finale dovrebbe avere questo aspetto:

```html live-sample___planet-data-table
<h1>Planet data table</h1>

<table>
  <caption>
    Data about the planets of our solar system (Planetary facts taken from
    <a href="https://nssdc.gsfc.nasa.gov/planetary/factsheet/"
      >Nasa's Planetary Fact Sheet - Metric</a
    >).
  </caption>
  <colgroup>
    <col span="2" />
    <col class="column-border" />
    <col span="9" />
  </colgroup>
  <thead>
    <tr>
      <td colspan="2"></td>
      <th scope="col">Name</th>
      <th scope="col">Mass (10<sup>24</sup>kg)</th>
      <th scope="col">Diameter (km)</th>
      <th scope="col">Density (kg/m<sup>3</sup>)</th>
      <th scope="col">Gravity (m/s<sup>2</sup>)</th>
      <th scope="col">Length of day (hours)</th>
      <th scope="col">Distance from Sun (10<sup>6</sup>km)</th>
      <th scope="col">Mean temperature (°C)</th>
      <th scope="col">Number of moons</th>
      <th scope="col">Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" colspan="2" scope="rowgroup">Terrestrial planets</th>
      <th scope="row">Mercury</th>
      <td>0.330</td>
      <td>4,879</td>
      <td>5427</td>
      <td>3.7</td>
      <td>4222.6</td>
      <td>57.9</td>
      <td>167</td>
      <td>0</td>
      <td>Closest to the Sun</td>
    </tr>
    <tr>
      <th scope="row">Venus</th>
      <td>4.87</td>
      <td>12,104</td>
      <td>5243</td>
      <td>8.9</td>
      <td>2802.0</td>
      <td>108.2</td>
      <td>464</td>
      <td>0</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">Earth</th>
      <td>5.97</td>
      <td>12,756</td>
      <td>5514</td>
      <td>9.8</td>
      <td>24.0</td>
      <td>149.6</td>
      <td>15</td>
      <td>1</td>
      <td>Our world</td>
    </tr>
    <tr>
      <th scope="row">Mars</th>
      <td>0.642</td>
      <td>6,792</td>
      <td>3933</td>
      <td>3.7</td>
      <td>24.7</td>
      <td>227.9</td>
      <td>-65</td>
      <td>2</td>
      <td>The red planet</td>
    </tr>
    <tr>
      <th rowspan="4" scope="rowgroup">Jovian planets</th>
      <th rowspan="2" scope="rowgroup">Gas giants</th>
      <th scope="row">Jupiter</th>
      <td>1898</td>
      <td>142,984</td>
      <td>1326</td>
      <td>23.1</td>
      <td>9.9</td>
      <td>778.6</td>
      <td>-110</td>
      <td>67</td>
      <td>The largest planet</td>
    </tr>
    <tr>
      <th scope="row">Saturn</th>
      <td>568</td>
      <td>120,536</td>
      <td>687</td>
      <td>9.0</td>
      <td>10.7</td>
      <td>1433.5</td>
      <td>-140</td>
      <td>62</td>
      <td></td>
    </tr>
    <tr>
      <th rowspan="2" scope="rowgroup">Ice giants</th>
      <th scope="row">Uranus</th>
      <td>86.8</td>
      <td>51,118</td>
      <td>1271</td>
      <td>8.7</td>
      <td>17.2</td>
      <td>2872.5</td>
      <td>-195</td>
      <td>27</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">Neptune</th>
      <td>102</td>
      <td>49,528</td>
      <td>1638</td>
      <td>11.0</td>
      <td>16.1</td>
      <td>4495.1</td>
      <td>-200</td>
      <td>14</td>
      <td></td>
    </tr>
    <tr>
      <th colspan="2" scope="rowgroup">Dwarf planets</th>
      <th scope="row">Pluto</th>
      <td>0.0146</td>
      <td>2,370</td>
      <td>2095</td>
      <td>0.7</td>
      <td>153.3</td>
      <td>5906.4</td>
      <td>-225</td>
      <td>5</td>
      <td>
        Declassified as a planet in 2006, but this
        <a
          href="https://www.usatoday.com/story/tech/2014/10/02/pluto-planet-solar-system/16578959/"
          >remains controversial</a
        >.
      </td>
    </tr>
  </tbody>
</table>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content")}}
