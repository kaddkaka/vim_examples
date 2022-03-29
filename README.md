# Builtins

## Increment numbers (incremental sequence)
[`ctrl-a`](https://vimhelp.org/change.txt.html#CTRL-A) increments the number at or after the cursor, [`ctrl-x`](https://vimhelp.org/change.txt.html#CTRL-X) decrements the number. With visual selection an incremental sequence can be achieved with [`g ctrl-a`](https://vimhelp.org/change.txt.html#v_g_CTRL-A):

![g_ctrl_a](https://user-images.githubusercontent.com/4508793/142710199-0d605d4c-9d0a-42d2-976a-6d15742834b1.gif)

(key sequence in video: `jVG^A..uuugvg^A..uuugv10g^A`)

Tip: This also works with rectangle selection (`ctrl-v`).

## Replay macro on each line in visual selection
[`:norm @q`](https://vimhelp.org/various.txt.html#%3Anorm) in visual mode will perform the normal command `@q` (play macro in register q) on each line in selection. [`qq`](https://vimhelp.org/repeat.txt.html#q) is used to record the macro.

![norm_q](https://user-images.githubusercontent.com/4508793/143023739-894e32bf-c1f7-4a50-8c03-19b0771fb87b.gif)

(key sequence in video: `qqIconst ^[A;^[qjVjjj:norm @q<enter>`, q register content: `Iconst ^[A;^[`)

## Navigate quickfix list
The [quickfix list](https://vimhelp.org/quickfix.txt.html#quickfix) can be populated with locations in several ways. A key to using it effectively is to have mappings for [`:cnext`](https://vimhelp.org/quickfix.txt.html#%3Acnext) and [`:cprev`](https://vimhelp.org/quickfix.txt.html#%3Acprev).

- [`:vimgrep en *`](https://vimhelp.org/quickfix.txt.html#%3Avimgrep) is used to find all occurences of `en` in all files in cwd (current working directory).
- [`:copen`](https://vimhelp.org/quickfix.txt.html#%3Acopen) is used to open the quickfix window to show the quickfix list.

![qflist](https://user-images.githubusercontent.com/4508793/143112529-717fb6ea-d7ab-4f87-a5da-4c0df5f2a9c4.gif)

Mapping suggestions:
```vim
nnoremap <a-j> <cmd>cnext<cr>
nnoremap <a-k> <cmd>cprev<cr>
```

- `:vimgrep /def test/ **/*.py` find all matches of "def test" in all pythons files recursively from cwd.
- [`:cdo`](https://vimhelp.org/quickfix.txt.html#%3Acdo) executes a command at each entry in the quickfix list.

## Use `!` to run external command (e.g. sort lines)
[Filters](https://vimhelp.org/change.txt.html#filter) can be run for example by typing
[`!`](https://vimhelp.org/change.txt.html#!) with a visual selection. All text will be filtered
through an external command. To sort all lines by numeric sort, use `! sort -n`.

![filter in selection](sort.gif)

(key sequence in video: `vip! sort -n<enter>`)

## Use filtering and awk to sum all numbers in a specific column
awk is a powerful tool, here used to sum all the fields of a specific column:
 - `:%! awk '{print; s+=$2} END {print s}'` sums the second field (`$2`) of
   each line and prints the total at the `END`.

![filter with awk](filter_awk.gif)

## global: repeat a command for each line matching a pattern
With [`:global`](https://vimhelp.org/repeat.txt.html#%3Aglobal) a command can be repeated for each
line that matches a pattern. Default pattern is last used search pattern.

 - `:g//d` deletes all lines matching the last search pattern
 - `:g//d E` delete the same lines and appends each delete to register `e` which can be pasted with
   `"ep`

![global delete, apoend to register](global.gif)

key sequence in video: `*:g//d<enter>` `u` `:g//d E<enter>p`

 - `:g/pattern/norm @q`, play macro on each line matching pattern.
 - `:v/;$/ s/$/;`, Add `;` to all lines that does not end in semicolon.

More g power at [https://vim.fandom.com/wiki/Power_of_g](https://vim.fandom.com/wiki/Power_of_g)

## Replace only within selection
The search [pattern atom](https://vimhelp.org/pattern.txt.html#pattern-atoms) [`\%V`](https://vimhelp.org/pattern.txt.html#%2F%5C%25V) can be used to match inside visual area. This can be used to replace only within a (rectangle) selection.

![sub_in_selection](https://user-images.githubusercontent.com/4508793/143133723-69acd9ba-1516-4d21-a546-b04d9a84e622.gif)

(key sequence in video: `wwww^VG$se/o<enter>`)

Mapping suggestion:
```vim
xnoremap s :s/\%V
```

## Delete to search motion
Normal commands like [`d`](https://vimhelp.org/change.txt.html#d) can take any [motions](https://vimhelp.org/intro.txt.html#%7Bmotion%7D) for example a search `/`. When searching and current match is displayed, use [`ctrl-g`](https://vimhelp.org/cmdline.txt.html#%2F_CTRL-G) to move to the next match.

- `d/en<c-g><c-g><enter>` Will delete everything up to the 3rd match of search pattern "en".
- Use a [search-offset](https://vimhelp.org/pattern.txt.html#search-offset) like `/e` in `d/en/e` to delete to the end of the match.

![d_to_search](https://user-images.githubusercontent.com/4508793/143139836-a1ac23f4-9367-447b-867c-06b6a7c7fdc3.gif)

(key sequence in video: `d/en^G^G<enter>`)

## Select what was just pasted, reselect last selection

[`` `[ ``](https://vimhelp.org/motion.txt.html#%60%5B) moves to the first character of the previously changed or yanked text, [`` ]` ``](https://vimhelp.org/motion.txt.html#%60%5D) moves to the last. This means that `` `[v`]`` will visually select what was just pasted. [`gv`](https://vimhelp.org/visual.txt.html#gv) will reselect the previous selected area.

![gv](https://user-images.githubusercontent.com/4508793/143313428-e6bba2f8-425b-468e-87b9-f02afcbf09b8.gif)

(key sequence in video: ``viByPgg`[v`]^[jjjgv``)

Mapping suggestion:
```vim
nnoremap <leader>gv `[v`]
```

## Repeat the last command line
[`@:`](https://vimhelp.org/repeat.txt.html#%40%3A) can be used to repeat the last command line command. More convenient when mapped to a single key.

Mapping suggestion:

```vim
nnoremap , @:
```

## Recursive macros
By playing the content of a register at the end of the recording, you can make
a macro recursive. (Make sure it has a stop condition or you will have to break
it with `<ctrl-c>`!)

If you forgot to do the macro recursive while recording it the first time, you
make it recursive with the sequence `qQ@qq` (assuming your macro is in the `q`
register)

### Recursive within line
A movement within a line such as ``f `` (f followed by space) will stop the
recursive macro at the end of a line. This can be used to modify each word on a
line in some way:

1. `qq` `ciw"<c-r>-"<esc>f l@q` `q`: surround each "word" on the line in quotes
2. `qq` `gUiw2f l@q` `q`:  upper-case every 2nd "word" on the line

With both lines below visually selected, replaying macro 1 from above with 
`:norm e@q` will turn:
```
   Recursive over lines
Recursive over lines
```

Into:
```
   "Recursive" "over" "lines"
"Recursive" "over" "lines"
```

Mapping suggestion:

```vim
nnoremap <leader>q qqqqq
```

Questions:

- What is actually different between `<ctrl-r>-` and `<ctrl-r>"`?
- Why doesn't this mapping seem to work: `nnoremap <leader>q qqqqq`?
- Is there a "within line" version of `:g`?


# Ideas/TODOs
- appending to registers ([`"Ayy`](https://vimhelp.org/change.txt.html#quotea))
- inserting literal characters ([`ctrl-v`](https://vimhelp.org/insert.txt.html#i_CTRL-V))
- pasting from register in insert mode ([`ctrl-r`](https://vimhelp.org/insert.txt.html#i_CTRL-R))
- batch changes (`:cdo`, [`:bufdo`](https://vimhelp.org/windows.txt.html#%3Abufdo), `:windo`, `:argdo`, `:ldo`)
- changelist: go back to previous edit location ([`g;`](https://vimhelp.org/motion.txt.html#g%3B))
- insert at previous insert location ([`gi`](https://vimhelp.org/insert.txt.html#gi))
- normal commands in insert mode ([`ctrl-o`](https://vimhelp.org/insert.txt.html#i_CTRL-O))
