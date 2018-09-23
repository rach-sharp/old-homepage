---
layout: post
title: Fantasy Footbot
categories: [python, linear-programming]
---

Have you bitten off more than you can chew in the office fantasy football league?
Bench-warmers on your team? A deep seated need to solve linear maths problems?  Well
never fear - now you can delegate to [Fantasy Footbot](https://github.com/rach-sharp/fantasy-footbot)
and have it pick your team for you!

Last year I picked a totally random team and got _totally_ crushed in the first week.
Fantasy Football seems deceptively simple - some players are better than others at
scoring points for you, but they also cost more. So the game is finding the players
when they are undervalued and having the right players on your team at the right time.
The fantasy premier league provides a public API, so I wrote a script to decide my
team for me - it would look for players who would give a good return of points over
the season for the amount of money they would cost to have on my team. There are
two steps - first we need to rank all the players and then it becomes a linear
programming problem to conform to all the constraints FPL puts on your team. I’ll
cover the details of how I implemented this and then cover some of the factors which
actually make the problem more complex than Fantasy Footbot can currently handle.

The first step is to rank all the players in the league. The fantasy premier league
has all sorts of constraints about how many players you can pick from which teams,
but we need to know how all the players stack up against each other before we can
start picking a team of the best. The current approach is pretty simple and very
greedy:

- rank all players by how much they score over the course of a whole season
- Ignore any injured or banned players.
- If you don’t have enough data about the current season (less than 5 games), use a 
  weighted average of score in the current and previous season.

Initially I tried to rank players using points per game, or points per minute
played. The problem with this is that even if the players are scoring well for how much
they play, if they aren’t getting consistent game time, they will be dead weight
on your fantasy football team.

<Example of ranking the players>

Now you have a list of all the players you want on your team - but the constraints
of fantasy football come into play:

- No more than 3 players from the same real life team
- Your squad has to have a standard formation (e.g. 3 attackers, 5 midfielders, 5 defenders, 2 goalkeepers)
- You only have 100 million to spend - the average player costs about (NUMBER)
  and the best players cost a lot more

Sounds like a linear programming problem - we need to solve a series of linear
equations to work out an optimal solution. I got a lead from someone else who had
tried something along the same lines also in Fantasy Football, <lead here>.

Expressed as a linear programming problem:

For the 500 players in the league, where each player is either in or out of the team, maximise the total ranking score, while conforming to the constraints (sum of costs < 100), (no of players from <team> < 3) (once for each team), (no of players in <position> == <n>) (once for each position: n pair)

I’m using the Pulp library to solve the linear programming problem expressed above.

<Explanation of simplex and what it’s doing behind the scenes>

CBC (COIN-OR branch and cut) solver https://www.coin-or.org/Cbc/cbcuserguide.html
https://en.wikipedia.org/wiki/Branch_and_cut

All this is implemented in Python, written so that any function can be added to rank the
players or build the team.

Some of the challenges involved with FF. Lol Man City’s spread of players.
Quirks of the game, e.g. defenders playing up and midfielders playing down.

- TODO produce some more content about PULP, fantasy football quirks
- Add examples of the program
- Other possible continuations
- Final pass over for quality assurance

