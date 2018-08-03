---
title:  "How To: Migrate To Bitbucket"
date:   2018-08-01 11:00:49 -0700
categories: how-to
---

## Where are you moving from?
It doesn't really matter which repository hosting platform you are moving from.  The steps to move from gitolite, github, gitlab, or others to Bitbucket is more or less the same.  The only differnce is if you were to use the import repository tool.  We won't be covering that tool here.

Do you have multiple developers working on the repo you are trying to move?  That's fine we'll just be asking them to stop committing work for about 5 minutes for their own good.  If they complain, we'll just revoke their repo access for the day....kidding.

## The Setup
To move forward you will need the following setup:

* A Bitbucket account (duh)
* Your public SSH key (id_rsa.pub) added to your account
* A new, blank repository created; we'll call it 'myrepo'

While you don't strictly have to use a ssh key, I find it much more conveienent.  If you are using HTTPS you will notice that the url to access bitbucket will be different.  Then what we are using here.

## Migrating existing repo to Bitbucket
On the command line, get to a place you can create a temporary repo. `source.example.com` is the domain of your current/old repo host. You may or may not have a `<team>` so you may just be able to leave that out in step 4. Bitbucket will give you the clone url anyway if you are confused.  Just be sure its the SSH version.

1. Request all developers to stop pushing to the repo.  The can continue to pull/commit/fetch. Just no pushing or their work may be lost.
2. `git clone --mirror git@source.example.com:myrepo.git myrepo-mirror`
3. `cd myrepo-mirror`
4. `git push --mirror git@bitbucket.org:<team>/myrepo.git`
5. `cd ..`
6. `rm -rf myrepo-mirror`

That's it! now your _myrepo_ is now hosted on bitbucket.

## Updating local repositories
Now you and your fellow developers and devops need to update your local git repositories.  This is really simple; a one-liner really.  It's always good to verify so it will actually be 3 steps. 

In your local repository do:
1. `git remote -v`  (Notice that the response is your fetch and pull URLs)
2. `git remote set-url origin git@bitbucket.org:<team>/myrepo.git`
3. `git remote -v` (You should see the two URLs now updated to bitbucket)

_ *Note* If you are using HTTPS url from bitbucket, use that URL format in step 2._

## What about the old repository host?
Personally, I like to remove all write permissions to the repositories I've moved. I leave them up as read-only for a few weeks. After those few weeks have passed I'll either archive or delete it.