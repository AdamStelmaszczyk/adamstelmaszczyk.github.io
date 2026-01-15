---
layout: page
title: Modele bayesowskie
permalink: /teaching/wsi/cw7
hidden: true
---

# Zadanie

Zadanie polega na skonstruowaniu sieci bayesowskiej oraz zbadaniu wpływu informacji o statusie w komunikatorze na prawdopodobieństwo świecenia się światła w pokoju pracownika.

## Scenariusz

Pracownik spędza 40% czasu pracy w swoim pokoju na uczelni. Pozostałe 60% czasu pracuje zdalnie. Kiedy jest w swoim pokoju, połowę czasu ma wyłączone światło. 
Gdy nie ma go w pokoju, zostawia włączone światło tylko w 5% przypadków. 
80% czasu, gdy jest w swoim biurze, jest zalogowany na komunikatorze. Ponieważ czasami loguje się z domu, w 5% przypadków, gdy nie ma go na uczelni, nadal jest zalogowany na komunikatorze.

## Kroki do wykonania

1. Skonstruuj sieć bayesowską, aby przedstawić opisany scenariusz.
2. Załóżmy, że sprawdzamy status pracownika na komunikatorze i widzimy, że jest zalogowany. Jaki wpływ ma to na nasze przekonanie, że światło w pokoju pracownika jest włączone?

## Uwagi

- Do implementacji można użyć np. biblioteki [pgmpy](https://pgmpy.org/).

## Źródło

Powyższe zadanie jest parafrazą problemu nr 3, rozdz. 2 z książki *Bayesian Artificial Intelligence* autorstwa Kevina B. Korba i Ann E. Nicholson, CRC Press, 2010.
