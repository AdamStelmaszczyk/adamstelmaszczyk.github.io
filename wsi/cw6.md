---
layout: page
title: Uczenie ze wzmocnieniem
permalink: /teaching/wsi/cw6
hidden: true
---

# Zadanie

Zadanie polega na implementacji algorytmu Q-learning oraz zastosowaniu go do rozwiązania problemu [Cliff Walking](https://gymnasium.farama.org/environments/toy_text/cliff_walking/). 
Środowisko to jest dostępne w pakiecie [gymnasium](https://pypi.org/project/gymnasium/): `gym.make('CliffWalking-v0')`.

## Kroki do wykonania

1. Implementacja algorytmu Q-learning.
2. Zbadanie skuteczności działania algorytmu dla problemu [Cliff Walking](https://gymnasium.farama.org/environments/toy_text/cliff_walking/) dla różnych wartości współczynnika uczenia i różnej liczby epizodów (w procesie trenowania).

## Uwagi

Implementacja algorytmu powinna być uniwersalna, tzn. możliwa do wykorzystania dla różnych środowisk o dyskretnej przestrzeni stanów i akcji.
