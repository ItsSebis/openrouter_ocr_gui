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

- **Prompt-Generierung (bewährter Workflow):** Für deutlich bessere OCR- und Extraktionsqualität hat sich bewährt, den API-Prompt mit einem Online-KI-Tool iterativ zu generieren (so wie wir es hier gemacht haben): Produkt/Marke kurz beschreiben, Beispiele für obskure Kassenzettel-Abkürzungen nennen und klare JSON-Ausgabe-Regeln vorgeben. Das Ergebnis ist oft wesentlich robuster als ein generischer „mach OCR“-Prompt.
  **Template-Prompt (nur Produktdetails einsetzen):**
  ```text
  Du bist ein hochpräzises OCR- und Beleg-Parsing-System. Du erhältst ein Foto eines Kassenzettels.
  Ziel: Alle Positionen mit Preisen extrahieren und zusätzlich ein bestimmtes Produkt auch bei verkürzten/obskuren Namen sicher erkennen.
  Produktdetails:
  - Marke: {BRAND}
  - Produktlinie/Produktname: {PRODUCT_LINE}
  - Varianten/Sorten: {VARIANTS}  (z.B. "A", "B")
  - Typische Bon-Schreibweisen/Abkürzungen: {RECEIPT_ALIASES}
  - Erkennungs-Hinweise (Slogans/Keywords/Größe): {HINTS}
  Anforderungen:
  - Gib NUR gültiges JSON zurück.
  - Extrahiere alle line_items vollständig (description_raw, quantity, unit_price, total_price, confidence).
  - Extrahiere auch die Daten des Kaufes selbst, z.B. den Markt, die Adresse (besonders die PLZ) und das Datum
  - Implementiere ein flexibles Matching gegen die Produktdetails und schreibe Treffer in matched_product inkl. match_reason + match_confidence.
  - Erfinde keine Werte; bei Unsicherheit null + low confidence + notes.
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
