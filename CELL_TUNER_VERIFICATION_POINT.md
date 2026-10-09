# CELL Tuner Verification Point

## Cel
Zanim powstanie kolejny pełny build CELL Tunera, zweryfikować sam łańcuch uruchamiania SPRX i logowania. Overlay zostaje na później. Priorytet: dojść do stabilnego, mierzalnego punktu startowego dla overclockingu CELL.

## 1. VSH plugin inventory
Przed ładowaniem własnego SPRX:
- zrzucić wszystkie zajęte VSH plugin slots,
- zapisać nazwę/ścieżkę każdego pluginu,
- potwierdzić, gdzie siedzi webMAN MOD,
- nie zakładać na sztywno konkretnego slotu — znaleźć wolny slot automatycznie.

Powtórzyć test:
1. z webMAN MOD aktywnym,
2. po unloadzie webMAN MOD.

Jeżeli SPRX działa tylko bez webMAN-a, konflikt slotu/zasobów staje się głównym tropem.

## 2. Minimalny Boot SPRX Probe
SPRX ma robić tylko cztery punkty kontrolne:
1. Loader potwierdza załadowanie modułu + module ID + slot.
2. Pierwsza instrukcja `module_start` tworzy marker `START_ENTERED`.
3. Wynik `sys_ppu_thread_create` jest zapisany jako kod zwrotny.
4. Pierwsza instrukcja worker thread tworzy marker `WORKER_ALIVE`.

### Ważne
Nie używać wyłącznie jednego loggera jako źródła prawdy.
Każdy punkt kontrolny ma mieć co najmniej jeden niezależny marker, np. plik na HDD/USB + drugi kanał.

## 3. LAN logger timing
Jeżeli logger po LAN uruchamia się przy boot:
- `module_start`
- utworzenie workera
- `sleep 3–5 s`
- marker lokalny
- dopiero potem inicjalizacja loggera LAN

Cel: wykluczyć sytuację, w której SPRX działa, ale logger startuje zanim sieć jest gotowa.

## 4. Interpretacja wyników
- Moduł widoczny jako załadowany, ale brak `START_ENTERED` → problem na odcinku load → entrypoint / ABI / module start.
- `START_ENTERED` jest, ale brak `WORKER_ALIVE` → problem przy `sys_ppu_thread_create`, parametrach workera albo starcie wątku.
- Markery lokalne są, ale LAN nic nie pokazuje → problem loggera / timingu sieci, nie samego SPRX.
- SPRX działa bez webMAN-a, a z webMAN-em nie → zbadać konflikt slotu, pamięci lub hooków.

## 5. Dopiero po stabilnym SPRX
Dopiero gdy powyższe przechodzi powtarzalnie:
- wykonać jeden kontrolowany odczyt CELL,
- logować wejście/wyjście funkcji,
- porównać wartości z tym, co pokazuje webMAN MOD,
- nie dodawać jeszcze overlayu ani zapisu/zmiany zegara.

## Kryterium zakończenia
Verification Point jest zaliczony dopiero wtedy, gdy jedna minimalna wersja:
- ładuje się powtarzalnie,
- zostawia wszystkie markery start/thread,
- raportuje zajęte plugin slots,
- daje jasny wynik porównania z webMAN-em i bez niego,
- daje wiarygodny log jednego odczytu CELL.

Dopiero potem wracamy do faktycznego CELL Tuner / OC.
