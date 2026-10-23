
---
title: "Notes on sorting music"
date: 2026-10-23T09:55:00
tags: ["Linux", "Notes", "Software"]
---

# Background

As a bit of background, I used to have an enormous music library (some 650GB of mixed quality MP3 files) stored on a local server that I could akwardly stream from (if the stars aligned).

Now this kind of setup is great, however there are a good few downsides to this approach, namely...

* Audio quality is fixed at whatever you originally sourced the files at
* Organisation and metadata is an ongoing nightmare
* [Bitrot](https://en.wikipedia.org/wiki/Data_degradation) is definitely a thing
* Tastes change, but the library is mostly static
* Keeping up with new releases is a full time job
* Pruning files is yet another job in itself
* Most of it just isn't listened to regularly
* There's no good middle ground between singles and albums
* Music is kind of awkward to _obtain_ versus TV shows and movies

So, with this what I decided to do was to work my way through each and every artist in my library, adding what I liked to a Spotify playlist that I'd [export out](/music/) every few months as a form of backup just in case anything happened with my account or Spotify at large.

# Exporting

Exporting your liked lists isn't the simplest of things.

There are a number of tools out there but the one I keep coming back to is [exportify](https://watsonbox.github.io/exportify/). Once you're logged in you're presented with your playlists and then you simply download the 'Liked' list and you're given a handy-dandy `liked.csv` that we're able to actually work on.

# Processing

As with most data and lists I work on, I've streamlined (ish) this process into [an unweildy script](https://github.com/breadcat/nix-configs/blob/main/scripts/blog-music.nix). Inside this you'll find how it converts CSV input into a reasonably coherent, sorted markdown table.

# Future expansion

Currently my library file holds no details regarding track metadata outside of Artist, Title and a link, it's just a list (hey, we like those!). At some point however I'd like to be able to append genres and styles to the list such as the following.

<details><summary>Genres</summary>
<ul>
<pre>
Blues\Electric
Blues\Rock
Blues\Traditional
Classical\Minimal
Classical\Traditional
Electronic\Aggrotech
Electronic\Ambient
Electronic\Downtempo
Electronic\Drum & Bass
Electronic\Electronica
Electronic\Glitch
Electronic\House
Electronic\IDM
Electronic\Industrial
Electronic\Minimal
Electronic\Neurofunk
Electronic\Psybient
Electronic\Synthpop
Electronic\Trance
Electronic\Trip-hop
Hip-Hop\Alternative
Hip-Hop\Electronic
Hip-Hop\Rap
Metal\Blackgaze
Metal\Drone
Metal\Glam
Metal\Groove
Metal\Heavy
Metal\Melodic Death
Metal\Nu
Metal\Post
Metal\Rap
Metal\Sludge
Metal\Symphonic
Metal\Thrash
Pop\Dance
Pop\Electro
Pop\Rock
Pop\Synth
Rock\Acid
Rock\Alt
Rock\Brit
Rock\Emo
Rock\Folk
Rock\Garage
Rock\Goth
Rock\Grunge
Rock\Hard
Rock\Indie
Rock\Industrial
Rock\Kraut
Rock\Math
Rock\Post
Rock\Post-Punk
Rock\Post-Rock
Rock\Progressive
Rock\Psychedelic
Rock\Punk
Rock\Shoegaze
Rock\Ska
Rock\Space
Rock\Stoner
Rock\Surf
Soundtrack\Anime
Soundtrack\Game
Soundtrack\Movie
Soundtrack\Remix
Soundtrack\Score
Soundtrack\Television
</pre>
</details>
