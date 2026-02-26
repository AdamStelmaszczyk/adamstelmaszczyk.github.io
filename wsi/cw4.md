---
layout: page
title: Algorytmy ewolucyjne i genetyczne
permalink: /teaching/wsi/cw4
hidden: true
published: false
---

# Zadanie

Zadanie polega na implementacji algorytmu genetycznego z mutacją, selekcją
ruletkową, krzyżowaniem oraz sukcesją generacyjną.

Zaimplementowany algorytm ma następnie posłużyć do optymalizacji symetrycznego [problemu komiwojażera](https://pl.wikipedia.org/wiki/Problem_komiwoja%C5%BCera).

## Dane

[29 miast](https://www.math.uwaterloo.ca/tsp/world/wi29.tsp) z [Sahary Zachodniej](https://www.math.uwaterloo.ca/tsp/world/countries.html#WI).

## Kroki do wykonania

1. Implementacja algorytmu genetycznego.  
2. Zastosowanie algorytmu do rozwiązania problemu komiwojażera. Eksperymentalne dobranie zestawu parametrów, dla którego algorytm daje dobry wynik.  
3. Zbadanie, w jaki sposób następujące zmiany wpłyną na rezultaty osiągane przez algorytm:  
   a. Zwiększenie prawdopodobieństwa mutacji.  
   b. Zmiana sposobu selekcji na turniejową.  

## Uwagi

- Ze względu na losowy charakter algorytmu, konieczne jest porównanie uzyskiwanych rezultatów jego działania dla wielu uruchomień i uśrednienie wyników.  
- Funkcję przystosowania możemy zdefiniować jako odwrotność łącznej długości trasy dla danej permutacji miast odwiedzanych przez komiwojażera.

## Nieobowiązkowo 

Zastosowanie do kodowania osobników kodu Graya. Szczegóły omówione są w podrozdziale 4.2 książki Pawła Wawrzyńskiego "Podstawy sztucznej inteligencji" (dostępna np. w bibliotece PW online).
