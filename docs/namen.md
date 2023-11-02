# Namengebung

Da es in der Programmierung (für uns Menschen) allgemein einfacher ist, sich Namen statt komplexer Daten zu merken, können wir unseren Daten in unserem Code Namen zuweisen.


Es gibt insgesamt drei Arten von Namen:

## Einzellige Variablen
Hierfür verwenden wir einzelne Buchstaben in Kleinbuchstaben, wie folgt:

```
a = 123
```

Es gibt jedoch eine Ausnahme: "s" ist bereits von SuperCollider (SC) reserviert und verweist auf den aktuellen Server. Daher sollte "s" nicht überschrieben werden.

## Lokale Variablen
Alternativ können wir vollständige Namen verwenden. Es ist wichtig, dass diese mit Kleinbuchstaben beginnen. Für diese Art von Namen müssen sie mit "var" deklariert und in einem Code-Block stehen. Außerhalb der geschweiften Klammern haben diese Namen keine Bedeutung. Code-Blöcke sind alle Teile, die sich zwischen zwei geschweiften Klammern befinden:

```supercollider
(
var name = 123;
name = name + 1;
name
)
```

## Globale Variable
Die letzte Art der Namenszuweisung erfolgt, indem wir vor den Namen eine Tilde setzen. Dadurch müssen diese Namen nicht in einem Code-Block eingeschlossen sein und können überall im Code verwendet werden. Beachten Sie, dass auch hier die Namen mit Kleinbuchstaben beginnen müssen:

```supercollider
~drei = 3;
(
~vier = 4;
)
~sieben = ~drei + ~vier;
```

(Wenn Sie mit Codeblöcken arbeiten, achten Sie darauf, dass das Ergebnis eines Codeblocks das ist, was in der letzten Zeile des Blocks steht!)
