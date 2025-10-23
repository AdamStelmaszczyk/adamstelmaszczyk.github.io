---
layout: page
title: Dwuosobowe gry deterministyczne
permalink: /teaching/wsi/cw2
hidden: true
---

# Zadanie

Zadanie polega na implementacji algorytmu min-max z przycinaniem alfa-beta 
i zastosowaniu go do gry w kółko i krzyżyk (tic-tac-toe). 
Do rozwiązania zadania można wykorzystać gotową implementację gry w kółko
i krzyżyk: [ttt.zip](/teaching/wsi/ttt.zip). 
Gra ta posiada prosty interfejs tekstowy, zaimplementowanego gracza "ludzkiego" 
i losowego, jak również zaślepkę dla gracza wykorzystującego algorytm min-max.

## Kroki do wykonania

1.  Uruchomienie gry dla gracza losowego i ludzkiego (w celu zapoznania
    się ze sposobem działania gry).
2.  Implementacja algorytmu min-max z przycinaniem alfa-beta. Ocena
    gracza min-max (porównanie jego zachowania w starciu z graczem
    ludzkim w stosunku do gracza losowego).
3.  Rozszerzenie gry o możliwość konfiguracji różnych poziomów
    głębokości drzewa przeszukiwań dla dwóch graczy min-max.
    Uruchomienie gry dla dwóch graczy min-max o różnej głębokości drzewa
    przeszukiwań (dla kilku wartości tego parametru) i skomentowanie
    wpływu głębokości przeszukiwania drzewa gry na jakość wyników
    uzyskiwanych przez graczy.

Punkty 2 i 3 proszę wykonać dla standardowego rozmiaru planszy (3x3),
jak również dla jednego wybranego większego rozmiaru.
