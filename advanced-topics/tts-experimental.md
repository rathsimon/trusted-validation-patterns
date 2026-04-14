---
description: >-
  When speed is a must - trade performance for security (only experimental -
  please be aware).
metaLinks: {}
---

# TTS (experimental)

{% hint style="danger" %}
**ATTENTION: This pattern is purely experimental. It is not recommended to implement TTS in a system outside a sandbox.**

**Use these patterns only after careful consideration - and only with caution!**
{% endhint %}

{% hint style="info" %}
Despite the significant reduction in security when applying this pattern, it is recommended to also use HVR for this pattern to protect requests that run through the validation layer.
{% endhint %}

### Initial

TTS (short for Trusted Timeframe System) is an experimental pattern that is used in systems with a high demand for performance and the lowest possible delay, but in an exchange for security - to increase throughput and reduce latency.

***

### Problem statement

Some systems require fast processing / execution of requests. Furthermore, it may be the case in these systems that invalidated requests cannot cause great damage. In this system, performance gains and latency minimization are exchanged for a major reduction in security.\
\
In these systems, it may be the case that the validation of each individual request by the validation layer simply takes too long.

***

### Pattern description

If a specific system (without user involvement) or a specific system with a specific authentication context (with user involvement) sends a certain number of valid requests via X (for example, 100 requests), then one might assume that this source is trustworthy, or at least more trustworthy than a source that sent 5 requests, 3 of which were unvalidated and therefore incorrect.

{% hint style="warning" %}
Of course, the comparison isn't quite that simple: a system that has sent 100 correct requests could just as easily send an incorrect request on the 101st try - the comparison is intentionally kept very simple.
{% endhint %}

And then of course you can say: This source now has an advantage - after the X correct requests, it may send X requests, which do not have to be validated, but are sent directly to L3.

This pattern can depict different logics from fixed rules (X validated -> then X without validation possible; X validated -> X without validation with samples) to dynamic rules (confidence scores, etc.).

In the end, however, it always comes down to one thing - a "trusted" system always has the possibility to send unvalidated requests to the Processing Layer (L3) to optimize the performance.

There are also 3 ways to do this:

1. Nevertheless, you do not allow a connection from L1 directly to L3 to forward the request, but continue to forward the requests via the Validation Layer (L2), which marks the requests as "Skip because of TTS" and lets them through. In this case, L2 resumes the task of validating requests again as soon as the TTS period expires.\
   (Hint: Especially when using HVR, this route is significantly slower than the other two.)
2. There is a TTS endpoint on L3 which is also accessible for L1, whereby a token must be given with every transfer of requests (also possible in bulk).\
   Here, L3 must independently check the validity of TTS tokens.
3. As soon as TTS is activated, a certain endpoint is exactly valid for the source (either by special endpoint URL or restriction with source IP if static) is opened -> almost the same functionality as No. 2 only different response channel.

As already mentioned above, this pattern should only be used after a fundamental and detailed consideration. For (presumably) 99% of all use cases this is probably not recommended and the extreme exchange of the results in much less security absolutely not worth the performance gain.
