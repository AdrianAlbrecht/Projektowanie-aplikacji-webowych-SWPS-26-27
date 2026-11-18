# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 09 — Modele Django, baza danych, migracje i ORM

---

## 1. Cel zajęć

Na poprzednich zajęciach przenieśliśmy frontend Biblioteki do szablonów Django i przekazywaliśmy dane z widoków przez `context`.

Lista książek nadal była jednak wpisana ręcznie w:

```text
views.py
```

Przykład:

```python
books = [
    {
        "title": "Solaris",
        "author": "Stanisław Lem",
        "year": 1961,
        "available": True,
    },
]
```

Dzisiaj zastąpimy takie dane **prawdziwą bazą danych**.

Po zakończeniu laboratorium powinniśmy:

- rozumieć rolę modelu Django,
- utworzyć modele `Author` i `Book`,
- utworzyć relację pomiędzy modelami,
- rozumieć podstawową rolę migracji,
- wykonać `makemigrations` i `migrate`,
- wiedzieć, gdzie Django przechowuje lokalną bazę SQLite,
- utworzyć konto administratora,
- dodawać dane przez panel administracyjny Django,
- wykonać podstawowe zapytania za pomocą ORM,
- pobierać książki z bazy w widoku,
- wyświetlać dane z bazy w szablonie.

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
```

Uruchom:

```bash
python manage.py check
```

---

# 3. Przykładowe rozwiązania z poprzedniego laboratorium

Na końcu `lab08` znajdowało się pięć nieobowiązkowych zadań.

Poniżej znajduje się przykładowe rozwiązanie każdego z nich.

---

## Zadanie 1 — autorzy z `context`

W:

```text
biblioteka/views.py
```

możemy przygotować dane:

```python
def author_list(request):
    authors = [
        {
            "name": "Stanisław Lem",
            "country": "Polska",
        },
        {
            "name": "J.R.R. Tolkien",
            "country": "Wielka Brytania",
        },
        {
            "name": "George Orwell",
            "country": "Wielka Brytania",
        },
    ]

    context = {
        "authors": authors,
    }

    return render(
        request,
        "biblioteka/author_list.html",
        context,
    )
```

Szablon:

```text
biblioteka/templates/biblioteka/author_list.html
```

może wyglądać:

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
                        {{ author.name }}
                    </h3>

                    <p>
                        <strong>Kraj:</strong>
                        {{ author.country }}
                    </p>
                </article>

            {% endfor %}

        </div>
    </section>

{% endblock %}
```

---

## Zadanie 2 — pusta lista

W widoku można tymczasowo ustawić:

```python
books = []
```

W:

```text
book_list.html
```

możemy użyć:

```django
{% for book in books %}

    <article class="book-card">
        <h3>
            {{ book.title }}
        </h3>
    </article>

{% empty %}

    <p>
        Brak książek do wyświetlenia.
    </p>

{% endfor %}
```

`{% empty %}` zostanie wykonane wtedy, gdy kolekcja:

```text
books
```

nie zawiera żadnego elementu.

Po sprawdzeniu działania przywracamy wcześniejsze dane.

---

## Zadanie 3 — strona „O bibliotece”

Widok:

```python
def about(request):
    return render(
        request,
        "biblioteka/about.html",
    )
```

Routing:

```python
path("about/", views.about, name="about"),
```

Szablon:

```text
biblioteka/templates/biblioteka/about.html
```

```django
{% extends "biblioteka/base.html" %}


{% block title %}
    O bibliotece
{% endblock %}


{% block content %}

    <section>
        <h2>O bibliotece</h2>

        <p>
            Biblioteka jest projektem ćwiczeniowym rozwijanym
            podczas laboratoriów z Projektowania aplikacji webowych.
        </p>
    </section>

{% endblock %}
```

W:

```text
base.html
```

możemy dodać:

```django
<a href="{% url 'about' %}">
    O bibliotece
</a>
```

---

## Zadanie 4 — dodatkowe dane na stronie głównej

Widok:

```python
def home(request):
    context = {
        "books_count": 3,
        "authors_count": 3,
    }

    return render(
        request,
        "biblioteka/home.html",
        context,
    )
```

W:

```text
home.html
```

możemy wyświetlić:

```django
<section>
    <h2>Biblioteka w liczbach</h2>

    <p>
        Liczba książek:
        {{ books_count }}
    </p>

    <p>
        Liczba autorów:
        {{ authors_count }}
    </p>
</section>
```

Dzisiaj zastąpimy te liczby wartościami obliczanymi na podstawie **prawdziwej bazy danych**.

---

## Zadanie 5 — własny podział szablonów

Przykład dla planera nauki:

```text
templates/
└── planner/
    ├── base.html
    ├── task_list.html
    ├── task_detail.html
    └── task_form.html
```

Możemy przyjąć:

```text
base.html
= wspólna struktura

task_list.html
= lista zadań

task_detail.html
= jeden konkretny element

task_form.html
= dodawanie lub edycja danych
```

Podobny schemat będzie później przydatny również w projekcie zaliczeniowym.

---

# 4. Po co nam baza danych?

Do tej pory nasze dane znajdowały się bezpośrednio w kodzie:

```python
books = [
    {
        "title": "Solaris",
        ...
    },
]
```

Ma to kilka problemów.

Po restarcie lub zmianie kodu dane nadal są tylko częścią programu.

Użytkownik nie może wygodnie:

```text
dodawać książek
edytować książek
usuwać książek
```

Potrzebujemy miejsca przeznaczonego do trwałego przechowywania danych.

W naszym projekcie będzie to:

```text
SQLite
```

---

# 5. SQLite w Django

Domyślny projekt Django jest już skonfigurowany do pracy z SQLite.

Możemy to zobaczyć w:

```text
config/settings.py
```

Fragment:

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}
```

Nie musimy tego zmieniać.

Po wykonaniu migracji w katalogu projektu pojawi się:

```text
db.sqlite3
```

Jest to plik naszej lokalnej bazy danych.

> [!IMPORTANT]
> Pliku `db.sqlite3` nie edytujemy ręcznie.
>
> Strukturą bazy będziemy zarządzali przez modele i migracje Django.

---

# 6. Model Django

Model opisuje **rodzaj danych**, które chcemy przechowywać.

Przykładowo dla autora potrzebujemy:

```text
imię
nazwisko
```

Dla książki:

```text
tytuł
autor
rok wydania
kategoria
dostępność
opis
```

W Django model jest klasą Pythona.

---

# 7. Model `Author`

Otwórz:

```text
biblioteka/models.py
```

Wklej:

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
```

---

# 8. Co oznacza `models.Model`?

Mamy:

```python
class Author(models.Model):
```

Oznacza to, że:

```text
Author
```

jest modelem Django.

Django będzie potrafiło na jego podstawie m.in.:

- utworzyć tabelę w bazie,
- zapisywać autorów,
- pobierać autorów,
- aktualizować dane,
- usuwać dane.

Nie piszemy ręcznie poleceń SQL.

---

# 9. `CharField`

Mamy:

```python
first_name = models.CharField(
    max_length=100,
)
```

`CharField` służy do przechowywania krótkiego tekstu.

Musimy określić:

```text
max_length
```

czyli maksymalną długość.

Podobnie:

```python
last_name = models.CharField(
    max_length=100,
)
```

---

# 10. `__str__`

Mamy:

```python
def __str__(self):
    return f"{self.first_name} {self.last_name}"
```

Metoda określa, w jaki sposób obiekt ma być prezentowany jako tekst.

Bez niej Django może wyświetlać coś podobnego do:

```text
Author object (1)
```

Dzięki `__str__` zobaczymy:

```text
Stanisław Lem
```

Będzie to szczególnie widoczne w panelu administratora.

---

# 11. Model `Book`

Pod modelem `Author` dodaj:

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

    def __str__(self):
        return self.title
```

Pełny:

```text
models.py
```

powinien teraz zawierać dwa modele.

---

# 12. Podstawowe typy pól

W naszym przykładzie wykorzystujemy kilka typów.

## `CharField`

```python
models.CharField(
    max_length=200,
)
```

Krótki tekst.

Przykład:

```text
tytuł
imię
nazwisko
kategoria
```

---

## `PositiveIntegerField`

```python
models.PositiveIntegerField()
```

Dodatnia liczba całkowita.

Używamy jej dla:

```text
roku wydania
```

---

## `BooleanField`

```python
models.BooleanField(
    default=True,
)
```

Wartość:

```text
True
False
```

W naszym przypadku:

```text
książka dostępna / niedostępna
```

---

## `TextField`

```python
models.TextField(
    blank=True,
)
```

Dłuższy tekst.

W naszym przypadku:

```text
opis książki
```

---

# 13. `blank=True`

Przykład:

```python
category = models.CharField(
    max_length=100,
    blank=True,
)
```

oznacza, że pole może pozostać puste w formularzach Django.

Podobnie:

```python
description = models.TextField(
    blank=True,
)
```

Na razie wystarczy rozumieć:

```text
blank=True
= pole nie musi zostać wypełnione
```

---

# 14. Relacja `ForeignKey`

Najważniejszym nowym elementem jest:

```python
author = models.ForeignKey(
    Author,
    on_delete=models.CASCADE,
    related_name="books",
)
```

Oznacza:

> każda książka jest przypisana do jednego autora.

Schemat:

```text
Author
   |
   | 1
   |
   +--------< wiele książek

Book
Book
Book
```

Przykład:

```text
Stanisław Lem
   |
   +-- Solaris
   +-- Eden
   +-- Cyberiada
```

---

# 15. `on_delete=models.CASCADE`

Mamy:

```python
on_delete=models.CASCADE
```

Django musi wiedzieć:

> co zrobić z książkami, jeżeli usuniemy ich autora?

`CASCADE` oznacza:

```text
usuń autora
      |
      v
usuń również powiązane książki
```

Nie oznacza to, że `CASCADE` jest najlepszym rozwiązaniem w każdej aplikacji.

Na tym etapie korzystamy z niego jako prostego przykładu relacji.

---

# 16. Co to są migracje?

Samo zapisanie modelu w:

```text
models.py
```

nie zmienia jeszcze bazy.

Django potrzebuje informacji:

> jakie zmiany w strukturze bazy należy wykonać?

Do tego służą:

```text
migracje
```

Schemat:

```text
models.py
   |
   v
makemigrations
   |
   v
plik migracji
   |
   v
migrate
   |
   v
baza danych
```

---

# 17. `makemigrations`

Zatrzymaj serwer, jeżeli jest to potrzebne, lub otwórz drugi terminal.

Wykonaj:

```bash
python manage.py makemigrations
```

Powinien pojawić się komunikat podobny do:

```text
Migrations for 'biblioteka':
  biblioteka/migrations/0001_initial.py
    + Create model Author
    + Create model Book
```

Django utworzyło plik opisujący zmianę struktury bazy.

---

# 18. Plik migracji

W katalogu:

```text
biblioteka/migrations/
```

pojawi się:

```text
0001_initial.py
```

Nie będziemy pisać tego pliku ręcznie.

Na początku kursu przyjmujemy prostą zasadę:

```text
zmieniam models.py
        |
        v
makemigrations
        |
        v
migrate
```

---

# 19. `migrate`

Wykonaj:

```bash
python manage.py migrate
```

Django zastosuje migracje do bazy.

W katalogu projektu powinien pojawić się:

```text
db.sqlite3
```

Pierwsze `migrate` wykonuje również migracje potrzebne przez wbudowane elementy Django, np.:

```text
auth
admin
sessions
```

Dlatego lista wykonanych migracji może być dość długa.

---

# 20. Sprawdzamy migracje

Możemy wykonać:

```bash
python manage.py showmigrations
```

Przy zastosowanych migracjach zobaczymy:

```text
[X]
```

Przykład:

```text
biblioteka
 [X] 0001_initial
```

Nie trzeba zapamiętywać wszystkich migracji systemowych Django.

---

# 21. `db.sqlite3` i Git

Baza:

```text
db.sqlite3
```

zawiera lokalne dane.

W projekcie zespołowym nie chcemy przesyłać jej przez Git i powodować konfliktów binarnego pliku.

Dodaj do:

```text
.gitignore
```

wiersz:

```gitignore
# Lokalna baza danych
db.sqlite3
```

Po pobraniu projektu na innym komputerze bazę odtwarzamy poleceniem:

```bash
python manage.py migrate
```

Dane testowe można ponownie dodać przez panel administratora.

> [!CAUTION]
> Jeżeli już raz dodaliśmy do repozytorium gita nasz plik bazy danych to już tam zostanie - w naszym przypadku będzie to wygodniejsze.

---

# 22. Panel administratora Django

Django posiada gotowy panel administracyjny.

Adres:

```text
http://127.0.0.1:8000/admin/
```

Żeby z niego skorzystać, potrzebujemy konta administratora.

---

# 23. Tworzymy superusera

Wykonaj:

```bash
python manage.py createsuperuser
```

Django poprosi o:

```text
Username
Email address
Password
```

Adres e-mail może być opcjonalny.

Hasło podczas wpisywania może nie być widoczne w terminalu.

To normalne.

> [!IMPORTANT]
> Nie używaj podczas zajęć hasła wykorzystywanego do prawdziwych kont.
>
> To lokalne konto testowe.

---

# 24. Rejestrujemy modele w adminie

Otwórz:

```text
biblioteka/admin.py
```

Wklej:

```python
from django.contrib import admin

from .models import Author, Book


admin.site.register(Author)
admin.site.register(Book)
```

Uruchom:

```bash
python manage.py runserver
```

Otwórz:

```text
http://127.0.0.1:8000/admin/
```

Zaloguj się kontem utworzonym przez:

```text
createsuperuser
```

Powinny pojawić się:

```text
Authors
Books
```

---

# 25. Dodajemy autorów

W panelu administratora dodaj:

```text
Stanisław Lem
Bolesław Prus
J.R.R. Tolkien
```

Dzięki:

```python
def __str__(self):
```

na liście powinniśmy widzieć nazwiska i imiona zamiast:

```text
Author object
```

---

# 26. Dodajemy książki

Dodaj minimum trzy książki.

### Solaris

```text
Tytuł:
Solaris

Autor:
Stanisław Lem

Rok:
1961

Kategoria:
Science fiction

Dostępna:
Tak
```

### Lalka

```text
Tytuł:
Lalka

Autor:
Bolesław Prus

Rok:
1890

Kategoria:
Powieść

Dostępna:
Nie
```

### Hobbit

```text
Tytuł:
Hobbit

Autor:
J.R.R. Tolkien

Rok:
1937

Kategoria:
Fantasy

Dostępna:
Tak
```

Opis może być krótki lub pozostać pusty.

---

# 27. ORM

ORM oznacza:

> **Object-Relational Mapping**

Pozwala pracować z bazą danych za pomocą obiektów i kodu Pythona.

Zamiast pisać ręcznie SQL:

```sql
SELECT * FROM biblioteka_book;
```

możemy użyć:

```python
Book.objects.all()
```

Na naszych zajęciach będziemy korzystać głównie z ORM Django.

---

# 28. Django shell

Zatrzymaj serwer lub otwórz drugi terminal.

Uruchom:

```bash
python manage.py shell
```

Zaimportuj modele:

```python
from biblioteka.models import Author, Book
```

---

# 29. Pobieramy wszystkie książki

W shellu:

```python
Book.objects.all()
```

Powinniśmy zobaczyć obiekty książek.

Dzięki:

```python
def __str__(self):
    return self.title
```

będą prezentowane przy użyciu tytułów.

---

# 30. Liczba książek

```python
Book.objects.count()
```

Powinniśmy otrzymać np.:

```text
3
```

---

# 31. Dostępne książki

```python
Book.objects.filter(
    available=True,
)
```

Zapytanie oznacza:

> pobierz wszystkie książki, dla których `available=True`.

Możemy też użyć:

```python
Book.objects.filter(
    available=False,
)
```

---

# 32. Pobieranie jednego obiektu

Jeżeli istnieje książka o `id=1`:

```python
Book.objects.get(
    id=1,
)
```

Możemy zapisać ją do zmiennej:

```python
book = Book.objects.get(
    id=1,
)
```

i sprawdzić:

```python
book.title
```

```python
book.year
```

```python
book.author
```

---

# 33. Relacja w ORM

Mając:

```python
book = Book.objects.get(
    id=1,
)
```

możemy odczytać autora:

```python
book.author
```

oraz:

```python
book.author.first_name
```

```python
book.author.last_name
```

Z drugiej strony możemy pobrać książki autora.

Przykład:

```python
author = Author.objects.get(
    last_name="Lem",
)
```

Następnie:

```python
author.books.all()
```

Nazwa:

```text
books
```

pochodzi z:

```python
related_name="books"
```

w modelu `Book`.

---

# 34. Kończymy shell

Wykonaj:

```python
exit()
```

---

# 35. Dane z bazy w `book_list`

Najważniejszy krok dzisiejszych zajęć.

Otwórz:

```text
biblioteka/views.py
```

Dodaj import na początku pliku:

```python
from .models import Book
```

Usuń ręcznie przygotowaną listę, która imitowała działanie BD:

```python
books = [
    ...
]
```

i zmień widok na:

```python
def book_list(request):
    books = Book.objects.all()

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

# 36. Pełny fragment `views.py`

Może wyglądać:

```python
from django.shortcuts import render

from .models import Book


def home(request):
    return render(
        request,
        "biblioteka/home.html",
    )


def book_list(request):
    books = Book.objects.all()

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


def about(request):
    return render(
        request,
        "biblioteka/about.html",
    )
```

---

# 37. Szablon prawie nie musi się zmieniać

Nasz:

```text
book_list.html
```

już wcześniej korzystał z:

```django
{{ book.title }}
```

```django
{{ book.author }}
```

```django
{{ book.year }}
```

```django
{{ book.category }}
```

```django
{% if book.available %}
```

Dlatego po zmianie źródła danych szablon nadal działa.

To bardzo ważne.

Wcześniej:

```text
books
```

było listą słowników.

Teraz:

```text
books
```

jest wynikiem zapytania do bazy.

Szablon nadal otrzymuje dane pod tą samą nazwą.

---

# 38. Aktualizujemy stronę główną

W poprzednim labie mogliśmy mieć:

```python
context = {
    "books_count": 3,
    "authors_count": 3,
}
```

Teraz liczby możemy pobrać z bazy.

Zmodyfikuj import modeli na:

```python
from .models import Author, Book
```

A widok `home` na:

```python
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
```

Liczby nie są już wpisane ręcznie.

Jeżeli dodamy nową książkę w panelu administratora, licznik zmieni się automatycznie.

---

# 39. Autorzy również z bazy

Zmień:

```python
def author_list(request):
```

na:

```python
def author_list(request):
    authors = Author.objects.all()

    context = {
        "authors": authors,
    }

    return render(
        request,
        "biblioteka/author_list.html",
        context,
    )
```

Szablon może wyglądać:

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

---

# 40. Pełny `views.py` po zmianach

Na koniec:

```text
biblioteka/views.py
```

może wyglądać:

```python
from django.shortcuts import render

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
    books = Book.objects.all()

    context = {
        "books": books,
    }

    return render(
        request,
        "biblioteka/book_list.html",
        context,
    )


def author_list(request):
    authors = Author.objects.all()

    context = {
        "authors": authors,
    }

    return render(
        request,
        "biblioteka/author_list.html",
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

# 41. Przepływ danych po dzisiejszych zajęciach

Mamy już:

```text
przeglądarka
     |
     | GET /books/
     v
urls.py
     |
     v
book_list()
     |
     v
Book.objects.all()
     |
     v
SQLite
     |
     v
QuerySet książek
     |
     v
context
     |
     v
book_list.html
     |
     v
HTML
     |
     v
przeglądarka
```

To jest pierwszy pełny przepływ danych od bazy do użytkownika.

---

# 42. Zmieniamy dane bez zmiany kodu

Uruchom:

```bash
python manage.py runserver
```

Otwórz:

```text
http://127.0.0.1:8000/admin/
```

Dodaj nową książkę.

Przykład:

```text
1984
George Orwell
1949
Dystopia
Dostępna
```

Jeżeli nie ma autora:

```text
George Orwell
```

najpierw go dodaj.

Następnie otwórz:

```text
http://127.0.0.1:8000/books/
```

Nowa książka powinna pojawić się automatycznie.

Nie zmienialiśmy:

```text
views.py
```

ani:

```text
book_list.html
```

Zmieniły się tylko **dane w bazie**.

---

# 43. Lepszy wygląd panelu administratora

Podstawowa rejestracja:

```python
admin.site.register(Author)
admin.site.register(Book)
```

jest wystarczająca.

Możemy jednak niewielkim kosztem poprawić listę książek.

Zmień:

```text
biblioteka/admin.py
```

na:

```python
from django.contrib import admin

from .models import Author, Book


@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    list_display = (
        "first_name",
        "last_name",
    )


@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = (
        "title",
        "author",
        "year",
        "available",
    )
```

Odśwież panel administratora.

Lista danych powinna być bardziej czytelna.

Nie będziemy szczegółowo poznawali konfiguracji admina.

Panel służy nam przede wszystkim jako **wygodne narzędzie do pracy z danymi podczas tworzenia aplikacji**.

---

# 44. Aktualna struktura projektu

Najważniejsze nowe elementy:

```text
paw_biblioteka/
│
├── biblioteka/
│   │
│   ├── migrations/
│   │   ├── __init__.py
│   │   └── 0001_initial.py
│   │
│   ├── admin.py
│   ├── models.py
│   ├── views.py
│   └── ...
│
├── config/
│   └── ...
│
├── db.sqlite3
├── manage.py
└── ...
```

`db.sqlite3` istnieje lokalnie, ale po dodaniu go do:

```text
.gitignore
```

nie jest wysyłany do repozytorium.

Migracje:

```text
biblioteka/migrations/
```

**powinny znajdować się w Git**.

Dzięki nim na innym komputerze możemy odtworzyć strukturę bazy.

---

# 45. Odtworzenie projektu na innym komputerze

Po:

```bash
git clone ADRES_REPOZYTORIUM
```

wykonujemy:

```bash
python -m venv .venv
```

aktywujemy środowisko i instalujemy zależności:

```bash
python -m pip install -r requirements.txt
```

Następnie:

```bash
python manage.py migrate
```

Jeżeli potrzebujemy panelu administratora:

```bash
python manage.py createsuperuser
```

Na końcu:

```bash
python manage.py runserver
```

Dane takie jak książki i autorzy trzeba w lokalnej bazie dodać ponownie.

---

# 46. Zapisujemy zmiany w Git

Sprawdź:

```bash
git status
```

Powinniśmy zobaczyć m.in.:

```text
biblioteka/models.py
biblioteka/admin.py
biblioteka/migrations/0001_initial.py
biblioteka/views.py
.gitignore
```

Nie powinien pojawić się:

```text
db.sqlite3
```

Dodaj zmiany:

```bash
git add .
```

Commit:

```bash
git commit -m "Dodanie modeli i bazy danych"
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

## Zadanie 1 — liczba książek autora

W:

```text
author_list.html
```

przy każdym autorze wyświetl liczbę jego książek.

Dzięki:

```python
related_name="books"
```

możemy w szablonie użyć:

```django
{{ author.books.count }}
```

Przykładowy efekt:

```text
Stanisław Lem
Liczba książek: 2
```

Dodaj przynajmniej jedną dodatkową książkę wybranego autora i sprawdź, czy licznik się zmieni.

---

## Zadanie 2 — filtrowanie dostępnych książek

W:

```text
views.py
```

tymczasowo zmień:

```python
books = Book.objects.all()
```

na:

```python
books = Book.objects.filter(
    available=True,
)
```

Sprawdź wynik.

Następnie przywróć:

```python
Book.objects.all()
```

---

## Zadanie 3 — pole liczby stron

Rozbuduj model:

```python
Book
```

o pole:

```python
pages
```

Przykład:

```python
pages = models.PositiveIntegerField(
    null=True,
    blank=True,
)
```

Następnie wykonaj:

```bash
python manage.py makemigrations
python manage.py migrate
```

Dodaj liczbę stron wybranym książkom w panelu administratora.

Wyświetl tę informację w:

```text
book_list.html
```

---

## Zadanie 4 — nowy model

Dodaj prosty model:

```text
Category
```

z jednym polem:

```text
name
```

Na tym etapie **nie musisz jeszcze łączyć go relacją z książką**.

Wymagania:

1. model `Category`,
2. `__str__`,
3. `makemigrations`,
4. `migrate`,
5. rejestracja w `admin.py`,
6. dodanie kilku kategorii w panelu administratora.

---

## Zadanie 5 — ORM w shellu

Uruchom:

```bash
python manage.py shell
```

Wykonaj:

```python
from biblioteka.models import Author, Book
```

Następnie samodzielnie sprawdź:

```python
Book.objects.all()
```

```python
Book.objects.count()
```

```python
Book.objects.filter(available=True)
```

```python
Author.objects.all()
```

oraz znajdź jednego autora przez:

```python
Author.objects.get(...)
```

i wyświetl jego książki:

```python
author.books.all()
```

---

# 48. Co dalej?

Mamy już:

```text
modele
SQLite
migracje
ORM
panel administratora
dane pobierane z bazy
```

Na następnym laboratorium rozwiniemy pracę z ORM i relacjami.

Zaczniemy również przygotowywać stronę:

```text
/books/1/
```

czyli **szczegóły konkretnej książki**.

Dzięki temu pojawi się nowy element routingu:

```text
/books/<id>/
```

i połączymy:

```text
adres URL
   |
   v
konkretny rekord z bazy
   |
   v
widok
   |
   v
szablon szczegółów
```

Będzie to kolejny krok przed formularzami Django i pełnym CRUD.
