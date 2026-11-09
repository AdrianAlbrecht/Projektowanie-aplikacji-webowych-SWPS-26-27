# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 07 — Django: pierwszy projekt, routing i widoki

---

## 1. Cel zajęć

Na poprzednich zajęciach zakończyliśmy podstawowy blok frontendowy.

Mamy już statyczny prototyp Biblioteki przygotowany w:

```text
HTML
CSS
```

Dzisiaj rozpoczynamy pracę z:

```text
Django
```

i po raz pierwszy uruchomimy właściwą aplikację backendową.

Po zakończeniu laboratorium powinniśmy:

- zainstalować Django w środowisku wirtualnym,
- utworzyć projekt Django w istniejącym repozytorium,
- rozumieć podstawową strukturę projektu,
- znać rolę `manage.py`,
- utworzyć pierwszą aplikację Django,
- uruchomić serwer deweloperski,
- utworzyć własny routing,
- utworzyć pierwszy widok,
- zobaczyć zależność:

```text
URL -> routing -> view -> response
```

Na tym etapie **nie korzystamy jeszcze z szablonów ani bazy danych**.

Najpierw chcemy dobrze zobaczyć najprostszy przepływ żądania przez Django.

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

Przejdź do głównego katalogu projektu:

```bash
cd paw_biblioteka
```

Nie uruchamiamy już:

```bash
python -m http.server
```

Od dzisiaj serwer będzie uruchamiany przez Django.

---

# 3. Przykładowe rozwiązania z poprzedniego laboratorium

Na końcu `lab06` znajdowało się pięć nieobowiązkowych zadań.

Poniżej znajduje się przykładowe rozwiązanie każdego z nich.

---

## Zadanie 1 — responsywne karty

W poprzednim labie mieliśmy:

```css
.book-card {
    flex: 1 1 250px;
}
```

Możemy zmienić preferowaną szerokość kart np. na:

```css
.book-card {
    flex: 1 1 350px;
}
```

Przy większej wartości w jednym wierszu zmieści się mniej kart.

Przykładowe rozwiązanie:

```css
.book-list {
    display: flex;

    gap: 20px;
    flex-wrap: wrap;
}

.book-card {
    flex: 1 1 350px;

    padding: 15px;

    background-color: #fafafa;

    border: 1px solid #cccccc;
    border-radius: 6px;
}
```

Nie istnieje jedna obowiązkowa wartość.

Ważne jest, aby rozumieć, że zmiana:

```text
180px
250px
350px
500px
```

wpływa na preferowaną szerokość elementu i liczbę kart mieszczących się obok siebie.

---

## Zadanie 2 — nawigacja

Przykładowe rozwiązanie:

```css
nav {
    display: flex;
    gap: 5px;
}

nav a {
    display: block;

    padding: 10px 12px;

    color: white;

    text-decoration: none;

    border-radius: 4px;
}

nav a:hover {
    background-color: #315c87;
}

@media (max-width: 700px) {

    nav {
        flex-direction: column;
        width: 100%;
    }

    nav a {
        width: 100%;

        box-sizing: border-box;
    }

}
```

Dzięki `padding` użytkownik nie musi kliknąć dokładnie w sam tekst linku.

Klikalny jest większy obszar.

---

## Zadanie 3 — sekcja autorów

Przykładowy HTML:

```html
<section id="authors">
    <h2>Polecani autorzy</h2>

    <div class="author-list">

        <article class="author-card">
            <h3>Stanisław Lem</h3>

            <p>
                Polski pisarz science fiction,
                autor m.in. powieści „Solaris”.
            </p>
        </article>

        <article class="author-card">
            <h3>J.R.R. Tolkien</h3>

            <p>
                Autor powieści fantasy,
                m.in. „Hobbita”.
            </p>
        </article>

        <article class="author-card">
            <h3>George Orwell</h3>

            <p>
                Brytyjski pisarz,
                autor m.in. powieści „1984”.
            </p>
        </article>

    </div>
</section>
```

Przykładowy CSS:

```css
.author-list {
    display: flex;

    gap: 20px;
    flex-wrap: wrap;
}

.author-card {
    flex: 1 1 220px;

    padding: 15px;

    background-color: #fafafa;

    border: 1px solid #cccccc;
    border-radius: 6px;
}
```

Dzięki:

```css
flex-wrap: wrap;
```

karty mogą przechodzić do kolejnego wiersza.

---

## Zadanie 4 — Flexbox we własnym projekcie

Przykład dla planera nauki.

HTML:

```html
<section>
    <h2>Zadania</h2>

    <div class="task-list">

        <article class="task-card">
            <h3>Formularze HTML</h3>

            <p>
                Termin: 15 października
            </p>
        </article>

        <article class="task-card">
            <h3>Powtórka ze statystyki</h3>

            <p>
                Termin: 18 października
            </p>
        </article>

        <article class="task-card">
            <h3>Przygotowanie prezentacji</h3>

            <p>
                Termin: 20 października
            </p>
        </article>

    </div>
</section>
```

CSS:

```css
.task-list {
    display: flex;

    gap: 20px;
    flex-wrap: wrap;
}

.task-card {
    flex: 1 1 250px;

    padding: 15px;

    background-color: white;

    border: 1px solid #cccccc;
    border-radius: 8px;
}
```

---

## Zadanie 5 — własna media query

Przykładowe rozwiązanie:

```css
@media (max-width: 600px) {

    main {
        width: 95%;
    }

    section {
        padding: 12px;
    }

    .book-list {
        flex-direction: column;
    }

    .author-list {
        flex-direction: column;
    }

}
```

Na ekranie o szerokości maksymalnie:

```text
600 px
```

zmniejszamy odstępy i zmieniamy układ kart.

---

# 4. Co zmienia Django?

Do tej pory korzystaliśmy z:

```bash
python -m http.server
```

Serwer potrafił:

```text
otrzymać żądanie
znaleźć plik
odesłać plik
```

Nie potrafił jednak wykonywać logiki naszej aplikacji.

Przykładowo nie potrafił:

```text
pobrać książek z bazy danych
sprawdzić formularza
zalogować użytkownika
zapisać nowej książki
wybrać danych dla konkretnego użytkownika
```

Django będzie działało pomiędzy:

```text
przeglądarką
```

a:

```text
danymi i logiką aplikacji
```

W uproszczeniu:

```text
przeglądarka
     |
     | request
     v
   Django
     |
     | logika aplikacji
     v
 response
     |
     v
przeglądarka
```

---

# 5. Aktywujemy środowisko wirtualne

W głównym katalogu projektu powinniśmy mieć:

```text
.venv
```

Na Windows — CMD:

```cmd
.venv\Scripts\activate.bat
```

Na Windows — PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Linux / macOS:

```bash
source .venv/bin/activate
```

Po aktywacji powinno być widoczne:

```text
(.venv)
```

Jeżeli `.venv` nie istnieje na danym komputerze:

```bash
python -m venv .venv
```

lub:

```bash
py -m venv .venv
```

i dopiero potem aktywujemy środowisko.

---

# 6. Instalujemy Django

Na zajęciach korzystamy z serii:

```text
Django 5.2.x
```

W aktywnym środowisku wykonaj:

```bash
python -m pip install "Django>=5.2,<5.3"
```

Sprawdź wersję:

```bash
python -m django --version
```

Powinna pojawić się wersja:

```text
5.2.x
```

---

# 7. requirements.txt

Dobrą praktyką jest zapisanie bibliotek wymaganych przez projekt.

Wykonaj:

```bash
python -m pip freeze > requirements.txt
```

W pliku:

```text
requirements.txt
```

pojawią się zainstalowane pakiety.

Dzięki temu na innym komputerze będzie można później wykonać:

```bash
python -m pip install -r requirements.txt
```

zamiast instalować każdą bibliotekę osobno.

---

# 8. Tworzymy projekt Django

Jesteśmy w katalogu:

```text
paw_biblioteka
```

Wykonaj:

```bash
django-admin startproject config .
```

Kropka na końcu jest ważna:

```text
.
```

oznacza:

> utwórz projekt Django w bieżącym katalogu.

Po wykonaniu polecenia pojawią się nowe pliki:

```text
paw_biblioteka/
│
├── config/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── frontend/
│   └── ...
│
├── manage.py
├── requirements.txt
├── README.md
├── .gitignore
└── ...
```

---

# 9. Projekt Django a aplikacja Django

W Django słowa:

```text
projekt
```

i:

```text
aplikacja
```

mają konkretne znaczenie.

## Projekt

Projekt opisuje konfigurację całej aplikacji webowej.

W naszym przypadku:

```text
config/
```

zawiera ustawienia całego projektu.

## Aplikacja

Aplikacja Django odpowiada za konkretną część funkcjonalności.

Za chwilę utworzymy aplikację:

```text
biblioteka
```

Możemy to uprościć:

```text
projekt Django
│
├── konfiguracja całego systemu
│
└── jedna lub więcej aplikacji
```

W małym projekcie może istnieć tylko jedna własna aplikacja Django.

I to jest całkowicie poprawne.

---

# 10. manage.py

W katalogu głównym pojawił się:

```text
manage.py
```

Jest to plik służący do wykonywania poleceń związanych z projektem Django.

Będziemy bardzo często korzystali z poleceń:

```bash
python manage.py runserver
```

```bash
python manage.py startapp ...
```

a później także:

```bash
python manage.py makemigrations
```

```bash
python manage.py migrate
```

Nie modyfikujemy `manage.py` podczas zwykłej pracy.

---

# 11. Uruchamiamy Django po raz pierwszy

Wykonaj:

```bash
python manage.py runserver
```

Powinien pojawić się adres:

```text
http://127.0.0.1:8000/
```

Otwórz go w przeglądarce.

Jeżeli wszystko działa poprawnie, Django wyświetli swoją stronę startową.

Możemy też zobaczyć komunikat o niezastosowanych migracjach.

Na tym etapie nie jest to problem.

Do migracji wrócimy przy omawianiu bazy danych.

Serwer zatrzymujemy:

```text
Ctrl + C
```

---

# 12. Serwer deweloperski

Polecenie:

```bash
python manage.py runserver
```

uruchamia **serwer deweloperski**.

Służy on do:

```text
tworzenia aplikacji
testowania aplikacji
pracy lokalnej
```

Nie jest przeznaczony do uruchamiania produkcyjnej aplikacji dla prawdziwych użytkowników.

Na naszych laboratoriach jest dokładnie tym, czego potrzebujemy.

---

# 13. Pierwsza aplikacja Django

Zatrzymaj serwer:

```text
Ctrl + C
```

Wykonaj:

```bash
python manage.py startapp biblioteka
```

Pojawi się katalog:

```text
biblioteka/
```

Przykładowa struktura:

```text
biblioteka/
│
├── migrations/
│   └── __init__.py
│
├── __init__.py
├── admin.py
├── apps.py
├── models.py
├── tests.py
└── views.py
```

Na razie najważniejsze będą dla nas:

```text
views.py
```

oraz utworzony za chwilę:

```text
urls.py
```

---

# 14. Dodajemy aplikację do projektu

Otwórz:

```text
config/settings.py
```

Znajdź:

```python
INSTALLED_APPS = [
    ...
]
```

Dodaj:

```python
"biblioteka",
```

Czyli fragment może wyglądać:

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",

    "biblioteka",
]
```

Dzięki temu projekt Django wie, że aplikacja:

```text
biblioteka
```

jest częścią naszego systemu.

---

# 15. Pierwszy widok

Otwórz:

```text
biblioteka/views.py
```

Zastąp zawartość:

```python
from django.shortcuts import render
```

następującym kodem:

```python
from django.http import HttpResponse


def home(request):
    return HttpResponse(
        "<h1>Biblioteka</h1>"
        "<p>Aplikacja Django działa poprawnie.</p>"
    )
```

Mamy tutaj funkcję:

```python
home
```

która przyjmuje:

```python
request
```

i zwraca:

```python
HttpResponse
```

Czyli:

```text
request
   |
   v
home()
   |
   v
HttpResponse
```

To dokładnie schemat omawiany od pierwszych zajęć.

---

# 16. Potrzebujemy adresu URL

Sam widok jeszcze nie oznacza, że Django wie:

> kiedy należy uruchomić funkcję `home`?

Musimy powiązać:

```text
adres URL
```

z:

```text
widokiem
```

Do tego służy routing.

---

# 17. urls.py aplikacji

W katalogu:

```text
biblioteka/
```

utwórz nowy plik:

```text
urls.py
```

Wklej:

```python
from django.urls import path

from . import views


urlpatterns = [
    path("", views.home, name="home"),
]
```

Mamy tutaj:

```python
path("", views.home, name="home")
```

Możemy to czytać jako:

> dla pustej ścieżki uruchom widok `home`.

---

# 18. Podłączamy routing aplikacji

Otwórz:

```text
config/urls.py
```

Domyślnie plik wygląda podobnie do:

```python
from django.contrib import admin
from django.urls import path


urlpatterns = [
    path("admin/", admin.site.urls),
]
```

Zmieniamy import:

```python
from django.urls import include, path
```

i dodajemy:

```python
path("", include("biblioteka.urls")),
```

Pełna zawartość:

```python
from django.contrib import admin
from django.urls import include, path


urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("biblioteka.urls")),
]
```

---

# 19. Jak działa include?

Mamy teraz dwa poziomy routingu.

## Projekt

```text
config/urls.py
```

mówi:

```text
dla zwykłych adresów sprawdź biblioteka.urls
```

## Aplikacja

```text
biblioteka/urls.py
```

decyduje:

```text
jaki konkretny widok uruchomić
```

Schemat:

```text
request
   |
   v
config/urls.py
   |
   v
biblioteka/urls.py
   |
   v
views.home
   |
   v
HttpResponse
```

---

# 20. Uruchamiamy własny widok

Uruchom:

```bash
python manage.py runserver
```

Otwórz:

```text
http://127.0.0.1:8000/
```

Powinno pojawić się:

```text
Biblioteka

Aplikacja Django działa poprawnie.
```

Nie oglądamy już domyślnej strony Django.

Odpowiedź została przygotowana przez naszą funkcję:

```python
home()
```

---

# 21. Dodajemy widok listy książek

Otwórz:

```text
biblioteka/views.py
```

Zastąp zawartość pełnym kodem:

```python
from django.http import HttpResponse


def home(request):
    return HttpResponse(
        "<h1>Biblioteka</h1>"
        "<p>Aplikacja Django działa poprawnie.</p>"
        '<p><a href="/books/">Przejdź do książek</a></p>'
    )


def book_list(request):
    return HttpResponse(
        "<h1>Lista książek</h1>"
        "<ul>"
        "<li>Solaris — Stanisław Lem</li>"
        "<li>Lalka — Bolesław Prus</li>"
        "<li>Hobbit — J.R.R. Tolkien</li>"
        "</ul>"
        '<p><a href="/">Powrót</a></p>'
    )
```

Na tym etapie dane są wpisane bezpośrednio w kod.

To rozwiązanie jest tymczasowe.

Później książki będą pochodziły z bazy danych.

---

# 22. Dodajemy adres /books/

Otwórz:

```text
biblioteka/urls.py
```

Zastąp zawartość:

```python
from django.urls import path

from . import views


urlpatterns = [
    path("", views.home, name="home"),
    path("books/", views.book_list, name="book_list"),
]
```

Odśwież:

```text
http://127.0.0.1:8000/
```

Kliknij:

```text
Przejdź do książek
```

lub bezpośrednio otwórz:

```text
http://127.0.0.1:8000/books/
```

---

# 23. Co dzieje się dla /books/?

Przeglądarka wysyła:

```text
GET /books/
```

Django sprawdza:

```text
config/urls.py
```

który przekazuje obsługę do:

```text
biblioteka/urls.py
```

Tam znajduje:

```python
path("books/", views.book_list, name="book_list")
```

i uruchamia:

```python
book_list(request)
```

Widok zwraca:

```python
HttpResponse(...)
```

Schemat:

```text
GET /books/
      |
      v
config/urls.py
      |
      v
biblioteka/urls.py
      |
      v
book_list()
      |
      v
HttpResponse
      |
      v
przeglądarka
```

---

# 24. Sprawdzamy żądanie w DevTools

Przy uruchomionym Django:

1. otwórz `F12`,
2. przejdź do `Network`,
3. otwórz:

```text
/books/
```

4. wybierz żądanie.

Powinniśmy zobaczyć m.in.:

```text
Request URL:
http://127.0.0.1:8000/books/

Request Method:
GET
```

Czyli po kilku laboratoriach mamy już własny przykład całego przepływu:

```text
przeglądarka
-> HTTP
-> Django
-> routing
-> view
-> response
-> przeglądarka
```

---

# 25. Co oznacza name w path?

Mamy:

```python
path("books/", views.book_list, name="book_list")
```

Fragment:

```python
name="book_list"
```

nadaje adresowi nazwę.

Na razie nie wykorzystujemy jej bezpośrednio.

Będzie bardzo przydatna, kiedy zaczniemy tworzyć:

```text
szablony Django
```

Zamiast wpisywać adresy ręcznie:

```text
/books/
```

będziemy mogli odwoływać się do nazwy:

```text
book_list
```

---

# 26. Pierwszy błąd 404

Przy działającym serwerze otwórz:

```text
http://127.0.0.1:8000/abc/
```

Nie zdefiniowaliśmy takiej ścieżki.

Django wyświetli stronę informującą, że adres nie został znaleziony.

W trybie deweloperskim zobaczymy również listę ścieżek, które Django próbowało dopasować.

To jest przydatne podczas tworzenia aplikacji.

Jeżeli widzimy taki ekran podczas pracy, warto przede wszystkim sprawdzić:

```text
czy adres znajduje się w urls.py?
```

---

# 27. Automatyczne przeładowanie serwera

Podczas działania:

```bash
python manage.py runserver
```

zmień tekst w:

```text
views.py
```

Zapisz plik.

W terminalu powinna pojawić się informacja o przeładowaniu aplikacji.

W większości przypadków nie trzeba po każdej zmianie wykonywać ponownie:

```bash
python manage.py runserver
```

Wystarczy odświeżyć przeglądarkę.

---

# 28. Aktualna struktura projektu

Po dzisiejszych zajęciach projekt wygląda mniej więcej tak:

```text
paw_biblioteka/
│
├── biblioteka/
│   ├── migrations/
│   │   └── __init__.py
│   │
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
│
├── config/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── frontend/
│   └── ...
│
├── .gitignore
├── manage.py
├── README.md
├── requirements.txt
└── check_environment.py
```

Najważniejsze pliki na dzisiaj:

```text
manage.py
config/settings.py
config/urls.py
biblioteka/urls.py
biblioteka/views.py
```

---

# 29. Najważniejszy schemat z laboratorium

Na tym etapie Django możemy sprowadzić do:

```text
adres
  |
  v
urls.py
  |
  v
view
  |
  v
response
```

Przykład:

```text
/books/
  |
  v
path("books/", ...)
  |
  v
book_list()
  |
  v
HttpResponse(...)
```

Na następnym laboratorium dodamy do tego:

```text
template
```

czyli:

```text
adres
  |
  v
urls.py
  |
  v
view
  |
  v
template
  |
  v
HTML
```

---

# 30. Co z naszym starym frontendem?

W katalogu:

```text
frontend/
```

mamy już przygotowane:

```text
index.html
add_book.html
contact.html
register.html
styles.css
```

Na razie **niczego nie usuwamy**.

W następnym laboratorium zaczniemy przenosić przygotowany HTML do:

```text
szablonów Django
```

Dzięki temu nie będziemy pisać interfejsu od początku.

Wykorzystamy kod przygotowany podczas wcześniejszych laboratoriów.

---

# 31. Sprawdzamy projekt

Django posiada polecenie:

```bash
python manage.py check
```

Uruchom je.

Jeżeli wszystko jest poprawnie skonfigurowane, powinniśmy otrzymać informację podobną do:

```text
System check identified no issues
```

To przydatne polecenie, gdy chcemy sprawdzić podstawową konfigurację projektu.

---

# 32. Zapisujemy zmiany w Git

Przed zapisaniem zmian sprawdź:

```bash
git status
```

Powinniśmy zobaczyć m.in.:

```text
biblioteka/
config/
manage.py
requirements.txt
```

Dodaj:

```bash
git add .
```

Commit:

```bash
git commit -m "Utworzenie projektu Django i pierwszych widoków"
```

Wyślij:

```bash
git push
```

---

# 33. Odtworzenie projektu na innym komputerze

Od tego momentu samo:

```bash
git clone
```

nie wystarczy jeszcze do uruchomienia aplikacji.

Na nowym komputerze wykonujemy:

```bash
git clone ADRES_REPOZYTORIUM
cd paw_biblioteka
```

Tworzymy środowisko:

```bash
python -m venv .venv
```

Aktywujemy je.

Następnie instalujemy zależności:

```bash
python -m pip install -r requirements.txt
```

i uruchamiamy:

```bash
python manage.py runserver
```

Schemat:

```text
git clone
   |
   v
venv
   |
   v
pip install -r requirements.txt
   |
   v
runserver
```

---

# 34. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Przykładowe rozwiązania zostaną pokazane na początku następnego laboratorium.

---

## Zadanie 1 — strona autorów

Utwórz widok:

```python
author_list
```

który zwraca prostą stronę:

```text
Autorzy

Stanisław Lem
Bolesław Prus
J.R.R. Tolkien
```

Dodaj adres:

```text
/authors/
```

Wymagane pliki:

```text
biblioteka/views.py
biblioteka/urls.py
```

---

## Zadanie 2 — strona kontaktowa

Utwórz widok:

```python
contact
```

i adres:

```text
/contact/
```

Na stronie wyświetl:

```text
Kontakt

Biblioteka Akademicka
ul. Przykładowa 1
10-000 Olsztyn
```

Dodaj link do kontaktu na stronie głównej.

---

## Zadanie 3 — dodatkowa podstrona

Dodaj dowolną trzecią podstronę, np.:

```text
/about/
```

albo:

```text
/rules/
```

Przygotuj:

1. funkcję widoku,
2. wpis w `urlpatterns`,
3. link prowadzący do strony.

---

## Zadanie 4 — własna aplikacja

Dla swojego pomysłu na projekt zapisz na kartce lub w pliku `notes.md` minimum trzy przyszłe adresy URL.

Przykład dla planera nauki:

```text
/tasks/
/tasks/add/
/subjects/
```

Dla każdego adresu zapisz nazwę funkcji widoku, np.:

```text
/tasks/      -> task_list
/tasks/add/  -> task_add
/subjects/   -> subject_list
```

Nie trzeba jeszcze tworzyć osobnego projektu Django.

---

# 35. Co dalej?

Mamy już działający backend Django.

Obecnie widok zwraca HTML bezpośrednio:

```python
return HttpResponse(
    "<h1>Lista książek</h1>"
)
```

Dla kilku linijek jest to możliwe.

Dla prawdziwej strony byłoby jednak bardzo niewygodne.

Dlatego na kolejnym laboratorium przejdziemy do:

```text
templates
```

czyli szablonów Django.

Przeniesiemy przygotowany wcześniej frontend:

```text
HTML
CSS
```

do projektu Django.

Zaczniemy również korzystać z:

```python
render()
```

zamiast ręcznego tworzenia HTML w:

```python
HttpResponse
```

Efekt będzie wyglądał tak:

```text
request
   |
   v
urls.py
   |
   v
view
   |
   v
template
   |
   v
HTML response
   |
   v
przeglądarka
```
