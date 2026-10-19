# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 04 — DOM i formularze HTML

---

## 1. Cel zajęć

Na poprzednich zajęciach utworzyliśmy pierwszą statyczną stronę projektu **Biblioteka**.

Dzisiaj rozbudujemy ją o elementy, które pozwalają użytkownikowi **wprowadzać dane**.

Po zakończeniu laboratorium powinniśmy:

- rozumieć, czym jest **DOM**,
- potrafić odnaleźć element strony w narzędziach deweloperskich,
- rozumieć relacje rodzic–dziecko pomiędzy elementami HTML,
- potrafić utworzyć formularz HTML,
- znać podstawowe elementy formularzy,
- rozumieć rolę atrybutów `name`, `value`, `required`, `min`, `max`,
- potrafić przygotować formularz wyszukiwania działający metodą `GET`,
- wiedzieć, kiedy formularz powinien korzystać z `POST`,
- przygotować statyczny formularz „Dodaj książkę”, który później wykorzystamy w Django.

> [!CAUTION]
> **Biblioteka, katalog książek oraz system wypożyczania książek są tematami niedozwolonymi w projekcie zaliczeniowym.**
>
> Biblioteka jest wspólnym projektem ćwiczeniowym rozwijanym podczas laboratoriów.

---

# 2. Początek pracy

Na początku pobierz najnowszą wersję projektu:

```bash
git pull
```

Jeżeli pracujesz na nowym komputerze:

```bash
git clone ADRES_REPOZYTORIUM
cd paw_biblioteka
```

Przejdź do katalogu:

```bash
cd frontend
```

Uruchom prosty serwer:

```bash
python -m http.server 8000
```

lub:

```bash
py -m http.server 8000
```

Otwórz:

```text
http://127.0.0.1:8000/
```

---

# 3. Przykładowe rozwiązania z poprzedniego laboratorium

Na końcu `lab03` znajdowało się sześć nieobowiązkowych zadań.

Poniżej znajduje się jedno z możliwych rozwiązań każdego z nich.

Nie są to jedyne poprawne rozwiązania.

---

## Zadanie 1 — godziny otwarcia

Do `frontend/index.html` można dodać:

```html
<section id="opening-hours">
    <h2>Godziny otwarcia</h2>

    <ul>
        <li>Poniedziałek – 8:00–16:00</li>
        <li>Wtorek – 8:00–16:00</li>
        <li>Środa – 10:00–18:00</li>
        <li>Czwartek – 10:00–18:00</li>
        <li>Piątek – 8:00–14:00</li>
    </ul>
</section>
```

W nawigacji możemy również dodać:

```html
<a href="#opening-hours">Godziny otwarcia</a>
```

---

## Zadanie 2 — kontakt i nawigacja

Przykładowa sekcja:

```html
<section id="contact">
    <h2>Kontakt</h2>

    <p>Biblioteka Akademicka</p>
    <p>ul. Przykładowa 1</p>
    <p>10-000 Olsztyn</p>
</section>
```

W `<nav>`:

```html
<a href="#contact">Kontakt</a>
```

Po kliknięciu linku strona przewinie się do elementu posiadającego:

```html
id="contact"
```

---

## Zadanie 3 — dodatkowe książki

Przykładowo:

```html
<article>
    <h3>Mały Książę</h3>

    <div class="book-info">
        <p>
            <strong>Autor:</strong>
            Antoine de Saint-Exupéry
        </p>

        <p>
            <strong>Rok wydania:</strong>
            1943
        </p>
    </div>

    <p>
        Opowieść o podróży Małego Księcia i jego spotkaniach
        z mieszkańcami różnych planet.
    </p>
</article>

<article>
    <h3>1984</h3>

    <div class="book-info">
        <p>
            <strong>Autor:</strong>
            George Orwell
        </p>

        <p>
            <strong>Rok wydania:</strong>
            1949
        </p>
    </div>

    <p>
        Dystopijna powieść przedstawiająca społeczeństwo
        kontrolowane przez totalitarne państwo.
    </p>
</article>
```

---

## Zadanie 4 — rozszerzenie tabeli

Przykładowa tabela z dodatkową kolumną:

```html
<table>
    <thead>
        <tr>
            <th>Tytuł</th>
            <th>Autor</th>
            <th>Rok</th>
            <th>Dostępna</th>
        </tr>
    </thead>

    <tbody>
        <tr>
            <td>Solaris</td>
            <td>Stanisław Lem</td>
            <td>1961</td>
            <td>Tak</td>
        </tr>

        <tr>
            <td>Lalka</td>
            <td>Bolesław Prus</td>
            <td>1890</td>
            <td>Nie</td>
        </tr>

        <tr>
            <td>Hobbit</td>
            <td>J.R.R. Tolkien</td>
            <td>1937</td>
            <td>Tak</td>
        </tr>

        <tr>
            <td>Mały Książę</td>
            <td>Antoine de Saint-Exupéry</td>
            <td>1943</td>
            <td>Tak</td>
        </tr>

        <tr>
            <td>1984</td>
            <td>George Orwell</td>
            <td>1949</td>
            <td>Nie</td>
        </tr>
    </tbody>
</table>
```

---

## Zadanie 5 — obraz lokalny

Dodajmy przykładowy obraz biblioteki (np. takiej z SWPS we Wrocławiu):

![alt text](library.jpg)

Przykładowa struktura katalogów:

```text
frontend/
│
├── images/
│   └── library.jpg
│
└── index.html
```

W HTML:

```html
<img
    src="images/library.jpg"
    alt="Wnętrze biblioteki"
>
```

Jeżeli wpiszemy błędną nazwę pliku:

```html
<img
    src="images/nieistniejacy.jpg"
    alt="Wnętrze biblioteki"
>
```

przeglądarka nie będzie mogła wczytać obrazu.

W takiej sytuacji znaczenie ma m.in. poprawnie przygotowany:

```text
alt
```

---

## Zadanie 6 — własna wersja strony

Przykładowy temat:

> **Planer nauki**

Utwórz plik:

```text
frontend/practice.html
```

Przykładowe kompletne rozwiązanie:

```html
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Planer nauki</title>
</head>

<body>

    <header>
        <h1>Planer nauki</h1>

        <nav>
            <a href="#subjects">Przedmioty</a>
            <a href="#tasks">Zadania</a>
        </nav>
    </header>

    <main>

        <section id="subjects">
            <h2>Przedmioty</h2>

            <article>
                <h3>Programowanie</h3>

                <p>
                    Ćwiczenia z podstaw programowania
                    oraz aplikacji webowych.
                </p>
            </article>

            <article>
                <h3>Statystyka</h3>

                <p>
                    Powtórzenie materiału przed kolokwium.
                </p>
            </article>
        </section>

        <section id="tasks">
            <h2>Najbliższe zadania</h2>

            <article>
                <h3>Przygotować formularz HTML</h3>

                <p>
                    Termin: 15 października
                </p>
            </article>

            <article>
                <h3>Powtórzyć materiał ze statystyki</h3>

                <p>
                    Termin: 18 października
                </p>
            </article>
        </section>

    </main>

    <footer>
        <p>
            Przykładowy projekt ćwiczeniowy
        </p>
    </footer>

</body>
</html>
```

To rozwiązanie zawiera wymagane:

```text
header
nav
main
2 x section
co najmniej 2 x article
footer
```

i pokazuje, że ta sama struktura HTML może zostać wykorzystana dla zupełnie innego rodzaju aplikacji.

---

## Zapisanie rozwiązań w Git

Jeżeli zadania zostały wykonane w projekcie:

```bash
git status
git add .
git commit -m "Rozwiązanie zadań z lab03"
git push
```

Od kolejnych laboratoriów będziemy stosowali tę samą zasadę:

> **Jeżeli poprzedni lab kończy się zadaniami programistycznymi, na początku następnego pojawia się kompletne przykładowe rozwiązanie.**

---

# 4. Czym jest DOM?

DOM oznacza:

> **Document Object Model**

Przeglądarka po wczytaniu pliku HTML tworzy w pamięci strukturę dokumentu.

Dla kodu:

```html
<body>
    <main>
        <section>
            <h2>Książki</h2>
            <p>Lista książek.</p>
        </section>
    </main>
</body>
```

możemy wyobrazić sobie strukturę:

```text
body
└── main
    └── section
        ├── h2
        └── p
```

Elementy tworzą więc **drzewo**.

---

# 5. Rodzic i dziecko

Dla:

```html
<section>
    <h2>Książki</h2>
    <p>Lista książek.</p>
</section>
```

element:

```html
<section>
```

jest **rodzicem** elementów:

```html
<h2>
<p>
```

Natomiast `h2` i `p` są jego **dziećmi**.

Takie zależności będą później istotne m.in. podczas:

- stylowania CSS,
- pracy z bardziej rozbudowanymi szablonami,
- analizowania struktury strony w DevTools.

Nie musimy uczyć się formalnego modelu DOM na pamięć.

Najważniejsze jest rozumienie, że HTML tworzy **hierarchiczną strukturę elementów**.

---

# 6. DOM w DevTools

Przy uruchomionej stronie:

1. naciśnij `F12`,
2. otwórz `Elements` lub `Inspector`,
3. znajdź `<body>`,
4. rozwiń `<main>`,
5. znajdź sekcję książek,
6. rozwiń jeden z elementów `<article>`.

Spróbuj wskazać element:

```html
<h3>Solaris</h3>
```

Następnie kliknij go dwukrotnie w DevTools i zmień tekst na:

```text
Solaris — wydanie specjalne
```

Zmiana pojawi się od razu na stronie.

Odśwież stronę.

Tekst wróci do poprzedniej wersji.

Dlaczego?

Ponieważ zmieniliśmy tylko **DOM istniejący aktualnie w pamięci przeglądarki**.

Nie zmieniliśmy pliku:

```text
index.html
```

---

# 7. Formularze HTML

Do tej pory użytkownik mógł głównie:

```text
czytać
klikać linki
przeglądać dane
```

Aplikacja webowa musi jednak często pozwalać również na:

```text
wprowadzanie danych
wyszukiwanie
logowanie
dodawanie
edytowanie
wysyłanie informacji
```

Do tego służą formularze.

Podstawowy formularz:

```html
<form>
    ...
</form>
```

W jego wnętrzu umieszczamy pola formularza.

---

# 8. Pierwsze pole tekstowe

Przykład:

```html
<input type="text">
```

Tworzy zwykłe pole tekstowe.

Znacznie lepiej połączyć je z opisem:

```html
<label for="title">Tytuł:</label>
<input type="text" id="title">
```

Atrybut:

```text
for="title"
```

w `label` wskazuje na element:

```text
id="title"
```

Dzięki temu kliknięcie tekstu:

```text
Tytuł:
```

ustawia kursor w odpowiednim polu.

---

# 9. Atrybut name

Bardzo ważny jest atrybut:

```html
name="title"
```

Pełny przykład:

```html
<label for="title">Tytuł:</label>

<input
    type="text"
    id="title"
    name="title"
>
```

`id` służy przede wszystkim do identyfikacji elementu w dokumencie.

`name` określa **nazwę danych wysyłanych przez formularz**.

Przykładowo:

```text
title=Solaris
```

oznacza:

```text
pole title ma wartość Solaris
```

Podczas pracy z Django atrybut `name` będzie bardzo istotny.

---

# 10. Formularz wyszukiwania — GET

Dodajmy do Biblioteki prosty formularz wyszukiwania.

W pliku:

```text
frontend/index.html
```

dodaj przed sekcją książek:

```html
<section id="search">
    <h2>Wyszukaj książkę</h2>

    <form method="get">
        <label for="query">Tytuł:</label>

        <input
            type="text"
            id="query"
            name="query"
            placeholder="np. Solaris"
        >

        <button type="submit">
            Szukaj
        </button>
    </form>
</section>
```

Zapisz plik i odśwież stronę.

Wpisz:

```text
Solaris
```

i kliknij:

```text
Szukaj
```

Spójrz na adres w przeglądarce.

Powinien wyglądać podobnie do:

```text
http://127.0.0.1:8000/?query=Solaris
```

---

# 11. Co właśnie się wydarzyło?

Formularz:

```html
<form method="get">
```

wysłał dane metodą:

```text
GET
```

Pole posiadało:

```html
name="query"
```

i wpisaliśmy wartość:

```text
Solaris
```

Dlatego w adresie pojawiło się:

```text
?query=Solaris
```

Czyli:

```text
nazwa pola = wartość pola
```

W przypadku kilku pól parametry mogą wyglądać np.:

```text
?query=Solaris&year=1961
```

Na razie prosty serwer nie wykonuje żadnego rzeczywistego wyszukiwania.

Później Django odczyta te parametry i użyje ich do filtrowania danych z bazy.

---

# 12. placeholder

W poprzednim przykładzie użyliśmy:

```html
placeholder="np. Solaris"
```

`placeholder` wyświetla podpowiedź wewnątrz pustego pola.

Przykład:

```html
<input
    type="text"
    name="query"
    placeholder="Wpisz tytuł książki"
>
```

Nie zastępuje jednak elementu:

```html
<label>
```

`label` nadal powinien informować użytkownika, czego dotyczy pole.

---

# 13. required

Jeżeli dane pole musi zostać wypełnione, możemy użyć:

```html
required
```

Przykład:

```html
<input
    type="text"
    id="title"
    name="title"
    required
>
```

Spróbuj wysłać pusty formularz.

Przeglądarka zablokuje wysłanie i poprosi o uzupełnienie pola.

Jest to pierwsza prosta forma **walidacji danych**.

Później zobaczymy, że aplikacja nie może polegać wyłącznie na walidacji wykonywanej przez przeglądarkę.

---

# 14. Pole liczbowe

Dla roku wydania możemy użyć:

```html
<input type="number">
```

Przykład:

```html
<label for="year">Rok wydania:</label>

<input
    type="number"
    id="year"
    name="year"
    min="0"
    max="2100"
>
```

Atrybuty:

```text
min
max
```

ograniczają zakres wartości.

---

# 15. Pole daty

Przykład:

```html
<label for="publish-date">Data wydania:</label>

<input
    type="date"
    id="publish-date"
    name="publish_date"
>
```

Przeglądarka może wyświetlić gotowy interfejs wyboru daty.

---

# 16. Pole email

Dla adresu email możemy użyć:

```html
<input type="email">
```

Przykład:

```html
<label for="email">E-mail:</label>

<input
    type="email"
    id="email"
    name="email"
    required
>
```

Przeglądarka sprawdzi podstawowy format adresu.

Takie pole wykorzystamy później przy przykładach związanych z użytkownikami.

---

# 17. Pole wyboru — select

Jeżeli użytkownik powinien wybrać jedną wartość z przygotowanej listy:

```html
<select>
```

Przykład:

```html
<label for="category">Kategoria:</label>

<select id="category" name="category">
    <option value="">Wybierz kategorię</option>
    <option value="fantasy">Fantasy</option>
    <option value="science-fiction">Science fiction</option>
    <option value="crime">Kryminał</option>
    <option value="other">Inna</option>
</select>
```

Wartość wysłana przez formularz pochodzi z:

```html
value="..."
```

Np.:

```html
<option value="fantasy">Fantasy</option>
```

oznacza:

```text
użytkownik widzi: Fantasy
formularz wysyła: fantasy
```

---

# 18. Checkbox

Pole typu:

```html
<input type="checkbox">
```

umożliwia zaznaczenie opcji typu:

```text
tak / nie
```

Przykład:

```html
<input
    type="checkbox"
    id="available"
    name="available"
>

<label for="available">
    Książka dostępna
</label>
```

Możemy używać checkboxów np. dla informacji:

```text
dostępna
aktywna
zaakceptowano regulamin
```

---

# 19. Radio

Jeżeli użytkownik ma wybrać dokładnie jedną opcję z kilku:

```html
<input type="radio">
```

Przykład:

```html
<p>Format:</p>

<input
    type="radio"
    id="paper"
    name="format"
    value="paper"
>

<label for="paper">
    Papierowa
</label>

<input
    type="radio"
    id="ebook"
    name="format"
    value="ebook"
>

<label for="ebook">
    E-book
</label>
```

Pola należą do jednej grupy dzięki temu samemu:

```text
name="format"
```

---

# 20. textarea

Dla dłuższego tekstu używamy:

```html
<textarea>
```

Przykład:

```html
<label for="description">Opis:</label>

<textarea
    id="description"
    name="description"
    rows="5"
    cols="40"
></textarea>
```

Może służyć np. do:

```text
opisu książki
komentarza
notatki
wiadomości
```

---

# 21. Przycisk

Najczęściej będziemy używali:

```html
<button type="submit">
    Zapisz
</button>
```

`submit` oznacza:

> Wyślij formularz.

Możemy również wykorzystać:

```html
<button type="reset">
    Wyczyść
</button>
```

który przywraca pola do wartości początkowych.

---

# 22. Formularz „Dodaj książkę”

Teraz przygotujemy większy formularz, który później wykorzystamy podczas pracy z Django.

Utwórz nowy plik:

```text
frontend/add_book.html
```

Wklej:

```html
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dodaj książkę — Biblioteka</title>
</head>

<body>

    <header>
        <h1>Biblioteka</h1>

        <nav>
            <a href="index.html">Strona główna</a>
            <a href="add_book.html">Dodaj książkę</a>
        </nav>
    </header>

    <main>

        <section>
            <h2>Dodaj książkę</h2>

            <form method="post">

                <div>
                    <label for="title">
                        Tytuł:
                    </label>

                    <input
                        type="text"
                        id="title"
                        name="title"
                        required
                    >
                </div>

                <div>
                    <label for="author">
                        Autor:
                    </label>

                    <input
                        type="text"
                        id="author"
                        name="author"
                        required
                    >
                </div>

                <div>
                    <label for="year">
                        Rok wydania:
                    </label>

                    <input
                        type="number"
                        id="year"
                        name="year"
                        min="0"
                        max="2100"
                    >
                </div>

                <div>
                    <label for="category">
                        Kategoria:
                    </label>

                    <select
                        id="category"
                        name="category"
                    >
                        <option value="">
                            Wybierz kategorię
                        </option>

                        <option value="fantasy">
                            Fantasy
                        </option>

                        <option value="science-fiction">
                            Science fiction
                        </option>

                        <option value="crime">
                            Kryminał
                        </option>

                        <option value="other">
                            Inna
                        </option>
                    </select>
                </div>

                <div>
                    <p>Format:</p>

                    <input
                        type="radio"
                        id="paper"
                        name="format"
                        value="paper"
                        checked
                    >

                    <label for="paper">
                        Papierowa
                    </label>

                    <input
                        type="radio"
                        id="ebook"
                        name="format"
                        value="ebook"
                    >

                    <label for="ebook">
                        E-book
                    </label>
                </div>

                <div>
                    <input
                        type="checkbox"
                        id="available"
                        name="available"
                        checked
                    >

                    <label for="available">
                        Książka dostępna
                    </label>
                </div>

                <div>
                    <label for="description">
                        Opis:
                    </label>

                    <textarea
                        id="description"
                        name="description"
                        rows="6"
                        cols="50"
                    ></textarea>
                </div>

                <div>
                    <button type="submit">
                        Dodaj książkę
                    </button>

                    <button type="reset">
                        Wyczyść formularz
                    </button>
                </div>

            </form>
        </section>

    </main>

    <footer>
        <p>
            Projekt ćwiczeniowy — Projektowanie aplikacji webowych
        </p>
    </footer>

</body>
</html>
```

---

# 23. Dodajemy link do formularza

W pliku:

```text
frontend/index.html
```

w nawigacji dodaj:

```html
<a href="add_book.html">
    Dodaj książkę
</a>
```

Przykładowo:

```html
<nav>
    <a href="#about">O bibliotece</a>
    <a href="#books">Książki</a>
    <a href="#rules">Jak wypożyczyć?</a>
    <a href="add_book.html">Dodaj książkę</a>
</nav>
```

Teraz możemy przechodzić pomiędzy:

```text
index.html
```

i:

```text
add_book.html
```

---

# 24. Dlaczego formularz POST jeszcze nie działa?

W formularzu użyliśmy:

```html
<form method="post">
```

Jest to poprawny kierunek dla formularza, który ma **dodawać dane**.

Problem polega na tym, że obecnie korzystamy z:

```bash
python -m http.server
```

Ten prosty serwer służy głównie do udostępniania plików statycznych.

Nie posiada logiki:

```text
odbierz dane formularza
sprawdź je
zapisz książkę
```

Po próbie wysłania formularza może pojawić się błąd informujący, że metoda `POST` nie jest obsługiwana.

To jest **oczekiwane zachowanie**.

Pełny przepływ będzie wyglądał dopiero później:

```text
formularz HTML
      |
      | POST
      v
Django
      |
      | walidacja
      v
model
      |
      v
baza danych
```

Dzisiejszym celem jest przygotowanie **interfejsu formularza**.

---

# 25. GET a POST w praktyce

Mamy teraz dwa formularze o różnych zastosowaniach.

## Wyszukiwanie

```html
<form method="get">
```

Użytkownik:

```text
szuka danych
```

Adres może zawierać:

```text
?query=Solaris
```

To jest sensowne, ponieważ wyszukiwanie nie zmienia danych w bazie.

---

## Dodawanie książki

```html
<form method="post">
```

Użytkownik:

```text
wysyła dane
```

które później mają zostać zapisane.

Adres nie powinien zawierać całej przesyłanej zawartości formularza.

Do obsługi tego formularza potrzebujemy backendu.

---

# 26. Rozbudowujemy formularz wyszukiwania

W `index.html` zmień formularz wyszukiwania na:

```html
<form method="get">

    <div>
        <label for="query">
            Tytuł lub autor:
        </label>

        <input
            type="text"
            id="query"
            name="query"
            placeholder="np. Solaris"
        >
    </div>

    <div>
        <label for="search-category">
            Kategoria:
        </label>

        <select
            id="search-category"
            name="category"
        >
            <option value="">
                Wszystkie
            </option>

            <option value="fantasy">
                Fantasy
            </option>

            <option value="science-fiction">
                Science fiction
            </option>

            <option value="crime">
                Kryminał
            </option>
        </select>
    </div>

    <button type="submit">
        Szukaj
    </button>

</form>
```

Wybierz np.:

```text
Solaris
Science fiction
```

i wyślij formularz.

Adres może wyglądać tak:

```text
http://127.0.0.1:8000/?query=Solaris&category=science-fiction
```

Widzimy tutaj, że dane z kilku pól zostały zapisane jako parametry adresu.

---

# 27. Grupowanie formularza

W przykładach używamy:

```html
<div>
    ...
</div>
```

aby zgrupować:

```text
label + pole
```

Przykład:

```html
<div>
    <label for="title">
        Tytuł:
    </label>

    <input
        type="text"
        id="title"
        name="title"
    >
</div>
```

Na razie `div` nie zmienia wyglądu formularza.

Na zajęciach z CSS będziemy mogli dzięki takim kontenerom wygodnie ustawić:

- odstępy,
- szerokości,
- układ pól,
- wygląd przycisków.

---

# 28. Struktura projektu po laboratorium

Po dzisiejszych zajęciach:

```text
paw_biblioteka/
│
├── .gitignore
├── README.md
├── check_environment.py
│
└── frontend/
    │
    ├── index.html
    ├── add_book.html
    │
    └── images/
        └── ...
```

Mamy już dwa różne widoki interfejsu:

```text
strona główna / lista
formularz dodawania książki
```

Na razie oba są statyczne.

---

# 29. Sprawdzamy DOM formularza

Otwórz:

```text
http://127.0.0.1:8000/add_book.html
```

Naciśnij:

```text
F12
```

Otwórz:

```text
Elements / Inspector
```

Znajdź:

```html
<form>
```

Rozwiń jego strukturę.

Powinniśmy zobaczyć mniej więcej:

```text
form
├── div
│   ├── label
│   └── input
├── div
│   ├── label
│   └── input
├── div
│   ├── label
│   └── select
├── div
│   ├── input
│   └── label
├── div
│   ├── label
│   └── textarea
└── div
    ├── button
    └── button
```

To jest DOM naszego formularza.

---

# 30. Mała modyfikacja podczas zajęć

Dodaj do formularza pole:

```text
ISBN
```

Przykład:

```html
<div>
    <label for="isbn">
        ISBN:
    </label>

    <input
        type="text"
        id="isbn"
        name="isbn"
        placeholder="np. 978-83-..."
    >
</div>
```

Dodaj również pole:

```text
Liczba egzemplarzy
```

Przykład:

```html
<div>
    <label for="copies">
        Liczba egzemplarzy:
    </label>

    <input
        type="number"
        id="copies"
        name="copies"
        min="0"
        value="1"
    >
</div>
```

Atrybut:

```text
value="1"
```

ustawia wartość początkową pola.

---

# 31. Zapisujemy zmiany w Git

Sprawdź:

```bash
git status
```

Dodaj zmiany:

```bash
git add .
```

Utwórz commit:

```bash
git commit -m "Dodanie formularzy HTML i podstaw DOM"
```

Wyślij:

```bash
git push
```

---

# 32. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Przykładowe rozwiązanie zadań programistycznych zostanie krótko pokazane na początku kolejnego laboratorium.

---

## Zadanie 1 — formularz kontaktowy

Utwórz plik:

```text
frontend/contact.html
```

Przygotuj formularz zawierający:

```text
imię
adres e-mail
temat
wiadomość
```

Wymagania:

- `imię` — pole tekstowe,
- `e-mail` — `type="email"`,
- `temat` — pole tekstowe,
- `wiadomość` — `textarea`,
- wszystkie pola obowiązkowe,
- przycisk „Wyślij”.

Formularz może korzystać z:

```html
method="post"
```

Nie musi jeszcze faktycznie wysyłać wiadomości.

---

## Zadanie 2 — filtr książek

Rozbuduj formularz wyszukiwania o pole:

```text
Rok wydania od
```

Użyj:

```html
<input type="number">
```

Po wysłaniu formularza spróbuj uzyskać adres podobny do:

```text
?query=Solaris&category=science-fiction&year_from=1950
```

---

## Zadanie 3 — formularz rejestracji

Utwórz plik:

```text
frontend/register.html
```

Formularz powinien zawierać:

```text
imię
nazwisko
e-mail
hasło
powtórz hasło
akceptacja regulaminu
```

Dla hasła wykorzystaj:

```html
<input type="password">
```

Dla akceptacji regulaminu:

```html
<input type="checkbox">
```

Dodaj odpowiednie `label`, `name` oraz `required`.

---

## Zadanie 4 — własny formularz

Dla pomysłu na własną aplikację z wcześniejszych zajęć przygotuj formularz dodawania jednego z głównych elementów.

Przykłady:

### Planer nauki

```text
nazwa zadania
przedmiot
termin
priorytet
opis
```

### Katalog filmów

```text
tytuł
rok
gatunek
ocena
opis
```

### Rejestr treningów

```text
data
rodzaj treningu
czas
intensywność
notatka
```

Wykorzystaj przynajmniej:

- `input type="text"`,
- `input type="number"` lub `date`,
- `select`,
- `textarea`,
- `button`.

---

# 33. Co dalej?

Mamy już:

```text
HTML
strukturę dokumentu
DOM
tabele
obrazy
linki
formularze
GET
POST
```

Strona nadal wygląda jednak bardzo surowo.

Na kolejnych zajęciach zaczniemy pracę z:

```text
CSS
```

Dzięki temu nadamy Bibliotece:

- kolory,
- odstępy,
- szerokości,
- obramowania,
- czytelniejszą typografię,
- wygląd formularzy,
- wygląd tabeli,
- bardziej uporządkowany interfejs.

Nie będziemy zmieniać logiki strony.

CSS będzie odpowiadał przede wszystkim za **sposób prezentacji istniejącego HTML**.
