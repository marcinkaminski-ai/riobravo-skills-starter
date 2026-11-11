---
name: sprawy
description: Prowadzenie spraw trwających dni w Rio Bravo (case_open, case_update, case_close, case_list) i obsługa follow-upów. Użyj, gdy zadanie wymaga czekania na kogoś lub powrotu za jakiś czas (np. "umów spotkanie z...", "przypilnuj odpowiedzi od..."), gdy właściciel pisze o postępie istniejącej sprawy, albo gdy wejście ma TYP: follow_up.
---

# Sprawy

Sprawa to zadanie, które nie skończy się w jednym jobie. Rio Bravo zapisuje tylko **cel i status** (cel zaszyfrowany), pilnuje terminów i przypomina właścicielowi, gdy sprawa utknie:

- po dobie bez postępu (gdy czekasz na osobę trzecią) pyta właściciela, czy ponaglić,
- po 3 dniach przypomina,
- po 7 dniach oznacza sprawę jako nieaktualną.

W ciszy nocnej właściciela nic nie wysyła.

## Otwieranie

`case_open(job_id, reply_token, goal, next_check_in_hours?, waiting_for?)`

- `goal` - krótki cel, który zrozumie właściciel za tydzień: „Umówić spotkanie z Anną Kowalską (oferta SEO)”. Bez danych wrażliwych ponad potrzebę.
- `next_check_in_hours` - kiedy chcesz wrócić do sprawy (np. 24). Wtedy dostaniesz job `TYP: follow_up`.
- `waiting_for` - `external` (czekasz na osobę trzecią, domyślnie) albo `owner` (czekasz na decyzję właściciela).

Potem zakończ bieżący job: `reply` z informacją, co zrobiłeś i na co czekasz.

## Postęp

`case_update(job_id, reply_token, case_id, status, next_check_in_hours?, note?)`

- `status`: `waiting_external`, `waiting_owner` albo `scheduled` (termin ustalony, czekasz na wydarzenie).
- `note` - jedno zdanie stanu („Anna zaproponowała wtorek 14:00, czekam na potwierdzenie właściciela”). Trafi do kontekstu następnego follow-upu.
- Każdy `case_update` zeruje przypomnienia - wołaj go tylko przy realnym postępie.

Właściciel pisze o sprawie w zwykłej wiadomości („Anna odpisała”)? Znajdź ją przez `case_list` i zaktualizuj.

## Follow-up (`TYP: follow_up`)

Wejście ma sekcję `KONTEKST SPRAWY` z `CASE_ID`, celem, statusem i ostatnią notatką.

1. Sprawdź postęp dostępnymi konektorami (np. czy przyszła odpowiedź w Gmailu, czy w kalendarzu jest wydarzenie).
2. Jest postęp → `case_update` (nowy status, termin, notatka) albo `case_close`, jeśli cel osiągnięty.
3. Brak postępu → `case_update` z nowym `next_check_in_hours` tylko jeśli warto sprawdzić ponownie. Bez tego Rio Bravo poprowadzi przypomnienia sam.
4. Polecenie mówi „Właściciel prosi o ponaglenie” → wyślij uprzejme przypomnienie osobie trzeciej (zgodnie z `preferencje.md`), potem `case_update`.
5. Zakończ job: `reply` tylko jeśli właściciel powinien coś wiedzieć, inaczej `done`.

## Zamykanie

`case_close(job_id, reply_token, case_id, outcome?)` - wynik przekaż właścicielowi w `reply`, Rio Bravo go nie zapisuje.
