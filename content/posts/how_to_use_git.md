+++
title = 'How to start a git repo'
date = 2025-02-17T15:18:00-06:00
draft = false
tags = ["git", "github"]
categories = ["Git"]
+++

# Ok, for the last time Ross...

So I LOVE using git. Once you get over the initial learning curve of which commands do what, it's super easy to save your work and roll it back if you make mistakes.

More than that though, it's probably the most reliable way, I've found to take notes.

# The problem...

I always have to Brave search how to set up a git repo because I can never remember:
- Whether to use master or main
- Whether I need to set up the remote user or not
- When the ssh key comes into play
- etc.

Essentially, I go long enough between setting up repos that I NEVER remember this part and have to search it, so I want to run a little reminder for how I set up a github repo.

# STEP 1: Set up your keys:

Github refuses to let you login with a username or a password. Tell that to the freaking tutorials from 2016 though. The first thing you need to do is to:

1. Go into Github

2. Click on your username

3. Click on settings

4. Click on "SSH AND GPG KEYS"

5. Insert your public key. (If you don't have one, generate it with `ssh-keygen -t ed-25519` and cat the .pub file into your clipboard, then paste it into the new key section and save.

6. Add the key to your ssh agent:

```
eval $(ssh-agent)
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```
# Step 2: Set up a repo:

## Note: For some reason, you think the key section is linked to the repo, but the ssh key is for your account, not individual repo.

Now that you've got your login mechanism sorted, create the repo you want.

1. In the repositories section of github, click new.

2. Set the name, permissions, etc, and click "Create Repository."

3. run `git init`

4. run `git add .` to add all the files you want to the repo

5. run `git commit -m "first commit"` to commit your changes to the local repo copy.

6. run `git branch -M main` to set the branch to main.

7. run `git remote add origin git@github.com:username/repo_name.git`

8. run `git push -u origin main` to push all the files up.

# Step 3: Use git as normal.

Every time you change the files for your project and you want to save the changes.

1. run `git pull` to make sure your local copy is as up to date as possible. This prevents collisions.

2. run `git add .` to add all the files. You can also be more selective. Make sure to use .gitignore if you've got secrets.

3. run `git commit -m "some kind of message"` to save the change locally.

4. run `git status` to make sure you didn't miss anything.

5. run `git push` to send the changes to github.

# In Conclusion:

Seriously man, it's not that hard, but I know you'll forget it again in two weeks, so come back here instead of searching the web.
