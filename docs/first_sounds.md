# Die ersten Töne

## Starten des Servers
Im letzten Kapitel haben wir uns mit der Sprache SuperCollider beschäftigt. Die Sprache allein kann keinen Klang erzeugen, sondern ist lediglich für die Weitergabe unserer Befehle an den Klang-Server verantwortlich. Um unseren ersten Ton in SuperCollider zu erzeugen, müssen wir zuerst den Server starten. Erinnern wir uns daran, dass die Einzelvariabel "s" für den Server steht. Mit dem Aufrufen des `boot`-Befehls starten wir ihn:

```
s.boot;
```
Aus historischen Gründen werden in SuperCollider auch die Objekte, die für die Klangerzeugung verantwortlich sind, als "UGen" bezeichnet. Eine dieser UGens ist SinOsc, die eine einfache Sinusschwingung generiert. Die meisten UGens in SuperCollider benötigen Informationen von uns (ähnlich wie Argumente in Funktionen im letzten Kapitel). Zum Beispiel benötigt unser SinOsc UGen folgende Eingaben: Frequenz, Phase, Amplitude und DC-Offset. Geben Sie den folgenden Code in Ihren SuperCollider-Editor ein. Sobald Sie die Klammer öffnen, sollten Sie diese Argumente sehen können. Achten Sie darauf, dass diese Argumente auch Standardwerte haben, so dass, wenn wir keine Informationen bereitstellen, diese Standardwerte übernommen werden:

```supercollider
SinOsc.ar();
```
(Tipp: Wenn die Argumentenliste nicht angezeigt wird, kann man sie durch Drücken der Tastenkombination **Shift+Ctrl+L** wieder einblenden.)

Im Kontext von SuperCollider steuern die Argumente des SinOsc (Sinus Oszillator) die Eigenschaften der erzeugten Sinusschwingung. Hier sind die Argumente:

1. **Frequenz (freq):** Dies bestimmt, wie viele Schwingungen pro Sekunde erzeugt werden. Eine höhere Frequenz erzeugt einen höheren Ton.

2. **Phase (phase):** Die Phase gibt den Startpunkt der Sinusschwingung an. Eine Phase von 0 bedeutet, dass die Schwingung bei null Grad beginnt.

3. **Amplitude (mul):** Die Amplitude beeinflusst die Höhe der Schwingung und damit die Lautstärke des Tons. Eine höhere Amplitude erzeugt einen lauter klingenden Ton (Werte zwischen 0 und 1).

4. **DC-Offset (add):** Dies ist eine konstante Spannung, die zur Sinuswelle hinzugefügt wird. Sie beeinflusst den Grundpegel der Welle und kann dazu verwendet werden, sicherzustellen, dass die Welle um den Nullpunkt zentriert ist oder einen bestimmten Ausgangspunkt hat.

Der Code `SinOsc.ar()` erzeugt standardmäßig eine Sinusschwingung mit den Standardwerten für Frequenz, Phase, Amplitude und DC-Offset. Wenn Sie spezifische Werte angeben möchten, können Sie sie als Argumente im SinOsc-Objekt setzen, z.B. `SinOsc.ar(440, 0, 0.5, 0)`, wobei 440 die Frequenz, 0 die Phase, 0.5 die Amplitude und 0 das DC-Offset ist.

Um nun den Klang einer 440-Hz-Frequenz zu hören, müssen wir unsere UGens in eine Funktion einbetten und die `play`-Nachricht an diese Funktion senden. Dadurch wird die enthaltene UGen an den Server gesendet, und wir können etwas hören.

Bevor wir weitermachen und den ersten Sinuston an den Server senden, ist es wichtig zu wissen, wie wir den Ton wieder ausschalten können. Merken Sie sich die Tastenkombination **Ctrl+.**
Dies deaktiviert den Server, und wir nutzen momentan diese Methode, um den Ton schnell auszuschalten.

Schreiben Sie nun den folgenden Code-Block und führen Sie ihn aus (mit dem Cursor innerhalb des Blocks **Ctrl+Enter** drücken):

```supercollider
{SinOsc.ar(440, 0, 0.5, 0)}.play;
```
Stoppen Sie den Server mit **Ctrl+.** und lassen Sie uns den obigen Code etwas genauer betrachten.

Wir öffnen zuerst mit den runden Klammern einen Block. Dies ist praktisch, wenn unser Code über mehrere Zeilen geht, damit wir alle Zeilen auf einmal auswerten können.

```supercollider
()
```

Als Nächstes öffnen wir ein paar geschweifte Klammern, um eine anonyme Funktion zu definieren. Sie ist anonym, weil wir diese Funktion nur an dieser Stelle als Beispiel verwenden und ihr keinen Namen geben:

```supercollider
({})
```

Innerhalb unserer Funktion schreiben wir unsere UGen mit den gewünschten Argumenten:

```supercollider
({SinOsc.ar(440, 0, 0.5, 0)})
```

Auf das Objekt SinOsc wenden wir die Nachricht `ar` an, die für Audio Rate steht. Die `ar`-Funktion sorgt dafür, dass die Sample-Ausgaben der UGen (hier SinOsc) in einer Audiorate (oft 44100 Mal pro Sekunde) vom Server an die Audiokarten unseres Rechners geschickt werden. Von dort werden diese Samples (die im Prinzip nichts anderes sind als Dezimalzahlen) an einen sogenannten DAC (Digital Audio Converter) geschickt, um dann als Spannungsvariationen (nach eventueller Verstärkung) an Lautsprecher gesendet zu werden. Wichtig ist auch zu verstehen, dass die `ar`-Methode gleichzeitig eine Instanz der SinOsc-Klasse generiert.

Wenn wir an dieser Stelle unseren Code-Block auswerten, sehen wir im Post-Fenster, dass er zu einer Funktion evaluiert wurde (`-> a Function`).

Jetzt können wir unsere Funktion an den Server senden. Dafür rufen wir die `play`-Methode auf die Funktion auf. Was wir hier über die `play`-Methode einer Funktion wissen müssen: `play` generiert auf dem Server einen sogenannten Synth. Synths sind die Klanggeneratoren auf der Server-Seite. Diese Synth wird nach der Erstellung zum Klingen gebracht:

```supercollider
({SinOsc.ar(440, 0, 0.5, 0)}.play)
```

Wenn wir die tatsächliche Synth, die für uns von `play` generiert wurde, weiterhin kontrollieren möchten (der serverinterne Name wird im Post-Fenster nach dem Auswerten der obigen Zeile angezeigt), können wir ganz einfach die ausgegebene Synth einer Variable zuweisen:

```supercollider
(x = {SinOsc.ar(440, 0, 0.5, 0)}.play)
```

Jetzt enthält `x` die Synth, und wir können sie zum Beispiel über die Variable `x` ausschalten, indem wir der Variable `x` die Nachricht `free` senden:

```
x.free;
```
Das ist praktisch, da wir mit `free` nur diese eine Synth-Instanz abschalten können (im Unterschied zu unserer vorherigen Lösung **Ctrl+.**, die den kompletten Server herunterfährt).

Wir können unserer Funktion Argumente hinzufügen, sodass wir die Parameter unseres SinOsc explizit eingeben können:

```supercollider
(
x = {
	arg freq = 440, amp = 0.5;
	SinOsc.ar(freq, mul: amp)
}.play
)
```

So können wir durch die `set`-Methode unserer Synth-Instanzen die Frequenz oder die Amplitude neu setzen, und das während der Synth läuft (also während unser Sinuston weiter erklingt):

```supercollider
// Amplitude halbieren
x.set(\amp, 0.25);

// Die Frequenz um eine Oktave tiefer spielen
x.set(\freq, 220);
```
