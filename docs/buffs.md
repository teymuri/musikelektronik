# Buffers
Buffers (auf Deutsch: Puffer) sind Speicherbereiche im Arbeitsspeicher unseres Rechners, auf die wir eine Sequenz von Zahlen schreiben können. Diese Zahlen repräsentieren oft einzelne Samples einer Audio-Datei.
Wir können mit der `read`-Methode (als Klassenmethode) auf dem Server einen Buffer reservieren und eine Audiodatei darauf schreiben:
```
b = Buffer.read(s, "/pfad/zur/audio-datei/glass-bowl.wav")
```
Mit der `query`-Methode können wir einige schnelle Informationen von unserem neu zugewiesenen Buffer abrufen, und mit der `play`-Methode können wir den Inhalt des Buffers abspielen:
```
b.query

/*
bufnum: 8
numFrames: 118769
numChannels: 2
sampleRate: 44100.0
*/

b.play
```
__Anmerkung__ Unter der Haube ruft die `read`-Methode die Klassenmethode `alloc` der Buffer-Klasse auf, die einen Puffer im Speicherplatz zuweist und die neu erstellte Pufferinstanz zurückgibt. Das ist der Grund, warum wir diese Instanz der Variable `b` zuweisen, damit wir weiter mit unserem Pufferobjekt arbeiten können.

## Abspielen des Buffers
Das Abspielen eines Buffers kann mithilfe des UGens `PlayBuf` erfolgen. Hier ist ein Beispiel, wie ein Puffer (`b`) mit `PlayBuf` abgespielt wird, zusammen mit der Erklärung der Argumente von `PlayBuf`:

```
// Beispiel PlayBuf
b = Buffer.read(s, "/home/okavango/Desktop/glass-bowl.wav");
(
// Das Argument `numChannels` bezieht sich auf die
// Anzahl der Kanäle der Audio-Datei im Buffer.
// Es ist sicherer, dieses Argument mit `b.numChannels`
// zu belegen, damit automatisch die richtige Anzahl an
// Kanälen übermittelt wird.
var numChannels = b.numChannels;
/*
Das Argument `rate` stellt die Abspielgeschwindigkeit des Buffers dar,
wobei 1 der Originalgeschwindigkeit entspricht. Bei 2 wird der Buffer doppelt
so schnell abgespielt (eine Oktave höher), und bei 0.5 wird er halb so schnell
abgespielt (eine Oktave tiefer). Negative Zahlen bewirken, dass der Buffer
rückwärts abgespielt wird.
*/
var rate = 1;
/*
Das `doneAction`-Argument bewirkt, ähnlich wie bei den oben erklärten Hüllkurven,
dass das `PlayBuf`-UGen aus dem Arbeitsspeicher entfernt wird, sobald die Audiodatei 
bis zum Ende abgespielt ist.
*/
var action = 2;
{PlayBuf.ar(numChannels: numChannels, bufnum: b, rate: rate, doneAction: action)}.play;
)
```

Es kann passieren, dass die Audiodatei, die wir in den Puffer schreiben, mit einer anderen Abtastrate aufgenommen wurde, und der Supercollider-Server mit einer unterschiedlichen Abtastrate läuft. Wenn dies der Fall ist, wird es zu Unstimmigkeiten bezüglich der Endabspiel-Frequenz kommen. 

Beispiel: Wenn wir eine Audiodatei mit einer Abtastrate von 44100 Hz auf einen Puffer laden und unser Server mit einer Taktfrequenz von 48000 Hz läuft, wird das Audio um einen Faktor von 48000 / 44100 = 1.0884353741497 höher klingen als die originale Audio-Datei. Um dies zu kompensieren (sprich einen Abspielfaktor von 1 zu erreichen), müssen wir die Abspielrate mit dem Kehrwert dieses Faktors multiplizieren. Die Abtastraten der jeweiligen Server und des Buffers können wir folgendermaßen herausfinden: Mit den UGens `BufSampleRate.ir(b)` können wir die Abtastrate des Buffers `b` abrufen, und das UGen `SampleRate.ir` für die Abtastrate des Servers verwenden. Alternativ können wir die Variablen `b.sampleRate` (für den Buffer `b`) und `s.sampleRate` (für den lokalen Server `s`) verwenden.

Also kann das obige Beispiel folgendermaßen verbessert werden, um etwaige Abtastdifferenzen beim Abspielen auszugleichen:

```
(
var numChannels = b.numChannels;
var rate = 1;
var action = 2;
{
	PlayBuf.ar(
		numChannels: numChannels,
		bufnum: b,
		rate: BufSampleRate.ir(b) / SampleRate.ir * rate,
		doneAction: action)
}.play;
)
```

Es ist ebenso möglich, das UGen `BufRateScale` zu verwenden, das den erforderlichen Ausgleichsfaktor für uns berechnet.
Dadurch ersparen wir uns den Schritt der Division:


```
(
var numChannels = b.numChannels;
var rate = 1;
var action = 2;
{
	PlayBuf.ar(
		numChannels: numChannels,
		bufnum: b,
		rate: BufRateScale.ir(b) * rate,
		doneAction: action)
}.play;
)
```

Drei weitere Argumente von `PlayBuf` sind `trigger`, `loop` und `startPos`.

Ein Trigger findet statt, wenn ein Signal einen Übergang von einem negativen zu einem positiven (oder 0) Wert durchläuft. Beispielsweise erfolgt der Trigger, wenn der Signalwert von -0.1234 auf 0.1234 oder ähnlich wechselt. Viele UGens erzeugen Werte, die sich im negativen und positiven Bereich bewegen. Ein Beispiel dafür ist der `Impulse` UGen, der in einer bestimmten Frequenz den Wert 1 ausgibt und sonst 0. 

Führen Sie die folgende Zeile aus und beobachten Sie das Post-Fenster:
```
{Impulse.kr(1).poll(trig: 2)}.play
```

Wir übergeben unserem `PlayBuf` ein `Impulse`-UGen als Argument für den Trigger:
```
(
var numChannels = b.numChannels;
var rate = 1;
var action = 2;
{
	PlayBuf.ar(
		numChannels: numChannels,
		trigger: Impulse.kr(1).poll(trig: 2),
		bufnum: b,
		rate: BufRateScale.ir(b) * rate,
		doneAction: action
	)
}.play;
)
```
__Übung__ Ändern Sie die Frequenz des Triggers, indem Sie die UGens `MouseX` als Argument für den `Impulse`-Generator verwenden, um Werte zwischen den Frequenzen 0.1 und 10 zu erhalten. Wenn das `PlayBuf` beendet wird, kommentieren Sie das `doneAction`-Argument aus und versuchen Sie es erneut. Versuchen Sie den Zusammenhang zu erklären.

Das Argument `startPos` bestimmt die Samplenummer, ab der das Abspielen beginnen soll. Mit dem UGen `BufFrames` erhalten wir die Anzahl der Samples im Buffer. Mögliche Werte für `startPos` können zwischen 0 und `BufFrames` liegen:
```
(
var numChannels = b.numChannels;
var rate = 1;
var action = 2;
{
	PlayBuf.ar(
		numChannels: numChannels,
		trigger: Impulse.kr(1),
		// Bewegen Sie die Maus nach oben und unten
		startPos: MouseY.kr(BufFrames.ir(b), 0),
		bufnum: b,
		rate: BufRateScale.ir(b) * rate
	)
}.play;
)
```

Und das boolesche Argument `loop` (selten verwendet) setzt das Abspielen in einer Schleife fort: Sobald die Datei bis zum letzten Sample abgespielt wurde, beginnt das Abspielverfahren wieder beim ersten Sample und so weiter.
