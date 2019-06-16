---
layout: post
title:      "Marvel Meets Coding"
date:       2019-06-16 12:44:14 -0400
permalink:  cli_project_-_comic-cli
---


For 8 months now I have dreamed about creating an extensive project. After all that's what Software Engineering is about, right? Creating code, fixing bugs, and solving problems. That is all true however, staring at a blank text editor with no ideas, no tests, no guidelines of how to complete this task started to become very overwhelming.

This Blog Post is about the journey of completing my very first software engineering project. I knew the project was coming up and two weeks prior to start date my anticipation started to grow. Ideas were running through my head, and they all seemed like winners. That feeling came to a screeching halt on day one of project week. 

It seems like decades ago when the Slack post with the project guidelines were released. I remember eagerly waiting to read what I was going to have to complete. The feeling of excitement dropped as soon as I read the requirements. The Project was about scraping and all of my ideas were about cli games. However I believe this was the easiest problem to solve in my whole project. I spent the next 2 hours brainstorming ideas, thinking of websites I used regularly and things I wish were more accessible.[Marvel Comics](http://www.marvel.com/comics?&options%5Boffset%5D=0&totalcount=12) became a clear decision. I quickly found a well designed website that showed newly released comics and had a second level with more information to scrap. Marvel Comics would become the data supply for my project.

![](http://media.giphy.com/media/vBjLa5DQwwxbi/giphy.gif)

The project objective was to create a gem that scrapes two levels of a website and displayed them in a command line interface to the user. Now that I had decided on my web page it was time to start building my gem. Looking at an empty text editor quickly became nauseating. 

![](http://media.giphy.com/media/AiEr9b7sX5VKIoIvQL/giphy.gif)

I had to break it down and take the project one step at a time, or one git commit at a time if your a software engineer. That proved to be the key to the puzzle that was this project.

Step one was to create the gem and get it required correctly. Doing that process sets the files up allowing you to output a string to the command line from any file in the directory. From there I began to work backwards creating code I wish I could use to then developing methods to make it possible. Step by step the text editor went from a blank slate to quickly filling up. Eventually I was able to get the first output completed, and my enthusiasm quickly returned. At that moment I began to form a rough draft of a command line class. At this point the information I wanted to display started to fall into place. This made the next step easier... I could simply create a Comic class that would instantiate the objects I wanted to present. Knowing what information I needed to display made creating the Comic Class effortless.

Finally after the Comic class was set up I needed to scrape my first level of data.I repeated the process for the second level of scraping (using a url I scraped from the first level of course). Before long I had a functioning (very buggy) command line scrapper assembled. With a ton of trial and error I made my command line look the way I envisioned and implemented edge casing (caps and invalid responses). The final step was shortening and refactoring the code to be as clean, dry, and readable as possible.

![](http://media.giphy.com/media/1Zp8tUAMkOZDMkqcHb/giphy.gif)

The results were a functioning project that I am very proud of, but more importantly I gained a massive amount of knowledge, renewed confidence in my code, and refreshed my sense of enthusiasm for software engineering. This project was, without any doubt, the hardest coding challenge I have had to overcome. It has left me anticipating the next section of coding and next project.



