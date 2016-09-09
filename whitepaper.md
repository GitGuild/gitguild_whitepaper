# Git Guild Blockchain

*A Git Guild is a git-native contract and payment network.*

Git is a flexible version control system in use by tens of millions of collaborators worldwide. Git [stores data](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) in sha256 hash trees, and [supports](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work) gpg signing of commits. With strict usage guidelines, these cryptographic utilities create a blockchain made up of gpg-signed, sha256-hashed commits.

## Proof of Labor

A chain of signed commits is hearafter referred to as Proof of Labor. Proof of Labor is a flexible, meritocratic mining system.

Each block of data is made up of a pgp-signed git commit. This commit consistitutes some Labor performed by one or more members of the guild. The other members of the guild then vote on accepting the PR or not by merging the commit or creating a fork.

Once a commit has been merged into the master branch, it may still be reverted if the charter allows for such votes. In this way, Proof of Labor chains are not completely immutable. Consideration for this must be made when writing charters and voting rules.

#### Crypto

While this paper has not yet been reviewed by cryptographers, it should not be controversial. Proof of Labor uses standard, well established tools in a downright orthodox way.

The following rules of git and other software usage are suggested to be observed by all guilds.

| Rule | Is Required | Description |
|-------|----------------|----------------|
| Must sign | yes | Every commit must be signed by a signature of a member to be considered for merge. |
| Member branches | Yes | A member's branch is their vote on the state and contents of the chain. Only the member is allowed to sign their own branch. |
| Sync all members | Yes | Each member must respect the longest valid master branch and the longest valid member branch of each other member in the master branch member.csv file. |
| Must merge votes | Yes | Any new master block must include every passing vote. Basically, master branch is consensus of valid votes. |
| Merge conflict resolution | Yes | Votes that are valid but behind the master branch must be merged, if there are no conflicts. |
| Vote keywords | No | For automation and standards, some universal voting keywords could be created. I.e. charter, member, payment |

Currently a trusted git server is used as the source of truth. The master branch must always be kept in a signed and complete state. Git's hash tree is also independently maintained by each member, serving as a complete, independent record of events. There are many ways this basic hosting setup can be enhanced, such as inserting the master commit hashes into a proof of work blockchain.

## Creating a Git Guild

A Git Guild is created by the members of a team, to serve as a contract for cooperation. The team's membership, stake, and voting are tracked and enacted in a data repository. [example data dir](https://github.com/isysd/gitguild_whitepaper_data)

To register a guild, the following rough steps are required.

1. Create data repository and link it as a submodule in each working repository.
2. Populate data repository with charter, member file, and ledger.
3. Founding members sign commit creating the genesis block.
4. Genesis block merged to master of data repository.
5. Genesis block of data repository referenced in submodule links.

#### Membership

Members apply to a guild by creating branch of data repository master, adding their gpg key to the member file, and submitting the gpg-signed result as a PR. The existing members then vote, and, if the approval threshold is reached, the PR is merged, accepting the member into the guild.

#### Voting

Voting "Yes" is done by merging and gpg signing a vote proposal. Voting "No" is done by merging and gpg signing a corresponding dissent branch.

Voting rules and thresholds should be described in the guild charter. For now, vote types can be described with a percent (%) approval threshold, as well as min and max vote times. For a vote to be merged, the threshold must be met within the vote time window after first submission.

## File Structure

For a repository to be conforming Git Guild, it must have the following file structure:

| Location | Description |
|----------|-------------|
| .gg/     | Git Guild data directory. A git [submodule](http://www.git-scm.com/book/en/v2/Git-Tools-Submodules). |
| .gg/charter.md | Charter for the project |
| .gg/members.csv  | Registry of members, with public keys |
| .gg/ledger.dat | A ledger of accounts for this project. Record of all credits, debits, and promises. |
| .gg/contracts/ | Directory for contracts to be governed by the guild. |

#### Charter

The Charter is pretty arbitrary, as it is supposed to be a natural language contract. The one thing that is absolutely essential to function as a guild are voting rules. These should be specified in a table, like so.

| Vote Type | Approval Threshold | Min. Vote Time | Max. Vote Time |
|-----------|--------------------|----------------|----------------|
| Charter Amendment | 90%        | 1 month        | 6 months       |
| Accept member | 66%        | 1 minute          | 1 month       |
| Accept PR | 51%        | 1 minute          | 3 months       |

It is not yet clear what rules may exist for guild charters and contracts. It is certainly possible to write broken ones, as well as good ones. Use your discretion, and the consent of your members to write mutually agreeable contracts.

#### Members

So say we have a simple project withi a single member. Our members.csv might have content that looks like this:

| Name | Keyfp | Status |
|------|------|------------|
|isysd | 9D85 DCE3 5BD7 3BE6 6233  C3E0 0E16 B546 5C35 86F6 | active |

If all of these files are up to date, and signed by sufficient members, the repository is considered in good standing.

#### Ledger

Each guild should keep a ledger file using the [ledger-cli](http://ledger-cli.org) data format. This double entry accounting system is both human readable and mathematically accurate. Furthermore it is arbitrary enough to track or even create any currency, commodity, or digital token.  

For example, the ledger for the guild governing this paper has the traditional currency $ (US Dollars), XGG (Guild coins, a transferrable digital commodity), and finally XP (a non-transferrable voting token).

```
; -*- ledger -*-

; XP (Experience Points) are for reputation and voting. Cannot be transferred.
; Though they have no monetary value, XP are earned 1:1 with XGG. The exception
; is 1 XP which is issued to the founding members.

; XGG Guild Coins are claims against the Assets:Treasury.
; Any member may unilaterally transfer their XGG to another by submitting a new
; entry to this ledger. XGG may only be redeemed with fees and vote approval,
; as specified by the charter.

2016/09/07 19:00:00 * Founder Block
    Assets:Treasury               $1
    Grant:GitGuild:Infrastructure -$1
    XP:Issued                     -1 XP
    XP:Earned:isysd               1 XP
    XGG:Issued                    -1 XGG
    XGG:Earned:isysd              1 XGG

P 2016/09/07 19:00:01 XGG $1
```

These choices are optional, but not arbitrary. Great care should be taken in defining your guild economics.

Transactions can be made by making a new, accounting valid transaction in the ledger. We won't go into the rules of general accounting, since ledger-cli will already enforce these. Consult a professional accountant if in doubt.

## Living Document

Don't take our word for it. This paper is governed by [a guild](https://github.com/GitGuild/gitguild_whitepaper_data/tree/master). Try the walkthroughs below to demo and register with the founding guild.

 + [register](https://github.com/GitGuild/gitguild_whitepaper/blob/master/tutorials/register.md)
 + [vote](https://github.com/GitGuild/gitguild_whitepaper/blob/master/tutorials/vote.md)
 + [count votes](https://github.com/GitGuild/gitguild_whitepaper/blob/master/tutorials/vote_counting.md)

