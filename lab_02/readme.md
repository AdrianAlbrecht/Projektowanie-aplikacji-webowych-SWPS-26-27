# Projektowanie aplikacji webowych — semestr 2026Z

## Lab 02 — Środowisko pracy, Git i GitHub

---

## 1. Cel zajęć

Na tych zajęciach przygotujemy środowisko, z którego będziemy korzystać podczas dalszej części przedmiotu.

Nie będziemy poznawać wszystkich możliwości Gita. Potrzebujemy tylko zestawu narzędzi pozwalającego:

- utworzyć projekt,
- pracować nad nim na swoim komputerze,
- zapisywać kolejne wersje,
- przesłać projekt do GitHuba,
- pobrać projekt ponownie na innym komputerze,
- odzyskać zmiany przed kolejnymi zajęciami.

Po zakończeniu laboratorium każdy powinien posiadać własne repozytorium z kopią projektu ćwiczeniowego **Biblioteka**.

> [!CAUTION]
> **Biblioteka, katalog książek oraz system wypożyczania książek są tematami niedozwolonymi w projekcie zaliczeniowym.**
>
> Biblioteka jest wspólnym projektem ćwiczeniowym rozwijanym podczas laboratoriów.

---

# 2. Narzędzia używane podczas zajęć

Podczas kursu będziemy korzystać przede wszystkim z:

```text
Python
IDE / edytor kodu
Git
GitHub
przeglądarka internetowa
```

W dalszej części semestru dojdą:

```text
HTML
CSS
Django
SQLite
```

Na dzisiejszych zajęciach przygotujemy podstawę pod dalszą pracę.

---

# 3. Sprawdzenie Pythona

Otwórz terminal.

Może to być np.:

```text
Terminal w Visual Studio Code
PowerShell
Wiersz polecenia
Git Bash
terminal w PyCharm
```

Sprawdź, czy Python jest dostępny:

```bash
python --version
```

Jeżeli polecenie `python` nie działa na Windows, spróbuj:

```bash
py --version
```

Powinien pojawić się numer zainstalowanej wersji Pythona, np.:

```text
Python 3.13.24
```

Sprawdź również dostępność `pip`:

```bash
python -m pip --version
```

lub:

```bash
py -m pip --version
```

`pip` jest narzędziem służącym do instalowania bibliotek Pythona.

Później użyjemy go między innymi do instalacji Django.

---

# 4. Tworzymy katalog projektu

Utwórz folder:

```text
paw_biblioteka
```

Możesz zrobić to z poziomu eksploratora plików lub terminala.

Przykład:

```bash
mkdir paw_biblioteka
cd paw_biblioteka
```

Otwórz ten katalog w wybranym IDE.

Od tego momentu traktujemy katalog:

```text
paw_biblioteka
```

jako główny katalog naszego projektu ćwiczeniowego.

Na razie będzie prawie pusty.

W kolejnych laboratoriach będziemy stopniowo dodawali do niego następne elementy.

---

# 5. Środowisko wirtualne Pythona

Podczas pracy z większym projektem nie chcemy instalować wszystkich bibliotek globalnie dla całego systemu.

Dlatego dla projektu tworzymy **środowisko wirtualne**.

Możemy potraktować je jako osobne środowisko Pythona przeznaczone tylko dla jednego projektu.

W katalogu:

```text
paw_biblioteka
```

wykonaj:

```bash
python -m venv .venv
```

lub na Windows:

```bash
py -m venv .venv
```

Po wykonaniu polecenia pojawi się katalog:

```text
.venv
```

Nie modyfikujemy ręcznie jego zawartości.

---

# 6. Aktywacja środowiska

## Windows — PowerShell

```powershell
.venv\Scripts\Activate.ps1
```

## Windows — CMD

```cmd
.venv\Scripts\activate.bat
```

## Linux / macOS

```bash
source .venv/bin/activate
```

Po aktywacji na początku wiersza terminala zwykle pojawi się:

```text
(.venv)
```

Przykład:

```text
(.venv) C:\projekty\paw_biblioteka>
```

Sprawdź ponownie:

```bash
python --version
```

oraz:

```bash
python -m pip --version
```

> [!NOTE]
> Jeżeli aktywacja w PowerShell jest blokowana przez ustawienia komputera w sali, można skorzystać z terminala `cmd` i polecenia:
>
> ```cmd
> .venv\Scripts\activate.bat
> ```

---

# 7. Pierwszy plik projektu

Na razie nie tworzymy Django.

Dodamy jedynie prosty plik pozwalający sprawdzić, z jakiego Pythona korzystamy.

Utwórz plik:

```text
check_environment.py
```

i wklej:

```python
import sys

print("Środowisko działa.")
print("Wersja Pythona:")
print(sys.version)

print("\nInterpreter:")
print(sys.executable)
```

Uruchom:

```bash
python check_environment.py
```

Powinniśmy zobaczyć m.in.:

```text
Środowisko działa.
Wersja Pythona:
...

Interpreter:
...\paw_biblioteka\.venv\...
```

Jeżeli ścieżka interpretera prowadzi do katalogu `.venv`, pracujemy w utworzonym środowisku wirtualnym.

---

# 8. Plik README.md projektu

Utwórz w głównym katalogu plik:

```text
README.md
```

Wklej:

```markdown
# Biblioteka

Projekt ćwiczeniowy rozwijany podczas laboratoriów z przedmiotu
**Projektowanie aplikacji webowych**.

## Cel

Celem projektu jest stopniowe poznanie podstaw:

- HTML,
- CSS,
- Django,
- baz danych,
- formularzy,
- operacji CRUD,
- uwierzytelniania użytkowników,
- podstaw API REST.

> Projekt „Biblioteka” jest projektem dydaktycznym i nie może zostać
> wykorzystany jako temat projektu zaliczeniowego.
```

Plik `README.md` będzie opisem projektu widocznym również na GitHubie.

---

# 9. Plik .gitignore

Środowiska `.venv` **nie wysyłamy do repozytorium**.

Każdy użytkownik może utworzyć je ponownie na swoim komputerze.

W głównym katalogu projektu utwórz plik:

```text
.gitignore
```

i wklej:

```gitignore
# Środowisko wirtualne
.venv/

# Python
__pycache__/
*.pyc

# IDE
.idea/
.vscode/

# Pliki systemowe
.DS_Store
Thumbs.db

# Dane lokalne / poufne
.env
```

Plik `.gitignore` informuje Git, których plików i katalogów nie powinien śledzić.

Dzięki temu do GitHuba nie trafi np. całe środowisko `.venv`.

---

# 10. Struktura projektu po tym etapie

Powinniśmy mieć:

```text
paw_biblioteka/
│
├── .venv/
├── .gitignore
├── README.md
└── check_environment.py
```

Katalog `.venv` znajduje się na komputerze, ale nie będzie dodawany do repozytorium.

---

# 11. Sprawdzenie Gita

W terminalu wykonaj:

```bash
git --version
```

Powinien pojawić się numer wersji, np.:

```text
git version ...
```

Jeżeli polecenie nie działa, Git nie jest zainstalowany lub nie został dodany do zmiennej `PATH`.

---

# 12. Git — tylko to, czego naprawdę potrzebujemy

Git służy do śledzenia zmian w projekcie.

*Dokumentacja?* [git-scm.com](https://git-scm.com/book/pl/v2).

Podczas zajęć najczęściej będziemy wykonywali następujący cykl:

```text
zmieniam pliki
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

Na początku kolejnych zajęć lub po zmianie komputera:

```text
git pull
```

To jest najważniejszy schemat pracy, który należy zapamiętać.

---

# 13. Tworzymy repozytorium Git

Upewnij się, że terminal znajduje się w katalogu:

```text
paw_biblioteka
```

Wykonaj:

```bash
git init
```

Następnie ustaw nazwę głównej gałęzi:

```bash
git branch -M main
```

Git utworzy ukryty katalog:

```text
.git
```

To właśnie on sprawia, że zwykły katalog staje się repozytorium Git.

Nie modyfikujemy ręcznie jego zawartości.

---

# 14. Konfiguracja autora commitów

Git zapisuje informację o autorze każdej zmiany.

Na komputerach prywatnych można użyć konfiguracji globalnej.

Na komputerach współdzielonych w sali bezpieczniej jest skonfigurować dane **tylko dla bieżącego repozytorium**.

Wykonaj:

```bash
git config --local user.name "Imię Nazwisko"
git config --local user.email "twoj@email.pl"
```

Przykład:

```bash
git config --local user.name "Jan Kowalski"
git config --local user.email "jan.kowalski@example.com"
```

Sprawdź konfigurację:

```bash
git config --local --list
```

Powinny pojawić się m.in.:

```text
user.name=Jan Kowalski
user.email=jan.kowalski@example.com
```

---

# 15. Sprawdzamy stan repozytorium

Wykonaj:

```bash
git status
```

Git powinien pokazać pliki, które nie są jeszcze śledzone.

Powinniśmy zobaczyć m.in.:

```text
.gitignore
README.md
check_environment.py
```

Nie powinniśmy natomiast widzieć całej zawartości:

```text
.venv/
```

ponieważ została dodana do `.gitignore`.

---

# 16. Pierwszy commit

Dodaj wszystkie pliki:

```bash
git add .
```

Ponownie sprawdź:

```bash
git status
```

Pliki powinny być przygotowane do zapisania w historii.

Tworzymy pierwszy commit:

```bash
git commit -m "Utworzenie projektu i środowiska pracy"
```

Commit można potraktować jako **zapis konkretnego stanu projektu**.

Warto tworzyć commit po zakończeniu konkretnego fragmentu pracy.

---

# 17. Historia zmian

Sprawdź historię:

```bash
git log --oneline
```

Powinien pojawić się pierwszy commit, np.:

```text
a12bc34 Utworzenie projektu i środowiska pracy
```

Nie trzeba zapamiętywać identyfikatora po lewej stronie.

Istotne jest to, że Git przechowuje historię kolejnych wersji projektu.

---

# 18. Tworzymy repozytorium na GitHubie

Zaloguj się do:

```text
https://github.com
```

Utwórz nowe repozytorium.

Nazwa:

```text
paw_biblioteka
```

Repozytorium może być:

```text
Private
```

lub:

```text
Public
```

zgodnie z naszymi potrzebami. Utwórzmy **publiczne repozytorium**.

Podczas tworzenia **nie dodawaj** automatycznie:

- `README`,
- `.gitignore`,
- licencji.

Mamy już te pliki (oprócz licencji) lokalnie.

Po utworzeniu repozytorium GitHub wyświetli jego adres, np.:

```text
https://github.com/uzytkownik/paw_biblioteka.git
```

---

# 19. Łączymy lokalny projekt z GitHubem

Skopiuj adres własnego repozytorium i wykonaj:

```bash
git remote add origin ADRES_REPOZYTORIUM
```

Przykład:

```bash
git remote add origin https://github.com/uzytkownik/paw_biblioteka.git
```

Sprawdź:

```bash
git remote -v
```

Powinniśmy zobaczyć adres repozytorium `origin`.

---

# 20. Pierwszy push

Wyślij projekt do GitHuba:

```bash
git push -u origin main
```

Przy pierwszym połączeniu GitHub może poprosić o zalogowanie w przeglądarce lub inną formę autoryzacji.

Po poprawnym wykonaniu polecenia odśwież stronę repozytorium na GitHubie.

Powinny być widoczne:

```text
.gitignore
README.md
check_environment.py
```

Nie powinien być widoczny katalog:

```text
.venv
```

---

# 21. Codzienny schemat pracy

Podczas dalszych laboratoriów będziemy bardzo często wykonywali:

```bash
git status
git add .
git commit -m "Krótki opis zmian"
git push
```

Przykład po wykonaniu strony HTML:

```bash
git add .
git commit -m "Dodanie pierwszej strony HTML"
git push
```

Przykład po dodaniu modeli Django:

```bash
git add .
git commit -m "Dodanie modeli książki i autora"
git push
```

Opis commitu powinien krótko informować:

> **Co zostało wykonane?**

Zamiast:

```text
zmiany
```

lepiej:

```text
Dodanie formularza książki
```

---

# 22. Po co nam GitHub podczas tych zajęć?

GitHub nie służy wyłącznie do oddania projektu.

Ma rozwiązywać praktyczny problem:

> **Co zrobić, jeśli na kolejnych zajęciach siedzę przy innym komputerze?**

Jeżeli poprzednio wykonaliśmy:

```bash
git push
```

projekt znajduje się w repozytorium zdalnym i możemy pobrać go ponownie.

Sprawdzimy to teraz w praktyce.

---

# 23. Praktyka — pobieramy projekt od zera

Najpierw upewnij się, że wszystkie zmiany zostały wysłane:

```bash
git status
```

oraz:

```bash
git push
```

Wyjdź w terminalu katalog wyżej:

```bash
cd ..
```

Utwórz katalog testowy:

```bash
mkdir test_clone
cd test_clone
```

Skopiuj adres własnego repozytorium GitHub.

Wykonaj:

```bash
git clone ADRES_REPOZYTORIUM
```

Przykład:

```bash
git clone https://github.com/uzytkownik/paw_biblioteka.git
```

Przejdź do pobranego projektu:

```bash
cd paw_biblioteka
```

Sprawdź zawartość.

Powinny znaleźć się tam:

```text
.gitignore
README.md
check_environment.py
```

Nie będzie natomiast:

```text
.venv/
```

To poprawne zachowanie.

Środowisko wirtualne na nowym komputerze tworzymy ponownie:

```bash
python -m venv .venv
```

lub:

```bash
py -m venv .venv
```

To bardzo ważny fragment laboratorium.

> Repozytorium powinno zawierać **kod i pliki potrzebne do odtworzenia projektu**, a nie całe środowisko konkretnego komputera.

Po sprawdzeniu możesz usunąć katalog `test_clone`.

---

# 24. git pull — pobieranie nowych zmian

Jeżeli repozytorium już istnieje na komputerze, nie wykonujemy kolejnego `git clone`.

Wchodzimy do istniejącego projektu:

```bash
cd paw_biblioteka
```

i pobieramy nowe zmiany:

```bash
git pull
```

Najprostsza zasada na dalsze zajęcia:

### Początek pracy

```bash
git pull
```

### Koniec pracy

```bash
git add .
git commit -m "Opis wykonanych zmian"
git push
```

---

# 25. Małe ćwiczenie — pełny cykl pracy

Otwórz `README.md`.

Na końcu dopisz:

```markdown
## Autor

Imię Nazwisko
```

Zapisz plik.

Sprawdź:

```bash
git status
```

Następnie:

```bash
git add README.md
git commit -m "Dodanie autora projektu"
git push
```

Odśwież GitHub.

Zmiana powinna być widoczna w repozytorium.

---

# 26. Najważniejsze polecenia

Nie trzeba znać całego Gita.

Na tych zajęciach wystarczy następujący zestaw:

| Polecenie | Zastosowanie |
|---|---|
| `git clone URL` | pobranie projektu po raz pierwszy |
| `git status` | sprawdzenie zmian |
| `git pull` | pobranie nowych zmian |
| `git add .` | przygotowanie zmian do commitu |
| `git commit -m "opis"` | zapisanie wersji projektu |
| `git push` | wysłanie zmian do GitHuba |
| `git log --oneline` | wyświetlenie historii |

Dodatkowo podczas tworzenia nowego repozytorium użyliśmy:

```bash
git init
git branch -M main
git remote add origin URL
```

Nie będziemy wykonywali tych trzech poleceń przy każdym kolejnym laboratorium.

---

# 27. Czego na razie NIE potrzebujemy?

Git posiada znacznie więcej możliwości.

Na tym przedmiocie nie potrzebujemy obecnie szczegółowo omawiać:

```text
branchy
merge
rebase
tagi
cherry-pick
reset
stash
```

Jeżeli w dalszej pracy pojawi się potrzeba użycia któregoś z tych mechanizmów, wrócimy do niego wtedy, gdy będzie miał praktyczne zastosowanie.

Na razie ważniejsze jest poprawne opanowanie prostego cyklu:

```text
pull
pracuję
status
add
commit
push
```

---

# 28. Co powinno być gotowe po laboratorium?

Na GitHubie powinno istnieć repozytorium:

```text
paw_biblioteka
```

o strukturze:

```text
paw_biblioteka/
│
├── .gitignore
├── README.md
└── check_environment.py
```

Lokalnie dodatkowo:

```text
.venv/
```

> [!CAUTION]
> Jeżeli kopiujesz repozytorium na nowe urządzenie (inny komputer, laptop prywatny, komputer stacjonarny w domu, itd.) nie będzie on zawierał środowiska wirtualnego `.venv`! Pamiętajmy, żeby przy każdym kopiowaniu na NOWE urządzenie stworzyć srodowisko wirtualne i doinstalować menadżerem pakietów `pip` odpowiednie moduły potrzebne do pracy, np. `django`.

> [!CAUTION]
> Przy każdym pobraniu nowego kodu z repozytorium nie instalują się automatycznie biblioteki. Pamiętaj o ręcznym dosintalowaniu modułów do python! :)

Historia repozytorium powinna zawierać przynajmniej dwa commity, np.:

```text
Dodanie autora projektu
Utworzenie projektu i środowiska pracy
```

---

# 29. Zadania do samodzielnego przećwiczenia

Zadania są **nieobowiązkowe**.

## Zadanie 1 — kolejny commit

Utwórz plik:

```text
notes.md
```

i wpisz:

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

Dodaj go do repozytorium:

```bash
git add .
git commit -m "Dodanie notatek z laboratoriów"
git push
```

---

## Zadanie 2 — odtworzenie projektu

W innym katalogu wykonaj:

```bash
git clone ADRES_REPOZYTORIUM
```

Następnie:

1. utwórz `.venv`,
2. aktywuj środowisko,
3. uruchom `check_environment.py`.

Celem jest sprawdzenie, czy potrafisz odtworzyć środowisko projektu bez korzystania ze starego katalogu.

---

## Zadanie 3 — dobra historia commitów

Wprowadź do `README.md` dwie niewielkie zmiany.

Każdą zmianę zapisz jako **osobny commit** z opisem mówiącym, co zostało wykonane.

Na końcu sprawdź:

```bash
git log --oneline
```

Przykładowy efekt:

```text
c8d91ab Dodanie planowanych funkcji aplikacji
41ca220 Rozszerzenie opisu projektu
...
```

---

> [!NOTE]
> ## Zadanie dodatkowe
> Jeżeli nadal masz problemy z gitem, zerknij tutaj: https://learngitbranching.js.org/?locale=pl

---

# 30. Co dalej?

Na kolejnych zajęciach zaczniemy tworzyć pierwszy rzeczywisty element interfejsu Biblioteki.

Przejdziemy do:

```text
HTML
```

i przygotujemy pierwszą statyczną stronę projektu.

Będzie to pierwszy moment, w którym repozytorium zacznie zawierać kod bezpośrednio związany z wyglądem naszej aplikacji.

Od tego momentu po każdych zajęciach warto wykonać:

```bash
git add .
git commit -m "Opis zmian"
git push
```

Dzięki temu na końcu semestru repozytorium będzie zawierało nie tylko gotową aplikację, ale również historię jej stopniowego powstawania.