# Datenstrukturen

Und natürlich können wir nicht nur Zahlen Namen geben, sondern jeder Art Datenstruktur. In Supercollider gibt es verschiedene Klassen, die ein Blueprint für die Daten repräsentieren:

Es gibt z.B. Zahlenklassen Integer und Float. Beispiele für diese Klassen sind 1 (Integer) oder 3.14 (Float).

Die Klassen werden durch ihre Eigenschaften voneinander unterschieden. Wir können die Eigenschaften der Klassen in folgender Schreibweise abfragen:

`Klasse.Eigenschaft`

Beispiel: Die Klasse Integer (und viele andere Objekte in SC) besitzt u.a. die Eigenschaft `class`, die angibt zu welcher Klasse eine Integer-Instanz gehört:

```
1.class
```

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

- Integer ('1')
- Float ('3.14')
- String ('"Musik Elektronik"')
- Array ('[1, 2, 3, 4]')
- Boolean ('true')

## Übung
Schreiben Sie für jede der folgenden Aufgaben eine Zeile Code:

* Summe der Zahlen 2.4, 45, 0.234

* Produkt der oben genannten Zahlen


* Benutzen Sie die `size`-Eigenschaft, um herauszufinden, aus wie vielen Zeichen der folgende String besteht:
	```
	"1k2k3ja98d4nj281jkswe8s0dnmwer8c7q23ij18"
	```

* Die `size`-Eigenschaft funktioniert auch für Arrays. Finden Sie die Anzahl der Objekte in dem folgenden Array heraus:
	```
	[1, 2, 3, 4, 5, 6, 7, 8, 7, 6, 5, 4, 3, 2, 3, 4, 5, 6, 5, 4, 3, 2]
	```

* Benutzen Sie die `sum`-Eigenschaft für Arrays, um die Summe aller Zahlen im obigen Array zu berechnen.
