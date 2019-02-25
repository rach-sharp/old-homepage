---
layout: post
title: The Worst Python Code Ever
categories: [python]
has_more: true
---

A while ago some friends and I brainstormed some dumb functionality we would include in an
over-engineered esoteric programming language. I’ve had a go at implementing
some of them as [a Python module (`esoterrible`)](https://www.github.com/rach-sharp/esoterrible){:target="_blank" rel="noopener"} and
I can confidently say it’s the worst code I’ve ever written. When you import it, it messes
with your builtins to achieve the following “features”


##### UTC? What's UTC?

Don’t even need to set timezones on your datetimes! Esoterrible will analyse whether your 
source code contains more British or American English (anglocentric, I know) and use that to enforce timezones 
on `datetime` objects. Also it complains if you try to use both British and American English.

<img src="{{ site.baseurl }}public/images/language-error.png" alt="When you use American and British spelling together">

<!--more-->

##### Truthiness is in the eye of the caller

Tired of only having `True` and `False` boolean built-ins? Are you a butterfingers who ends
up writing `Talse` by accident? A whole new spectrum of booleans are available, which are true
or false a random percentage of the time based on how closely they resemble `True` or `False`

<img src="{{ site.baseurl }}public/images/new-booleans.png" alt="New boolean builtins">

##### Traditional Dictionary "Lookups"

It’s all well and good being able to access dictionary values in O(1) time, but don’t you
wish those pesky Python dictionaries acted a bit more like the real thing? Instead of reporting
KeyErrors on missing keys, `dict` will return a lovely dictionary definition instead.

<img src="{{ site.baseurl }}public/images/oxford-dict.png" alt="Missing dict key default behaviour">

##### Looks optimised to me

Wouldn’t it be great if optimising your code for performance were as simple as making it
look like its been optimised for performance? `time.sleep` now checks the amount of whitespace
and number of lines in the code it was called from, so bunched up illegible
functions now sleep for less time than beautiful, cleanly-formatted code. In theory you could
add this lag effect to every built-in.

##### \*beep\* warning, variable reversing

Access a reversed list or string just by using the reversed name of the variable! One less
builtin to worry about! Note: not at all practical to implement so it just does it in a really
useless way.

<img src="{{ site.baseurl }}public/images/reversing.png" alt="Reversing a variable name">


### Why? How?

Just in case looking at [the source code](https://www.github.com/rach-sharp/esoterrible) melts
your face off à la Raiders of the Lost Ark,
here’s the highlights of how I achieved the above effects:

- Take all the possible 4-5 letter words using the set of letters (T, R, U, E, F, A, L, S),
take the ones which have a low summed [word-distance](https://en.wikipedia.org/wiki/Levenshtein_distance){:target="_blank" rel="noopener"}
to True and False (so they are similar and en-route when transforming one to the other). The
distance from False indicates how “truthy”
they should be. Looks like overwriting dict literal syntax is not trivial so it only works with
`dict()` constructor.

- Inspect the `__main__` module’s file location to identify American/British spellings, using a
saved list of differences taken from [this site](http://www.tysto.com/uk-us-spelling-list.html){:target="_blank" rel="noopener"}

- Use the `inspect` module to find the name and source code of the function/module which calls
the function I override `time.sleep` with.

- Reversing names proved difficult for a couple of reasons:
  - Not easily able to fire on variable creation to create the counterpart variable with reversed
name. The helper function `rotate_the_board()` is what creates the counterparts when called
  - Updating an arbitrary local scope with new variables is challenging/impossible so all
  updates are just applied to builtins. I’m guessing some introspection of frames miiight make
  it possible to access the caller’s globals

- Very liberal use of `setattr(builtins, <name>, <value>)`

Feel free to throw any garbage PRs onto this fire.