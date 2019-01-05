---
layout: post
title: The Worst Code I've Ever Written
categories: [python]
---

A while ago some friends and I brainstormed some features we would include in a bad, bad,
incredibly bad esoteric programming language. I’ve had a go at implementing some of them and
I can confidently say it’s the worst code I’ve ever written. It’s available as a Python
module called `esoterrible` - when you import it, it messes with your builtins to achieve
the following “features”:

- Don’t even need to set datetime timezones! Esoterrible will analyse whether your 
source code contains more British or American English and use that to enforce timezones 
on `datetime` objects. Also it complains if you try to use both British and American English.

- Tired of only having True and False boolean built-ins? Are you a butterfingers who ends
up writing Talse by accident? A whole new spectrum of booleans are available, which are True
or False a random percentage of the time based on how closely they resemble True or False

- It’s all well and good being able to access dictionary values in O(1) time, but don’t you
wish those pesky Python dictionaries acted a bit more like the real thing? Instead of reporting
KeyErrors on missing keys, `dict` will return a lovely dictionary definition instead.

- Wouldn’t it be great if optimising your code for performance were as simple as making it
look like its been optimised for performance? `time.sleep` now checks the amount of whitespace
and number of lines wherever it was called from, so bunched up illegible methods now sleeps
for less time than beautiful, cleanly-formatted code.

- Access a reversed list or string just by using the reversed name of the variable! One less
builtin to worry about!

Just in case looking at the source code melts your face off a la Raiders of the Lost Ark,
here’s the highlights of how I achieved the above:

- Take all the possible 4-5 letter words using the set of letters (T, R, U, E, F, A, L, S),
take the ones which have a low summed [word-distance]() to True and False (so they are similar
and en-route when transforming one to the other). The distance from False indicates how “truthy”
they should be.

- Inspect the __main__ module’s file location to identify American/British spellings, using a
saved list of differences taken from [this site](http://www.tysto.com/uk-us-spelling-list.html)

- Use the `inspect` module to find the name and source code of the function/module which calls
the function I override `time.sleep` with.

- Reversing names proved difficult for a couple of reasons
  - Not easily able to fire on variable creation to create the counterpart variable with reversed
name. The helper function `rotate_the_board()` is what creates the counterparts
  - Updating an arbitrary local scope is challenging so all updates are just applied to builtins.
I’m guessing some introspection of frames might make it possible to access the caller’s globals

- Very liberal use of `setattr(builtins, <name>, <value>)`

Here’s some other ideas we had:
