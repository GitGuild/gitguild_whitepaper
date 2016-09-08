# Git Guild Blockchain

*A Git Guild is a git-native contract and payment network.*

Git is a flexible version control system in use by tens of millions of collaborators worldwide. Git [stores data](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) in sha256 hash trees, and [supports](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work) gpg signing of commits. With strict usage guidelines, these cryptographic utilities create a blockchain made up of gpg-signed, sha256-hashed commits.

### Creating a Git Guild

A Git Guild is created by the members of a team, to serve as a contract for cooperation. The team's membership, stake, and voting are tracked and enacted in a data repository. (See Data Directory section)

To register a guild, the following rough steps are required.

1. Create data repository and link it as a submodule in each working repository.
2. Populate data repository with charter, member file, and ledger.
3. Founding members sign charter, member file, and ledger, creating the genesis block.
4. Genesis block merged to master of data repository.
5. Genesis block of data repository referenced in submodule links.

### Proof of Labor

A chain of signed commits is hearafter referred to as Proof of Labor. Proof of Labor is a flexible, meritocratic mining system.

Each block of data is made up of a pgp-signed git commit. This commit consistitutes some Labor performed by one or more members of the guild. The other members of the guild then vote on accepting the PR or not by merging the commit or creating a fork.

Once a commit has been merged into the master branch, it may still be reverted if the charter allows for such votes. In this way, Proof of Labor chains are not completely immutable. Consideration for this must be made when writing charters and voting rules.

## Membership

Members apply to a guild by creating branch of data repository master, adding their gpg key to the member file, and submitting the gpg-signed result as a PR. The existing members then vote, and, if the approval threshold is reached, the PR is merged, accepting the member into the guild.

### Voting

Voting "Yes" is done by merging and gpg signing a vote proposal. Voting "No" is done by merging and gpg signing a corresponding dissent branch.

Voting rules and thresholds should be described in the guild charter. For now, vote types can be described with a percent (%) approval threshold, as well as min and max vote times. For a vote to be merged, the threshold must be met within the vote time window after first submission.

| Vote Type | Approval Threshold | Min. Vote Time | Max. Vote Time |
|-----------|--------------------|----------------|----------------|
| Charter Amendment | 90%        | 1 month        | 6 months       |
| Accept member | 66%        | 1 minute          | 1 month       |
| Accept PR | 51%        | 1 minute          | 3 months       |

##### Crypto

While this paper has not yet been reviewed by cryptographers, it should not be controversial. Proof of Labor uses standard, well established tools in a downright orthodox way.

Git's hash tree is independently maintained by each member. 

1. Every commit must be signed by a signature of a member to be considered for merge.
2. Each PR must meet the approval threshold vote by being merged into sufficient member branches.
3. Currently a trusted git server is used as the source of truth. The master branch must always be kept in a signed and complete state. Member branches are the responsibility of each member.

### Economics and Finance

Each guild should keep a ledger file using the [ledger-cli](http://ledger-cli.org) data format. This double entry accounting system is both human readable and mathematically accurate. Furthermore it is arbitrary enough to track or even create any currency, commodity, or digital token.  

For example, the ledger for the guild governing this paper has the traditional currency $ (US Dollars), XGG (Guild coins, a transferrable digital commodity), and finally XP (a non-transferrable voting token).

These choices are optional, but not arbitrary. Great care should be taken in defining your guild economics.

Transactions can be made by making a new, accounting valid transaction in the ledger. We won't go into the rules of general accounting, since ledger-cli will already enforce these. Consult a professional accountant if in doubt.

##### Registration Walkthrough

__WARNING: the example output in this section is guaranteed to be out of date__

Don't take our word for it. This paper is governed by a guild. Try the commands below to demo and register with the founding guild.

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

Who is this isysd fellow anyway, though? And what about the contents of this directory itself? The best place to start is by reading the charter. As it will tell you, members like isysd must be registered in the `members.csv` file.

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

##### Voting Walkthrough

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

##### Vote Counting and Merging

Now the origin repository is ready for vote counting. The first thing we need to know is how many votes are required? The ledger balance will tell us how many XP (voting tokens) have been issued, and to whom.

```sh
$ ledger -f ledger.dat bal
                  $1  Assets:Treasury
                 $-1  Grant:GitGuild:Infrastructure -$1
                   0  XGG
               1 XGG    Earned:isysd
              -1 XGG    Issued
                   0  XP
                1 XP    Earned:isysd
               -1 XP    Issued
--------------------
                   0
```

For the troll4u application, the isysd vote gives us a 100% yes. The PR must therefore be merged, by the rules of the charter.

```
$ git checkout master
$ git merge --verify-signatures -S troll4u
$ git push origin master
```

The vote is complete, and the resulting action has been commited to the chain. Our applicant troll4u is now a member. As a member, troll4u is entitled to the full benefits and responsibilities of membership described in the charter. Primarily, they are able to earn XP and XGG by making contributions. Until troll4u earns some XP, their votes will not count.