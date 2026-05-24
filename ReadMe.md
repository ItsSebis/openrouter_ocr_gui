# OpenRouter Proven Working Models

Diese Liste enthält Modelle, die erfolgreich mit OpenRouter getestet wurden (z.B. für Vision/OCR per `chat/completions` mit `image_url`). Ziel ist es, schnell verlässliche Modell-IDs zu haben, die in der Praxis tatsächlich funktionieren.

## Modelle (proven working)

- **openai/gpt-4o**
  - **Notes:** Sehr kompetent, aber teuer
- **mistralai/mistral-small-3.2-24b-instruct**
  - **Notes:** Keine Fehler gehabt und eindeutig günstiger
- **qwen/qwen3.5-flash-02-23**
  - **Notes:** Auch keine Fehler und etwas schneller

> Hinweis: Wenn du weitere Modelle in deinem OpenRouter-Account erfolgreich getestet hast, ergänze sie hier (inkl. genauer Model-ID).

## Hinweise

- **Kompatibilität (Vision/OCR):** Für OCR aus Bildern brauchst du ein Modell, das Bildinput unterstützt. Nicht jede Model-ID auf OpenRouter ist multimodal.
- **API-Endpunkt:** `https://openrouter.ai/api/v1/chat/completions`
- **Auth:** `Authorization: Bearer <OPENROUTER_API_KEY>`
- **Ressourcen:**
  - OpenRouter Docs: https://openrouter.ai/docs
  - Model-Verzeichnis: https://openrouter.ai/models

## Weiterführendes / Kontakt

- Für Projekt-spezifische Fragen: prüfe zuerst die OpenRouter-Dokumentation und die Model-Seite des jeweiligen Providers.
- Optional: Lege in deiner `.env` die Variablen `OPENROUTER_API_KEY`, `OPENROUTER_MODEL` und `API_PROMPT` fest (oder nutze die GUI, die diese Werte automatisch speichert).
