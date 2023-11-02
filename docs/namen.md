# Namengebung

In der Programmierung (für uns Menschen) ist es allgemein einfacher, sich Namen anstelle von komplexen Daten zu merken. Daher können wir in unserem Code Namen für unsere Daten zuweisen. Es gibt drei Möglichkeiten Namen zu definieren:

## Einzellige Namen

Hierfür verwenden wir einzelne Kleinbuchstaben, wie folgt:

```supercollider
a = 123
```

Es gibt jedoch eine Ausnahme: "s" ist bereits von SuperCollider (SC) reserviert und verweist auf den aktuellen Server. Daher sollte "s" nicht überschrieben werden.

## Lokale Namen

Alternativ können wir vollständige Namen verwenden. Es ist wichtig, dass diese mit Kleinbuchstaben beginnen. Für diese Art von Namen müssen sie mit "var" deklariert und in einem Code-Block stehen. Außerhalb der geschweiften Klammern haben diese Namen keine Bedeutung. Code-Blöcke sind alle Teile, die sich zwischen zwei geschweiften Klammern befinden:

```supercollider
(
var name = 123;
name = name + 1;
name
)
```

## Globale Namen

Die letzte Art der Namenszuweisung erfolgt, indem wir vor den Namen eine Tilde setzen. Dadurch müssen diese Namen nicht in einem Code-Block eingeschlossen sein und können überall im Code verwendet werden. Beachten Sie, dass auch hier die Namen mit Kleinbuchstaben beginnen müssen:

```supercollider
~drei = 3;
(
~vier = 4;
)
~sieben = ~drei + ~vier;
```

Achtung: Wenn Sie mit Codeblöcken arbeiten, achten Sie darauf, dass das Ergebnis eines Codeblocks das ist, was in der letzten Zeile des Blocks steht!