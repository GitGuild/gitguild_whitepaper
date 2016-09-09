## Registration Walkthrough

__WARNING: the example output in this section is guaranteed to be out of date__

First, start with some git configuration, to set up your identity. Specifically, you'll want to set an email and matching [gpg key](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work).

```sh
$ git config --global user.email "troll4u@gitguild.com"
$ git config --global user.signingkey 69F7F1A6
```

Congratulations, you're now a git power user. Clone the repository you'd like to collaborate on. I.e. this one.

```sh
$ git clone --recursive https://github.com/isysd/gitguild_whitepaper.git
$ cd gitguild_whitepaper
```

This is the main working repository. In this case, the work done here is the publication of documents like this one. For other guilds, the work could be source code, media files, or other data types.

Close inspection also shows a git submodule in the `.gg` directory. This is the data repo for the guild. Its commits form our Proof of Labor chain, and each must be signed and merged into a threshold percentage of member branches.

```sh
$ cd .gg
$ git log --show-signature -1
commit aea8cefd2f4ccd504f2e271a5b4cd087c5064b69
gpg: Signature made Wed 07 Sep 2016 09:47:14 PM EST using RSA key ID 5C3586F6
gpg: Good signature from "Ira Miller (Strong Git Guild id key) <ira@gitguild.com>"
Author: isysd <ira@deginner.com>
Date:   Wed Sep 7 21:47:14 2016 -0500

    found the guild
```

This shows that the signature is good, and checking the only contemporary member branch (isysd) shows the same good signature on the same commit. This is the genesis block of our chain.

Who is this isysd fellow anyway, though? And what about the contents of this directory itself? The best place to start is by reading the [charter](https://github.com/isysd/gitguild_whitepaper_data/blob/isysd/charter.md). As it will tell you, members like isysd must be registered in the `members.csv` file.

```sh
$ cat members.csv
Name, Keyfp, Status
isysd, 9D85 DCE3 5BD7 3BE6 6233  C3E0 0E16 B546 5C35 86F6, active
```

Would you like to register with our guild? Simply create a branch, add your username and gpg key to the keyring, gpg sign the changes as a commit, then submit as a PR. Lets take that step by step.

Create a branch named after yourself. This example assumes your name is `troll4u`.

```sh
$ git checkout -b troll4u
```

Add your username and gpg key to the keyring.

```sh
$ echo "troll4u, 8F5E 2CD7 67CA D246 AD0C  AEDC 600E AD50 69F7 F1A6, active" >> members.csv
```

Commit your branch to the git server.

```sh
$ git add members.csv
$ git commit -S -m "registering myself with a guild"
$ git push origin troll4u
```

Note that you may not be able to commit to origin, in which case you need to use a fork instead of a branch.

Finally use a web browser to go to the [data repository on github](https://github.com/isysd/gitguild_whitepaper_data) and [create a Pull Request](https://help.github.com/articles/creating-a-pull-request/) from your branch into master.

That's it! You successfully submitted your application to be a guild member. Not so hard right? Don't worry. We'll automate it and create interfaces soon.  

If your membership application is approved by the current members, you will be eligible to earn and transact XGG, as well as earn and vote using XP.
