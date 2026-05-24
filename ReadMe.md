# OpenRouter Proven Working Models

Diese Liste enthält Modelle, die erfolgreich mit OpenRouter getestet wurden (z.B. für Vision/OCR per `chat/completions` mit `image_url`). Ziel ist es, schnell verlässliche Modell-IDs zu haben, die in der Praxis tatsächlich funktionieren.

## Modelle (proven working)

- **openai/gpt-4o**
  - **Notes:** Sehr kompetent, aber teuer
- **mistralai/mistral-small-3.2-24b-instruct**
  - **Notes:** Keine Fehler gehabt und eindeutig günstiger
- **qwen/qwen3.5-flash-02-23**
  - **Notes:** Auch keine Fehler und etwas schneller
- **google/gemini-3.1-flash-lite**
  - **Notes:** Schnell, aber fehlerhaft bei dünner Schrift und inkonstantes Layout

> Hinweis: Wenn du weitere Modelle in deinem OpenRouter-Account erfolgreich getestet hast, ergänze sie hier (inkl. genauer Model-ID).

## Hinweise

- **Prompt-Generierung (bewährter Workflow):** Für deutlich bessere OCR- und Extraktionsqualität hat sich bewährt, den *finalen* OCR/API-Prompt nicht direkt „aus dem Bauch heraus“ zu schreiben, sondern ihn zuerst mit einem Online-KI-Tool als **maßgeschneiderten Prompt** generieren zu lassen. Dabei gibst du dem Online-Tool klare Ziele (Voll-OCR + strukturierte Extraktion), harte Ausgabe-Regeln (nur JSON, keine Halluzination) und produkt-spezifische Matching-Hinweise (Abkürzungen/Slogans). Das hat hier besonders gut funktioniert.
  **Meta-Prompt (Prompt zur Generierung deines OCR-Prompts):**
  > Diesen Text kopierst du in ein Online-KI-Tool. Als Ergebnis soll dir das Tool einen fertigen Prompt liefern, den du anschließend in `API_PROMPT` (bzw. in dein Script/.env) einfügst.
  ```text
  Erstelle einen maximal robusten Prompt für ein multimodales OCR/Beleg-Parsing-Modell (Receipt Photo OCR). 
  Kontext: Der Input ist ein Foto eines Kassenzettels unter realen Bedingungen (schräg fotografiert, Schatten/Blitz, Unschärfe, verkürzte Artikeltexte).
  Ziele des finalen Prompts:
  1) Vollständige OCR-Transkription (zeilenweise, Reihenfolge beibehalten) + eine normalisierte Textvariante.
  2) Strukturierte Extraktion als JSON mit mindestens:
     - merchant.name (Markt/Shop)
     - merchant.address (inkl. PLZ falls vorhanden)
     - receipt.date (YYYY-MM-DD, falls möglich)
     - receipt.time (falls vorhanden)
     - line_items[] mit description_raw, quantity, unit_price, total_price, confidence
     - totals (subtotal/tax/total soweit vorhanden)
  3) Spezial-Identifikation eines Zielprodukts, auch wenn der Name auf dem Bon obskur/abgekürzt ist.
  Zielprodukt-Daten (bitte im finalen Prompt berücksichtigen):
  - Marke: {BRAND}
  - Produktlinie/Produktname: {PRODUCT_LINE}
  - Varianten/Sorten: {VARIANTS}
  - Typische Bon-Schreibweisen/Abkürzungen (inkl. OCR-Fehler): {RECEIPT_ALIASES}
  - Zusätzliche Hinweise/Keywords/Slogans/Größe: {HINTS}
  Anforderungen an den finalen Prompt:
  - Ausgabe ausschließlich als gültiges JSON (keine Markdown-Fences, kein zusätzlicher Text).
  - Strikte Anti-Halluzination: Wenn unsicher => null + confidence="low" + notes.
  - Zahlen-Regeln: Dezimalpunkt im JSON, Währungszeichen entfernen, Komma->Punkt.
  - Matching-Logik: flexible Heuristiken (Brand-Varianten, Keywords, Varianten-Hinweise, Größen-Hinweise), aber ohne zu raten.
  - Vollständigkeit: Alle Produkte/Positionen ausgeben, auch wenn unklar/verkürzt.
  Liefere als Antwort NUR den fertigen OCR/API-Prompt, den ich 1:1 in mein Script übernehmen kann.
  ```

- **Kompatibilität (Vision/OCR):** Für OCR aus Bildern brauchst du ein Modell, das Bildinput unterstützt. Nicht jede Model-ID auf OpenRouter ist multimodal.
- **API-Endpunkt:** `https://openrouter.ai/api/v1/chat/completions`
- **Auth:** `Authorization: Bearer <OPENROUTER_API_KEY>`
- **Ressourcen:**
  - OpenRouter Docs: https://openrouter.ai/docs
  - Model-Verzeichnis: https://openrouter.ai/models

## Weiterführendes / Kontakt

- Für Projekt-spezifische Fragen: prüfe zuerst die OpenRouter-Dokumentation und die Model-Seite des jeweiligen Providers.
- Optional: Lege in deiner `.env` die Variablen `OPENROUTER_API_KEY`, `OPENROUTER_MODEL` und `API_PROMPT` fest (oder nutze die GUI, die diese Werte automatisch speichert).
