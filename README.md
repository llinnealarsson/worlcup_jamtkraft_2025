# Jämtkraft World Cup 2025 - Resultatavla

Digital resultatavla för Jämtkraft - Världscupen Östersund 2025.

## Översikt

Detta projekt är en webbsida som visar resultat från Jämtkraft World Cup 2025. Resultaten hämtas automatiskt från ett Google Sheets-dokument och visas i realtid på webbsidan.

## Funktioner

- **Automatisk uppdatering**: Hämtar nya resultat var 3:e sekund från Google Sheets
- **Datumfiltrering**: Visar resultat för olika datum med en dropdown-meny
- **Kategorier**: Visar topp 5 resultat för Dam, Herr och Ungdom
- **Donation tracking**: Räknar automatiskt totala donationer (50 kr per resultat)
- **Anti-flicker-mekanism**: Förhindrar att senaste resultatet försvinner och återkommer

## Teknisk implementation

### Anti-flicker-lösning

För att förhindra att senaste resultatet "flimrar" (visas och döljs upprepade gånger) har vi implementerat en monoton uppdateringsmekanism:

1. **Problem**: Google Sheets kan tillfälligt servera inkonsekvent data när nya poster läggs till, vilket orsakar att den senaste posten visas och döljs upprepat.

2. **Lösning**: Vi spårar antalet resultat från föregående hämtning och uppdaterar endast displayen om det nya datat har minst lika många poster som tidigare. Detta garanterar att en gång ett resultat visas, kommer det inte att försvinna på grund av tillfälliga inkonsistenser.

3. **Implementation**:
   ```javascript
   let previousResultCount = 0;
   
   // I loadData():
   if (parsed.length >= previousResultCount) {
     allResults = parsed;
     previousResultCount = parsed.length;
     // ... uppdatera display
   }
   ```

### CSV-format

Webbsidan förväntar sig ett CSV med följande kolumner:
- **Tidsstämpel**: När resultatet registrerades
- **Namn**: Deltagarens namn
- **Tid (i sekunder)**: Tiden i sekunder
- **Kategori**: "Dam", "Herr" eller "Ungdom"
- **Datum**: Datumet för tävlingen (används för filtrering)

## Installation

1. Klona detta repository
2. Öppna `index.html` i en webbläsare
3. Inga beroenden eller build-steg krävs - det är en fristående HTML-fil

## Konfiguration

I `index.html`, uppdatera följande konstanter vid behov:

```javascript
const CSV_URL = "...";              // URL till Google Sheets CSV
const DONATION_PER_RESULT = 50;     // Donation per resultat
const REFRESH_MS = 3000;            // Uppdateringsintervall i millisekunder
```

## Cache-busting

För att säkerställa att vi alltid får den senaste datan från Google Sheets använder vi:
- Timestamp-parameter i URL:en (`&t=` + nuvarande tidsstämpel)
- `cache: "no-store"` i fetch-konfigurationen

## Support

För frågor eller problem, kontakta projektets maintainer.
