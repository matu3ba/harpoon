# Harpoon
Fork of to be eventually rewritten https://github.com/ThePrimeagen/harpoon
adjusted for more ~~allowing yourself to shoot in your foot~~ direct control of things.
See "More Power".

## ⇁ Installation
* neovim 0.5.0+ required
* install using your favorite plugin manager (`vim-plug` in this example)
```vim
Plug 'nvim-lua/plenary.nvim' " don't forget to add this one if you don't have it yet!
Plug 'ThePrimeagen/harpoon'
```

## ⇁ Harpooning
here we'll explain how to wield the power of the harpoon:


### Marks
you mark files you want to revisit later on
```lua
:lua require("harpoon.mark").add_file()
```

### File Navigation
view all project marks with:
```lua
:lua require("harpoon.ui").toggle_quick_menu()
```
you can go up and down the list, enter, delete or reorder. `q` and `<ESC>` exit and save the menu

you also can switch to any mark without bringing up the menu, use the below with the desired mark index
```lua
:lua require("harpoon.ui").nav_file(3)                  -- navigates to file 3
```
you can also cycle the list in both directions
```lua
:lua require("harpoon.ui").nav_next()                   -- navigates to next mark
:lua require("harpoon.ui").nav_prev()                   -- navigates to previous mark
```

### Terminal Navigation
this works like file navigation except that if there is no terminal at the specified index
a new terminal is created.
```lua
lua require("harpoon.term").gotoTerminal(1)             -- navigates to term 1
```

### Commands to Terminals
commands can be sent to any terminal
```lua
lua require("harpoon.term").sendCommand(1, "ls -La")    -- sends ls -La to tmux window 1
```
further more commands can be stored for later quick
```lua
lua require('harpoon.cmd-ui').toggle_quick_menu()       -- shows the commands menu
lua require("harpoon.term").sendCommand(1, 1)           -- sends command 1 to term 1
```

### Tmux Support
tmux is supported out of the box and can be used as a drop-in replacement to normal terminals
by simply switching `'term' with 'tmux'` like so

```lua
lua require("harpoon.tmux").gotoTerminal(1)             -- goes to the first tmux window
lua require("harpoon.tmux").sendCommand(1, "ls -La")    -- sends ls -La to tmux window 1
lua require("harpoon.tmux").sendCommand(1, 1)           -- sends command 1 to tmux window 1
```

`sendCommand` and `goToTerminal` also accept any valid [tmux pane identifier](https://man7.org/linux/man-pages/man1/tmux.1.html#COMMANDS).
```lua
lua require("harpoon.tmux").gotoTerminal("{down-of}")   -- focus the pane directly below
lua require("harpoon.tmux").sendCommand("%3", "ls")     -- send a command to the pane with id '%3'
```

Once you switch to a tmux window you can always switch back to neovim, this is a
little bash script that will switch to the window which is running neovim.

In your `tmux.conf` (or anywhere you have keybinds), add this
```bash
bind-key -r G run-shell "path-to-harpoon/harpoon/scripts/tmux/switch-back-to-nvim"
```

### Telescope Support
1st register harpoon as a telescope extension
```lua
require("telescope").load_extension('harpoon')
```
currently only marks are supported in telescope
```
:Telescope harpoon marks
```

## ⇁ Configuration
if configuring harpoon is desired it must be done through harpoons setup function
```lua
require("harpoon").setup({ ... })
```

### Global Settings
here are all the available global settings with their default values
```lua
global_settings = {
    -- sets the marks upon calling `toggle` on the ui, instead of require `:w`.
    save_on_toggle = false,

    -- saves the harpoon file upon every change. disabling is unrecommended.
    save_on_change = true,

    -- sets harpoon to run the command immediately as it's passed to the terminal when calling `sendCommand`.
    enter_on_sendcmd = false,

    -- closes any tmux windows harpoon that harpoon creates when you close Neovim.
    tmux_autoclose_windows = false,

    -- filetypes that you want to prevent from adding to the harpoon list menu.
    excluded_filetypes = { "harpoon" },

    -- set marks specific to each git branch inside git repository
    mark_branch = false,
}
```

### More Power
Work on underlying buffer and terminal ids to ~~not~~ utterly break things:
```lua
lua require("harpoon.ui").getBufferId(1)
lua require("harpoon.term").getBufferTerminalId(1)
-- :lua = require("harpoon.term").getBufferTerminalId(1)
-- Returns:
-- {
--   buf_id = 7,
--   term_id = 8
-- }
```

This allows lua code to do things like movements, which I find convenient ie for
sending `G` after sending `!!` for a command.

## ⇁ Social
For questions about Harpoon, there's a #harpoon channel on [the Primagen's Discord](https://discord.gg/theprimeagen) server.
* [Discord](https://discord.gg/theprimeagen)
* [Twitch](https://www.twitch.tv/theprimeagen)
* [Twitter](https://twitter.com/ThePrimeagen)
