+++
draft = false
author = "Andy Huynh"
date = "2016-09-07T10:25:06-07:00"
title = "Create a Leader Command for Vim Paste Mode"
+++

Ever copy something to your clipboard to **NOT** have it appear as you would expect in Vim? Pasting from another application to Vim yields wonky results because Vim doesn't know how to handle the clipboard's content.

Vim has a paste mode you can toggle on or off using `:set paste` and `:set nopaste`.

<img src="/images/vim_paste_mode.png" alt="vim_paste_mode" />

Vim has a one liner you can add to `vimrc` to transform <F2> into a Vim paste toggle: `set pastetoggle=<F2>`.

However, leader commands are omnipresent in my workflow. [Codegangsta](https://github.com/codegangsta) at [Kajabi](https://newkajabi.com/) wrote a succinct Vim script for this very purpose.

#### .vimrc
```
function! TogglePaste()
        if(&paste == 0)
                set paste
                echo "Paste Mode Enabled"
        else
                set nopaste
                echo "Paste Mode Disabled"
        endif
endfunction

map <leader>p :call TogglePaste()<cr>
```

My leader button of choice is `\`; thus `\p` to toggle your paste on and off.
