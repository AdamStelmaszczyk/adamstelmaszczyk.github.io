---
layout: page
title: Sztuczne sieci neuronowe
permalink: /teaching/wsi/cw5
hidden: true
---

# Zadanie

Zadanie polega na implementacji perceptronu dwuwarstwowego oraz nauczeniu go reprezentowania zadanej funkcji f(x), opisującej rozkład Laplace'a. Funkcja ta jest dana wzorem:

<p align="center">
  <img alt="Wzór Laplace'a" title="f(x) = \\frac{1}{2b} e^{-\\frac{|x-\\mu|}{b}}" src="https://latex.codecogs.com/svg.latex?f(x)=\frac{1}{2b}e^{-\frac{|x-\mu|}{b}}" />
</p>

- Zakres x: [-8, 8].
- Wartości mu i b: mu = 0, b = 1.

Zadanie realizowane jest w parach, dobieracie się Państwo sami. Każda osoba z pary powinna rozumieć całe rozwiązanie. Rozwiązanie przesyła jedna osoba, w raporcie podpisuje się para, odbiór też musi być w parach.

## Kroki do wykonania

1. Zaimplementuj perceptron dwuwarstwowy, który będzie reprezentował funkcję f(x) dla zadanego zakresu x oraz wartości mu i b.
2. Zbadaj jakość aproksymacji, obliczając Mean Squared Error (MSE) oraz Mean Absolute Error (MAE) między wartościami rzeczywistymi funkcji a wartościami przewidywanymi przez sieć.
3. Przedstaw wykres funkcji rzeczywistej oraz funkcji przewidywanej przez sieć.
4. Zbadaj, jak liczba neuronów w warstwie ukrytej wpływa na jakość aproksymacji, zmieniając jej wartość i porównując wyniki.

## Wskazówki

- Użyj funkcji aktywacji sigmoidalnej w warstwie ukrytej i metody gradientowej do znajdowania wag sieci.
- Zadbaj o odpowiedni dobór współczynnika uczenia oraz liczby iteracji uczenia.






