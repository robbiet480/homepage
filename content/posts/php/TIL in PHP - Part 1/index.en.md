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

[PHP](https://php.net) is a language that many love and (at least felt) hate even more. Even though the second group doesn't like to hear it, PHP is currently the language most often used for backend applications on the web.

[w3techs.com](https://w3techs.com/) regularly publishes figures on the use of web technologies related to frequency. For example, w3techs.com states that [currently 78.9% of all websites are server-side PHP-based](https://w3techs.com/technologies/details/pl-php/all/all). Another statistic is by [SimilarTech](https://www.similartech.com/), which counts the absolute usage of PHP and comes to a number of [7,394,225 sites](https://www.similartech.com/technologies/php). In [comparison to Python with 123,879](https://www.similartech.com/compare/php-vs-python).

The fact is that PHP is still there. Therefore, out of interest in the language and also for other very good (professional) reasons, I am now again increasingly concerned with it. My last trip with PHP already quite a while ago.


## PHP 7 is evolving. And somehow not.

PHP has always had old customs; we all know that. With version 7, the developers of the language were able to upgrade and improve it significantly. [Golem](https://www.golem.de/news/programmiersprache-im-detail-mit-php-7-wird-das-internet-schneller-1512-116750-2.html) has published a nice article about which features the language can call its own. The official documentation can be found on the [Homepage of PHP](http://php.net/manual/de/migration70.new-features.php).

But the old soul still has the language ...


## Scoping (or not)

A friend of mine sent me the following code and asked me which output I expected from the `echo`:

```php
$arr = [1, 2, 3];
echo implode(", ", $arr), "\n";

foreach ($arr as &$val) {}
echo implode(", ", $arr), "\n";

foreach ($arr as $val) {}
echo implode(", ", $arr), "\n";
```

After some thinking I came to the following result:

```php
// [ 1, 2, 3 ]
// [ 1, 2, 3 ]
// [ 1, 2, 3 ]
```

But who's surprised? It's not right.

`foreach` is not scoped in PHP. This means that the declaration of `$val` in the first of the two `foreach` loops creates the variable globally.

So what exactly happens here?

In the first `foreach` loop, `$val` is assigned the reference to a place of `$arr`. In the last iteration `$val` is a reference to `$arr[2]`.

In the second `foreach` loop the following happens:

1. iteration: `$val` is assigned the content of `$arr[0]`. Since `$val` is a reference to `$arr[2]`, the value does not change from `$val` itself, but from `$arr[2]`. So `$arr` looks like this after the first iteration: `[ 1, 2, 1]`.
2. iteration: Now `$val` is assigned the value of `$arr[1]`, thus `2`. Since nothing has changed in the references, `$arr` now looks like this: `[ 1, 2, 2 ]`.
3. iteration: The same game. `$val` is now the value of `$arr[2]` and assignes itself. Result: `[ 1, 2, 2 ]`.

In general, the functionality of references is no witchcraft now. But you wouldn't expect the behavior here, because you would assume that `$val` is scoped. Finally it was declared in a `foreach` context.

To solve the problem professionally, we recommend explicit dereferencing after each `foreach` loop.

```php
foreach ($arr as $val) {}
unset($val);
```

Somehow it's a pity that they didn't improve it. Instead there is now a


## Spaceship Operator

The Space Operator](http://php.net/manual/de/migration70.new-features.php) is an operator that compares two values. Basically the operator behaves like a scale. If the left value is larger, the result is `1`. If both values are equal, the result of the operation is `0`. If the right value is greater, it returns `-1`.

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

Practical? Useless? I am not yet so conclusive myself. In any case, this function should be used with caution, especially when comparing two characters, as only the ASCII table value is checked here. That means:

```php
echo chr(42) <=> chr(43); // -1
```
As expected. But:

```php
echo "a" <=> "A"; // 1
```
Even though it is logical that `"a"` (ASCII value 97) is greater than `"A"` (ASCII value 65), it is not immediately apparent and less intuitive.



## Stay curious

The language still has a lot to offer. Since PHP is one of the oldest languages in the web area, it has had the most errors and problems. It is really instructive to explore what exactly modern languages do better. And how PHP itself develops and improves.


I think that there will be some interesting learnings to follow ...
