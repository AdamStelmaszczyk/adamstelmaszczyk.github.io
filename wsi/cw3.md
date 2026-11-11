---
layout: page
title: Dwuosobowe gry deterministyczne
permalink: /teaching/wsi/cw3
hidden: true
published: true
---

# Zadanie

Zadanie polega na implementacji algorytmu Minimax z przycinaniem alfa-beta 
i zastosowaniu go do gry w kółko i krzyżyk. 
Do rozwiązania zadania można wykorzystać gotową implementację gry: [ttt.zip](/wsi/ttt.zip). 
Posiada ona prosty interfejs tekstowy, gracza "ludzkiego" i losowego, jak również zaślepkę dla gracza Minimax.
Poniższe kroki proszę wykonać dla dwóch rozmiarów planszy: 3x3 oraz 4x4.

## Kroki do wykonania

1.  Uruchomienie gry dla gracza losowego i ludzkiego (dla zapoznania
    się).
2.  Test-driven development (TDD). Najpierw napisanie testów, które optymalnie grający Minimax powinien przechodzić. [Przykład testów](https://github.com/AdamStelmaszczyk/gtsa/blob/master/cpp/tests/test_tic_tac_toe.cpp).
4.  Implementacja algorytmu Minimax. Przejście wszystkich testów. Dodanie przycinania alfa-beta, upewnienie się, że czas przeszukiwania się skrócił. Ponowne zaliczenie wszystkich testów automatycznych.
5.  Ocena gracza Minimax (porównanie jego zachowania w stosunku do gracza ludzkiego oraz losowego).
6.  Rozszerzenie gry o możliwość konfiguracji różnych poziomów
    głębokości drzewa przeszukiwań dla dwóch graczy Minimax.
7.  Uruchomienie gry dla dwóch graczy Minimax o różnej głębokości drzewa
    przeszukiwań (dla kilku wartości tego parametru) i skomentowanie
    wpływu głębokości na jakość gry.

## Nieobowiązkowo

Użyj Negamax zamiast Minimax.
