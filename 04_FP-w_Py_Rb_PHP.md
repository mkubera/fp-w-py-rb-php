# Funkcyjne programowanie w Pythonie, Ruby, oraz PHP


# Spis tresci
- [Czym jest FP](#czym-jest-fp)
- [Jezyki FP vs jezyki z paradygmatem FP](#jezyki-fp-vs-jezyki-z-paradygmatem-fp)
- [Koncepty funkcyjnego programowania](#koncepty-funkcyjnego-programowania)
  - Pure functions
  - Immutability
  - First-class functions
  - Higher-order functions
  - Declarative code
  - Funkcje czastkowe
  - Currying
  - Recursion
  - Popularne operacje na kolekcjach
- [Przykladowe uzycia](#przykladowe-uzycia)
- [Cwiczenia](#cwiczenia)
- [Przykłady użycia funkcyjnego paradygmatu](#przyklady-uzycia-funkcyjnego-paradygmatu)
  - Mosty do innych jezykow
- [Cwiczenia 2](#cwiczenia-2)
- [Uzyteczne linki](#uzyteczne-linki)



# Czym jest FP

Funkcyjne programowanie (FP, functional programming) jest jednym z kilku wiodacych stylow programowania. Na skali popularnosci, zdecydowanie wciaz za style obiektowym (OOP, object-oriented programming), ale przed stylem reaktywnym (RP, reactive programming) czy reaktywno-funkcyjnym (RFP, reactive functional programming).  
Dzieki uniwersytetom, kursom, codecamp'om, itp. OOP stalo sie i pozostaje najbardziej znanym stylem, wobec czego zaloze na moment, ze jest z nim obeznany przynajmniej w podstawowym stopniu, i wyjawie co nastepuje:
- w FP nie ma:
  - klas
  - instancji
  - obiektow
  - stanu (z pewnymi wyjatkami)
  - metod
  - dziedziczenia
  - polimorfizmu
  - `this`, ani `self` (ani zadnego ich ekwiwalentu)
  - imperatywnego kodu

Istnieja zas nastepujace:
  - moduly
  - funkcje
  - abstrakcja (w sensie funkcji prywatnych i publicznych)
  - funkcje pierwszej kategorii i funkcje wyzszego rzedu
  - funkcje nazwane i anonimowe (lambdy)
  - deklaratywny kod
Oraz:
  - (typowo) kolekcje w formie list, a nie arrays
  - rekursja ogonowa (tail recursion)
  - currying
  - funkcje czastkowe i czastkowa aplikacja funkcji (partial application)
  - oraz inne, ktore dzis poznasz


# Jezyki FP vs jezyki z paradygmatem FP

Wiele jezykow zaimplementowalo elementy FP, ale tylko niektore staly sie jezykami czysto FP paradygmatowymi. Oto kilka przykladow:

Czesciowa implementacja w:
- Ruby
- Python
- PHP
- Java oraz Scala
- JavaScript
- C# oraz F#
- Lua

Jezyki funkcyjne:
- Elixir
- Erlang
- [Haskell](https://www.haskell.org/)
- Clojure
- [Elm](https://www.elm-lang.org/)

Warto tez wspomniec istnienie tzw. czysto funkcyjnych jezykow (purely functional languages). W tych jezykach mozemy pisac wylacznie czyste funkcje. Do tej rodziny naleza:
- Haskell
- Elm
- Idris
- [i inne](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type#Pure)


Dla nieco lepszego zrozumienia (czysto) funkcyjnych jezykow polecam wpis ze strony Haskell:

> Kazda funkcja w j. Haskell jest funkcja w sensie matematycznym (t.j. "czysta"). Nawet operacje typu IO (Input/Output) sa niczym objasnienia tego, co nalezy zrobic, stworzone przez czysty kod. Nie ma 'statements' czy 'instructions', a sa jedynie 'expressions', ktore nie moga mutowac zmiennych (lokalnych czy globalnych) i nie maja dostepu do czasu czy losowych numerow.  
  

OK, wiec czemu w ogole powinnismy byc zainteresowani FP w naszym ulubionym jezyku? Poniewaz nasze programy beda benefitowac z nastepujacych:
- latwe do testowania, male funkcje
- przejrzystosc programu
- latwiejszy refaktoring
- latwiej bedzie nam rozumowac na temat programu
- mniej kodu do pisania, edycji, i czytania
- (jezeli uzywamy tylko lub duzo czystych funkcji) wieksza pewnosc jakosci kodu

No i radosc z pisania :-)


# Koncepty funkcyjnego programowania

Mala notka na wstepie. Poniewaz uzywamy funkcyjnego paradygmatu, to koncept `this` czy `self` wyrzucamy do lamusa. Nie tykamy sie lokalnego stanu w naszych funkcjach.


## Pure functions

Spelnione musza byc dwa warunki, zebysmy mogli otrzymac funkcje typu pure ("czysta"; w odroznieniu od impure).
1. Niezaleznie od tego, ile razy wywolamy funkcje, zawsze otrzymamy ten sam rezultat (output) przy tych samych
argumentach (input).
2. Funkcja nie stworzy zadnych efektow ubocznych (side-effects), np. nie wywola HTTP request, ktory bedzie
mial wplyw na inny system (serwer, baze danych, etc.).  
  

Przyklady:

```php
// funkcja otrzymuje jakis numer jako argument (n) i dodaje do niego 1
$addOne = function ($n) {
    return $n + 1;
};

// wielokrotne wywolanie funkcji z tym samym argumentem (input) produkuje ten sam rezultat (output)
$addOne(2); // 3
$addOne(2); // 3
$addOne(2); // 3

```

```python
// funkcja otrzymuje jakis numer jako argument (n) i dodaje do niego 1
def add_one(n):
    return n + 1

// wielokrotne wywolanie funkcji z tym samym argumentem (input) produkuje ten sam rezultat (output)
add_one(2); // 3
add_one(2); // 3
add_one(2); // 3
```

```ruby
// funkcja otrzymuje jakis numer jako argument (n) i dodaje do niego 1
def add_one(n)
  n + 1
end

// wielokrotne wywolanie funkcji z tym samym argumentem (input) produkuje ten sam rezultat (output)
add_one(2); // 3
add_one(2); // 3
add_one(2); // 3

```

Kiedy zaczyna sie niebezpieczenstwo impure functions? Wtedy, gdy nasza funkcja moze zaczac produkowac rozne rezultaty.

```php
$someNum = 1;

$addOne = function ($n) {
    return $n + $someNum;
};

// wielokrotne wywolanie funkcji z tym samym argumentem (input) produkuje ten sam rezultat (output)
$addOne(2); // 3
$addOne(2); // 3
$addOne(2); // 3

$someNum = $someNum + 1;

$addOne(3); // 4
$addOne(3); // 4
$addOne(3); // 4
```

Albo gorzej!

```php
$someNum = 1;

$addOne = function ($n) {
    $someNum = $someNum + 1;
    return $n + $someNum;
};

// wielokrotne wywolanie funkcji z tym samym argumentem (input) produkuje ten sam rezultat (output)
$addOne(2); // 4
$addOne(2); // 5
$addOne(2); // 6
```

Czy kazde uzycie jakiejs wartosci spoza funkcji sprawia, ze funkcja staje sie impure? Nie koniecznie.

```php
$someNum = function () { 
    return 1;
};

$addOne = function ($n) {
    return $n + $someNum;
};

// wielokrotne wywolanie funkcji z tym samym argumentem (input) produkuje ten sam rezultat (output)
$addOne(2); // 3
$addOne(2); // 3
$addOne(2); // 3
```

Teraz `someNum` tez jest czysta funkcja, wobec czego wyprodukowana wartosc nigdy sie nie zmieni! Pure awesomeness 😀


## Immutability

Jedna z najprzyjemniejszych cech funkcyjnego stylu jest niezmiennosc danych (immutability of data). Niestety, wiekszosc danych w niefunkcyjnych jezykach jest mutowalna. Te dane sa niemutowalne:
- PHP: 
  - `DateTimeImmutable` class, `define()`, `const` pozwalaja na tworzenie niemutowalnych danych
  - inne metody, tj. enkapsulacja danych i udostepnienie jedynie read-only metod, jak opisane [tutaj](https://www.simonholywell.com/post/2017/03/php-and-immutability/) daja efekt niemutowalnosci
- Python
  - numeryczne typy, stringi, bajty, zamrozone sety, oraz krotki (tuples) [notka: wiecej informacji [tutaj](https://towardsdatascience.com/immutable-vs-mutable-data-types-in-python-e8a9a6fcfbdc)]
- Ruby
  - uzywanie `dup`, `clone` albo `freeze` na arrays
  - freezing strings
  - (uzywajac [biblioteki immutable-ruby](https://github.com/immutable-ruby/immutable-ruby)) hashe, vectory, sety, sortowane sety, listy, or deques (czyli stacks)
  - wiecej informacji [tutaj](https://valve.github.io/blog/2014/07/04/from-object-to-functional-immutability/)

Co istotne, dla wszystkich jezykow, jezeli uzyjemy czystej funkcji, dane pozostana niezmienione.  
  
Prosty przyklad:

```php
$kidsStuff = ['ball', 'dinosaur', 'lollypop'];

$stuff = array_map(function ($item) { return "my" . $item; }, $kidsStuff);
```

`stuff` jest nowym array (po transformacji), ktory w zaden sposob nie wplywa na ksztalt `kidsStuff` array. 



## First-class functions

Funkcje w Py/Rb/PHP, podobnie jak w jezykach funkcyjnych, sa funkcjami pierwszej kategorii (czasem nazywanymi tez obywatelami pierwszej kategorii (first-class citizens), tzn. funkcje moga brac inne funkcje jako argumenty (input), oraz zwracac funkcje (output).

```php
// tutaj, funkcja map bierze anonimowa funkcje (tzw. predykat), 
// ktora mowi co ma sie stac z kazdym elementem array

$stuff = array_map(function ($n) { return $n + 1; }, [1,2,3]); // [2,3,4]
```

notka: map czesto dziala nie tylko na arrays, ale ogolnie na kolekcjach (np. na zlaczonych listach (linked lists))  
notka2: jezyki imperatywne czesto preferuja arrays jako podstawowa kolekcje, natomiast jezyki funkcyjne zazwyczaj preferuja listy


## Higher-order functions

Typowymi funkcjami wyzszego rzedu, bez ktorych zaden jezyk funkcjny obyc sie nie moze, sa `map`, `filter`, oraz `reduce`. W jezykach funkcyjnych generalnie nie uzywamy arrays, wobec czego nie mamy dostepu do indeksow w kolekcji. Natomiast mamy mozliwosc dzialac na wszystkich lub roznych elementach kolekcji. Mamy rowniez mozliwosc "podzielenia" listy na pierwszy element (`head`, glowke) oraz pozostale elementy (`tail`, ogonek) -- wiecej o nich za chwile.  
  

`map` pozwala nam transformowac dane w kolekcji.  
`filter` pozwala nam wybierac konkretne dane z kolekcji.  
`reduce` pozwala nam zredukowac dane z kolekcji.  
  

Przyklady:

```php
// map
array_map(function ($n) { return $n + $n; }, [1,2,3]); // [2,4,6]

// filter
array_filter([1,2,3], function ($n) { return $n > 2; }); // [3]

// reduce
array_reduce([1,2,3], function ($acc, $n) { return $acc + $n; }, 0); // (1+2+3) == 6
```

Bedziesz uzywal tych funkcji bardzo czesto, i omowimy je lepiej w nastepnych sekcjach.  
  

Notka: uwaga. W Python `reduce` zostalo przesuniete do biblioteki `functools`!


## Declarative code

Piszac paradygmacie FP bedziemy (zupelnie jak jezyki funkcyjne) uzywac wylacznie deklaratywnego, raczej niz imperatywnego kodu. Jaka jest roznica pomiedzy stylem imperatywnym a deklaratywnym (imperative vs declarative)? Najprosciej rzecz ujmujac, imperatywny styl zmusza programiste do pisania w stylu JAK (HOW). Natomiast deklaratywny styl pozwala na pisanie w stylu CO (WHAT). A wiec nie "jak mam to zrobic?", ale "co mam zrobic?" (gdyby komputer nas pytal 😉).  
  
Wysmienitym przykladem jest uzywanie funkcji `map`, ktora jest deklaratywna alternatywa do petli typu `for` (for loop), i wyglada nastepujaco:

```php
array_map(function ($n) { return $n * $n; }, [1,2,3]); // [1,4,9]
```

`map` bierze liste liczb (`[1,2,3]`), i aplikuje anonimowa funkcje (`function`) do kazdej z nich. Dla celow wylacznie edukacyjnych, tu jest kod, ktory dokona tego samego, ale z uzyciem nazwanej funkcji w miejscu anonimowej):

Nie interesuje nas JAK `map` zmienia kazdy element podanej listy. Obchodzi nas jedynie, CO ma zostac zrobione.  
  
Gdybysmy pisali to w stylu imperatywnym, musielibysmy zrobic to pewnie tak:

```php
$arr = [1,2,3];
$arr2 = [];

for ($x = 0; $x < count($arr); $x++) {
  array_push($arr2, $arr[$x] * $arr[$x]);
}

print($arr2);  // [1,4,9]
```
  
Deklaratywny styl pozwala na pisanie mniej kodu, ktory w dodatku jest bardziej przejrzysty. Przejrzystosc i zwiezlosc gwarantuja mniej bugow, a takze ulatwiaja testowanie (zwlaszcza jednostkowe).



## Currying

Rozwijanie funkcji (lub 'currying') to proces konwertowania funkcji, ktora bierze wiele argumentow w funkcje, ktora bierze tylko jeden argument.  
  
Najlepiej pokaze to prosty przyklad, najpierw wykorzystujac pomocna biblioteke `functools` w Pythonie:

```python
from functools import partial

def add_nums(n1):
  return lambda n2: n1 + n2

# wywolujemy funkcje, dajac jej nie 2 argumenty na raz, ale 1 argument, i ponownie 1 argument
print (add_nums(2)(1)) # 3

# albo, uzywajac funkcji partial dla uzyskania czastkowej aplikacji

add_to_two = partial(add_nums, 2)
print(add_to_two(1)) # 3
```

W PHP rzecz jest troszke bardziej "zawila", choc sa prostsze sposoby, jak biblioteka [php-curry](https://github.com/matteosister/php-curry), ktora pozwala nam na currying bezbolesnie:

```php
use Cypress\Curry as C;

$adder = function ($a, $b, $c, $d) {
  return $a + $b + $c + $d;
};

$firstTwo = C\curry($adder, 1, 2);
echo $firstTwo(3, 4); // output 10

$firstThree = $firstTwo(3);
echo $firstThree(14); // output 20
```

Wersja vanilla PHP wygladalaby natomiast tak:

```php
function curry($f, ...$argsCurried)
{
    return function (...$args) use ($f, $argsCurried) {
        return $f(...$argsCurried, ...$args);
    };
}

function add($n1, $n2)
{
    return $n1 + $n2;
}

$addFive = curry('add', 5);

echo $addFive(2), PHP_EOL;
```

W Ruby currying jest dosc latwy:

```ruby
adder = -> a, b { a + b }
add_two = adder.curry.(2)
add_two.(5) # => 7
```


## Recursion

Recursion to forma loopowania w FP. Funkcja rekursywna wywoluje sama siebie az jej kod jej nie zatrzyma.  
  
Typowy przyklad to funkcja `factorial`:

```python
def factorial(n):
    if n>1:
        x = n*factorial(n-1)
    else:
        x = 1
    return x

print(factorial(6))  # 6*5*4*3*2*1 = 720
```


## Popularne operacje na kolekcjach

Jezyki funkcyjne typowo maja w swoich podstawowych bibliotekach zbior funkcji do pracy na kolekcjach. Jezyki z funkcyjnym paradygmatem czesto nie sa inne.

## head i tail

Jesli dzialamy na arrays to myslimy w kontekscie indeksow. Ale FP zazwyczaj uzywala polaczonych list (linked lists), gdzie ostatni element trzyma referencje na temat elementu po lewej rece. Ten po lewej trzyma referencje do kolejnego elementu po jego lewej stronie, itd. W funkcyjnych jezykach jest to bardzo latwo zobrazowac. Wezmy np. backendowy Elixir (powiedzmy, dziecko papy Erlanga i mamy Ruby):

```elixir
# pusta lista
[]

# dodajmy 1 (mozemy to zrobic w ten sposob tylko z lewej strony ze wzgledu na to, jak zbudowane sa polaczone listy)
[1 | []]

# teraz dodajmy 2
[2 | [1]]

# albo zobaczmy jak naprawde wyglada teraz lista
[2 | [1 | []]]

# mozemy pojsc o jeden krok dalej dla odrobiny wprawy :)
[3 | [2 | [1 | []]]]

# prosciej
[3 | [2, 1]]

# i oczywiscie ekwiwalentem dwoch powyzszych jest najbardziej dla nas zrozumialy zapis
[3, 2 ,1]
```

Warto to wiedziec. Teraz, pomimo tego, ze np. Ruby uzywa arrays, a nie lists (a Python pod nazwa "lista" ukrywa tak naprawde "array"), to istnieje mozliwosc brania `head` (pierwszego elementu) oraz `tail` (pozostalych elementow):

```ruby
head, *tail = [1,2,3];
head  # => 1
tail  # => [2,3]
```

W Pythonie kod wyglada dokladnie tak samo jak w Ruby.  
Niestety, PHP nie ma tak latwego dostepu do tych dwoch funkcji.

## zip

```javascript
// zip
// laczy ze soba dwie listy w krotki (tuples). JS nie ma krotek, wiec otrzymujemy array of arrays.
R.zip([1, 2, 3], ['a', 'b', 'c']); //=> [[1, 'a'], [2, 'b'], [3, 'c']]


// take
// bierze liczbe elementow z listy zaczynajac od lewej
R.take(2, ['foo', 'bar', 'baz']); //=> ['foo', 'bar']


// drop
// przeciwienstwo 'take'. Odrzuca liczbe elementow z listy zaczynajac od lewej
R.drop(2, ['foo', 'bar', 'baz']); //=> ['baz']


// all
// sprawdza, czy wszystkie elementy listy zgadzaja sie z predykatem
R.all(R.equals(3))([3, 3, 1, 3]); //=> false


// any
// sprawdza, czy ktorykolwiek z elementow listy zgadza sie z predykatem
R.any(n => n < 0)([1, 2]); //=> false
R.any(R.flip(R.lt)(0))([1, 2]); //=> false
R.any(R.lt(0))([1, 2]); //=> true


// includes
// sprawdza, czy element jest w liscie/string
R.includes(3, [1, 2, 3]); //=> true
R.includes(4, [1, 2, 3]); //=> false
R.includes({ name: 'Fred' }, [{ name: 'Fred' }]); //=> true
R.includes([42], [[42]]); //=> true
R.includes('ba', 'banana'); //=>true
```


## Pipe(line) operator

Funkcyjne jezyki czesto pozwalaja na 'pipe'owanie' danych poprzez funkcje. Imperatywne jezyki, niestety, bardzo rzadko posiadaja `pipe` operator zaimplementowany, dlatego zaczniemy wyjasnienia od pokazania operatora w Elixirze. Operator wyglada tak: `|>`, i "wklada" dane z lewej strony do funkcji pop prawej stronie jako pierwszy argument funkcji. Oto przyklad:  
  
```elixir
[1,2,3]
|> Enum.random
|> to_string  # "3" (moze byc inny int :))
```

Pipe jest o tyle fajny, ze pozwala pipe'owac dane niezliczona ilosc razy przez kolejne funkcje. Jako, ze funkcyjne jezyki (a poprzez to, i FP) najlepiej nadaja sie do pracy nad danymi, raczej anizeli komputacja, to ow operator przydaje sie bardzo czesto.  
  
Teraz, co mozemy zrobic w PHP, Pythonie, i Ruby? Mozemy sprobowac siegnac po implementacje.  

- [Python](https://hackernoon.com/adding-a-pipe-operator-to-python-19a3aa295642)
- [PHP](https://github.com/sebastiaanluca/php-pipe-operator)

Szczesliwie dla Rubyistow, Ruby ma juz natywna implementacje operatora: dobry artykul na ten temat na dev.to jest [tutaj](https://dev.to/baweaver/ruby-2-7-the-pipeline-operator-1b2d).


# Przykladowe uzycia


### Dodawanie do Listy

Dodawanie elementu do listy zawsze stworzy nowa liste.  
  
Polaczmy dwa arrays w PHP:

```php
array_merge([1], [2,3])  // [1,2,3]  <= nowy array
```



### Usuwanie z Listy

Usuwanie z listy wymaga uzycia funkcji `filter`. Filter pozwala nam 'filtrowac' elementy w liscie, i wybrac jedynie te, o ktore nam chodzi.   
Filtrowanie odbywa sie za pomoca anonimowej funkcji (lub nazwanej), ktora przyjmuje jeden argument (element z listy), a produkuje `boolean`.
Funkcja bedzie wywolana dla kazdego elementu listy.

W ponizszym przykladzie uzywamy operatora `(!==)`, a wiec 'nie rowna sie', i 'usuwamy' z listy liczbe `1`.

```php
array_filter(function ($n) { return $n !== 1; }, [1,2,3])  // [2,3]
```

Mozemy smialo usunac z listy wiecej niz jeden element.  
  
W przykladzie ponizej wybieramy jedynie liczby, ktore rownaja sie 3 lub sa wieksze (operator `(>=)`).

```php
array_filter(function ($n) { return $n >= 3; }, [1,2,3,4,5])  // [3,4,5]
```


### Update'owanie Listy

Czasem musimy zmienic konkretny element (lub elementy) w liscie lub zmienic nawet wszystkie elementy. W funkcyjnych jezykach do transformacji list sluzy nam funkcja `map`.  
  
Przyklad update'u wszystkich elementow listy:

```php
array_map(function ($n) { return $n + $n; }, [1,2,3]);  // [2,4,6]
```

```python
people = [
  dict(name = "Tom", age = 22), 
  dict(name = "Dick", age = 33), 
  dict(name = "Mortimer", age = 44)
]

def inc_age(p):
    p['age'] = p['age'] + 1
    return p

list(map(inc_age, people))  
# [
#  {'name': 'Tom', 'age': 23}, 
#  {'name' = "Dick", 'age' = 34}, 
#  {'name' = "Mortimer", 'age' = 45}
# ]
```

Przyklad zmiany jedynie osoby `Tom`.

```python
people = [
  dict(name = "Tom", age = 22), 
  dict(name = "Dick", age = 33), 
  dict(name = "Mortimer", age = 44)
]

def inc_age(p):
    if p['name'] == 'Tom':
      p['age'] = p['age'] + 1

    return p

list(map(inc_age, people))  
# [
#  {'name': 'Tom', 'age': 23}, 
#  {'name' = "Dick", 'age' = 33}, 
#  {'name' = "Mortimer", 'age' = 44}
# ]
```


### Wybieranie z Listy

Wybieranie z listy jest podobne do usuwania z listy. Zasadniczo jest to to samo (tez uzywamy funkcji `filter`). Postanowilem rozpisac to na dwa koncepty, bo tak dyktuje mi doswiadczenie w uczeniu nowych funkcyjnych programistow 😄.

```python
list(filter(lambda name: name == 'John', ['Mary', 'Yousef', 'John']))  # ['John']
```

Notka: Zauwaz prosze, ze wybierajac cos z listy, otrzymujemy nie sam element, ale liste z elementem (lub wieloma elementami). Zeby dostac tylko ten jeden element, musimy albo uzyc indeksu (jesli to array), albo uzyc funkcji `head`.

Dla zobrazowania w Elixirze:

```elixir
['Mary', 'Yousef', 'John']
|> Enum.filter(fn name -> name == 'John' end)
|> hd()  # head

# rezultat: 'John'
```



# Cwiczenia

Do cwiczen polecam uzyc
- wybrany online REPL (np. [repl.it](https://repl.it/)) w swoim ulubionym jezyku, lub
- lokalny REPL, albo 
- pisac w pliku :)

```json
// Dane w formacie JSON
// ====================
[
  {"name": "John", "age": 10},
  {"name": "Jack", "age": 20},
  {"name": "Jake", "age": 25},
  {"name": "Jesse", "age": 39},
  {"name": "Jill", "age": 54}
]
```

```
// Cwiczenie 1:
// ============
// 1. Zwroc head oraz tail (w formie zmiennych) z array [1,2,3].
// 2. Napisz czysta funkcje, ktora zwroci nowy array w takiej postaci:
// [1, [2,3]] (zakladajac, ze input to [1,2,3])
// Jesli Twoj jezyk nie pozwala na 1., zrob tylko 2.

// Cwiczenie 2:
// ============
// Napisz czysta funkcje `sum`, ktora przyjmuje array z liczbami,
// a zwraca integer, ktory jest suma wszystkich liczb z array.
// Zobacz, czy dasz rade zbudowac te funkcje na wiecej, 
// niz jeden tylko sposob :)

// Cwiczenie 3:
// ============
// Zbuduj nowy array zwracajac jedynie imie pierwszej osoby.
// Nastepnie, zwroc tylko samo imie.

// Cwiczenie 4:
// ============
// Uzyj `map` oraz `filter`, zeby postarzec kazda z osob o 1 rok, 
// a nastepnie zwrocic tylko tych, ktorzy maja 25 lat lub mniej.

// Cwiczenie 5:
// ============
// Uzyj `map` do stworzenia nowego array, ktorego kazdy 
// element bedzie wygladal tak: [name, age] (np. ["John", 10]).

// Cwiczenie 6:
// ============
// 1. Zbuduj nowy array jedynie z osob o wieku 10, 25, i 54. 
// 2. Uzyj funkcji `reduce`, zeby zsumowac wiek ludzi.
//
// Poprawny wynik: 89

// Cwiczenie 7:
// ============
// Uzyj operatora `pipe` (lub najlepszej, podobnej metody w Twoim jezyku), zeby
// - zsumowac array [2,3]
// - dodac do niego 3
// - zwiekszyc o jeden
// - a ostatecznie zanegowac (tzn. zmienic z dodatniego na ujemny)
//
// Poprawny wynik: -9
```


# Przyklady uzycia funkcyjnego paradygmatu

Reasumujac, kazdy z trzech jezykow (gdy porownamy go z funkcyjnymi jezykami) pozwala poslugiwac sie imperatywnym kodem, ktory niekoniecznie pomaga nam napisac bezpieczny, pewny, mozliwie zwiezly program. Dzis, smiem twierdzic :), z pomoca przychodzi nam programowanie funkcyjne. A zatem, FP zacheca nas do trzymania sie nastepujacych, podstawowych zasad, ktore powinnismy powtarzac jak mantre:
- trzymac sie z dala od mutacji
- trzymac sie z dala od efektow ubocznych
- trzymac sie z dala od stanu obiektow czy funkcji

Lekarstwem na powyzsze problemy jest trzymanie sie nastepujacych zasad:
- piszmy czyste funkcje
- piszmy deklaratywny kod
- piszmy male funkcje, z ktorych mozemy komponowac wieksze funkcje, z ktorych z kolei bedzie skomponowany nasz program
- trzymajmy sie niemutowalnych danych, a gdy chcemy cos zmienic, klonujmy kolekcje

## Mosty do innych jezykow

- Ruby do Elixir
- C# do F#
- Java do Scala
- JS do Elm
- mozliwe inne przejscia
  - np. Python do Elixir
  - PHP do Elixir (zwlaszcza z frameworku Laravel do frameworku [Phoenix](https://www.phoenixframework.org/))


# Cwiczenia 2

- Zbuduj malutka appke, ktora pracuje na kolekcjach (czytanie, tworzenie, edycja, usuwanie, wybieranie, sortowanie, itp.) starajac sie uzyc jedynie konceptow FP.
  - Krok 1: stworz prosty plan
  - Krok 2: uzyj swojego ulubionego frameworku (albo vanilla code), w ktorym jestes najproduktywniejsza/y (np. Flask, Sinatra); To moze byc appka z HTML albo czyste JSON/GraphQL API, albo cos innego, jesli masz sprytniejszy pomysl :)
  - Krok 3: pomysl o malych, czystych funkcjach
  - Krok 4: jak utrzymasz stan appki i jak bedziesz go zmieniac? Pamietaj o niemutowalnosci danych.
  - Krok 5: zacznij budowac! :)
  - Krok 6: przed koncem warsztatu zapreentuj swoja appke, opowiadajac w kilku zdaniach o swoich pomyslach i wyborach

Smialo konsultuj/pytaj ze mna lub/i innymi uczestnikami warsztatu przez Zoom lub/i na Slacku w trakcie trwania cwiczenia!  



# Uzyteczne linki

## PHP
* [Functional Programming and PHP](https://www.sitepoint.com/functional-programming-and-php/)
* [Mozliwa implementacja `tail`, choc na wzor UNIXowy](https://tekkie.dev/php/tail-functionality-in-php)

## Python
* [Pure functions](https://www.pythoninformer.com/programming-techniques/functional-programming/pure-functions/)
* [Map/reduce](https://www.pythoninformer.com/programming-techniques/functional-programming/map-reduce-example/)

## Ruby
* [Functional programming](https://womanonrails.com/functional-programming-ruby)
* [Functional Programming In Ruby (Complete Guide)](https://www.rubyguides.com/2018/01/functional-programming-ruby/)
* [Functional Programming in Ruby for people who don’t know what functional programming is](https://medium.com/@heyamberwilkie/functional-programming-in-ruby-for-people-who-dont-know-what-functional-programming-is-f0c4ab7dc68c)

## Artykuly
* [Why OO Sucks by Joe Armstrong](http://harmful.cat-v.org/software/OO_programming/why_oo_sucks)
* [Idiom: First-class function : generic composition](https://www.programming-idioms.org/idiom/36/first-class-function-generic-composition)

