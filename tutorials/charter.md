# Charter a New Guild

To charter a new guild, simply create the required file structure, and start signing every commit. Even existing repositories can be converted into a valid guild in this manner. As a reminder, the file structure is below.

| Location | Description |
|----------|-------------|
| .gg/     | Git Guild data directory. A git [submodule](http://www.git-scm.com/book/en/v2/Git-Tools-Submodules). |
| .gg/charter.md | Charter for the project |
| .gg/members.csv  | Registry of members, with public keys |
| .gg/ledger.dat | A ledger of accounts for this project. Record of all credits, debits, and promises. |

### Charter Existing Project

To charter a new guild for existing git project(s), the following rough steps are required.

1. Create data repository and link it as a submodule in each working repository.
2. Populate data repository with charter, member file, and ledger.
3. Founding members sign commit creating the genesis block.
4. Genesis block merged to master of data repository. (submodule link updated)

This is effectively the same as a sidechain, but is special because it keeps all governance, ledger, and other guild meta data out of the working repository.
