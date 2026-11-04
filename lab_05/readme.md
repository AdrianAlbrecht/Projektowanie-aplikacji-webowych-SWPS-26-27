# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 05 — CSS: wygląd i stylowanie strony

---

## 1. Cel zajęć

Na poprzednich zajęciach przygotowaliśmy formularze HTML oraz omówiliśmy DOM.

Dzisiaj zaczniemy pracę z:

```text
CSS
```

czyli warstwą odpowiadającą za wygląd strony.

Po zakończeniu laboratorium powinniśmy:

- wiedzieć, jak podłączyć plik CSS do dokumentu HTML,
- znać podstawowe selektory CSS,
- rozumieć różnicę między selektorem elementu, klasy i `id`,
- potrafić ustawiać kolory, czcionki, odstępy i obramowania,
- rozumieć podstawy box model,
- znać podstawowe jednostki CSS,
- potrafić ostylować formularz,
- potrafić ostylować tabelę,
- przygotować bardziej czytelny i uporządkowany wygląd projektu Biblioteka.

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

Na końcu `lab04` znajdowały się cztery nieobowiązkowe zadania programistyczne.

Poniżej znajduje się przykładowe rozwiązanie każdego z nich.

---

## Zadanie 1 — formularz kontaktowy

Plik:

```text
frontend/contact.html
```

Przykładowa zawartość:

```html
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Kontakt — Biblioteka</title>
</head>

<body>

    <header>
        <h1>Biblioteka</h1>

        <nav>
            <a href="index.html">Strona główna</a>
            <a href="add_book.html">Dodaj książkę</a>
            <a href="contact.html">Kontakt</a>
        </nav>
    </header>

    <main>

        <section>
            <h2>Kontakt</h2>

            <form method="post">

                <div>
                    <label for="name">
                        Imię:
                    </label>

                    <input
                        type="text"
                        id="name"
                        name="name"
                        required
                    >
                </div>

                <div>
                    <label for="email">
                        E-mail:
                    </label>

                    <input
                        type="email"
                        id="email"
                        name="email"
                        required
                    >
                </div>

                <div>
                    <label for="subject">
                        Temat:
                    </label>

                    <input
                        type="text"
                        id="subject"
                        name="subject"
                        required
                    >
                </div>

                <div>
                    <label for="message">
                        Wiadomość:
                    </label>

                    <textarea
                        id="message"
                        name="message"
                        rows="6"
                        cols="50"
                        required
                    ></textarea>
                </div>

                <div>
                    <button type="submit">
                        Wyślij
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

## Zadanie 2 — filtr książek

Przykładowe rozszerzenie formularza wyszukiwania:

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

    <div>
        <label for="year-from">
            Rok wydania od:
        </label>

        <input
            type="number"
            id="year-from"
            name="year_from"
            min="0"
        >
    </div>

    <button type="submit">
        Szukaj
    </button>

</form>
```

Przykładowy adres po wysłaniu:

```text
?query=Solaris&category=science-fiction&year_from=1950
```

---

## Zadanie 3 — formularz rejestracji

Plik:

```text
frontend/register.html
```

Przykładowa zawartość:

```html
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Rejestracja — Biblioteka</title>
</head>

<body>

    <header>
        <h1>Biblioteka</h1>

        <nav>
            <a href="index.html">Strona główna</a>
            <a href="register.html">Rejestracja</a>
        </nav>
    </header>

    <main>

        <section>
            <h2>Rejestracja</h2>

            <form method="post">

                <div>
                    <label for="first-name">
                        Imię:
                    </label>

                    <input
                        type="text"
                        id="first-name"
                        name="first_name"
                        required
                    >
                </div>

                <div>
                    <label for="last-name">
                        Nazwisko:
                    </label>

                    <input
                        type="text"
                        id="last-name"
                        name="last_name"
                        required
                    >
                </div>

                <div>
                    <label for="register-email">
                        E-mail:
                    </label>

                    <input
                        type="email"
                        id="register-email"
                        name="email"
                        required
                    >
                </div>

                <div>
                    <label for="password">
                        Hasło:
                    </label>

                    <input
                        type="password"
                        id="password"
                        name="password"
                        required
                    >
                </div>

                <div>
                    <label for="password-repeat">
                        Powtórz hasło:
                    </label>

                    <input
                        type="password"
                        id="password-repeat"
                        name="password_repeat"
                        required
                    >
                </div>

                <div>
                    <input
                        type="checkbox"
                        id="terms"
                        name="terms"
                        required
                    >

                    <label for="terms">
                        Akceptuję regulamin
                    </label>
                </div>

                <div>
                    <button type="submit">
                        Zarejestruj
                    </button>
                </div>

            </form>
        </section>

    </main>

</body>
</html>
```

---

## Zadanie 4 — własny formularz

Przykładowy temat:

> **Planer nauki**

Przykładowy formularz:

```html
<form method="post">

    <div>
        <label for="task-name">
            Nazwa zadania:
        </label>

        <input
            type="text"
            id="task-name"
            name="task_name"
            required
        >
    </div>

    <div>
        <label for="subject">
            Przedmiot:
        </label>

        <select
            id="subject"
            name="subject"
            required
        >
            <option value="">
                Wybierz przedmiot
            </option>

            <option value="programming">
                Programowanie
            </option>

            <option value="statistics">
                Statystyka
            </option>

            <option value="psychology">
                Psychologia
            </option>
        </select>
    </div>

    <div>
        <label for="deadline">
            Termin:
        </label>

        <input
            type="date"
            id="deadline"
            name="deadline"
            required
        >
    </div>

    <div>
        <label for="priority">
            Priorytet:
        </label>

        <select
            id="priority"
            name="priority"
        >
            <option value="low">
                Niski
            </option>

            <option value="medium">
                Średni
            </option>

            <option value="high">
                Wysoki
            </option>
        </select>
    </div>

    <div>
        <label for="description">
            Opis:
        </label>

        <textarea
            id="description"
            name="description"
            rows="5"
        ></textarea>
    </div>

    <button type="submit">
        Dodaj zadanie
    </button>

</form>
```

---

# 4. Czym jest CSS?

CSS oznacza:

> **Cascading Style Sheets**

HTML opisuje strukturę strony.

CSS opisuje sposób jej prezentacji.

Możemy to uprościć do:

```text
HTML
= co znajduje się na stronie

CSS
= jak to wygląda
```

Przykład HTML:

```html
<h1>Biblioteka</h1>
```

Przykład CSS:

```css
h1 {
    color: darkblue;
}
```

Po zastosowaniu stylu nagłówek nadal jest elementem `<h1>`, ale zmienia się jego wygląd.

---

# 5. Tworzymy plik CSS

W katalogu:

```text
frontend
```

utwórz katalog:

```text
css
```

W nim plik:

```text
styles.css
```

Struktura:

```text
frontend/
│
├── css/
│   └── styles.css
│
├── images/
│   └── ...
│
├── index.html
├── add_book.html
├── contact.html
└── register.html
```

---

# 6. Podłączamy CSS do HTML

W każdym pliku HTML w sekcji:

```html
<head>
```

dodaj:

```html
<link rel="stylesheet" href="css/styles.css">
```

Przykład:

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Biblioteka</title>

    <link rel="stylesheet" href="css/styles.css">
</head>
```

Dodaj ten sam wpis do:

```text
index.html
add_book.html
contact.html
register.html
```

Od tego momentu wszystkie te strony mogą korzystać z jednego wspólnego pliku CSS.

---

# 7. Pierwszy styl

Do:

```text
frontend/css/styles.css
```

wklej:

```css
body {
    background-color: #f5f5f5;
}
```

Zapisz plik i odśwież stronę.

Tło powinno zmienić kolor.

Podstawowa składnia CSS wygląda tak:

```css
selektor {
    właściwość: wartość;
}
```

Przykład:

```css
h1 {
    color: navy;
}
```

---

# 8. Selektor elementu

Możemy stylować wszystkie elementy danego typu.

Przykład:

```css
h1 {
    color: #1f3c5c;
}
```

Styl zostanie zastosowany do każdego:

```html
<h1>
```

na stronie.

Inny przykład:

```css
p {
    line-height: 1.6;
}
```

Dotyczy wszystkich akapitów.

---

# 9. Selektor klasy

Klasa pozwala zaznaczyć wybraną grupę elementów.

HTML:

```html
<div class="book-info">
```

CSS:

```css
.book-info {
    color: #555555;
}
```

Kropka:

```text
.
```

oznacza selektor klasy.

Czyli:

```css
.book-info
```

oznacza:

> wszystkie elementy posiadające `class="book-info"`

Jedna klasa może występować na wielu elementach.

---

# 10. Selektor id

Element:

```html
<section id="books">
```

możemy wybrać przez:

```css
#books {
    background-color: white;
}
```

Znak:

```text
#
```

oznacza selektor `id`.

Czyli:

```css
#books
```

oznacza:

> element posiadający `id="books"`

`id` powinno być unikalne na stronie.

---

# 11. Klasa a id

Najprościej:

```text
class
= może być używana wiele razy

id
= powinno być unikalne
```

Przykład:

```html
<article class="book-card">
    ...
</article>

<article class="book-card">
    ...
</article>

<article class="book-card">
    ...
</article>
```

Tutaj wszystkie książki mogą korzystać z tej samej klasy:

```text
book-card
```

---

# 12. Przygotowujemy klasy dla książek

W `index.html` zmień:

```html
<article>
```

dla każdej książki na:

```html
<article class="book-card">
```

Przykład:

```html
<article class="book-card">
    <h3>Solaris</h3>

    <div class="book-info">
        <p>
            <strong>Autor:</strong>
            Stanisław Lem
        </p>

        <p>
            <strong>Rok wydania:</strong>
            1961
        </p>
    </div>

    <p>
        Powieść science fiction opowiadająca o próbie
        kontaktu człowieka z obcą formą inteligencji.
    </p>
</article>
```

Dzięki temu wszystkie książki możemy ostylować jednocześnie.

---

# 13. Kolory

CSS pozwala zapisywać kolory na wiele sposobów.

Przykład nazwą:

```css
color: red;
```

Przykład w zapisie HEX:

```css
color: #1f3c5c;
```

Przykład RGB:

```css
color: rgb(31, 60, 92);
```

Na zajęciach możemy korzystać głównie z prostych wartości HEX.

W HEX kolor zapisywany jest w postaci heksadecymalnej. Nie jest wymagana znajomość na pamięć wszystkich kolorów, natomiast warto pamiętać jaki jest wzór takiej postaci:

- na początku znak `#`, który informuje nas o kodowaniu heksadecymalnym
- następnie 3 liczby 2 cyfrowe (wartości od 0-255) w systemie szesnastkowym (00 - FF) opisującym wkład kolejnych kolorów w systemie RGB (Red Green Blue), czyli kolejno:
    - czerwony (R)
    - zielony (G)
    - niebieski (B)

Czyli przykładowo kolor `#00FFFF` oznacza nam wkład o wartości 0 (00) koloru czerwonego, wkład o wartości 255 (FF) zielonego i wkład o wartości 255 (FF) niebieskiego, co tworzy nam kolor o nazwie `cyan`.

Istnieją gotowe stronki czy algorytmy do wybierania kolorów, tzw. `Color Picker`. Oto jeden z nich: [HTML Color Picker](https://htmlcolorcodes.com/).

---

# 14. Kolor tekstu i tła

Przykład:

```css
body {
    background-color: #f5f5f5;
    color: #222222;
}
```

`background-color`:

```text
kolor tła
```

`color`:

```text
kolor tekstu
```

---

# 15. Czcionka

Dodaj do `body`:

```css
body {
    background-color: #f5f5f5;
    color: #222222;

    font-family: Arial, Helvetica, sans-serif;
}
```

Przeglądarka będzie próbowała kolejno używać:

```text
Arial
Helvetica
dowolnej czcionki bezszeryfowej
```

---

# 16. Rozmiar tekstu

Przykład:

```css
h1 {
    font-size: 2rem;
}
```

Inny przykład:

```css
p {
    font-size: 1rem;
}
```

---

# 17. Podstawowe jednostki

Podczas zajęć najczęściej będziemy korzystali z:

```text
px
%
rem
```

## px

Przykład:

```css
padding: 20px;
```

Jednostka pikselowa.

## %

Przykład:

```css
width: 80%;
```

Wartość zależna od rozmiaru elementu nadrzędnego.

## rem

Przykład:

```css
font-size: 1.5rem;
```

Jednostka związana z bazowym rozmiarem tekstu dokumentu.

Na początku nie musimy analizować wszystkich możliwych jednostek CSS.

---

# 18. Szerokość głównej zawartości

Dodaj:

```css
main {
    width: 90%;
    max-width: 1000px;

    margin: 0 auto;
}
```

`width`:

```text
szerokość elementu
```

`max-width`:

```text
maksymalna szerokość
```

`margin: 0 auto`:

```text
wyśrodkowanie elementu w poziomie
```

Dzięki temu zawartość nie będzie rozciągała się przez całą szerokość dużego ekranu.

---

# 19. Box model

Każdy element HTML możemy wyobrazić sobie jako prostokąt.

Uproszczony model:

```text
+-----------------------------+
|           margin            |
|   +---------------------+   |
|   |       border        |   |
|   |   +-------------+   |   |
|   |   |   padding   |   |   |
|   |   | +---------+ |   |   |
|   |   | | content | |   |   |
|   |   | +---------+ |   |   |
|   |   +-------------+   |   |
|   +---------------------+   |
+-----------------------------+
```

Czyli:

```text
content
= treść

padding
= odstęp wewnętrzny

border
= obramowanie

margin
= odstęp zewnętrzny
```

---

# 20. Praktyka z box model

Dodaj:

```css
section {
    background-color: white;

    margin: 20px 0;
    padding: 20px;

    border: 1px solid #dddddd;
}
```

Odśwież stronę.

Każda sekcja powinna wyglądać jak osobny blok.

Możemy dodać:

```css
section {
    background-color: white;

    margin: 20px 0;
    padding: 20px;

    border: 1px solid #dddddd;
    border-radius: 8px;
}
```

`border-radius` zaokrągla narożniki.

---

# 21. Stylujemy header

Dodaj:

```css
header {
    background-color: #1f3c5c;
    color: white;

    padding: 20px;
}
```

Nagłówek strony otrzyma ciemne tło.

---

# 22. Stylujemy nawigację

Dodaj:

```css
nav {
    margin-top: 15px;
}
```

Następnie:

```css
nav a {
    color: white;
    text-decoration: none;

    margin-right: 15px;
}
```

Selektor:

```css
nav a
```

oznacza:

> wszystkie elementy `<a>` znajdujące się wewnątrz `<nav>`

Dodaj:

```css
nav a:hover {
    text-decoration: underline;
}
```

`:hover` oznacza styl obowiązujący po najechaniu kursorem.

---

# 23. Stylujemy karty książek

Dodaj:

```css
.book-card {
    border: 1px solid #cccccc;
    border-radius: 6px;

    padding: 15px;
    margin: 15px 0;

    background-color: #fafafa;
}
```

Dodatkowo:

```css
.book-card h3 {
    margin-top: 0;
}
```

oraz:

```css
.book-info {
    color: #555555;
}
```

Widzimy tutaj trzy rodzaje selektorów:

```text
.book-card
.book-card h3
.book-info
```

---

# 24. Stylujemy obraz

Dodaj:

```css
img {
    max-width: 100%;
    height: auto;
}
```

Dzięki temu obraz nie powinien wychodzić poza szerokość rodzica.

Jeżeli chcemy stylować konkretny obraz, możemy nadać mu klasę.

HTML:

```html
<img
    class="library-image"
    src="images/library.jpg"
    alt="Wnętrze biblioteki"
>
```

CSS:

```css
.library-image {
    width: 300px;
    border-radius: 8px;
}
```

---

# 25. Stylujemy tabelę

Dodaj:

```css
table {
    width: 100%;
    border-collapse: collapse;

    margin: 20px 0;
}
```

Następnie:

```css
th,
td {
    border: 1px solid #cccccc;
    padding: 10px;
    text-align: left;
}
```

Nagłówek:

```css
th {
    background-color: #eeeeee;
}
```

Efekt powinien być znacznie czytelniejszy niż domyślna tabela HTML.

---

# 26. Stylujemy formularze

Dodaj:

```css
form {
    max-width: 600px;
}
```

Następnie:

```css
form div {
    margin-bottom: 15px;
}
```

Dla etykiet:

```css
label {
    display: block;
    margin-bottom: 5px;

    font-weight: bold;
}
```

Dla pól:

```css
input,
select,
textarea {
    width: 100%;

    padding: 8px;

    border: 1px solid #bbbbbb;
    border-radius: 4px;

    box-sizing: border-box;
}
```

---

# 27. Dlaczego box-sizing?

Domyślnie szerokość elementu może być liczona bez `padding` i `border`.

To może powodować sytuację, w której:

```css
width: 100%;
```

plus padding sprawia, że pole staje się większe niż jego kontener.

Dlatego często stosujemy:

```css
box-sizing: border-box;
```

Wtedy:

```text
width
```

obejmuje również:

```text
padding
border
```

---

# 28. Problem z checkbox i radio

Reguła:

```css
input {
    width: 100%;
}
```

nie jest dobra dla:

```text
checkbox
radio
```

Dlatego zamiast stylować wszystkie `input` jednakowo, możemy napisać:

```css
input[type="text"],
input[type="email"],
input[type="password"],
input[type="number"],
input[type="date"],
select,
textarea {
    width: 100%;

    padding: 8px;

    border: 1px solid #bbbbbb;
    border-radius: 4px;

    box-sizing: border-box;
}
```

A dla:

```text
checkbox
radio
```

pozostawić domyślny rozmiar.

---

# 29. Selektor atrybutu

Zapis:

```css
input[type="text"]
```

oznacza:

> wybierz element `input`, którego atrybut `type` ma wartość `text`

Inny przykład:

```css
input[type="email"]
```

Nie musimy dodawać osobnych klas do każdego pola.

---

# 30. Stylujemy przyciski

Dodaj:

```css
button {
    background-color: #1f3c5c;
    color: white;

    border: none;
    border-radius: 4px;

    padding: 10px 16px;

    cursor: pointer;
}
```

Po najechaniu:

```css
button:hover {
    background-color: #315c87;
}
```

---

# 31. Stylujemy footer

Dodaj:

```css
footer {
    margin-top: 30px;
    padding: 20px;

    text-align: center;

    background-color: #eeeeee;
}
```

---

# 32. Gotowy styles.css

Po wykonaniu poprzednich kroków plik:

```text
frontend/css/styles.css
```

może wyglądać następująco:

```css
body {
    margin: 0;

    background-color: #f5f5f5;
    color: #222222;

    font-family: Arial, Helvetica, sans-serif;
}

header {
    background-color: #1f3c5c;
    color: white;

    padding: 20px;
}

header h1 {
    margin-top: 0;
}

nav {
    margin-top: 15px;
}

nav a {
    color: white;
    text-decoration: none;

    margin-right: 15px;
}

nav a:hover {
    text-decoration: underline;
}

main {
    width: 90%;
    max-width: 1000px;

    margin: 0 auto;
}

section {
    background-color: white;

    margin: 20px 0;
    padding: 20px;

    border: 1px solid #dddddd;
    border-radius: 8px;
}

.book-card {
    border: 1px solid #cccccc;
    border-radius: 6px;

    padding: 15px;
    margin: 15px 0;

    background-color: #fafafa;
}

.book-card h3 {
    margin-top: 0;
}

.book-info {
    color: #555555;
}

img {
    max-width: 100%;
    height: auto;
}

.library-image {
    width: 300px;
    border-radius: 8px;
}

table {
    width: 100%;

    border-collapse: collapse;

    margin: 20px 0;
}

th,
td {
    border: 1px solid #cccccc;

    padding: 10px;

    text-align: left;
}

th {
    background-color: #eeeeee;
}

form {
    max-width: 600px;
}

form div {
    margin-bottom: 15px;
}

label {
    display: block;

    margin-bottom: 5px;

    font-weight: bold;
}

input[type="text"],
input[type="email"],
input[type="password"],
input[type="number"],
input[type="date"],
select,
textarea {
    width: 100%;

    padding: 8px;

    border: 1px solid #bbbbbb;
    border-radius: 4px;

    box-sizing: border-box;
}

button {
    background-color: #1f3c5c;
    color: white;

    border: none;
    border-radius: 4px;

    padding: 10px 16px;

    cursor: pointer;
}

button:hover {
    background-color: #315c87;
}

footer {
    margin-top: 30px;
    padding: 20px;

    text-align: center;

    background-color: #eeeeee;
}
```

Skopiowanie tego pliku daje kompletny punkt wyjścia do dalszego stylowania projektu.

---

# 33. Sprawdzamy CSS w DevTools

Otwórz stronę i naciśnij:

```text
F12
```

Wybierz dowolny element.

Po prawej stronie powinny być widoczne style CSS.

Znajdź np.:

```css
.book-card {
    ...
}
```

Spróbuj tymczasowo zmienić:

```text
background-color
padding
border-radius
```

DevTools pozwala obserwować efekt natychmiast.

Pamiętaj:

> zmiany wykonane tylko w DevTools znikną po odświeżeniu strony.

Jeżeli chcesz zachować zmianę, trzeba ją wpisać do:

```text
styles.css
```

---

# 34. Mała modyfikacja podczas zajęć

Dodaj klasę:

```text
available
```

do dostępnej książki:

```html
<article class="book-card available">
```

oraz:

```text
unavailable
```

do niedostępnej:

```html
<article class="book-card unavailable">
```

W CSS dodaj:

```css
.available {
    border-left: 6px solid green;
}

.unavailable {
    border-left: 6px solid darkred;
}
```

W ten sposób element może posiadać więcej niż jedną klasę:

```html
class="book-card available"
```

Pierwsza klasa odpowiada za ogólny wygląd karty.

Druga informuje o jej dodatkowym stanie.

---

# 35. Dziedziczenie

Niektóre właściwości są dziedziczone przez elementy potomne.

Przykład:

```css
body {
    color: #222222;
}
```

Tekst wielu elementów wewnątrz `body` automatycznie otrzyma ten kolor.

Podobnie:

```css
header {
    color: white;
}
```

powoduje, że tekst wewnątrz nagłówka może odziedziczyć biały kolor.

Nie wszystkie właściwości są dziedziczone.

Nie musimy zapamiętywać ich listy.

Najważniejsze jest zauważenie, że CSS działa **kaskadowo** i reguły mogą na siebie wpływać.

---

# 36. Co oznacza „Cascading”?

Możemy mieć kilka reguł dotyczących tego samego elementu.

Przykład:

```css
p {
    color: black;
}
```

oraz:

```css
.book-info {
    color: gray;
}
```

Akapit znajdujący się wewnątrz:

```html
<div class="book-info">
```

może otrzymać styl wynikający z bardziej konkretnej reguły.

Na tym etapie nie będziemy szczegółowo analizować całego systemu priorytetów CSS.

Ważne jest tylko to, że:

```text
kilka reguł może wpływać na jeden element
```

---

# 37. Aktualna struktura projektu

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
    ├── css/
    │   └── styles.css
    │
    ├── images/
    │   └── ...
    │
    ├── index.html
    ├── add_book.html
    ├── contact.html
    └── register.html
```

Mamy już:

```text
strukturę HTML
formularze
wspólny wygląd CSS
```

---

# 38. Zapisujemy zmiany w Git

Sprawdź:

```bash
git status
```

Dodaj:

```bash
git add .
```

Commit:

```bash
git commit -m "Dodanie podstawowych stylów CSS"
```

Wyślij:

```bash
git push
```

---

# 39. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Przykładowe rozwiązania zostaną pokazane na początku następnego laboratorium.

---

## Zadanie 1 — wyróżnienie kategorii książek

Dodaj książkom dodatkowe klasy zależne od kategorii.

Przykład:

```html
<article class="book-card fantasy">
```

lub:

```html
<article class="book-card science-fiction">
```

Przygotuj różne style, np.:

```css
.fantasy {
    ...
}

.science-fiction {
    ...
}
```

Nie musisz mocno zmieniać wyglądu.

Wystarczy np. inny kolor obramowania.

---

## Zadanie 2 — styl formularza rejestracji

Rozbuduj CSS tak, aby:

- formularz rejestracji był wyśrodkowany,
- miał białe tło,
- posiadał padding,
- posiadał obramowanie,
- posiadał zaokrąglone narożniki.

Możesz nadać sekcji lub formularzowi dodatkową klasę.

---

## Zadanie 3 — dodatkowy przycisk

Dodaj w `index.html` link:

```text
Dodaj książkę
```

i ostyluj go tak, aby wyglądał podobnie do przycisku.

Przykładowy HTML:

```html
<a
    class="button-link"
    href="add_book.html"
>
    Dodaj książkę
</a>
```

Przygotuj odpowiednią klasę:

```css
.button-link {
    ...
}
```

---

## Zadanie 4 — własna karta

Dla strony `practice.html` przygotowanej wcześniej utwórz własną klasę karty.

Przykład:

```html
<article class="task-card">
```

Dodaj CSS odpowiadający za:

- obramowanie,
- padding,
- margin,
- kolor tła,
- zaokrąglenie narożników.

---

## Zadanie 5 — DevTools

W DevTools zmień tymczasowo:

```text
kolor nagłówka
padding sekcji
szerokość main
kolor przycisku
```

Następnie wybierz jedną zmianę, która wygląda najlepiej, i przenieś ją do:

```text
styles.css
```

---

# 40. Co dalej?

Mamy już podstawy:

```text
HTML
DOM
formularze
CSS
```

Na kolejnym laboratorium skupimy się na **układzie strony**.

Poznamy:

```text
Flexbox
```

i wykorzystamy go do:

- lepszego ułożenia nawigacji,
- ułożenia kart książek,
- poprawienia formularzy,
- przygotowania prostego układu responsywnego.

Częścią zajęć będzie również praktyczne ćwiczenie z:

```text
Flexbox Froggy
```

Dzięki temu zakończymy podstawową część frontendową przed przejściem do Django.
