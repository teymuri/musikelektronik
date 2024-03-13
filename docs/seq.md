
# Sequenzierung

## Routine
Das Abspielen mehrerer Synth-Instanzen mit einfachen Iterationsmöglichkeiten wie `do` und `collect` führt zwangsläufig zu Akkorden, da wir mit `do`/`collect`-Konstruktionen keine Möglichkeit zum sequenziellen Abspielen der Synth-Instanzen haben (diese werden praktisch gleichzeitig ausgeführt). Mit der Klasse `Routine` können wir dies erreichen: Eine Routine ist im Grunde genommen eine Funktion, deren Ausführung wir mitten in der Aufführung für eine beliebige Zeit pausieren können.

Um eine Routine zu schreiben, müssen wir lediglich den Iterationscode innerhalb der Routine einsetzen und an der passenden Stelle bestimmen, wie lange unsere Routine nach jeder Iteration warten soll, bevor die nächste Iteration einsetzt. Dies geschieht mit Hilfe der `wait`-Methode. Am Ende müssen wir unsere fertige Routine mithilfe der `play`-Methode starten. Eine fertige Routine sieht folgendermaßen aus; hier spielen wir die ersten 20 Partialtöne der Grundfrequenz 110 Hz in einer Sequenz, wobei wir nach jedem Ton 150 Millisekunden warten (`0.15.wait`):

```
(
Routine {
	(Array.series(20, 1) * 110).do {
		arg freq;
		freq.postln;
		Synth(\meinSynth, [freq: freq, dur: 0.5]);
		0.15.wait
	}
}.play
)
```

**Übung** Verändern Sie die Parameter der obigen Routine, wie zum Beispiel das Array von Frequenzen, über das wir iterieren wollen. Passen Sie die Wartezeit der Routine an, sodass diese einen zufälligen Wert zwischen 100 Millisekunden und 500 Millisekunden annimmt (Stichwort `rrand`).

In der Tat kann man mit der `next`-Methode den nächsten Wert einer Routine abrufen. Dies ermöglicht uns, auch unendliche Sequenzen nach bestimmten Mustern zu generieren. Im folgenden Beispiel definieren wir unsere Funktion aus dem Kapitel (?) zur Generierung der Fibonacci-Zahlen. Hierbei gestalten wir die Funktion jedoch so, dass wir mit einer `inf.do` eine unendliche Iteration starten (`inf` steht für Infinite und ist ein von SuperCollider definierter Begriff). Die unendliche Iteration wird jedoch hier durch `yield` unterbrochen. `yield` funktioniert synonym zu `wait` und gibt einen Zwischenwert der Funktion zurück:

```
(
// Die Funktion zum Generieren der Fibonacci Sequenz
~fibGen = {
	var a=0, b=1;
	var tmpA = a;
	inf.do {
		a.yield;
		a = b;
		b = tmpA + b;
		tmpA = a;
	}
}
);

~fibs = Routine(~fibGen)
```
Man kann nun mithilfe der `next`-Methode die einzelnen Werte unserer Routine abfragen:

```supercollider
// Evaluieren Sie die folgende Zeile mehrmals
~fibs.next;
/*
-> 0
-> 1
-> 1
-> 2
-> 3
-> 5
-> 8
-> 13
-> 21
-> 34
-> 55
-> 89
-> 144
-> 233
-> 377
-> 610
-> 987
-> 1597
   usw.
*/
```

Eine Routine kann außerdem gestoppt werden, indem man die `stop`-Methode verwendet:

```supercollider
// Nach der Evaluierung dieser Zeile hat unsere Routine keine Werte mehr zum Zurückgeben
~fibs.stop
```
und sie kann durch die `reset`-Methode zurückgesetzt werden:
```supercollider
// Durch die `reset`-Methode können wir immer wieder zum Anfang zurück springen.
// Evaluieren Sie die reset- und anschließend die next-Methode
~fibs.reset
~fibs.next
```

Die Klasse `Routine` erbt die Methode `play` von der abstrakten Klasse `Stream` (eine Klasse, die wir nie direkt verwenden werden, sondern nur für die Definition weiterer Klassen vorgesehen ist). Die Methode `play` funktioniert im Wesentlichen wie das wiederholte Aufrufen der Methode `next`, bis die Routine erschöpft ist und `nil` zurückgibt. Zu diesem Zeitpunkt wird das weitere Abrufen von Werten gestoppt. Ähnlich wie im oben genannten Fibonacci-Beispiel können wir den Fortschritt der Werteberechnungen in einer Schleife (wie beispielsweise `do`) verpacken und dabei entsprechende `wait`- oder `yield`-Anweisungen an geeigneten Stellen einfügen:
```supercollider
(
Routine {
	5.do {
		arg i;
		i.postln;
		"  +".postln;
		1.wait;
		"  -".postln;
		1.wait;
	};
	"Ende".postln
}.play
)

/*
-> a Routine
0
  +
  -
1
  +
  -
2
  +
  -
3
  +
  -
4
  +
  -
Ende
*/
```

oder wir können die Eingabewerte von `yield` als Wartezeit interpretieren: Wenn die Eingabe für `yield` eine Zahl ist, wird diese automatisch als Wartezeit interpretiert. Es wird so lange gewartet, bis der nächste Aufruf der `next`-Methode stattfindet:
```supercollider
(
Routine {
	0.25.postln.yield;
	0.5.postln.yield;
	1.postln.yield;
	2.postln.yield;
	4.postln.yield;
	"Ende".postln
}.play
)

/*
-> a Routine
0.25
0.5
1
2
4
Ende
*/
```

## Task

Ein Task ist größtenteils eine ähnliche Konstruktion wie eine Routine.

## Methodenvergleichstabelle
Die folgende Tabelle fasst einige der wichtigsten (gemeinsamen) Methoden der beiden Klassen zusammen. Für vollständige Informationen
konsultieren Sie bitte die Dokumentation der jeweiligen Klasse.

| Methodennamen| Routine   | Task |
|--------------|-----------|------------|
| `reset`      | Setzt den internen Zustand des Streams auf den Anfang zurück | Gleich wie bei **Routine**
| `pause`      | *Nicht vorhanden!*  | Unterbricht momentan den Stream-Ablauf. Der nächste `play`-Abruf nimmt den Vorgang dort, wo er abgebrochen wurde, wieder auf
| `stop`      | Zerstört den Stream-Ablauf  | Gleich wie `pause`
| `play`      | Evaluiert die Funktion der Klasse (das erste Argument) solange, bis sie ausgelaufen ist (d.h. nichts mehr zurückgeben kann bzw. `nil` zurückgegeben hat)  | Gleich wie bei **Routine**
