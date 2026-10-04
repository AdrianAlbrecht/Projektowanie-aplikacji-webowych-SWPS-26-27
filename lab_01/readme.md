# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 01 — Wprowadzenie do aplikacji webowych

---

## 1. Cel zajęć

Pierwsze zajęcia mają charakter organizacyjno-wprowadzający.

Na początku omawiamy zasady przedmiotu opisane w głównym pliku `README.md`, a następnie przechodzimy do krótkiego wprowadzenia do aplikacji webowych.

Po tych zajęciach osoba studencka powinna przede wszystkim:

- wiedzieć, jak będą wyglądały kolejne laboratoria,
- znać przykładowy projekt rozwijany podczas semestru,
- rozumieć podstawowy podział aplikacji na **frontend**, **backend** i **bazę danych**,
- rozumieć model **klient–serwer**,
- wiedzieć, czym jest adres URL,
- rozumieć podstawową ideę komunikacji `request -> response`,
- wiedzieć, czym różnią się w podstawowym zakresie metody `GET` i `POST`,
- potrafić podejrzeć podstawowe informacje o stronie za pomocą narzędzi deweloperskich przeglądarki.

Na tych zajęciach **nie tworzymy jeszcze projektu Django**.

---

# 2. Jak będziemy pracować?

Podczas kolejnych laboratoriów prowadzący będzie rozwijał krok po kroku przykładową aplikację.

Materiały do każdego laboratorium będą zawierały:

1. krótkie wprowadzenie do omawianego zagadnienia,
2. gotowe fragmenty kodu,
3. informację, do jakiego pliku należy wkleić dany fragment,
4. omówienie działania kodu,
5. efekt, który powinien być widoczny po uruchomieniu aplikacji,
6. opcjonalne zadania do samodzielnego przećwiczenia.

Kod będzie przygotowany w taki sposób, aby podczas zajęć można było skupić się przede wszystkim na:

- rozumieniu działania aplikacji,
- omawianiu kolejnych elementów,
- uruchamianiu projektu,
- analizie efektów,
- modyfikowaniu gotowego przykładu.

Nie chodzi więc o przepisywanie dużych fragmentów kodu znak po znaku.

> [!NOTE]
> Jeżeli pod koniec laboratorium pojawi się opcjonalne zadanie programistyczne, przykładowe rozwiązanie może zostać krótko przedstawione na początku kolejnych zajęć.

---

# 3. Projekt przewodni — „Biblioteka”

Przez większość semestru będziemy wspólnie rozwijać jeden przykładowy projekt:

> **Biblioteka**

Dzięki temu kolejne zagadnienia nie będą oderwanymi przykładami, ale elementami jednej rozwijanej aplikacji.

Docelowo Biblioteka będzie mogła zawierać m.in.:

- książki,
- autorów,
- użytkowników,
- wypożyczenia,
- listę danych,
- szczegóły wybranego elementu,
- formularze,
- dodawanie danych,
- edycję danych,
- usuwanie danych,
- logowanie,
- ograniczanie dostępu do wybranych funkcji.

Pod koniec semestru będzie to kompletny przykład niewielkiej aplikacji Django podobnej pod względem technicznym do projektu zaliczeniowego.

> [!CAUTION]
> **Biblioteka, katalog książek oraz system wypożyczania książek są tematami niedozwolonymi w projekcie zaliczeniowym.**
>
> Projekt wykonywany podczas laboratoriów ma służyć jako wzór techniczny, a nie jako gotowa baza do skopiowania na zaliczenie.

---

# 4. Czym jest aplikacja webowa?

Aplikacja webowa to program, z którego użytkownik korzysta za pomocą przeglądarki internetowej.

Przykładowe operacje wykonywane w aplikacjach webowych:

- logowanie,
- wyszukiwanie danych,
- dodawanie nowych danych,
- edycja danych,
- wykonywanie rezerwacji,
- przesyłanie formularzy,
- przeglądanie informacji zapisanych w bazie.

W naszej przykładowej Bibliotece użytkownik będzie mógł np.:

```text
otworzyć listę książek
wybrać konkretną książkę
dodać nową książkę
edytować książkę
usunąć książkę
```

Żeby było to możliwe, kilka elementów aplikacji musi ze sobą współpracować.

---

# 5. Klient i serwer

W najprostszym ujęciu aplikacja webowa działa według schematu:

```text
KLIENT  <------------>  SERWER
```

## Klient

Klient wysyła żądanie.

W czasie naszych zajęć klientem będzie najczęściej:

```text
przeglądarka internetowa
```

np. Chrome, Firefox albo Edge.

## Serwer

Serwer:

1. odbiera żądanie,
2. wykonuje odpowiednią operację,
3. przygotowuje odpowiedź,
4. odsyła ją do klienta.

W naszym projekcie po stronie serwera będzie działał:

```text
Python + Django
```

Przykład:

```text
Użytkownik otwiera listę książek
        |
        v
Przeglądarka wysyła żądanie
        |
        v
Django przetwarza żądanie
        |
        v
Django przygotowuje stronę
        |
        v
Przeglądarka wyświetla wynik
```

---

# 6. Frontend, backend i baza danych

Naszą aplikację możemy uprościć do trzech głównych części:

```text
+----------------------+
|       FRONTEND       |
|      HTML + CSS      |
+----------+-----------+
           |
           v
+----------------------+
|       BACKEND        |
|    Python + Django   |
+----------+-----------+
           |
           v
+----------------------+
|     BAZA DANYCH      |
|        SQLite        |
+----------------------+
```

## Frontend

Frontend to część widoczna dla użytkownika.

Na zajęciach będą to przede wszystkim:

```text
HTML
CSS
```

Będziemy tworzyć m.in.:

- strony,
- formularze,
- listy danych,
- przyciski,
- menu,
- prosty układ interfejsu.

## Backend

Backend odpowiada za logikę działania aplikacji.

Będzie m.in.:

- odbierał żądania użytkownika,
- pobierał dane,
- zapisywał dane,
- sprawdzał formularze,
- wybierał odpowiednią stronę do wyświetlenia.

Backend wykonamy w:

```text
Python + Django
```

## Baza danych

Baza danych przechowuje informacje wykorzystywane przez aplikację.

W Bibliotece mogą być to np.:

```text
Autor
- imię
- nazwisko

Książka
- tytuł
- rok wydania
- autor
```

Podczas zajęć będziemy korzystać z:

```text
SQLite
```

---

# 7. Adres URL

Każda strona lub funkcja aplikacji może posiadać własny adres.

Przykładowo:

```text
/books/
```

może oznaczać listę książek.

```text
/books/5/
```

może oznaczać szczegóły książki numer 5.

```text
/books/add/
```

może prowadzić do formularza dodawania książki.

W lokalnej aplikacji Django pełny adres może wyglądać np.:

```text
http://127.0.0.1:8000/books/
```

Na kolejnych zajęciach sami skonfigurujemy takie adresy.

---

# 8. Request i response

Komunikację między klientem a serwerem możemy zapisać bardzo prosto:

```text
REQUEST  ->  RESPONSE
```

czyli:

```text
ŻĄDANIE  ->  ODPOWIEDŹ
```

Przykład:

```text
Przeglądarka:
Chcę zobaczyć /books/

        |
        v

Django:
Przygotowuję stronę z książkami.

        |
        v

Przeglądarka:
Wyświetlam otrzymaną stronę.
```

Ten prosty schemat będzie podstawą praktycznie całego dalszego kursu.

---

# 9. GET i POST

Na początku wystarczy rozróżniać dwie najczęściej spotykane metody.

## GET

`GET` wykorzystujemy przede wszystkim wtedy, gdy chcemy **pobrać lub wyświetlić dane**.

Przykład:

```text
GET /books/
```

możemy rozumieć jako:

> Pokaż listę książek.

## POST

`POST` wykorzystujemy najczęściej wtedy, gdy **wysyłamy dane do aplikacji**.

Przykładem będzie później formularz:

```text
Dodaj książkę
```

Po jego wysłaniu dane trafią do Django.

Na tym etapie nie potrzebujemy jeszcze dokładniejszej obsługi metod HTTP. Wrócimy do tego podczas pracy z formularzami i API.

---

# 10. Pierwsza praktyka — narzędzia deweloperskie

Każda współczesna przeglądarka posiada narzędzia przeznaczone dla osób tworzących strony i aplikacje webowe.

Najczęściej możemy je otworzyć klawiszem:

```text
F12
```

Interesują nas dzisiaj dwie zakładki:

```text
Elements / Inspector
Network / Sieć
```

## 10.1. Podejrzenie strony

1. Otwórz dowolną stronę internetową.
2. Naciśnij `F12`.
3. Otwórz zakładkę `Elements` lub `Inspector`.
4. Wybierz dowolny element strony.
5. Spróbuj odnaleźć odpowiadający mu fragment HTML.

Nie trzeba jeszcze rozumieć składni HTML.

Chodzi tylko o zauważenie zależności:

```text
to, co widzę w przeglądarce
        |
        v
struktura dokumentu HTML
```

Do HTML wrócimy na kolejnych zajęciach.

## 10.2. Podejrzenie żądania

1. Otwórz zakładkę `Network`.
2. Odśwież stronę.
3. Znajdź główne żądanie dotyczące otwartej strony.
4. Odszukaj:
   - adres żądania,
   - metodę żądania.

Najczęściej zobaczymy coś w rodzaju:

```text
Request URL: https://...
Request Method: GET
```

To dokładnie te elementy, które omówiliśmy wcześniej.

---

# 11. Jak będziemy rozwijać Bibliotekę?

Projekt będzie powstawał stopniowo.

W uproszczeniu:

```text
HTML
  |
  v
CSS
  |
  v
Django
  |
  +--> adresy URL
  |
  +--> widoki
  |
  +--> szablony
  |
  +--> modele
  |
  +--> baza danych
  |
  +--> formularze
  |
  +--> CRUD
  |
  +--> użytkownicy
  |
  +--> API
```

Każdy kolejny etap będzie rozwinięciem wcześniejszego.

Nie będziemy tworzyć kilkunastu niezależnych miniaplikacji.

Na końcu powstanie jeden spójny przykład.

---

# 12. Przykładowe tematy projektów zaliczeniowych

Na tym etapie nie trzeba jeszcze wybierać ostatecznego tematu projektu.

Warto jednak zacząć myśleć o prostej aplikacji, którą można rozwijać przez cały semestr.

Przykładowe tematy:

- planer nauki,
- planer zadań,
- katalog filmów,
- katalog gier,
- lista wydarzeń,
- prosty system rezerwacji,
- baza przepisów,
- dziennik aktywności,
- rejestr treningów,
- system zapisów na wydarzenie,
- prosty system ankiet,
- dziennik nawyków,
- katalog muzyki,
- planer podróży,
- baza zwierząt do adopcji.

Dobry pierwszy projekt powinien mieć:

- kilka rodzajów danych,
- możliwość ich wyświetlania,
- możliwość dodawania lub edytowania danych,
- prostą i jasno określoną funkcję.

Nie musi być rozbudowany.

> [!IMPORTANT]
> Lepiej przygotować **prostą aplikację, którą w pełni rozumiesz**, niż rozbudowany system składający się z kodu, którego nie potrafisz wyjaśnić.

---

# 13. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

Mają pomóc w utrwaleniu materiału i przygotowaniu do kolejnych zajęć.

## Zadanie 1 — znajdź HTML

Otwórz dowolną stronę internetową.

Za pomocą `F12` i zakładki `Elements`:

1. znajdź nagłówek,
2. znajdź link,
3. znajdź obraz lub przycisk,
4. sprawdź, jaki fragment HTML odpowiada za wybrany element.

Nie musisz jeszcze wiedzieć, co oznaczają wszystkie znaczniki.

## Zadanie 2 — znajdź żądanie GET

W zakładce `Network`:

1. odśwież stronę,
2. znajdź główne żądanie,
3. sprawdź jego adres,
4. sprawdź metodę HTTP.

Spróbuj znaleźć przynajmniej jedno żądanie wykorzystujące:

```text
GET
```

## Zadanie 3 — pomysł na aplikację

Wymyśl prostą aplikację webową.

Zapisz:

```text
Nazwa:

Jakie dane przechowuje?

Co użytkownik może zrobić?
```

Przykład:

```text
Nazwa:
Planer nauki

Dane:
- przedmioty
- zadania
- terminy

Funkcje:
- wyświetlenie zadań
- dodanie zadania
- edycja zadania
- usunięcie zadania
```

---

# 14. Co dalej?

Na kolejnych zajęciach przygotujemy środowisko pracy i podstawowy sposób organizacji kodu.

Następnie przejdziemy do:

```text
HTML
DOM
CSS
```

Dopiero później rozpoczniemy budowanie właściwej aplikacji Django.

Dzisiejsze najważniejsze zależności można sprowadzić do:

```text
użytkownik
    |
    v
przeglądarka
    |
 request
    |
    v
backend
    |
    v
baza danych
    |
    v
backend
    |
 response
    |
    v
przeglądarka
```

Nie trzeba jeszcze znać szczegółów implementacji.

Na tym etapie najważniejsze jest zrozumienie **z jakich elementów będzie składał się projekt, który będziemy wspólnie budować przez cały semestr**.