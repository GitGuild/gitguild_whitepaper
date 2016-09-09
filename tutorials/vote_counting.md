## Vote Counting and Merging

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

User isysd has 1 XP out of the total 1 XP that have been issued. This means that isysd has 1 vote, and no other users have any. If another user had 1.1 XP, then that user would have 1.1 votes.

For the troll4u application, the isysd vote gives us a 100% yes. The PR must therefore be merged, by the rules of the charter.

```
$ git checkout master
$ git merge --verify-signatures -S troll4u
$ git push origin master
```

The vote is complete, and the resulting action has been commited to the chain. Our applicant troll4u is now a member. As a member, troll4u is entitled to the full benefits and responsibilities of membership described in the charter. Primarily, they are able to earn XP and XGG by making contributions. Until troll4u earns some XP, their votes will not count.
