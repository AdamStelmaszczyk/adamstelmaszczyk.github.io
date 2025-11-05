---
layout: page
title: Dwuosobowe gry deterministyczne
permalink: /teaching/wsi/cw2
hidden: true
---

# Zadanie

Zadanie polega na implementacji algorytmu Minimax z przycinaniem alfa-beta 
i zastosowaniu go do gry w kółko i krzyżyk. 
Do rozwiązania zadania można wykorzystać gotową implementację gry: [ttt.zip](/wsi/ttt.zip). 
Posiada ona prosty interfejs tekstowy, gracza "ludzkiego" i losowego, jak również zaślepkę dla gracza Minimax.

## Kroki do wykonania

1.  Uruchomienie gry dla gracza losowego i ludzkiego (w celu zapoznania
    się ze sposobem działania gry).
2.  Implementacja algorytmu Minimax z przycinaniem alfa-beta. Ocena
    gracza Minimax (porównanie jego zachowania w starciu z graczem
    ludzkim w stosunku do gracza losowego).
3.  Rozszerzenie gry o możliwość konfiguracji różnych poziomów
    głębokości drzewa przeszukiwań dla dwóch graczy Minimax.
    Uruchomienie gry dla dwóch graczy Minimax o różnej głębokości drzewa
    przeszukiwań (dla kilku wartości tego parametru) i skomentowanie
    wpływu głębokości przeszukiwania drzewa gry na jakość wyników
    uzyskiwanych przez graczy.

Kroki 2 i 3 proszę wykonać dla standardowego rozmiaru planszy (3x3),
jak również dla jednego wybranego większego rozmiaru.

Polecam użyć Negamax zamiast Minimax, natomiast nie jest to obowiązkowe.
