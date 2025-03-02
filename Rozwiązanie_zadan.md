# Rozwiązanie zadań

**Zadanie 1. Stwórz klasę Time zajmującą się obsługą czasu. Dodaj w niej dwa pola (zmienna) o nazwie hour i minute do przechowywania godzin i minut.**
**W klasie dodaj następujące metody:**
**- funkcja, która zwraca wartość logiczną sprawdzającą czy zawartość pól może być poprawną godziną w formacie 24-godzinnym**
**- funkcja, która odpowiada za dodawanie dwóch obiektów typu Time. W środku należy dodać godziny do godzin, minuty do minuty.**
**Jeśli minuty są większe lub równe niż 60, to należy odpowiednie zwiększyć pole godzin i pomniejszyć o 60 minuty.**
**Jeśli godziny są większe lub równe niż 24, to należy od godzin odjąć 24. Funkcja ma zwrócić obiekt typu Time przechowujący “sumę”.**
**- dodaj odpowiednią metodę, która odpowiada za sortowanie obiektów typu Time (wg dowolnego wybranego przez siebie klucza)
Następnie stwórz co najmniej 3 obiekty i wywołaj na nich każdą z funkcji co najmniej 1 raz.**

```python
class Time:

    def __init__(self, hour, minute):
        self.hour = hour
        self.minute = minute

    def is_valid(self):
        return 0 <= self.hour < 24 and 0 <= self.minute < 60

    def __add__(self, other):
        if isinstance(other, Time):
            new_hour = self.hour + other.hour
            new_minute = self.minute + other.minute
            if new_minute >= 60:
                new_hour += new_minute // 60
                new_minute %= 60
            new_hour %= 24
            return Time(new_hour, new_minute)
        return NotImplemented

    def __lt__(self, other):
        return (self.hour, self.minute) < (other.hour, other.minute)

    def __str__(self):
        return f"{self.hour:02d}:{self.minute:02d}"


time1 = Time(23, 50)
time2 = Time(1, 30)
time3 = Time(12, 15)
print("Poprawność godzin:", time1.is_valid(), time2.is_valid(), time3.is_valid())
print("Suma czasów:", time1 + time2)
print("Sortowanie czasów:", sorted([time1, time2, time3]))
```

**Zadanie 2. Stwórz klasę Movie z polami przechowującymi tytuł, rok oraz ocenę filmu.
Dodaj w klasie konstruktor (metodę __init__) ustawiającą wszystkie pola tej klasy.
Następnie stwórz listę zawierającą co najmniej 10 obiektów tego typu. 
Posortuj listę wg różnych klucz na co najmniej 5 różnych sposobów. W komentarzach umieść informację o zasadach sortowania.**

```python
class Movie:
    def __init__(self, title, year, rating):
        self.title = title
        self.year = year
        self.rating = rating

    def __str__(self):
        return f"{self.title} ({self.year}) - Ocena: {self.rating}"

movies = [
    Movie("Inception", 2010, 8.8),
    Movie("The Matrix", 1999, 8.7),
    Movie("Interstellar", 2014, 8.6),
    Movie("Pulp Fiction", 1994, 8.9),
    Movie("The Godfather", 1972, 9.2),
    Movie("Shrek", 2001, 7.8),
    Movie("Forrest Gump", 1994, 8.8),
    Movie("Fight Club", 1999, 8.8),
    Movie("The Dark Knight", 2008, 9.0),
    Movie("Gladiator", 2000, 8.5),
]
print("Sortowanie według roku:", [str(m) for m in sorted(movies, key=lambda m: m.year)])
print("Sortowanie według oceny:", [str(m) for m in sorted(movies, key=lambda m: m.rating)])
print("Sortowanie według tytułu:", [str(m) for m in sorted(movies, key=lambda m: m.title)])
print("Sortowanie według długości tytułu:", [str(m) for m in sorted(movies, key=lambda m: len(m.title))])
print("Sortowanie według ostatniej cyfry roku:", [str(m) for m in sorted(movies, key=lambda m: m.year % 10)])
```

**Zadanie 3. Napisz klasę Vector2D, która będzie reprezentować wektor dwuwymiarowy. 
Klasa powinna zawierać metody magiczne __add__ oraz __radd__, aby umożliwić dodawanie dwóch wektorów 2D oraz dodawanie krotek (x, y) do wektorów.**

```python
class Vector2D:

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        if isinstance(other, Vector2D):
            return Vector2D(self.x + other.x, self.y + other.y)
        elif isinstance(other, tuple) and len(other) == 2:
            return Vector2D(self.x + other[0], self.y + other[1])
        return NotImplemented

    def __radd__(self, other):
        return self.__add__(other)

    def __str__(self):
        return f"({self.x}, {self.y})"


# Testy dla Vector2D
v1 = Vector2D(3, 4)
v2 = Vector2D(1, 2)
print("Suma wektorów:", v1 + v2)
print("Dodanie krotki do wektora:", v1 + (2, 3))
```

**Zadanie 4. Napisz klasę Polynomial, która będzie reprezentować wielomian jednej zmiennej.**
**Klasa powinna zawierać metody magiczne __add__, __radd__, __getitem__ oraz __setitem__,**
**aby umożliwić dodawanie dwóch wielomianów, dodawanie liczby rzeczywistej do wielomianu,**
**uzyskiwanie współczynników wielomianu oraz ustawianie wartości współczynników.**

```python
class Polynomial:

    def __init__(self, coefficients):
        self.coefficients = list(coefficients)  # Upewniamy się, że to lista

    def __add__(self, other):
        if isinstance(other, Polynomial):
            max_len = max(len(self.coefficients), len(other.coefficients))
            new_coeffs = [0] * max_len
            for i in range(max_len):
                if i < len(self.coefficients):
                    new_coeffs[i] += self.coefficients[i]
                if i < len(other.coefficients):
                    new_coeffs[i] += other.coefficients[i]
            return Polynomial(new_coeffs)
        elif isinstance(other, (int, float)):  # Dodawanie liczby
            new_coeffs = self.coefficients[:]
            new_coeffs[0] += other  # Dodajemy do stałej
            return Polynomial(new_coeffs)
        return NotImplemented

    def __radd__(self, other):
        return self.__add__(other)  # Symetryczne dodawanie liczby

    def __getitem__(self, index):
        if 0 <= index < len(self.coefficients):
            return self.coefficients[index]
        return 0  # Jeśli współczynnik nie istnieje, traktujemy jako 0

    def __setitem__(self, index, value):
        if index >= len(self.coefficients):  # Rozszerzamy listę, jeśli potrzeba
            self.coefficients.extend([0] * (index - len(self.coefficients) + 1))
        self.coefficients[index] = value

    def __str__(self):
        return " + ".join(f"{c}x^{i}" for i, c in enumerate(self.coefficients) if c)


p1 = Polynomial([1, 2, 3])
p2 = Polynomial([0, 1, 4])
print("Suma wielomianów:", p1 + p2)
print("Współczynnik x^1 w p1:", p1[1])
p1[1] = 5
print("Nowy wielomian p1:", p1)
```

**Zadanie 5. Napisz klasę Playlist, która będzie reprezentować listę odtwarzania utworów muzycznych.
Klasa powinna zawierać metody magiczne __add__, __radd__, __getitem__ oraz __setitem__,
aby umożliwić łączenie dwóch list odtwarzania, dodawanie utworu do listy odtwarzania,
uzyskiwanie utworów z listy odtwarzania oraz ustawianie wartości utworów na liście odtwarzania.**

```python
class Playlist:

    def __init__(self):
        self.songs = []

    def __add__(self, other):
        if isinstance(other, Playlist):
            new_playlist = Playlist()
            new_playlist.songs = self.songs + other.songs
            return new_playlist
        elif isinstance(other, str):
            new_playlist = Playlist()
            new_playlist.songs = self.songs + [other]
            return new_playlist
        return NotImplemented

    def __radd__(self, other):
        return self.__add__(other)  # Obsługuje "Song X" + Playlist()

    def __getitem__(self, index):
        return self.songs[index]

    def __setitem__(self, index, value):
        self.songs[index] = value

    def __str__(self):
        return "\n".join(self.songs)


pl1 = Playlist()
pl2 = Playlist()
pl1 += "Song A"
pl2 += "Song B"
merged = pl1 + pl2
print("Połączona playlista:\n", merged)
print("Pierwsza piosenka:", merged[0])
merged[0] = "New Song"
print("Po zmianie:", merged)
```

**Zadanie 6. Napisz klasę Car, która będzie reprezentować samochód. 
Klasa powinna zawierać atrybuty instancyjne make, model, year, mileage oraz price, inicjalizator, 
właściwości dla mileage oraz price (umożliwiające odczyt i zapis z walidacją wartości) oraz metody magiczne __str__ i __repr__. 
Dodatkowo, klasa powinna posiadać metody drive (aktualizującą przebieg samochodu) oraz calculate_depreciation (obliczającą spadek wartości samochodu na podstawie jego przebiegu i wieku).**

```python
from functools import total_ordering

class Car:

    def __init__(self, make, model, year, mileage, price):
        self.make = make
        self.model = model
        self.year = year
        self._mileage = mileage  # Używamy _ do prywatnych atrybutów
        self._price = price

    @property
    def mileage(self):
        return self._mileage

    @mileage.setter
    def mileage(self, value):
        if value < self._mileage:
            raise ValueError("Przebieg nie może maleć!")
        self._mileage = value

    @property
    def price(self):
        return self._price

    @price.setter
    def price(self, value):
        if value < 0:
            raise ValueError("Cena nie może być ujemna!")
        self._price = value

    def drive(self, distance):
        self.mileage += distance

    def calculate_depreciation(self):
        age = 2025 - self.year
        return max(self._price * (0.8 ** age), 1000)  # Minimalna wartość 1000

    def __str__(self):
        return f"{self.make} {self.model} ({self.year}) - {self.mileage} km, ${self.price}"


car = Car("Toyota", "Corolla", 2015, 120000, 10000)
car.drive(500)
print("Po jeździe:", car)
print("Wartość po amortyzacji:", car.calculate_depreciation())
```

**Zadanie 7. Napisz klasę Samochod, która będzie mieć następujące atrybuty instancyjne:
marka - marka samochodu
model - model samochodu
rok_produkcji - rok produkcji samochodu
przebieg - przebieg samochodu
Klasa powinna mieć następujące metody:
__init__(self, marka, model, rok_produkcji, przebieg) - konstruktor, który będzie inicjalizował atrybuty marka, model, rok_produkcji i przebieg
__str__(self) - metoda magiczna, która będzie zwracać reprezentację napisową obiektu klasy Samochod
__lt__(self, other) - metoda magiczna, która będzie porównywać dwa samochody po ich przebiegu. Metoda ma zwracać True, jeśli przebieg samochodu self jest mniejszy niż przebieg samochodu other, a w przeciwnym wypadku False.**

```python
class Samochod:

    def __init__(self, marka, model, rok_produkcji, przebieg):
        self.marka = marka
        self.model = model
        self.rok_produkcji = rok_produkcji
        self.przebieg = przebieg

    def __str__(self):
        return f"{self.marka} {self.model} ({self.rok_produkcji}) - {self.przebieg} km"

    def __repr__(self):
        return f"Samochod('{self.marka}', '{self.model}', {self.rok_produkcji}, {self.przebieg})"

    def __lt__(self, other):
        return self.przebieg < other.przebieg


# Testy dla Samochod
s1 = Samochod("Audi", "A4", 2018, 60000)
s2 = Samochod("BMW", "X5", 2020, 30000)
print("Czy Audi ma mniejszy przebieg niż BMW?:", s1 < s2)
```
