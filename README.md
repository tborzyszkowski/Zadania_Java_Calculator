
# Zadanie: Koszyk Internetowy (JavaMarkt)

| Termin oddania | Punkty |
| --- | --- |
| 29.11.2026 23:00 | 20 pkt |

---

> **Kary za opóźnienia:**
> Przekroczenie terminu o **$n$** zajęć wiąże się z karą: uzyskana liczba punktów za realizację zadania jest dzielona przez **$2^n$**.

---

## 1. Opis problemu

Twoja firma otrzymała zlecenie na stworzenie modułu obsługi koszyka zakupowego dla sklepu internetowego **JavaMarkt**.

Jedną z kluczowych funkcjonalności systemu jest elastyczny mechanizm naliczania promocji, rabatów i ofert specjalnych. W obecnym etapie projektowym wymagane jest uwzględnienie następujących reguł:

* **Rabat wartościowy:** Jeśli wartość zamówienia przekracza **300 zł**, klient otrzymuje **5% zniżki** na całe zamówienie.
* **Promocja 2+1:** Jeśli klient kupi co najmniej **3 produkty**, najtańszy z nich otrzymuje **gratis** (zniżka 100% na ten produkt).
* **Gratis rzeczowy:** Jeśli wartość zamówienia przekracza **200 zł**, klient otrzymuje **firmowy kubek gratis** (produkt o cenie 0 zł).
* **Kupon rabatowy:** Jednorazowy kupon **30%** na jeden wybrany produkt.

Promocje mogą zmieniać się dynamicznie w trakcie działania programu - istniejące reguły mogą być wycofywane, a nowe dodawane. System musi być otwarta na rozbudowę o kolejne, nieznane jeszcze typy rabatów.

---

## 2. Wymagania funkcjonalne

Zaimplementuj logikę biznesową operującą na obiektach typu `Product`, która umożliwia:

1. **Wyszukiwanie ekstremów:** Znajdowanie najtańszego oraz najdroższego produktu w zadanej kolekcji produktów.
2. **Wyszukiwanie $n$ produktów:** Znajdowanie $n$ najtańszych oraz $n$ najdroższych produktów w zadanej kolekcji.
3. **Sortowanie elastyczne:** Domyślne sortowanie produktów w koszyku **malejąco według ceny**, a następnie **alfabetycznie według nazwy**. Mechanizm sortowania musi umożliwiać zmianę kryteriów w czasie działania aplikacji.
4. **Kalkulacja wartości:** Wyliczanie łącznej sumy cen wszystkich produktów w koszyku.
5. **Aplikowanie promocji:** Przeliczanie cen produktów i wartości koszyka po zastosowaniu zdefiniowanych promocji.
6. **Rozszerzalność (SOLID):** Dodawanie nowych reguł promocyjnych bez konieczności modyfikacji istniejącego kodu.
7. **(Zadanie dodatkowe, +3 pkt):** **Optymalizacja promocji** — stworzenie algorytmu proponującego najkorzystniejszy sposób zastosowania promocji (porównanie wyników przyłożenia promocji w różnej kolejności i wybór wariantu dającego najniższą cenę końcową dla klienta).

---

## 3. Specyfikacja klasy `Product`

Klasa `Product` powinna posiadać co najmniej następujące atrybuty:

* `code` (`String`) — unikalny kod produktu
* `name` (`String`) — nazwa produktu
* `price` (`double`) — cena bazowa produktu
* `discountPrice` (`double`) — cena produktu po uwzględnieniu naliczonych promocji

> 💡 **Uwaga dotycząca typu `double`:**
> W tym zadaniu dla uproszczenia używamy typu `double` do przechowywania cen. Należy mieć świadomość, że w produkcyjnych systemach finansowych typ `double` jest **nieodpowiedni** ze względu na błędy precyzji arytmetyki zmiennoprzecinkowej (np. `0.1 + 0.2 != 0.3`). W rzeczywistych aplikacjach finansowych stosuje się typ `BigDecimal` lub operuje na kwotach wyrażonych w groszach/centach (typ `long`).

---

## 4. Etapy realizacji & Refaktoryzacja

Zadanie należy wykonać dwuetapowo:

1. **Etap I:** W pierwszej wersji rozwiązania jako kolekcję produktów w koszyku wykorzystaj **zwykłą tablicę produktów** (`Product[]`).
2. **Etap II:** Przeprowadź **refaktoryzację** kodu, zastępując tablicę interfejsem `List<Product>`.

Przeanalizuj, jak zmiana wewnętrznej struktury danych wpływa na pozostały kod aplikacji — jest to praktyczne ćwiczenie z zasady **Open/Closed Principle (OCP)** oraz programowania do interfejsów (*Program to an interface, not an implementation*).

---

## 5. Wytyczne jakościowe i architektoniczne

### Uwaga 1: Testy jednostkowe

Projekt powinien zawierać kompletne testy jednostkowe (np. JUnit 5) weryfikujące poprawność operacji na koszyku, wyszukiwania, sortowania oraz naliczania promocji.

### Uwaga 2: Wzorce projektowe

Projektując mechanizm promocji oraz operacji na koszyku, rozważ zastosowanie jednego z poniższych wzorców projektowych (lub ich kombinacji):

* **[Strategy](https://www.oodesign.com/strategy-pattern.html)** — każda promocja jako osobna strategia obliczania ceny. Stosuj, gdy promocje wpływają na sposób wyliczania końcowej ceny koszyka i mogą być wymieniane dynamicznie.
* **[Command](https://www.oodesign.com/command-pattern.html)** — każda operacja na koszyku (dodanie produktu, zastosowanie promocji, cofnięcie promocji) jako osobne polecenie. Stosuj, gdy chcesz umożliwić cofanie operacji (*undo*) lub ich kolejkowanie do późniejszego wykonania.

> **Wymóg:** Uzasadnij swój wybór wzorca w komentarzu w kodzie lub w osobnym pliku `DECISIONS.md`.

### Uwaga 3: Sortowanie i DIP

Planując mechanizm sortowania, zwróć uwagę na **[Dependency Inversion Principle (DIP)](https://www.oodesign.com/dependency-inversion-principle.html)** oraz wykorzystaj interfejsy dostępne w języku Java: `Comparable` oraz `Comparator`.

### Uwaga 4: Enkapsulacja i mutowalność

Zadbaj o poprawną **enkapsulację** — rozważ, czy klasa `Product` powinna być mutowalna (*mutable*), czy niemutowalna (*immutable*). Uzasadnij podjętą decyzję.

### Uwaga 5: Sytuacje brzegowe

Kod powinien poprawnie obsługiwać sytuacje skrajne i nietypowe, takie jak:

* Pusty koszyk
* Produkty z cenie zerowej (`0.0`)
* Próba zastosowania promocji na pustym koszyku
* Wartości `null` w kolekcji produktów

### Uwaga 6: Diagram klas UML

Do rozwiązania dołącz **diagram klas UML** przedstawiający zaprojektowaną strukturę (w formacie graficznym, np. PNG, lub w formacie tekstowym, np. PlantUML / Mermaid).
