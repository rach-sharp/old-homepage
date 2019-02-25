---
layout: post
title: Launching Magic Playlists
categories: [python]
has_more: true
---

<img src="{{ site.baseurl }}public/images/magic_playlists.png" alt="Magic Playlists Site">

[Magic Playlists](https://music.rachsharp.co.uk){:target="_blank" rel="noopener"} has been a side project I've been working on sporadically since early 2017 and I'm
overjoyed to finally be able to release it today for anyone to use. It integrates with a Spotify
account to provide two awesome features: <!--more-->

- Infographics based on listening data, similar to [Spotify Wrapped](https://spotifywrapped.com){:target="_blank" rel="noopener"}
- Playlists that update themselves automatically in the same way as Discover Weekly

Originally I was calling it Spotify++ and all it was doing at the time was running on a Raspberry Pi, managing the automated playlists.
It worked well for _me_ at the time, but it couldn't
be enabled for new users without a painful manual process of signing someone up by hand.
I rewrote some poorly designed parts, came up with a shiny new database model and then worked on a new
web app frontend so that users could sign up themselves and pick which playlists they wanted.

This is when I succumbed to feature creep and added infographics rather than shipping straight away.
Not good. After a while I could see how much delay it was causing. I recovered my senses and cut
out most of the original stats and graphs I was producing because they wouldn't be impressive for a new
joiner to the site - the Spotify API only provides the 50 most recent listens, so it takes time to get
a useful amount of data.

The initial infographic is a special case - it uses the Spotify API Endpoint 
[User Top Artists and Tracks](https://developer.spotify.com/documentation/web-api/reference/personalization/get-users-top-artists-and-tracks/){:target="_blank" rel="noopener"}
and it needs to be built as soon as someone signs up to the site. In future, infographics can be built for existing
users as a batch job before release.

My favourite chart that got cut is Months in Music. Spotify Wrapped was just telling me my top 5 genres,
Modern Rock, Indie, Electronica, etc. etc. It's the same every year and it gets a bit stale. There might
even be a bias towards more generic genres, because they encompass sub-genres and artists can be tagged
with multiple genres. Months in Music displays your top three genres each month, relative to how much
you listened to that genre all year, which gives you a view of which genres were trending at the time.
It's kind of like [TF-IDF](https://en.wikipedia.org/wiki/Tf–idf){:target="_blank" rel="noopener"} but with genres.

<figure>
    <img src="{{ site.baseurl }}public/images/months_in_music.png" alt="Months in Music">
    <figcaption><i>See if you can guess which month I saw Hamilton</i></figcaption>
</figure>

I'm hoping it will be able to make a return at some point, along with info about which artists you like are
actually within that genre.

The other two things I learned are:

- If you think you're going to be using a web app at any point, you should probably start with a web app framework and
work backwards. Starting with just SQLAlchemy and leaving frontend design until later was like building
a bungalow and then deciding halfway through that you would actually quite like that extra storey.
- Making images programmatically is like making a website while only using absolute positioning for everything
and should be avoided like the plague. I think there's potential to create a wrapper around `PIL` to make the process
of building an image easier, but it's also questionable whether these infographics need to be images at all (main
benefit being how easy it is for users to share and keep the image).

Magic Playlists Roadmap:

- Email Updates when new infographics are ready
- Magic Playlists shared between friends, e.g. songs two people have in common
- Regular Infographics
- Releasing the code as open source

Check out the site at [music.rachsharp.co.uk](https://music.rachsharp.co.uk)

<figure>
    <iframe class="iframe-img" src="https://open.spotify.com/embed/user/rachel94sharp/playlist/67lBTn759313qtfzawWcxz" width="350" height="380" frameborder="0" allowtransparency="true" allow="encrypted-media"></iframe>
    <figcaption>Automatic! Systematic! Hydromatic!</figcaption>
</figure>
