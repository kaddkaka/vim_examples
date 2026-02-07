I mostly use git from the command line, but there are some vim-git integrations that I really like, listed below.

For the repository where I spend the majority of my working time, I use `git worktree`s to manage several ongoing items:

 * `main` - worktree that tracks the main branch (readonly intention) 
 * `feature` - current feature work
 * `review` - for code review
 * `tmp`
 * `hack`

The worktrees are added from the primary worktree (e.g. `git worktree add ../feature`) and they let me have several branches checked out at the same time. I don't create new worktrees, I reuse the ones I have.

To work on a new feature I `cd` to `feature/`, create a new branch and launch my editor from there. 

These worktrees share one `.git` (only need to fetch once, and saves disk space compared to having several full clones) and they also share one stash (which means I can `git stash` in one worktree and `git stash pop` in another). They also share the same settings and git hooks. 

`git branch -vv` lists all my local branches and in what worktree they are checked out in.

To review a branch I use `git switch -d origin/feature_A_from_coworker` to avoid creating a local copy of the branch.

# git-jump
The git source contains several peripheral tools, [git-jump](https://github.com/git/git/blob/master/contrib/git-jump/git-jump) 
is one of them. It can be used to start vim with a bunch of location loaded into quickfix list:

- `git jump grep banana` quickly find all bananas in the repo
- `git jump merge` quickly fix all merge conflicts (e.g. when rebasing)
- `git jump diff` revisit all positions of unstaged changes
- `git jump diff HEAD~`

# low-tech way of solving merge conflicts
1. `git rebase -i origin/main` \- Do a rebase and get a conflict
2. `git jump merge`\* \- Open Neovim with conflict places loaded into quickfix list
3. Solve the first using [https://github.com/inkarkat/vim-ConflictMotions](https://github.com/inkarkat/vim-ConflictMotions)
4. `:cnext` (mapped with `nnoremap <a-j> <cmd>cnext<cr>`) to go the next conflict, repeat from 3 until done.

`.gitconfig`:
```gitconfig
    core.editor = nvim
    [merge]
    ff=false
    conflictStyle=diff3   # <-- imo makes it much easier to understand the conflict.
```
I tried `merge.tool=nvimdiff` a little bit, but it's annoying that it goes to every changed place, not just the unresolved conflicts.

# vim-fugitive
The parts of [vim-fugitive](https://github.com/tpope/vim-fugitive) I use the most:

- `:Ggrep banana` - find all bananas in the repo
- `:Git blame`, mappings:
  - `P`/`~` reblame at `commit~`
  - `o` view the patch
- `:Gvsplit main:%` - inspect the current file's state at branch "main"
- `:Gdiffsplit main:%` compare with main branch

Related config:
```vim
Plug 'tpope/vim-fugitive'          
nnoremap <leader>g :Ggrep -q <c-r><c-w>
nnoremap gb <cmd>Git blame<cr>
```

# reviewing a branch
I review a git branch using my own command git-review: https://github.com/kaddkaka/dotfiles/blob/main/bin/executable_git-review
It utilizes fugitive command `:GcLog` to load commits into quickfix list.

# fzf
[fzf](https://github.com/junegunn/fzf)'s vim plugin has the handy command `:Gfiles` to quickly find files in a git repo.
I also have `:Files %:h` mapped to quickly find other files in the same folder as the current file.

Related [config] (https://github.com/kaddkaka/dotfiles/blob/main/dot_config/nvim/small.vim#L47):
```vim
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'

map <leader>f <cmd>GFiles<cr>   
map <leader>F <cmd>Files<cr>    
map <leader>l <cmd>Files %:h<cr>
```

# tig
I use [tig](https://github.com/jonas/tig) to get an overview the commit history. It has builtin mapping for pressing `e` at a change in a 
diff to immediately open vim at that locaiton.

I have some bindings for quickly rebasing the top of my commit history:

```
bind main R !git rebase -i %(commit) --keep-empty
bind diff R !git rebase -i %(commit) --keep-empty
bind main S !git rebase -i %(commit) --autosquash
bind diff S !git rebase -i %(commit) --autosquash
```

When I want to fix comments from a Merge Review I have a custom script bound to `E` that starts a rebase edit session 
with all changes of that commit in quickfix list with git-jump.

```
bind main E <tig_edit %(commit)
bind diff E <tig_edit %(commit)
```

```bash
#!/bin/bash
echo "Editing: $1"
GIT_EDITOR='sed -i 1s/pick/edit/' git rebase -i "$1"~
git jump diff HEAD~
git commit -a --fixup=HEAD --no-edit
```

This has the upside of fixing the code directly on the correct commit, reducing the total number of merge/rebase conflicts. 
