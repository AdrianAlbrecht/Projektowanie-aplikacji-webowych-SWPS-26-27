# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 10 — ORM, relacje i szczegóły obiektów

---

## 1. Cel zajęć

Na poprzednich zajęciach utworzyliśmy modele:

```text
Author
Book
```

oraz zaczęliśmy korzystać z:

```text
Django ORM
```

Dzisiaj rozwiniemy pracę z danymi.

Po zakończeniu laboratorium powinniśmy:

- potrafić pobrać pojedynczy obiekt z bazy,
- rozumieć dynamiczne fragmenty adresu URL,
- utworzyć stronę szczegółów książki,
- korzystać z `get_object_or_404()`,
- tworzyć link do konkretnego rekordu,
- korzystać z relacji `Book -> Author`,
- korzystać z relacji odwrotnej `Author -> Book`,
- filtrować dane przez ORM,
- sortować dane przez ORM,
- wykorzystać parametr `GET` do prostego wyszukiwania,
- przygotować stronę szczegółów autora wraz z jego książkami.

> [!CAUTION]
> **Biblioteka, katalog książek oraz system wypożyczania książek są tematami niedozwolonymi w projekcie zaliczeniowym.**
>
> Biblioteka jest wspólnym projektem ćwiczeniowym rozwijanym podczas laboratoriów.

---

# 2. Początek pracy

Pobierz najnowszą wersję projektu:

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
python manage.py migrate
```

Uruchom:

```bash
python manage.py check
```

---

# 3. Przykładowe rozwiązania z poprzedniego laboratorium

Na końcu `lab09` znajdowało się pięć nieobowiązkowych zadań.

Poniżej znajduje się przykładowe rozwiązanie każdego z nich.

---

## Zadanie 1 — liczba książek autora

Jeżeli model `Book` posiada:

```python
author = models.ForeignKey(
    Author,
    on_delete=models.CASCADE,
    related_name="books",
)
```

to w szablonie możemy użyć:

```django
{{ author.books.count }}
```

Przykładowy:

```text
author_list.html
```

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    Autorzy — Biblioteka
{% endblock %}


{% block content %}

    <section>
        <h2>Autorzy</h2>

        <div class="author-list">

            {% for author in authors %}

                <article class="author-card">

                    <h3>
                        {{ author.first_name }}
                        {{ author.last_name }}
                    </h3>

                    <p>
                        <strong>Liczba książek:</strong>
                        {{ author.books.count }}
                    </p>

                </article>

            {% empty %}

                <p>
                    Brak autorów do wyświetlenia.
                </p>

            {% endfor %}

        </div>
    </section>

{% endblock %}
```

Jeżeli dodamy kolejną książkę tego autora w panelu administratora, wartość:

```django
{{ author.books.count }}
```

zmieni się automatycznie.

---

## Zadanie 2 — filtrowanie dostępnych książek

W:

```text
biblioteka/views.py
```

możemy tymczasowo użyć:

```python
books = Book.objects.filter(
    available=True,
)
```

Cały widok:

```python
def book_list(request):
    books = Book.objects.filter(
        available=True,
    )

    context = {
        "books": books,
    }

    return render(
        request,
        "biblioteka/book_list.html",
        context,
    )
```

Dzięki temu w szablonie pojawią się wyłącznie książki, dla których:

```text
available = True
```

Po ćwiczeniu wracamy do:

```python
Book.objects.all()
```

---

## Zadanie 3 — pole liczby stron

Model:

```python
class Book(models.Model):
    title = models.CharField(
        max_length=200,
    )

    author = models.ForeignKey(
        Author,
        on_delete=models.CASCADE,
        related_name="books",
    )

    year = models.PositiveIntegerField()

    category = models.CharField(
        max_length=100,
        blank=True,
    )

    available = models.BooleanField(
        default=True,
    )

    description = models.TextField(
        blank=True,
    )

    pages = models.PositiveIntegerField(
        null=True,
        blank=True,
    )

    def __str__(self):
        return self.title
```

Po zmianie modelu:

```bash
python manage.py makemigrations
python manage.py migrate
```

W szablonie:

```django
{% if book.pages %}

    <p>
        <strong>Liczba stron:</strong>
        {{ book.pages }}
    </p>

{% endif %}
```

> [!NOTE]
> Pole `pages` było zadaniem dodatkowym.
>
> Jeżeli go nie wykonywałeś, nie jest potrzebne do dalszej części dzisiejszego laboratorium.

---

## Zadanie 4 — model `Category`

Przykładowy model:

```python
class Category(models.Model):
    name = models.CharField(
        max_length=100,
    )

    def __str__(self):
        return self.name
```

Po dodaniu:

```bash
python manage.py makemigrations
python manage.py migrate
```

W:

```text
biblioteka/admin.py
```

możemy dodać:

```python
from .models import Author, Book, Category
```

oraz:

```python
admin.site.register(Category)
```

lub:

```python
@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    list_display = (
        "name",
    )
```

Następnie w panelu administratora możemy utworzyć np.:

```text
Fantasy
Science fiction
Powieść
Kryminał
```

> [!NOTE]
> Model `Category` również był zadaniem dodatkowym.
>
> Dzisiejszy lab nadal działa bez niego.

---

## Zadanie 5 — ORM w shellu

Uruchamiamy:

```bash
python manage.py shell
```

Import:

```python
from biblioteka.models import Author, Book
```

Wszystkie książki:

```python
Book.objects.all()
```

Liczba książek:

```python
Book.objects.count()
```

Dostępne książki:

```python
Book.objects.filter(
    available=True,
)
```

Wszyscy autorzy:

```python
Author.objects.all()
```

Przykładowe pobranie autora:

```python
author = Author.objects.get(
    last_name="Lem",
)
```

Książki autora:

```python
author.books.all()
```

Koniec:

```python
exit()
```

---

# 4. Punkt wyjścia

Dzisiejszy lab zakłada jedynie podstawowe modele z `lab09`:

```python
from django.db import models


class Author(models.Model):
    first_name = models.CharField(
        max_length=100,
    )

    last_name = models.CharField(
        max_length=100,
    )

    def __str__(self):
        return f"{self.first_name} {self.last_name}"


class Book(models.Model):
    title = models.CharField(
        max_length=200,
    )

    author = models.ForeignKey(
        Author,
        on_delete=models.CASCADE,
        related_name="books",
    )

    year = models.PositiveIntegerField()

    category = models.CharField(
        max_length=100,
        blank=True,
    )

    available = models.BooleanField(
        default=True,
    )

    description = models.TextField(
        blank=True,
    )

    def __str__(self):
        return self.title
```

Jeżeli posiadasz również:

```text
pages
Category
```

możesz je pozostawić.

Nie będą przeszkadzały.

---

# 5. Lista a szczegóły

Obecnie mamy stronę:

```text
/books/
```

która pokazuje wiele książek.

Chcemy teraz utworzyć:

```text
/books/1/
/books/2/
/books/3/
```

czyli stronę konkretnej książki.

Schemat:

```text
/books/
   |
   +-- Solaris
   |      |
   |      v
   |   /books/1/
   |
   +-- Lalka
          |
          v
       /books/2/
```

---

# 6. Dynamiczny fragment adresu

Django pozwala umieścić zmienną w ścieżce URL.

Przykład:

```python
path(
    "books/<int:book_id>/",
    views.book_detail,
    name="book_detail",
)
```

Fragment:

```text
<int:book_id>
```

oznacza:

> w tym miejscu oczekujemy liczby całkowitej i przekażemy ją do widoku pod nazwą `book_id`.

Przykład:

```text
/books/5/
```

spowoduje:

```text
book_id = 5
```

---

# 7. Dodajemy routing szczegółów książki

Otwórz:

```text
biblioteka/urls.py
```

Dodaj:

```python
path(
    "books/<int:book_id>/",
    views.book_detail,
    name="book_detail",
),
```

Pełny plik może wyglądać:

```python
from django.urls import path

from . import views


urlpatterns = [
    path("", views.home, name="home"),
    path("books/", views.book_list, name="book_list"),

    path(
        "books/<int:book_id>/",
        views.book_detail,
        name="book_detail",
    ),

    path("authors/", views.author_list, name="author_list"),
    path("contact/", views.contact, name="contact"),
    path("about/", views.about, name="about"),
]
```

---

# 8. Pierwszy `book_detail`

W:

```text
biblioteka/views.py
```

dodaj:

```python
def book_detail(request, book_id):
    book = Book.objects.get(
        id=book_id,
    )

    context = {
        "book": book,
    }

    return render(
        request,
        "biblioteka/book_detail.html",
        context,
    )
```

Zauważ, że funkcja posiada teraz dwa argumenty:

```python
request
book_id
```

Django przekazuje `book_id` na podstawie adresu.

---

# 9. Szablon szczegółów książki

Utwórz:

```text
biblioteka/templates/biblioteka/book_detail.html
```

Wklej:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    {{ book.title }} — Biblioteka
{% endblock %}


{% block content %}

    <section>

        <h2>
            {{ book.title }}
        </h2>

        <p>
            <strong>Autor:</strong>
            {{ book.author }}
        </p>

        <p>
            <strong>Rok wydania:</strong>
            {{ book.year }}
        </p>

        <p>
            <strong>Kategoria:</strong>

            {% if book.category %}
                {{ book.category }}
            {% else %}
                Brak kategorii
            {% endif %}
        </p>

        <p>
            <strong>Dostępność:</strong>

            {% if book.available %}
                Dostępna
            {% else %}
                Niedostępna
            {% endif %}
        </p>

        {% if book.description %}

            <h3>Opis</h3>

            <p>
                {{ book.description }}
            </p>

        {% endif %}

        <p>
            <a
                class="button-link"
                href="{% url 'book_list' %}"
            >
                Powrót do listy
            </a>
        </p>

    </section>

{% endblock %}
```

---

# 10. Sprawdzamy stronę

Uruchom:

```bash
python manage.py runserver
```

Spróbuj:

```text
http://127.0.0.1:8000/books/1/
```

Jeżeli w bazie istnieje książka o:

```text
id = 1
```

powinny pojawić się jej dane.

---

# 11. Problem z `objects.get()`

Spróbuj wejść na:

```text
http://127.0.0.1:8000/books/99999/
```

Najprawdopodobniej taki rekord nie istnieje.

Przy:

```python
Book.objects.get(
    id=book_id,
)
```

Django zgłosi wyjątek:

```text
Book.DoesNotExist
```

Podczas tworzenia aplikacji zobaczymy stronę błędu.

Dla użytkownika chcemy jednak zwrócić:

```text
404
```

czyli:

> taki zasób nie istnieje.

---

# 12. `get_object_or_404()`

Django posiada wygodną funkcję:

```python
get_object_or_404
```

Zmień import:

```python
from django.shortcuts import get_object_or_404, render
```

Następnie widok:

```python
def book_detail(request, book_id):
    book = get_object_or_404(
        Book,
        id=book_id,
    )

    context = {
        "book": book,
    }

    return render(
        request,
        "biblioteka/book_detail.html",
        context,
    )
```

Teraz:

```text
/books/99999/
```

zwróci poprawną odpowiedź 404.

---

# 13. Co robi `get_object_or_404()`?

Możemy myśleć o nim tak:

```text
spróbuj znaleźć obiekt
        |
        +-- znaleziono
        |      |
        |      v
        |   zwróć obiekt
        |
        +-- nie znaleziono
               |
               v
             404
```

Jest to bardzo częsty sposób pobierania pojedynczego rekordu w widoku.

---

# 14. Link „Szczegóły” na liście

W:

```text
book_list.html
```

w każdej karcie dodaj:

```django
<a
    class="button-link"
    href="{% url 'book_detail' book.id %}"
>
    Szczegóły
</a>
```

Przykładowa karta:

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
            <strong>Rok:</strong>
            {{ book.year }}
        </p>

    </div>

    <a
        class="button-link"
        href="{% url 'book_detail' book.id %}"
    >
        Szczegóły
    </a>

</article>
```

---

# 15. Dynamiczny `{% url %}`

Wcześniej używaliśmy:

```django
{% url 'book_list' %}
```

Teraz adres potrzebuje dodatkowego argumentu:

```text
id książki
```

Dlatego używamy:

```django
{% url 'book_detail' book.id %}
```

Jeżeli:

```text
book.id = 3
```

Django wygeneruje:

```text
/books/3/
```

Nie wpisujemy tego adresu ręcznie.

---

# 16. Relacja `Book -> Author`

Każda książka posiada:

```python
book.author
```

Dlatego w szablonie możemy użyć:

```django
{{ book.author }}
```

albo:

```django
{{ book.author.first_name }}
```

i:

```django
{{ book.author.last_name }}
```

Przykład:

```django
<p>
    <strong>Autor:</strong>
    {{ book.author.first_name }}
    {{ book.author.last_name }}
</p>
```

Django automatycznie korzysta z relacji zapisanej przez:

```text
ForeignKey
```

---

# 17. Relacja odwrotna `Author -> Book`

W modelu mamy:

```python
related_name="books"
```

Dlatego autor posiada:

```python
author.books
```

Przykład w shellu:

```bash
python manage.py shell
```

```python
from biblioteka.models import Author
```

```python
author = Author.objects.get(
    last_name="Lem",
)
```

```python
author.books.all()
```

Możemy więc przejść w obie strony:

```text
book.author
```

oraz:

```text
author.books.all()
```

---

# 18. Szczegóły autora

Naturalnym kolejnym krokiem jest:

```text
/authors/1/
```

Dodaj routing:

```python
path(
    "authors/<int:author_id>/",
    views.author_detail,
    name="author_detail",
),
```

---

# 19. Widok `author_detail`

Dodaj:

```python
def author_detail(request, author_id):
    author = get_object_or_404(
        Author,
        id=author_id,
    )

    context = {
        "author": author,
    }

    return render(
        request,
        "biblioteka/author_detail.html",
        context,
    )
```

Nie musimy osobno pobierać książek.

W szablonie mamy dostęp do:

```python
author.books.all()
```

---

# 20. Szablon autora

Utwórz:

```text
biblioteka/templates/biblioteka/author_detail.html
```

Wklej:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    {{ author }} — Biblioteka
{% endblock %}


{% block content %}

    <section>

        <h2>
            {{ author.first_name }}
            {{ author.last_name }}
        </h2>

        <h3>Książki autora</h3>

        <div class="book-list">

            {% for book in author.books.all %}

                <article class="book-card">

                    <h4>
                        {{ book.title }}
                    </h4>

                    <p>
                        Rok:
                        {{ book.year }}
                    </p>

                    <a
                        class="button-link"
                        href="{% url 'book_detail' book.id %}"
                    >
                        Szczegóły
                    </a>

                </article>

            {% empty %}

                <p>
                    Ten autor nie ma jeszcze książek w bazie.
                </p>

            {% endfor %}

        </div>

        <p>
            <a
                class="button-link"
                href="{% url 'author_list' %}"
            >
                Powrót do autorów
            </a>
        </p>

    </section>

{% endblock %}
```

---

# 21. Link do autora na liście

W:

```text
author_list.html
```

dodaj:

```django
<a
    class="button-link"
    href="{% url 'author_detail' author.id %}"
>
    Szczegóły
</a>
```

Przykład:

```django
<article class="author-card">

    <h3>
        {{ author.first_name }}
        {{ author.last_name }}
    </h3>

    <p>
        Liczba książek:
        {{ author.books.count }}
    </p>

    <a
        class="button-link"
        href="{% url 'author_detail' author.id %}"
    >
        Szczegóły
    </a>

</article>
```

---

# 22. Link do autora ze szczegółów książki

W:

```text
book_detail.html
```

zamiast zwykłego tekstu autora możemy użyć linku:

```django
<p>
    <strong>Autor:</strong>

    <a href="{% url 'author_detail' book.author.id %}">
        {{ book.author }}
    </a>
</p>
```

Teraz możemy przejść:

```text
książka
   |
   v
autor
   |
   v
inne książki autora
```

---

# 23. Sortowanie QuerySetu

Obecnie:

```python
Book.objects.all()
```

nie mówi wprost, w jakiej kolejności mają pojawić się rekordy.

Możemy użyć:

```python
Book.objects.all().order_by(
    "title",
)
```

Książki zostaną posortowane rosnąco po tytule.

---

# 24. Sortowanie malejące

Znak:

```text
-
```

oznacza kolejność malejącą.

Przykład:

```python
Book.objects.all().order_by(
    "-year",
)
```

Najpierw pojawią się nowsze książki.

Przykład:

```text
2024
2001
1984
1961
```

---

# 25. Kilka pól sortowania

Możemy użyć:

```python
Book.objects.all().order_by(
    "author__last_name",
    "title",
)
```

Oznacza:

```text
najpierw nazwisko autora
potem tytuł książki
```

Zapis:

```text
author__last_name
```

przechodzi przez relację:

```text
Book -> Author -> last_name
```

---

# 26. Filtrowanie po zwykłym polu

Przykład:

```python
Book.objects.filter(
    available=True,
)
```

Inny przykład:

```python
Book.objects.filter(
    year=1961,
)
```

---

# 27. Filtrowanie przez relację

Możemy filtrować książki po danych autora.

Przykład:

```python
Book.objects.filter(
    author__last_name="Lem",
)
```

Czytamy:

> książki, których autor ma nazwisko Lem.

Podwójne podkreślenie:

```text
__
```

pozwala przechodzić przez relacje i używać dodatkowych operatorów ORM.

---

# 28. `contains` i `icontains`

Możemy wyszukiwać fragment tekstu.

Przykład:

```python
Book.objects.filter(
    title__contains="Sol",
)
```

`contains` rozróżnia wielkość liter zależnie od bazy danych.

W praktyce wyszukiwania często wygodniejszy jest:

```python
Book.objects.filter(
    title__icontains="sol",
)
```

`icontains` oznacza w przybliżeniu:

> zawiera tekst bez zwracania uwagi na wielkość liter.

---

# 29. Uruchamiamy działające wyszukiwanie

Wcześniej w HTML przygotowaliśmy formularz:

```html
<form method="get">
```

Teraz możemy go naprawdę obsłużyć.

Zmień widok:

```python
def book_list(request):
    books = Book.objects.all().order_by(
        "title",
    )

    query = request.GET.get(
        "query",
        "",
    )

    if query:
        books = books.filter(
            title__icontains=query,
        )

    context = {
        "books": books,
        "query": query,
    }

    return render(
        request,
        "biblioteka/book_list.html",
        context,
    )
```

---

# 30. `request.GET`

Jeżeli adres wygląda:

```text
/books/?query=sol
```

to:

```python
request.GET.get(
    "query",
    "",
)
```

zwróci:

```text
sol
```

Jeżeli parametr nie istnieje, otrzymamy:

```text
""
```

czyli pusty tekst.

---

# 31. Formularz wyszukiwania w `book_list.html`

Dodaj przed listą książek:

```django
<form
    class="search-form"
    method="get"
>

    <div>
        <label for="query">
            Tytuł:
        </label>

        <input
            type="text"
            id="query"
            name="query"
            value="{{ query }}"
            placeholder="np. Solaris"
        >
    </div>

    <button type="submit">
        Szukaj
    </button>

</form>
```

Po wpisaniu:

```text
sol
```

adres będzie wyglądał:

```text
/books/?query=sol
```

a Django wykona:

```python
Book.objects.filter(
    title__icontains="sol",
)
```

---

# 32. Dlaczego `value="{{ query }}"`?

Mamy:

```django
value="{{ query }}"
```

Dzięki temu po wyszukaniu wpisany tekst nadal pozostaje w polu formularza.

Bez tego po odświeżeniu wyników pole ponownie byłoby puste.

---

# 33. Przycisk wyczyszczenia wyników

Możemy dodać:

```django
<a
    class="button-link"
    href="{% url 'book_list' %}"
>
    Wyczyść
</a>
```

Ten link prowadzi do:

```text
/books/
```

bez parametru:

```text
?query=...
```

czyli ponownie wyświetla wszystkie książki.

---

# 34. Pełny `book_list.html`

Przykładowa wersja:

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    Książki — Biblioteka
{% endblock %}


{% block content %}

    <section>

        <h2>Lista książek</h2>

        <form
            class="search-form"
            method="get"
        >

            <div>
                <label for="query">
                    Tytuł:
                </label>

                <input
                    type="text"
                    id="query"
                    name="query"
                    value="{{ query }}"
                    placeholder="np. Solaris"
                >
            </div>

            <button type="submit">
                Szukaj
            </button>

            <a
                class="button-link"
                href="{% url 'book_list' %}"
            >
                Wyczyść
            </a>

        </form>

        <div class="book-list">

            {% for book in books %}

                <article
                    class="book-card
                    {% if book.available %}
                        available
                    {% else %}
                        unavailable
                    {% endif %}"
                >

                    <h3>
                        {{ book.title }}
                    </h3>

                    <div class="book-info">

                        <p>
                            <strong>Autor:</strong>
                            {{ book.author }}
                        </p>

                        <p>
                            <strong>Rok:</strong>
                            {{ book.year }}
                        </p>

                    </div>

                    <a
                        class="button-link"
                        href="{% url 'book_detail' book.id %}"
                    >
                        Szczegóły
                    </a>

                </article>

            {% empty %}

                <p>
                    Nie znaleziono książek.
                </p>

            {% endfor %}

        </div>

    </section>

{% endblock %}
```

---

# 35. Filtrowanie po dostępności

Możemy rozbudować wyszukiwanie o prosty filtr:

```text
wszystkie
dostępne
niedostępne
```

W szablonie dodaj do formularza:

```django
<div>
    <label for="available">
        Dostępność:
    </label>

    <select
        id="available"
        name="available"
    >
        <option value="">
            Wszystkie
        </option>

        <option
            value="yes"
            {% if available_filter == "yes" %}
                selected
            {% endif %}
        >
            Dostępne
        </option>

        <option
            value="no"
            {% if available_filter == "no" %}
                selected
            {% endif %}
        >
            Niedostępne
        </option>
    </select>
</div>
```

---

# 36. Obsługa filtra w widoku

Zmień:

```python
def book_list(request):
```

na:

```python
def book_list(request):
    books = Book.objects.all().order_by(
        "title",
    )

    query = request.GET.get(
        "query",
        "",
    )

    available_filter = request.GET.get(
        "available",
        "",
    )

    if query:
        books = books.filter(
            title__icontains=query,
        )

    if available_filter == "yes":
        books = books.filter(
            available=True,
        )

    elif available_filter == "no":
        books = books.filter(
            available=False,
        )

    context = {
        "books": books,
        "query": query,
        "available_filter": available_filter,
    }

    return render(
        request,
        "biblioteka/book_list.html",
        context,
    )
```

Przykładowy adres:

```text
/books/?query=sol&available=yes
```

---

# 37. Łączenie filtrów

Zauważ:

```python
books = Book.objects.all()
```

następnie:

```python
books = books.filter(
    title__icontains=query,
)
```

a później:

```python
books = books.filter(
    available=True,
)
```

Filtry można składać krok po kroku.

W efekcie otrzymujemy:

```text
książki
których tytuł pasuje do wyszukiwania
I
które są dostępne
```

Nie musimy pobierać wszystkich danych do listy Pythona i filtrować ich ręcznie.

ORM buduje odpowiednie zapytanie do bazy.

---

# 38. `QuerySet`

Wynik:

```python
Book.objects.all()
```

nie jest zwykłą listą Pythona.

Jest to:

```text
QuerySet
```

QuerySet reprezentuje zestaw obiektów pobieranych z bazy.

Możemy na nim wykonywać m.in.:

```text
filter()
order_by()
count()
first()
last()
```

Na potrzeby tego kursu nie musimy znać wszystkich metod ORM.

---

# 39. `first()` i `last()`

Przykład w shellu:

```python
Book.objects.order_by(
    "title",
).first()
```

oraz:

```python
Book.objects.order_by(
    "title",
).last()
```

Zwracają pierwszy lub ostatni obiekt według danego uporządkowania.

---

# 40. `exists()`

Możemy sprawdzić, czy istnieje przynajmniej jeden rekord:

```python
Book.objects.filter(
    available=True,
).exists()
```

Wynik:

```text
True
```

lub:

```text
False
```

Nie jest to dziś konieczne w naszej stronie, ale warto znać ten sposób sprawdzania istnienia danych.

---

# 41. Pełny routing po laboratorium

Plik:

```text
biblioteka/urls.py
```

może wyglądać:

```python
from django.urls import path

from . import views


urlpatterns = [
    path(
        "",
        views.home,
        name="home",
    ),

    path(
        "books/",
        views.book_list,
        name="book_list",
    ),

    path(
        "books/<int:book_id>/",
        views.book_detail,
        name="book_detail",
    ),

    path(
        "authors/",
        views.author_list,
        name="author_list",
    ),

    path(
        "authors/<int:author_id>/",
        views.author_detail,
        name="author_detail",
    ),

    path(
        "contact/",
        views.contact,
        name="contact",
    ),

    path(
        "about/",
        views.about,
        name="about",
    ),
]
```

---

# 42. Pełny `views.py` po laboratorium

Przykładowo:

```python
from django.shortcuts import get_object_or_404, render

from .models import Author, Book


def home(request):
    context = {
        "books_count": Book.objects.count(),
        "authors_count": Author.objects.count(),
    }

    return render(
        request,
        "biblioteka/home.html",
        context,
    )


def book_list(request):
    books = Book.objects.all().order_by(
        "title",
    )

    query = request.GET.get(
        "query",
        "",
    )

    available_filter = request.GET.get(
        "available",
        "",
    )

    if query:
        books = books.filter(
            title__icontains=query,
        )

    if available_filter == "yes":
        books = books.filter(
            available=True,
        )

    elif available_filter == "no":
        books = books.filter(
            available=False,
        )

    context = {
        "books": books,
        "query": query,
        "available_filter": available_filter,
    }

    return render(
        request,
        "biblioteka/book_list.html",
        context,
    )


def book_detail(request, book_id):
    book = get_object_or_404(
        Book,
        id=book_id,
    )

    context = {
        "book": book,
    }

    return render(
        request,
        "biblioteka/book_detail.html",
        context,
    )


def author_list(request):
    authors = Author.objects.all().order_by(
        "last_name",
        "first_name",
    )

    context = {
        "authors": authors,
    }

    return render(
        request,
        "biblioteka/author_list.html",
        context,
    )


def author_detail(request, author_id):
    author = get_object_or_404(
        Author,
        id=author_id,
    )

    context = {
        "author": author,
    }

    return render(
        request,
        "biblioteka/author_detail.html",
        context,
    )


def contact(request):
    return render(
        request,
        "biblioteka/contact.html",
    )


def about(request):
    return render(
        request,
        "biblioteka/about.html",
    )
```

---

# 43. Aktualny przepływ danych

Dla:

```text
/books/2/
```

mamy:

```text
GET /books/2/
      |
      v
urls.py
      |
      v
book_id = 2
      |
      v
book_detail()
      |
      v
get_object_or_404()
      |
      v
Book
      |
      v
SQLite
      |
      v
konkretny obiekt
      |
      v
book_detail.html
      |
      v
HTML
      |
      v
przeglądarka
```

---

# 44. Aktualna struktura szablonów

Po dzisiejszych zajęciach:

```text
biblioteka/
└── templates/
    └── biblioteka/
        ├── base.html
        ├── home.html
        ├── book_list.html
        ├── book_detail.html
        ├── author_list.html
        ├── author_detail.html
        ├── contact.html
        └── about.html
```

Zaczyna pojawiać się powtarzalny schemat:

```text
lista
szczegóły
```

W kolejnych zajęciach dojdą:

```text
formularz
edycja
usuwanie
```

---

# 45. Sprawdzamy aplikację

Uruchom:

```bash
python manage.py check
```

oraz:

```bash
python manage.py runserver
```

Sprawdź:

```text
/books/
```

```text
/books/1/
```

```text
/authors/
```

```text
/authors/1/
```

Sprawdź też nieistniejący rekord:

```text
/books/99999/
```

Powinien pojawić się błąd 404, a nie wyjątek `DoesNotExist`.

---

# 46. Zapisujemy zmiany w Git

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
git commit -m "Dodanie szczegółów obiektów i filtrowania ORM"
```

Wyślij:

```bash
git push
```

---

# 47. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Przykładowe rozwiązania zostaną pokazane na początku następnego laboratorium.

---

## Zadanie 1 — wyszukiwanie po autorze

Rozbuduj wyszukiwanie książek tak, aby wpisany tekst mógł być dopasowany nie tylko do tytułu, ale również do nazwiska autora.

Możesz skorzystać z:

```python
from django.db.models import Q
```

oraz:

```python
Q(...)
```

Przykładowa idea:

```python
books = books.filter(
    Q(title__icontains=query)
    |
    Q(author__last_name__icontains=query)
)
```

Sprawdź wyszukiwanie:

```text
Solaris
```

oraz:

```text
Lem
```

---

## Zadanie 2 — filtrowanie po roku

Dodaj do formularza pole:

```text
Rok od
```

o nazwie:

```text
year_from
```

Następnie wykorzystaj:

```python
year__gte
```

Przykład:

```python
books = books.filter(
    year__gte=year_from,
)
```

Pamiętaj, aby filtrować tylko wtedy, gdy użytkownik podał wartość.

---

## Zadanie 3 — sortowanie z formularza

Dodaj:

```html
<select name="sort">
```

z opcjami:

```text
Tytuł A-Z
Tytuł Z-A
Rok rosnąco
Rok malejąco
```

W widoku na podstawie wartości `sort` użyj odpowiedniego:

```python
order_by(...)
```

Przykłady:

```python
order_by("title")
order_by("-title")
order_by("year")
order_by("-year")
```

---

## Zadanie 4 — ostatnie książki na stronie głównej

W widoku:

```python
home
```

pobierz trzy książki o najwyższym `id`:

```python
latest_books = Book.objects.order_by(
    "-id",
)[:3]
```

Przekaż je przez `context`.

Na:

```text
home.html
```

wyświetl sekcję:

```text
Ostatnio dodane książki
```

z linkami do szczegółów.

---

## Zadanie 5 — linki między obiektami

Na stronie autora każda książka powinna prowadzić do:

```text
book_detail
```

Na stronie książki autor powinien prowadzić do:

```text
author_detail
```

Jeżeli jeszcze tego nie zrobiłeś podczas zajęć, przygotuj kompletne przejście:

```text
lista książek
   |
   v
szczegóły książki
   |
   v
szczegóły autora
   |
   v
inna książka autora
```

---

# 48. Co dalej?

Mamy już:

```text
modele
relacje
ORM
listy
szczegóły
filtrowanie
sortowanie
wyszukiwanie
```

Do tej pory dane zmieniamy jednak głównie przez:

```text
panel administratora
```

Na kolejnym laboratorium zaczniemy tworzyć formularze Django.

Zamiast ręcznie pisać wszystkie pola:

```html
<input>
<select>
<textarea>
```

wykorzystamy:

```text
ModelForm
```

i przygotujemy pierwszą funkcję:

```text
Dodaj książkę
```

dostępną bezpośrednio z naszej aplikacji.

Schemat zacznie wyglądać:

```text
formularz
   |
   v
POST
   |
   v
Django
   |
   v
walidacja
   |
   v
model
   |
   v
baza danych
```

Będzie to pierwszy krok do pełnego:

```text
CRUD
```
