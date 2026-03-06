# ☀ Solcellskalkyl — Flerbostadshus

Ekonomisk investeringsanalys för solceller till flerbostadshus — 30-årsberäkning.
**[Länk till verktyg →](https://dimfield-git.github.io/solcellskalkyl/)**

## Syfte

Verktyg för solcellskalkyler i flerbostadshus. En enda HTML-fil som fungerar offline utan externa beroenden.

## Användning

Öppna `index.html` i valfri webbläsare. Fyll i offertdata och årsredovisningsdata, resultat beräknas automatiskt.

## Funktioner

- Enkel beräkningsmodell med automatisk uppskattning av självförbrukningsandel
- 30-årig årstabell med degradering och elprisinflation
- Sammanfattning med nyckeltal, återbetalningstid och CO₂-besparing
- 8 valbara färgteman (6 ljusa, 2 mörka)
- PNG-export av sammanfattning och årstabell separat
- Stöd för kollektiv egenförbrukning med dynamisk vägledning
- Helt offline — inga servrar eller externa beroenden (PNG-export kräver internet)

## Beräkningsmodell

### Självförbrukningsandel

Andelen producerad solel som föreningen tillgodogör sig beräknas med en exponentiell modell med takfaktor:

```
E_tillgodo = α × (1 − e^(−C/P))
```

- **C** = total förbrukning (kWh/år)
- **P** = förväntad årsproduktion (kWh/år)
- **α** = takfaktor — fysiskt tak för självförbrukning utan batterilagring

#### Takfaktor α

Solceller producerar dagtid medan förbrukning sker dygnet runt. Utan batterilagring finns därför en övre gräns för hur stor andel av produktionen som kan tillgodogöras, oavsett hur hög kvoten C/P är.

| Scenario | α (default) | Motivering |
|----------|-------------|------------|
| Ej kollektiv egenförbrukning | 0,85 | PV bakom gemensam mätare, enbart fastighetsel absorberar solelen |
| Kollektiv egenförbrukning | 0,92 | Lägenheterna absorberar solel via IMD, jämnare lastprofil dygnet runt |

Värdet på α kan justeras manuellt. Referens: Sommerfeldt & Madani (2016) — optimal dimensionering av PV för svenska flerbostadshus visar självförbrukning på 60–80% vid typisk dimensionering utan lagring.

#### Kollektiv egenförbrukning

Verktyget har en toggle som styr vilken förbrukning som ska anges:

- **Av:** Solcellerna sitter bakom föreningens gemensamma elmätare. Enbart fastighetselen (hissar, ventilation, belysning, tvättstuga) ska anges som total förbrukning.
- **På:** Lägenheterna kan absorbera solel. Total förbrukning inklusive lägenhetsförbrukning (IMD) ska anges. α höjs automatiskt till 0,92.

### Degradering

Linjär regression från 100% vid år 0 till angiven kvarvarande effekt vid år 25 (default 85%), extrapolerad till år 30.

```
Degraderingsfaktor per år: d = (100 − kvarvarande%) / 25
Kvarvarande effekt år n: (100 − d × n) / 100
```

### Elprisinflation

Både inköpspris och försäljningspris inflationsjusteras årligen:

```
Pris år n = Pris år 0 × (1 + inflation/100)^n
```

Default: 3%/år.

### CO₂-besparing

```
CO₂ = Total produktion 30 år × 0,08 / 1000 (ton)
```

Baserat på svensk residualmix, ca 80 g CO₂/kWh. Källa: Energimyndigheten.

## Release Notes

### v1.5 (2025)

- **Kollektiv egenförbrukning:** Toggle med dynamisk vägledning som styr vilken förbrukning som ska anges och justerar α automatiskt
- **Takfaktor α:** Nytt redigerbart fält (default 0,85 / 0,92 beroende på scenario) som sätter fysiskt tak för självförbrukningsmodellen
- **Uppdaterad självförbrukningsmodell:** `E_tillgodo = α × (1 − e^(−C/P))` — ger rimliga värden över hela spektrat av dimensioneringar
- **PNG-export:** Sammanfattning och årstabell exporteras separat som PNG-bilder i 2× upplösning, ersätter PDF-export
- **Bredare layout:** Tabellen ryms utan horisontell scrollning
- **Tema Kooperativ:** Nytt svart/vitt/rött färgtema
- **Redigerbar elprisinflation och degradering** även i enkel modell
- **Kompaktare tabellrubriker**

### v1.2 (2025)

- Sju valbara färgteman (5 ljusa, 2 mörka) med grupperad temaväljare
- PDF-export via utskriftsdialogen med anpassad print-CSS
- Rensa-knapp
- Redigerbar elprisinflation och degradering

### v1.1 (2025)

- Auto-beräkning av årsproduktion, E_tillgodo och E_sälj
- Validering av obligatoriska fält
- Systemfont — 100% offline utan Google Fonts
- Svenskt talformat: komma som decimaltecken, tusenavgränsare

### v1.0 (2025)

- Första prototyp: enkel modell med 30-årsberäkning
- Tre färgteman (Nordisk Energi, Solcell, Blueprint)

## Licens

GPL-3.0 — se [LICENSE](LICENSE)
