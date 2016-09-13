## Vote Counting and Merging

Now the origin repository is ready for vote counting. The first thing we need to know is how many votes are required? The ledger balance will tell us how many XP (voting tokens) have been issued, and to whom.

```sh
$ ledger -f ledger.dat bal
               -1 XP  Experience
              -1 XGG  Liabilities
               1 XGG
                1 XP  m:isysd
               1 XGG    Assets
                1 XP    Experience
                   0  v
                   0    Agreement
                   0      TGG_Transition
               -1 XP        isysd
                1 XP        yes
                   0      XGG
               -1 XP        isysd
                1 XP        yes
                   0    Ambassador:isysd
               -1 XP      isysd
                1 XP      yes
                   0    Member:troll4u
               -1 XP      isysd
                1 XP      yes
--------------------
                   0
```

User isysd has 1 XP out of the total 1 XP that have been issued. This means that isysd has 1 vote, and no other users have any. If another user had 1.1 XP, then that user would have 1.1 votes.

For the troll4u Member vote, isysd's 1 XP out of the total 1 XP gives us a 100% `yes`. The PR must therefore be merged, by the rules of the charter.

```
$ git checkout master
$ git merge --verify-signatures -S troll4u
$ git push origin master
```

The vote is complete, and the resulting action has been commited to the chain. Our applicant troll4u is now a member. As a member, troll4u is entitled to the full benefits and responsibilities of membership described in the charter. Primarily, they are able to earn XP and XGG by making contributions. Until troll4u earns some XP, their votes will not count.
