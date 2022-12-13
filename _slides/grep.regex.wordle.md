---
title: grep, regex, Wordle
description: Using grep to help with Wordle
theme: black
---

## grep

reveal.js enables you to create beautiful interactive slide decks using HTML. This presentation will show you examples of what it can do.

----

## Vertical slides

Slides can be nested inside of each other.

Use the **Space** key to navigate through all slides.

<a href="#" class="navigate-down">
  <img width="178" height="238" data-src="https://s3.amazonaws.com/hakim-static/reveal-js/arrow.png" alt="Down arrow">
</a>

~~

## Basement level 1

Nested slides are useful for adding additional detail underneath a high level horizontal slide.

~~

## Basement level 2

That's it, time to go back up.

<a href="#/2">
  <img width="178" height="238" data-src="https://s3.amazonaws.com/hakim-static/reveal-js/arrow.png" alt="Up arrow" style="transform: rotate(180deg); -webkit-transform: rotate(180deg);">
</a>

----

# grep, regular expressions, Wordle

enormous number of problems revolve around reducing the search or solution space.
Reduce by increasing constraints:
- sieve of Eratosthenes
https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes
- finding someone lost at sea
- guessing a number between 1 and 1,000


duality of constraints:
- positive/negative space
- image: David
- quote: "How did you create David?", "It was easy. I just chipped away the stone that doesn’t look like David."
https://en.wikipedia.org/wiki/David_(Michelangelo)

- figure/ground
- image: wife and mother-in-law
https://en.wikipedia.org/wiki/My_Wife_and_My_Mother-in-Law
- image: vase and two profiles
https://en.wikipedia.org/wiki/Figure%E2%80%93ground_(perception)#Non-visual

- selection/counter-selection
image: petri dish
https://www.snopes.com/fact-check/petri-dishes-coughed-on-mask/


With data
grep: select/exclude

things you can do with grep, regular expressions:
- parse data
- find patterns in a list of strings


Protype search problem: Wordle
https://en.wikipedia.org/wiki/Wordle

Large solution space that gets smaller with each successive guess, i.e. constraints.
- 660+ words in /usr/share/dict/words

Process: reduce the search space
- Initial search space
  1 - get a list of words
  2 - select for 5-letter lower-case words
  3 - guess first word
- Reduce search space
  1 - select for letters that exist at a position
  2 - exclude letters that don't exist anywhere
  3a - select for letters that exist somewhere
  3b - exclude letters that don't exist at a position
  4 - guess next word
  5 - repeat

We can use regular expression patterns to express constraints.

