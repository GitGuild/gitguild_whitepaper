# Git Guild Blockchain

*A git guild is a git-native contract and payment network.*

__Document version 0.1.0__

## Abstract

Git is a flexible version control system in use by tens of millions of collaborators worldwide. Git [stores data](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) in sha1 hash trees, and [supports](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work) gpg signing of commits (since v1.7.9). With strict usage guidelines, these cryptographic utilities create a blockchain made up of gpg-signed, sha1-hashed commits.

## Proof of Labor

A chain of signed commits is hearafter referred to as Proof of Labor. Proof of Labor is a flexible, meritocratic mining system.

Each block of data is made up of a pgp-signed git commit. This commit consistitutes some Labor performed by one or more members of the guild. The other members of the guild then vote on accepting the PR or not by merging the commit or creating a fork.

Once a commit has been merged into the master branch, it may not be reverted or overwritten without being detected by other members of the guild. Git guild are immutable unless a fork aka rebase is explicitely voted for.

#### Crypto

While this paper has not yet been reviewed by cryptographers, it should not be controversial. Proof of Labor uses standard, well established tools in a downright orthodox way.  

GNU Privacy Guard aka GPG is a popular encryption program first released in 1999. GPG is open source software package compliant with [RFC 4880](https://tools.ietf.org/html/rfc4880), which is the IETF standards track specification of OpenPGP. The latest version (2.0.30) supports RSA, ElGamal and DSA signing, as well as a number of ciphers, hashes, and compression algorithms. It is commonly used for encrypting and/or signing emails, and is growing in use in the git community.

Git was developed as a file system, by Linus Torvalds, in 2005. The SHA-256 hash tree that git uses was introduced in that first version. Since git has become one of the most popular software projects of all time, this hash tree has seen a lot of real world use, as well as development. Key to the hash tree is that all files are hashed into a commit, along with the parent commit(s). Git's hash tree is a directed acyclic graph. This means it is a one directional, immutable tree with branches and branch resolution. Loops, rewriting history, and a number of other logical inconsistencies are prevented.

Since git v1.7.9, released in 2012, git has supported GPG signing commits. This release was the final technical advancement needed to allow Proof of Labor.

## Git Usage

The following rules of git and other software usage are necessary for the Proof of Labor model to function.

| Rule | Is Required | Description |
|-------|----------------|----------------|
| Must sign | yes | Every commit must be signed by a signature of a member to be considered for merge. |
| Member branches | Yes | A member's branch is their vote on the state and contents of the chain. Only the member is allowed to sign their own branch. |
| Sync all members | Yes | Each member must respect the longest valid master branch and the longest valid member branch of each other member in the master branch member.csv file. |
| Must merge votes | Yes | Any new master block must include every passing vote. Basically, master branch is consensus of valid votes. |
| Merge conflict resolution | Yes | Votes that are valid but behind the master branch must be merged, if there are no conflicts. |
| Vote keywords | No | For automation and standards, some universal voting keywords could optionally be created. I.e. charter, member, XP |

The last rule Vote keywords is important, but the implementation specifics can vary quite broadly. To optimize the reference software implementations, as well as to improve human understanding, some defaults have been chosen for the sake of this paper. These include those mentioned above: `charter`, `member`, `XP`.

#### Branches

Proof of Labor uses git branches for its data structure, and so needs strict control of these. The following keywords are reserved branch names.

| Branch             | Maintainer       | Description           |
|--------------------|--------------------|------------------------|
| master             | Last to commit  | The master branch is a protected communal branch. It reflects the latest valid state of the sum of all member branches. |
| staging            | Last to commit  | The staging branch is an unprotected communal branch. It reflects commits which incompleted in some way, and are waiting for some more work to be complete. |
| username             | username  | Each member must maintain their own branch, and use this branch for submitting votes (commits) or voting on the commits of others (merging). |

Currently a trusted git server is used as the source of truth. The master branch must always be kept in a signed and complete state. Git's hash tree is also independently maintained by each member, serving as a complete, independent record of events. There are many ways this basic hosting setup can be enhanced, such as inserting the master commit hashes into a proof of work blockchain.

#### Sidechains

A sidechain is a pretty loose concept in a guild, basically equivalent to a git submodule. Git submodules are hashed checkpoints between repositories, and can serve as accounting anchors. A git submodule is represented in the `.gitmodules` file as a relative path and a URL to another repository.

```
[submodule "sidechains/DashMNGuild"]
        path = sidechains/DashMNGuild
        url = https://github.com/GitGuild/DashMNGuild.git
```

Depending on how your git software is configured, and the specific commands used, this path may show an empty directory, or a populated git clone of the url shown. Going to the relative path in the git tree gives us a hash to a specific commit in the other repository's chain.

```
Subproject commit 3a6296807e286b015f5b0f06ebb9eacd92e51dda
```

While the consensus rules around sidechains are still being worked out, it seems that one temporary rule will ensure safety: all commits are required to sync sidechains with a maximum depth of one. That is, if your guild references a sidechain, members are required to keep the reference to that chain up to date, but are not required to dig into any sidechains referenced further down the tree. This prevents loops, and simplifies commit validation.

The most common usage of this feature is to import assets and transactions between ledgers. It can also be used to escrow transactions or sign contracts across chains.

## File Structure

For a repository to be conforming guild, it must have the following file structure:

| Location | Description |
|----------|-------------|
| .gg/     | guild data directory. A git [submodule](http://www.git-scm.com/book/en/v2/Git-Tools-Submodules). |
| .gg/charter.md | Charter for the project |
| .gg/ledger.dat | A ledger of accounts for this project. Record of all assets, liabilities, and votes. |
| .gg/assets/ | A directory of assets available for use on the ledger. |
| .gg/assets/CODE | An asset definition file for CODE. |
| .gg/agreements | A directory of all agreements active in this guild. |
| .gg/sidechains | A directory of all sidechains to this guild. |
| .gg/sidechains/Guildname | A sidechain for the guild Guildname, represented as a git submodule |
| .gg/members | A directory of member profiles. |
| .gg/members/username | A member profile named by username. |

If all of these files are up to date, and signed by sufficient members, the repository is considered in good standing. It is now eligible to be made into a commit and signed as a valid block.

#### Charter

The Charter is pretty arbitrary, as it is supposed to be a natural language contract. There are a number of bottlenecks, or points of centralization to consider in a guild charter. These are: git server(s), pgp keyserver(s), and optionally some legal contract or jurisdiction. These should be specified in an easy to read table.

| Trust Type | Name | Address |
|------------|------|---------|
| Git server | github | https://github.com |
| PGP server | MIT | https://pgp.mit.edu |

The second thing that is absolutely essential to function as a guild are voting rules.

Votes are counted by XP shares in the ledger, so be sure to distribute these to your guild members carefully. The following example vote table is taken from the Git Guild.

| Vote Type     | Example Threshold | Description |
|---------------|-------------------|-----------------------------------------------|
| Fork          | 90%               | Any break of the git chain, i.e. a rebase     |
| Charter       | 90%               | Any change to the charter.md file.            |
| Sidechain     | 75%               | Add, remove or approve a charter ammendment for a sidechain. |
| Agreement     | 66%               | Accept or amend a guild-wide agreement.                |
| Member        | 51%               | Accept or reject a member. Also change a member's XP. |

One of the hardest part of any Proof of Labor implementation will be vote counting. The above keywords are not required, but care should be taken in deviation, as it will probably break software.

It is not yet clear what rules may exist for guild charters and contracts. It is certainly possible to write broken ones, as well as good ones. Use your discretion, and the consent of your members to write mutually agreeable contracts.

#### Members

Members apply to a guild by creating branch of data repository master, adding a file with their gpg public key to the members directory, and submitting the gpg-signed result as a PR. The existing members then vote, and, if the approval threshold is reached, the PR is merged, accepting the member into the guild.

If your username is isysd, then your member file goes in `members/isysd.md`. This file is expected to have a section titled `##### Public Key` which contains your ascii armored GPG key. This key will look something like the (compressed) example that followed.

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: SKS 1.1.5
Comment: Hostname: pgp.mit.edu

mQINBFfQrQsBEACxSZOk0ZaH9J7jHogmDfji3T4eoFPYlYBx3Cw/PbzflRUlHzNYxVuM/48K
...
8IsDOQ5eZkXaEykwAUwNgA==
=YCLJ
-----END PGP PUBLIC KEY BLOCK-----
```

It will look like the above but with lots and lots more characters in the middle. You can get this via the command line by running `gpg --export -a my@email.com`.

As a non-member, you cannot have any XP, and cannot vote. Still, others will vote for or against your application both by merging your member file, but also by voting in the ledger.

```
2016/09/07 19:00:00 * troll4u application
    v:Member:troll4u:yes            1 XP
    v:Member:troll4u:isysd          -1 XP
```

Because usernames are used throughout the guild data structures, a number of names are forbidden. As of now, usernames may only contain letters, numbers, `-`, and `_`. Note that usernames are not case-sensitive. The following keywords are also reserved as forbidden usernames.

| Name               | Reason Forbidden       |
|--------------------|------------------------|
| master             | Keyword for the consensus branch. |
| staging            | Keyword for near consensus branch. |
| consensus          | Alias for the consensus branch. |
| voting             | Alias for near consensus branch. |



#### Ledger

Each guild should keep a ledger file using the [ledger-cli](http://ledger-cli.org) data format. This double entry accounting system is both human readable and mathematically accurate. Furthermore it is arbitrary enough to track or even create any currency, commodity, or digital token.  

Guilds should use some standard accounting keywords and rules. First, `Assets` refers to a credit that the parent has in their account, and `Liabilities` refers to obligations of the parent.

```
2016/09/07 19:00:00 * Founder Block
    Experience                    -1 XP
    Liabilities                   -1 XGG
    m:isysd:Experience            1 XP
    m:isysd:Assets                1 XGG
```

The asset `XP` and its origin account `Experience` are also reserved keywords for voting shares in any guild ledger. Member accounts are all children of the `m` account.

Also see that there is an `isysd` account as the parent of `Assets` in this transaction. This indicates that member `isysd` is the account owner. If no member is specified, then the `Assets` or `Liabilities` are against the guild Treasury.

##### Voting

In a strict Proof of Labor system, voting "Yes" is done by merging and gpg signing a vote proposal, while voting "No" is done by merging and gpg signing a corresponding dissent branch.

In addition to voting by merging and signing the commits of others, members are asked to make explicit vote entries in the ledger. This makes counting of votes a trivial accounting matter, rather than a complex search through a forrest of branches. Each member is allowed to vote `yes` or `no` for each of their XP, and must record the XP spent on the vote as shown below.

```
2016/09/07 19:00:00 * Voting Block
    v:Member:isysd:yes                 1 XP
    v:Member:isysd:isysd               -1 XP
    v:Agreement:XGG:yes                1 XP
    v:Agreement:XGG:isysd              -1 XP
    v:Agreement:TGG_Transition:yes     1 XP
    v:Agreement:TGG_Transition:isysd   -1 XP
```

The voting namespace starts with `v`, then has the vote type, which determines the approval threshold. Next is the subject of the vote, which is determined by the type. For instance, if the vote type is member, then the subject would be a member asking to join. On the other hand, if the vote type is an Agreement, then the subject is the name of the agreement.

The easiest way to track your votes is to spend them as you earn them. In the real ledger, the Voting Block above and Founder Block from the last section are one transaction.

Once another asset is imported (see sidechains), the two ledgers can address each other's accounts through the Guild Exchange (GX) ledger account. For example, to record a transaction against the DashMN Guild Treasury, the `GX:DashMNGuild` account would be used. If a specific user within the sidechain guild is being addressed, just append their account after the guild name. i.e. `GX:GitGuild:isysd`

```
2016/09/10 12:00:00 * Funding froom LegacyGG
    Assets                        1003.92 GDASH
    GX:DashMNGuild                -1003.92 GDASH
```

This is meant to reflect one side of the same double entry transaction, and the DashMNGuild would have a corresponding entry for `GX:GitGuild`.

```
2016/09/10 12:00:00 * Funding Block
    ; Actually control of DASH on Xh4GuTaGPZ9Rtjqm2ME1dY2g8toGhvWJdH
    Assets                        1003.92 DASH
    FX:LegacyGG                   -1003.92 DASH
    Liabilities                   -1003.92 GDASH
    GX:GitGuild                   1003.92 GDASH
```

Finally, the keyword `FX` (Foreign Exchange) is reserved for transactions where the other side is on a non-guild ledger. Examples of this are Bitcoin or SWIFT transactions. Above an incoming Dash transaction is shown depositing DASH into the guild Treasury.

## Summary

If this seems too easy, you're not alone. For software developers, git feels like water to a fish. It is challenging to discover something new about the environment you survive in.

The contributors to Proof of Labor are amazed at how broad the applications seem to be. The natural contracting language and strong reputation system of PoL are unique in a blockchain. Due to this broad scope, we have erred on the side of abstraction over specification. While Proof of Labor could be even more loose, and many alternate implementations are possible, we believe this is a flexible, minimal working ruleset.

Proof of Labor is complimentary with PoW systems in many ways, and could even be applied after the fact as a governance layer to blockchains like Bitcoin. It could be used as an oracle by other blockchains, or lend them sidechain functionality. It is not likely to be as fast or as directly scalable, but perhaps with advanced sidechain usage, even massive parallel processing could be possible. Most likely, however, an optimal hybrid will emerge after much experimentation.

#### Living Document

Don't take our word for it. This paper is governed by [a guild](https://github.com/GitGuild/GitGuild/tree/master). Try the walkthroughs below to demo and register with the founding guild.

 + [charter](https://github.com/GitGuild/gitguild_whitepaper/blob/master/tutorials/charter.md)
 + [register](https://github.com/GitGuild/gitguild_whitepaper/blob/master/tutorials/register.md)
 + [vote](https://github.com/GitGuild/gitguild_whitepaper/blob/master/tutorials/vote.md)
 + [count votes](https://github.com/GitGuild/gitguild_whitepaper/blob/master/tutorials/vote_counting.md)

