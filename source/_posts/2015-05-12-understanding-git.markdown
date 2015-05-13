---
layout: post
title: "Understanding Git: A Beginner's Guide (Part I)"
date: 2015-04-06 13:14:23 -0400
comments: true
categories: Flatiron School
---
Note - the first few paragraphs are not git related, so please feel free to skip ahead and ignore my ramblings.

In one of my previous incarnations, I worked as a business analyst at a financial tech startup here in New York. We were a small team, and sharply divided along product and tech lines. I spent my days counting down the clock, and cultivating severe developer envy. I wanted to be on the side that builds things, but I had no idea how to get there.

Anyway, every once in a while I would overhear someone on the tech side say they had submitted a pool request, which invariably evoked the image of a limpid pool in a fairytale clearing in the middle of a lush forest (very specific imagery I know). I had a vague idea that they were all requesting access to this magical pool where all code lives happily ever after. (No, i didn't think it through.)

It wasn't until last week that I figured out they had actually been saying pull, not pool. In any case, this post is about git, and my struggle to make sense of it. It's motivated by a strong suspicion on my part that you could get away with using git for a very long time without actually understanding how it works. Cloning, adding, comitting, and pushing are motions that are simple enough to go through, and there are loads of tutorials on how to set git up and get it working just enough to be lulled into a false sense of competence.

Which is not to say that there aren't loads of really great resources for learning git - there absolutely are. I just couldn't find any that tried explain it to me like I was a slower-than-average 5 year old with limited computer knowledge. In that spirit, this post will be very light on the technical sides of git and focus instead on conceptualizing the workflow. 

So what is git? git is a version control system. We all know that. But here's how I wish someone had explained the basics to me from the user's side of things, without going into under-the-hood mechanics:

You're sitting at your desk, working on your 3 volume memoir, when it hits you that the paragraph you deleted yesterday was the key to the whole story, and you would give anything to get it back. There should be a way to keep track of changes and be able to get back older versions!

But wait, there is! It's called git! Wonderful. So you download git, create a github account like everyone tells you to, run git init from the command line to get things going, then realize you have no idea where to go from there.

What happened when you ran git init? A new repository was created in the folder you're currently in, which you can think of as an empty directory waiting to store whatever you decide to put in it.

Ok. So you get back to work on your novel, and decide you want to store the current version in the new git directory, just in case you need to come back to it later. One catch: your directory doesn't know about that version yet. You need to create a channel between your version and the git directory by using git add novel. Once that's done, your git repository becomes aware of your version and you can store a copy of it by saying git commit* -m "first novel commit"*. The -m lets you add a message so you know what you've committed.

So far so good. Everytime you have a working version that you want to save, you can just go through the same process. Let's say you finished your novel and started proofreading, and want to commit any edits that you make. Running git status will tell you if there are any changes to your file that haven't been added or committed yet, so you can make sure not to miss anything.

So now you have a repository that stores all past versions of your novel. One catch: the repository is on your laptop, so if you throw it out the window in the throes of an existential crisis, well then that's the end of that. You need to store the repository remotely! That's where github comes in. It's one of many remote repositories you can use to store your commits somewhere safe.

And here's where things get fancy. You decide to collaborate on your memoir with your long lost evil twin, but the two of you can't be in the same room together. What's to be done? You send her a link to your git repository, and tell her to fork and clone it. She is initially offended but eventually understands that this is git speak for creating her own copy of your repository on github and as well as locally. Once she's done that, she'll have the most up to date version of your book. But wait! What does it mean for you both to have a copy of the book? What if you both make changes to the same paragraph? Who has the master version? How is this going to be ok?

All that's for another day folks. Part II coming soon!
