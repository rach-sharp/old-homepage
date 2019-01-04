---
layout: post
title: AWS GameDay Review
categories: [aws]
---

<img src="{{ site.baseurl }}public/images/unicorn_rentals.png" align="right" alt="Unicorn Rentals Logo">

Welcome to Unicorn Rentals! [AWS GameDay](https://aws.amazon.com/gameday/){:target="_blank"} 
invites you to try your hand at managing
the infrastructure of the biggest player in the Legendary Animal Rental Market (LARM).
This fictional company that AWS made for people to play as is an excellent parody of all the worst practices you've heard of and lived through, believing in security
through obscurity and firing their engineering team every few weeks. Unicorn Rentals needs
YOUR HELP to deal with a swathe of mission-critical issues throughout the day.

<!--more-->

A while ago I attended an AWS GameDay run for my company and had a fantastic time.
I had a smattering of AWS knowledge around buckets and EC2, and learned some more about those and
a dozen other AWS services. This is an overview of how the day worked, the different tasks we had to
complete, and the touches that made the event so satisfying.

### dude where's my website?

>Six months ago, one of the Ops team began a project called "dude where's my website". 
This was a skunkworks project to create backups and a DR environment for Unicorn.Rentals, 
in the unlikely event that we'd ever need to recover the site.    
It turns out that this is rather more likely than we expected.

This website recovery challenge was the first thing we faced after everyone grouped up into teams. The aim was
to score as many points as possible from completing different challenges.
It started off easy - serving a static website from an S3 bucket, where the site is stored in another bucket
as a .zip file. Later parts of the challenge involved setting up an EC2 to run a Flask WebApp and routing
traffic to serve image files from a separate instance.

All the challenges were worth an amount of points for a successful completion and points over time for the rest
of the day. Success was determined automatically by AWS-managed scripts and points would appear on a dashboard on one
side of the room. Points over time was an elegant way to award more points to early completions, especially since getting
that initial points bonus didn't mean you were out of the woods. Maintaining points income was a challenge in the face of
AWS meddling with some resources on your account, leading to impromptu debugging sessions to find and correct whatever the
GameDay Staff / scripts were changing. We were off to a good start and were the first to complete this challenge, 
but later in the day we were hemorrhaging points from an unresolved issue.


### Where in the world is Kyle Sandiego?

>Our CEO, Kyle, has taken a sabbatical and is traveling around the world. Being that he is a
social media junkie, he keeps posting selfies of himself around the world. But he's moving so
quickly that we don't have enough time for our interns to look at the images to try and find
out where he is.

New challenges were cropping up one-by-one throughout the day, at a rate which meant that for the majority
of the time, teams were best off splitting into two and pair programming. This next challenge involved 
using Rekognition from a Lambda function to perform image analysis on incoming pictures being deposited
in an S3 bucket and identify them as 'Kyle' or 'Not Kyle'.


### Casual Infrastructure Causes Disasters

>Anyway, we have a unicorn metadata service, that we never secured in any way. 
And now its being abused by third parties...

TODO CICD

Remaining challenges

- An AWS Glue Task about using SQL on unstructured CSV data
- Implementing a delivery pipeline