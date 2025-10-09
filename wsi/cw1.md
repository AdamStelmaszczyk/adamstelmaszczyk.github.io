---
layout: page
title: Algorytmy ewolucyjne i genetyczne
permalink: /teaching/wsi/cw1
hidden: true
---

# Zadanie

Zadanie polega na implementacji algorytmu genetycznego z mutacją, selekcją
ruletkową, krzyżowaniem jednopunktowym oraz sukcesją generacyjną.

Zaimplementowany algorytm ma następnie posłużyć do optymalizacji klasycznego, symetrycznego [problemu komiwojażera](https://pl.wikipedia.org/wiki/Problem_komiwoja%C5%BCera).

## Dane

| Miasto | Współrzędne |
|--------|-------------|
| A      | (0, 0)      |
| B      | (1, 3)      |
| C      | (2, 1)      |
| D      | (4, 6)      |
| E      | (5, 2)      |
| F      | (6, 5)      |
| G      | (8, 7)      |
| H      | (9, 4)      |
| I      | (10, 8)     |
| J      | (12, 3)     |

## Kroki do wykonania

1. Implementacja algorytmu genetycznego.  
2. Zastosowanie algorytmu do rozwiązania problemu komiwojażera. Eksperymentalne dobranie zestawu parametrów, dla którego algorytm daje dobry wynik.  
3. Zbadanie, w jaki sposób następujące zmiany wpłyną na rezultaty osiągane przez algorytm:  
   1. Zwiększenie prawdopodobieństwa mutacji.  
   2. Zmiana sposobu selekcji na turniejową.  

## Uwagi

- Ze względu na losowy charakter algorytmu, konieczne jest porównanie uzyskiwanych rezultatów jego działania dla wielu uruchomień i uśrednienie wyników.  
- Funkcję przystosowania możemy zdefiniować jako odwrotność łącznej długości trasy dla danej permutacji miast odwiedzanych przez komiwojażera.  
- Interesującym, choć **nieobowiązkowym** dodatkiem jest zastosowanie do kodowania osobników kodu Graya. Szczegóły omówione są w podrozdziale 4.2 książki Pawła Wawrzyńskiego "Podstawy sztucznej inteligencji" (dostępna np. w bibliotece PW online).
