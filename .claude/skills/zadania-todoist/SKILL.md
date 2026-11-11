---
name: zadania-todoist
description: Dodaje jedno lub kilka zadań do Todoista właściciela (termin, priorytet, projekt, etykiety, jeśli podano). Użyj, gdy właściciel prosi o dodanie zadania, przypomnienia lub "todo", np. "dodaj zadanie...", "wrzuć do todoista...", "zapisz mi na jutro...".
---

# Zadania w Todoist

Skill używa konektora **Todoist** (narzędzia `mcp__Todoist__*`). Jeśli są odroczone, załaduj je przez `ToolSearch` (`select:mcp__Todoist__add-tasks,mcp__Todoist__find-projects`). Brak konektora → `fail` z `user_message`: „Włącz konektor Todoist w ustawieniach rutyny w claude.ai.”

## Kroki

1. **Wyciągnij zadania z polecenia.** Dla każdego:
   - `content` - krótko, od czasownika („Zadzwonić do księgowej”),
   - `dueString` - termin po angielsku („tomorrow 9am”, „next friday”), tylko jeśli podano; licz od pola `CZAS`,
   - `priority` - `p1` tylko przy „pilne/ważne”, inaczej pomiń,
   - projekt i etykiety - tylko jeśli wymienione. Domyślny projekt: `preferencje.md`.
2. **Projekt.** Nazwa podana → `find-projects`. Brak dopasowania → dodaj do Inboxa i napisz o tym w odpowiedzi (nie twórz nowych projektów).
3. **Jedno wywołanie** `add-tasks` ze wszystkimi zadaniami w tablicy `tasks`.
4. **Odpowiedz** przez `reply`, jedną linijką: co dodałeś, z terminem.

## Zasady

- Nie zgaduj terminów ani priorytetów.
- Prośba jest jasna → dodaj bez dopytywania.

## Przykład

Polecenie: „dodaj: jutro o 9 wysłać fakturę klientowi X, pilne, i kupić mleko”

```json
{ "tasks": [
  { "content": "Wysłać fakturę klientowi X", "dueString": "tomorrow 9am", "priority": "p1" },
  { "content": "Kupić mleko" }
] }
```

Odpowiedź: „Dodane: Wysłać fakturę klientowi X (jutro 9:00, pilne) i Kupić mleko.”
