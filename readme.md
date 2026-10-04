# Projektowanie aplikacji webowych — semestr 2026Z

## 1. Cel przedmiotu

Celem przedmiotu jest poznanie podstaw projektowania i działania współczesnych aplikacji webowych oraz praktyczne wykorzystanie tych zagadnień podczas tworzenia własnej aplikacji.

Celem przedmiotu nie jest szczegółowe poznanie wszystkich możliwości wykorzystywanych technologii ani tworzenie zaawansowanych systemów informatycznych. Najważniejsze jest zrozumienie podstawowych mechanizmów działania aplikacji webowych oraz zdobycie praktycznego doświadczenia związanego z realizacją pierwszego większego projektu programistycznego.

W trakcie zajęć omówione zostaną zarówno elementy **frontendu**, czyli części aplikacji widocznej dla użytkownika, jak i **backendu**, odpowiedzialnego między innymi za przetwarzanie danych, realizację logiki aplikacji oraz komunikację z bazą danych.

Główną technologią wykorzystywaną podczas zajęć będzie język **Python** oraz framework **Django**. Do przygotowania interfejsu użytkownika wykorzystane zostaną **HTML i CSS**. Z racji skomplikowania na zajęciach nie zostanie szczegółowo omawiany **JavaScript (JS)**.

Celem zajęć jest przede wszystkim zrozumienie:

- jak zbudowana jest współczesna aplikacja webowa,
- jak komunikują się ze sobą klient i serwer,
- jaka jest rola frontendu i backendu,
- jak działa protokół HTTP,
- czym są adresy URL, żądania, odpowiedzi, nagłówki i kody statusu,
- jak tworzona jest struktura dokumentu HTML i czym jest DOM,
- jak CSS odpowiada za wygląd i układ strony,
- jak działa framework Django,
- jaka jest rola routingu, widoków i szablonów,
- jak aplikacja komunikuje się z bazą danych,
- czym są modele i ORM,
- jak obsługiwać formularze i walidować dane,
- na czym polegają podstawowe operacje CRUD,
- jak działają uwierzytelnianie, autoryzacja, sesje i podstawowe mechanizmy bezpieczeństwa,
- czym jest API REST, JSON i serializacja danych,
- jak korzystać z systemu kontroli wersji Git.

Efektem końcowym zajęć będzie przygotowanie **prostego, działającego projektu aplikacji webowej wykonanej z wykorzystaniem Django**.

---

## 2. Organizacja zajęć

Przedmiot obejmuje **18 spotkań po 90 minut**.

W jednym dniu realizowane są dwa kolejne spotkania.

Łącznie daje to **27 godzin zajęć**.

Ostatnie dwa spotkania, tj. **zajęcia nr 17 i 18**, przeznaczone są na prezentację i obronę projektów zaliczeniowych.

Projekt rozwijany jest stopniowo w trakcie semestru. Elementy poznawane na kolejnych zajęciach powinny być, w miarę możliwości, wykorzystywane również we własnym projekcie.

---

## 3. Oprogramowanie

Do pracy podczas zajęć potrzebne będą:

- interpreter **Python**,
- framework **Django**,
- środowisko programistyczne, np.:
  - Visual Studio Code,
  - PyCharm,
  - inne IDE obsługujące język Python,
- **Git**,
- konto w serwisie **GitHub**,
- współczesna przeglądarka internetowa z narzędziami deweloperskimi,
- baza **SQLite**, wykorzystywana domyślnie przez Django.

Na potrzeby części poświęconej API mogą zostać wykorzystane również narzędzia umożliwiające wykonywanie zapytań HTTP.

Instalowanie środowiska Node.js oraz frameworków JavaScript nie jest wymagane.

---

# 4. Plan zajęć

| Data | Spotkanie | Zakres |
|---|---:|---|
| 09.10.2026r. | **1** | **Wprowadzenie do aplikacji webowych** — klient i serwer, frontend i backend, baza danych, architektura aplikacji, HTTP, request/response, metody HTTP, sesje, REST, podstawowe zagadnienia bezpieczeństwa |
| 09.10.2026r. | **2** | **Środowisko pracy** — Python, IDE, terminal, struktura projektu, Git i GitHub, repozytorium, commit, push, pull, podstawowa organizacja pracy |
| 23.10.2026r. | **3** | **HTML** — struktura dokumentu, podstawowe znaczniki, semantyka HTML, tekst, listy, linki, obrazy, tabele |
| 23.10.2026r. | **4** | **HTML i DOM** — struktura drzewa dokumentu, formularze HTML, pola formularzy, GET i POST, narzędzia deweloperskie przeglądarki |
| 6.11.2026r. | **5** | **CSS** — selektory, klasy, identyfikatory, dziedziczenie, kolory, jednostki, box model, podstawowe stylowanie aplikacji |
| 6.11.2026r. | **6** | **Układ strony** — Flexbox, podstawy responsywności, organizacja interfejsu, ćwiczenia praktyczne, m.in. Flexbox Froggy. **Ustalenie tematów projektów** |
| 13.11.2026r. | **7** | **Django — podstawy** — utworzenie projektu i aplikacji, struktura Django, `manage.py`, uruchamianie serwera, routing i pierwszy widok |
| 13.11.2026r. | **8** | **Widoki i szablony Django** — URL → view → template, przekazywanie danych do szablonu, dziedziczenie szablonów, pliki statyczne, połączenie Django z HTML i CSS |
| 20.11.2026r. | **9** | **Modele i baza danych** — Django ORM, tworzenie modeli, podstawowe typy pól, migracje, SQLite, panel administracyjny Django |
| 20.11.2026r. | **10** | **ORM i relacje pomiędzy modelami** — pobieranie i filtrowanie danych, QuerySet, podstawowe relacje pomiędzy obiektami, wykorzystanie danych w widokach i szablonach |
| 4.12.2026r. | **11** | **Formularze Django** — `Form`, `ModelForm`, walidacja danych, obsługa GET i POST, komunikaty o błędach |
| 4.12.2026r. | **12** | **CRUD** — wyświetlanie, dodawanie, edytowanie i usuwanie danych, routing, widoki, formularze i szablony |
| 18.12.2026r. | **13** | **Użytkownicy i bezpieczeństwo** — logowanie, wylogowanie, sesje, uwierzytelnianie, autoryzacja, ograniczanie dostępu, CSRF oraz podstawowe zagrożenia aplikacji webowych |
| 18.12.2026r. | **14** | **REST API** — JSON, idea API, endpointy, serializacja, podstawowe zapytania do API i przykład prostego API w Django. API REST nie jest obowiązkowym elementem projektu zaliczeniowego |
| 22.01.2027r. | **15** | **Praca projektowa i konsultacje** |
| 22.01.2027r. | **16** | **Praca projektowa i konsultacje końcowe** —  **OSTATECZNY TERMIN ODDANIA PROJEKTU *DO KOŃCA TEGO DNIA*** |
| 29.01.2027r. | **17** | **Prezentacje i obrony projektów** |
| 29.01.2027r. | **18** | **Prezentacje i obrony projektów** |

---

# 5. Projekt zaliczeniowy

Podstawą zaliczenia przedmiotu jest projekt aplikacji webowej.

Projekt może być realizowany:

- **indywidualnie**, albo
- **w zespole dwuosobowym**.

W przypadku projektu dwuosobowego obie osoby powinny aktywnie uczestniczyć w jego realizacji.

Projekt ma być przede wszystkim **prostą, działającą aplikacją**, wykorzystującą zagadnienia poznane podczas zajęć. Nie jest wymagane tworzenie rozbudowanego lub zaawansowanego systemu.

Tematyka projektu jest dowolna, ale jego zakres musi umożliwiać praktyczne zastosowanie podstawowych mechanizmów Django.

Temat projektu powinien zostać przedstawiony i uzgodniony z prowadzącym **najpóźniej do końca zajęć nr 6 - 6.11.2026r.**.

Projekt powinien być rozwijany systematycznie i przechowywany w repozytorium Git.

Repozytorium musi być dostępne dla prowadzącego przez okres realizacji projektu.

## Minimalny zakres projektu

Projekt powinien zawierać:

1. działającą aplikację Django,
2. logiczną strukturę adresów URL,
3. własne widoki,
4. szablony HTML,
5. własne style CSS,
6. bazę danych obsługiwaną przez Django ORM,
7. co najmniej dwa modele odpowiadające elementom projektowanej aplikacji,
8. wykorzystanie przynajmniej jednej relacji pomiędzy modelami,
9. prezentowanie danych zapisanych w bazie,
10. formularze umożliwiające wprowadzanie lub modyfikowanie danych,
11. podstawowe operacje CRUD dla przynajmniej jednego elementu aplikacji,
12. podstawową walidację danych,
13. wykorzystanie mechanizmu logowania lub ograniczania dostępu do wybranej części aplikacji,
14. poprawną obsługę podstawowych sytuacji błędnych,
15. czytelny i uporządkowany kod,
16. historię pracy w repozytorium Git,
17. plik `README.md` opisujący projekt, sposób jego uruchomienia oraz najważniejsze funkcjonalności.

REST API **nie jest wymaganym elementem projektu zaliczeniowego**.

Może zostać wykorzystane jako element dodatkowy, jeżeli odpowiada charakterowi projektu i autorzy rozumieją jego działanie.

---

# 6. Podział pracy w projekcie

Do każdego projektu należy obowiązkowo dołączyć tabelę przedstawiającą wykonane zadania.

W przypadku projektu indywidualnego tabela pokazuje główne elementy wykonane przez autora.

W przypadku projektu dwuosobowego tabela musi jednoznacznie wskazywać, **która osoba odpowiadała za poszczególne elementy projektu**.

Tabela powinna znajdować się w pliku `README.md` projektu.

Przykład:

| Osoba | Element projektu | Zakres wykonanych prac |
|---|---|---|
| Jan Kowalski | Modele | Utworzenie modeli `Book` i `Author`, relacja pomiędzy modelami, migracje |
| Jan Kowalski | CRUD | Widoki dodawania i edycji książek |
| Anna Nowak | Frontend | Szablony HTML, CSS, menu aplikacji |
| Anna Nowak | Użytkownicy | Logowanie, wylogowanie, ograniczenie dostępu |
| Wspólnie | Testowanie | Testowanie działania aplikacji i poprawianie błędów |

Tabela nie musi odwzorowywać każdej pojedynczej linii kodu.

Powinna jednak umożliwiać ocenę, czy obie osoby miały **rzeczywisty i istotny wkład w projekt**.

W projekcie dwuosobowym niedopuszczalna jest sytuacja, w której jedna osoba wykonuje praktycznie cały projekt, a druga uczestniczy jedynie symbolicznie.

---

# 7. Termin oddania projektu

Ostateczny termin oddania projektu przypada **do końca dnia, w którym realizowane są zajęcia nr 16, czyli 22.01.2027r.**.

Za wersję oddaną uznawany jest stan projektu znajdujący się w repozytorium w momencie upływu terminu i załączony zarówno jako link jak i w archiwum `.zip` do wyznaczonego zadania w Classroom.

**Terminowe oddanie kompletnego projektu jest warunkiem dopuszczenia do obrony.**

Projekt oddany po terminie bez wcześniejszego uzgodnienia z prowadzącym nie daje prawa do przystąpienia do obrony w podstawowym terminie.

Po upływie terminu można poprawiać wyłącznie błędy wskazane lub zaakceptowane przez prowadzącego. Nie należy po terminie znacząco rozbudowywać projektu przed obroną.

---

# 8. Obrona projektu

Obrona projektu odbywa się podczas zajęć nr **17 i 18**, czyli **29.01.2027r.**.

Na obronę projektu przewidziane jest około **10–15 minut**.

W przypadku projektu dwuosobowego obrona odbywa się wspólnie, jednak **każdy członek zespołu jest oceniany indywidualnie**.

Obrona składa się z:

1. krótkiego przedstawienia założeń projektu,
2. demonstracji działania aplikacji,
3. przedstawienia najważniejszych elementów kodu,
4. omówienia podziału prac,
5. pytań prowadzącego dotyczących zastosowanych rozwiązań,
6. pytań kontrolnych dotyczących zagadnień realizowanych podczas zajęć,
7. w razie potrzeby wykonania niewielkiej modyfikacji kodu, wyjaśnienia błędu lub wskazania sposobu rozwiązania prostego problemu.

Każda osoba powinna znać **cały projekt**, również te elementy, których głównym autorem był drugi członek zespołu.

Nie jest wymagane pamiętanie kodu na pamięć.

Student powinien jednak rozumieć strukturę aplikacji i wiedzieć:

- jak uruchamiana jest aplikacja,
- jak zorganizowany jest projekt Django,
- jak działa routing,
- który widok odpowiada za określoną funkcjonalność,
- w jaki sposób dane trafiają do szablonu,
- jak działają modele,
- jakie relacje istnieją pomiędzy modelami,
- w jaki sposób aplikacja komunikuje się z bazą danych,
- jak działają formularze,
- gdzie znajduje się walidacja,
- jak działa logowanie i ograniczanie dostępu,
- gdzie realizowane są najważniejsze funkcjonalności projektu.

Obrona odbywa się **bez korzystania z narzędzi AI**.

---

# 9. Korzystanie ze sztucznej inteligencji

Podczas realizacji projektu **dozwolone jest korzystanie z narzędzi sztucznej inteligencji**, takich jak modele językowe i asystenci programistyczni.

AI powinno pełnić rolę **narzędzia wspomagającego naukę i pracę**, a nie autora projektu.

Dopuszczalne jest między innymi:

- proszenie o wyjaśnienie niezrozumiałego zagadnienia,
- pomoc w znalezieniu błędu,
- wyjaśnienie komunikatu błędu,
- konsultowanie możliwych sposobów rozwiązania problemu,
- generowanie niewielkich fragmentów pomocniczego kodu, jeżeli autor potrafi je następnie wyjaśnić, zweryfikować i dostosować,
- pomoc przy tworzeniu dokumentacji.

Niedopuszczalne jest między innymi:

- wygenerowanie przez AI całego projektu lub jego zasadniczej części,
- generowanie kolejnych modułów aplikacji bez rozumienia powstającego kodu,
- kopiowanie wygenerowanego kodu bez jego analizy,
- zlecenie AI realizacji zadania zaliczeniowego od początku do końca,
- tzw. **vibecoding**, czyli budowanie aplikacji przede wszystkim poprzez wydawanie kolejnych poleceń AI bez samodzielnego rozumienia i tworzenia rozwiązania.

Przyjmuje się, że **minimum 70% pracy nad projektem powinno zostać wykonane bezpośrednio przez jego autora lub autorów**.

Nie jest to rozumiane jako matematyczny procent liczby linii kodu.

Oceniany jest całokształt procesu tworzenia projektu, historia repozytorium, podejmowane decyzje, sposób pracy oraz znajomość przygotowanego rozwiązania.

W pliku `README.md` projektu należy umieścić krótką sekcję:

## Wykorzystanie AI

Powinna ona zawierać informację:

- czy podczas realizacji projektu korzystano z AI,
- z jakich narzędzi korzystano,
- do jakich zadań zostały wykorzystane.

Samo korzystanie z AI **nie obniża oceny**, jeżeli odbywa się zgodnie z powyższymi zasadami.

Autorzy projektu odpowiadają jednak za **całość oddanego kodu**, niezależnie od tego, czy dany fragment został napisany całkowicie samodzielnie, z pomocą dokumentacji, Internetu czy narzędzia AI.

---

# 10. Samodzielność pracy

Projekt może być wykonany samodzielnie lub w parze, jednak niedopuszczalne jest korzystanie z kodu przygotowanego przez osoby spoza zespołu.

Każdy członek zespołu musi rozumieć **cały oddawany projekt**.

W szczególności powinien być w stanie:

- wskazać miejsce implementacji konkretnej funkcjonalności,
- wyjaśnić działanie najważniejszych fragmentów kodu,
- wyjaśnić podstawowy przepływ danych przez aplikację,
- odpowiedzieć na pytania dotyczące modeli, widoków, formularzy i szablonów,
- wyjaśnić swój własny wkład w projekt,
- samodzielnie dokonać niewielkiej modyfikacji projektu.

Historia repozytorium Git oraz tabela podziału pracy mogą być wykorzystywane przy ocenie przebiegu pracy nad projektem.

Pojedynczy commit zawierający praktycznie cały projekt, brak wcześniejszej historii pracy albo nagłe pojawienie się dużych fragmentów kodu może być przedmiotem dodatkowej weryfikacji podczas obrony.

**Brak znajomości własnego projektu, niemożność wyjaśnienia jego podstawowych elementów albo stwierdzenie, że zasadnicza część projektu została wykonana przez inną osobę lub wygenerowana przez AI bez świadomego udziału autora, stanowi podstawę do niezaliczenia projektu.**

W przypadku projektu dwuosobowego ocena poszczególnych osób może się różnić, jeżeli podczas obrony lub na podstawie historii pracy zostaną wykazane istotne różnice w wiedzy, samodzielności lub zaangażowaniu.

---

# 11. Punktacja

Za przedmiot można uzyskać maksymalnie **50 punktów**.

## Projekt — maksymalnie 40 pkt

| Element | Punkty |
|---|---:|
| Działanie aplikacji i realizacja założonych funkcjonalności | **10 pkt** |
| Architektura Django: routing, widoki, szablony i organizacja projektu | **6 pkt** |
| Modele, relacje, baza danych i wykorzystanie ORM | **6 pkt** |
| Formularze, CRUD i walidacja danych | **6 pkt** |
| Frontend: HTML, CSS, czytelność i użyteczność interfejsu | **4 pkt** |
| Użytkownicy, ograniczanie dostępu i podstawowe mechanizmy bezpieczeństwa | **3 pkt** |
| Git, systematyczność pracy, jakość kodu i dokumentacja | **3 pkt** |
| Opis wkładu autora/autorów i poprawnie przygotowana tabela podziału pracy | **2 pkt** |
| **Razem** | **40 pkt** |

## Obrona — maksymalnie 10 pkt

| Element | Punkty |
|---|---:|
| Prezentacja projektu i demonstracja działania | **2 pkt** |
| Znajomość własnego projektu i umiejętność wyjaśnienia zastosowanych rozwiązań | **4 pkt** |
| Odpowiedzi na pytania kontrolne dotyczące projektu i materiału z zajęć | **3 pkt** |
| Umiejętność samodzielnej analizy prostego problemu lub niewielkiej modyfikacji | **1 pkt** |
| **Razem** | **10 pkt** |

---

# 12. Warunki konieczne zaliczenia

Do zaliczenia przedmiotu konieczne jest jednoczesne:

1. terminowe oddanie projektu,
2. umieszczenie w projekcie wymaganej dokumentacji i tabeli podziału pracy,
3. przystąpienie do obrony,
4. uzyskanie minimum **21 z 40 punktów za projekt**,
5. uzyskanie minimum **5 z 10 punktów za obronę**,
6. uzyskanie minimum **26 z 50 punktów łącznie**,
7. potwierdzenie podczas obrony znajomości projektu i własnego udziału w jego realizacji.

Spełnienie wyłącznie progu punktowego nie gwarantuje zaliczenia w przypadku stwierdzenia niesamodzielności pracy.

W szczególności **działająca i dobrze wyglądająca aplikacja, której autor nie potrafi wyjaśnić, nie może stanowić podstawy zaliczenia przedmiotu**.

---

# 13. Oceny

| Punkty | Ocena |
|---:|:---|
| 0–25 pkt | **2,0** |
| 26–30 pkt | **3,0** |
| 31–35 pkt | **3,5** |
| 36–40 pkt | **4,0** |
| 41–45 pkt | **4,5** |
| 46–50 pkt | **5,0** |

Warunki minimalne dotyczące projektu, obrony i samodzielności obowiązują niezależnie od sumy zdobytych punktów.

---

# 14. Obecność

Na zajęciach sprawdzana jest obecność.

Dopuszczalne są maksymalnie **3 nieobecności nieusprawiedliwione**.

Nieobecność na zajęciach nie zwalnia z obowiązku samodzielnego uzupełnienia realizowanego materiału oraz wykonania prac związanych z projektem.

Ze względu na warsztatowy charakter przedmiotu zalecana jest systematyczna obecność oraz bieżące rozwijanie projektu.

---

# 15. Ważne informacje

> **Projekt oraz jego obrona stanowią podstawę zaliczenia przedmiotu.**
>
> Projekt ma potwierdzać przede wszystkim zrozumienie podstawowych mechanizmów działania aplikacji webowej oraz umiejętność wykorzystania ich w praktyce.
>
> Projekt nie musi być skomplikowany. Znacznie ważniejsze jest, aby był działający, uporządkowany i zrozumiały dla jego autorów.
>
> Można korzystać z dokumentacji, materiałów z zajęć, Internetu oraz narzędzi AI, ale **nie można oddać jako własnej pracy rozwiązania, którego działania się nie rozumie**.
>
> Każdy autor odpowiada za całość kodu znajdującego się w oddanym projekcie.
>
> W przypadku stwierdzenia niesamodzielności pracy projekt może zostać oceniony na **0 punktów**, niezależnie od jego technicznej jakości.

## Kontakt do prowadzącego

- Adrian Albrecht — aalbrecht@swps.edu.pl