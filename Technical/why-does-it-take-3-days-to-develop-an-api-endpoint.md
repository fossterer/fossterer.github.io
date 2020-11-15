# Why does it take you 3 days to develop an API endpoint?

Date: Monday Jun 24 2019

Just as I am releasing fixes for misconfigurations I did in API specification and the bugs crept in as a result, I'm recording this.
Initially I estimated it takes 3 days to develop and release a REST endpoint but as I remembered some past comments of a few developers, I felt I should make this fast and released it in 1.5 days.

There I overlooked how I was misconfiguring indentation in API spec, leading to failure in existing Integration tests that I chose to overlook - **Mistake no: 1**

I further did not write integration tests for these new set of endpoints which in themselves didn't cause error but after releasing fix for that 'misindentation', these new endpoints started throwing errors. These could have been caught way before in the process, have I had tests for them ready - **Mistake no:2**

So, understand and assert that *Engineering is not just programming*.

>Either we release fast, in an attempt to prove my self-worth (?!), break things and take additional time to fix those things or we take time to develop tests, give them full run before each however seemingly small feature/fix.
>
>See, there's choice. I'll let you pick

## Time spent writing tests

I get it. Writing tests is unrewarding. Let's say 97%[1] of the tests we write simply pass and only 3% fail. These 3% if caught early save us from new efforts in the future to hunt the cause, understand the original intent and fix.

Humans are fallible. There's a reason these many years of software engineering practice resulted in us writing tests. So we need tests.

But, is it reasonable to require writing tests just to catch for those minute no. of failures? Hmm, may be. You might have a point. My current take is indeed to continue writing tests so when future engineers (or we ourselves in the future) get handed the responsibility of managing (or debugging an issue and fixing) this part of code, they (or we) would have an idea of why something is to be preserved as-is (or with non-breaking modifications). *Naming your tests* to match the original requirement helps here.

For example, instead of

>ClickComponentTest

call your test

>ClickShouldSendEmail

The latter has the benefit of preserving the original (business or otherwise) for future reference while the former just tells us the name of function we chose to write during the initial devleopment

---

### Footnotes

[1] Numbers are made-up for effect
