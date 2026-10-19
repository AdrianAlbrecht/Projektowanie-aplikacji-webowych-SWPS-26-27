# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 03 — HTML: pierwsza strona Biblioteki

---

## 1. Cel zajęć

Na poprzednich zajęciach przygotowaliśmy środowisko pracy i repozytorium Git.

Dzisiaj tworzymy **pierwszy rzeczywisty fragment naszej aplikacji** — statyczną stronę HTML projektu Biblioteka.

Po zakończeniu laboratorium powinniśmy:

- znać podstawową strukturę dokumentu HTML,
- rozumieć różnicę między zawartością `<head>` i `<body>`,
- potrafić tworzyć nagłówki, akapity, listy i linki,
- znać podstawowe znaczniki semantyczne,
- potrafić przygotować prostą strukturę strony,
- uruchomić stronę za pomocą prostego serwera HTTP,
- zapisać wykonane zmiany w Git.

> [!CAUTION]
> **Biblioteka, katalog książek oraz system wypożyczania książek są tematami niedozwolonymi w projekcie zaliczeniowym.**
>
> Biblioteka jest projektem ćwiczeniowym rozwijanym wspólnie podczas laboratoriów.

---

# 2. Początek pracy

Na początku pobierz najnowszą wersję projektu:

```bash
git pull
```

Jeżeli pracujesz na nowym komputerze i nie masz jeszcze projektu:

```bash
git clone ADRES_REPOZYTORIUM
cd paw_biblioteka
```

Jeżeli potrzebujesz środowiska wirtualnego:

```bash
python -m venv .venv
```

lub:

```bash
py -m venv .venv
```

Aktywacja na Windows:

```cmd
.venv\Scripts\activate.bat
```

---

# 3. Krótkie rozwiązanie z poprzedniego laboratorium

Jednym z opcjonalnych zadań było utworzenie pliku:

```text
notes.md
```

Przykładowa zawartość:

```markdown
# Notatki

## Lab 01

- klient i serwer,
- frontend i backend,
- request i response,
- GET i POST.

## Lab 02

- środowisko wirtualne,
- Git,
- GitHub,
- commit,
- push,
- pull.
```

Dodanie pliku do repozytorium:

```bash
git add .
git commit -m "Dodanie notatek z laboratoriów"
git push
```

Najważniejszy schemat pracy pozostaje bez zmian:

```text
git pull
   |
   v
pracuję nad projektem
   |
   v
git status
   |
   v
git add .
   |
   v
git commit
   |
   v
git push
```

---

# 4. Tworzymy katalog na statyczną wersję strony

W głównym katalogu projektu utwórz katalog:

```text
frontend
```

W nim utwórz plik:

```text
index.html
```

Struktura projektu:

```text
paw_biblioteka/
│
├── .gitignore
├── README.md
├── check_environment.py
│
└── frontend/
    └── index.html
```

Na kilku najbliższych zajęciach katalog `frontend` będzie zawierał statyczny prototyp interfejsu.

Później, kiedy rozpoczniemy Django, wykorzystamy podobny kod jako podstawę **szablonów Django**.

---

# 5. Czym jest HTML?

HTML oznacza:

> **HyperText Markup Language**

HTML służy do opisania **struktury i znaczenia zawartości strony**.

Nie jest językiem programowania.

HTML mówi przeglądarce np.:

```text
to jest główny nagłówek
to jest akapit
to jest link
to jest lista
to jest sekcja strony
```

Za wygląd tych elementów będzie później odpowiadał przede wszystkim **CSS**.

Przykład:

```html
<h1>Biblioteka</h1>

<p>Witaj w naszej bibliotece.</p>
```

Przeglądarka interpretuje znaczniki i wyświetla odpowiednio przygotowaną treść.

---

# 6. Znaczniki HTML

Większość elementów HTML posiada:

```html
<znacznik>zawartość</znacznik>
```

Przykład:

```html
<p>To jest akapit.</p>
```

Mamy tutaj:

```text
<p>     — znacznik otwierający

tekst   — zawartość

</p>    — znacznik zamykający
```

Elementy mogą znajdować się wewnątrz innych elementów:

```html
<article>
    <h2>Solaris</h2>
    <p>Autor: Stanisław Lem</p>
</article>
```

Takie umieszczanie elementów jeden wewnątrz drugiego nazywamy **zagnieżdżaniem**.

---

# 7. Podstawowa struktura dokumentu

Do pliku:

```text
frontend/index.html
```

wklej:

```html
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Biblioteka</title>
</head>
<body>

    <h1>Biblioteka</h1>

    <p>
        Projekt ćwiczeniowy z przedmiotu Projektowanie aplikacji webowych.
    </p>

</body>
</html>
```

To będzie punkt wyjścia do dalszej pracy.

---

# 8. Co oznaczają poszczególne elementy?

## `<!DOCTYPE html>`

```html
<!DOCTYPE html>
```

Informuje przeglądarkę, że dokument wykorzystuje współczesny HTML.

---

## `<html>`

```html
<html lang="pl">
```

Jest głównym elementem dokumentu.

Atrybut:

```text
lang="pl"
```

informuje, że podstawowym językiem strony jest polski.

---

## `<head>`

```html
<head>
    ...
</head>
```

Zawiera informacje **o stronie**, które zwykle nie są bezpośrednio wyświetlane w jej treści.

---

## Kodowanie znaków

```html
<meta charset="UTF-8">
```

Pozwala poprawnie wyświetlać m.in. polskie znaki:

```text
ą ć ę ł ń ó ś ź ż
```

---

## Ustawienia urządzeń mobilnych

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Pomaga przeglądarce poprawnie dopasować stronę do szerokości urządzenia.

Do responsywności wrócimy dokładniej podczas CSS.

---

## `<title>`

```html
<title>Biblioteka</title>
```

Ustawia tytuł widoczny m.in. na karcie przeglądarki.

---

## `<body>`

```html
<body>
    ...
</body>
```

Tutaj znajduje się zawartość widoczna dla użytkownika.

---

# 9. Uruchamiamy stronę przez HTTP

Zamiast otwierać `index.html` bezpośrednio z dysku, uruchomimy prosty serwer HTTP.

Przejdź do katalogu:

```bash
cd frontend
```

Uruchom:

```bash
python -m http.server 8000
```

Jeżeli korzystasz z `py`:

```bash
py -m http.server 8000
```

Powinien pojawić się komunikat podobny do:

```text
Serving HTTP on ... port 8000
```

Otwórz w przeglądarce:

```text
http://127.0.0.1:8000/
```

Powinna pojawić się nasza pierwsza strona:

```text
Biblioteka

Projekt ćwiczeniowy z przedmiotu Projektowanie aplikacji webowych.
```

Serwer zatrzymujemy kombinacją:

```text
Ctrl + C
```

---

# 10. Sprawdzamy request z poprzednich zajęć

Kiedy serwer działa:

1. otwórz `F12`,
2. przejdź do zakładki `Network`,
3. odśwież stronę,
4. wybierz żądanie dotyczące dokumentu.

Powinniśmy zobaczyć m.in.:

```text
Request URL: http://127.0.0.1:8000/
Request Method: GET
```

Czyli dokładnie:

```text
przeglądarka
     |
     | GET /
     v
prosty serwer HTTP
     |
     | index.html
     v
przeglądarka
```

Na późniejszych zajęciach prosty serwer zostanie zastąpiony przez **Django**.

---

# 11. Nagłówki

HTML posiada nagłówki:

```html
<h1>Najważniejszy nagłówek</h1>
<h2>Nagłówek drugiego poziomu</h2>
<h3>Nagłówek trzeciego poziomu</h3>
```

Nagłówki opisują **strukturę treści**, a nie wyłącznie jej wygląd.

W naszej Bibliotece:

```html
<h1>Biblioteka</h1>

<h2>Polecane książki</h2>

<h3>Solaris</h3>
```

`<h1>` powinien opisywać najważniejszy temat strony.

---

# 12. Akapity

Do zwykłych fragmentów tekstu używamy:

```html
<p>Treść akapitu.</p>
```

Przykład:

```html
<p>
    Witaj w przykładowej aplikacji Biblioteka.
    Projekt będzie rozwijany przez cały semestr.
</p>
```

Nie używamy wielu znaczników `<br>` tylko po to, aby tworzyć odstępy pomiędzy fragmentami strony.

Wyglądem zajmiemy się później w CSS.

---

# 13. Listy

## Lista nieuporządkowana

```html
<ul>
    <li>Solaris</li>
    <li>Lalka</li>
    <li>Hobbit</li>
</ul>
```

Stosujemy ją wtedy, gdy kolejność elementów nie jest najważniejsza.

## Lista uporządkowana

```html
<ol>
    <li>Wybierz książkę.</li>
    <li>Zaloguj się.</li>
    <li>Wypożycz książkę.</li>
</ol>
```

Tutaj kolejność ma znaczenie.

---

# 14. Linki

Link tworzymy znacznikiem:

```html
<a href="ADRES">tekst linku</a>
```

Przykład:

```html
<a href="https://www.djangoproject.com/">
    Django
</a>
```

Możemy również przygotować odnośnik do miejsca na naszej stronie.

Najpierw nadajemy elementowi identyfikator:

```html
<section id="books">
```

a potem tworzymy link:

```html
<a href="#books">Książki</a>
```

Po kliknięciu przeglądarka przewinie stronę do elementu:

```text
id="books"
```

---


# 15. Obrazy

Obraz dodajemy za pomocą znacznika:

```html
<img src="ADRES_OBRAZU" alt="OPIS_OBRAZU">
```

Przykład:

```html
<img
    src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Book_icon_1.svg/512px-Book_icon_1.svg.png"
    alt="Ikona książki"
>
```

Najważniejsze atrybuty:

```text
src  — określa źródło obrazu
alt  — opisuje obraz
```

Atrybut `alt` jest ważny m.in. dla:

- dostępności,
- czytników ekranowych,
- sytuacji, gdy obraz nie może zostać załadowany.

W prawdziwym projekcie najczęściej będziemy korzystać z obrazów zapisanych lokalnie.

Utwórz katalog:

```text
frontend/images/
```

Docelowa struktura może wyglądać tak:

```text
frontend/
│
├── images/
│   └── library.jpg
│
└── index.html
```

Jeżeli w katalogu `images` znajduje się plik:

```text
library.jpg
```

możemy wyświetlić go tak:

```html
<img src="images/library.jpg" alt="Wnętrze biblioteki">
```

> [!NOTE]
> Plik obrazu musi rzeczywiście znajdować się pod wskazaną ścieżką.
>
> Na kolejnych zajęciach nauczymy się zmieniać rozmiar i wygląd obrazów za pomocą CSS.

---

# 16. Tabele

Tabele wykorzystujemy do prezentowania **danych tabelarycznych**.

Nie powinny służyć do budowania układu całej strony.

Przykład:

```html
<table>
    <thead>
        <tr>
            <th>Tytuł</th>
            <th>Autor</th>
            <th>Rok wydania</th>
        </tr>
    </thead>

    <tbody>
        <tr>
            <td>Solaris</td>
            <td>Stanisław Lem</td>
            <td>1961</td>
        </tr>

        <tr>
            <td>Lalka</td>
            <td>Bolesław Prus</td>
            <td>1890</td>
        </tr>
    </tbody>
</table>
```

Najważniejsze elementy:

```text
<table>   — cała tabela
<thead>   — nagłówek tabeli
<tbody>   — właściwe dane
<tr>      — wiersz
<th>      — komórka nagłówkowa
<td>      — zwykła komórka
```

W naszej Bibliotece tabela może później służyć np. do prezentowania:

```text
listy książek
listy wypożyczeń
listy użytkowników
```

---

# 17. Element div

`<div>` jest uniwersalnym kontenerem.

Przykład:

```html
<div>
    <h3>Solaris</h3>
    <p>Autor: Stanisław Lem</p>
</div>
```

Sam `div` nie mówi jednak nic o znaczeniu swojej zawartości.

Dlatego jeżeli istnieje bardziej odpowiedni element semantyczny, zwykle lepiej go użyć.

Zamiast:

```html
<div>
    <h2>Polecane książki</h2>
</div>
```

lepiej:

```html
<section>
    <h2>Polecane książki</h2>
</section>
```

Zamiast:

```html
<div>
    <h3>Solaris</h3>
    <p>Autor: Stanisław Lem</p>
</div>
```

w naszym przypadku lepiej użyć:

```html
<article>
    <h3>Solaris</h3>
    <p>Autor: Stanisław Lem</p>
</article>
```

`div` będzie jednak przydatny wtedy, gdy potrzebujemy po prostu **zgrupować kilka elementów**, np. na potrzeby CSS:

```html
<div class="book-info">
    <p><strong>Autor:</strong> Stanisław Lem</p>
    <p><strong>Rok:</strong> 1961</p>
</div>
```

Do atrybutu `class` wrócimy dokładniej podczas CSS.

---
# 18. Podstawowe elementy semantyczne

Możemy zbudować całą stronę za pomocą wielu elementów `<div>`.

Warto jednak używać elementów, których nazwy opisują ich przeznaczenie.

Podczas zajęć będziemy korzystali m.in. z:

```html
<header>
<nav>
<main>
<section>
<article>
<footer>
```

Można je rozumieć następująco:

```text
header   — górna część strony
nav      — nawigacja
main     — główna zawartość
section  — większa część tematyczna
article  — samodzielny fragment treści
footer   — stopka strony
```

Przeglądarka nie sprawi automatycznie, że będą wyglądały „ładnie”.

Ich znaczenie dotyczy przede wszystkim **struktury dokumentu**.

---

# 19. Budujemy stronę Biblioteki

Zastąp całą zawartość pliku:

```text
frontend/index.html
```

następującym kodem:

```html
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Biblioteka</title>
</head>
<body>

    <header>
        <h1>Biblioteka</h1>

        <p>
            Przykładowa aplikacja rozwijana podczas laboratoriów.
        </p>

        <nav>
            <a href="#about">O bibliotece</a>
            <a href="#books">Książki</a>
            <a href="#rules">Jak wypożyczyć?</a>
        </nav>
    </header>

    <main>

        <section id="about">
            <h2>O bibliotece</h2>

            <p>
                Witaj w przykładowej Bibliotece.
                W kolejnych laboratoriach ta prosta strona zostanie
                rozbudowana do aplikacji działającej w Django.
            </p>

            <p>
                Docelowo użytkownik będzie mógł przeglądać książki,
                wyświetlać ich szczegóły oraz wykonywać podstawowe
                operacje związane z wypożyczeniami.
            </p>

            <img
                src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Book_icon_1.svg/512px-Book_icon_1.svg.png"
                alt="Ikona książki"
                width="120"
            >
        </section>

        <section id="books">
            <h2>Polecane książki</h2>

            <table>
                <thead>
                    <tr>
                        <th>Tytuł</th>
                        <th>Autor</th>
                        <th>Rok</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Solaris</td>
                        <td>Stanisław Lem</td>
                        <td>1961</td>
                    </tr>
                    <tr>
                        <td>Lalka</td>
                        <td>Bolesław Prus</td>
                        <td>1890</td>
                    </tr>
                    <tr>
                        <td>Hobbit</td>
                        <td>J.R.R. Tolkien</td>
                        <td>1937</td>
                    </tr>
                </tbody>
            </table>

            <article>
                <h3>Solaris</h3>

                <div class="book-info">
                    <p>
                        <strong>Autor:</strong> Stanisław Lem
                    </p>

                    <p>
                        <strong>Rok wydania:</strong> 1961
                    </p>
                </div>

                <p>
                    Powieść science fiction opowiadająca o próbie
                    kontaktu człowieka z obcą formą inteligencji.
                </p>
            </article>

            <article>
                <h3>Lalka</h3>

                <p>
                    <strong>Autor:</strong> Bolesław Prus
                </p>

                <p>
                    Powieść przedstawiająca historię Stanisława
                    Wokulskiego oraz społeczeństwo XIX-wiecznej Warszawy.
                </p>
            </article>

            <article>
                <h3>Hobbit</h3>

                <p>
                    <strong>Autor:</strong> J.R.R. Tolkien
                </p>

                <p>
                    Historia wyprawy Bilba Bagginsa, który opuszcza
                    spokojny dom i wyrusza w niebezpieczną podróż.
                </p>
            </article>
        </section>

        <section id="rules">
            <h2>Jak wypożyczyć książkę?</h2>

            <ol>
                <li>Znajdź interesującą Cię książkę.</li>
                <li>Sprawdź, czy jest dostępna.</li>
                <li>Zaloguj się do aplikacji.</li>
                <li>Wybierz opcję wypożyczenia.</li>
            </ol>
        </section>

        <section>
            <h2>Planowane funkcje</h2>

            <ul>
                <li>lista książek,</li>
                <li>szczegóły książki,</li>
                <li>autorzy,</li>
                <li>dodawanie i edycja danych,</li>
                <li>logowanie użytkowników,</li>
                <li>wypożyczenia.</li>
            </ul>
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

Zapisz plik i odśwież przeglądarkę.

---

# 20. Co właśnie zbudowaliśmy?

Struktura strony wygląda w uproszczeniu tak:

```text
body
│
├── header
│   ├── h1
│   ├── p
│   └── nav
│
├── main
│   │
│   ├── section #about
│   │   └── img
│   │
│   ├── section #books
│   │   ├── table
│   │   ├── article
│   │   │   └── div .book-info
│   │   ├── article
│   │   └── article
│   │
│   ├── section #rules
│   │
│   └── section
│
└── footer
```

Taki zapis pokazuje, że elementy HTML tworzą **strukturę drzewa**.

Do tego wrócimy dokładniej podczas omawiania DOM.

---

# 21. Kilka nowych elementów z kodu

## `<strong>`

```html
<strong>Autor:</strong>
```

Oznacza tekst o większym znaczeniu.

Przeglądarka domyślnie wyświetla go zwykle jako pogrubiony.

---

## Atrybut `id`

```html
<section id="books">
```

`id` identyfikuje konkretny element dokumentu.

W jednym dokumencie nie powinniśmy używać tego samego `id` dla kilku różnych elementów.

Dzięki:

```html
<a href="#books">Książki</a>
```

możemy przejść do elementu:

```html
<section id="books">
```

---

## Obraz w gotowej stronie

W przykładzie użyliśmy również:

```html
<img
    src="..."
    alt="Ikona książki"
    width="120"
>
```

Atrybut:

```text
width="120"
```

ustawia szerokość obrazu na 120 pikseli.

Jest to wygodne na początku, ale później rozmiar obrazów będziemy kontrolować głównie za pomocą CSS.

---

## Tabela

Tabela w sekcji książek pokazuje te same dane w bardziej zwartej formie.

Na tym etapie celowo nie dodajemy jej żadnych stylów — wyglądem zajmiemy się podczas CSS.

---

## div

W danych książki użyliśmy:

```html
<div class="book-info">
```

`div` grupuje informacje o autorze i roku wydania.

Klasa:

```text
book-info
```

nie zmienia jeszcze wyglądu elementu.

Będzie bardzo przydatna na zajęciach z CSS.

---

# 22. Sprawdzamy strukturę w DevTools

Przy uruchomionej stronie:

1. naciśnij `F12`,
2. otwórz `Elements` / `Inspector`,
3. znajdź `<body>`,
4. rozwiń kolejno:
   - `<header>`,
   - `<main>`,
   - `<section id="books">`,
   - jeden z elementów `<article>`.

Spróbuj wskazać na stronie element odpowiadający fragmentowi:

```html
<h3>Solaris</h3>
```

Następnie znajdź element:

```html
<nav>
```

Widzimy tutaj pierwszy raz praktycznie zależność:

```text
kod HTML
   |
   v
struktura strony widziana przez przeglądarkę
```

Na następnym laboratorium rozwiniemy ten temat jako **DOM**.

---

# 23. Mała modyfikacja podczas zajęć

Dodaj do sekcji książek czwartą książkę.

Przykład:

```html
<article>
    <h3>Mały Książę</h3>

    <div class="book-info">
        <p>
            <strong>Autor:</strong> Antoine de Saint-Exupéry
        </p>

        <p>
            <strong>Rok wydania:</strong> 1943
        </p>
    </div>

    <p>
        Opowieść o podróży Małego Księcia i jego spotkaniach
        z mieszkańcami różnych planet.
    </p>
</article>
```

Dodaj również odpowiedni wiersz do tabeli:

```html
<tr>
    <td>Mały Książę</td>
    <td>Antoine de Saint-Exupéry</td>
    <td>1943</td>
</tr>
```

Odśwież stronę.

Nowe elementy powinny pojawić się bez konieczności restartowania prostego serwera.

---

# 24. Zapisujemy zmiany w Git

Sprawdź:

```bash
git status
```

Powinien pojawić się nowy katalog:

```text
frontend/
```

Dodaj zmiany:

```bash
git add .
```

Utwórz commit:

```bash
git commit -m "Dodanie pierwszej strony HTML Biblioteki"
```

Wyślij zmiany:

```bash
git push
```

Na GitHubie powinien pojawić się:

```text
frontend/index.html
```

---

# 25. Aktualna struktura projektu

Po laboratorium projekt powinien wyglądać mniej więcej tak:

```text
paw_biblioteka/
│
├── .gitignore
├── README.md
├── check_environment.py
│
└── frontend/
    └── index.html
```

Plik:

```text
frontend/index.html
```

jest obecnie statycznym prototypem strony.

Nie posiada jeszcze:

- CSS,
- formularzy,
- bazy danych,
- Django,
- dynamicznych danych.

Będziemy dodawali te elementy stopniowo.

---

# 26. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Przykładowe rozwiązanie zadań programistycznych zostanie krótko pokazane na początku kolejnego laboratorium.

---

## Zadanie 1 — godziny otwarcia

Dodaj na stronie nową sekcję:

```text
Godziny otwarcia
```

Powinna zawierać listę:

```text
Poniedziałek – 8:00–16:00
Wtorek – 8:00–16:00
Środa – 10:00–18:00
Czwartek – 10:00–18:00
Piątek – 8:00–14:00
```

Użyj odpowiedniego nagłówka i listy.

---

## Zadanie 2 — kolejna sekcja i nawigacja

Dodaj sekcję:

```text
Kontakt
```

z przykładowymi danymi:

```text
Biblioteka Akademicka
ul. Przykładowa 1
10-000 Olsztyn
```

Nadaj sekcji:

```html
id="contact"
```

Następnie dodaj do `<nav>` link:

```text
Kontakt
```

który przewija stronę do tej sekcji.

---

## Zadanie 3 — dodatkowe książki

Dodaj jeszcze przynajmniej dwie książki.

Każda książka powinna być osobnym:

```html
<article>
```

i posiadać:

- tytuł,
- autora,
- krótki opis.

---

## Zadanie 4 — tabela książek

Rozbuduj tabelę o kolumnę:

```text
Dostępna
```

Przykład:

```text
Tak
Nie
Tak
```

Dodaj również przynajmniej dwa nowe wiersze.

---

## Zadanie 5 — obraz lokalny

Znajdź dowolny obraz, którego możesz użyć do ćwiczenia.

Zapisz go w katalogu:

```text
frontend/images/
```

Dodaj go na stronie za pomocą:

```html
<img src="images/NAZWA_PLIKU" alt="Krótki opis">
```

Sprawdź, co stanie się po celowym wpisaniu błędnej nazwy pliku.

---

## Zadanie 6 — własna wersja strony

Wymyśl prostą stronę odpowiadającą jednemu z możliwych tematów projektu zaliczeniowego, np.:

```text
planer nauki
katalog filmów
rejestr treningów
lista wydarzeń
```

Utwórz osobny plik, np.:

```text
frontend/practice.html
```

Powinien zawierać minimum:

```text
header
nav
main
2 x section
2 x article
footer
```

Treść powinna odpowiadać wybranemu tematowi.

---

# 27. Co dalej?

Na kolejnym laboratorium zajmiemy się bardziej szczegółowo strukturą dokumentu.

Rozwiniemy:

```text
DOM
```

oraz przygotujemy:

```text
formularze HTML
```

Dzięki temu nasza statyczna Biblioteka zacznie przypominać interfejs prawdziwej aplikacji, do której użytkownik może również **wprowadzać dane**, a nie tylko je przeglądać.
