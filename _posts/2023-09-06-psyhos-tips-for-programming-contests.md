## Psyho's tips for programming contests

Translating the Polish wiki [page](https://pl.wikipedia.org/wiki/Przemys%C5%82aw_D%C4%99biak) to English, Psyho is:

> Polish computer scientist, multiple winner of international programming competitions, expert in algorithmics, artificial intelligence and data mining, member of Mensa.  
> He is also a multiple Polish puzzle solving champion and a Polish representative at the World Puzzle Championship and World Sudoku Championship.

I've recently stumbled across his Twitter [mega thread](https://twitter.com/FakePsyho/status/1605570944537280512), in which he posted many programming tips. Goldmine.

I wished I had all of them a bit cleaned up and consolidated into one page, for easier reading and further reference. Perhaps also many others would prefer a single article instead of a Twitter thread. Also creating such a page will help me in absorbing those tips. So here we go.

## Abbreviations

- CP = (Classic) Competetive Programming
- BC = Bot Contests
- HC = Heuristic Contests
- SA = Simulated Annealing

## Psyho's tips

0.  Everyone is unique & has different background. Each one of us struggles with different subjects. Not every piece of advice is going to be applicable for you, but I'll try making all tips as general as possible & mention if something is situation specific. 

1.  In a week-long contest there's enough to do ALL of the research during the contest. No matter how inexperienced you are, if you put a lot of effort into a single contest. You have a chance of winning it. This is in contrast to CP where you have to train for years.   

2.  You never need any advanced math. There is always alternative of writing simulation code. In ~200 contests I've done, I can recall a single instance someone performed better due to math knowledge. Exception: Bayes' theorem. For reference: I don't know calculus 😅   

3.  In HC there are only two common techniques that are worth knowing: Beam Search & Simulated Annealing. Rarely, it's useful to have decent experience with Graph Theory, Dynamic Programming, Linear Programming & Flow Problems.   

4.  Touch typing is extremely important. If you can't, that should be your #1 priority. It takes roughly 10-30h for most. Touch typing gives you an ability to think about other things while coding. Reduces number of bugs, smooths out prototyping, etc.   

5.  HC vs BC, which ones are better? Other than different set of standard techniques, the main difference is that HC give you fast & precise feedback, while BC are generally more fun. You'll learn at much faster pace in HC.   

6.  The best sites for contests:  

    HC: Topcoder (MM, Marathon Match), AtCoder (AHC, AtCoder Heuristic Contest)  
    BC: CodinGame
    
    All sites feature good problems, fair evaluation system & community that talks about solutions post-contest. Some sites might be rough on the UI side ;)

8.  In HC, Googling about the problem is generally a waste of time. Good problem design (above sites) means that problem writers made sure that the problems are unique. There are rare exceptions; it might be useful to research a particular subproblem that exist.

9.  For most people, the most limiting skill is prototyping speed. With some experience, you'll always have dozens of ideas that you want to implement. The limiting factor is how quickly can you iterate through them. The faster you go, the quicker you learn about the problem.

10.  Best way to get good quickly is to focus on a single contest instead of half-assing many. You won't learn much by only doing basic solutions. The proper learning period starts when you hit a wall and don't know how to proceed. You have to push yourself to get results.

11. All local-search alternatives to SA are (almost always) bad. Genetic Algorithms, Tabu Search, Ant Colony, etc. Just forgot them. They are not only less effective, but slower to implement.

12. Why SA?
    
    - You can start with Hill Climbing and convert to SA with 2 additional lines.
    - Gives you ability of fast dynamic evaluations.
    - When tweaked properly has higher potential (due to fast evaluations) than other methods.
    - Allows for very fast iterations.
       
14. Standard SA:
    
    Temperature schedule: `t = t_start * pow((t_final / t_start), time_passed)`, where `time_passed` is in `0..1`.  
    Acceptance (when lower result is better): `RNG() < exp((cur_result - new_result) / t)`, where `RNG()` returns `0..1` uniformly.
    
    It's the best starting point in almost all cases.

15. Good SA temperature schedule is such where the acceptance rate slowly decreases. If it's stuck on the same acceptance rate value for a long time, it won't be effective.

16. Things to do when it's easy to stuck in a local optimum:
    
    - very high temperatures / acceptace rate
    - complex transition states
    - (rarely) kicks
    - (rarely) soft restarts (restarting temperature schedule without solution)
    - (rarely) multiple runs  

17. Things to do when a lot of states are invalid (due to restrictions):

    - add an additional scoring component based on number of collisions that has initial weight 0 and a final weight of infinity
    - (if possible) do many short SA runs with high temperature on a partial subsolution.

18. Things to do when your eval is very fast:
    
    - make sure your RNG is not a bottleneck
    - make sure your exp function is not a bottleneck
    - make sure your function that gets current time is not a bottleneck  

    That happens way more often than you may think.

19. Overall priorities for SA:
    
    - prototype different transition functions
    - make evaluation function very fast (= make it dynamic)
    - find proper temperatue schedule (usually 3-4 runs are enough)
    - test a lot
    - only when you're done with the above move to other things

20. Make sure that you constantly update your best result and return your best instead of the final one. I can't tell how many times I saw this mistake. Constantly updating your best results allows you to have a much higher final temperature.

21. When SA is not great:
    
    - it's very easy to get stuck in local optima: check #14 for hints
    - you mix simple transitions with complex ones: have a separate acceptance function for each transition
    - your evaluation is very erratic during transitions: you're out of luck
   
22. Contests are essentially simple feedback loops:
    
    a) do stuff  
    b) get results  
    c) extract information  

    You can get better by improving any of those areas. Testing is the most neglected aspect of contests by almost everyone. It is your way of getting feedback. If your testing is slow you're going to progress very slowly. If your testing is inaccurate, it's going to be very hard to figure out correct next steps. This will mostly focus on HC, as testing in BC is very hard and quite often nearly unfeasible.

23. Use a local tester. If you're only using provisional testing for evaluation, you're making everything extremely hard for yourself. You're not saving time, you're wasting it.

24. Use a good local tester. The absolute minimum are two functionalities:
    
    - multithreading (ability to run multiple tests on separate threads)
    - relative scoring (which is a standard on topcoder)  
     
    If you don't have your own, use [mine](https://github.com/FakePsyho/psytester).

25.  Testing speed is often a limiting factor. When your solution is compact and easy to extend, you can add/remove features in matter of seconds. That means at some point in the contest, testing different versions of your solutions is going to be main bottleneck.

26. Use external VMs for quick testing. If you don't have a powerful machine, you can use AWS/GCP/Azure for quick testing. It takes around 1-2h to learn if you have linux knowledge. With spots, for 5$ you can get an equivalent of your laptop running for a week.

27. Run your tests as often as you can. You have a built-in end to end testing coming with your contest. Testing is your magical command that detects that you've made a mistake somewhere. It also gives you feedback on every possible incremental change you're able to add.

28. Version control is bad. You may be very experienced with git and you may feel that it doesn't slow you down. And that might be true. That being said, if you feel it's useful for you then you're bad at prototyping. Fix your prototyping skills.

29. Version control is bad. Except when running tests. When you run tests, it's useful to save your current source files. This mostly functions as a sanity-check when you get unexpected results. Just a make short .sh/.bat script that saves source files when testing.

30. If you're ever wondering if it's worth optimizing your code, run your solution while extending the time limit. This is a simple trick that gets used very often. Don't optimize your code if potential gains are very low.

31. Language choice: C++ is the lone king. If you know what you're doing you can squeeze much more out of C++ than any other language, while macros can fix many of C++'s problems. I don't have experience with rust + it's a very complex topic, so don't take this as the golden standard.

32. You should prioritize removing bugs over anything else. When you have bugs, your solution doesn't work the way you believe it does. This means that every time your tests run, you're getting incorrect results. Your intuition for the problem won't properly develop.

33. Every time you suddenly get results that do not align with your intuition you should investigate. Either you have a bug (see above) or your intuition is off. For latter, you should treat this as a learning experience and figure out why you were wrong.

34. Try to keep your solution small. More code means:
    
    - more bugs
    - remembering full solution is much more mentally demanding
    - (usually) more dependencies -> costly refactoring
    Most of the winning solutions are around 400-1000 lines of code.

35. Read editorials after every contest you participate in. In all of the platforms I have mentioned, (nearly) all of the top competitors describe their solution after the contest is finished. It's an incredible waste of opportunity to compete and then not read what others did.

36. The obvious one first. It takes time to develop a good solution. There's no magic trick or workaround. Over time, you'll get faster though.

37. Execution > Idea. You may wonder, why you often find many different approaches at the top, but all of them achieve similar scores. Executing your idea well is generally way more important than having the perfect idea.

38. Reread the problem statement until you remember every small detail of it. Read the tester code as well. There are often small details that you may have missed on your first reading. Read the supplied code as well, especially if it involves non-trivial logic.

39. Find what motivates you. The biggest driving factor for most competitors is the joy of self-improvement. HC & BC are mostly a solitary game where future you tries to be better than past you. Treat others competitors as a benchmark and not as your goal.

40. Don't waste your time on solutions that can't get you far. If you know that the current solution is not able to achieve good results, there's no point in squeezing maximum value out of it. The easiest way to end up at the top of the leaderboard is to aim high and fail.

41. Tools increase your productivity. By a lot. For HC it's mainly a local tester. For BC it's a local league system (although it's less reliable). My estimate is that after polishing my local [tester](https://github.com/FakePsyho/psytester). I'm spending around 30-40% less time on contests.

42. Good competitors never run out of ideas. You can get more inspiration by running tests, looking at the visualizer / games. You get ideas by working on your solution. At the start, even the best people have no clue what's the best solution is going to be.

43. CP and HC/HB require completely different skills. CP is a sprint, requires insane concentration and a large knowledge base. HC/BC are marathons, it's mostly long-term planning and creative thinking/problem solving.

44. In CP, your "opponent" is the problem setter. In HC/BC (but mostly HC) your opponent is the past you. In HC/BC you're never done with the problem and solving is an ongoing process. You have to fit your life between the coding sessions. It's very hard to switch between problems.

45. In CP you care about the worse case. In HC/BC you care about the average case. This opens a complete new class of techniques:
    
    - randomized algorithms
    - different versions for quickly handling special cases
    - pruning (early exits)

46. Being experienced in CP gives you a solid foundation for HC/BC. While algorithmic knowledge from CP is only somewhat useful, CP is wonderful for practicing your programming language & writing bug-free code as it provides very quick iteration cycles.

47. CP will land you a job, HC/BC will make you good at your job 😁 A lot of companies use programming puzzles during hiring process, but beyond that CP isn't that useful. HC/BC contests are similar to small-scale research that you may often encounter if you join any AI company.

48. If you're very new to the programming I would not recommend HC/BC contests. Try CP first, learn syntax of your language, learn simple algorithms like BFS/DFS, 1D DP etc. You need faster feedback cycle in order to quickly go through basics.

49. Find a contest, reserve some time for it and try to do your best. Most probably you'll fail and that's ok. Failing is how you learn and winning teaches you nothing. Make a mental note of all the reasons why you think you didn't perform well. Find spots where you struggle.

50. It's useful to read problems even if you don't compete in them. When you see a new contest that looks familiar to the one you've seen before, read all of the editorials and see if anything could be applied. By far the easiest way to perform well with little experience.

51. At the start of the contest focus on experimenting with (conceptually) simple solutions. Your goal should be to develop your intuition about the problem, not to score some quick points. Quick adrenaline boost is nice, but gets you nowhere in the long term.

52.  Good way of evaluating potential approaches is to try to think about what kind of steps do they have to perform in order to end with a good answer. If you believe it's not possible or very improbable then probably it's not a good approach.

53. Focus on small incremental changes. Avoid rewriting your solution. Small incremental changes = fast feedback cycle, fewer bugs, better motivation. For the record, I never rewrite my solution and rarely I'm going for longer than 10-15 minutes without testing it.

54. Code optimization can turn bad solution into a great one. With most iterative approaches (like SA) you need some critical number of iterations/steps before that approach yields good results. Sometimes code optimization opens up a completely new possibility.

55. If you feel you're getting inconsistent results from your tests/games, it's usually a tell to increase the number of tests/games. While your solution improves, you'll require more tests in order to have good testing accuracy. It's not uncommon to use 2-5K tests per run.

56. Don't use python for contests. Python is great for some uses, but it's roughly 100x slower for normal operations. You'll have to get very creative with coding/algorithms in order to get reasonable speed which means your code is going to be much more complex. Never worth it.

57. Believe in yourself. It's very hard to stay motivated if you'll just blindly assume that you won't perform well and it's out of your reach. I met many people that suddenly performed well just because they changed their mindset.

58. There's no theorycrafting with code optimization. You have to run code on your target machine and measure it. Different compiler version, different OS, different processor have huge effect.

59. Merge N-dimensional coordinates into 1D. This is very common. Imagine you have 2D grid and you perform BFS on it. Instead of using (row, column), merge them into a single value. Reduces memory footprint, number of operations & code itself. It's usually a 10-30% speedup.

60. Memory allocation is extremely slow. Unless you're experienced with memory pooling, you generally want to have all your arrays allocated only once. Function with O(N) time & memory complexity can be up to 10-50x slower if you reallocate memory each call.

61. The only data structure that you really need is an array. It's very rare that you can't replace all your data with simple arrays. That being said, data structure optimization can be considered pre-mature optimization, since it can increase complexity.

62. Modulo is surprisingly slow. Switching:
    ```
    x = (x + y) % m
    ```
    to
    ```
    x = x + y
    x -= (x >= m) ? m : 0
    ```
    is quite often faster. Assuming `y < m, y > 0, m > 0`.
   
    Citing [https://twitter.com/cdkrot](https://twitter.com/cdkrot): 
   
    > Because addition/subtraction is in AC0, and modulo is not. To say it in a less sophisticated way, the circuit for addition is constant-depth and also much simpler. Which is important given that your processor literally does math via in-built circuits. 

    Integer division has roughly ~100x longer latency than `add`/`sub` for Intel Skylake, [http://gmplib.org/~tege/x86-timing.pdf](http://gmplib.org/~tege/x86-timing.pdf).

    Ternary operator `?:` will not cause branch prediction miss, because it is compiled to a conditional move, `cmov`, which has latency similar to `add` or slightly worse (but not 100x worse).

64. Pre-compute everything you can and store those values in arrays. It's not always better, but some operations might be slower than you think. For example, precalculating `pow(x, y)` is quite often faster than calculating it on the fly.

65. For large arrays, use smaller variable types if it's enough. Memory copying/clearing only cares about the number of bytes. This also improves overall cache efficiency. Don't do it for single variables.

66. Use "Fast-Clearing Array". Imagine you need a boolean array with 3 different operations:
    ```c
    int get(i)
    void set(i, value)
    void clear() // clears full array
    ```
    Naive implementation is O(1) + O(1) + O(N), but you can use int array with a threshold (`clear` will be `threshold++`) to make it O(1) + O(1) + O(1).

67. Extend "Fast-Clearing Array".

    - If each clear increases your threshold by `RANGE`, you can store values in `0..RANGE-1`.
    - If collisions are acceptable, in many cases this can replace your sets/maps.  
  
        ```c
        int RANGE = 1000;
        int threshold = 0;
        int a[N] = {0,};
        
        int get(int i) {
           return a[i] >= threshold ? a[i] - threshold : 0;
        }
        
        void set(int i, int value) {
           if (value > RANGE) {
               throw std::invalid_argument("Value greater than RANGE")
           }
           a[i] = value + threshold;
        }
        
        void clear() {
           if (threshold >= INT_MAX - RANGE) {
               throw std::runtime_error("Threshold overflow")
           }
           threshold += RANGE;
        }
        ```

68. Adding special guardians/boundaries in your graphs can reduce the number of operations during pathfinding. For example, when you run BFS on 2D grid, you can extend the grid by 1 in all directions to avoid checking for boundary condition (stepping outside of grid).

69. The most important speedup you'll commonly perform is to make your algorithm dynamic. For example if you apply SA to Traveling Salesman Problem and your transition is move a city to another point in path, your evaluation should be O(1), because you only care about delta.

70. Measuring time is (often) costly. The fastest way is to call `rdtsc`, but some sites use different way of measuring time. Commonly used `gettimeofday()` in C++ is a system call and on some platforms those are very expensive. There was one time when 1K calls took over 1s.

71. Use pruning / early exits / simplified calculations. This doesn't happen very often, but there are cases where you simply skip some calculations. This mostly happens with your evaluation function where some transitions completely ruin your score and you can quickly tell that.

72. If you're doing very heavy code optimization mirror the "judge" machine. Having the exact version of gcc is way more important than anything else. That being said, I generally don't advise fighting for <5% speed ups. Parameter optimization is way easier.

73. SIMD/auto-vectorization is very powerful. I never really use this nowadays, but it's still a powerful tool. Using SSE/AVX instruction set feels like solving mini puzzles. It's easy to outperform the best compilers, and you can get 2-20x speed up in critical places.

74. You can (mostly) override compiler flags via `#pragma GCC optimize (...)`. I have `Ofast,omit-frame-pointer,inline,unroll-all-loops`. In theory, `omit-frame-pointer` is included in -O1, but it was faster 🤷 You can do HC on compiler flags 😅

75. There's a new concept in BC: metagame. What is the best strategy often depends on the dominating strategies of other players. This is especially problematic in games with imperfect information. Forget about decent evaluation of your solution.

76. Each "test" is essentially a single 2-player game with binary outcome. Either you win or lose, no matter how big your advantage/disadvantage was. Forget about decent evaluation of your solution.

77. In order to be able to perform viable local tests, you need a wide spectrum of different approaches that's representative of bots on the leaderboard. Forget about decent evaluation of your solution.

78. Remember tip 20? Contests are feedback loops. When you can't effectively evaluate your solution, it's hard to quickly develop your intuition. In BC you get a lot of noisy feedback and you have to make very wild guesses about the next steps.

79. BC require much bigger time investment than heuristic contests.

    - very weak feedback mechanism
    - constantly evolving metagame
    - access to all replays means that everyone has access to how your bot plays (and they can counter it
    - (often) more complex rules
  
80. Due to interactivity they tend to be way more fun than HC. Despite that, I generally don't recommend BC as a good learning platform for newcomers. Feedback loop is ineffective so it's harder to associate your actions with the outcome.

81. Most of the skills & wisdom gained from HC are applicable to BC. Prototyping. Good workflow. Code optimization. Many of the algorithms & techniques are still applicable (beam search is still useful).

82. Most of the techniques fall into two types: complex state evaluation functions (aka heuristics) and search (beam search, rollouts, MCTS). It highly depends on the problem which one is more viable, but you have to master both to succeed. Extreme form of state evaluations are neural networks.

83. Because the evaluation is very noisy it's often a good idea to ignore it altogether and focus on a solution that you believe will work. Search-based solutions tend to be more metagame agnostic. You can also develop a local league system for automatic state evaluation adjustments. I've been working on a simple local league [system](https://github.com/FakePsyho/psyleague) that would do automatic matchmaking. Somewhat similar tool to [psytester](https://github.com/FakePsyho/psytester) but for bot contests.
    
84. CodinGame specific: Usually time limit for the first turn is 1s, while it's only 50ms for future turns. It's very often useful to use the first turn to "think" for the next 2-3 turns if opponent have very limited actions.

85. Chasing current metagame and imitating top players might give you good placement, but won't teach you too much in the long-term. If your goal is self-improvement, try avoiding those approaches.
