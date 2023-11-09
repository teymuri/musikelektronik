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
