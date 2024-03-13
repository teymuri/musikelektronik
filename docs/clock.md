# Zeitmanagement mit Uhren (Clocks)

Clocks in SuperCollider sind Objekte, die zur Zeitplanung von Ereignissen eingesetzt werden. Ein Ereignis kann dabei jedes beliebige Objekt sein. Unterschiedliche Objekte führen dabei unterschiedliche Aktionen aus, je nach Implementierung ihrer internen `awake`-Methode, die von einem Clock zu einer bestimmten Zeit aufgerufen wird. Zum Beispiel führt dies bei einem Funktionsobjekt zu einem Aufruf seiner `value`-Methode, was die Ausführung der Funktion auslöst.

Es gibt verschiedene Arten von Clock-Objekten. Unten diskutieren wir das `TempoClock`, das Ereignisse in einem bestimmten Tempo plant.

## TempoClock
Wir erstellen einen Tempo-Clock und betrachten einige seiner Eigenschaften. Das wichtigste Argument dabei ist das Tempo, mit dem wir den Tempo-Clock laufen lassen möchten. Dieses Tempo wird in Beats pro Sekunde (nicht pro Minute) angegeben:

```
t = TempoClock.new(2) // 2 entspricht einem Tempo von 120 BPM (Beats pro Minute)
```
Im obigen Clock tickt die Uhr doppelt so schnell wie im normalen Standard-Clock von SuperCollider, der einmal pro Sekunde tickt. Wir können die BPS (Beats pro Sekunde) bei einem Tempo-Clock abfragen:
```
t.tempo; // => 2.0
// Und hier ist das Tempo beim Standard-Tempo-Clock:
TempoClock.default.tempo; // => 1.0
```
Um dies zu sehen, können wir die folgende Routine sowohl auf dem Standard-Clock als auch auf unserem eigenen Clock ausführen. Beachten Sie, dass wir die Wartezeit der Endlosschleife in der Funktion der Routine auf 1 gesetzt haben (die Zeiteinheit). Diese Einheit entspricht bei unserem Tempo-Clock einer Zeitmaße von 0.5 Sekunden. Achten Sie dabei auf das Post-Fenster:
```
(
r = Routine {
	inf.do {
		arg i;
		i.postln;
		1.wait
	}
}
)

// Wir lassen die Routine zuerst auf dem Standard-Tempo-Clock laufen:
r.play // Das entspricht dem folgenden Ausdruck: `r.play(clock: TempoClock.default)`

// Und einmal auf unserem eigenen Clock:
r.play(clock: t)
```

Und wir können die Routine danach fragen, auf welchem Clock sie läuft:
```
r.clock.tempo // => 2.0
```

Bzw. können wir während die Routine läuft das Tempo ihres Clocks einstellen:

```
r.clock.tempo = 0.5 // Das Tempo wird auf 30 BPM herabgesetzt
```
