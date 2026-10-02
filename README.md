# Zadanie Kalkulator

| Termin oddania | Punkty     |
|----------------|:-----------|
|    25.10.2026 23:00  |   10       |

--- 
Przekroczenie terminu o **n** zajęć wiąże się z karą:
- punkty uzyskania za realizację zadania są dzielone przez **2<sup>n</sup>**.

--- 

# Projekt: Kalkulator Java – Rozbudowa, Testy i Architektura SOLID

W katalogu `src` znajduje się wstępna implementacja kalkulatora wraz z przykładowymi testami jednostkowymi. Celem zadania jest rozbudowa aplikacji, pokrycie jej kompleksowymi testami oraz stopniowa refaktoryzacja kodu zgodnie z dobrymi praktykami inżynierii oprogramowania i zasadami **SOLID**.

---

## Zadanie 1: Pokrycie testami operacji podstawowych i analiza wartości brzegowych

**Cel:** Uzupełnienie i uszczelnienie zestawu testów jednostkowych dla istniejących operacji (dodawanie, mnożenie).

**Wymagania:**

* Dodaj testy jednostkowe sprawdzające zachowanie kalkulatora na krańcach dziedziny (edge cases). Weryfikacji poddaj m.in.:
* Elementy neutralne operacji (np. dodawanie $0$, mnożenie przez $0$ oraz przez $1$).
* Działania na liczbach ujemnych oraz wartościach o mieszanych znakach.
* Wartości skrajne i przekroczenie zakresu typu liczbowego (np. wartości bliskie `Double.MAX_VALUE`, `Double.MIN_VALUE`, `NaN` lub `Double.POSITIVE_INFINITY`).


* **Jakość testów:**
  * Stosuj konwencję strukturyzacji testów **AAA (Arrange-Act-Assert)** lub **Given-When-Then**.
  * Zadbaj o intencjonalne nazwy metod testowych jednoznacznie opisujące testowany przypadek (np. `shouldReturnZeroWhenMultiplyingByZero`).



---

## Zadanie 2: Rozbudowa o nowe operacje i architektura OCP (Open/Closed Principle)

**Cel:** Wprowadzenie nowych działań matematycznych z zachowaniem elastyczności architektury.

**Wymagania:**

* **Zasada Otwarte/Zamknięte (OCP):** Przebuduj architekturę kalkulatora tak, aby dodanie nowej operacji odbywało się poprzez **rozszerzenie kodu** (dodanie nowej klasy/komponentu), a **nie modyfikację** istniejących struktur sterujących (np. unikaj rozbudowywania instrukcji `switch` lub ciągów `if-else` w głównej klasie kalkulatora). Zastosuj odpowiedni wzorzec projektowy (np. *Strategia*, interfejs `Operation` lub obiekty funkcyjne).
* **Implementacja operacji:** Dodaj do kalkulatora co najmniej dwie nowe operacje (np. dzielenie, potęgowanie, pierwiastkowanie).
* **Wykluczenia z dziedziny:** Przynajmniej jedna z nowych operacji musi posiadać ograniczenia dziedzinowe (np. dzielenie przez $0$, pierwiastkowanie liczby ujemnej).
* **Testy:** Przygotuj zestaw testów jednostkowych weryfikujących zarówno poprawne wyniki nowych operacji, jak i prawidłowe zgłaszanie błędów przy przekroczeniu dziedziny.

---

## Zadanie 3: Obsługa pamięci i izolacja odpowiedzialności (SRP – Single Responsibility Principle)

**Cel:** Dodanie rejestru pamięci kalkulatora z wyselekcjonowanym podziałem ról w kodzie.

**Wymagania:**

* **Funkcjonalność pamięci:** Zaimplementuj obsługę pamięci (np. zapamiętanie bieżącego wyniku `MS`, odczyt wartości z pamięci `MR`, czyszczenie pamięci `MC`, dodanie wyniku do pamięci `M+`). Kalkulator musi umożliwiać wykonywanie dalszych obliczeń z wykorzystaniem wartości zapisanej w pamięci.
* **Zasada Jednej Odpowiedzialności (SRP):** Klasa kalkulatora nie powinna bezpośrednio zarządzać strukturą i stanem pamięci. Przenieś odpowiedzialność za rejestr pamięci do osobnej, dedykowanej klasy/komponentu (np. `CalculatorMemory`), z której kalkulator korzysta na zasadzie kompozycji.
* **Testy:** Napisz testy jednostkowe weryfikujące pełny cykl życia pamięci (zapis, odczyt, czyszczenie, nadpisywanie oraz wykonywanie operacji arytmetycznych z wartością pobraną z pamięci).

---

## Zadanie 4: Jawna reprezentacja stanu błędu i odporność aplikacji

**Cel:** Wprowadzenie kontrolowanego mechanizmu obsługi i reprezentacji stanu błędu w kalkulatorze.

**Wymagania:**

* **Jawny stan kalkulatora:** Zamiast dopuszczać do niekontrolowanego rzucania wyjątków podczas obliczeń, kalkulator powinien utrzymywać jawny stan reprezentujący status ostatniej operacji (np. za pomocą typu wyliczeniowego `CalculationStatus` o wartościach takich jak `OK`, `DIVIDE_BY_ZERO`, `INVALID_DOMAIN`, `OVERFLOW` lub obiektu wynikowego `CalculationResult`).
* **Propagacja i czyszczenie błędu:** Zdefiniuj i zaimplementuj zasady zachowania kalkulatora po wystąpieniu błędu (np. czy wykonanie kolejnej poprawnej operacji automatycznie czyści stan błędu, czy wymagane jest wywołanie metody czyszczącej/resetującej).
* **Refaktoryzacja istniejących operacji:** Dostosuj wszystkie wcześniej zaimplementowane operacje tak, aby ustawiały odpowiedni stan błędu w przypadku wystąpienia sytuacji krytycznej.
* **Testy:** Napisz testy jednostkowe sprawdzające:
  * Poprawność przechodzenia kalkulatora w stan błędu dla nieprawidłowych danych wejściowych.
  * Zachowanie kalkulatora podczas próby wykonywania dalszych działań w stanie błędu.
  * Prawidłowe powracanie kalkulatora do stanu gotowości (`OK`).
