## Psyho's tips for programming contests

Translating the Polish [wiki](https://pl.wikipedia.org/wiki/Przemys%C5%82aw_D%C4%99biak) page to English, Psyho is:

> Polish computer scientist, multiple winner of international programming competitions, expert in algorithmics, artificial intelligence and data mining, member of Mensa.  
> He is also a multiple Polish puzzle solving champion and a Polish representative at the World Puzzle Championship and World Sudoku Championship.

I've recently stumbled across his Twitter [mega thread](https://twitter.com/FakePsyho/status/1605570944537280512), in which he posted many programming tips. Goldmine.

I wished I had all of them consolidated into one page, for easier reading and further reference. Perhaps also many others would prefer a single article instead of a Twitter thread. Also creating such a page will help me in understanding those tips. So here we go.

## Abbreviations

BC = Bot Contests  
CP = Competetive Programming  
HC = Heuristic Contests  
SA = Simulated Annealing

## Psyho's tips

0. Everyone is unique & has different background. Each one of us struggles with different subjects. Not every piece of advice is going to be applicable for you, but I'll try making all tips as general as possible & mention if something is situation specific.
1. In a week-long contest there's enough to do ALL of the research during the contest. No matter how inexperienced you are, if you put a lot of effort into a single contest. You have a chance of winning it. This is in contrast to [classical] CP contests where you have to train for years.
2. You never need any advanced math. There is always alternative of writing simulation code. In ~200 contests I've done, I can recall a single instance someone performed better due to math knowledge. Exception: Bayes' theorem. For reference: I don't know calculus 😅
3. In HC there are only two common techniques that are worth knowing: Beam Search & Simulated Annealing. Rarely, it's useful to have decent experience with Graph Theory, Dynamic Programming, Linear Programming & Flow Problems.
4. Touch typing is extremely important. If you can't, that should be your #1 priority. It takes roughly 10-30h for most. Touch typing gives you an ability to think about other things while coding. Reduces number of bugs, smooths out prototyping, etc.
5. HC vs BC, which ones are better? Other than different set of standard techniques, the main difference is that HC give you fast & precise feedback, while BC are generally more fun. You'll learn at much faster pace in HC.
6. The best sites for contests:

    HC: Topcoder (MM, Marathon Match), AtCoder (AHC, AtCoder Heuristic Contest)  
    BC: CodinGame

    All sites feature good problems, fair evaluation system & community that talks about solutions post-contest. Some sites might be rough on the UI side ;)

7. [HC] Googling about the problem is generally a waste of time. Good problem design (above sites) means that problem writers made sure that the problems are unique. There are rare exceptions; it might be useful to research a particular subproblem that exist.

8. For most people, the most limiting skill is prototyping speed. With some experience, you'll always have dozens of ideas that you want to implement. The limiting factor is how quickly can you iterate through them. The faster you go, the quicker you learn about the problem.

9. Best way to get good quickly is to focus on a single contest instead of half-assing many. You won't learn much by only doing basic solutions. The proper learning period starts when you hit a wall and don't know how to proceed. You have to push yourself to get results.

10. All local-search alternatives to SA are (almost always) bad. Genetic Algorithms, Tabu Search, Ant Colony, etc. Just forgot them. They are not only less effective, but slower to implement.

11. Why SA?
    - You can start with Hill Climbing and convert to SA with 2 additional lines.
    - Gives you ability of fast dynamic evals.
    - When tweaked properly has higher potential (due to fast evels) than other methods.
    - Allows for very fast iterations.  

12. Standard SA:
    
    temp schedule: `t = t_start * (t_final / t_start) ^ time_passed`, where `time_passed` is in `0..1`.  
    acceptance (when min is better): `RNG() < exp((cur_result - new_result) / t)`, where `RNG()` returns `0..1` uniformly.
    
    It's the best starting point in almost all cases.
