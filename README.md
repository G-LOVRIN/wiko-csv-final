# 🔄 WIKO BaseLinker - Automatyczne stany magazynowe

Automatyczny system synchronizacji stanów magazynowych WIKO z BaseLinker przy użyciu GitHub Pages i GitHub Actions.

![GitHub Actions Status](https://github.com/G-LOVRIN/wiko-csv-final/actions/workflows/update-fixed-wiko.yml/badge.svg)

## 📊 Status systemu

**🔗 URL dla BaseLinker:**
```
https://g-lovrin.github.io/wiko-csv-final/wiko-stany.csv
```

**📈 Ostatni raport:**
- [Sprawdź status aktualizacji](https://g-lovrin.github.io/wiko-csv-final/last-update.txt)

## ⚙️ Konfiguracja BaseLinker

### Ustawienia importu CSV:

1. **Magazyny** → **Magazyny zewnętrzne** → **Dodaj magazyn**
2. **Typ:** Import CSV
3. **URL:** `https://g-lovrin.github.io/wiko-csv-final/wiko-stany.csv`
4. **Separator:** przecinek (,)
5. **Kodowanie:** UTF-8
6. **Pierwsza linia to nagłówki:** ✅ **TAK**
7. **Kolumna główna (Powiązanie):** **`produkt_sku`**

### Mapowanie kolumn:

| Kolumna CSV | Pole BaseLinker | Opis |
|-------------|-----------------|------|
| `produkt_sku` | SKU produktu | Kod produktu WIKO |
| `ilosc_wiko` | Ilość/Stan magazynowy | Aktualny stan |
| `nazwa_produktu` | Nazwa produktu | Pełna nazwa (opcjonalnie) |
| `ostatnia_aktualizacja` | Data aktualizacji | Timestamp (opcjonalnie) |

### ⚠️ Ważne ustawienia:

- ❌ **Odznacz:** "Zaktualizuj wszystkie obrazki"
- ✅ **Zaznacz:** "Nie aktualizuj pustych wartości"
- ✅ **Zaznacz:** "Pomiń pierwszy wiersz"

## 🔄 Automatyzacja

### Harmonogram aktualizacji:
- ⏰ **Automatycznie:** co 10 minut
- 🔄 **Ręcznie:** przycisk "Run workflow" w Actions
- 📊 **Monitorowanie:** logi w sekcji Actions

### Workflow wykonuje:
1. **Pobiera XML** z serwerów WIKO (`https://eml.pl/wiko/wiko_lovrin.xml`)
2. **Parsuje produkty** i wyciąga dane:
   - `indeks_katalogowy` → SKU produktu
   - `stan_magazynowy` → ilość na magazynie
   - `nazwa` → nazwa produktu
3. **Generuje CSV** z aktualnymi danymi
4. **Publikuje** na GitHub Pages
5. **Tworzy raport** z informacją o przetworzeniu

## 📋 Format danych

### Przykład wygenerowanego CSV:
```csv
produkt_sku,ilosc_wiko,nazwa_produktu,ostatnia_aktualizacja
000604,78,"000604 OBRAZEK ANIOŁKI 7x9 CM","2025-08-12 14:30:00"
000605,15,"000605 RAMKA SREBRNA 10x15 CM","2025-08-12 14:30:00"
000606,0,"000606 ŚWIECZNIK METALOWY","2025-08-12 14:30:00"
```

### Pola produktu WIKO:
- **SKU:** Unikalny kod produktu (np. `000604`)
- **Ilość:** Stan magazynowy (liczba całkowita)
- **Nazwa:** Pełna nazwa produktu
- **Timestamp:** Data i czas ostatniej aktualizacji

## 🛠️ Zarządzanie systemem

### Ręczna aktualizacja:
1. **Actions** → **Fixed WIKO Parser**
2. **Run workflow** → **Run workflow**

### Sprawdzanie logów:
1. **Actions** → wybierz ostatnie wykonanie
2. **Fetch and process WIKO XML** → szczegółowe logi

### Diagnostyka problemów:
1. **Debug workflow:** `Debug XML Structure` - sprawdza strukturę XML
2. **Simple debug:** `Simple XML Debug` - podstawowa analiza
3. **Logi wykonania** - szczegółowe informacje o błędach

## 📊 Monitorowanie

### Wskaźniki jakości:
- **Liczba produktów:** ~1757 (wszystkie z feed WIKO)
- **Częstotliwość:** aktualizacja co 10 minut
- **Dostępność:** 99.9% (GitHub Pages SLA)
- **Opóźnienie:** < 2 minuty od zmiany w XML

### Alerting:
- **Brak aktualizacji:** sprawdź Actions
- **Mało produktów:** sprawdź logi parsera
- **Błędy 404:** sprawdź dostępność XML WIKO

## 🔧 Rozwiązywanie problemów

### Częste problemy:

#### ❌ BaseLinker pobiera kod HTML zamiast CSV
**Rozwiązanie:** Sprawdź URL - musi być `https://g-lovrin.github.io/wiko-csv-final/wiko-stany.csv`

#### ❌ Mała liczba produktów (< 100)
**Rozwiązanie:** 
1. Sprawdź logi workflow
2. Uruchom debug workflow
3. Sprawdź dostępność XML WIKO

#### ❌ Stare dane (timestamp sprzed kilku godzin)
**Rozwiązanie:**
1. Sprawdź czy workflow działa (Actions)
2. Sprawdź czy są błędy w logach
3. Uruchom ręczną aktualizację

#### ❌ Workflow nie uruchamia się automatycznie
**Rozwiązanie:**
1. Settings → Actions → sprawdź czy są włączone
2. Sprawdź składnię cron w workflow
3. Sprawdź uprawnienia repozytorium

### Kontakt z WIKO:
Jeśli XML feed nie jest dostępny, skontaktuj się z WIKO w sprawie:
- Dostępności feed XML
- Struktury danych
- Autoryzacji (jeśli wymagana)

## 🚀 Rozwój systemu

### Planowane ulepszenia:
- [ ] **Webhook trigger** - natychmiastowa aktualizacja po zmianie w WIKO
- [ ] **Filtrowanie produktów** - tylko aktywne/dostępne
- [ ] **Walidacja danych** - sprawdzanie poprawności stanów
- [ ] **Historia zmian** - śledzenie zmian stanów
- [ ] **Alerting** - powiadomienia o problemach

### Struktura repozytorium:
```
wiko-csv-final/
├── .github/
│   └── workflows/
│       ├── update-fixed-wiko.yml    # Główny workflow
│       ├── debug-xml.yml            # Debug struktury XML
│       └── debug-simple.yml         # Prosty debug
├── wiko-stany.csv                   # Główny plik dla BaseLinker
├── last-update.txt                  # Raport ostatniej aktualizacji
├── sample_product.txt               # Przykład struktury produktu
└── README.md                        # Ta dokumentacja
```

## 📞 Wsparcie

### W przypadku problemów:

1. **Sprawdź Actions** - czy workflow działa bez błędów
2. **Sprawdź logi** - szczegółowe informacje o problemach  
3. **Uruchom debug** - workflow diagnostyczny
4. **Sprawdź URL** - czy GitHub Pages działa
5. **Skontaktuj się** z administratorem systemu

### Przydatne linki:
- **🔗 CSV dla BaseLinker:** [wiko-stany.csv](https://g-lovrin.github.io/wiko-csv-final/wiko-stany.csv)
- **📊 Status systemu:** [last-update.txt](https://g-lovrin.github.io/wiko-csv-final/last-update.txt)
- **🔍 Logi Actions:** [GitHub Actions](../../actions)
- **⚙️ Ustawienia workflow:** [Workflows](../../tree/main/.github/workflows)

---

*Ostatnia aktualizacja dokumentacji: 2025-08-12*  
*System działa automatycznie 24/7 dzięki GitHub Actions i GitHub Pages*
