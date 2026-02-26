# ☀ Solcellskalkyl — Flerbostadshus

Ekonomisk investeringsanalys för solceller till flerbostadshus — 30-årsberäkning.
**[Länk till verktyg →]([https://dimfield-git.github.io/solcellsber/tod-architecture.html](https://github.com/dimfield-git/solcellskalkyl/blob/main/index.html)**

## Syfte

Verktyg för energikonsulter som gör solcellskalkyler åt bostadsrättsföreningar. En enda HTML-fil som fungerar offline utan externa beroenden.

## Användning

Öppna `index.html` i valfri webbläsare. Fyll i offertdata och årsredovisningsdata, resultat beräknas automatiskt.

## Funktioner

- Enkel beräkningsmodell med automatisk uppskattning av självförbrukningsandel
- 30-årig årstabell med degradering och elprisinflation
- Sammanfattning med nyckeltal, återbetalningstid och CO₂-besparing
- 7 valbara färgteman (5 ljusa, 2 mörka)
- PDF-export via utskriftsdialogen
- Helt offline — inga servrar eller externa beroenden

## Beräkningsmodell

- **Degradering:** Linjär regression, default 85% kvarvarande effekt vid år 25, extrapolerad till 30 år
- **Elprisinflation:** Default 3%/år, appliceras på både inköps- och försäljningspris
- **Självförbrukningsandel:** `E_tillgodo = 1 − e^(−C/P)` där C = total förbrukning, P = årsproduktion
- **CO₂-referens:** Svensk residualmix, 80 g CO₂/kWh (Energimyndigheten)

## Licens

GPL-3.0
