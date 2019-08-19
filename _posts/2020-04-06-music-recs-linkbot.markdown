---
layout: post
title:  "Music Recs linkBot"
date:   2019-07-30 03:45:02 -0700
categories: blog
tags: vue javascript discord heroku firebase github 
---

Music Recs linkBot is a discord bot that creates a webpage out of music links posted to a discord channel. It sorts these links into categories based on the major web-based music services:

- Youtube
- Spotify
- Bandcamp
- Soundcloud

The idea is that this makes it easier to browse through the history of what music has been posted to channel. You can see an example of the site that linkBot produces here: [Music Recs](https://alexharris.online/https://alexharris.online/musicRecsLinkBot/).

linkBot consists of two parts:

<div class="columns">
  <div class="column">
    <div class="box">
        <h2>linkBot</h2>
        <p>linkBot is a a discord bot, hosted on heroku, that listens in a channel, gets any links that are posted, sorts them, and writes the data to firebase.</p>
        <a class="button is-link is-outlined" href="https://github.com/alexharris/linkBot">View the repo</a>
    </div>
  </div>
  <div class="column">
    <div class="box">
        <h2>Musicrecs</h2>
        <p>musicrecs is a vue.js page hosted on github pages that gets the data from firebase and displays them by category.</p>
        <a class="button is-link is-outlined" href="https://github.com/alexharris/musicrecs">View the repo</a>    
    </div>
  </div>
</div>

It basically works like this:

![linkbot diagram](/assets/linkbot_diagram.svg)




