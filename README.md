## Builtins

### Increment numbers (incremental sequence)
`ctrl-a` Can be used to increment the next number on the current line by 1. `ctrl-x` decrements the number. With visual selection an incremental sequence can be achieved with `g ctrl-a`:

![g_ctrl_a](https://user-images.githubusercontent.com/4508793/142710199-0d605d4c-9d0a-42d2-976a-6d15742834b1.gif)

[`:h v_g_CTRL-A`](https://vimhelp.org/change.txt.html#v_g_CTRL-A) (key sequence: `jVG^A..uuugvg^A..uuugv10g^A`)

### Replay macro on each line in visual selection
`q` can be used to record macros. In this example `qq` is used to record a macro into register `q` and then it is repeated for each line in a visual selection using `:norm @q` to perform the normal command `@q` (play the macro) on each line in selection.

![norm_q](https://user-images.githubusercontent.com/4508793/143023739-894e32bf-c1f7-4a50-8c03-19b0771fb87b.gif)

[`:h q`](https://vimhelp.org/repeat.txt.html#q), [`:h norm`](https://vimhelp.org/various.txt.html#%3Anorm) (key sequence: `qqIconst ^[A;^[qjVjjj:norm @q^M`, q register content: `Iconst ^[A;^[`)

### Navigate quickfix list
The [quickfix list](https://vimhelp.org/quickfix.txt.html#quickfix) can be populated with locations in several ways. A key to using it effectively is to have mappings for [`:cnext`](https://vimhelp.org/quickfix.txt.html#%3Acnext) and [`:cprev`](https://vimhelp.org/quickfix.txt.html#%3Acprev).

- [`:vimgrep en *`](https://vimhelp.org/quickfix.txt.html#%3Avimgrep) is used to find all occurences of `en` in all files in cwd (current working directory).
- [`:copen`](https://vimhelp.org/quickfix.txt.html#%3Acopen) is used to open the quickfix window to show the quickfix list.

![qflist](https://user-images.githubusercontent.com/4508793/143112529-717fb6ea-d7ab-4f87-a5da-4c0df5f2a9c4.gif)

Mapping suggestions:
```
nnoremap <a-j> <cmd>cnext<cr>
nnoremap <a-k> <cmd>cprev<cr>
```
