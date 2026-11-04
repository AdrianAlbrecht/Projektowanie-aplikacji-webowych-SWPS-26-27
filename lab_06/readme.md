# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 06 — Flexbox, układ strony i podstawy responsywności

---

## 1. Cel zajęć

Na poprzednich zajęciach dodaliśmy do projektu podstawowe style CSS.

Dzisiaj skupimy się na **układzie strony**.

Po zakończeniu laboratorium powinniśmy:

- rozumieć, czym jest Flexbox,
- znać podstawowe właściwości kontenera flex,
- potrafić układać elementy poziomo i pionowo,
- potrafić rozkładać przestrzeń pomiędzy elementami,
- rozumieć działanie `gap`,
- potrafić tworzyć prosty układ kart,
- potrafić poprawić układ nawigacji,
- znać podstawową ideę responsywności,
- potrafić wykorzystać prostą regułę `@media`,
- przygotować bardziej elastyczny wygląd projektu Biblioteka.

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

Na końcu `lab05` znajdowało się pięć nieobowiązkowych zadań.

Poniżej znajduje się przykładowe rozwiązanie każdego z nich.

---

## Zadanie 1 — wyróżnienie kategorii książek

Przykładowy HTML:

```html
<article class="book-card fantasy">
    <h3>Hobbit</h3>

    <div class="book-info">
        <p>
            <strong>Autor:</strong>
            J.R.R. Tolkien
        </p>
    </div>

    <p>
        Historia wyprawy Bilba Bagginsa.
    </p>
</article>

<article class="book-card science-fiction">
    <h3>Solaris</h3>

    <div class="book-info">
        <p>
            <strong>Autor:</strong>
            Stanisław Lem
        </p>
    </div>

    <p>
        Powieść science fiction.
    </p>
</article>
```

Przykładowy CSS:

```css
.fantasy {
    border-left: 6px solid #7b4fa3;
}

.science-fiction {
    border-left: 6px solid #2f7c9f;
}
```

Możemy więc łączyć kilka klas:

```html
class="book-card fantasy"
```

---

## Zadanie 2 — styl formularza rejestracji

Przykładowo w `register.html` możemy nadać sekcji klasę:

```html
<section class="form-section">
```

CSS:

```css
.form-section {
    max-width: 650px;

    margin: 30px auto;
    padding: 25px;

    background-color: white;

    border: 1px solid #dddddd;
    border-radius: 8px;
}
```

Dzięki:

```css
margin: 30px auto;
```

sekcja zostanie wyśrodkowana.

---

## Zadanie 3 — link wyglądający jak przycisk

HTML:

```html
<a
    class="button-link"
    href="add_book.html"
>
    Dodaj książkę
</a>
```

CSS:

```css
.button-link {
    display: inline-block;

    padding: 10px 16px;

    background-color: #1f3c5c;
    color: white;

    text-decoration: none;

    border-radius: 4px;
}

.button-link:hover {
    background-color: #315c87;
}
```

`display: inline-block` pozwala stosować do linku m.in.:

```text
padding
szerokość
wysokość
```

podobnie jak do przycisku.

---

## Zadanie 4 — własna karta

Przykład dla planera nauki:

```html
<article class="task-card">
    <h3>Przygotować formularz HTML</h3>

    <p>
        Termin: 15 października
    </p>
</article>
```

CSS:

```css
.task-card {
    margin: 15px 0;
    padding: 15px;

    background-color: #ffffff;

    border: 1px solid #cccccc;
    border-radius: 8px;
}
```

---

## Zadanie 5 — zmiana przez DevTools

Przykładowo podczas pracy w DevTools można było zmienić:

```css
section {
    padding: 30px;
}
```

Jeżeli efekt wygląda dobrze, zapisujemy go następnie w:

```text
styles.css
```

czyli:

```css
section {
    background-color: white;

    margin: 20px 0;
    padding: 30px;

    border: 1px solid #dddddd;
    border-radius: 8px;
}
```

Zmiany wykonane wyłącznie w DevTools nadal znikają po odświeżeniu strony.

---

# 4. Problem z obecnym układem

Do tej pory elementy na stronie są układane głównie jeden pod drugim.

Przykład:

```text
Książka 1

Książka 2

Książka 3

Książka 4
```

To działa, ale czasem chcemy uzyskać układ:

```text
Książka 1    Książka 2    Książka 3
```

albo:

```text
Logo        Nawigacja
```

lub:

```text
Pole formularza        Przycisk
```

Do takich układów bardzo wygodny jest:

```text
Flexbox
```

---

# 5. Flexbox

Flexbox jest mechanizmem CSS służącym do układania elementów.

Najważniejsza zasada:

> Flexbox działa na **dzieciach elementu**, któremu ustawimy `display: flex`.

Przykład HTML:

```html
<div class="container">
    <div>Element 1</div>
    <div>Element 2</div>
    <div>Element 3</div>
</div>
```

CSS:

```css
.container {
    display: flex;
}
```

Elementy:

```text
Element 1
Element 2
Element 3
```

zamiast układać się pionowo, mogą zostać ułożone w jednym wierszu.

---

# 6. Prosty przykład

Utwórz tymczasowo w `index.html`:

```html
<section>
    <h2>Test Flexbox</h2>

    <div class="flex-test">
        <div>1</div>
        <div>2</div>
        <div>3</div>
    </div>
</section>
```

W CSS:

```css
.flex-test {
    display: flex;
}
```

Po odświeżeniu zobaczymy:

```text
1 2 3
```

---

# 7. gap

Elementy są bardzo blisko siebie.

Dodaj:

```css
.flex-test {
    display: flex;
    gap: 20px;
}
```

`gap` ustawia odstęp pomiędzy elementami wewnątrz kontenera flex.

Jest wygodniejszy niż ręczne dodawanie `margin` do każdego elementu.

---

# 8. flex-direction

Domyślnie Flexbox ustawia elementy:

```text
poziomo
```

czyli:

```css
flex-direction: row;
```

Możemy ustawić:

```css
.flex-test {
    display: flex;
    flex-direction: column;
}
```

Wtedy elementy wrócą do układu pionowego.

Najczęściej będziemy korzystali z:

```text
row
column
```

---

# 9. justify-content

`justify-content` odpowiada za rozmieszczenie elementów wzdłuż głównej osi Flexbox.

Przykład:

```css
.flex-test {
    display: flex;

    justify-content: center;
}
```

Elementy zostaną wyśrodkowane.

Inne często używane wartości:

```text
flex-start
center
flex-end
space-between
space-around
space-evenly
```

---

# 10. space-between

Przykład:

```css
.flex-test {
    display: flex;

    justify-content: space-between;
}
```

Efekt:

```text
1                       2                       3
```

Wolna przestrzeń zostanie rozłożona **pomiędzy elementami**.

To będzie szczególnie przydatne w nagłówku strony.

---

# 11. align-items

`align-items` odpowiada za ustawienie elementów na drugiej osi.

Przykład:

```css
.flex-test {
    display: flex;

    align-items: center;
}
```

Bardzo często spotkamy połączenie:

```css
display: flex;
justify-content: space-between;
align-items: center;
```

---

# 12. Poprawiamy nagłówek Biblioteki

Obecnie w `header` mamy np.:

```html
<header>
    <h1>Biblioteka</h1>

    <nav>
        ...
    </nav>
</header>
```

Chcemy, aby:

```text
Biblioteka                  menu
```

znalazły się w jednym wierszu.

W `styles.css` zmień `header` na:

```css
header {
    display: flex;

    justify-content: space-between;
    align-items: center;

    gap: 20px;

    background-color: #1f3c5c;
    color: white;

    padding: 20px;
}
```

Usuń lub zmień:

```css
nav {
    margin-top: 15px;
}
```

Możemy teraz ustawić:

```css
nav {
    display: flex;
    gap: 15px;
}
```

A z:

```css
nav a {
    margin-right: 15px;
}
```

możemy usunąć `margin-right`.

---

# 13. Aktualny CSS nagłówka

Może wyglądać tak:

```css
header {
    display: flex;

    justify-content: space-between;
    align-items: center;

    gap: 20px;

    background-color: #1f3c5c;
    color: white;

    padding: 20px;
}

header h1 {
    margin: 0;
}

nav {
    display: flex;
    gap: 15px;
}

nav a {
    color: white;
    text-decoration: none;
}

nav a:hover {
    text-decoration: underline;
}
```

---

# 14. Układ kart książek

Chcemy, aby książki mogły być wyświetlane obok siebie.

Najpierw musimy je zgrupować.

W `index.html` znajdź karty książek i otocz je:

```html
<div class="book-list">

    <article class="book-card science-fiction">
        ...
    </article>

    <article class="book-card">
        ...
    </article>

    <article class="book-card fantasy">
        ...
    </article>

</div>
```

Czyli:

```text
book-list
├── book-card
├── book-card
└── book-card
```

---

# 15. Flexbox dla kart

Dodaj:

```css
.book-list {
    display: flex;

    gap: 20px;
}
```

Karty powinny ustawić się obok siebie.

Może jednak pojawić się problem:

```text
elementy robią się zbyt wąskie
```

albo:

```text
nie mieszczą się na ekranie
```

---

# 16. flex-wrap

Domyślnie Flexbox próbuje umieścić wszystkie elementy w jednym wierszu.

Możemy pozwolić na przenoszenie elementów:

```css
.book-list {
    display: flex;

    gap: 20px;

    flex-wrap: wrap;
}
```

Teraz karta może przejść do kolejnego wiersza.

---

# 17. Ustawiamy szerokość kart

Możemy użyć:

```css
.book-card {
    flex: 1 1 250px;
}
```

Na tym etapie potraktujmy ten zapis praktycznie.

Oznacza on w uproszczeniu:

> karta może się rozszerzać, zmniejszać i ma preferowaną szerokość około 250 px.

Pełny przykład:

```css
.book-list {
    display: flex;

    gap: 20px;
    flex-wrap: wrap;
}

.book-card {
    flex: 1 1 250px;

    border: 1px solid #cccccc;
    border-radius: 6px;

    padding: 15px;

    background-color: #fafafa;
}
```

---

# 18. Ważna uwaga o flex

Nie trzeba zapamiętywać wszystkich możliwości Flexbox.

Na tych zajęciach najważniejsze są:

```css
display: flex;

flex-direction: row;
flex-direction: column;

justify-content: ...;

align-items: ...;

gap: ...;

flex-wrap: wrap;
```

oraz praktyczna umiejętność:

> potrafię ustawić kilka elementów w sensownym układzie.

---

# 19. Formularz wyszukiwania w jednym wierszu

Obecnie każde pole formularza wyszukiwania może być ułożone pionowo.

Dodaj do formularza klasę:

```html
<form
    class="search-form"
    method="get"
>
```

W CSS:

```css
.search-form {
    display: flex;

    gap: 15px;

    align-items: end;

    max-width: none;
}
```

---

# 20. Kontenery pól

Wewnątrz formularza mamy:

```html
<div>
    <label>...</label>
    <input ...>
</div>
```

Dodaj:

```css
.search-form div {
    flex: 1;
    margin-bottom: 0;
}
```

Dla przycisku możemy dodać:

```css
.search-form button {
    flex: 0 0 auto;
}
```

Efekt:

```text
Tytuł          Kategoria        Rok od         [Szukaj]
[________]     [________]       [____]
```

---

# 21. Problem małego ekranu

Na szerokim ekranie formularz wygląda dobrze.

Na małym ekranie może wyglądać źle:

```text
bardzo wąskie pola
```

To prowadzi nas do pojęcia:

```text
responsywność
```

---

# 22. Responsywność

Responsywna strona dostosowuje swój układ do wielkości ekranu.

Przykładowo:

### Duży ekran

```text
Karta 1    Karta 2    Karta 3
```

### Mniejszy ekran

```text
Karta 1    Karta 2

Karta 3
```

### Telefon

```text
Karta 1

Karta 2

Karta 3
```

Część tego zachowania otrzymujemy już dzięki:

```css
flex-wrap: wrap;
```

---

# 23. Media query

Możemy również ustalić inne style dla mniejszych ekranów.

Przykład:

```css
@media (max-width: 700px) {

    header {
        flex-direction: column;
        align-items: flex-start;
    }

}
```

Oznacza:

> jeżeli szerokość ekranu wynosi maksymalnie 700 px, zastosuj te reguły.

---

# 24. Responsywna nawigacja

Dodaj na końcu `styles.css`:

```css
@media (max-width: 700px) {

    header {
        flex-direction: column;
        align-items: flex-start;
    }

    nav {
        flex-direction: column;
    }

}
```

Zmniejsz szerokość okna przeglądarki.

Nawigacja powinna przejść do układu pionowego.

---

# 25. Responsywny formularz

Rozbuduj media query:

```css
@media (max-width: 700px) {

    header {
        flex-direction: column;
        align-items: flex-start;
    }

    nav {
        flex-direction: column;
    }

    .search-form {
        flex-direction: column;
        align-items: stretch;
    }

}
```

Teraz na mniejszym ekranie pola formularza ustawią się jedno pod drugim.

---

# 26. Pełne media query

Możemy użyć:

```css
@media (max-width: 700px) {

    header {
        flex-direction: column;
        align-items: flex-start;
    }

    nav {
        flex-direction: column;
    }

    .search-form {
        flex-direction: column;
        align-items: stretch;
    }

    main {
        width: 95%;
    }

}
```

To wystarczy jako podstawowy przykład responsywności.

Nie będziemy budowali rozbudowanego systemu wielu breakpointów.

---

# 27. Test responsywności w DevTools

W DevTools można włączyć widok urządzenia mobilnego.

Najczęściej służy do tego ikona:

```text
telefon / tablet
```

lub skrót zależny od przeglądarki.

Włącz tryb urządzenia mobilnego i sprawdź:

```text
nagłówek
nawigację
formularz wyszukiwania
karty książek
tabelę
```

Nie wszystko musi wyglądać idealnie.

Celem jest zauważenie, że:

```text
strona na dużym monitorze
```

i:

```text
strona na telefonie
```

nie powinny być traktowane identycznie.

---

# 28. Tabela na małym ekranie

Tabela może być problematyczna na telefonie.

Możemy ją umieścić w kontenerze:

```html
<div class="table-container">

    <table>
        ...
    </table>

</div>
```

CSS:

```css
.table-container {
    overflow-x: auto;
}
```

Jeżeli tabela jest zbyt szeroka, użytkownik może ją przewijać poziomo zamiast niszczyć cały układ strony.

---

# 29. Flexbox Froggy

Praktycznym ćwiczeniem dotyczącym Flexbox jest:

[**Flexbox Froggy**](https://flexboxfroggy.com/#pl)

Gra polega na ustawianiu żab w odpowiednich miejscach za pomocą właściwości Flexbox.

Podczas zajęć wykonaj przynajmniej kilka pierwszych poziomów.

W szczególności zwróć uwagę na:

```text
justify-content
align-items
flex-direction
```

Jeżeli tempo pracy pozwala, spróbuj przejść całą grę. Jeżeli nie - warto poćwiczyć w domu.

---

# 30. Modyfikacja Biblioteki — gotowy układ

Po zmianach kluczowe elementy `styles.css` mogą wyglądać następująco:

```css
body {
    margin: 0;

    background-color: #f5f5f5;
    color: #222222;

    font-family: Arial, Helvetica, sans-serif;
}

header {
    display: flex;

    justify-content: space-between;
    align-items: center;

    gap: 20px;

    background-color: #1f3c5c;
    color: white;

    padding: 20px;
}

header h1 {
    margin: 0;
}

nav {
    display: flex;
    gap: 15px;
}

nav a {
    color: white;
    text-decoration: none;
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

.book-list {
    display: flex;

    gap: 20px;
    flex-wrap: wrap;
}

.book-card {
    flex: 1 1 250px;

    border: 1px solid #cccccc;
    border-radius: 6px;

    padding: 15px;

    background-color: #fafafa;
}

.book-card h3 {
    margin-top: 0;
}

.book-info {
    color: #555555;
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

.table-container {
    overflow-x: auto;
}

form {
    max-width: 600px;
}

form div {
    margin-bottom: 15px;
}

.search-form {
    display: flex;

    gap: 15px;

    align-items: end;

    max-width: none;
}

.search-form div {
    flex: 1;
    margin-bottom: 0;
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

button,
.button-link {
    display: inline-block;

    background-color: #1f3c5c;
    color: white;

    border: none;
    border-radius: 4px;

    padding: 10px 16px;

    text-decoration: none;

    cursor: pointer;
}

button:hover,
.button-link:hover {
    background-color: #315c87;
}

footer {
    margin-top: 30px;
    padding: 20px;

    text-align: center;

    background-color: #eeeeee;
}

@media (max-width: 700px) {

    header {
        flex-direction: column;
        align-items: flex-start;
    }

    nav {
        flex-direction: column;
    }

    .search-form {
        flex-direction: column;
        align-items: stretch;
    }

    main {
        width: 95%;
    }

}
```

Nie trzeba przepisywać wszystkiego od nowa.

Wprowadzamy zmiany do istniejącego:

```text
styles.css
```

---

# 31. Mała modyfikacja podczas zajęć

W sekcji z książkami dodaj przy każdej książce link:

```html
<a
    class="button-link"
    href="#"
>
    Szczegóły
</a>
```

Na razie:

```text
href="#"
```

nie prowadzi do prawdziwej strony szczegółów.

Później w Django zamienimy go na prawdziwy adres konkretnej książki.

Ustaw przycisk na dole karty za pomocą:

```css
.book-card {
    display: flex;
    flex-direction: column;
}
```

oraz:

```css
.book-card .button-link {
    margin-top: auto;
}
```

Dzięki temu link może być ustawiony bliżej dolnej części karty niezależnie od długości opisu.

---

# 32. Aktualna struktura projektu

Po dzisiejszych zajęciach struktura plików nadal wygląda podobnie:

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
    ├── register.html
    └── practice.html
```

Największe zmiany dotyczą:

```text
HTML
+ klasy potrzebne do układu

CSS
+ Flexbox
+ responsywność
```

---

# 33. Zapisujemy zmiany w Git

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
git commit -m "Dodanie układu Flexbox i podstaw responsywności"
```

Wyślij:

```bash
git push
```

---

# 34. Projekt zaliczeniowy — temat

Zgodnie z zasadami przedmiotu **do końca tych zajęć należy ustalić temat projektu zaliczeniowego**.

Projekt może być wykonywany:

```text
samodzielnie
```

lub:

```text
w parze
```

Na tym etapie wystarczy krótki opis:

```text
Temat:

Autor / autorzy:

Co będzie przechowywała aplikacja?

Jakie będą 3–5 podstawowych funkcji?

Jakie mogą być 2–3 główne rodzaje danych?
```

Przykład:

```text
Temat:
Planer nauki

Autorzy:
Jan Kowalski
Anna Nowak

Dane:
- przedmiot
- zadanie
- termin

Podstawowe funkcje:
- wyświetlenie zadań
- dodanie zadania
- edycja zadania
- usunięcie zadania
- oznaczenie jako wykonane
```

Nie trzeba jeszcze przygotowywać modeli Django ani bazy danych.

Chodzi o ustalenie **realnego i możliwego do wykonania zakresu**.

---

# 35. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Przykładowe rozwiązania zostaną pokazane na początku następnego laboratorium.

---

## Zadanie 1 — responsywne karty

Sprawdź działanie:

```css
flex: 1 1 250px;
```

Następnie zmień `250px` na:

```text
180px
350px
500px
```

Zaobserwuj, jak zmienia się liczba kart mieszczących się w jednym wierszu.

Na końcu wybierz wartość, która Twoim zdaniem wygląda najlepiej.

---

## Zadanie 2 — nawigacja

Zmodyfikuj nawigację tak, aby:

- na dużym ekranie była pozioma,
- na małym ekranie była pionowa,
- linki posiadały większy obszar klikalny.

Możesz dodać do linków:

```css
padding
```

---

## Zadanie 3 — sekcja autorów

Dodaj do `index.html` sekcję:

```text
Polecani autorzy
```

Dodaj przynajmniej trzy elementy.

Przygotuj układ za pomocą Flexbox, tak aby:

```text
duży ekran
= autorzy obok siebie

mały ekran
= elementy mogą przechodzić do kolejnego wiersza
```

---

## Zadanie 4 — własny projekt

Dla `practice.html` przygotuj układ Flexbox odpowiadający tematowi strony.

Przykład dla planera:

```text
karty zadań obok siebie
```

Przykład dla katalogu filmów:

```text
karty filmów
```

Przykład dla wydarzeń:

```text
karty wydarzeń
```

Użyj minimum:

```css
display: flex;
gap: ...;
flex-wrap: wrap;
```

---

## Zadanie 5 — własna media query

Dodaj własną regułę:

```css
@media (max-width: ...)
```

która zmienia przynajmniej dwa elementy strony.

Przykładowo:

```text
zmniejsza padding
zmienia układ flex
zmienia szerokość main
```

---

# 36. Co dalej?

Na tym etapie zakończyliśmy podstawowy blok frontendowy.

Mamy:

```text
HTML
DOM
formularze
CSS
Flexbox
podstawy responsywności
```

Od kolejnego laboratorium zaczynamy:

```text
Django
```

Po raz pierwszy utworzymy właściwy projekt backendowy.

Zobaczymy m.in.:

```text
django-admin
manage.py
projekt Django
aplikację Django
runserver
routing
pierwszy view
```

Statyczny frontend przygotowany podczas ostatnich laboratoriów nie zostanie wyrzucony.

Będziemy go stopniowo przenosili do:

```text
szablonów Django
```

Dzięki temu projekt Biblioteka zacznie przechodzić z:

```text
statycznej strony HTML
```

do:

```text
działającej aplikacji webowej
```
