# Einführung in Synth
## Instrumente definieren mit SynthDef
In diesem Kapitel lernen wir die Kernelemente der Klanggenerierung auf dem SuperCollider-Server kennen. Diese tragen den Namen "Synth" (für Synthesize). Jedes Mal, wenn wir in unserem letzten Kapitel eine Funktion mit einer SinOsc UGen abgespielt haben, hat der SuperCollider-Server hinter den Kulissen einen Synth generiert. Das konnten wir auch im Post-Fenster sehen, wenn wir auf unsere Funktion die Methode `play` angewendet haben:

```supercollider
{SinOsc.ar(mul: 0.2)}.play
```

Im Post-Fenster sehen wir etwas wie **-> Synth('temp__0' : 1000)**, eine Darstellung des frisch für uns generierten Synths, der hier den Namen `temp__0` und eine ID-Nummer 1000 trägt (Dies kann bei Ihnen anders aussehen).

Wenn wir unsere Funktionsargumente definieren, können wir mit dem entstehenden Synth kommunizieren:

```supercollider
y = {arg freq=440, amp=0.2; SinOsc.ar(freq: freq, mul: amp)}.play
```

Eine Veränderung der Frequenz:

```supercollider
y.set(\freq, 330)
```

oder Veränderung der Amplitude:

```supercollider
y.set(\amp, 0.2)
```

können wir durch die `set` Methode der Synth-Klasse vornehmen.

Wir lassen unseren Synth geschmeidig über eine Sekunde ausklingen:

```supercollider
y.release(1)
```

Wir definieren einen ähnlichen Synth selbst! Das Erstellen von Synths geschieht durch die SynthDef-Klasse, wie folgt. Evaluieren Sie den folgenden Block. Im Post-Fenster sehen wir, dass unser Synth erfolgreich für den Server definiert wurde (`-> SynthDef:meinSynth`):

```supercollider
(
SynthDef(\meinSynth, {
	arg freq=440, amp=0.2;
	Out.ar(0, SinOsc.ar(freq: freq, mul: amp))
}).add
)
```

Ich gehe jede Zeile der obigen Synth-Definition durch:

```supercollider
(
// Mit der SynthDef-Klasse definieren wir den Synth. Das erste Argument ist der Name, den wir unserem
// Synth geben möchten (hier meinSynth). Dieses Argument kann entweder vom Typ Symbol sein (wie \meinSynth) oder ein String (wie "meinSynth").
SynthDef(\meinSynth,
	// Das zweite Argument ist eine Funktion, die definiert, wie wir Ugens zusammen kombinieren
	// möchten, um unser Instrument zu bauen.
	{
		// Argumente für die Funktion.
		arg freq=440, amp=0.2;
		// Es gibt hier einen großen Unterschied zu unserer {}.play Form früher. Da hatte für uns
		// SuperCollider entschieden, dass unser Signal zum ersten (linken) Kanal rausgeschickt wird.
		// Beim Definieren eines Instruments durch SynthDef müssen wir dem Instrument genau mitteilen,
		// zu welchem Ausgang unser Signal rausgeschickt werden soll. Dafür nutzen wir ein UGen
		// namens Out und instanzieren es mit der ar Methode. Das erste Argument dazu bestimmt
		// die Nummer des Ausgangskanals (mit 0 für ersten Kanal, 1 für zweiten Kanal usw.). Das
		// zweite Argument bestimmt das eigentliche Signal (also wie unser Instrument klingen wird).
		Out.ar(0, SinOsc.ar(freq: freq, mul: amp))
	}
	// Nachdem wir mit der Definition unserer Synth-Funktion fertig sind, müssen wir durch
	// die add Methode unser Instrument der Liste der für den Server bekannten Synths
	// hinzufügen:
).add
)
```

Ab jetzt können wir unser Instrument zum Erklingen bringen (wir geben unserer Synth-Instanz den Namen `x`, damit wir die Parameter des Instruments mit `set` modifizieren können). Dafür schreiben wir:

```supercollider
x = Synth(\meinSynth)
x.set(\freq, 220)
x.set(\amp, 0.1)
x.free

x.defName // -> meinSynth
```

Sinnvollerweise sind wir in der Lage, den Ausgangskanal (das erste Argument zu dem Out Ugen) ebenfalls als ein Argument für unsere Synth-Funktion zu definieren. So können wir für jede einzelne Instanz unseres Instruments einen anderen physischen Kanal auswählen. Dafür modifiziere ich die obige Definition folgendermaßen:

```supercollider
(
SynthDef(\meinSynth, {
	arg freq=440, amp=0.2, kanal=0;
	Out.ar(kanal, SinOsc.ar(freq: freq, mul: amp))
}).add
)
```

Jetzt können wir das Signal von links nach rechts schicken:

```supercollider
x = Synth(\meinSynth)
x.set(\kanal, 1)
x.free
```

Die Synth-Klasse erlaubt es uns, die Argumente unserer Synth-Funktion schon beim Kreieren des Instruments zu übergeben. Dafür übergeben wir als das zweite Argument zum Synth ein Array mit Namen der Funktionsargumente (als Symbole oder Strings) und die Werte, getrennt durch Komma:

```supercollider
Synth(\meinSynth, [\freq, 220, \amp, 0.1, \kanal, 1])
```

Wir können durch mehrere Instanzen unseres Instruments mit jeweils unterschiedlichen Parametern erstellen.

**Übung:** Evaluieren Sie den folgenden Block. Versuchen Sie zu beschreiben, was dort passiert:

```supercollider
(
var n = 40;
n.do {
	arg x;
	Synth(\meinSynth, [\freq, 220 + (x * 2), \amp, n.reciprocal, \kanal: 0.rrand(1)])
}
)
```

**Übung:** Interferenz zweier naher Frequenzen

Nutzen Sie unser Wissen aus dem letzten Kapitel über die Mehrkanalerweiterung, um ein Instrument zu definieren, das auf den rechten und linken Kanälen zwei leicht unterschiedliche Frequenzen erklingen lässt. Verwenden Sie dafür den UGen "Saw" (ein Sägezahn-Oszillator). Verwenden Sie für den linken Kanal eine Frequenz von 40 Hz und auf dem rechten Kanal eine Frequenz von 41 Hz. Setzen Sie die Amplitude des Sägezahns auf den Wert 0.2.




