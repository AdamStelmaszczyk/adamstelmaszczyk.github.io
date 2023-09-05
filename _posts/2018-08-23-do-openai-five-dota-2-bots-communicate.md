
## Do OpenAI Five Dota 2 bots communicate?

**Update on September 12:**

I’m sorry, I was wrong. One data point supporting [Cunningham’s Law](https://meta.wikimedia.org/wiki/Cunningham%27s_Law). [Ajedi32](https://news.ycombinator.com/item?id=17968829)’s and [gdb](https://twitter.com/gdb/status/1039912132622442496)’s comments changed my mind. I didn’t know that in Dota a player has perfect information of the state of all allied units. So the blocks above LSTM are combined allies’ observations, not observations from one hero as I thought. This changes mine interpretation of “max-pool across Players”. Now I just see it as an observation processing. No communication (excluding something like [bee dance](https://en.wikipedia.org/wiki/Waggle_dance), I don’t know if it occurs).

Original post from August 23:

**tl;dr** Yes, in one direction, in broadcast-like fashion with superhuman bandwidth.

[This](https://s3-us-west-2.amazonaws.com/openai-assets/dota_benchmark_results/network_diagram_08_06_2018.pdf) is the model from August 6. Version annotated by me:

![Original image: [https://s3-us-west-2.amazonaws.com/openai-assets/dota_benchmark_results/network_diagram_08_06_2018.pdf](https://s3-us-west-2.amazonaws.com/openai-assets/dota_benchmark_results/network_diagram_08_06_2018.pdf) published on [https://blog.openai.com/openai-five-benchmark-results](https://blog.openai.com/openai-five-benchmark-results/)](https://cdn-images-1.medium.com/max/2000/1*udBwxYpGPBCK-i5VPck4SQ.png)

There are 5 bots, each is using a copy of the big model from the picture. Each model takes observations and passes it to its “brain” where it decides an action to execute. This happens in milliseconds and is repeated over and over again.

The red arrow on the left points to an important block, let’s zoom on it:

![](https://cdn-images-1.medium.com/max/2000/1*A5Vpvxg5rB3qajdOC7d0Yg.png)

So, the observations from one hero are going through the “concat” (concatenation), which just puts all its inputs in one long vector. Then the observations go through “FC-relu”, which is a fully connected neural network. “FC-relu” modifies the observations, extracts features. Then we take first 512 values of that (left slice from 0 to 512) and pass it through **“max-pool across Players”**. Output of this we concatenate with the remaining hero’s observation (slice from 512 to 2048) and pass to the hero’s “brain” to decide an action.

This “max-pool across Players” is the only block that does something “across Players”, all the other blocks are looking only at one hero’s data. “max-pool across Players” looks at 512 signals **from all the teammates **and passes the maximum values. These are signals of a great value — for example spotted enemy hero data.

Overall this behaves like an instant input about something important that any teammate saw. Like if one bot had also eyes of all the other bots. Humans would need to say something on the microphone or signal it on the map. And that would be only a couple of values passed — e.g. the position of a spotted enemy. OpenAI Five sends 512 such values every couple of milliseconds— imagine how a mini map would be glowing. Or, every team member shouting 512 numbers on the microphone every couple of milliseconds and understanding the loudest bits perfectly.

![Augmented Dota player](https://cdn-images-1.medium.com/max/2000/1*xE6LyOzFmzQwphaRvCJgHA.jpeg)

So, I wouldn’t say that they are not communicating at all. They pass a lot of observations to their teammates, in one direction, very fast. It’s not a full communication — they don’t reply. It’s just like a quick broadcast/shout: “I see such item here, such enemy hero there, he has 1234 HP, 234 mana” etc. And then heroes decide for themselves (in milliseconds) what actions to take in response to the “loudest” signals.

Also note that bots’ “brains” are the same. These two (broadcasts and the same “brains”) are one of the reasons why OpenAI bots are so good in group fighting. There’s also the mechanical superiority part: constant 200 ms response time and direct API connection instead of screen scrolling and clicking.

[On June 6](https://d4mucfpksywv.cloudfront.net/research-covers/openai-five/network-architecture.pdf) the model didn’t have anything working across players.

I’m not saying anything is bad or good — it just worked like that on August 6 (so likely on The International, August 22–23, too). I enjoy what OpenAI is doing, very exciting. It’s just an interpretation of how bots communication work, I hope it was helpful.
