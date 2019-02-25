---
layout: post
title: AWS GameDay
categories: [aws]
has_more: true
---

<img src="{{ site.baseurl }}public/images/unicorn_rentals.png" style="float:right" alt="Unicorn Rentals Logo">

Welcome to Unicorn Rentals! [AWS GameDay](https://aws.amazon.com/gameday/){:target="_blank" rel="noopener"}
invites you to try your hand at managing
the infrastructure of the biggest player in the Legendary Animal Rental Market (LARM).
This fictional company that AWS made for people to play as on GameDay is an excellent parody of all the worst practices you've
heard of and lived through, believing in security through obscurity and firing their engineering team
every few weeks. Unicorn Rentals needs YOUR HELP to deal with a swathe of mission-critical issues throughout the day.

<!--more-->

A while ago I attended an AWS GameDay run for my company and had a fantastic time.
I had a smattering of AWS knowledge around buckets and EC2, and learned some more about those and
a dozen other AWS services, enough of a variety that those with more experience could still work on something
new to them. Working on challenges as a group and having staff on hand with advice meant you were never stuck for
too long on something.

All the challenges were worth an amount of points for a successful completion and points over time for the rest
of the day, displayed on a dashboard at the front of the room. Points over 
time was an elegant way to award more points to early completions, especially since maintaining points income was
also part of the challenge. Success was determined automatically by AWS-managed scripts (e.g. regularly requesting data from
various endpoints on a website). Certain implementations might not scale well and lose you points over time, and the resources
on your account could even be meddled with by AWS. You would see your team losing points over time and a rough error
message, leading to sudden bonus rounds where you debugged what you or your teammates created before.

<figure>
    <img src="{{ site.baseurl }}public/images/game_day_dashboard.png" alt="GameDay Dashboard">
    <figcaption><i>Dashboard with total score and "trend", current rate of score change</i></figcaption>
</figure>

The sandbox that AWS had built that tested your code and awarded points automatically, providing instant feedback, was an
engaging framework for the GameDay and I'm sure it helps AWS to scale these events up with less staff.
If you get the chance to participate at a conference or otherwise, I'd recommend GameDay to anyone relatively new to AWS.
Here's some of the flavourful challenge briefings to give you a feel of the theme and scope of the day.


##### dude where's my website?

>Six months ago, one of the Ops team began a project called "dude where's my website". 
This was a skunkworks project to create backups and a DR environment for Unicorn.Rentals, 
in the unlikely event that we'd ever need to recover the site.    
It turns out that this is rather more likely than we expected.

- Serving a static website from S3
- Running a Flask WebApp on EC2
- Path-Based Routing to serve images from a different host


##### Where in the world is Kyle Sandiego?

>Our CEO, Kyle, has taken a sabbatical and is traveling around the world. Being that he is a
social media junkie, he keeps posting selfies of himself around the world. But he's moving so
quickly that we don't have enough time for our interns to look at the images to try and find
out where he is.

- Lambda Functions
- Rekognition to perform Image Analysis (Kyle or Not Kyle)
- S3 Buckets

##### Casual Infrastructure Causes Disasters

>“During the hyper growth of Unicorn Rentals, we struggled to retain experienced developers.
Some got better offers from "real" organizations, others were allergic to chowder, some
objected to frequent Thunder-dome challenges…

>Anyway, we have a unicorn metadata service, that we never secured in any way. 
And now its being abused by third parties...    
> Now there are many smart ways to solve this problem, but we do not currently have the time or people. 
So, instead, we are going to build a shared secret into the source code of an API Proxy application 
and deploy it every 4 minutes.

- Code Pipeline, Code Build, Code Deploy
- Auto Scaling Groups


##### Fix our Unicorn Rental Return Processor
>This module will repair our broken Unicorn Rental Return processor and hopefully allow us to successfully
track down our missing and outstanding unicorns. We will do this by modernizing our existing code base
and adopting some Cloud Native AWS Services.

> Our modernization effort has taken a steep dive and ground to a screeching halt. In fact, all Unicorn
Rentals that are being returned by our extremely satisfied customers are no longer being processed. This
is causing a very large backlog in the rental return processing queue.

- More Code Pipeline
- Setting up an Application Load Balancer



### [Full GameDay Let's Play](https://www.youtube.com/watch?v=1ry0mYEdfno){:target="_blank" rel="noopener"}

<div class="iframeVideo">
<iframe src="https://www.youtube-nocookie.com/embed/1ry0mYEdfno" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>