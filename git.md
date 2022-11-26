I mostly use git form the command line, but there are some vim-git integrations that I really like:

# git-jump
The git source contains several peripheral tools, [git-jump](https://github.com/git/git/blob/master/contrib/git-jump/git-jump) 
is one of them. It can be used to start vim with a bunch of location loaded into quickfix list:

- `git jump grep banana` quickly find all bananas in the repo
- `git jump merge` quickly fix all merge conflicts (e.g. when rebasing)
- `git jump diff` revisit all positions of unstaged changes
- `git jump diff HEAD~`

# vim-fugitive
The parts of [vim-fugitive](https://github.com/tpope/vim-fugitive) I use the most:

- `:Ggrep banana` - find all bananas in the repo
- `:Git blame`, mappings:
  - `P` reblame at `commit~`
  - `o` view the patch
- `:Gvsplit main:%` - inspect the current files state at branch "main"
- `:Gdiffsplit main:%` compare with main branch

Related config:
```vim
Plug 'tpope/vim-fugitive'                               
Plug 'https://github.com/tommcdo/vim-fugitive-blame-ext'

nnoremap <leader>g :Ggrep -q <c-r><c-w>
nnoremap gb <cmd>Git blame<cr>
```

# fzf
fzf has the hand command `:Gfiles` to quickly find files in a git repo. I also have `:Files %:h`
mapped to quickly find other files in the same folder as the current file.

Related config:
```vim
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'

map <leader>f <cmd>GFiles<cr>   
map <leader>F <cmd>Files<cr>    
map <leader>l <cmd>Files %:h<cr>
```

# tig
I use tig to get an overview the commit history. It has builtin mapping for pressing `e` at a change in a 
diff to immediately open vim at that locaiton.

I have some bindings for quickly rebasing the top of my commit history:

```
bind main R !git rebase -i %(commit) --keep-empty
bind diff R !git rebase -i %(commit) --keep-empty
bind main S !git rebase -i %(commit) --autosquash
bind diff S !git rebase -i %(commit) --autosquash
```
