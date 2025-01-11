+++ 
ShowToc = false
date = 2023-10-01T12:00:00Z
description = "A guide on using act to run GitHub Actions locally."
tags = ["github", "actions", "docker", "development"]
title = "Running GitHub Actions Locally with act"

+++

Hey there! Let’s talk about something that’s been a game changer for me: 
`act`. It basically lets you run your GitHub Actions on your machine 
without a whole lot of configuration—none for me!

So if you're too excited to check this out on your own, check out 
`act` [here](https://github.com/nektos).

OR if you want a personalized tutorial on how I went about it, there 
you go...

I wanted this for a `loooong time`. If you’re like me, you’ve probably 
spent way too much time pushing commits with names like “testing action 
1” to “testing action 1001” because I can't get these configs right 
the first time. 😭 It’s a drag, right?

## Why Use `act`?

`act` lets you run your GitHub Actions locally using Docker. This means 
you can test your workflows without waiting for them to run on GitHub. 
It’s a huge time-saver and a life-saver if you're writing complex 
workflows!

## Getting Started

- Install it however you like; follow the instructions 
  [here](https://nektosact.com/installation/index.html). I installed it 
  using GitHub CLI (don't hate me for using it).

  ```bash
  gh extension install https://github.com/nektos/gh-act
  ```

- Create your workflow in the `.github/workflows` directory, however 
  you want it.

- Run `act` to execute every action, or you can do something specific 
  like `act push` to run all the actions triggered by a push. You can 
  do a lot [more](https://nektosact.com/usage/index.html).

- If you installed it using GitHub CLI like me, just do `gh act ...`.

NOW HERE'S a problem I faced on my machine: if you don't get this, 
you're done!

If Docker on your machine needs `sudo` access (like mine does), you 
might hit a snag. When you try to run `gh act`, it won’t have access 
to Docker. So, what do you do? 

### The Simple Hack

Here’s a quick fix: use `sudo -E` to run commands while keeping your 
user context. This way, you can run `gh act` with Docker access 
without any hassle. 

- **Run Your Workflow**: Use this command:
  
  ```bash
  sudo -E gh act push
  ```

The first run might take a bit since it downloads everything (images), 
but after that, it’s smooth sailing. No more endless commits just to 
test your actions!

So there you have it! With `act`, you can save time and avoid the 
hassle of pushing multiple commits just to test your actions. If you 
want to learn more, check out the official documentation for `act` 
[here](https://nektosact.com/usage/index.html).
