# Übungslösungen
## Übung: Simulation des Klanges einer Klarinette
```
(
{
	// Grundfrequenz (mit der Mousebewegung auf der X-Achse kontrollierbar)
	var f0 = MouseX.kr(20, 200).round(20);
	// Anzahl der Partialtöne
	var sz = 10;
	Mix.new(
		SinOsc.ar(
			freq: f0 * Array.series(sz, 1, 2),
			mul: Array.geom(sz, 0.5, 0.5)
		)
	)
}.play
)
```

## Anpassen der Wartezeit zufällig zwischen 100 und 500 Millisekunden (in Sekunden ausgedrückt: 0,1 und 0,5 Sekunden)
```
(
Routine {
    (Array.series(20, 1) * 110).do {
        arg freq;
        freq.postln;
        Synth(\meinSynth, [freq: freq, dur: 0.5]);
		// Wartezeit nach jedem Ton
        0.1.rrand(0.5).wait
    }
}.play
)
```
