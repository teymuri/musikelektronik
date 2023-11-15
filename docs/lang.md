# Einführung in die Sprache SuperCollider
## Namengebung

In der Programmierung (für uns Menschen) ist es allgemein einfacher, sich Namen anstelle von komplexen Daten zu merken. Daher können wir in unserem Code Namen für unsere Daten zuweisen. Es gibt drei Möglichkeiten Namen zu definieren:

### Einzellige Namen

Hierfür verwenden wir einzelne Kleinbuchstaben, wie folgt:

```supercollider
a = 123
```

Es gibt jedoch eine Ausnahme: "s" ist bereits von SuperCollider (SC) reserviert und verweist auf den aktuellen Server. Daher sollte "s" nicht überschrieben werden.

### Lokale Namen

Alternativ können wir vollständige Namen verwenden. Es ist wichtig, dass diese mit Kleinbuchstaben beginnen. Für diese Art von Namen müssen sie mit "var" deklariert und in einem Code-Block stehen. Außerhalb der geschweiften Klammern haben diese Namen keine Bedeutung. Code-Blöcke sind alle Teile, die sich zwischen zwei geschweiften Klammern befinden:

```supercollider
(
var name = 123;
name = name + 1;
name
)
```

### Globale Namen

Die letzte Art der Namenszuweisung erfolgt, indem wir vor den Namen eine Tilde setzen. Dadurch müssen diese Namen nicht in einem Code-Block eingeschlossen sein und können überall im Code verwendet werden. Beachten Sie, dass auch hier die Namen mit Kleinbuchstaben beginnen müssen:

```supercollider
~drei = 3;
(
~vier = 4;
)
~sieben = ~drei + ~vier;
```

Achtung: Wenn Sie mit Codeblöcken arbeiten, achten Sie darauf, dass das Ergebnis eines Codeblocks das ist, was in der letzten Zeile des Blocks steht!# Datenstrukturen

Und natürlich können wir nicht nur Zahlen Namen geben, sondern jeder Art Datenstruktur. In Supercollider gibt es verschiedene Klassen, die ein Blueprint für die Daten repräsentieren:

Es gibt z.B. Zahlenklassen Integer und Float. Beispiele für diese Klassen sind 1 (Integer) oder 3.14 (Float).

Die Klassen werden durch ihre Eigenschaften voneinander unterschieden. Wir können die Eigenschaften der Klassen in folgender Schreibweise abfragen:

`Klasse.Eigenschaft`

> **Achtung**
>
> Wenn es sich bei der Eigenschaft um eine Funktion handelt, ist diese Schreibweise:
> 
> ```
> Klasse.Funktion
> ```
> 
> äquivalent zu:  
> 
> ```
> Funktion(Klasse)
> ```
>
> Mehr zu Funktionen später...

Beispiel: Die Klasse Integer (und viele andere Objekte in SC) besitzt u.a. die Eigenschaft `class`, die angibt zu welcher Klasse eine Integer-Instanz gehört:

```
1.class
```

Platzieren Sie den Cursor auf der Zeile und drücken Sie die Tastenkombination **Shift+Enter**. Im Post-Window sehen Sie das Ergebnis `-> Integer`. Wir sagen, der Ausdruck `1.class` *evaluiert zu* `Integer`.

Auch der Quadratwurzel einer Zahl können wir durch die Eigenschaft `sqrt` (für square root) abfragen:

```
4.sqrt
```

Achten Sie darauf, dass die `sqrt`-Eigenschaft nur für Zahlen gilt! Wir können z.B. diese Eigenschaft bei einem String (eine Zeichenkette eingeschlossen zwischen zwei Anführungszeichen) nicht abfragen, da diese Eigenschaft dort keinen Sinn ergibt. SuperCollider wird uns in dem Fall einen Fehler ausgeben:

```
"Musik Elektronik".sqrt
```

Die `class`-Eigenschaft hingegen kann sinnvollerweise auch auf Strings (und jedes andere Objekt) angewendet werden:

```
"Musik Elektronik".class
```

Hier sind einige Klassen (auf der Sprach-Ebene), mit denen wir sehr viel arbeiten werden:

- Integer (`1`)
- Float (`3.14`)
- String (`"Musik Elektronik"`)
- Array (`[1, 2, 3, 4]`)
- Boolean (`true`)

**Übung**
Schreiben Sie für jede der folgenden Aufgaben eine Zeile Code:

> Summe der Zahlen 2.4, 45, 0.234

> Produkt der oben genannten Zahlen


**Übung**
Benutzen Sie die `size`-Eigenschaft, um herauszufinden, aus wie vielen Zeichen der folgende String besteht:
	```
	"1k2k3ja98d4nj281jkswe8s0dnmwer8c7q23ij18"
	```

**Übung**
Die `size`-Eigenschaft funktioniert auch für Arrays. Finden Sie die Anzahl der Objekte in dem folgenden Array heraus:
	```
	[1, 2, 3, 4, 5, 6, 7, 8, 7, 6, 5, 4, 3, 2, 3, 4, 5, 6, 5, 4, 3, 2]
	```

**Übung**
Benutzen Sie die `sum`-Eigenschaft für Arrays, um die Summe aller Zahlen im obigen Array zu berechnen.

## Funktionen
Wie bereits oben beschrieben, können wir eine Funktion in der folgenden Schreibweise aufrufen:

```
Objekt.Funktion_des_Objekts
```
oder
```
Funktione_des_objekts(Objekt)
```

Beispiele hierfür sind:

```
// Summe eines Arrays berechnen:
[1, 2, 3, 4].sum;
// Äquivalent zu:
sum([1, 2, 3, 4]);

// Oder den Kehrwert einer Zahl:
4.reciprocal;
// Gleichbedeutend mit:
reciprocal(4);
```

Aber manchmal benötigt eine Funktion mehr als eine Eingabe. Ein Beispiel ist das Berechnen einer Potenz, für das wir eine Basis und einen Exponenten benötigen. In SuperCollider lässt sich "zwei hoch drei" sowohl mit der Punkt-Schreibweise ausdrücken:

```
2.pow(3)
```

als auch in Funktions-Schreibweise:

```
pow(2, 3)
```

Ein weiteres Beispiel ist die Funktion `linlin`, bei der eine Zahl aus einem linearen Zahlenraum auf einen anderen linearen Zahlenraum abgebildet wird. In unserem Beispiel wird die Zahl 0.5 aus dem Bereich von 0 bis 1 auf den Bereich von 10 bis 20 abgebildet, wobei dies der Zahl 15 entspricht.

```
0.5.linlin(0, 1, 10, 20)
```

Wir können auch unsere eigenen Funktionen definieren. Unten habe ich alle Zeilen innerhalb eines Code-Blocks (runden Klammern) platziert, um sie alle zusammen evaluieren zu können. Ich verwende auch einen globalen Namen (mit einer Tilde), da ich von überall auf meine Funktion zugreifen möchte. Die Funktion erhält zwei Zahlen als Argumente und evaluiert zu ihrer Summe der Quadratzahlen.

Ich definiere hier meine Funktion und gebe ihr den Namen "quadsum".
Der Funktionskörper wird zwischen geschweiften Klammern definiert.
Der Wert, zu dem die Funktion ausgewertet wird, ist der letzte Ausdruck im Funktionskörper.
```
(
~quadsum = {}
)
```
Hier definiere ich zuerst, wie viele Eingaben meine Funktion akzeptieren soll,
und gebe ihnen eindeutige Namen:
```
(
~quadsum = {
    arg num1, num2;
}
)
```

Ab hier beginnt die eigentliche Funktionsdefinition. Zuerst speichere ich
die Quadrate der beiden Argumente "num1" und "num2" in internen Variablen.
Zur Berechnung der Quadrate der Zahlen verwende ich die eingebaute Funktion "squared".
```
(
~quadsum = {
    arg num1, num2;
	var quad1 = num1.squared;
	var quad2 = num2.squared;
}
)
```
Und der letzte Ausdruck ist der eigentliche Wert, den die Funktion zurückgibt:
```
(
~quadsum = {
    arg num1, num2;
	var quad1 = num1.squared;
	var quad2 = num2.squared;
    quad1 + quad2;
}
)
```

Jetzt, mit dem Cursor innerhalb des Blocks (zwischen den beiden runden Klammern), drücken Sie die Tastenkombination **Ctrl+Enter**. Der Block (und der darin enthaltene Code) wird ausgewertet, und wir sehen im Post-Window "-> a Function".

Jetzt können wir unsere Funktion aufrufen:

```supercollider
~quadsum.(2, 3); // -> 13
~quadsum.(4, 5); // -> 41
```

Bitte beachten Sie, dass für selbstdefinierte Funktionen die Schreibweise mit dem Punkt nicht existiert. Um unsere eigene Funktion aufzurufen, verwenden wir die folgende Syntax:

```supercollider
Funktion.(arg1, arg2, ...)
```

Falls unsere Funktion keine Argumente bekommt, bleibt die Klammer natürlich leer!

```supercollider
(
// Zufällige Zahl zwischen 1 und 10
~rand_1_to_10 = {
	1.rrand(10)
}
)

// Aufruf wie folgt:
~rand_1_to_10.()
```

**Übung**
Schreiben Sie eine Funktion, die zwei Argumente erhält. Das erste Argument ist ein Array, das eine beliebige Anzahl von Zahlen enthält, und das zweite Argument ist die Index-Nummer eines der Elemente in diesem Array. Das Ergebnis der Funktion sollte ein neues Array sein, in dem jedes Element des Eingabe-Arrays mit der Zahl an der Index-Nummer skaliert ist.

Tipp 1:
Sie können ein Array von Zahlen skalieren, indem Sie das gesamte Array mit einer Zahl multiplizieren. Zum Beispiel:

```supercollider
[1, 2, 3, 4] * 10 // -> [10, 20, 30, 40]
```

Tipp 2:
Um ein Element an einer bestimmten Index-Nummer aus einem Array zu nehmen, verwenden Sie die `at`-Methode für Arrays. Diese Methode wird wie folgt aufgerufen (Beachten Sie, dass Index-Nummern 0-basiert sind):

```supercollider
[1, 2, 3, 4].at(2) // -> 3
```


**Übung: Mittlere Zahl finden**
Schreiben Sie eine Funktion, die drei Zahlen als Argumente erhält und die mittlere Zahl zurückgibt. Nennen Sie die
Funktion `~findMiddle`.

Tipp 1: Die `sort` Methode eines Arrays sortiert das Array *in-place*.

Tipp 2: Die `at` Methode erlaubt das Abrufen eines Elements an einer bestimmten Indexposition in einem Array.



**Übung: Berechne den Durchschnitt**
Schreiben Sie eine Funktion namens `~average`, die eine Liste von Zahlen als Argument erhält und den Durchschnitt dieser Zahlen berechnet und zurückgibt.

Tipp: Folgende Methoden können hilfreich sein: `size` und `sum`.




**Übung: Umdrehen einer Zeichenkette**
Schreiben Sie eine Funktion `~reverseString`, die eine Zeichenkette als Argument erhält und die umgekehrte Zeichenkette zurückgibt.

Tipp: Drücken Sie die Tastenkombination Shift+Ctrl+D, um die Dokumentationsseite zu durchsuchen. Suchen Sie nach "reverse". Dort werden alle Klassen aufgelistet, die diese Funktion/Eigenschaft implementieren. Klicken Sie auf den Eintrag "ArrayedCollection". Sie können die Methode `reverse` in Ihrer Funktion verwenden.

**Übung: Flächeninhalt eines Rechtecks berechnen**
Schreiben Sie eine Funktion `~calculateRectangleArea`, die die Länge und Breite eines Rechtecks als Argumente erhält und den Flächeninhalt des Rechtecks zurückgibt.


**Übung: Volumen einer Kugel berechnen**
Schreiben Sie eine Funktion `~calculateSphereVolume`, die den Radius einer Kugel als Argument erhält und das Volumen der Kugel berechnet und zurückgibt.

Tipp 1: Die Formel zur Berechnung des Volumens einer Kugel lautet:

*V = 4/3 x π x r^3*

Tipp 2: Die Zahl Pi können Sie einfach als `pi` schreiben.




**Übung: Tonleiter generieren**
Definieren Sie zwei globale Variablen `~majorPattern` und `~minorPattern`. Definieren Sie eine Funktion `~generateScale`, die drei Argumente erhält: eine MIDI-Keynummer, ein Skalenmuster und eine Oktavnummer. Die Funktion soll die gewünschte Tonleiter (angegeben als `scalePattern`, entweder `~majorPattern` oder `~minorPattern`) generieren, beginnend bei der MIDI-Keynummer und transponiert um die angegebene Anzahl von Oktaven nach oben oder nach unten.


## Iterieren mit `do` und `collect`
In SuperCollider ist das Iterieren über Listen und Arrays und das Sammeln von Ergebnissen eine wichtige Technik, um komplexe Operationen auf Daten auszuführen. Die Methoden `do` und `collect` sind dabei sehr hilfreich. 

### Das `do`-Muster
Die Methode `do` wird verwendet, um über die Elemente einer Liste oder Array zu iterieren und eine Aktion auf jedes Element auszuführen. Zum Beispiel:

```supercollider
// Iteriert über die Array und zeigt jedes Element im Post-Window
[1, 2, 3, 4, 5].do { 
    arg element;
    element.postln;
}
```

Dieser Code gibt nacheinander die Zahlen 1 bis 5 aus.

### Das `collect`-Muster
Die Methode `collect` wird verwendet, um eine neue Liste/Array zu erstellen, indem eine Aktion auf jedes Element der ursprünglichen Liste angewendet wird. Zum Beispiel:

```supercollider
// Erstellt eine neue Array/Liste, in der jedes Element um 10 erhöht ist
var originalList = [1, 2, 3, 4, 5];
var newList = originalList.collect { 
   arg element;
   element + 10;
};
newList.postln; // -> [ 11, 12, 13, 14, 15 ]
```


Beachten Sie, dass `collect` eine neue Liste erstellt, und Sie können sie einer Variablen zuweisen, um das Ergebnis zu speichern.
Sie können viele andere Operationen mit `do` und `collect` durchführen, z. B. Filtern von Elementen, Berechnen von Durchschnittswerten, Suchen nach bestimmten Werten usw.

**Übung: Quadratzahlen erzeugen**
Schreiben Sie eine Funktion namens `~squareNumbers`, die eine Liste von Zahlen als Argument erhält und eine Liste zurückgibt, in der die Quadrate der ursprünglichen Zahlen stehen. Benutzen sie `collect`.


**Übung: Fibonacci-Folge generieren**
Schreiben Sie eine Funktion `~generateFibonacci`, die eine positive Ganzzahl n als Argument erhält und die ersten n Zahlen in der Fibonacci-Folge zurückgibt.


## Kontrollstrukturen

Manchmal möchten wir Teile unseres Codes nur ausführen, wenn eine bestimmte Bedingung erfüllt ist. Das erreichen wir mithilfe der sogenannten [Kontrollstrukturen](https://de.wikipedia.org/wiki/Kontrollstruktur). 
### `if`
Eines dieser Mittel ist das `if`-Statement, das die folgende Form hat:

```
(
if
  (Bedingung ist erfüllt),
  {Dann evaluiere diese Funktion},
  {Ansonsten evaluiere diese Funktion}
) 
```

Wir schauen uns ein Beispiel an: Wir testen den Wert einer Zufallszahl und möchten eine Zeile im Post-Fenster ausgeben lassen. Diese Zeile soll die Nachricht "Größer als Zehn" ausgeben, wenn unsere Zahl größer als 10 ist, und "Kleiner als Zehn" ausgeben, wenn die Zahl kleiner als 10 ist.

```
(
// Eine Zufallszahl generieren
var zahl = 0.rrand(20);
if (
	// Den Wert der Zufallszahl auswerten
	(zahl > 10),
	// Wenn die Zahl größer ist als 10, dann rufen wir diese Funktion auf
	{zahl.postln; "Die Zahl ist größer als 10"},
	// Ansonsten rufen wir diese Funktion auf
	{zahl.postln; "Die Zahl ist kleiner als 10"}
)
)
```

**Übung:** 
Schreiben Sie eine Funktion namens `randomColor`, die 50% der Zeit den String "Rot" und 50% der Zeit den String "Gelb" ausgibt.

Tipp: Nutzen Sie die eingebaute Funktion `coin`. Diese Funktion erhält eine Zahl zwischen 0 und 1 als Argument und gibt mit der angegebenen Wahrscheinlichkeit die booleschen Werte `true` oder `false` aus. Zum Beispiel wird `1.coin` immer `true` ausgeben, während `0.coin` immer zu `false` evaluiert wird.

**Übung: Überprüfung der Geradheit einer Zahl**
Schreiben Sie eine Funktion, die überprüft, ob eine gegebene Zahl gerade oder ungerade ist, und eine entsprechende Nachricht ausgibt.

Tipp: Die `mod`-Funktion kann nützlich sein. Lesen Sie dafür die entsprechende Dokumentationsseite.

**Übung: Palindrom-Überprüfung**
Schreiben Sie eine Funktion, die überprüft, ob ein gegebener String ein Palindrom ist, d.h., ob er von vorne und von hinten gleich gelesen wird.

**Übung: Schachbrett-Muster**
Schreiben Sie eine Funktion, die ein Schachbrett-Muster mit den Zeichen "#" und " " erstellt. Die Größe des Schachbretts soll als Argument übergeben werden.

Tipp: Mit folgendem Ausdruck erhalten Sie ein zweidimensionales Array, dessen Unter-Arrays mit Zahlen zwischen 0 und 7 (einschließlich) gefüllt sind und sich rotierend von der nächsten Zahl fortsetzen:

```
Array.fill(8, {arg x; Array.iota(8).rotate(x * -1)})
```
Zur Veranschaulichung evaluieren Sie den folgenden Block, der dieses zweidimensionale Array im Post-Window ausgibt:
```
(
Array.fill(8, {arg x; Array.iota(8).rotate(x * -1)}).do {
	arg row;
	row.do {
		arg col;
		"% ".postf(col)
	};
	"".postln
}
)
```

```
// Ergebnis im Post-Window:

0 1 2 3 4 5 6 7
1 2 3 4 5 6 7 0
2 3 4 5 6 7 0 1
3 4 5 6 7 0 1 2
4 5 6 7 0 1 2 3
5 6 7 0 1 2 3 4
6 7 0 1 2 3 4 5
7 0 1 2 3 4 5 6
```

### `case`
Wenn wir mehr als eine Bedingung testen möchten, um in jeder Situation anders vorzugehen, können wir die `case`-Konstruktion nutzen. Sie hat die folgende Form:

```supercollider
(
case
  // Kondition und Funktion 1
  {Kondition 1 ist erfüllt} {evaluiere die Funktion 1}
  // Kondition und Funktion 2
  {Kondition 2 ist erfüllt} {evaluiere die Funktion 2}
  // Kondition und Funktion 3
  {Kondition 3 ist erfüllt} {evaluiere die Funktion 3}
  // Diese Zeile wird nur evaluiert, wenn keine der Bedingungen oben erfüllt sind
  {true} {sonst evaluiere diese Funktion}
)
```

Ein Beispiel:
```
(
// Testen, ob eine Zufallszahl zwischen 10 und 100 durch 7, 5 oder 3 teilbar ist
var n = rrand(10, 100);
case 
    { n.mod(7) == 0 } { "% ist durch 7 teilbar".postf(n) }
    { n.mod(5) == 0 } { "% ist durch 5 teilbar".postf(n) }
    { n.mod(3) == 0 } { "% ist durch 3 teilbar".postf(n) }
    { true } { "% ist durch 7, 5 und 3 nicht teilbar".postf(n) }
)
```

**Übung: Tonart-Erkennung** Schreiben Sie mithilfe von `case` eine Funktion, die als Argument eine Sequenz von MIDI-Notennummern erhält. Das Programm soll diese Sequenz intervalle analysieren. Wenn diese Sequenz einer Dur- oder Moll-Tonleiter (*natürliches*,  *harmonisches* oder *melodisches* Moll, alle in aufsteigender Form) entspricht, soll die Funktion den Namen der Tonleiter als einen String zurückgeben. Andernfalls soll der String "Keine erkennbare Tonleiter" zurückgegeben werden.

Tipp: Folgende Definitionen können Ihnen bei der Aufgabe nützlich sein:
```
(
// Die Intervallmuster der Dur-Tonleiter und drei Varianten der Moll-Tonleiter
~majorIntervals = [ 2, 2, 1, 2, 2, 2, 1 ];
~nat_minorIntervals = [ 2, 1, 2, 2, 1, 2, 2 ];
~harm_minorIntervals = [ 2, 1, 2, 2, 1, 3, 1 ];
~mel_minorIntervals = [ 2, 1, 2, 2, 2, 2, 1 ];

// Bekommt ein Array von Zahlen (arg nums) und gibt ein Array von
// Intervallen zwischen zwei benachbarten Zahlen aus
~getIntervals = {
	arg nums;
	var intervals = Array.new(nums.size - 1);
	nums.doAdjacentPairs({arg a, b; intervals.add(b - a)});
	intervals
}
)
```
Tipp: Die Funktion `midiname` erhält eine Zahl und gibt den Namen der Note mit der MIDI-Nummer aus.

Tipp: String-Konkatenation können wir mit dem `++`-Operator erreichen:

```supercollider
"Super" ++ "Collider" // -> "SuperCollider"
```

Für weitere Informationen zu Kontrollstrukturen suchen Sie in der Dokumentation nach "Control Structures"
(**Ctrl+Shift+D** und nach dem Begriff suchen).
