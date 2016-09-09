## Voting Walkthrough

__WARNING: the example output in this section is guaranteed to be out of date__

Now that our guest has applied to the guild, lets switch to isysd's perspective. As the sole contemporary member of the guild of our example, the vote counting will be very easy.

For pretend sake, lets assume the following magic commands put us in the right context of isysd's command line environment for our example.

```
$ su isysd
$ cd gitguild_whitepaper/.gg
```

Lets sync our local git repo with the remote. We should be able to discover a new branch from our applicant. Unfortunately, this discovery is not easily automated

```
$ git fetch origin
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/isysd/gitguild_whitepaper_data
 * [new branch]      troll4u    -> origin/troll4u
$ git checkout --track origin/troll4u
Branch troll4u set up to track remote branch troll4u from origin by rebasing.
Switched to a new branch 'troll4u'
```

OK, now lets check out the application from this troll4u character. First, their member entry should have their name, key fingerprint, and the status active.

```sh
$ tail -n 1 members.csv 
troll4u, 8F5E 2CD7 67CA D246 AD0C  AEDC 600E AD50 69F7 F1A6, active
```

Look good. Lets assume we can get and confirm their key from a keyserver lke pgp.mit.edu.

```sh
$ git log --show-signature -1
commit 6ac332b120f1b7f93bb97370d0ee3707ac2a4445
gpg: Signature made Thu 08 Sep 2016 12:34:17 AM EST using RSA key ID 69F7F1A6
gpg: Good signature from "Troll 4 U (Troll gitguild account) <troll4u@gitguild.com>"
Author: troll4u <troll4u@gitguild.com>
Date:   Thu Sep 8 05:34:17 2016 +0000

    registering myself with a guild
```

So it is a complete, properly signed application. We have a user, with their own member branch, and a signed commit with their member info. Since this is our example account, we'll have isysd go ahead and vote to approve the application by merging the commit.

```sh
$ git checkout isysd
$ git merge --verify-signatures -S troll4u
Commit 6ac332b has a good GPG signature by Troll 4 U (Troll gitguild account) <troll4u@gitguild.com>
Updating aea8cef..6ac332b
Fast-forward
 members.csv | 1 +
 1 file changed, 1 insertion(+)
$ git push origin isysd
```

Vote submitted.