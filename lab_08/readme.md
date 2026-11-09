# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 08 — Szablony Django, `render()`, context i pliki statyczne

---

## 1. Cel zajęć

Na poprzednich zajęciach uruchomiliśmy pierwszy projekt Django i przygotowaliśmy routing oraz widoki.

Na razie widoki zwracają HTML bezpośrednio:

```python
return HttpResponse(
    "<h1>Biblioteka</h1>"
)
```

Dla kilku linijek jest to możliwe, ale przy większej stronie szybko staje się niewygodne.

Dzisiaj przechodzimy do:

```text
templates
```

czyli szablonów Django.

Po zakończeniu laboratorium powinniśmy:

- korzystać z `render()` zamiast budować HTML w `HttpResponse`,
- utworzyć katalog szablonów Django,
- przygotować wspólny `base.html`,
- korzystać z dziedziczenia szablonów,
- znać znaczenie `{% block %}` i `{% extends %}`,
- przekazać dane z widoku do szablonu przez `context`,
- wyświetlić dane za pomocą `{{ ... }}`,
- wykorzystać prostą pętlę `{% for %}`,
- korzystać z nazw URL przez `{% url %}`,
- podłączyć plik CSS jako plik statyczny Django,
- przenieść przygotowany wcześniej wygląd Biblioteki do aplikacji Django.

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
cd paw_biblioteka
```

Aktywuj środowisko wirtualne.

Windows — CMD:

```cmd
.venv\Scripts\activate.bat
```

Windows — PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Linux / macOS:

```bash
source .venv/bin/activate
```

Jeżeli pracujesz na nowym komputerze:

```bash
python -m pip install -r requirements.txt
```

Uruchom projekt:

```bash
python manage.py runserver
```

---

# 3. Przykładowe rozwiązania z poprzedniego laboratorium

Na końcu `lab07` znajdowały się cztery nieobowiązkowe zadania.

Poniżej znajduje się przykładowe rozwiązanie każdego z nich.

---

## Zadanie 1 — strona autorów

Plik:

```text
biblioteka/views.py
```

Przykładowy widok:

```python
def author_list(request):
    return HttpResponse(
        "<h1>Autorzy</h1>"
        "<ul>"
        "<li>Stanisław Lem</li>"
        "<li>Bolesław Prus</li>"
        "<li>J.R.R. Tolkien</li>"
        "</ul>"
        '<p><a href="/">Powrót</a></p>'
    )
```

W:

```text
biblioteka/urls.py
```

dodajemy:

```python
path("authors/", views.author_list, name="author_list"),
```

Pełny fragment może wyglądać:

```python
urlpatterns = [
    path("", views.home, name="home"),
    path("books/", views.book_list, name="book_list"),
    path("authors/", views.author_list, name="author_list"),
]
```

Adres:

```text
http://127.0.0.1:8000/authors/
```

---

## Zadanie 2 — strona kontaktowa

W:

```text
biblioteka/views.py
```

dodaj:

```python
def contact(request):
    return HttpResponse(
        "<h1>Kontakt</h1>"
        "<p>Biblioteka Akademicka</p>"
        "<p>ul. Przykładowa 1</p>"
        "<p>10-000 Olsztyn</p>"
        '<p><a href="/">Powrót</a></p>'
    )
```

W:

```text
biblioteka/urls.py
```

dodaj:

```python
path("contact/", views.contact, name="contact"),
```

Na stronie głównej możemy dodać link:

```python
def home(request):
    return HttpResponse(
        "<h1>Biblioteka</h1>"
        "<p>Aplikacja Django działa poprawnie.</p>"
        '<p><a href="/books/">Książki</a></p>'
        '<p><a href="/authors/">Autorzy</a></p>'
        '<p><a href="/contact/">Kontakt</a></p>'
    )
```

---

## Zadanie 3 — dodatkowa podstrona

Przykład:

```text
/about/
```

Widok:

```python
def about(request):
    return HttpResponse(
        "<h1>O bibliotece</h1>"
        "<p>"
        "Projekt ćwiczeniowy rozwijany podczas laboratoriów "
        "z Projektowania aplikacji webowych."
        "</p>"
        '<p><a href="/">Powrót</a></p>'
    )
```

Routing:

```python
path("about/", views.about, name="about"),
```

---

## Zadanie 4 — adresy dla własnej aplikacji

Przykład dla:

> **Planera nauki**

Możemy zaplanować:

```text
/tasks/          -> task_list
/tasks/add/      -> task_add
/tasks/5/        -> task_detail
/subjects/       -> subject_list
```

Na tym etapie nie tworzymy jeszcze osobnego projektu.

Celem ćwiczenia było zauważenie zależności:

```text
funkcja aplikacji
        |
        v
adres URL
        |
        v
widok
```

---

# 4. Dlaczego nie chcemy budować HTML w `HttpResponse`?

Przykład:

```python
return HttpResponse(
    "<h1>Lista książek</h1>"
    "<ul>"
    "<li>Solaris</li>"
    "<li>Lalka</li>"
    "<li>Hobbit</li>"
    "</ul>"
)
```

Już teraz jest mało czytelny.

Gdybyśmy dodali:

```text
header
nav
main
section
formularz
tabelę
footer
CSS
```

plik `views.py` stałby się bardzo trudny do utrzymania.

Dlatego rozdzielamy:

```text
Python
= logika aplikacji

HTML
= szablon strony
```

Widok będzie odpowiadał za przygotowanie danych.

Szablon będzie odpowiadał za ich prezentację.

---

# 5. Pierwszy katalog templates

W aplikacji:

```text
biblioteka/
```

utwórz strukturę:

```text
biblioteka/
└── templates/
    └── biblioteka/
```

Czyli:

```text
biblioteka/
│
├── migrations/
│
├── templates/
│   └── biblioteka/
│
├── admin.py
├── apps.py
├── models.py
├── urls.py
└── views.py
```

Dlaczego mamy dwa razy:

```text
biblioteka/
```

?

Pierwszy to aplikacja Django.

Drugi to przestrzeń nazw dla szablonów.

Dzięki temu przy większym projekcie różne aplikacje mogą posiadać pliki o takich samych nazwach bez konfliktów.

---

# 6. Pierwszy szablon

Utwórz:

```text
biblioteka/templates/biblioteka/home.html
```

Wklej:

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
        Aplikacja Django działa poprawnie.
    </p>

    <p>
        <a href="/books/">
            Książki
        </a>
    </p>

    <p>
        <a href="/authors/">
            Autorzy
        </a>
    </p>

    <p>
        <a href="/contact/">
            Kontakt
        </a>
    </p>

</body>
</html>
```

---

# 7. `render()`

Otwórz:

```text
biblioteka/views.py
```

Zamiast:

```python
from django.http import HttpResponse
```

zaimportuj:

```python
from django.shortcuts import render
```

Widok strony głównej zmień na:

```python
def home(request):
    return render(
        request,
        "biblioteka/home.html",
    )
```

Możemy to zapisać także w jednej linii:

```python
def home(request):
    return render(request, "biblioteka/home.html")
```

Na zajęciach będziemy często używać wersji wieloliniowej, kiedy do `render()` dojdą dodatkowe argumenty.

---

# 8. Uruchamiamy szablon

Przy działającym:

```bash
python manage.py runserver
```

otwórz:

```text
http://127.0.0.1:8000/
```

Powinna pojawić się strona z:

```text
Biblioteka
Aplikacja Django działa poprawnie.
```

HTML nie znajduje się już w:

```text
views.py
```

tylko w:

```text
home.html
```

Schemat:

```text
/
 |
 v
home()
 |
 v
render()
 |
 v
home.html
 |
 v
HTML
 |
 v
przeglądarka
```

---

# 9. Problem z powtarzaniem HTML

Za chwilę będziemy mieli kilka stron:

```text
home.html
book_list.html
author_list.html
contact.html
```

Każda potrzebuje:

```text
<!DOCTYPE html>
<html>
<head>
header
nav
main
footer
```

Nie chcemy kopiować tego samego kodu do każdego pliku.

Dlatego utworzymy:

```text
base.html
```

czyli wspólny szablon bazowy.

---

# 10. `base.html`

Utwórz:

```text
biblioteka/templates/biblioteka/base.html
```

Wklej:

```html
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>
        {% block title %}
            Biblioteka
        {% endblock %}
    </title>
</head>

<body>

    <header>
        <h1>Biblioteka</h1>

        <nav>
            <a href="/">
                Strona główna
            </a>

            <a href="/books/">
                Książki
            </a>

            <a href="/authors/">
                Autorzy
            </a>

            <a href="/contact/">
                Kontakt
            </a>
        </nav>
    </header>

    <main>

        {% block content %}

        {% endblock %}

    </main>

    <footer>
        <p>
            Projekt ćwiczeniowy —
            Projektowanie aplikacji webowych
        </p>
    </footer>

</body>
</html>
```

---

# 11. `{% block %}`

W `base.html` użyliśmy:

```django
{% block title %}
    Biblioteka
{% endblock %}
```

oraz:

```django
{% block content %}

{% endblock %}
```

Są to miejsca, które szablony potomne mogą zastąpić własną treścią.

Możemy to wyobrazić sobie jako:

```text
base.html

+-----------------------------+
| wspólny header              |
+-----------------------------+
|                             |
|       BLOCK CONTENT         |
|                             |
+-----------------------------+
| wspólny footer              |
+-----------------------------+
```

---

# 12. Dziedziczymy `base.html`

Zmień:

```text
home.html
```

na:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    Strona główna — Biblioteka
{% endblock %}


{% block content %}

    <section>
        <h2>Witaj w Bibliotece</h2>

        <p>
            Projekt ćwiczeniowy rozwijany podczas laboratoriów.
        </p>

        <p>
            W kolejnych zajęciach aplikacja będzie rozbudowywana
            o bazę danych, formularze i logowanie użytkowników.
        </p>
    </section>

{% endblock %}
```

Nie potrzebujemy już wpisywać ponownie:

```text
html
head
body
header
nav
footer
```

Te elementy pochodzą z:

```text
base.html
```

---

# 13. `{% extends %}`

Zapis:

```django
{% extends "biblioteka/base.html" %}
```

oznacza:

> ten szablon dziedziczy strukturę z `base.html`.

Szablon:

```text
home.html
```

wypełnia jedynie wybrane bloki.

To podejście będzie stosowane praktycznie na wszystkich kolejnych stronach.

---

# 14. Szablon listy książek

Utwórz:

```text
biblioteka/templates/biblioteka/book_list.html
```

Na razie wklej:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    Książki — Biblioteka
{% endblock %}


{% block content %}

    <section>
        <h2>Lista książek</h2>

        <p>
            Za chwilę dane zostaną przekazane tutaj z widoku Django.
        </p>
    </section>

{% endblock %}
```

---

# 15. Zmieniamy widok `book_list`

W:

```text
biblioteka/views.py
```

ustaw:

```python
def book_list(request):
    return render(
        request,
        "biblioteka/book_list.html",
    )
```

Otwórz:

```text
http://127.0.0.1:8000/books/
```

Strona korzysta już ze wspólnego:

```text
base.html
```

---

# 16. Przekazywanie danych do szablonu

Nie chcemy wpisywać książek bezpośrednio w HTML.

Na razie nie mamy jeszcze bazy danych, więc przygotujemy dane w Pythonie.

W:

```text
biblioteka/views.py
```

zmień widok:

```python
def book_list(request):
    books = [
        {
            "title": "Solaris",
            "author": "Stanisław Lem",
            "year": 1961,
            "available": True,
        },
        {
            "title": "Lalka",
            "author": "Bolesław Prus",
            "year": 1890,
            "available": False,
        },
        {
            "title": "Hobbit",
            "author": "J.R.R. Tolkien",
            "year": 1937,
            "available": True,
        },
    ]

    context = {
        "books": books,
    }

    return render(
        request,
        "biblioteka/book_list.html",
        context,
    )
```

---

# 17. `context`

Mamy:

```python
context = {
    "books": books,
}
```

`context` jest słownikiem zawierającym dane przekazywane do szablonu.

W Pythonie:

```python
"books"
```

jest nazwą, pod którą dane będą dostępne w HTML.

Schemat:

```text
views.py

books = [...]
     |
     v
context
     |
     v
render()
     |
     v
book_list.html
```

---

# 18. Wyświetlamy wartość w szablonie

W szablonie Django dane wyświetlamy za pomocą:

```django
{{ nazwa }}
```

Przykład:

```django
{{ book.title }}
```

Na razie jednak mamy kilka książek.

Potrzebujemy pętli.

---

# 19. Pętla `{% for %}`

Zmień:

```text
book_list.html
```

na:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    Książki — Biblioteka
{% endblock %}


{% block content %}

    <section>
        <h2>Lista książek</h2>

        <div class="book-list">

            {% for book in books %}

                <article class="book-card">

                    <h3>
                        {{ book.title }}
                    </h3>

                    <div class="book-info">

                        <p>
                            <strong>Autor:</strong>
                            {{ book.author }}
                        </p>

                        <p>
                            <strong>Rok wydania:</strong>
                            {{ book.year }}
                        </p>

                    </div>

                </article>

            {% endfor %}

        </div>
    </section>

{% endblock %}
```

Odśwież:

```text
/books/
```

Powinny pojawić się trzy książki.

---

# 20. Co robi `{% for %}`?

Mamy:

```django
{% for book in books %}
```

Możemy to czytać jako:

> dla każdej książki znajdującej się w `books`

wykonaj HTML znajdujący się pomiędzy:

```django
{% for ... %}
```

a:

```django
{% endfor %}
```

To odpowiednik idei znanej z Pythona:

```python
for book in books:
    ...
```

Tyle że tutaj pętla generuje fragment HTML.

---

# 21. Prosty warunek `{% if %}`

Mamy w danych:

```python
"available": True
```

lub:

```python
"available": False
```

Możemy wykorzystać:

```django
{% if book.available %}
```

Zmień kartę książki:

```django
<article class="book-card">

    <h3>
        {{ book.title }}
    </h3>

    <div class="book-info">

        <p>
            <strong>Autor:</strong>
            {{ book.author }}
        </p>

        <p>
            <strong>Rok wydania:</strong>
            {{ book.year }}
        </p>

        <p>
            <strong>Dostępność:</strong>

            {% if book.available %}
                Dostępna
            {% else %}
                Niedostępna
            {% endif %}
        </p>

    </div>

</article>
```

Nie będziemy na tym etapie poznawali całego języka szablonów Django.

Najważniejsze elementy to dziś:

```text
{{ ... }}
{% for %}
{% if %}
{% extends %}
{% block %}
```

---

# 22. Szablon autorów

Utwórz:

```text
biblioteka/templates/biblioteka/author_list.html
```

Wklej:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    Autorzy — Biblioteka
{% endblock %}


{% block content %}

    <section>
        <h2>Autorzy</h2>

        <ul>
            <li>Stanisław Lem</li>
            <li>Bolesław Prus</li>
            <li>J.R.R. Tolkien</li>
        </ul>
    </section>

{% endblock %}
```

Widok:

```python
def author_list(request):
    return render(
        request,
        "biblioteka/author_list.html",
    )
```

---

# 23. Szablon kontaktu

Utwórz:

```text
biblioteka/templates/biblioteka/contact.html
```

Wklej:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    Kontakt — Biblioteka
{% endblock %}


{% block content %}

    <section>
        <h2>Kontakt</h2>

        <p>Biblioteka Akademicka</p>
        <p>ul. Przykładowa 1</p>
        <p>10-000 Olsztyn</p>
    </section>

{% endblock %}
```

Widok:

```python
def contact(request):
    return render(
        request,
        "biblioteka/contact.html",
    )
```

---

# 24. Pełny `views.py` na tym etapie

Plik:

```text
biblioteka/views.py
```

może wyglądać tak:

```python
from django.shortcuts import render


def home(request):
    return render(
        request,
        "biblioteka/home.html",
    )


def book_list(request):
    books = [
        {
            "title": "Solaris",
            "author": "Stanisław Lem",
            "year": 1961,
            "available": True,
        },
        {
            "title": "Lalka",
            "author": "Bolesław Prus",
            "year": 1890,
            "available": False,
        },
        {
            "title": "Hobbit",
            "author": "J.R.R. Tolkien",
            "year": 1937,
            "available": True,
        },
    ]

    context = {
        "books": books,
    }

    return render(
        request,
        "biblioteka/book_list.html",
        context,
    )


def author_list(request):
    return render(
        request,
        "biblioteka/author_list.html",
    )


def contact(request):
    return render(
        request,
        "biblioteka/contact.html",
    )
```

---

# 25. Nie wpisujemy adresów ręcznie

W `base.html` mamy na razie:

```html
<a href="/books/">
    Książki
</a>
```

To działa.

Django pozwala jednak korzystać z nazw zdefiniowanych w:

```text
urls.py
```

Przypomnijmy:

```python
path("books/", views.book_list, name="book_list")
```

Mamy nazwę:

```text
book_list
```

---

# 26. `{% url %}`

W `base.html` zamiast:

```html
<a href="/books/">
```

użyj:

```django
<a href="{% url 'book_list' %}">
```

Pełna nawigacja:

```django
<nav>
    <a href="{% url 'home' %}">
        Strona główna
    </a>

    <a href="{% url 'book_list' %}">
        Książki
    </a>

    <a href="{% url 'author_list' %}">
        Autorzy
    </a>

    <a href="{% url 'contact' %}">
        Kontakt
    </a>
</nav>
```

Teraz nie musimy pamiętać:

```text
/books/
/authors/
/contact/
```

Korzystamy z nazw adresów.

Jeżeli później zmienimy:

```python
path("books/", ...)
```

na:

```python
path("ksiazki/", ...)
```

link wykorzystujący:

```django
{% url 'book_list' %}
```

nadal może działać bez zmiany szablonu.

---

# 27. Pliki statyczne

Nasz wcześniejszy frontend posiada:

```text
frontend/css/styles.css
```

Chcemy teraz podłączyć CSS do Django.

W aplikacji utwórz:

```text
biblioteka/static/biblioteka/css/
```

Struktura:

```text
biblioteka/
│
├── static/
│   └── biblioteka/
│       └── css/
│           └── styles.css
│
└── templates/
    └── biblioteka/
        └── ...
```

---

# 28. Przenosimy CSS

Skopiuj zawartość wcześniejszego:

```text
frontend/css/styles.css
```

do:

```text
biblioteka/static/biblioteka/css/styles.css
```

Na tym etapie możemy pozostawić stary katalog:

```text
frontend/
```

jako materiał referencyjny.

Nie jest już jednak używany przez Django.

---

# 29. `{% load static %}`

Na samej górze:

```text
base.html
```

dodaj:

```django
{% load static %}
```

Czyli początek:

```django
{% load static %}

<!DOCTYPE html>
<html lang="pl">
```

Następnie w `<head>` dodaj:

```django
<link
    rel="stylesheet"
    href="{% static 'biblioteka/css/styles.css' %}"
>
```

---

# 30. Gotowy początek `base.html`

Powinien wyglądać tak:

```django
{% load static %}

<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>
        {% block title %}
            Biblioteka
        {% endblock %}
    </title>

    <link
        rel="stylesheet"
        href="{% static 'biblioteka/css/styles.css' %}"
    >
</head>
```

Odśwież stronę.

Powinien wrócić wygląd przygotowany podczas wcześniejszych laboratoriów.

---

# 31. Dlaczego static?

Pliki takie jak:

```text
CSS
obrazy
JavaScript
```

są w Django traktowane jako:

```text
static files
```

czyli pliki statyczne.

Kod Pythona nie generuje ich zawartości przy każdym żądaniu.

Django posiada mechanizmy pozwalające poprawnie odwoływać się do takich plików.

Na naszych zajęciach będą to przede wszystkim:

```text
CSS
obrazy
```

---

# 32. Obraz jako static

Jeżeli chcemy wykorzystać lokalny obraz, możemy utworzyć:

```text
biblioteka/static/biblioteka/images/
```

i umieścić tam np.:

```text
library.jpg
```

W szablonie:

```django
{% load static %}
```

a potem:

```django
<img
    class="library-image"
    src="{% static 'biblioteka/images/library.jpg' %}"
    alt="Wnętrze biblioteki"
>
```

Ponieważ `base.html` ładuje `static`, w szablonie dziedziczącym również możemy korzystać z tego mechanizmu, ale dla czytelności można dodać `{% load static %}` również w szablonie, który bezpośrednio używa tagu `static`.

---

# 33. Przenosimy fragment strony głównej

Z wcześniejszego:

```text
frontend/index.html
```

możemy wykorzystać część treści.

Przykładowy:

```text
home.html
```

może wyglądać:

```django
{% extends "biblioteka/base.html" %}
{% load static %}


{% block title %}
    Strona główna — Biblioteka
{% endblock %}


{% block content %}

    <section id="about">
        <h2>O bibliotece</h2>

        <p>
            Witaj w przykładowej Bibliotece.
            Aplikacja jest rozwijana podczas laboratoriów
            z Projektowania aplikacji webowych.
        </p>

        <img
            class="library-image"
            src="{% static 'biblioteka/images/library.jpg' %}"
            alt="Wnętrze biblioteki"
        >
    </section>

    <section>
        <h2>Planowane funkcje</h2>

        <ul>
            <li>lista książek,</li>
            <li>autorzy,</li>
            <li>dodawanie i edycja danych,</li>
            <li>logowanie użytkowników,</li>
            <li>wypożyczenia.</li>
        </ul>
    </section>

{% endblock %}
```

Jeżeli nie masz pliku:

```text
library.jpg
```

usuń chwilowo element `<img>` albo dodaj dowolny własny obraz.

---

# 34. Pełny `base.html`

Po dzisiejszych zmianach:

```text
biblioteka/templates/biblioteka/base.html
```

może wyglądać tak:

```django
{% load static %}

<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>
        {% block title %}
            Biblioteka
        {% endblock %}
    </title>

    <link
        rel="stylesheet"
        href="{% static 'biblioteka/css/styles.css' %}"
    >
</head>

<body>

    <header>
        <h1>Biblioteka</h1>

        <nav>
            <a href="{% url 'home' %}">
                Strona główna
            </a>

            <a href="{% url 'book_list' %}">
                Książki
            </a>

            <a href="{% url 'author_list' %}">
                Autorzy
            </a>

            <a href="{% url 'contact' %}">
                Kontakt
            </a>
        </nav>
    </header>

    <main>

        {% block content %}

        {% endblock %}

    </main>

    <footer>
        <p>
            Projekt ćwiczeniowy —
            Projektowanie aplikacji webowych
        </p>
    </footer>

</body>
</html>
```

---

# 35. Aktualny `urls.py`

Plik:

```text
biblioteka/urls.py
```

powinien wyglądać mniej więcej tak:

```python
from django.urls import path

from . import views


urlpatterns = [
    path("", views.home, name="home"),
    path("books/", views.book_list, name="book_list"),
    path("authors/", views.author_list, name="author_list"),
    path("contact/", views.contact, name="contact"),
]
```

---

# 36. Aktualna struktura projektu

Po laboratorium najważniejsza część projektu wygląda tak:

```text
paw_biblioteka/
│
├── biblioteka/
│   │
│   ├── migrations/
│   │
│   ├── static/
│   │   └── biblioteka/
│   │       ├── css/
│   │       │   └── styles.css
│   │       │
│   │       └── images/
│   │           └── library.jpg
│   │
│   ├── templates/
│   │   └── biblioteka/
│   │       ├── base.html
│   │       ├── home.html
│   │       ├── book_list.html
│   │       ├── author_list.html
│   │       └── contact.html
│   │
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── urls.py
│   └── views.py
│
├── config/
│   └── ...
│
├── frontend/
│   └── ...
│
├── manage.py
└── requirements.txt
```

---

# 37. Najważniejszy przepływ po dzisiejszych zajęciach

Mamy teraz:

```text
GET /books/
      |
      v
urls.py
      |
      v
book_list()
      |
      v
context
      |
      v
book_list.html
      |
      v
base.html
      |
      v
styles.css
      |
      v
gotowy HTML
      |
      v
przeglądarka
```

To jest już znacznie bliższe strukturze prawdziwej aplikacji Django.

---

# 38. Mała modyfikacja podczas zajęć

Dodaj do danych książek pole:

```python
"category"
```

Przykład:

```python
{
    "title": "Solaris",
    "author": "Stanisław Lem",
    "year": 1961,
    "available": True,
    "category": "Science fiction",
}
```

Zrób to dla wszystkich książek.

Następnie w:

```text
book_list.html
```

dodaj:

```django
<p>
    <strong>Kategoria:</strong>
    {{ book.category }}
</p>
```

Odśwież stronę.

Dane powinny pojawić się bez ręcznego tworzenia osobnego HTML dla każdej książki.

---

# 39. Dodajemy klasę zależną od dostępności

Możemy wykorzystać warunek bezpośrednio w atrybucie HTML:

```django
<article
    class="book-card
    {% if book.available %}
        available
    {% else %}
        unavailable
    {% endif %}"
>
```

Dzięki stylom przygotowanym wcześniej:

```css
.available {
    border-left: 6px solid green;
}

.unavailable {
    border-left: 6px solid darkred;
}
```

wygląd karty może zależeć od danych przekazanych z Pythona.

To pierwszy przykład, w którym:

```text
dane z backendu
```

wpływają na:

```text
wygląd frontendu
```

---

# 40. Sprawdzamy projekt

Uruchom:

```bash
python manage.py check
```

Następnie sprawdź kolejno:

```text
/
```

```text
/books/
```

```text
/authors/
```

```text
/contact/
```

Sprawdź również:

- czy działa nawigacja,
- czy CSS się ładuje,
- czy lista książek powstaje z pętli,
- czy dostępność książek jest wyświetlana poprawnie.

---

# 41. Zapisujemy zmiany w Git

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
git commit -m "Dodanie szablonów Django i plików statycznych"
```

Wyślij:

```bash
git push
```

---

# 42. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Przykładowe rozwiązania zostaną pokazane na początku następnego laboratorium.

---

## Zadanie 1 — autorzy z `context`

Przenieś dane autorów z HTML do:

```text
views.py
```

Przykładowa struktura:

```python
authors = [
    {
        "name": "Stanisław Lem",
        "country": "Polska",
    },
    {
        "name": "J.R.R. Tolkien",
        "country": "Wielka Brytania",
    },
]
```

Przekaż je przez:

```python
context
```

i wyświetl w:

```text
author_list.html
```

za pomocą:

```django
{% for %}
```

---

## Zadanie 2 — pusta lista

W widoku:

```python
book_list
```

tymczasowo ustaw:

```python
books = []
```

W szablonie wykorzystaj:

```django
{% empty %}
```

Przykład:

```django
{% for book in books %}

    ...

{% empty %}

    <p>
        Brak książek do wyświetlenia.
    </p>

{% endfor %}
```

Sprawdź efekt.

Na końcu przywróć książki.

---

## Zadanie 3 — strona „O bibliotece”

Przygotuj:

```text
/about/
```

ale tym razem z wykorzystaniem szablonu.

Utwórz:

```text
biblioteka/templates/biblioteka/about.html
```

Szablon powinien:

- dziedziczyć `base.html`,
- ustawić własny `title`,
- zawierać minimum jedną sekcję,
- być dostępny z nawigacji przez `{% url %}`.

---

## Zadanie 4 — dodatkowe dane na stronie głównej

W widoku:

```python
home
```

przekaż:

```python
context = {
    "books_count": 3,
    "authors_count": 3,
}
```

Na stronie głównej wyświetl:

```text
Liczba książek: 3
Liczba autorów: 3
```

Nie wpisuj liczb bezpośrednio w HTML.

---

## Zadanie 5 — własny szablon

Dla pomysłu na własny projekt przygotuj na kartce lub w osobnym pliku przykładowy podział:

```text
base.html
lista elementów
strona szczegółów
formularz
```

Przykład dla planera:

```text
base.html
task_list.html
task_detail.html
task_form.html
```

Nie trzeba jeszcze tworzyć osobnego projektu Django.

---

# 43. Co dalej?

Na tym etapie mamy już:

```text
routing
views
templates
context
static files
HTML
CSS
```

Brakuje nam najważniejszego źródła danych:

```text
bazy danych
```

Na kolejnym laboratorium przejdziemy do:

```text
models.py
Django ORM
migrations
SQLite
admin
```

Zamiast:

```python
books = [
    {...},
    {...},
]
```

zaczniemy przechowywać książki w prawdziwej bazie danych.

Docelowy przepływ stanie się więc:

```text
request
   |
   v
URL
   |
   v
view
   |
   v
model
   |
   v
database
   |
   v
view
   |
   v
template
   |
   v
response
```
