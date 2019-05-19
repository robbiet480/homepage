---
title: "TIL in PHP - Part 1"
date: 2019-02-04T13:23:00+01:00
draft: false
images:
tags:
    - languages
    - php
    - why
---

[PHP](https://php.net) ist ja eine Sprache, die viele lieben und (zumindest gefühlt) noch mehr hassen. Auch wenn gerade die zweite Gruppe das nicht gerne hört, aber PHP ist aktuell die Sprache, die am häufigsten für Backendanwendungen im Web verwendet wird.

[w3techs.com](https://w3techs.com/) veröffentlicht regelmäßig Zahlen zur Nutzung von Webtechnologien im Zusammenhang mit der Häufigkeit. So ermittelt w3techs.com, dass [derzeit 78,9% aller Webseiten serverseitig auf PHP basieren](https://w3techs.com/technologies/details/pl-php/all/all). Eine andere Statistik ermittelt [SimilarTech](https://www.similartech.com/), die die absoluten Einsätze von PHP zählt und auf eine Zahl von [7.394.225 Webseiten](https://www.similartech.com/technologies/php) kommen. Im [Vergleich dazu Python mit 123.879](https://www.similartech.com/compare/php-vs-python).

Fakt ist also, dass PHP noch da ist. Deswegen, aus Interesse an der Sprache und auch aus anderen sehr guten (beruflichen) Gründen befasse ich mich nun wieder verstärkt damit. Meine letzte Reise mit PHP schon eine ganze Weile her.


## PHP 7 entwickelt sich

PHP hatte schon immer alte Zöpfe; das wissen wir alle. Mit der Version 7 konnten die Entwicklern der Sprache diese auf jeden Fall deutlich aufwerten und verbessern. [Golem](https://www.golem.de/news/programmiersprache-im-detail-mit-php-7-wird-das-internet-schneller-1512-116750-2.html) hat dazu einen schönen Artikel darüber herausgebracht, welche Features die Sprache ihr eigen nennen darf. Die offizielle Doku dazu findet sich auf der [Homepage von PHP](http://php.net/manual/de/migration70.new-features.php).

Ein paar komsiche Dinge gibt es da aber doch noch ...

## (Kein) Scoping

Ein Freund von mir schickte mir folgenden Code und fragte mich, welche Ausgabe ich bei den `echo` erwarte:

```php
$arr = [1, 2, 3];
echo implode(", ", $arr), "\n";

foreach ($arr as &$val) {}
echo implode(", ", $arr), "\n";

foreach ($arr as $val) {}
echo implode(", ", $arr), "\n";
```

Nach einigem Überlegen kam ich auf folgendes Ergebnis:

```php
// [ 1, 2, 3 ]
// [ 1, 2, 3 ]
// [ 1, 2, 3 ]
```

Aber, wen wundert es, es ist natürlich nicht richtig.

`foreach` wird in PHP nicht gescoped. Das bedeutet, dass die Deklaration von `$val` in der ersten der beiden `foreach` Schleifen die Variable global anlegt.

Was passiert hier also genau?

In der ersten `foreach` Schleife wird `$val` die Referenz auf eine Stelle in `$arr` zugewiesen. In der letzten Iteration ist `$val` also eine Referenz auf `$arr[2]`.

In der zweiten `foreach` Schleife passiert nun folgendes:

1. Iteration: `$val` wird der Inhalt von `$arr[0]` zugewiesen. Da `$val` eine Referenz auf `$arr[2]` ist, ändert sich nicht der Wert von `$val`, sondern von `$arr[2]`. Somit sieht `$arr` nach der ersten Iteration so aus: `[ 1, 2, 1]`.
2. Iteration: Jetzt wird `$val` der Wert von `$arr[1]` zugewiesen, also `2`. Da sich an den Referenzen nichts verändert hat, sieht `$arr` nun wie folgt aus: `[ 1, 2, 2 ]`.
3. Iteration: Das gleiche Spiel. `$val` wird nun der Wert von `$arr[2]` zugewiesen. Endergebnis: `[ 1, 2, 2 ]`.

Generell ist die Funktionsweise von Referenzen jetzt kein Hexenwerk. Allerdings würde man das Verhalten hier so gar nicht erwarten, da man auf jeden Fall davon ausgehen würde, dass `$val` gescoped ist. Schließlich wurde es in einem `foreach` Kontext deklariert.

Um das Problem übrigens professionell zu lösen, empfiehlt sich nach jeder `foreach` Schleife die explizite Dereferenzierung.

```php
foreach ($arr as $val) {}
unset($val);
```

## Spaceship Operator

[Der Spaceship Operator](http://php.net/manual/de/migration70.new-features.php) ist ein Operator, welcher zwei Werte vergleicht. Im Grunde verhält sich der Operator wie eine Waage. Ist der linke Wert größer, ist das Ergebnis `1`. Sind beide Werte gleich, so ist das Ergebnis der Operation `0`. Ist der rechte Wert größer, lautet das Ergebnis `-1`.

```php
// Integers
echo 1 <=> 1;  // 0
echo 1 <=> 2;  // -1
echo 2 <=> 1;  // 1

// Floats
echo 1.5 <=> 1.5 // 0
echo 1.5 <=> 2.5 // -1
echo 2.5 <=> 1.5 // 1

// Strings
echo "a" <=> "a" // 0
echo "a" <=> "b" // -1
echo "b" <=> "a" // 1
```

Ich bin bisher noch nicht in einen Fall geraten, in dem das nützlich ist. Wie sinnvoll und nutzbringend das also ist, ist mir noch nicht ganz klar. Auf jeden Fall ist auch diese Funktion mit Vorsicht zu genießen, vor allem beim vergleichen zweier Character, da hier lediglich der ASCII-Tabellen-Wert überprüft wird. Das heißt:

```php
echo chr(42) <=> chr(43); // -1
```
Wie erwartet. Aber:

```php
echo "a" <=> "A"; // 1
```
Auch wenn es logisch ist, dass `"a"` (ASCII Wert 97) größer ist, als `"A"` (ASCII Wert 65), ist es nicht sofort ersichtlich und wenig intuitiv.



## Gespannt bleiben

Die Sprache hat noch einiges zu bieten. Da PHP mit die älteste Sprache im Webbereich ist, hat diese entsprechend auch die meisten Fehler und Probleme (gehabt) und mit auch eine starke Evolution durchgemacht. Es ist wirklich lehrreich zu erforschen, was genau moderne Sprachen wie besser machen. Und wie sich PHP selbst entwickelt und verbessert.
