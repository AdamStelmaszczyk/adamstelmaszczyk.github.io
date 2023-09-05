## Our NIPS 2017: Learning to Run approach

For 3 months, from July to 13 November, me and Piotr Jarosik participated in the [NIPS 2017: Learning to Run](https://www.crowdai.org/challenges/nips-2017-learning-to-run) competition. In this post we describe how it went. We release [the full source code](https://github.com/AdamStelmaszczyk/learning2run).

**tl;dr** 22nd place in the end, the final skeleton has a cheerful gait, we used PPO trained on 80 cores in a couple of days with manually prepared observation vector + a bit of reward hacking. The final result:

![](https://cdn-images-1.medium.com/max/2000/1*-ElNljBI2OmoqPJ8mO8GQw.gif)

## Challenge overview

Competitors were given a model of a human skeleton and OpenSim simulator. The task was to write a program that activates legs muscles in order to maximize the number of meters passed in 1000 timesteps. In the reinforcement learning setting, one would say that an action had 18 float values from 0 to 1, corresponding to muscle activations:

![Action, 18 muscle activations. Infographic made quickly for our purposes, so the lines pointing to the muscles might be wrong](https://cdn-images-1.medium.com/max/2174/1*o5o1M7__pT9lOL5ez2ehMw.png)

Actions were taken depending on the current observation (positions, velocities and so on):

![Observation, 41 floats. This infographic was also made in the beginning of the competition, bug marked with * was fixed](https://cdn-images-1.medium.com/max/2680/1*FO-Md8t9w8RdHp44ADDMqg.png)

A typical program would read the initial observation and output an action, activating some muscles. The simulator would update its state and return the observation for the next timestep + a reward (distance passed) for the previous timestep. So the learning algorithm received feedback if it did good or bad. Based on that, it updated a value/policy function approximations, neural networks. The simulation ended if pelvis.y dropped below 65 cm (assuming the skeleton has fallen) or after 1000 timesteps. The final score was the number of meters pelvis.x moved from the initial position.

## How we did

Round 1 finished on 4 November, our place on [the leaderboard](https://www.crowdai.org/challenges/nips-2017-learning-to-run/leaderboards) was 63rd with 17.51 metres, it looked like this:

 <iframe src="https://medium.com/media/387bd017b42c3f33d85d37aa48dcab8b" frameborder=0></iframe>

We had a model which scored 22.71 (which would be 42nd place), but due to some server-side bug it didn’t register on the leaderboard. It didn’t matter that much at that point, because the rules changed and not top 10 was advancing to the final Round 2 but competitors with score 15 or higher. Since we already had 17.51, we solely focused on training for Round 2, which was harder, since the number of obstacles (these spheres on the ground) was increased from 3 to 10. In the Round 2 we placed 22nd with 18.62 meters (GIF at the top of this post).

## What we did — software part

Since the beginning we learned that the simulator was [slow af](https://github.com/stanfordnmbl/osim-rl/issues/78) (compared to e.g. MuJoCo). It could take more than a minute for only 1 episode (full run from start to falling or 1000 timesteps). So we wanted to make the simulations as fast as possible. Speeding up network libraries (TensorFlow) or using GPU didn’t bring any value since OpenSim simulator took ~99% of the time.

One of the participants (Qin Yongliang) [posted a line of code](https://github.com/ctmakro/stanford-osrl#the-simulation-is-too-slow) which was changing the accuracy of the simulation to 3%. We recompiled OpenSim, that was tedious, especially on many hosts. However, with this change the simulations became ~3x faster. During the competition we were worried, that this would cause problems, because on the server the default accuracy of 0.1% was used. However, we didn’t notice any significant difference in the scores, so the “setAccuracy hack” seemed like a big win.

![](https://cdn-images-1.medium.com/max/2000/1*AoFJEbsJbtWYMW9Y4ZX6XQ.jpeg)

We couldn’t make OpenSim any faster than that, so we wanted to run them in parallel. The quick-start implementation given by organizers used DDPG from keras-rl library. In the beginning we tried to use it, but we weren’t quite happy. It was non-trivial how to run it with many parallel OpenSim environments. We also didn’t enjoy that the average score during training was fluctuating a lot and what’s worse — sometimes it could just drop and never regain. We tried to tune hyperparams, without any gain though.

One week after we started, on July 20, [OpenAI released their PPO implementation](https://blog.openai.com/openai-baselines-ppo/). It looked like it would be out-of-the-box parallelizable on multiple CPUs (with MPI). Their results were very impressive, they also used it for running (in harder setting, in 3D, our competition was really 2D, because the depth was fixed, skeleton couldn’t move along Z axis). It seemed perfect. It took about the same time to integrate as DDPG, we added plots, saving/loading state etc. We noticed that the average score is not fluctuating as much as with our tries with DDPG. Even better — it didn’t suddenly drop. So we switched to PPO and stayed with it until the end of competition.

We noticed that the RAM consumption grew constantly and after a day it could take 30 GB for a single process. Yep, memory leak. In the OpenSim simulator (or its Python wrappers). Some contestants were just automatically killing those leaky OpenSim processes and starting them again every say, 8 hours. We didn’t like it, it introduced another complexity, where things were already complex (DDPG, PPO and so on). We [found](https://github.com/stanfordnmbl/osim-rl/issues/10) that a combination of Python 2 + older version of OpenSim didn’t leak. With Python 3 it leaked.

So finally we could run multiple processes easily. We only had to use Python 2… Naturally, OpenAI baselines code required Python 3 :) However, Python 3 features were only used in a couple of places, so it was easy to backport the files we used.

## Hardware

In the beginning we used our laptops, 4 CPU cores each. The computing power was close to zero and it was horrible to hear the fan during the night. Piotr gained an access to 16 cores machine. We used it for a couple of weeks.

Then a lucky thing happened. We got an access to a machine with… 80 cores. That was a game changer. Now we could run dozens of experiments. To feed 8M timesteps of data into learning algorithm, it took about 24 hours, on 40 cores.

We also explored an option of running things on a cluster which had 24 nodes, each having 12 cores. However, the software there was very old and it was cumbersome to run things on it. We managed to compile a newer version of GCC and other stuff from dependency hell but finally our mana depleted.

In the middle of the competition top 100 received 300$ for AWS. We used that credit in the last 4 days, renting c5.9xlarge (1.728$/h) and c4.8xlarge (1.811$/h), each having 36 cores. About 86$ per 24h. Yikes. We wanted to use some instances with more cores, but all of the beefier ones had some default limit set to… 0. For any region. So, one could submit a ticket… for each instance type… and wait. We did that for m4.16xlarge (64 cores) and after 4 days AWS bumped the limit. That was 4 hours before the end of the whole competition. We could have requested it much earlier, but we didn’t know that “on-demand” meant “on-demand after 4 days after sending a ticket”.

![](https://cdn-images-1.medium.com/max/2000/1*4nuVTNLctk4WHfKNj7YrJw.gif)

## Back to the software

We found another interesting OpenAI blog post (all of them are great) about [Evolution Strategies](https://blog.openai.com/evolution-strategies/). We had 80 cores instead of 720, but we thought that maybe this was enough, even if it would be 9x slower (we still had more than a month till the end of Round 1). What we liked about ES is simplicity (compared to PPO), they are embarrassingly parallel. We didn’t have experience with in-memory database Redis that OpenAI implementation used. Also the code seemed tightly integrated with AWS, but after some struggle we managed to set it up and run locally. That came late though, 3 days before the end of Round 2. We couldn’t beat PPO, however the comparison isn’t fair, because on PPO we spent 2 months and on ES — about a week.

Ok, we had PPO from baselines running on 80 cores. We got some improvement when we extended the observation vector. 41 was already a lot, however it didn’t have all the velocities. Nor any accelerations. And RL algorithms work well if observation is Markovian, i.e. you can predict what happens next just from the given observation (in other words, you can forget about the past). So we added the remaining velocities and accelerations. Another important thing were obstacles. Original observation had only information about the next obstacle. It’s helpful to make an agent “remember” more, e.g. the previous obstacle as well. Otherwise he would “forget” about the one underneath it when seeing the next one. Original observations also didn’t have the force with which feet touch the ground. So we added an approximation of that. In the end we had 82 observations instead of 41. You can [refer to our code](https://github.com/AdamStelmaszczyk/learning2run/blob/master/baselines/baselines/pposgd/pposgd_simple.py) for the details.

To have more insight into what’s going on, we logged the mean and std (standard deviation) of all the observations, to see if the normalization is done well. Because normalization is one of [10 things that can easily go wron](http://theorangeduck.com/page/neural-network-not-working)g (great article). By default, PPO code uses a filter, which automatically normalizes every feature. For every feature, it keeps its running mean with std and does normalization by subtracting the mean and dividing by std. This works for most of the features. However, imagine a constant, like the strength of psoas (a muscle). The std is 0. The code in that case was just passing this value as it is. The magnitude of that value is treated as an importance when passed to a network. If it’s big, it will saturate all the other smaller inputs (which may be more important). Another problem was that the very first strength of psoas (due to a bug, later fixed) had some different value than the following ones. So that filter would calculate some arbitrary mean + std and later use them. Another problem are spiky features. Take velocity for example. Most of the time it’s quite low, but there is a moment it shoots up and it probably is an important moment. On one hand you want it big enough, so that the network picks it up, but not enormously huge, so that it doesn’t saturate other features. Because all of these problems, we skipped the auto normalizing and just manually normalized every feature. We would run our model, collect means with std and visualize:

![Top: mean of 82 observation values. Bottom: std (standard deviation).](https://cdn-images-1.medium.com/max/2648/1*ENlRHlIWp492HC4nFoVsxg.png)

If we saw that some value was not “in line” (mean not close to 0, std not close to 1), we would tweak the code and repeat. We ended up with something not ideal, but probably good enough so that the network can at least “see” all the given observations.

Another important thing was to use relative positions, not absolute. All our positions were relative to pelvis. Thanks to that similar poses were represented by a similiar observations. Think of a skeleton standing still, but in two positions, the initial one and the one 10 meters away from it. Absolute values for X in the initial position would be close to 0. 10 meters away they would be close to 10. However, relative X would be always close to 0 for skeleton standing still, no matter where on the absolute X axis.

The last thing that guided learning well was reward hacking. Essentially during training it’s worth to use a different reward than the one used during grading (the distance in meters). First reward hack came from DeepMind’s [“Emergence of Locomotion Behaviours in Rich Environment”](https://deepmind.com/blog/producing-flexible-behaviours-simulated-environments/) research paper. They used velocity instead of distance, which is better. Using velocity as a reward puts emphasis on passing the same distance in less timesteps. Using distance doesn’t, no matter if you passed 10 meters in 10 or 1000 timesteps. Problem we got was that lots of experiments got stuck in local maxima, which resembled jumping in place:

![We could maybe win NIPS 2018: Learning to Jump](https://cdn-images-1.medium.com/max/2000/1*i1f1MKdPlrMqlZ_uav7lvA.gif)

So, we added a small reward for every timestep “survived”. This helped it to take the first step and get out of local maximum. Naturally, we couldn’t give it too much reward for not falling, because then such things happened:

![](https://cdn-images-1.medium.com/max/2000/1*wEnJy41OpOpWGZ3rQuNIaQ.gif)

We were hesitant to adding more reward hacking throughout the challenge, it’s a bit ugly, we hoped for a more general solution. However, when the challenge was about to end and our models still performed so-so, we thought, well ok, let’s do it, there’s nothing else we could do. The nice thing about reward hacking is that the changes are quickly visible in training and they are easy to implement. However one could end up exploring not enough.

In the end we added two more. The first one was: “keep pelvis.x behind head.x”. It was causing it to lean a bit forward, because it’s obvious you must do that when running, but our models were not discovering it, they liked to wobble the head back and forth.

The second one was a penalty for straight legs. Our models all the time had two legs straight (or one in the best case). The penalty probably helps, but we didn’t have time to train with this long enough. Pity we didn’t try it earlier, it’s a simple one.

We tried many other things, for example reducing action to only 9 values for the right leg and copying them over to the left leg. We hoped to produce a fast hopper, kangaroo-like (with a simpler network). Our PPO however couldn’t train it well, skeleton was poor on obstacles. Here is how a good hopper looks like (this one is Henryk Michalewski’s result, it uses full 18 action values though, it just converged to this quite good local maximum):

 <iframe src="https://medium.com/media/10ca7db05343f006ea82c75920bc6d0f" frameborder=0></iframe>

## Summary

That was the longest competition we participated in, it was mentally tiring. However, lots of fun! We’ve also learnt much more than we anticipated.

In hindsight, we started to tweak the hyperparams of DDPG and PPO too early. It took a long time and almost all that work went to the bin, because we abandoned DDPG and in PPO we changed features and rewards (so we needed to tweak hyperparams again). In the end we used the original PPO architecture, 2 hidden layers with 64 neurons. It’s [hard](https://arxiv.org/pdf/1709.06560.pdf) to decide if some hyperparam is better, we finally run out of time. Also we felt it was better to give a couple of days of CPU to already walking model instead changing hyperparams and starting from scratch.

It may be that we too early abandoned DDPG. Some participants were using it. We could have tried with baselines DDPG implementation, especially after OpenAI [injected noise in the parameters](https://blog.openai.com/better-exploration-with-parameter-noise/) to improve exploration. We also feel that ES option still has a big potential. We saw TRPO, but PPO seems like improved TRPO, that’s why we didn’t try it. But maybe we’re wrong.

When taking some new algorithm, it will be better if we first replicate it on some easy/known environment first. To be sure that we got it working + gain a knowledge of how it trains. After that, plug in the complex environment. Because if we can’t learn the easier environment, we won’t learn the harder one for sure.

![](https://cdn-images-1.medium.com/max/2000/1*9coz4RsWAGi7AuRUD0Ag5g.jpeg)

This manual normalization also took very long. Now it seems it might be faster to use auto normalization for most of the features but only normalize manually a few chosen ones. I thought about it and tried, but my TensorFlow fu was too weak. I went for a simpler (but probably more time-consuming in hindsight) manually normalizing all the features.

I would implement logging earlier, to gain as much insight into that black-box as possible. That was our biggest problem I would say, lack of visibility of what’s going on during training. We could spend more time to learn TensorBoard or similar perhaps.

Ok, that’s it! Thanks again to the organizers and competitors! We’re looking forward to other write-ups, we’re very curious of other approaches. Update: they are [here](https://arxiv.org/abs/1804.00361).

In the beginning I think nobody was sure if it would be possible to run fast, but the results are really impressive and the answer is clearly: yes.

To see what’s possible, USTC-IMCL’s solution winning Round 1 with 44.61m:

 <iframe src="https://medium.com/media/25de4a2db955db8c54e242b9cbbdfddb" frameborder=0></iframe>

NNAISENSE (Wojciech Jaśkowski et al.) won the final Round 2 with a very impressive result of 45.96m, congratulations! State of the art running… In Round 1 they were 5th with 42.71m. All the videos from Round 1 and 2 are available [on the leaderboard](https://www.crowdai.org/challenges/nips-2017-learning-to-run/leaderboards?challenge_round_id=1).

Update: Award for the best comment goes to Imnimo reddit user:
>  Might be 22nd place for learning to run, but looks like 1st place for the Ministry of Silly Walks. I’d say it’s an uncannily accurate recreation of some of John Cleese’s techniques.
