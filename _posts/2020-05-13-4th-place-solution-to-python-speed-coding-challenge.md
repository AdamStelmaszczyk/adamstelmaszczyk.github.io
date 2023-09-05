## 4th place solution to Python Speed Coding Challenge

I participated in one week [challenge](https://speedcoding.toptal.com/faq?ch=python-scc) that started on May 4, I will describe my approach. 445 people entered the leaderboard:

![[https://speedcoding.toptal.com/leaderboard?ch=python-scc](https://speedcoding.toptal.com/leaderboard?ch=python-scc)](https://cdn-images-1.medium.com/max/2000/1*YWK-ffgMpkuyD6lnDlzIWQ.png)

## Challenge overview

Competitors were given a web UI with 33 short coding problems and 180 seconds time limit. There was no limit on the number of trials. Also we were encouraged to be creative, bots allowed.

Coding problems were worth 1740 points in total. For submitting before the time limit, 10 additional points for each second were given. So if one somehow sent them all instantly, it would be additional 1800 points, bringing the total to 3540. Top 6 had >3500 points, so 33 solutions sent under 4 seconds.

## How it went

In the beginning people started solving tasks and copy-pasting them manually, climbing to the point limit just from the coding tasks. One can paste manually 33 tasks before the timeout, a bit under 180 seconds. Speeding it up is when the fun begins.

Previously a similar JavaScript competition took place, here is the [retrospective](https://www.toptal.com/javascript/coding-challenge-retrospective). One can automate the pasting or skip the browser and just talk to the API. The second runs faster, so many people went for that. To quickly replicate the communication, one can inspect the HTTPS requests, save them as cURL and then [convert](https://curl.trillworks.com/) to Python requests library code. I also removed not needed data: left only a cookie from the headers and the task source code could be just a single character. “d” turned out to be the fastest due to its bit structure.

No, I’m joking, it could be anything (empty didn’t work). But yes, your code wasn’t run on the server side, server only checked the answers to the random test cases (which you needed to send). In the server reply the random test cases for the next task arrived.

If test cases weren’t random one could sent 33 requests even faster, waiting for the server reply won’t be needed. There was attempt ID, but when submitting quickly it would just increment.

Python submitting all the answers took ~11 seconds on my laptop. Let’s see where it was send to:

    $ host speedcoding.toptal.com
    speedcoding.toptal.com has address 104.22.43.181
    speedcoding.toptal.com has address 104.22.42.181
    speedcoding.toptal.com has IPv6 address 2606:4700:10::6816:2ab5
    speedcoding.toptal.com has IPv6 address 2606:4700:10::6816:2bb5

Checking the first address on e.g. [https://ipinfo.io/104.22.43.181](https://ipinfo.io/104.22.43.181) tells that it’s Cloudflare US. So packets first fly there and then to the actual grading server, round trip time matters a lot. But I could run it from a free instance on AWS/GCP, in some US region. After checking a couple of combinations, AWS us-east-1c seemed the fastest, bringing the time to under 4 seconds.

To increase the number of trials (of which the fastest counted on the leaderboard) I was submitting every 7 seconds with:

    $ while true; do date; time python3 run.py; sleep 7; done

Could log off and leave it for a night with:

    $ nohup ./loop.sh &

This also gave a nice nohop.out log to grep, sort by time etc.

A faster submit could happen after several hours. Cutting 100 ms gave 1 point (and top was tight, every point mattered). The other optimizations:

 1. Using keep-alive to avoid handshaking all the time.

 2. Turning off SSL verification (API only worked with HTTPS).

You can find the details in the [source code](https://gist.github.com/AdamStelmaszczyk/8a35ad8a727b7ef6f70c813b997f8e95). I publish as it was, only stripped out task solutions, because the organizers asked so that they can reuse the problems.

## Summary

I should have tried other Python libraries like urllib3 and pycurl, others said that they were faster. Makes sense because requests calls urllib3 and pycurl is a Python interface to libcurl.

Different things I tried, but didn’t bring improvement:

 1. More beefy machine (m5n.24xlarge) with “enhanced networking” on AWS. On GCP I didn’t see an equivalent for “enhanced networking”.

 2. HTTP/2, had too old curl version for it, found hyper Python library, but it wasn’t a drop-in replacement for requests, so didn’t invest more time in it.

Overall the challenge was light on time and the format was quite original. Thanks to the organizers and congrats to all the participants, it was fun.

If you like such competition summaries, I wrote another one some time ago: [Our NIPS 2017: Learning to Run approach](https://medium.com/mlreview/our-nips-2017-learning-to-run-approach-b80a295d3bb5).
