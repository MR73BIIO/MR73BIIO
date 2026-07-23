[English](README.md) · [**Polski**](README.pl.md)

# strategy-validation

**Autoaudyt własnej strategii tradingowej. 256 wariantów parametrów. Zero wdrożonych.**

Potok walidacyjny zbudowany po to, żeby strategię obalić, a nie potwierdzić. Przepuściłem przez
niego regułę wejścia kierunkowego z własnego bota na perpetualach Hyperliquid — cztery symbole,
cztery reżimy rynku. Każdy odrzucony. Nic nie poszło na żywo.

To repozytorium jest zapisem tego procesu.

---

## Problem

Backtest pokazuje zyski, live traci.

Przeszukaj siatkę kilkudziesięciu kombinacji parametrów, a zawsze znajdzie się jedna z ładną
krzywą kapitału. Pytanie brzmi, czy to przewaga, czy najlepiej dopasowany szum. Bez korekty na
sam fakt przeszukiwania jedno od drugiego nie różni się niczym — a większość publikowanych
backtestów tej korekty nie robi.

## Metoda

Cztery bramki. Kandydat musi przejść wszystkie.

| Bramka | Próg | Dlaczego |
|---|---|---|
| Próba out-of-sample | ≥ 100 transakcji | poniżej tego t-stat nic nie znaczy |
| Edge po opłatach | > 0 | próg opłat round-trip 0,0258% (maker+maker) |
| Istotność statystyczna | t ≥ 2,0 **na teście** | nie na zbiorze, który przeszukiwanie optymalizowało |
| Przetrwanie sąsiedztwa | ≥ 60% | prawdziwa przewaga to plateau, nie szpilka |

Do tego korekta Bonferroniego na wyniku treningowym — bo argmax z 64 kombinacji produkuje
fałszywych zwycięzców nawet na czystym szumie.

Dwie decyzje projektowe, które zmieniły wynik:

- **Bariery liczone wobec realnego high/low świecy**, nie wobec symulowanego knota. Wcześniejsza
  wersja używała symulowanego zakresu wewnątrz świecy około 21× za wąskiego dla świecy 4h — i to
  samo w sobie zmieniło edge OOS na BTC z wyraźnie dodatniego na ujemny. Cała pozorna przewaga
  była artefaktem uproszczenia modelu.
- **Test sąsiedztwa jest bramką rozstrzygającą.** Jeśli zestaw parametrów działa, a jego
  bezpośredni sąsiedzi w siatce nie — to dopasowanie do historii. Trzy z czterech symboli
  poległy właśnie tutaj.

## Wyniki

Wejście kierunkowe (momentum). Dane z archiwum Binance, realne OHLC, podział train/test 50/50.

| Symbol | TF | Świece | Okres | Transakcje OOS | Edge OOS | t | p OOS | Sąsiedztwo | Werdykt |
|---|---|---|---|---|---|---|---|---|---|
| BTCUSDT | 4h | 10 950 | 2021–2026 | 136 | −0,0923% | −0,50 | 0,615 | 58% | ODRZUCONY |
| ETHUSDT | 2h | 13 140 | 2023–2026 | 908 | +0,0065% | +0,10 | 0,923 | 23% | ODRZUCONY |
| LINKUSDT | 2h | 13 140 | 2023–2026 | 1 039 | −0,0790% | −1,26 | 0,208 | 0% | ODRZUCONY |
| SOLUSDT | 4h | 4 380 | 2024–2026 | 230 | +0,3137% | +1,59 | 0,111 | 93% | ODRZUCONY |

**0 z 256 kombinacji przeszło wszystkie bramki.**

Dwa symbole pokazały dodatni edge poza próbką. Żaden nie przetrwał. Najbliżej był SOL — +0,31%
na transakcję przy 93% przetrwania sąsiedztwa — i poległ na istotności: t = 1,59, p = 0,11, na
najkrótszej historii w zestawie, przy czym okno testowe wypadło lepiej niż treningowe. Ten
wzorzec jest sygnaturą szumu, a nie przewagi, która się ukrywała.

Dla każdego symbolu p-value z treningu po korekcie Bonferroniego na 64 przeszukane kombinacje
wyniosło 1,0000. Inaczej mówiąc: nic, co znalazło przeszukiwanie, nie odróżniało się od tego,
co znalazłoby na danych losowych.

### Potwierdzenie z niezależnego źródła

Wcześniejszy sweep obejmujący 1728 konfiguracji na krótkich interwałach doszedł do tego samego
inną drogą. Średni wynik netto był ujemny dla każdego okna momentum (od −0,048% do −0,083% na
transakcję), a garstka „rentownych" konfiguracji okazała się artefaktem: identyczna liczba
transakcji i identyczny wynik przy różnych ustawieniach TP/SL, co oznacza, że bariery w ogóle
nie były aktywne. Dokładnie ten błąd wyłapuje teraz etap diagnostyczny tego potoku, zanim
ruszy walidacja.

Live to potwierdził. 134 round-tripy przez cztery dni: trafność kierunku 49%, gross płasko,
a cała strata netto to opłaty.

## Czego to repozytorium NIE twierdzi

- **Nie ma wyników live.** Nic nie przeszło walidacji, więc nic nie zostało wdrożone.
- **Nie ma cherry-pickingu.** Najlepszy z 256 przebiegów nie jest przedstawiany jako dowód.
- **Nie twierdzę, że strategia jest zła** — twierdzę, że dane nie pozwalają odróżnić jej od szumu.
- **To nie jest porada inwestycyjna.**

## Co dalej

Momentum kierunkowe jest dla tego bota obalone: na 1m–15m, na 2h i 4h, na czterech symbolach,
na żywo i pod modelem barier uwzględniającym opłaty.

Oczywista alternatywa — delta-neutralny arbitraż fundingu — została policzona, a nie założona.
Mediana fundingu na giełdzie leży dokładnie na bazie odsetkowej, co daje sufit ok. 10,9% APR
brutto i około 6,5% netto przy holdzie tygodniowym. Zmiana giełdy sufitu nie podnosi.
Realne, ale przy małym kapitale za cienkie, żeby na tym budować.

Czyli: szukam dalej. Taki jest uczciwy stan rzeczy.

## Stack

Python · NumPy · Hyperliquid Info API · archiwum klines z Binance

---

## Po co publikować wynik negatywny

Zbudowałem to, bo miałem strategię, w którą chciałem uwierzyć. Framework powiedział „nie" — i to
jest ta część warta pokazania. Narzędzie, które potrafi odrzucić pomysł własnego autora, jest
coś warte.

Robię ten audyt dla traderów, którzy chcą poznać tę samą odpowiedź o swoim systemie, zanim
postawią za nim kapitał.

**[mr73biio.github.io](https://mr73biio.github.io)** · ruszczak11@gmail.com

<sub>*Pulsus Cordis Tui*</sub>
