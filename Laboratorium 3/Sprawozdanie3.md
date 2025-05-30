# Sprawozdanie z Laboratorium 3 – 417298

## Cel laboratorium

Celem laboratorium było zapoznanie się z procesem code review, podstawowymi plikami budującymi repozytorium (np. `.gitignore`, `.gitkeep`) oraz przywracaniem commitów za pomocą `git revert`. W ramach zadania wykonano szereg operacji na repozytorium Git, w tym aktualizację, tworzenie branchy, edycję kodu, code review, poprawki oraz przygotowanie sprawozdania.

---

## Krok 1: Aktualizacja repozytorium

W pierwszym kroku zaktualizowano repozytorium, aby upewnić się, że lokalna kopia jest zgodna z najnowszymi zmianami na zdalnym repozytorium.

### Wykonane polecenia:

```bash
git fetch --all
git checkout main
git pull
```

### Opis:

- `git fetch --all` pobrało wszystkie metadane z zdalnego repozytorium.
- `git checkout main` przełączyło na branch `main`.
- `git pull` pobrało najnowsze zmiany z brancha `main`.

### Zrzuty ekranu:
![ ](2.png)

![ ](3.png)
---

## Krok 2: Tworzenie nowego brancha

W tym kroku utworzono nowy branch o nazwie `lab_3/new_branch_417298` i wypchnięto go na zdalne repozytorium.

### Wykonane polecenia:

```bash
git switch -c lab_3/new_branch_417298
git push --set-upstream origin lab_3/new_branch_417298
```

### Opis:

- `git switch -c` utworzyło nowy branch i przełączyło na niego.
- `git push --set-upstream` wypchnęło branch na zdalne repozytorium i ustawiło śledzenie.

### Zrzut ekranu:
![ ](4.png)
---

## Krok 3: Edycja brancha

W tym kroku utworzono folder `model_417298` w katalogu `Lab_3`, dodano plik `417298.csv`, a następnie wykonano commit i wypchnięto zmiany.

### Wykonane polecenia:

```bash
mkdir Lab_3/model_417298
touch Lab_3/model_417298/417298.csv
echo "przyklad,danych,csv" > Lab_3/model_417298/417298.csv
git checkout lab_3/new_branch_417298
git add *
git commit -m "dodano plik csv do nowego folderu z modelem"
git push
```

### Opis:

- Utworzono folder i plik CSV, który zawierał przykładowe dane.
- Zmiany zostały dodane do indeksu Git (`git add *`), zapisane w commicie i wypchnięte na zdalne repozytorium.

### Zrzuty ekranu:
![ ](5.png)

![ ](6.png)
---

## Krok 4: Code review

W tym kroku utworzono Pull Request (PR) do brancha `TEST`, wykonano code review i dodano komentarze.

### Wykonane kroki:

1. Utworzono PR na GitHubie (base: `TEST`, compare: `lab_3/new_branch_417298`).
2. W trybie review:
   - W pliku `app.py` w sekcji importów dodano komentarz: "Brak importu dla nowo powstałego folderu model_417298."
   - Dla pliku `417298.csv` dodano ogólny komentarz: "Plik csv nie powinien znajdować się w repozytorium. Należy dodać go do .gitignore."
3. Zakończono review z podsumowaniem: "Proszę usunąć plik csv i dodać go do .gitignore." (bez akceptacji PR).

### Zrzuty ekranu:
![ ](7.png)

![ ](8.png)

![ ](9.png)

![ ](10.png)
---

## Krok 5: Poprawka kodu

W tym kroku usunięto plik CSV, dodano regułę do `.gitignore`, utworzono nowy plik CSV (który nie będzie śledzony) oraz dodano `.gitkeep`.

### Wykonane polecenia:

```bash
git rm Lab_3/model_417298/417298.csv
echo "*.csv" >> .gitignore
mkdir Lab_3/model_417298
touch Lab_3/model_417298/nowy_417298.csv
touch Lab_3/model_417298/.gitkeep
git add *
git commit -m "usunięto plik csv, dodano .gitignore i .gitkeep"
git push
```

### Opis:

- Usunięto plik `417298.csv` za pomocą `git rm`.
- Dodano regułę `*.csv` do `.gitignore`, aby Git ignorował pliki CSV.
- Utworzono nowy plik `nowy_417298.csv`, który nie został dodany do commita (z powodu `.gitignore`).
- Dodano `.gitkeep`, aby folder `model_417298` był śledzony przez Git.
- Wykonano commit i wypchnięto zmiany.

### Zrzuty ekranu:
![ ](11.png)

![ ](12.png)

![ ](13.png)
---

## Krok 6: Naprawa repo

W tym kroku dodano plik `test_plik.txt`, edytowano go, a następnie cofnięto zmiany za pomocą `git revert`.

### Wykonane polecenia:

```bash
echo "Przykładowy tekst" > test_plik.txt
git add test_plik.txt
git commit -m "dodano test_plik.txt"
git push
echo "Zmieniony tekst" > test_plik.txt
git add test_plik.txt
git commit -m "edytowano test_plik.txt"
git push
git revert 86e3941
git push
```

### Opis:

- Utworzono plik `test_plik.txt` z treścią "Przykładowy tekst" i wypchnięto zmiany (commit `58eab54`).
- Edytowano plik, zmieniając treść na "Zmieniony tekst", i wypchnięto zmiany (commit `86e3941`).
- Użyto `git revert 86e3941`, aby cofnąć zmiany z ostatniego commita, przywracając treść pliku do "Przykładowy tekst".
- Nie wystąpiły konflikty, więc zmiany zostały automatycznie cofnięte.
- Wypchnięto zmiany na zdalne repozytorium.

### Zrzuty ekranu:
![ ](14.png)

![ ](16.png)
---

## Krok 7: Sprawozdanie

W ostatnim kroku przygotowano sprawozdanie w formacie Markdown i dodano je do repozytorium.

### Wykonane polecenia:

```bash
mkdir Lab_3/sprawozdanie_417298
touch Lab_3/sprawozdanie_417298/sprawozdanie.md
git add Lab_3/sprawozdanie_417298
git commit -m "dodano sprawozdanie"
git push
```

### Opis:

- Utworzono folder `sprawozdanie_417298` w katalogu `Lab_3`.
- Stworzono plik `sprawozdanie.md` i wypełniono go treścią (opis kroków, zrzuty ekranu, tematy dodatkowe).
- Dodano sprawozdanie do repozytorium i wypchnięto zmiany.

### Zrzut ekranu:
![ ](18.png)
---

## Tematy dodatkowe

### Git hooks client-side vs server-side

- **Client-side hooks**: Są to hooki wykonywane lokalnie na komputerze developera. Przykłady:
  - `pre-commit`: Sprawdza kod przed wykonaniem commita (np. linting).
  - `pre-push`: Wykonuje się przed `git push`, np. do uruchamiania testów.
  - Używane do lokalnej walidacji kodu przed wysłaniem na zdalne repozytorium.
- **Server-side hooks**: Wykonywane na serwerze Git (np. GitHub). Przykłady:
  - `pre-receive`: Sprawdza zmiany przed zaakceptowaniem pusha, np. w pipeline’ach CI/CD.
  - `update`: Pozwala kontrolować, które branche mogą być aktualizowane.
  - Używane do zapewnienia zgodności z zasadami zespołu i automatycznych procesów CI/CD.

### Git reset vs git revert

- **Git reset**:
  - Usuwa commity z historii, przesuwając HEAD do wskazanego commita.
  - Przykład: `git reset --hard <hash>` usuwa wszystkie zmiany do wskazanego commita.
  - Niebezpieczne w pracy zespołowej, ponieważ zmienia historię, co może powodować konflikty przy współpracy na zdalnym repozytorium.
- **Git revert**:
  - Tworzy nowy commit, który odwraca zmiany z wybranego commita, nie zmieniając historii.
  - Przykład: `git revert <hash>` cofa zmiany z danego commita.
  - Bezpieczne dla zdalnych repozytoriów, ponieważ nie usuwa historii. W tym laboratorium użyto `git revert` do cofnięcia zmian w pliku `test_plik.txt`.

### Mocki w TDD

- **Rola mocków**:
  - Mocki to obiekty symulujące zachowanie zależności zewnętrznych (np. API, bazy danych) w testach.
  - W TDD (Test-Driven Development) pozwalają testować kod w izolacji, bez konieczności używania rzeczywistych zasobów.
- **Zastosowanie**:
  - Dzięki mockom można symulować odpowiedzi API, błędy czy dane z bazy, co przyspiesza pisanie testów.
  - Zapewniają izolację – testy nie zależą od zewnętrznych systemów, co czyni je bardziej niezawodnymi.
  - Przykład: W teście funkcji pobierającej dane z API można użyć mocka, który zwraca zaprogramowaną odpowiedź, zamiast wykonywać prawdziwe zapytanie.

---

## Podsumowanie

Laboratorium zostało wykonane zgodnie z instrukcją. Zaktualizowano repozytorium, utworzono nowy branch, wykonano edycję kodu, przeprowadzono code review, wprowadzono poprawki, cofnięto zmiany za pomocą `git revert`, a na koniec przygotowano sprawozdanie. Wszystkie kroki zostały udokumentowane zrzutami ekranu, a odpowiedzi na tematy dodatkowe zostały zawarte w sprawozdaniu. Pull Request jest gotowy do oddania.