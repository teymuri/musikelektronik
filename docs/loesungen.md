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