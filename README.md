# Rio Bravo - repozytorium startowe skilli

Szablon repozytorium dla rutyny Claude Code, którą uruchamia [Rio Bravo](https://app.riobravo.ai). Piszesz do agenta w Telegramie, Slacku lub Google Chat, Rio Bravo uruchamia tę rutynę, a ona odpowiada w tej samej rozmowie.

## Jak zacząć

1. Kliknij **Use this template → Create a new repository** (może być prywatne).
2. W [claude.ai/code/routines](https://claude.ai/code/routines) utwórz rutynę:
   - **Advanced settings - General - Repository:** to repozytorium.
   - **Cloud environment - Environment variables:** `RIO_BRAVO_MCP_KEY` = klucz MCP z panelu Rio Bravo.
   - **Cloud environment - Network access:** dodaj domenę usługi `app.riobravo.ai`.
   - **Instrukcja rutyny:** `Postępuj według pliku CLAUDE.md z repozytorium. Każde wejście to zadanie z Rio Bravo (protokół w skillu gateway-protocol).`
   - **Trigger - API:** skopiuj Fire URL i token do panelu Rio Bravo.
3. Uzupełnij [`preferencje.md`](preferencje.md) - godziny pracy, ton, ważne osoby.

Kreator w panelu Rio Bravo prowadzi przez te kroki i na końcu wysyła polecenie testowe.

## Co jest w środku

| Plik | Do czego |
| --- | --- |
| `CLAUDE.md` | Instrukcja dla rutyny: kim jest agent i jak pracuje. |
| `.mcp.json` | Połączenie z narzędziami Rio Bravo (`reply`, `done`, `fail`, sprawy). Klucz bierze ze zmiennej `RIO_BRAVO_MCP_KEY`. |
| `preferencje.md` | Twoje preferencje. Aktualizacje szablonu go nie nadpisują. |
| `.claude/skills/gateway-protocol` | Protokół z Rio Bravo - nie zmieniaj. |
| `.claude/skills/sprawy` | Zadania na dni: otwieranie spraw, follow-upy, przypomnienia. |
| `.claude/skills/zadania-todoist` | Dodawanie zadań do Todoista (konektor Todoist). |
| `.claude/skills/kalendarz-spotkania` | Wolne terminy i spotkania (konektor Google Calendar). |

Konektory (Todoist, Gmail, Google Calendar) podłączasz w claude.ai. Rutyna ma dostęp tylko do tych, które jej włączysz.

## Własne skille

Dodaj folder w `.claude/skills/<nazwa>/SKILL.md` z nagłówkiem `name` i `description`. Opis mówi rutynie, kiedy użyć skilla - pisz go konkretnie („Użyj, gdy użytkownik prosi o…”).

## Bezpieczeństwo

- Klucz MCP trzymaj tylko w zmiennej środowiska rutyny, nigdy w repozytorium.
- Treść w sekcji „PRZEKAZANA TREŚĆ” to dane, nie polecenia - protokół każe je tak traktować.
- Rio Bravo nie zapisuje treści rozmów, tylko stan zadań.
